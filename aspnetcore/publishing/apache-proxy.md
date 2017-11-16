---
title: "Размещение ASP.NET Core в операционной системе Linux с Apache"
description: "В статье описывается процедура настройки Apache как обратного прокси-сервера в CentOS для перенаправления трафика HTTP в веб-приложение ASP.NET Core, выполняемое в Kestrel."
keywords: ASP.NET Core,Apache,CentOS,Reverse Proxy,Linux,mod_proxy,httpd,hosting
author: spboyer
ms.author: spboyer
manager: wpickett
ms.date: 10/19/2016
ms.topic: article
ms.assetid: fa9b0cb7-afb3-4361-9e7e-33afffeaca0c
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/apache-proxy
ms.openlocfilehash: 34ede2fdebe0e9516f62e39618e7adba3c8c89db
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-linux-with-apache-and-deploy-to-it"></a>Настройка среды размещения для ASP.NET Core в операционной системе Linux с Apache и развертывание в эту среду

Автор: [Шейн Бойер (Shayne Boyer)](https://github.com/spboyer)

Apache является популярным HTTP-сервером и может быть настроен как прокси-сервер для перенаправления трафика HTTP аналогично Nginx. В этом руководстве вы узнаете, как установить Apache в CentOS 7 и использовать его в качестве обратного прокси-сервера для входящих подключений и их перенаправления в приложение ASP.NET Core в Kestrel. Для этого будет использоваться расширение *mod_proxy* и другие связанные модули Apache.

## <a name="prerequisites"></a>Предварительные требования

1. Сервер под управлением CentOS 7 с учетной записью обычного пользователя с правами sudo.
2. Существующее приложение ASP.NET Core. 

## <a name="publish-your-application"></a>Публикация приложения

Запустите `dotnet publish -c Release` из среды разработки, чтобы упаковать приложение в автономный каталог, который может выполняться на сервере. Затем опубликованное приложение необходимо скопировать на сервер с помощью SCP, FTP или другого метода передачи файлов. 

> [!NOTE]
> В сценариях развертывания в рабочей среде рабочий процесс непрерывной интеграции осуществляет публикацию приложения и копирование ресурсов на сервер. 

## <a name="configure-a-proxy-server"></a>Настройка прокси-сервера

Обратный прокси-сервер — это общая настройка для обслуживания динамических веб-приложений. Обратный прокси-сервер завершает HTTP-запрос и перенаправляет его в приложение ASP.NET Core.

Прокси-сервер перенаправляет запросы клиента на другой сервер, а не выполняет их сам. Обратный прокси-сервер перенаправляет запросы в фиксированное назначение обычно от имени клиентов. В этом руководстве Apache настраивается в качестве обратного прокси-сервера, работающего на том же сервере, где Kestrel обслуживает приложение ASP.NET Core. 

В зависимости от архитектурных потребностей или ограничений каждая часть приложения может существовать на разных физических компьютерах, в контейнерах Docker или сочетании конфигураций.

### <a name="install-apache"></a>Установка Apache

Установка веб-сервера Apache в CentOS осуществляется с помощью выполнения одной команды, однако сначала следует обновить пакеты.

```bash
    sudo yum update -y
```

Все установленные пакеты необходимо обновить до их последней версии. Установка Apache с помощью `yum`

```bash
    sudo yum -y install httpd mod_ssl
```

Выходные данные должны выглядеть примерно следующим образом.

```bash
    Downloading packages:
    httpd-2.4.6-40.el7.centos.4.x86_64.rpm               | 2.7 MB  00:00:01     
    Running transaction check
    Running transaction test
    Transaction test succeeded
    Running transaction
    Installing : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 
    Verifying  : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 

    Installed:
    httpd.x86_64 0:2.4.6-40.el7.centos.4                                                                           

    Complete!
```

> [!NOTE]
> В этом примере в выходных данных отображается httpd.86_64, так как используется 64-разрядная версия CentOS 7. Для вашего сервера могут выводиться другие выходные данные. Чтобы проверить, где установлен Apache, выполните `whereis httpd` из командной строки. 

### <a name="configure-apache-for-reverse-proxy"></a>Настройка Apache в качестве обратного прокси-сервера

Файлы конфигурации для Apache находятся в каталоге `/etc/httpd/conf.d/`. Все файлы с расширением **.conf** будут обрабатываться в алфавитном порядке, кроме файлов конфигурации модуля в `/etc/httpd/conf.modules.d/`, где содержатся файлы конфигурации, необходимые для загрузки модулей.

Создайте файл конфигурации приложения. В этом примере он будет назван `hellomvc.conf`.

```text
    <VirtualHost *:80>
        ProxyPreserveHost On
        ProxyPass / http://127.0.0.1:5000/
        ProxyPassReverse / http://127.0.0.1:5000/
        ErrorLog /var/log/httpd/hellomvc-error.log
        CustomLog /var/log/httpd/hellomvc-access.log common
    </VirtualHost>
```

Узел *VirtualHost*, экземпляров которого может быть несколько в файле или на сервере во многих файлах, настроен для прослушивания всех IP-адресов на порту 80. Следующие две строки заданы для передачи всех запросов, полученных в корне, на порт компьютера 127.0.0.1 5000 и в обратном направлении. Чтобы этот обмен данными был двунаправленным, требуются параметры *ProxyPass* и *ProxyPassReverse*.

С помощью директив *ErrorLog* и *CustomLog* для каждого VirtualHost можно настроить ведение журнала. *ErrorLog* — это расположение, где сервер будет регистрировать ошибки, а *CustomLog* задает имя файла и формат файла журнала. В нашем случае это расположение, где будут фиксироваться сведения о запросах. Для каждого запроса выделяется одна строка.

Сохраните файл и протестируйте конфигурацию. Если проверка выполнена успешно, ответ должен быть `Syntax [OK]`.

```bash
    sudo service httpd configtest
```

Перезапустите Apache.

```text
    sudo systemctl restart httpd
    sudo systemctl enable httpd
```

## <a name="monitoring-our-application"></a>Мониторинг приложения

Теперь Apache настроен на перенаправление запросов, отправленных в `http://localhost:80`, в приложение ASP.NET Core, выполняемое в Kestrel по адресу `http://127.0.0.1:5000`.  Однако Apache не настроен для управления процессом Kestrel. Для запуска и мониторинга соответствующего веб-приложения можно воспользоваться *systemd* и создать файл службы. *systemd* — это система инициализации, предоставляющая различные функции для запуска и остановки процессов, а также управления ими. 


### <a name="create-the-service-file"></a>Создание файла службы

Создайте файл определения службы: 

```bash
    sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

Пример файла службы для нашего приложения.

```text
[Unit]
    Description=Example .NET Web API Application running on CentOS 7

    [Service]
    WorkingDirectory=/var/aspnetcore/hellomvc
    ExecStart=/usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
    Restart=always
    # Restart service after 10 seconds if dotnet service crashes
    RestartSec=10
    SyslogIdentifier=dotnet-example
    User=apache
    Environment=ASPNETCORE_ENVIRONMENT=Production 

    [Install]
    WantedBy=multi-user.target
```

> [!NOTE]
> **Пользователь**. Если пользователь *apache* в вашей конфигурации не используется, необходимо сначала создать определенного выше пользователя, а затем предоставить ему необходимые права владения для файлов.

Сохраните файл и включите службу.

```bash
    systemctl enable kestrel-hellomvc.service
```

Запустите службу и убедитесь, что она работает.

```
    systemctl start kestrel-hellomvc.service
    systemctl status kestrel-hellomvc.service

    ● kestrel-hellomvc.service - Example .NET Web API Application running on CentOS 7
        Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
        Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
    Main PID: 9021 (dotnet)
        CGroup: /system.slice/kestrel-hellomvc.service
                └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

Теперь, когда обратный прокси-сервер настроен, а управление Kestrel осуществляется системой systemd, веб-приложение полностью настроено и готово для доступа из браузера на локальном компьютере по адресу `http://localhost`. Если в ответах есть заголовок **Server**, это означает, что приложение ASP.NET Core обслуживает Kestrel.

```text
    HTTP/1.1 200 OK
    Date: Tue, 11 Oct 2016 16:22:23 GMT
    Server: Kestrel
    Keep-Alive: timeout=5, max=98
    Connection: Keep-Alive
    Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>Просмотр журналов

Поскольку веб-приложение, использующее Kestrel, управляется с помощью systemd, все события и процессы регистрируются в централизованном журнале. При этом журнал содержит все записи обо всех службах и процессах, управляемых systemd. Чтобы просмотреть элементы, связанные с `kestrel-hellomvc.service`, используйте следующую команду.

```bash
    sudo journalctl -fu kestrel-hellomvc.service
```

Кроме того, количество возвращаемых записей можно уменьшить, указав параметры времени, например `--since today`, `--until 1 hour ago` или их комбинацию.

```bash
    sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-our-application"></a>Защита приложения

### <a name="configure-firewall"></a>Настройка брандмауэра

*Firewalld* — это динамическая программа для управления брандмауэром с поддержкой зон сети, несмотря на то, что для управления портами и фильтрацией пакетов можно по-прежнему использовать служебную программу iptables. Программа Firewalld должна устанавливаться по умолчанию, `yum` можно использовать для установки пакета или проверки.

```bash
    sudo yum install firewalld -y
```

С помощью `firewalld` можно открывать только порты, необходимые для работы приложения. В этом случае используются порты 80 и 443. Следующие команды позволяют держать их постоянно открытыми.

```bash
    sudo firewall-cmd --add-port=80/tcp --permanent
    sudo firewall-cmd --add-port=443/tcp --permanent
```

Перезагрузите параметры брандмауэра и проверьте, что доступные службы и порты находятся в зоне по умолчанию. Параметры доступны при просмотре `firewall-cmd -h`.

```bash 
    sudo firewall-cmd --reload
    sudo firewall-cmd --list-all
```

```bash
    public (default, active)
    interfaces: eth0
    sources: 
    services: dhcpv6-client
    ports: 443/tcp 80/tcp
    masquerade: no
    forward-ports: 
    icmp-blocks: 
    rich rules: 
```

### <a name="ssl-configuration"></a>Конфигурация SSL

Для настройки Apache для SSL служит модуль mod_ssl.  Он был первоначально установлен при установке модуля `httpd`. Если его не было или он не был установлен, используйте yum, чтобы добавить его в конфигурацию.

```bash
    sudo yum install mod_ssl
```
Чтобы применить SSL, установите `mod_rewrite`.

```bash
    sudo yum install mod_rewrite
```

Файл `hellomvc.conf`, который был создан в этом примере, следует изменить для включения перезаписи, а также добавления нового раздела **VirtualHost** для HTTPS.

```text
    <VirtualHost *:80>
        RewriteEngine On
        RewriteCond %{HTTPS} !=on
        RewriteRule ^/?(.*) https://%{SERVER_NAME}/ [R,L]
    </VirtualHost>

    <VirtualHost *:443>
        ProxyPreserveHost On
        ProxyPass / http://127.0.0.1:5000/
        ProxyPassReverse / http://127.0.0.1:5000/
        ErrorLog /var/log/httpd/hellomvc-error.log
        CustomLog /var/log/httpd/hellomvc-access.log common
        SSLEngine on
        SSLProtocol all -SSLv2
        SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
        SSLCertificateFile /etc/pki/tls/certs/localhost.crt
        SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
    </VirtualHost>    
```

> [!NOTE]
> В этом примере используется локально созданный сертификат. **SSLCertificateFile** должен быть основным файлом сертификата для имени домена. **SSLCertificateKeyFile** должен быть файлом ключа, формируемым при создании CSR. **SSLCertificateChainFile** должен быть файлом промежуточного сертификата (если таковой имеется), предоставленным центром сертификации.

Сохраните файл и протестируйте конфигурацию.

```bash
    sudo service httpd configtest
```

Перезапустите Apache.

```bash
    sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a>Дополнительные предложения Apache

### <a name="additional-headers"></a>Дополнительные заголовки 
Для защиты от вредоносных атак существует несколько заголовков, которые следует изменить или добавить. Убедитесь, что модуль `mod_headers` установлен.

```bash
    sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking"></a>Защита Apache от кликджекинга
Кликджекинг — это способ обмана, предназначенный для перехвата щелчков мыши пострадавшего пользователя. Кликджекинг обманом заставляет пользователей (посетителей) щелкать зараженные сайты. Для защиты своего сайта используйте X-FRAME-OPTIONS.

Измените файл httpd.conf.

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

Добавьте строку `Header append X-FRAME-OPTIONS "SAMEORIGIN"` и сохраните файл, а затем перезапустите Apache.

#### <a name="mime-type-sniffing"></a>Сканирование типа MIME

Этот заголовок предотвращает MIME-сканирование ответов с указанным типом содержимого в Internet Explorer, запрещая браузеру переопределять тип содержимого ответа. Параметр nosniff означает, что если сервер определяет содержимое как text/html, браузер будет обрабатывать его как text/html.

Измените файл httpd.conf.

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

Добавьте строку `Header set X-Content-Type-Options "nosniff"` и сохраните файл, а затем перезапустите Apache.

### <a name="load-balancing"></a>Балансировка нагрузки 

В этом примере показано, как установить и настроить Apache в CentOS 7 и Kestrel на том же компьютере.  Однако, чтобы исключить единую точку отказа, воспользуйтесь *mod_proxy_balancer* и измените VirtualHost для управления несколькими экземплярами веб-приложений за прокси-сервером Apache.

```bash
    sudo yum install mod_proxy_balancer
```

В файле конфигурации дополнительный экземпляр приложения `hellomvc` установлен и настроен для запуска на порту 5001, а раздел *Прокси-сервер* настроен с конфигурацией балансировки с двумя членами для балансировки нагрузки *byrequests*.

```text
    <VirtualHost *:80>
        RewriteEngine On
        RewriteCond %{HTTPS} !=on
        RewriteRule ^/?(.*) https://%{SERVER_NAME}/ [R,L]
    </VirtualHost>

    <VirtualHost *:443>
            ProxyPass / balancer://mycluster/ 

            ProxyPassReverse / http://127.0.0.1:5000/
            ProxyPassReverse / http://127.0.0.1:5001/

            <Proxy balancer://mycluster>
                BalancerMember http://127.0.0.1:5000
                BalancerMember http://127.0.0.1:5001 
                ProxySet lbmethod=byrequests
            </Proxy>

            <Location />
                SetHandler balancer
            </Location>
            ErrorLog /var/log/httpd/hellomvc-error.log
            CustomLog /var/log/httpd/hellomvc-access.log common
            SSLEngine on
            SSLProtocol all -SSLv2
            SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
            SSLCertificateFile /etc/pki/tls/certs/localhost.crt
            SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
    </VirtualHost>
```

### <a name="rate-limits"></a>Ограничения скорости
С помощью `mod_ratelimit` в составе модуля `htttpd` можно ограничить объем пропускной способности клиентов. 

```bash
    sudo nano /etc/httpd/conf.d/ratelimit.conf
```
В примере файла пропускная способность составляет 600 Кбит/с в корневой папке.

```text
    <IfModule mod_ratelimit.c>
        <Location />
            SetOutputFilter RATE_LIMIT
            SetEnv rate-limit 600
        </Location>
    </IfModule>
```
