## russiancrt - безопасные сертификаты Минцифры
Пакеты `russiancrt` (RPM / DEB) содержат модифицированные сертификаты доверия Минцифры с ограничением области применения посредством механизма **X.509 `nameConstraints`**.  
  
Ограничения разрешают использование сертификатной цепочки только для доменных зон:

* `.ru`
* `.su`
* `.рф` (`.xn--p1ai`)

Все последующие обновления сертификатов также будут обрабатываться с помощью [этого](https://habr.com/ru/articles/1071256/) механизма.  

**Требования к системе:** libnss >= v3.122; Mageia-10+, Fedora-44+, Ubuntu-26+, Debian-13.7+  
> [!IMPORTANT]
> **Если сертификаты Минцифры ставились ранее вручную, перед установкой пакета удалите их!**

---

**Содержимое пакета RPM:**
- /usr/share/pki/ca-trust-source/anchors/russian_secured_private_root_ca.crt
- /usr/share/pki/ca-trust-source/anchors/russian_constrained_intermediate_ca.crt
- /usr/share/doc/russiancrt/

**Содержимое пакета DEB:**
- /usr/local/share/ca-certificates/russian_secured_private_root_ca.crt
- /usr/local/share/ca-certificates/russian_constrained_intermediate_ca.crt
- /usr/share/doc/russiancrt/

**Для установки в Chromium-браузерах:**
- russian_secured_private_root_ca.crt - Доверенный сертификат
- russian_constrained_intermediate_ca.crt - Промежуточный сертификат
  
![](https://github.com/AKotov-dev/russiancrt/blob/main/test5.png)
  
Генерация модифицированных сертификатов выполнялась [скриптом](https://github.com/AKotov-dev/russiancrt/blob/main/secure-russiancrt.tar.gz), входящим в состав репозитория.  
Исходный закрытый ключ УЦ не используется и не требуется.  
  

**Порядок установки / использование:**

* **Mageia/Fedora** — Firefox использует системное хранилище сертификатов автоматически. Для Chromium-браузеров нужно зайти в менеджер сертификатов, например `brave://certificate-manager/`, отключить переключатель **«Использовать локальные сертификаты, импортированные из операционной системы»** и добавить сертификаты в **«Установленные вами»**: корневой — в **«Доверенный»**, промежуточный — в **«Промежуточный»** (см. выше).

* **Debian/Ubuntu** — Chromium-браузеры, как и в Mageia/Fedora требуют ручной установки сертификатов: `Доверенный` и `Промежуточный` (см. выше).

* **Debian/Ubuntu + Firefox** — одного системного `ca-certificates` недостаточно, если Firefox не подключён к системному хранилищу. Для Firefox, использующего собственный NSS, необходимо установить `p11-kit`/`p11-kit-modules` и подключить в **«Устройства безопасности»** модуль:

  ```text
  /usr/lib/x86_64-linux-gnu/pkcs11/p11-kit-trust.so
  ```

  После подключения Firefox получает сертификаты из системного **System Trust** и применяет `Name Constraints`.

* **Firefox Snap в Ubuntu** — особенно важно подключить `p11-kit-trust.so`: Snap Firefox использует собственный NSS и без этого модуля системные сертификаты могут не использоваться Firefox автоматически.

* После перехода на системное хранилище **необходимо удалить импортированные ранее сертификаты** (ещё раз), очистить историю и перезапустить Firefox. Теперь он будет читать их автоматически.

* Для проверки конечного результата:

  * https://sberbank.ru — должен открываться;
  * https://sberbank.com — должен блокироваться с ошибкой вида `SEC_ERROR_CERT_NOT_IN_NAME_SPACE`.

Проверить действие ограничений можно с помощью [test_gui](https://github.com/AKotov-dev/russiancrt/blob/main/test_gui.tar.gz) (требуется gtk2; см. скриншот).

---

<details>
<summary>Архивная информация (общий порядок установки оригинальных сертификатов)...</summary>

#### 

[Сертификаты Минцифры](https://www.gosuslugi.ru/crt) для установки в Linux.  
  
[Сертификаты Минцифры — как им не доверять](https://habr.com/ru/articles/1071256/)  
  
[Стоит ли устанавливать российские сертификаты?](https://www.blancvpn.com/blog/stoit-li-ustanavlivat-rossiiskii-tls-sertifikat-s-gosuslug)  
  
[Намордник - Ограниченное доверие корню Минцифры](https://github.com/ijustbsd/namordnik)  
  
[Техническое: локальный корневой сертификат с кросс-подписью и nameConstraints](https://dxdt.blog/2026/08/22/18987/?utm_source=chatgpt.com)  
  
**ВНИМАНИЕ!** ПЕРЕД УСТАНОВКОЙ ПАКЕТА СЕРТИФИКАТОВ ЗАКРОЙТЕ ВСЕ ОКНА ВАШЕГО БРАУЗЕРА!

**В процессе установки пакета выполняется:**
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

</details>
