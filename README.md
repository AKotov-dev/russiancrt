# russiancrt
[Стоит ли устанавливать российские сертификаты?](https://www.blancvpn.com/blog/stoit-li-ustanavlivat-rossiiskii-tls-sertifikat-s-gosuslug)  
  
**ВНИМАНИЕ!** Установка корневых сертификатов из пакета - это **крайний случай!** Установите «Яндекс Браузер», «Атом» и какой-либо другой отечественный браузер с вшитым Russian Trusted Root CA, чтобы заходить на сайты `Сбера`, `Госуслуги`, `mos.ru` и другие российские сайты, которые уже перешли или перейдут на сертификат Минцифры. Другие браузеры не будут на него «смотреть», так как в этом случае он установлен не на уровне ОС, а на уровне конкретного браузера.  
  
[Сертификаты Минцифры](https://www.gosuslugi.ru/crt) для установки в Linux  
  
**Зависимости:** rootcerts  
  
**Примечание:** Mozilla Firefox не поддерживается
  
ВНИМАНИЕ! ПЕРЕД УСТАНОВКОЙ ПАКЕТА СЕРТИФИКАТОВ ЗАКРОЙТЕ ВСЕ ОКНА ВАШЕГО БРАУЗЕРА!

В процессе установки пакета выполняется:
+ копирование сертификатов в `/usr/share/pki/ca-trust-source/anchors/`
+ апдейт хранилища сертификатов: `update-ca-trust`
+ очистка кеша и кукисов Firefox, Opera, Google Chrome, Chromium, Palemoon (см. [russiancrt.prj](https://github.com/AKotov-dev/russiancrt/blob/main/russiancrt.prj))
+ консольный вывод подтверждения установки: `trust list | grep Russian`

Проверить установку сертификатов можно здесь: http://www.sberbank.ru/ru/certificates
