<!-- TITLE: Home -->
<!-- SUBTITLE: A quick summary of Home -->

# Адаптер (драйвер) ioBroker для работы с Zigbee-устройствами

Драйвер использует библиотеку https://github.com/zigbeer/zigbee-shepherd реализующую шлюз (координатор) Zigbee-сети на базе SoC TI CC253x (и другими).

## MQTT варианты

Помимо драйвера для ioBroker, есть вариант реализации Zigbee-MQTT-шлюза с использованием устройств TI CC253x.

* https://github.com/Koenkk/zigbee2mqtt - активно развивающийся проект, ориентированный в первую очередь для интеграции с HomeAssistant
* https://github.com/Frans-Willem/AqaraHub - проект на C++, активно стартовал, но почему-то приостановил развитие

## Плюсы и минусы в сравнении со шлюзом Xiaomi

Основано на [заметке aspector на 4pda](http://4pda.ru/forum/index.php?showtopic=794186&view=findpost&p=70553896 )

Шлюз Xiaomi | cc253x (стик)
------------| -------------
![#f03c15](https://placehold.it/15/f03c15/000000?text=+) Нужен аккаунт Xiaomi для начальной настройки в MiHome | ![#c5f015](https://placehold.it/15/c5f015/000000?text=+) Независимость от аккаунта Xiaomi, Китая, наличия интернета
![#f03c15](https://placehold.it/15/f03c15/000000?text=+) Работает только с устройствами Xiaomi | ![#c5f015](https://placehold.it/15/c5f015/000000?text=+) Возможность работы с другими Zigbee-устройствами (не только Xiaomi, уже проверено в IKEA TRÅDFRI bulb и FLOALT panel)
![#f03c15](https://placehold.it/15/f03c15/000000?text=+) Ограниченный (~10м) радиус доступности устройств | ![#c5f015](https://placehold.it/15/c5f015/000000?text=+) Варианты с внешней антенной могут "услышать" устройства на достаточно большом расстоянии (~50м)
![#f03c15](https://placehold.it/15/f03c15/000000?text=+) Стоимость ~ $30 | ![#c5f015](https://placehold.it/15/c5f015/000000?text=+) Стоимость ~ $10
![#c5f015](https://placehold.it/15/c5f015/000000?text=+) Отличный внешний вид | ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) Голое устройство, с корпусом стоят дороже
![#c5f015](https://placehold.it/15/c5f015/000000?text=+) Свободная возможность размещения (там где есть розетка) и доступ по Wifi | ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) Вставляется в компьютер (или микрокомпьютер/одноплатник/малина) и зависит от его расположения
![#c5f015](https://placehold.it/15/c5f015/000000?text=+) Благодаря MiHome может управлять устройствами как со смартфона, так и из сторонней системы "Умного дома" | ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) Требует спец драйвер (софт), чтобы управлять устройствами из системы "Умного дома". Управление с телефона - только через систему "Умного дома".
![#1589F0](https://placehold.it/15/1589F0/000000?text=+) Сценарии работы устройств настраиваются в MiHome или в системе "Умного дома" | ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) Сценарии работы устройств настраиваются в системе "Умного дома"
![#c5f015](https://placehold.it/15/c5f015/000000?text=+) Можно использовать встроенную лампу, радио и датчик освещенности. Агрегирует также wifi-устройства Xiaomi | ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) Нет функций, кроме Zigbee
![#c5f015](https://placehold.it/15/c5f015/000000?text=+) Не требует специальной доработки и работает "из коробки" | ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) **Требуется специальная [[Прошивка]], без нее не работает.** Либо приобрести уже прошитый стик (в личку), но это увеличивает стоимость.

## Подготовка оборудования

Устройства SoC TI CC253x должны быть прошиты специальной ZNP-прошивкой (Zigbee network processor).

[[Прошивка]]

## Настройка адаптера

Для запуска адаптера необходимо указать порт, на котором подключен CC253x.

### Для Windows это порты COM**

Их можно найти в Диспетчере устройств:

![](https://github.com/kirovilya/files/blob/master/win-cc-port.PNG)

[Windows-драйвер для cc253x](https://github.com/kirovilya/files/blob/master/swrc088c.zip)

### Для Linux систем порт обычно бывает /dev/ttyACM0, либо /dev/ttyUSB0 (для UART подключения)

Если имя порта неизвестно, установите [serialport](https://www.npmjs.com/package/serialport) глобально:

`$ npm install -g @serialport/list`

затем в командной строке выполните команду `serialport-list` для получения списка доступных портов:

```
$ serialport-list
/dev/ttyACM0    usb-Texas_Instruments_TI_CC2531_USB_CDC___0X00124B000106B6C5-if00   Texas_Instruments
/dev/ttyS0
/dev/ttyS1
```

### Для контейнера Synology

Нужно пробросить внешний порт:

1. Запускаем контейнер с повышенным приоритетом

![](https://github.com/kirovilya/files/blob/master/308829c7-22e6-48a3-9d63-38b1789d08ea.jpg)

2. Коннектимся к Synology по SSH и даём команду
sudo chmod 777 /dev/ttyACM0

3. Ребутим контейнер

## Поддерживаемые устройства

Проверена работа следующих устройств:


Устройство | -
------------| -------------
IKEA FLOALT LED light panel (state, level, colortemp) | ![](https://github.com/kirovilya/ioBroker.zigbee/blob/master/admin/img/FLOALT.panel.WS.png)
IKEA TRÅDFRI bulb (state, level, colortemp) | ![](https://github.com/kirovilya/ioBroker.zigbee/blob/master/admin/img/TRADFRI.bulb.E27.png)
QBCZ11LM Aqara Smart Socket ZiGBee (state, load power, in use) | ![](https://github.com/kirovilya/ioBroker.zigbee/blob/master/admin/img/plug.png)
QBKG11LM Xiaomi Aqara Smart Wall Switch Line-Neutral Single-Button (click, state, load power) | ![](https://github.com/kirovilya/ioBroker.zigbee/blob/master/admin/img/ctrl_neutral1.png)
JTYJ-GD-01LM/BW Xiaomi Smoke Alarm (detected, voltage) | ![](https://github.com/kirovilya/ioBroker.zigbee/blob/master/admin/img/smoke.png)
ZNCZ02LM Xiaomi Smart Power Plug (state, load power, in use) | ![](https://github.com/kirovilya/ioBroker.zigbee/blob/master/admin/img/86plug.png)
QBKG03LM Xiaomi Aqara Light Switch (left is on, right is on, click left, click right, click both) | ![](https://github.com/kirovilya/ioBroker.zigbee/blob/master/admin/img/ctrl_ln2.png)
MFKZQ01LM Xiaomi Magic Cube Controller (shake, slide, flip90, flip180, tap, rotate, fall, wakeup, voltage) | ![](https://github.com/kirovilya/ioBroker.zigbee/blob/master/admin/img/cube.png)
SJCGQ11LM Aqara Smart Water Sensor (detected, voltage) | ![](https://github.com/kirovilya/ioBroker.zigbee/blob/master/admin/img/sensor_wleak_aq1.png)
WXKG02LM Aqara Smart Light Switch Wireless (click left, click right, click both, voltage) | ![](https://github.com/kirovilya/ioBroker.zigbee/blob/master/admin/img/86sw2.png)
WSDCGQ11LM Aqara Temperature Humidity Sensor (humidity, pressure, temperature, voltage) | ![](https://github.com/kirovilya/ioBroker.zigbee/blob/master/admin/img/aqara_temperature_sensor.png)
WSDCGQ01LM Xiaomi Smart Temperature Humidity Sensor (humidity, temperature, voltage) | ![](https://github.com/kirovilya/ioBroker.zigbee/blob/master/admin/img/sensor_ht.png)
MCCGQ11LM Aqara Window Door Sensor (contact, voltage) | ![](https://github.com/kirovilya/ioBroker.zigbee/blob/master/admin/img/sensor_magnet_aq2.png)
MCCGQ01LM Xiaomi Mi Smart Door/Window Sensor (contact, voltage)<br/><br/><sup>При подключении (спаривании) скрепкой нажать на кнопку подключения на 5 секунд (датчик переходит в режим спаривания и диод моргнет).<br/>Далее, необходимо регулярно раз в 2-3 секунды нажимать скрепкой эту же кнопку подключения, чтобы устройство оставалось активным до окончания подключения (или окончания отсчета).</sup> | ![](https://github.com/kirovilya/ioBroker.zigbee/blob/master/admin/img/contact.png)
WXKG11LM Aqara Smart Wireless Switch (click, double click, voltage) | ![](https://github.com/kirovilya/ioBroker.zigbee/blob/master/admin/img/aqara.switch.png)
WXKG01LM Xiaomi Smart Wireless Switch (click, double click, triple, long click, voltage)<br/><br/><sup>При подключении (спаривании) скрепкой нажать на кнопку подключения на 5 секунд (датчик переходит в режим спаривания и диод моргнет).<br/>Далее, необходимо регулярно раз в 2-3 секунды нажимать скрепкой эту же кнопку подключения, чтобы устройство оставалось активным до окончания подключения (или окончания отсчета).</sup> | ![](https://github.com/kirovilya/ioBroker.zigbee/blob/master/admin/img/xiaomi_wireless_switch.png)
RTCGQ11LM Aqara Human Body Sensor (illuminance, occupancy, voltage) | ![](https://github.com/kirovilya/ioBroker.zigbee/blob/master/admin/img/aqara_numan_body_sensor.png)
RTCGQ01LM Xiaomi Mi Smart IR Human Body Sensor (illuminance, occupancy, voltage)<br/><br/><sup>При подключении (спаривании) скрепкой нажать на кнопку подключения на 5 секунд (датчик переходит в режим спаривания и диод моргнет).<br/>Далее, необходимо регулярно раз в 2-3 секунды нажимать скрепкой эту же кнопку подключения, чтобы устройство оставалось активным до окончания подключения (или окончания отсчета).</sup> | ![](https://github.com/kirovilya/ioBroker.zigbee/blob/master/admin/img/motion.png)

**Особенности подключения некоторых устройств!**

* RTCGQ01LM Xiaomi Mi Smart IR Human Body Sensor 
* MCCGQ01LM Xiaomi Mi Smart Door/Window Sensor
* WXKG01LM Xiaomi Mi Smart Home Wireless Switch

При подключении (спаривании) перечисленных устройств к координатору (шлюзу, стику):
1. скрепкой нажать на кнопку подключения на 5 секунд (датчик переходит в режим спаривания)
2. необходимо регулярно раз в 2-3 секунды нажимать скрепкой кнопку подключения, чтобы устройство оставалось активным до окончания подключения (или окончания отсчета).

* IKEA TRÅDFRI bulb и FLOALT LED light panel

1. отвязываем лампу от всего (5 раз вкл-выкл) и оставляем выключенной
2. подносим близко к устройству (cc253x) 
3. запускаем на процесс спаривания
4. включаем лампу 
5. смотрим логи и ждем
