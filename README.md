# README

This README would normally document whatever steps are necessary to get your application up and running.
NOTE: user for Raspberry = kspetrov :)

## Addons installed

* File editor - edit cfg from browser
* Samba share - edit files from explorer
* Terminal & SSH - just terminal for linux and manual installation

### Install HACS

For custom integration repos - https://hacs.xyz/docs/setup/download

After install use HA integration (add integration HACS).

* Yandex Smart Home
* Xiaomi Home
* Xiaomi Gateway 3
* PetKit
* Alarmo
* Alarmo Card

## HA integrations

* Alarmo - just testing for alarm panel (CUSTOM, not use yet)
* Broalink - for broadlink
* HACS - for custom integrations (CUSTOM)
* Matter - for other ecosystems (smart homes, ex. Aqara)
* MQTT - for domofon
* Mobile App - for phone with mobile app HA
* PetKit - for cat (CUSTOM)
* Raspberry Pi Power (Supply Checker)
* Xiaomi Home - for humidifier and vacuum cleaner (CUSTOM)
* Xiaomi Gateway 3 - for humidifier and vacuum cleaner (CUSTOM, not use yet)
* Yandex Smart Home - for integration with yandex (CUSTOM)

## Архитектура умного дома

### Общая схема

```mermaid
flowchart LR
    User["👤 Пользователь"]
    subgraph Apps["📱 Приложения экосистем"]
        EcosystemApps["Aqara · Xiaomi · Яндекс · Petkit · HA · Smartintercom"]
    end
    User --> Apps
    subgraph Aqara["🟢 Экосистема Aqara"]
        HubAqara["Aqara Hub (Zigbee)<br/>📍 Коридор"]
        subgraph DevicesAqara["Устройства"]
            MotionKorr["Датчик движения + освещённость<br/>📍 Коридор"]
            MotionKuhn["Датчик движения + освещённость<br/>📍 Кухня"]
            MotionGost["Датчик движения + освещённость<br/>📍 Гостиная"]
            Contact["Датчик открытия двери<br/>📍 Туалет"]
            Leak["Датчик протечки<br/>📍 Ванная"]
            Lock["Замок N100<br/>📍 Входная дверь"]
        end
        HubAqara ---|Zigbee| MotionKorr
        HubAqara ---|Zigbee| MotionKuhn
        HubAqara ---|Zigbee| MotionGost
        HubAqara ---|Zigbee| Contact
        HubAqara ---|Zigbee| Leak
        HubAqara ---|Zigbee| Lock
    end
    subgraph Lovolo["🟡 Управление светом Lovolo"]
        Broadlink["BroadLink RM4 Pro<br/>📍 Коридор"]
        subgraph DevicesLovolo["Устройства"]
            Lights["Сенсорные выключатели (2.4 ГГц)<br/>📍 В квартире"]
        end
        Broadlink ---|RF 2.4 ГГц| Lights
    end
    subgraph HABox["🏠 Raspberry Pi 4 — Коридор"]
        HA["Home Assistant"]
        MatterServer["Matter Server"]
        MQTTBroker["MQTT Broker (Mosquitto)"]
    end
    subgraph Yandex["🔴 Экосистема Яндекса"]
        HubYandex["Яндекс Хаб (Zigbee)<br/>📍 Коридор"]
        subgraph DevicesYandex["Устройства"]
            YaTempChild["Датчик температуры/влажности<br/>📍 Детская"]
            YaTempBed["Датчик температуры/влажности<br/>📍 Спальня"]
            YaThermChild["Термостат на батарее<br/>📍 Детская"]
            YaThermBed["Термостат на батарее<br/>📍 Спальня"]
            YaSpeakerChild["Умная колонка<br/>📍 Детская"]
            YaSpeakerKitchen["Умная колонка<br/>📍 Кухня"]
        end
        HubYandex ---|Zigbee| YaTempChild
        HubYandex ---|Zigbee| YaTempBed
        HubYandex ---|Zigbee| YaThermChild
        HubYandex ---|Zigbee| YaThermBed
    end
    subgraph Xiaomi["🟠 Экосистема Xiaomi"]
        subgraph DevicesXiaomi["Устройства"]
            Roborock["Пылесос Roborock<br/>📍 Кабинет"]
            Humidifier["Увлажнитель воздуха<br/>📍 Детская"]
            Cam1["Камера<br/>📍 Кухня"]
            Cam2["Камера<br/>📍 Спальня / коридор"]
        end
    end
    subgraph Petkit["🟣 Экосистема Petkit"]
        subgraph DevicesPetkit["Устройства"]
            Feeder["Автокормушка<br/>📍 Кухня"]
        end
    end
    subgraph Intercom["🔵 Smartintercom"]
        IntercomDevice["Умный домофон<br/>📍 Коридор"]
    end
    subgraph Clouds["☁️ Облачные сервисы"]
        AqaraCloud["Aqara Cloud"]
        YandexCloud["Yandex Cloud"]
        XiaomiCloud["Xiaomi Cloud"]
        PetkitCloud["Petkit Cloud"]
    end
    Apps --> AqaraCloud
    Apps --> YandexCloud
    Apps --> XiaomiCloud
    Apps --> PetkitCloud
    Apps --> IntercomDevice
    Apps --> HA
    HubAqara -->|Matter| MatterServer
    MatterServer --> HA
    HubAqara -.->|опционально| AqaraCloud
    HubYandex -->|Wi-Fi| YandexCloud
    YaSpeakerChild -->|Wi-Fi| YandexCloud
    YaSpeakerKitchen -->|Wi-Fi| YandexCloud
    HA -->|Yandex Smart Home| YandexCloud
    HA -->|команды по Wi-Fi| Broadlink
    Roborock -->|Wi-Fi| XiaomiCloud
    Humidifier -->|Wi-Fi| XiaomiCloud
    Cam1 -->|Wi-Fi| XiaomiCloud
    Cam2 -->|Wi-Fi| XiaomiCloud
    XiaomiCloud -->|"MIoT (облачная)"| HA
    Feeder -->|Wi-Fi| PetkitCloud
    PetkitCloud -->|облачная интеграция| HA
    IntercomDevice -->|Wi-Fi| MQTTBroker
    MQTTBroker --> HA
    style Apps fill:#455a64,color:white
    style Aqara fill:#4caf50,color:white
    style DevicesAqara fill:#388e3c,color:white
    style HubAqara fill:#1b5e20,color:white
    style Yandex fill:#fc3f1d,color:white
    style DevicesYandex fill:#d32f2f,color:white
    style HubYandex fill:#b71c1c,color:white
    style Lovolo fill:#ff9800,color:white
    style DevicesLovolo fill:#f57c00,color:white
    style Broadlink fill:#e65100,color:white
    style Xiaomi fill:#ff5722,color:white
    style DevicesXiaomi fill:#e64a19,color:white
    style Petkit fill:#8bc34a,color:black
    style DevicesPetkit fill:#689f38,color:white
    style Intercom fill:#9c27b0,color:white
    style Clouds fill:#607d8b,color:white
    style HABox fill:#37474f,color:white
    style HA fill:#03a9f4,color:white
    style MatterServer fill:#0288d1,color:white
    style MQTTBroker fill:#0288d1,color:white
```

### Реализованные сценарии автоматизации

#### 1. Свет в туалете по движению ночью
- **Триггер:** движение в коридоре + освещённость ниже порога (реализовано на aqara, далее virtual switch aqara по matter уходит в HA)
- **Условие:** свет в туалете выключен
- **Действие:** включить свет в туалете

#### 2. Выключение света в туалете
- **Триггер:** движение пропало на заданное время
- **Условие:** дверь туалета открыта определенное время
- **Действие:** выключить свет в туалете

#### 3. Away Mode (режим «ухожу из дома»)
- **Триггер:** включение `door_lock_away_mode` на замке N100
- **Действие:** выключить свет во всех комнатах

#### 4. Открытие замка снаружи
- **Триггер:** выключение `door_lock_away_mode` на замке N100
- **Действие:** включить свет в туалете и ванной

### Идеи для развития

#### Климат-контроль

| Сценарий | Триггер | Действие |
|----------|---------|----------|
| Поддержание температуры в детской | Датчик Яндекса < 21°C | Открыть термостат |
| Ночное понижение температуры | 23:00 – 07:00 | Установить 18°C на оба термостата |
| Увлажнение по расписанию | Каждые 2 часа днём | Включить увлажнитель на 15 минут |
| Слишком сухо | Влажность < 40% | Голосовое уведомление на колонку |

#### Безопасность

| Сценарий | Триггер | Действие |
|----------|---------|----------|
| Протечка в ванной | Датчик протечки → wet | Уведомление в HA + голос на колонку |
| Входная дверь открыта ночью | Замок открыт с 00:00 до 06:00 | Включить свет в коридоре + уведомление |
| Дверь долго открыта | Открыта > 2 минут | Уведомление на колонку |
| Движение ночью на камере | Камера в коридоре обнаружила движение | Отправить снимок в Telegram |

#### Уборка и рутина

| Сценарий | Триггер | Действие |
|----------|---------|----------|
| Уборка при уходе | Активация Away Mode | Roborock → уборка всей квартиры |
| Вернулся — пылесос на базу | Замок открыт снаружи | Roborock → dock |
| Кормление кошки | 09:00 и 21:00 | Petkit → выдать порцию |
| Низкий уровень корма | Уведомление от Petkit | Уведомление в HA + голос на колонку |

### Список устройств

#### Aqara (Zigbee → Aqara Hub → Matter → HA)

| Устройство | Расположение |
|------------|--------------|
| Aqara Hub | Коридор |
| Датчик движения + освещённость | Коридор |
| Датчик движения + освещённость | Кухня |
| Датчик движения + освещённость | Гостиная |
| Датчик открытия двери | Туалет |
| Датчик протечки | Ванная |
| Замок N100 | Входная дверь |

#### Lovolo + BroadLink

| Устройство | Расположение |
|------------|--------------|
| BroadLink RM4 Pro | Коридор |
| Сенсорные выключатели 2.4 ГГц | во всей квартире |

#### Xiaomi (облачная интеграция)

| Устройство | Расположение |
|------------|--------------|
| Roborock пылесос | Кабинет |
| Увлажнитель воздуха | Детская |
| Камера | Кухня |
| Камера | Спальня / коридор |

#### Яндекс (Zigbee → Яндекс Хаб + облако)

| Устройство | Расположение |
|------------|--------------|
| Яндекс Хаб | Коридор |
| Датчик температуры/влажности | Детская |
| Датчик температуры/влажности | Спальня |
| Термостат на батарее | Детская |
| Термостат на батарее | Спальня |
| Умная колонка | Детская |
| Умная колонка | Кухня |

#### Прочие устройства

| Устройство | Расположение | Интеграция |
|------------|--------------|-------------|
| Raspberry Pi 4 (8GB) | Коридор | Home Assistant OS |
| Smartintercom | Коридор | MQTT |
| Petkit автокормушка | Кухня | Облачная интеграция |

### Типы интеграций

| Связь | Тип | Направление |
|-------|-----|-------------|
| Aqara Hub → HA | Matter (локально) | Данные от датчиков |
| HA → Яндекс | Yandex Smart Home (облако) | Управление голосом |
| BroadLink → HA | Локальная интеграция | Управление светом |
| HA → BroadLink | Wi-Fi → RF 2.4 ГГц | Команды на выключатели |
| Xiaomi → HA | MIoT (облако) | Данные и управление |
| Petkit → HA | Облачная интеграция | Данные и управление |
| Smartintercom → HA | Wi-Fi → MQTT | Данные и управление |
| Яндекс Хаб → Яндекс Облако | Wi-Fi | Данные датчиков |

### Заметки

- Все датчики Aqara работают через Matter стабильно
- Устройства Яндекса не проброшены в HA (только HA → Яндекс)
- MQTT брокер (Mosquitto) работает на том же RPi4
- Для отладки MQTT: `mosquitto_sub -t "#" -v`

### HA tricks
* sometimes, may be after update core or OS DNS need to be restarted for access homeassistant.local:8123. Use command ha dns restart
* mobile app integration after del you need logout from mapp, restart HA and login to mapp
* Aqara Bridge - в основном для управления (Aqara Bridge решает другие задачи, а не задачу "получить статус датчика в реальном времени".). Не подходит
* Aqara Gateway - не подходит для моей модели хаба, только перепайка поможет
* Поэтому Aqara через matter. Локально зато, а не облако. Но есть проблемы
  * Не все датчики по matter дают состояние свое постоянно. Часто это импульс (вкл и сразу выкл). Причем в HA это прилетает только первый раз, а потом нужно длительное время чтобы опять эта штука отбросилась в HA
  * Поэтому пришлось на Aqara городить виртуальный выключатель (просто автоматизация левая), но зато можно отлавливат ее состояние вкл и выкл

---

*Актуально на 29.05.2026*
