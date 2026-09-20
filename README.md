# russiancrt
- **Требования к системе:** libnss >= v3.122; Mageia-10+, Fedora-44+, Debian-Sid+
- **Важно!** Если сертификаты Минцифры ставились ранее вручную, перед установкой пакета удалите их!
 
Содержимое пакета RPM:
- /usr/share/pki/ca-trust-source/anchors/russian_secured_private_root_ca.crt
- /usr/share/pki/ca-trust-source/anchors/russian_constrained_intermediate_ca.crt
- /usr/share/doc/russiancrt/

Содержимое пакета DEB:
- /usr/local/share/ca-certificates/russian_secured_private_root_ca.crt
- /usr/local/share/ca-certificates/russian_constrained_intermediate_ca.crt
- /usr/share/doc/russiancrt/

**Для установки в chromium-браузерах:**
- russian_secured_private_root_ca.crt - Доверенный сертификат
- russian_constrained_intermediate_ca.crt - Промежуточный сертификат

## Безопасные сертификаты

Начиная с версии **v2022-3**, пакет `russiancrt` содержит модифицированные сертификаты доверия Минцифры с ограничением области применения посредством механизма **X.509 `nameConstraints`**.  
  
Ограничения разрешают использование сертификатной цепочки только для доменных зон:

* `.ru`
* `.su`
* `.рф` (`.xn--p1ai`)

Все последующие обновления сертификатов также будут обрабатываться с помощью [этого](https://habr.com/ru/articles/1071256/) механизма.  
  
![](https://github.com/AKotov-dev/russiancrt/blob/main/test4.png)
  
Генерация модифицированных сертификатов выполняется [скриптом](https://github.com/AKotov-dev/russiancrt/blob/main/secure-russiancrt.tar.gz), входящим в состав репозитория.  
Исходный закрытый ключ УЦ не используется и не требуется.  
  
**Примечание:**
- В Mageia/Fedora - Firefox берёт настройки сам из системного хранилища; для Chromium-браузеров нужно зайти в менеджер сертификатов, например `brave://certificate-manager/`, выключить переключатель `Использовать локальные сертификаты, импортированные из операционной системы` и добавить сертификаты в `Установленные вами` (Доверенный и Промежуточный. см. выше).
- В Debian-Sid - на уровне системы сертификаты будут видны; в браузерах оба сертификата нужно устанавливать вручную.
  
Проверить действие ограничений можно с помощью [test_gui](https://github.com/AKotov-dev/russiancrt/blob/main/test_gui.tar.gz) (требуется gtk2).  
  
...или просто зайти на сайты:
- https://sberbank.ru (должен открываться)
- https://sberbank.com (должна быть ошибка)
---
### Архивная информация (предыдущая, небезопасная редакция)
  
[Стоит ли устанавливать российские сертификаты?](https://www.blancvpn.com/blog/stoit-li-ustanavlivat-rossiiskii-tls-sertifikat-s-gosuslug)  
  
[Сертификаты Минцифры — как им не доверять](https://habr.com/ru/articles/1071256/)
  
[Техническое: локальный корневой сертификат с кросс-подписью и nameConstraints](https://dxdt.blog/2026/08/22/18987/?utm_source=chatgpt.com)  
  
[Сертификаты Минцифры](https://www.gosuslugi.ru/crt) для установки в Linux.  
  
**Примечание:** Mozilla Firefox не поддерживается (сертификаты ставятся вручную из браузера).  
  
ВНИМАНИЕ! ПЕРЕД УСТАНОВКОЙ ПАКЕТА СЕРТИФИКАТОВ ЗАКРОЙТЕ ВСЕ ОКНА ВАШЕГО БРАУЗЕРА!

В процессе установки пакета выполняется:
+ копирование сертификатов в `/usr/share/pki/ca-trust-source/anchors/`
+ апдейт хранилища сертификатов: `update-ca-trust`
+ консольный вывод подтверждения установки: `trust list | grep Russian`

Проверить установку сертификатов можно здесь: http://www.sberbank.ru/ru/certificates

### Ручная установка от Сбера
Инструкция: https://www.sberbank.ru/ru/certificates/linux

### Обновление вручную (Mageia)
```
su/password
mkdir -p /usr/share/pki/ca-trust-source/anchors
cd /usr/share/pki/ca-trust-source/anchors
```
### Скачать корневой сертификат
```
wget https://gu-st.ru/content/lending/russian_trusted_root_ca_pem.crt
```
### Скачать выпускающий сертификат
```
wget https://gu-st.ru/content/lending/russian_trusted_sub_ca_pem.crt
```
### Апдейт хранилища сертификатов
```
update-ca-trust
```
### Проверка установки сертификатов
```
trust list | grep Russian
```
### Проверка даты окончания действия сертификатов
```
openssl x509 -enddate -noout -in ./russian_trusted_sub_ca_pem.crt
openssl x509 -enddate -noout -in ./russian_trusted_root_ca_pem.crt
```
