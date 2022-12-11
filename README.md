# russiancrt
[Сертификаты Минцифры](https://www.gosuslugi.ru/crt) для установки в Linux (rpm/deb)

**Зависимости:** rootcerts

ВНИМАНИЕ! ПЕРЕД УСТАНОВКОЙ ПАКЕТА СЕРТИФИКАТОВ ЗАКРОЙТЕ ВСЕ ОКНА ВАШЕГО БРАУЗЕРА!

В процессе установки пакета выполняется:
+ копирование сертификатов в `/usr/share/pki/ca-trust-source/anchors/`
+ апдейт хранилища сертификатов: `update-ca-trust`
+ очистка кеша и кукисов браузеров Firefox, Opera, Google Chrome, Chromium, Palemoon (см. [russiancrt.prj](https://github.com/AKotov-dev/russiancrt/blob/main/russiancrt.prj))
+ консольный вывод подтверждения установки: `trust list | grep Russian`

Проверить установку сертификатов можно здесь: http://www.sberbank.ru/ru/certificates
