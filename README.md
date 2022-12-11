# russiancrt
Сертификаты Минцифры для установки (rpm/deb)

**Зависимости:** rootcerts

В процессе установки пакета выполняется:
+ копирование сертификатов в `/usr/share/pki/ca-trust-source/anchors/`
+ апдейт хранилища сертификатов: `update-ca-trust`
+ очистка кеша и кукисов браузеров Firefox, Opera, Google Chrome, Chromium, Palemoon (см. russiancrt.prj)
+ консольный вывод подтверждения установки: `trust list | grep Russian`

Проверить установку сертификатов можно здесь: http://www.sberbank.ru/ru/certificates
