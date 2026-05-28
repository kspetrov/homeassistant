# README #

This README would normally document whatever steps are necessary to get your application up and running.
NOTE: user for Raspberry = homeassistant :)

### Addons installed ###

* File editor - edit cfg from browser
* Samba share - edit files from explorer
* Terminal & SSH - just terminal for linux and manual installation

### Install HACS for custom integration repos - https://hacs.xyz/docs/setup/download ###

After install use HA integration (add integration HACS).

* Yandex.Station
* Yandex Smart Home
* Google Home

### HA integrations ###

* Universal_remote - for broadlink
* HACS - for custom integrations (CUSTOM)
* Keenetic NDMS2 Router - for router
* Mobile App - for phone with mobile app HA
* Raspberry Pi Power (Supply Checker)
* Xiaomi Miio - for humidifier and vacuum cleaner
* Yandex Smart Home - for integration with yandex (CUSTOM)
* Yandex.Station - for manage yandex station (CUSTOM)

### HA tricks ###
* sometimes, may be after update core or OS DNS need to be restarted for access homeassistant.local:8123. Use command ha dns restart
* mobile app integration after del you need logout from mapp, restart HA and login to mapp
* todo - system monitor
* вход только по QR коду заработал для интеграции с Yandex.Station


Aqara Bridge - в основном для управления (Aqara Bridge решает другие задачи, а не задачу "получить статус датчика в реальном времени".)
Aqara Gateway - не подходит для моей модели хаба, только перепайка поможет
