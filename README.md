# russiancrt
Пакет RPM для установки сертификатов Минцифры.  

**Зависимости:** rootcerts openssl
  
[Стоит ли устанавливать российские сертификаты?](https://www.blancvpn.com/blog/stoit-li-ustanavlivat-rossiiskii-tls-sertifikat-s-gosuslug)  
  
[Сертификаты Минцифры](https://www.gosuslugi.ru/crt) для установки в Linux.  
  
**Примечание:** Mozilla Firefox не поддерживается (сертификаты ставятся вручную из браузера).  
  
ВНИМАНИЕ! ПЕРЕД УСТАНОВКОЙ ПАКЕТА СЕРТИФИКАТОВ ЗАКРОЙТЕ ВСЕ ОКНА ВАШЕГО БРАУЗЕРА!

В процессе установки пакета выполняется:
+ копирование сертификатов в `/usr/share/pki/ca-trust-source/anchors/`
+ апдейт хранилища сертификатов: `update-ca-trust`
+ консольный вывод подтверждения установки: `trust list | grep Russian`

Проверить установку сертификатов можно здесь: http://www.sberbank.ru/ru/certificates

## Ручная установка от Сбера
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
update-ca-certificates
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
