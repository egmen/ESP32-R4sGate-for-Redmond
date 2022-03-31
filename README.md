# ESP32 Ready4Sky (R4S) шлюз для устройств Redmond+

RUS | [ENG](https://translate.google.com/translate?hl=ru&sl=ru&tl=en&u=https%3A%2F%2Fgithub.com%2Falutov%2FESP32-R4sGate-for-Redmond%2Fblob%2Fmaster%2FREADME.md)

> **Шлюз использует Mqtt, что позволяет ему работать с практически всеми системами умного дома. Отображаемая на экране дополнительная информация также берется с Mqtt. Любителей ESPHome прошу не спамить в issues, а искать другие решения - портирование в ESPHome не планируется.**<br> 
> **После прошивки с версии 2021.03.25 на более свежие проверяйте типы устройств. Из-за добавления 170 модели все сдвинулось.**

#### Текущая версия 2022.03.30.
* Добавлен вывод количества воды в чайнике на экран, сенсор количества воды после последнего кипячения. Добавлен BLE monitor. 
* Добавлен конвектор Redmond SkyHeat RCH-7001S.
* Добавлено вычисление количества воды в чайнике. Подробнее в описании ниже.
* Добавлена отдельно мультиварка RMC-M92S. Оптимизация, мелкие исправления. 
* Добавлена возможность поиска и подключения к устройствам как по имени, так и по MAC адресу. Пригодится при работе с устройствами с одинаковыми именами. MAC адрес вводится в поле имени устройства в виде строки из 12 символов большими или маленькими буквами без двоеточий и тире. Найти адрес можно в строке  "BLE Last found name/address" на главной страничке. 
* Добавлена мультиварка RMC-M961S. 
* Увеличено до 19 символов поле Mqtt Server. 
* Добавлены "configuration_url" и версия прошивки для шлюза. Первая опция работает только на 11 версии ХА, на 10 версии из-за ошибки в этой опции разбор discovery на ней тормозится и в итоге пропадает выключатель экрана. 
* Добавлена опция чистки топиков Mqtt. 
* Исправлена ошибка в версиях 2021.11.03 и 2021.11.04, не обновлялся статус 3-го устройства при локальном управлении. 
* Переделан вывод на экран даты для освобождения места. На освободившееся место цветом выводится состояние устройств, 1 - первое устройство, 2- второе и 3 - третье. Серый - не на связи или не определено, синий - выключено, красный - включено, желтый - подогрев, белый - установлена программа. Стало удобней пользоваться кнопками включения - выключения. 
* Добавлена мультиварка RMC-M224S. 
* Добавлено 5 портов ввода-вывода (Port1-Port5). Порты программируются на вывод(от 0 до 33 пина) или ввод(от 0 до 39), с учетом ограничений esp32, разумеется. Жесткой проверки не производится, так что возможно зависание esp32 при старте при неверном выборе пинов. Для первых трех портов предусмотрен также режим sw(sw1-sw3), когда порт работает в режиме ввода с активным низким уровнем как кнопка локального включения-выключения соответствующего устройства. В этом режиме состояние кнопки в mqtt не выводится. При включенном Hass Discovery порты в Ассистенте привязываются к шлюзу. Отмечу, что выключение порта не удаляет его из шлюза в Ассистенте, для этого нужно удалить в Mqtt брокере все топики с r4s, выбрав в настройках Delete Mqtt topics. При работе порта на ввод подтягивающие резисторы (pullup, pulldown) программно не задействовал, да и для пинов 34-39 esp32 это нельзя сделать. Пока потестируем, может что-то и стоит поправить. 
* Добавлена поддержка экрана на ili9342. 
* Добавлен топик heat. Включает подогрев с последней установленной температурой с момента соединения с чайником. При старте шлюза температура подогрева берется из Heat temp вкладки Setting. Если и там температура < 30&deg;C, то по умолчанию температура подогрева 40&deg;C. 
* В Hass Discovery добавлено поле "via_device". 
* Потребленная энергия выводится в кВт/ч.
<details>
     <summary>Другие версии...</summary>

     
**Версия 2021.10.17.**
     
* Во всех типах чайников топик "state" изменен на "boil". Буду изучать еще режим подогрева, возможно, добавлю топик "heat". 
* В чайниках 200-240 добавлено включение и выключение цветовой индикации температуры воды, включение и выключение звуковой индикации, а также статистика.
* Включенная "offline Response" выводит offline в топиках состояний при отсутствии связи с устройством. Если она выключена, об отсутствии связи можно судить только по топику status. Если включена опция Hass Discovery, опции true/false и offline выключаются за ненадобностью.

**Версия 2021.09.29.** 
     
* Добавлена лицензия. 
* Mqtt топик state чайника Redmond теперь управляет только режимом кипячения, на режим подогрева не влияет. 
* Увеличена длина пароля Mqtt до 19 символов. Работа с экраном теперь включается в настройках и по умолчанию выключена. Это нужно для запуска прошивки на esp32 с нестандартной разводкой пинов, например, на [ESP32-PICO-D4 (m5atom lite)](https://github.com/alutov/ESP32-R4sGate-for-Redmond/issues/48). 
* После кипячения чайник Mikettle переводится в режим поддержания температуры, что позволяет повторно включать кипячение без снятия с подставки. 
* Добавлен чайник M170S. 
* После обновления нужно проверить и при необходимости поправить тип чайника. При обновлении закрываются все BLE cоединения. Изменил параметры скана и добавил строку состояния BLE. При соединении показывает 2 этапа: open - установление соединения, auth - чтение характеристик устройства и авторизация. 
* Немного поправил wifi, теперь пытается восстановить соединение 10 раз до рестарта. 
* Уменьшил до 256 минут время сброса счетчика warming Mikettle. 
* Вроде бы нашел в коде [причину зависания BLE при сканировании](https://github.com/espressif/esp-idf/issues/6688). Зависаний BLE больше не замечал. Мониторинг состояния BLE сделан  опцией "BLE monitoring" и по умолчанию выключен, сейчас я им не пользуюсь. Если он включен, поведение прошивки аналогично старой версии, то есть рестарт при потере соединения более минуты и ошибке скана. 
* Добавлена поддержка мультиварки RMC-M903S и частично духового шкафа RO-5707S, за исключением режимов блокировки и автоподогрева. 
* В RMC-M800S добавлена программа мультиповар. В режиме RMC-M903S можно попробовать и другие мультиварки. 
* Отключена проверка состояния BLE  соединений во время обновления. 
* На экране вместо влажности в помещении выводится напряжение и ток из [ZMAi-90](https://github.com/alutov/zmai-90_tywe3s_mqtt_gateway).<br>
</details>

## 1. Возможности
&emsp; Шлюз ESP32 r4sGate в минимальной конфигурации (только esp32 с источником питания 3.3v) позволяет подключать BLE-совместимые устройства Redmond и чайники Xiaomi MiKettle к системе "умный дом" (Home Assistant, OpenHab, ioBroker, MajorDoMo и т.д.) по протоколу MQTT. Поддерживаются 3 BLE соединения. Управление Xiaomi MiKettle возможно только из режима "keep warm". 
<details>
<summary>Подробнее о Xiaomi MiKettle...</summary>
В этом режиме чайник поддерживает заданную шлюзом минимальную температуру 40&deg;C с гистерезисом примерно 4&deg;C, то есть при 36&deg;C подогрев включается, а при 44&deg;C отключается. Доступно включение и выключение кипячения (state = ON/OFF), установка температуры подогрева (heat_temp = 40...95). Можно перевести чайник в режим Idle (heat_temp = 0). Последняя команда выполняется с задержкой. После выполнения команды дальнейшее управление чайником недоступно. В отличие от выключения сенсором "warm" на чайнике при дальнейшем выключении и включении чайник возвращается в режим "keep warm". Возможно, это особенность конкретной версии MCU 6.2.1.9. Пока оставил так и включил чайник через редмондовскую розетку. Если ее выключить и опять включить, чайник переходит в режим подогрева. Все необходимые параметры чайника шлюз устанавливает самостоятельно, а родное приложение пригодится для обновления прошивки. Время подогрева установлено на 12 часов (720 минут), после 256 минут шлюз сбрасывает счетчик кратковременно включая и выключая кипячение. И все равно управление ограниченное. Главная проблема в том, что при включении кипячения сенсором "boil" на чайнике выключается режим "keep warm" и вернуть его можно только кнопкой "warm" на чайнике. По этой же причине пока отложил работы по Mikettle Pro.</details>

#### Список поддерживаемых устройств:

**Электрочайники:**
* Redmond SkyKettle RK-M170S / RK-M171S
* Redmond SkyKettle RK-M173S / RTP-M810S
* Redmond SkyKettle RK-G200S / RK-G204S / RK-G210S / RK-G211S / RK-G212S / RK-G214S / RK-M216S
* Redmond SkyKettle RK-G240S / RK-G204S / RK-G210S / RK-G211S / RK-G212S / RK-G214S / RK-M216S
* Xiaomi MiKettle YM-K1501(Int) - ProductId 275
* Xiaomi MiKettle YM-K1501(HK) - ProductId 131
* Xiaomi MiKettle V-SK152(Int) - ProductId 1116

**Мультиварки**

* Redmond SkyCooker RMC-M224S
* Redmond SkyCooker RMC-M800S
* Redmond SkyCooker RMC-M903S
* Redmond SkyCooker RMC-M92S
* Redmond SkyCooker RMC-M961S
     
**Кофеварки**

* Redmond SkyCoffee M1519S

**Розетки**

* Redmond SkyPort 103S

**Конвекторы электрические**

* Redmond SkyHeat RCH-7001S / RCH-7002S / RCH-7003S
* Redmond SkyHeat RCH-4529S (управляется как SkyPort 103S)

&emsp; Поддерживается вычисление количества воды в чайнике при нагреве в интервале 70-90&deg;C и более 3&deg;C с момента включения чайника. Одновременно может служить индикатором кипевшей воды. Вычисляется на основе затраченной энергии и разности температур. Вычисленное значение сбрасывается при снятии чайника с подставки. Опция работает только в чайниках со статистикой. КПД чайника изначально принят 80%. Точность так себе, у меня выходит где-то ~0.2литра. Для повышения точности предусмотрен режим корректировки значения КПД. Для этого нужно залить в чайник 1л воды и выбрать в web-интерфейсе "Boil 1l on". Когда режим отработает, нужно зайти в режим настроек. Новое значение будет выведено сразу за типом устройства. Записать новое значение в nvram можно командой Save setting. Как мне думается, получить большую точность нереально, так как КПД чайника со временем меняется, например, с появлением накипи, и, что хуже, затраченная энергия скорее всего не измеряется, а просто вычисляется процессором чайника исходя из номинальной мощности нагревателя и времени его работы. Отклонение питающего напряжения при работе от значения при калибровке вносит заметную погрешность, зависимость там квадратичная. У меня при кипячении чайником RK-M216S 1.7 литра воды при напряжении на входе в дом 200-204V в итоге вычисляется 1.8 литра, при напряжении 210-214V выходит 1.6 литра. При калибровке очевидно было что-то среднее.<br> &emsp;Поддерживается Home Assistant Mqtt Discovery. Шлюз также можно использовать для отслеживания до 8 ble устройств (меток) по MAC адресу. Выводится наличие/отсутствие метки и rssi. Предусмотрено 5 портов ввода-вывода для управления внешними устройствами и чтения их состояния. Три первых порта можно настроить для включения - выключения подключенных BLE устройств. С целью расширения возможностей шлюза возможно подключение TFT экрана 320 * 240 на чипах ili9341 и ili9342. На экран выводится текущее время, день и месяц, а также температура, напряжение и ток  в доме (не помешает при питании от генератора), состояние (синий- выкл., красный - вкл.) и температура на выходе котла, температура и влажность на улице. Все берется с Mqtt. Рядом с датой цветом выводится состояние BLE устройств, 1 - первое устройство, 2- второе и 3 - третье. Серый - не на связи или не определено, синий - выключено, красный - включено, желтый - подогрев, белый - установлена программа. Подробнее состояние и некоторые параметры подключенных BLE устройств по очереди отображаются в нижней строке. Есть возможность периодического вывода на экран картинки в формате jpeg 320 * 176, например, с камеры.  Большие размеры не поддерживаются из-за малого объема свободной оперативной памяти. Экран можно включать и выключать по Mqtt. Получилось что-то похожее на часы с термометром. Достаточно глянуть на экран, чтобы убедиться, все ли в порядке в доме, что там сегодня на улице.<br>

## 2. Комплектующие

![PROJECT_PHOTO](https://github.com/alutov/ESP32-R4sGate-for-Redmond/blob/master/jpg/myparts.jpg)
Картинка 1. Комплектующие для сборки шлюза.

&emsp; Если цель запустить шлюз с минимальными затратами, придется покупать запчасти, затем собирать из них шлюз. Я использовал [ESP32 WROOM ESP-32 4 Мб с встроенной антенной (слева внизу) или ESP32 WROOM ESP-32U 4 Мб с внешней (правее первой)](https://aliexpress.ru/item/32961594602.html?item_id=32961594602&sku_id=66888778667&spm=a2g2w.productlist.0.0.4f835c61Fvd1gD). Цена вопроса $2.5. Потом паял микросхему на ["адаптер-плату"($0.9 3шт.)](https://aliexpress.ru/item/32951593640.html?sku_id=66291291038&spm=a2g2w.productlist.0.0.3f641e90aBqAcg) и далее на макетную плату. Источник питания на 3.3 Hi-Link($2-$4). [Я их брал по цене $1.65](https://aliexpress.ru/item/32953853140.html?spm=a2g39.orderlist.0.0.32964aa6PePEbg&_ga=2.238912000.104655408.1636114275-428746708.1615828563&_gac=1.87036010.1634012869.Cj0KCQjwwY-LBhD6ARIsACvT72Na1GBQp7leEJDlxPCd0jTye8sF-GiknWzlo4hKElMNbtmI4DYpB_8aAktOEALw_wcB). ***Можно обойтись без пайки***, если использовать [esp32-wroom-devkit(внизу в центре, $14)](https://aliexpress.ru/item/4000127837743.html?sku_id=10000000372418546&spm=a2g0s.9042311.0.0.274233edNcajyj). Правда, эта плата сильно избыточна для проекта, [можно взять попроще за $3.54](https://aliexpress.ru/item/32928267626.html?item_id=32928267626&sku_id=12000016847177755&spm=a2g2w.productlist.0.0.430c65c8Kf9vOT). В нем esp32 идет вместе с платой, на которой есть еще преобразователи с 5в на 3.3в, USB-RS232 и стандартный разъем мини-USB. Через него можно питать esp32, используя пятивольтовое зарядное устройство от смартфона, и программировать прямо с компьютера без всяких переходников. И справа на фото [3.2" 320 * 240 TFT экран($18)](https://aliexpress.ru/item/32911859963.html?spm=a2g0s.9042311.0.0.274233edzZnjSp), который я использовал в шлюзе. Можно использовать и совместимые готовые устройства как с экраном (TTGO T-Watcher BTC Ticker, M5Stack BASIC Kit), так и без (m5atom lite).

## 3. Настройка шлюза

&emsp; Для запуска шлюза нужно [запрограммировать ESP32](https://www.youtube.com/watch?v=Vy-YTSdwy7s). Файл fr4sGate.bin в папке build это уже собранный бинарник для esp32 @160MHz с памятью 4 Мбайт, DIO загрузчиком и прошивается одним файлом с адреса 0x0000 на чистую esp32. Вместо него также можно использовать три стандартных файла для перепрошивки: bootloader.bin (адрес 0x1000),  partitions.bin (адрес 0x8000) и r4sGate.bin (адрес 0x10000). Файл r4sGate.bin можно также использовать для обновления прошивки через web интерфейс. Если же DIO загрузчик не стартует, можно использовать файл fqr4sGate.bin с загрузчиком QIO.<br>
&emsp; Затем нужно создать гостевую сеть Wi-Fi в роутере с ssid "r4s" и паролем "12345678", подождать, пока esp32 не подключится к нему, ввести esp32 IP-адрес в веб-браузере и во вкладке Setting установить остальные параметры. После чего гостевая сеть больше не нужна. Esp32 будет пытаться подключиться к сети "r4s" только при недоступности основной сети, например, при неправильном пароле. Если не удается подключиться и к гостевой сети, esp32 перезагружается. Вариант с гостевой сетью в отличие от общепринятой практики запуска точки доступа на esp32, как мне кажется, удобнее, так как позволяет настраивать все с компьютера не тыкая пальцами в смартфон, который при отсутствии инета так и норовит соскочить с esp32. Но, главное, в случае падения по каким-то причинам Wi-Fi роутера (а он может быть выделенным только для iot устройств) остальной Wi-Fi не засоряется дружно "вплывшими" esp32.<br>
&emsp; Далее нужно ввести имя Redmond устройства и привязать его к шлюзу. Поиск устройств запускается только тогда, когда есть хотя бы одно определенное, но не соединенное устройство. Если имя устройства точно не известно (а редмонды не всегда светятся по BLE как модель один в один), то для начала сканирования нужно ввести в поле "Name" в настройках любое имя, а потом заменить его найденным (строка "BLE Last found name/address") при сканировании и выбрать ближайший тип устройства. Есть возможность поиска и подключения к устройствам по MAC адресу. Пригодится при подключении нескольких устройств с одинаковыми именами. MAC адрес вводится в поле имени устройства в виде строки из 12 символов большими или маленькими буквами без двоеточий и тире. Найти и скопировать адрес можно также в строке "BLE Last found name/address" на главной страничке. Далее для привязки нужно нажать и удерживать кнопку "+" на чайнике или "таймер" на мультиварке  до тех пор, пока устройство не войдет в режим привязки и через некоторое время соединится со шлюзом.<br> 
&emsp; Предусмотрена возможность подключения к одному MQTT серверу нескольких шлюзов. Для этого нужно в каждом шлюзе установить свой r4sGate Number. Шлюз с номером 0 будет писать в топик r4s/devaddr/..., шлюз с номером 1 - r4s1/devaddr/... и т.д. Нужно только учесть, что запрос на авторизацию при привязке зависит от номера шлюза и от номера соединения в шлюзе. Это позволяет привязать 2 одинаковых чайника или мультиварки к 2 разным шлюзам или к 2 разным соединениям в пределах одного шлюза.<br>
&emsp; Для подключения к Mqtt брокеру нужно ввести его адрес и порт, а также логин и пароль. Если шлюз работает с Home Assistant в паре с mosquitto брокером, стоит использовать опцию Hass Discovery. Перед ее использованием рекомендую удалить в Mqtt брокере все топики с r4s, для чего выбрать в настройках Delete Mqtt topics. Если же система не поддерживает Mqtt Discovery, придется разбираться с Mqtt.  
<details>
<summary>Подробнее по Mqtt...</summary>
     У меня шлюз подключен к ioBroker. Мои настройки MQTT брокера ниже на картинке 2.<br>

![PROJECT_PHOTO](https://github.com/alutov/ESP32-R4sGate-for-Redmond/blob/master/jpg/mymqtt.jpg)
Картинка 2. Мои Mqtt настройки.<br><br>
     
&emsp; Снят флаг retain, чтобы брокер не запоминал, а считывал состояние устройств при соединении. В Home Assistant  установленный в нем и/или Mqtt брокере флаг retain [может приводить к самопроизвольному включению и выключению устройства](https://mjdm.ru/forum/viewtopic.php?f=8&t=5501&sid=de6b1e2b43f25c8d9ae9af5673ee9417&start=140#p121604).  Также установлен флаг публикации при подписке, что позволяет не вводить все топики вручную. Иногда при публикации сразу большого числа подписок ioBroker почему-то делает некоторые из них с защитой от записи :-), есть у меня такой глюк. Приходится их удалять и перезапускать Mqtt адаптер, чтобы они появились опять.<br>

#### Mqtt топики для чайника (см. картинку 3 ниже):

`r4s/devaddr/cmd/boil` <-- `0/off/false` - выключение кипячения, `1/on/true` - включение кипячения. Если перед этим был включен подогрев, включается кипячение с последующим подогревом;<br>
`r4s/devaddr/cmd/heat` <-- `0/off/false` - выключение подогрева, `1/on/true` - включение подогрева с последней запомненной шлюзом температурой. При старте шлюза температура берется из поля Heat в настройках;<br>
`r4s/devaddr/cmd/heat_temp` <-- `30...90` - включение подогрева, `> 97` - выключение, `< 30` - выключение, если подогрев был включен, последняя температура если был выключен;<br>
`r4s/devaddr/cmd/nightlight` <-- `0/off/false` - выключение ночника, `1/on/true` - включение ночника;<br>
`r4s/devaddr/cmd/nightlight_red` <-- `0..255` Уровень красного в ночнике;<br>
`r4s/devaddr/cmd/nightlight_green` <-- `0..255` Уровень зеленого в ночнике;<br>
`r4s/devaddr/cmd/nightlight_blue` <-- `0..255` Уровень синего в ночнике;<br>
`r4s/devaddr/rsp/` - текущее состояние, температура, rssi и т.д.;<br>

> Значения уровней запоминаются в шлюзе и передаются на чайник при включении подсветки.
<br>

![PROJECT_PHOTO](https://github.com/alutov/ESP32-R4sGate-for-Redmond/blob/master/jpg/mymqtt1.jpg)
Картинка 3. Мои Mqtt топики для чайника.

#### Mqtt топики для мультиварки (см. картинку 4 ниже):

`r4s/devaddr/cmd/state` <-- `0/off/false` - выключение, `1/on/true` - старт программы или подогрев, если программа не установлена;<br> 
`r4s/devaddr/cmd/prog` <-- номер программы 1-12, 0 - выключение;<br>
     Программы:<br>
     1 - Rice / Рис Крупы, 2 - Slow Cooking / Томление, 3 - Pilaf / Плов, 4 - Frying / Жарка;<br>
     5 - Stewing / Тушение, 6 - Pasta / Паста, 7 - Milk Porridge / Молочная каша, 8 - Soup / Суп;<br>
     9 - Yogurt / Йогурт, 10 - Baking / Выпечка, 11 - Steam / Пар, 12 - Hot / Варка Бобовые;<br>
`r4s/devaddr/cmd/mode` <-- режим: 1 - овощи, 2 - рыба, 3 - мясо для программ 4,5,11<br>
`r4s/devaddr/cmd/temp` <-- температура;<br>
`r4s/devaddr/cmd/set_hour` <-- время работы программы, часы;<br>
`r4s/devaddr/cmd/set_min` <-- время работы программы, минуты;<br>
`r4s/devaddr/cmd/delay_hour` <-- время работы программы плюс задержка до старта программы, часы;<br>
`r4s/devaddr/cmd/delay_min` <-- время работы программы плюс задержка до старта программы, минуты;<br>
`r4s/devaddr/cmd/warm` <-- подогрев после завершения программы;<br>

> Параметры `delay_hour` и `delay_min` запоминаются в шлюзе и передаются при установке программы, режима или подогрева, а потому устанавливаются перед установкой программы, режима или автоподогрева. При выборе программы устанавливаются температура и время работы программы по умолчанию, после установки mode еще раз корректируются. После установки программы и режима можно при необходимости скорректировать время и температуру. Этот порядок установки параметров обусловлен тем, что по Mqtt нельзя сразу установить одной командой все параметры, если они находятся в разных топиках. При установке же через web все параметры ставятся одной командой. И значения температуры и времени по умолчанию для каждой программы и режима устанавливаются через web только если перед этим они были  равны 0. Программа мультиповар пока не поддерживается, я не вижу смысла. При записи нуля в prog на мультиварку посылается команда выключения, что полезно для сброса программы.
<br>

![PROJECT_PHOTO](https://github.com/alutov/ESP32-R4sGate-for-Redmond/blob/master/jpg/mymqtt2.jpg)
 Картинка 4. Мои Mqtt топики для мультиварки.

#### Mqtt топики для кофеварки:

`r4s/devaddr/cmd/state` <-- `0/off/false` - выключение, `1/on/true` - включение;<br>
`r4s/devaddr/cmd/delay_hour` <-- время отложенного старта, часы;<br>
`r4s/devaddr/cmd/delay_min` <-- время отложенного старта, минуты;<br>
`r4s/devaddr/cmd/delay` <-- запуск отложенного старта, `0/off/false` - выключение, `1/on/true` - включение;<br>
`r4s/devaddr/cmd/lock` <-- блокировка, `0/off/false` - выключение, `1/on/true` - включение;<br>
`r4s/devaddr/cmd/strength` <-- крепость, `0/off/false` - выключение, `1/on/true` - включение;<br>
`r4s/devaddr/rsp/` - текущее состояние, rssi и т.д.;<br>

> Значения времени отложенного старта запоминаются в шлюзе и передаются на кофеварку при включении этого режима.

#### Mqtt топики для розетки:

`r4s/devaddr/cmd/state` <-- `0/off/false` - выключение, `1/on/true` - включение;<br>
`r4s/devaddr/cmd/lock` <-- блокировка, `0/off/false` - выключение, `1/on/true` - включение;<br>
`r4s/devaddr/rsp/` - текущее состояние, rssi и т.д.;

&emsp; Начиная с версии 2020.10.27 появилась возможность использовать совмещенные топики для команд и ответов. Опция включается в настройках. Мне это пригодилось при интеграции устройств в Google Home с помощью драйвера iot iobroker-а. Как я понял, этот драйвер не принимает раздельные топики команд/ответов. Кроме того, так как Google Home понимает `true / false` вместо `ON / OFF`, то нужно в настройках драйвера iot `Conversation to Google Home = function (value) {}` ввести строку вида `switch (value) {case "ON": return true ; break; default: return false;}`. Если же автозамена недоступна, то начиная с версии 2020.11.07 можно использовать опцию `"true / false" Response`. Опция не работает совместно с Hass Discovery, там она не нужна.<br>
</details>

#### Веб интерфейс

Устройствами можно управлять также и в веб интерфейсе шлюза. Примеры главной страницы и страницы настроек ниже на картинках 5 и 6.

![PROJECT_PHOTO](https://github.com/alutov/ESP32-R4sGate-for-Redmond/blob/master/jpg/myweb.jpg) 
Картинка 5. Главная страничка.

![PROJECT_PHOTO](https://github.com/alutov/ESP32-R4sGate-for-Redmond/blob/master/jpg/myweb1.jpg) 
Картинка 6. Страничка настроек.
 
## 4. BLE Monitor
&emsp; Для активации нужно в настройках установить в опции "BLE Monitoring" значения "Active" или "Passive" для активного или пассивного сканирования. Ранее опция использовалась для включения рестарта ESP32 при зависании BLE, но после релиза 2020.03.25 зависаний больше не наблюдалось. Активный сканер работает быстрее и дает больше информации, но расходует больше энергии на сканируемых устройствах. Для меток рекомендуется пассивный режим. Нужно учитывать, что при поиске устройств перед соединением сканер всегда работает в активном режиме. Если какое-то из устройств не используется, для включения пассивного сканирования его нужно убирать из конфигурации, очистив поле имени в настройках.<br> &emsp; После установки опции в меню появится вкладка "BLE Monitor", открыв которую, можно увидеть найденные устройства. В поле "Gap" выводится временной интервал между двумя последними пришедшими пакетами, в поле "Last" время с момента прихода последнего пакета. Тайм-аут по умолчанию 300 секунд, после чего устройство считается потерянным, а его данные, кроме MAC адреса и имени, удаляются из таблицы. Поле "Timeout" позволяет установить нужное значение, кроме того, ненулевое значение этого поля разрешает вывод данных в Mqtt. После установки всех полей нужно сохранить настройки во вкладке "Setting". После рестарта шлюза данные сохранятся, а Mqtt Discovery при старте передаст все в Home Assistant. Стоит напомнить, что лишние данные в Mqtt и Home Assistant можно удалить, выбрав в настройках "Delete Mqtt topics".  
![PROJECT_PHOTO](https://github.com/alutov/ESP32-R4sGate-for-Redmond/blob/master/jpg/blemon1.jpg) 
Картинка 7. Страничка BLE Monitor.
## 5. Поддержка экрана

&emsp; В первой версии шлюза оставался запас как оперативной (~100кБайт), так и программируемой (~400кБайт) свободной памяти, что позволяло расширить возможности прошивки, в частности, добавить поддержку экрана. К тому же у меня уже была собранная esp32 с экраном [3.2" 320x240 на чипе ili9341](https://www.aliexpress.com/item/32911859963.html?spm=a2g0s.9042311.0.0.274233edzZnjSp), работающая с прошивкой с сайта [wifi-iot](https://wifi-iot.com/). Возможно и использование для шлюза уже готовых устройств на чипах ili9341 и ili9342. В шлюзе я использовал только необходимые процедуры из [Bodmer](https://github.com/Bodmer/TFT_eSPI), адаптированные не совсем хорошо, но как есть для esp-iot. Пины для поключения экрана по умолчанию: MOSI-23, MISO-25, CLK-19, CS-16, DC-17, RST-18, BACKLIGHT-21. TOUCH CS пока не используется, но подключен к 33 пину, на котором постоянно высокий уровень. Пины можно переназначить в настройках. Есть также опция поворота экрана на 180&deg;, а также возможность включения и выключения дисплея по Mqtt, иcпользуя топик r4s/screen. Программа проверяет наличие экрана на шине SPI при старте. Предусмотрена возможность вывода на экран и картинки в формате jpeg 320x176. Размер картинки будет около 20 кБайт. Для этого нужно указать url картинки. У моей камеры url такой: http://192.168.1.7/auto.jpg?usr=admin&pwd=admin. Картинка грузится в буфер размером 32768 байт в оперативной памяти. Время обновления можно установить в настройках. Пример экрана на картинке 8. <br><br>
 ![PROJECT_PHOTO](https://github.com/alutov/ESP32-R4sGate-for-Redmond/blob/master/jpg/mytft3.jpg)
Картинка 8. Экран шлюза.<br><br>

&emsp; Стоит отметить, что сама TFT плата влияет на распространение как WiFi, так и BLE. И даже если антенна esp32 выглядывает из-под экрана, чувствительнось такого "бутерброда" заметно меньше обычной esp32. Рекомендую использовать с экраном вариант esp32 с внешней антенной. У меня в шлюзе с экраном замена esp32 на вариант с разъемом и установка внешней антенны дала прирост уровней WIFI и BLE примерно на 15-20dBm.<br>
&emsp; Если же экран не нужен, то нужно после программирования и настройки  esp32 подсоединить ее к источнику питания и спрятать где-нибудь на кухне.<br>

## 6. Совместимые готовые устройства
Если хочется запустить шлюз максимально быстро, без пайки, да еще и с приличным корпусом, стоит присмотреться к совместимым готовым устройствам. Ниже перечислены только проверенные мной устройства. Для прошивки использовалась программа [flash_download_tools](https://www.espressif.com/sites/default/files/tools/flash_download_tool_3.9.2.zip).

#### [TTGO T-Watcher BTC Ticker](https://aliexpress.ru/wholesale?catId=&SearchText=TTGO%20T-Watcher%20BTC%20Ticker)<br>
![PROJECT_PHOTO](https://github.com/alutov/ESP32-R4sGate-for-Redmond/blob/master/jpg/mytft4.jpg)
<br>Картинка 9. TTGO T-Watcher BTC Ticker.<br><br>
Я проверял работоспособность шлюза на TTGO T4 TFT BTC Ticker версии 1.3. Прошивается он через встроенный USB разъем, перед прошивкой устройства нужно соединить контакты 6 и 7 (gpio0 и gnd) в нижнем ряду разъема (картинка 10). Настройки экрана для версии 1.3: 12-MISO, 23-MOSI, 18-CLK, 27-CS, 32-DC, 5-RES, 4-LED, 0-T_CS, и для версии 1.2: 12-MISO, 23-MOSI, 18-CLK, 27-CS, 26-DC, 5-RES, 4-LED, 0-T_CS. В версии 1.2 нет управления включением и выключением экрана. Кнопки сверху вниз 38-Port1, 37-Port2, 39-Port3.
<br>
![PROJECT_PHOTO](https://github.com/alutov/ESP32-R4sGate-for-Redmond/blob/master/jpg/mytft5.jpg)
<br>Картинка 10. Соединить 6 и 7 пины разъема перед прошивкой TTGO T-Watcher BTC Ticker.<br><br>

#### [M5Stack BASIC Kit](https://docs.m5stack.com/en/core/basic)<br>
![PROJECT_PHOTO](https://github.com/alutov/ESP32-R4sGate-for-Redmond/blob/master/jpg/mytft6.jpg)
<br>Картинка 11. M5Stack BASIC Kit.<br><br>
Как я понял, старые версии M5Stack Basic шли с экраном на ili9341, и на этих версиях [работала и старая версия шлюза](https://github.com/alutov/ESP32-R4sGate-for-Redmond/issues/16). 
Настройки экрана для этой версии: 19-MISO, 23-MOSI, 18-CLK, 14-CS, 27-DC, 33-RES, 32-LED, 4-T_CS. Новые версии уже идут с экраном на ili9342. Начиная с версии 2021.10.29 добавлена поддержка экрана на ili9342. Я проверял работоспособность шлюза на новой версии M5Stack BASIC Kit. Прошивается он через встроенный USB разъем, перед прошивкой устройства нужно соединить последний контакт в верхнем ряду и 4 в нижнем ряду (gnd и gpio0) разъема (картинка 12). Настройки экрана для новой версии: 23-MISO, 23-MOSI, 18-CLK, 14-CS, 27-DC, 33-RES, 32-LED, 4-T_CS. Кнопки слева направо 39-Port1, 38-Port2, 37-Port3.<br>
![PROJECT_PHOTO](https://github.com/alutov/ESP32-R4sGate-for-Redmond/blob/master/jpg/mytft7.jpg)
<br>Картинка 12. Соединить последний контакт в верхнем ряду и 4 в нижнем ряду (gnd и gpio0) перед прошивкой M5Stack BASIC Kit.<br><br>

#### [ATOM-LITE-ESP32-DEVELOPMENT-KIT](https://docs.m5stack.com/en/core/atom_lite)<br>
![PROJECT_PHOTO](https://github.com/alutov/ESP32-R4sGate-for-Redmond/blob/master/jpg/mytft8.jpg)
<br>Картинка 13. ATOM-LITE-ESP32-DEVELOPMENT-KIT.<br><br>
     Прошивается атом по usb без установки перемычек. Кнопку использовал для включения-выключения одного из устройств (39-Port1), светодиод пока в прошивке не задействован. Устройство компактное (24 * 24 * 10 mm), devkit esp32 по размерам больше.
     
     
## 7. Сборка проекта и лицензия
&emsp; Для сборки бинарных файлов использован [Espressif IoT Development Framework.](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/). Добавлена лицензия MIT, что позволяет мне отказаться от всякой ответственности. Но не это главное. При написании прошивки год назад столкнулся с малым количеством примеров для этой среды разработки, по сравнению с тем же ардуино. Многое из того, что есть в ардуино, здесь пришлось писать самому. С другой стороны, тот же BLE  имеет большие возможности и работает надежнее, чем в ардуино. На красоту и эффективность кода не претендую, но можно что-то отсюда брать за основу в других Espressif IoT проектах.<br>
