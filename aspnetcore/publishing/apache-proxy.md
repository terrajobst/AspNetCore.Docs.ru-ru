---
title: "Размещение ASP.NET Core в операционной системе Linux с Apache"
description: "В статье описывается процедура настройки Apache как обратного прокси-сервера в CentOS для перенаправления трафика HTTP в веб-приложение ASP.NET Core, выполняемое в Kestrel."
keywords: "ASP.NET Core, Apache, CentOS, обратный прокси-сервер, Linux, mod_proxy, httpd, размещение"
author: spboyer
ms.author: spboyer
manager: wpickett
ms.date: 10/19/2016
ms.topic: article
ms.assetid: fa9b0cb7-afb3-4361-9e7e-33afffeaca0c
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/apache-proxy
ms.openlocfilehash: 831e2fa148e52f6447e9065f5949785627d5e248
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-linux-with-apache-and-deploy-to-it"></a><span data-ttu-id="e490f-104">Настройка среды размещения для ASP.NET Core в операционной системе Linux с Apache и развертывание в эту среду</span><span class="sxs-lookup"><span data-stu-id="e490f-104">Set up a hosting environment for ASP.NET Core on Linux with Apache, and deploy to it</span></span>

<span data-ttu-id="e490f-105">Автор: [Шейн Бойер (Shayne Boyer)](https://www.github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="e490f-105">By [Shayne Boyer](https://www.github.com/spboyer)</span></span>

<span data-ttu-id="e490f-106">Apache является популярным HTTP-сервером и может быть настроен как прокси-сервер для перенаправления трафика HTTP аналогично Nginx.</span><span class="sxs-lookup"><span data-stu-id="e490f-106">Apache is a very popular HTTP server and can be configured as a proxy to redirect HTTP traffic similar to nginx.</span></span> <span data-ttu-id="e490f-107">В этом руководстве вы узнаете, как установить Apache в CentOS 7 и использовать его в качестве обратного прокси-сервера для входящих подключений и их перенаправления в приложение ASP.NET Core в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="e490f-107">In this guide, we will learn how to set up Apache on CentOS 7 and use it as a reverse proxy to welcome incoming connections and redirect them to the ASP.NET Core application running on Kestrel.</span></span> <span data-ttu-id="e490f-108">Для этого будет использоваться расширение *mod_proxy* и другие связанные модули Apache.</span><span class="sxs-lookup"><span data-stu-id="e490f-108">For this purpose, we will use the *mod_proxy* extension and other related Apache modules.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e490f-109">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="e490f-109">Prerequisites</span></span>

1. <span data-ttu-id="e490f-110">Сервер под управлением CentOS 7 с учетной записью обычного пользователя с правами sudo.</span><span class="sxs-lookup"><span data-stu-id="e490f-110">A server running CentOS 7, with a standard user account with sudo privilege.</span></span>
2. <span data-ttu-id="e490f-111">Существующее приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e490f-111">An existing ASP.NET Core application.</span></span> 

## <a name="publish-your-application"></a><span data-ttu-id="e490f-112">Публикация приложения</span><span class="sxs-lookup"><span data-stu-id="e490f-112">Publish your application</span></span>

<span data-ttu-id="e490f-113">Запустите `dotnet publish -c Release` из среды разработки, чтобы упаковать приложение в автономный каталог, который может выполняться на сервере.</span><span class="sxs-lookup"><span data-stu-id="e490f-113">Run `dotnet publish -c Release` from your development environment to package your application into a self-contained directory that can run on your server.</span></span> <span data-ttu-id="e490f-114">Затем опубликованное приложение необходимо скопировать на сервер с помощью SCP, FTP или другого метода передачи файлов.</span><span class="sxs-lookup"><span data-stu-id="e490f-114">The published application must then be copied to the server using SCP, FTP or other file transfer method.</span></span> 

> [!NOTE]
> <span data-ttu-id="e490f-115">В сценариях развертывания в рабочей среде рабочий процесс непрерывной интеграции осуществляет публикацию приложения и копирование ресурсов на сервер.</span><span class="sxs-lookup"><span data-stu-id="e490f-115">Under a production deployment scenario, a continuous integration workflow does the work of publishing the application and copying the assets to the server.</span></span> 

## <a name="configure-a-proxy-server"></a><span data-ttu-id="e490f-116">Настройка прокси-сервера</span><span class="sxs-lookup"><span data-stu-id="e490f-116">Configure a proxy server</span></span>

<span data-ttu-id="e490f-117">Обратный прокси-сервер — это общая настройка для обслуживания динамических веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="e490f-117">A reverse proxy is a common setup for serving dynamic web applications.</span></span> <span data-ttu-id="e490f-118">Обратный прокси-сервер завершает HTTP-запрос и перенаправляет его в приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e490f-118">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET application.</span></span>

<span data-ttu-id="e490f-119">Прокси-сервер перенаправляет запросы клиента на другой сервер, а не выполняет их сам.</span><span class="sxs-lookup"><span data-stu-id="e490f-119">A proxy server is one which forwards client requests to another server instead of fulfilling them itself.</span></span> <span data-ttu-id="e490f-120">Обратный прокси-сервер перенаправляет запросы в фиксированное назначение обычно от имени клиентов.</span><span class="sxs-lookup"><span data-stu-id="e490f-120">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="e490f-121">В этом руководстве Apache настраивается в качестве обратного прокси-сервера, работающего на том же сервере, где Kestrel обслуживает приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e490f-121">In this guide, Apache is being configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core application.</span></span> 

<span data-ttu-id="e490f-122">В зависимости от архитектурных потребностей или ограничений каждая часть приложения может существовать на разных физических компьютерах, в контейнерах Docker или сочетании конфигураций.</span><span class="sxs-lookup"><span data-stu-id="e490f-122">Each piece of the application can exist on separate physical machines, Docker containers, or a combination of configurations depending on your architectural needs or restrictions.</span></span>

### <a name="install-apache"></a><span data-ttu-id="e490f-123">Установка Apache</span><span class="sxs-lookup"><span data-stu-id="e490f-123">Install Apache</span></span>

<span data-ttu-id="e490f-124">Установка веб-сервера Apache в CentOS осуществляется с помощью выполнения одной команды, однако сначала следует обновить пакеты.</span><span class="sxs-lookup"><span data-stu-id="e490f-124">Installing the Apache web server on CentOS is a single command, but first let's update our packages.</span></span>

```bash
    sudo yum update -y
```

<span data-ttu-id="e490f-125">Все установленные пакеты необходимо обновить до их последней версии.</span><span class="sxs-lookup"><span data-stu-id="e490f-125">This ensures that all of the installed packages are updated to their latest version.</span></span> <span data-ttu-id="e490f-126">Установка Apache с помощью `yum`</span><span class="sxs-lookup"><span data-stu-id="e490f-126">Install Apache using `yum`</span></span>

```bash
    sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="e490f-127">Выходные данные должны выглядеть примерно следующим образом.</span><span class="sxs-lookup"><span data-stu-id="e490f-127">The output should reflect something similar to the following.</span></span>

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
> <span data-ttu-id="e490f-128">В этом примере в выходных данных отображается httpd.86_64, так как используется 64-разрядная версия CentOS 7.</span><span class="sxs-lookup"><span data-stu-id="e490f-128">In this example the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="e490f-129">Для вашего сервера могут выводиться другие выходные данные.</span><span class="sxs-lookup"><span data-stu-id="e490f-129">The output may be different for your server.</span></span> <span data-ttu-id="e490f-130">Чтобы проверить, где установлен Apache, выполните `whereis httpd` из командной строки.</span><span class="sxs-lookup"><span data-stu-id="e490f-130">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span> 

### <a name="configure-apache-for-reverse-proxy"></a><span data-ttu-id="e490f-131">Настройка Apache в качестве обратного прокси-сервера</span><span class="sxs-lookup"><span data-stu-id="e490f-131">Configure Apache for reverse proxy</span></span>

<span data-ttu-id="e490f-132">Файлы конфигурации для Apache находятся в каталоге `/etc/httpd/conf.d/`.</span><span class="sxs-lookup"><span data-stu-id="e490f-132">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="e490f-133">Все файлы с расширением **.conf** будут обрабатываться в алфавитном порядке, кроме файлов конфигурации модуля в `/etc/httpd/conf.modules.d/`, где содержатся файлы конфигурации, необходимые для загрузки модулей.</span><span class="sxs-lookup"><span data-stu-id="e490f-133">Any file with the **.conf** extension will be processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="e490f-134">Создайте файл конфигурации приложения. В этом примере он будет назван `hellomvc.conf`.</span><span class="sxs-lookup"><span data-stu-id="e490f-134">Create a configuration file for your app, for this example we'll call it `hellomvc.conf`</span></span>

```text
    <VirtualHost *:80>
        ProxyPreserveHost On
        ProxyPass / http://127.0.0.1:5000/
        ProxyPassReverse / http://127.0.0.1:5000/
        ErrorLog /var/log/httpd/hellomvc-error.log
        CustomLog /var/log/httpd/hellomvc-access.log common
    </VirtualHost>
```

<span data-ttu-id="e490f-135">Узел *VirtualHost*, экземпляров которого может быть несколько в файле или на сервере во многих файлах, настроен для прослушивания всех IP-адресов на порту 80.</span><span class="sxs-lookup"><span data-stu-id="e490f-135">The *VirtualHost* node, of which there can be multiple in a file or on a server in many files, is set to listen on any IP address using port 80.</span></span> <span data-ttu-id="e490f-136">Следующие две строки заданы для передачи всех запросов, полученных в корне, на порт компьютера 127.0.0.1 5000 и в обратном направлении.</span><span class="sxs-lookup"><span data-stu-id="e490f-136">The next two lines are set to pass all requests received at the root to the machine 127.0.0.1 port 5000 and in reverse.</span></span> <span data-ttu-id="e490f-137">Чтобы этот обмен данными был двунаправленным, требуются параметры *ProxyPass* и *ProxyPassReverse*.</span><span class="sxs-lookup"><span data-stu-id="e490f-137">For there to be bi-directional communication, both settings *ProxyPass* and *ProxyPassReverse* are required.</span></span>

<span data-ttu-id="e490f-138">С помощью директив *ErrorLog* и *CustomLog* для каждого VirtualHost можно настроить ведение журнала.</span><span class="sxs-lookup"><span data-stu-id="e490f-138">Logging can be configured per VirtualHost using *ErrorLog* and *CustomLog* directives.</span></span> <span data-ttu-id="e490f-139">*ErrorLog* — это расположение, где сервер будет регистрировать ошибки, а *CustomLog* задает имя файла и формат файла журнала.</span><span class="sxs-lookup"><span data-stu-id="e490f-139">*ErrorLog* is the location where the server will log errors and *CustomLog* sets the filename and format of log file.</span></span> <span data-ttu-id="e490f-140">В нашем случае это расположение, где будут фиксироваться сведения о запросах.</span><span class="sxs-lookup"><span data-stu-id="e490f-140">In our case this is where request information will be logged.</span></span> <span data-ttu-id="e490f-141">Для каждого запроса выделяется одна строка.</span><span class="sxs-lookup"><span data-stu-id="e490f-141">There will be one line for each request.</span></span>

<span data-ttu-id="e490f-142">Сохраните файл и протестируйте конфигурацию.</span><span class="sxs-lookup"><span data-stu-id="e490f-142">Save the file, and test the configuration.</span></span> <span data-ttu-id="e490f-143">Если проверка выполнена успешно, ответ должен быть `Syntax [OK]`.</span><span class="sxs-lookup"><span data-stu-id="e490f-143">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
    sudo service httpd configtest
```

<span data-ttu-id="e490f-144">Перезапустите Apache.</span><span class="sxs-lookup"><span data-stu-id="e490f-144">Restart Apache.</span></span>

```text
    sudo systemctl restart httpd
    sudo systemctl enable httpd
```

## <a name="monitoring-our-application"></a><span data-ttu-id="e490f-145">Мониторинг приложения</span><span class="sxs-lookup"><span data-stu-id="e490f-145">Monitoring our application</span></span>

<span data-ttu-id="e490f-146">Теперь Apache настроен на перенаправление запросов, отправленных в `http://localhost:80`, в приложение ASP.NET Core, выполняемое в Kestrel по адресу `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="e490f-146">Apache is now setup to forward requests made to `http://localhost:80` on to the ASP.NET Core application running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="e490f-147">Однако Apache не настроен для управления процессом Kestrel.</span><span class="sxs-lookup"><span data-stu-id="e490f-147">However, Apache is not set up to manage the Kestrel process.</span></span> <span data-ttu-id="e490f-148">Для запуска и мониторинга соответствующего веб-приложения можно воспользоваться *systemd* и создать файл службы.</span><span class="sxs-lookup"><span data-stu-id="e490f-148">We will use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="e490f-149">*systemd* — это система инициализации, предоставляющая различные функции для запуска и остановки процессов, а также управления ими.</span><span class="sxs-lookup"><span data-stu-id="e490f-149">*systemd* is an init system that provides many powerful features for starting, stopping and managing processes.</span></span> 


### <a name="create-the-service-file"></a><span data-ttu-id="e490f-150">Создание файла службы</span><span class="sxs-lookup"><span data-stu-id="e490f-150">Create the service file</span></span>

<span data-ttu-id="e490f-151">Создайте файл определения службы:</span><span class="sxs-lookup"><span data-stu-id="e490f-151">Create the service definition file</span></span> 

```bash
    sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="e490f-152">Пример файла службы для нашего приложения.</span><span class="sxs-lookup"><span data-stu-id="e490f-152">An example service file for our application.</span></span>

```text
[Unit]
    Description=Example .NET Web API Application running on CentOS 7

    [Service]
    WorkingDirectory=/var/aspnetcore/hellomvc
    ExecStart=/usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
    Restart=always
    RestartSec=10                                          # Restart service after 10 seconds if dotnet service crashes
    SyslogIdentifier=dotnet-example
    User=apache
    Environment=ASPNETCORE_ENVIRONMENT=Production 

    [Install]
    WantedBy=multi-user.target
```

> [!NOTE]
> <span data-ttu-id="e490f-153">**Пользователь**. Если пользователь *apache* в вашей конфигурации не используется, необходимо сначала создать определенного выше пользователя, а затем предоставить ему необходимые права владения для файлов.</span><span class="sxs-lookup"><span data-stu-id="e490f-153">**User** -- If the user *apache* is not used by your configuration, the user defined here must be created first and given proper ownership for files</span></span>

<span data-ttu-id="e490f-154">Сохраните файл и включите службу.</span><span class="sxs-lookup"><span data-stu-id="e490f-154">Save the file and enable the service.</span></span>

```bash
    systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="e490f-155">Запустите службу и убедитесь, что она работает.</span><span class="sxs-lookup"><span data-stu-id="e490f-155">Start the service and verify that it is running.</span></span>

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

<span data-ttu-id="e490f-156">Теперь, когда обратный прокси-сервер настроен, а управление Kestrel осуществляется системой systemd, веб-приложение полностью настроено и готово для доступа из браузера на локальном компьютере по адресу `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="e490f-156">With the reverse proxy configured and Kestrel managed through systemd, the web application is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="e490f-157">Если в ответах есть заголовок **Server**, это означает, что приложение ASP.NET Core обслуживает Kestrel.</span><span class="sxs-lookup"><span data-stu-id="e490f-157">Inspecting the response headers, the **Server** still shows the ASP.NET Core application being served by Kestrel.</span></span>

```text
    HTTP/1.1 200 OK
    Date: Tue, 11 Oct 2016 16:22:23 GMT
    Server: Kestrel
    Keep-Alive: timeout=5, max=98
    Connection: Keep-Alive
    Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="e490f-158">Просмотр журналов</span><span class="sxs-lookup"><span data-stu-id="e490f-158">Viewing logs</span></span>

<span data-ttu-id="e490f-159">Поскольку веб-приложение, использующее Kestrel, управляется с помощью systemd, все события и процессы регистрируются в централизованном журнале.</span><span class="sxs-lookup"><span data-stu-id="e490f-159">Since the web application using Kestrel is managed using systemd, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="e490f-160">При этом журнал содержит все записи обо всех службах и процессах, управляемых systemd.</span><span class="sxs-lookup"><span data-stu-id="e490f-160">However, this journal includes all entries for all services and processes managed by systemd.</span></span> <span data-ttu-id="e490f-161">Чтобы просмотреть элементы, связанные с `kestrel-hellomvc.service`, используйте следующую команду.</span><span class="sxs-lookup"><span data-stu-id="e490f-161">To view the `kestrel-hellomvc.service` specific items, use the following command.</span></span>

```bash
    sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="e490f-162">Кроме того, количество возвращаемых записей можно уменьшить, указав параметры времени, например `--since today`, `--until 1 hour ago` или их комбинацию.</span><span class="sxs-lookup"><span data-stu-id="e490f-162">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
    sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-our-application"></a><span data-ttu-id="e490f-163">Защита приложения</span><span class="sxs-lookup"><span data-stu-id="e490f-163">Securing our application</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="e490f-164">Настройка брандмауэра</span><span class="sxs-lookup"><span data-stu-id="e490f-164">Configure firewall</span></span>

<span data-ttu-id="e490f-165">*Firewalld* — это динамическая программа для управления брандмауэром с поддержкой зон сети, несмотря на то, что для управления портами и фильтрацией пакетов можно по-прежнему использовать служебную программу iptables.</span><span class="sxs-lookup"><span data-stu-id="e490f-165">*Firewalld* is a dynamic daemon to manage firewall with support for network zones, although you can still use iptables to manage ports and packet filtering.</span></span> <span data-ttu-id="e490f-166">Программа Firewalld должна устанавливаться по умолчанию, `yum` можно использовать для установки пакета или проверки.</span><span class="sxs-lookup"><span data-stu-id="e490f-166">Firewalld should be installed by default, `yum` can be used to install the package or verify.</span></span>

```bash
    sudo yum install firewalld -y
```

<span data-ttu-id="e490f-167">С помощью `firewalld` можно открывать только порты, необходимые для работы приложения.</span><span class="sxs-lookup"><span data-stu-id="e490f-167">Using `firewalld` you can open only the ports needed for the application.</span></span> <span data-ttu-id="e490f-168">В этом случае используются порты 80 и 443.</span><span class="sxs-lookup"><span data-stu-id="e490f-168">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="e490f-169">Следующие команды позволяют держать их постоянно открытыми.</span><span class="sxs-lookup"><span data-stu-id="e490f-169">The following commands permanently sets these to open.</span></span>

```bash
    sudo firewall-cmd --add-port=80/tcp --permanent
    sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="e490f-170">Перезагрузите параметры брандмауэра и проверьте, что доступные службы и порты находятся в зоне по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e490f-170">Reload the firewall settings, and check the available services and ports in the default zone.</span></span> <span data-ttu-id="e490f-171">Параметры доступны при просмотре `firewall-cmd -h`.</span><span class="sxs-lookup"><span data-stu-id="e490f-171">Options are available by inspecting `firewall-cmd -h`</span></span>

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

### <a name="ssl-configuration"></a><span data-ttu-id="e490f-172">Конфигурация SSL</span><span class="sxs-lookup"><span data-stu-id="e490f-172">SSL configuration</span></span>

<span data-ttu-id="e490f-173">Для настройки Apache для SSL служит модуль mod_ssl.</span><span class="sxs-lookup"><span data-stu-id="e490f-173">To configure Apache for SSL, the mod_ssl module is used.</span></span>  <span data-ttu-id="e490f-174">Он был первоначально установлен при установке модуля `httpd`.</span><span class="sxs-lookup"><span data-stu-id="e490f-174">This was installed initially when we installed the `httpd` module.</span></span> <span data-ttu-id="e490f-175">Если его не было или он не был установлен, используйте yum, чтобы добавить его в конфигурацию.</span><span class="sxs-lookup"><span data-stu-id="e490f-175">If it was missed or not installed, use yum to add it to your configuration.</span></span>

```bash
    sudo yum install mod_ssl
```
<span data-ttu-id="e490f-176">Чтобы применить SSL, установите `mod_rewrite`.</span><span class="sxs-lookup"><span data-stu-id="e490f-176">To enforce SSL, install `mod_rewrite`</span></span>

```bash
    sudo yum install mod_rewrite
```

<span data-ttu-id="e490f-177">Файл `hellomvc.conf`, который был создан в этом примере, следует изменить для включения перезаписи, а также добавления нового раздела **VirtualHost** для HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e490f-177">The `hellomvc.conf` file that was created for this example needs to be modified to enable the rewrite as well as adding the new **VirtualHost** section for HTTPS.</span></span>

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
> <span data-ttu-id="e490f-178">В этом примере используется локально созданный сертификат.</span><span class="sxs-lookup"><span data-stu-id="e490f-178">This example is using a locally generated certificate.</span></span> <span data-ttu-id="e490f-179">**SSLCertificateFile** должен быть основным файлом сертификата для имени домена.</span><span class="sxs-lookup"><span data-stu-id="e490f-179">**SSLCertificateFile** should be your primary certificate file for your domain name.</span></span> <span data-ttu-id="e490f-180">**SSLCertificateKeyFile** должен быть файлом ключа, формируемым при создании CSR.</span><span class="sxs-lookup"><span data-stu-id="e490f-180">**SSLCertificateKeyFile** should be the key file generated when you created the CSR.</span></span> <span data-ttu-id="e490f-181">**SSLCertificateChainFile** должен быть файлом промежуточного сертификата (если таковой имеется), предоставленным центром сертификации.</span><span class="sxs-lookup"><span data-stu-id="e490f-181">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by your certificate authority</span></span>

<span data-ttu-id="e490f-182">Сохраните файл и протестируйте конфигурацию.</span><span class="sxs-lookup"><span data-stu-id="e490f-182">Save the file, and test the configuration.</span></span>

```bash
    sudo service httpd configtest
```

<span data-ttu-id="e490f-183">Перезапустите Apache.</span><span class="sxs-lookup"><span data-stu-id="e490f-183">Restart Apache.</span></span>

```bash
    sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="e490f-184">Дополнительные предложения Apache</span><span class="sxs-lookup"><span data-stu-id="e490f-184">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="e490f-185">Дополнительные заголовки</span><span class="sxs-lookup"><span data-stu-id="e490f-185">Additional Headers</span></span> 
<span data-ttu-id="e490f-186">Для защиты от вредоносных атак существует несколько заголовков, которые следует изменить или добавить.</span><span class="sxs-lookup"><span data-stu-id="e490f-186">In order to secure against malicious attacks there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="e490f-187">Убедитесь, что модуль `mod_headers` установлен.</span><span class="sxs-lookup"><span data-stu-id="e490f-187">Ensure that the `mod_headers` module is installed.</span></span>

```bash
    sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking"></a><span data-ttu-id="e490f-188">Защита Apache от кликджекинга</span><span class="sxs-lookup"><span data-stu-id="e490f-188">Secure Apache from clickjacking</span></span>
<span data-ttu-id="e490f-189">Кликджекинг — это способ обмана, предназначенный для перехвата щелчков мыши пострадавшего пользователя.</span><span class="sxs-lookup"><span data-stu-id="e490f-189">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="e490f-190">Кликджекинг обманом заставляет пользователей (посетителей) щелкать зараженные сайты.</span><span class="sxs-lookup"><span data-stu-id="e490f-190">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="e490f-191">Для защиты своего сайта используйте X-FRAME-OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="e490f-191">Use X-FRAME-OPTIONS to secure your site.</span></span>

<span data-ttu-id="e490f-192">Измените файл httpd.conf.</span><span class="sxs-lookup"><span data-stu-id="e490f-192">Edit the httpd.conf file.</span></span>

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="e490f-193">Добавьте строку `Header append X-FRAME-OPTIONS "SAMEORIGIN"` и сохраните файл, а затем перезапустите Apache.</span><span class="sxs-lookup"><span data-stu-id="e490f-193">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"` and save the file, then restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="e490f-194">Сканирование типа MIME</span><span class="sxs-lookup"><span data-stu-id="e490f-194">MIME-type sniffing</span></span>

<span data-ttu-id="e490f-195">Этот заголовок предотвращает MIME-сканирование ответов с указанным типом содержимого в Internet Explorer, запрещая браузеру переопределять тип содержимого ответа.</span><span class="sxs-lookup"><span data-stu-id="e490f-195">This header prevents Internet Explorer from MIME-sniffing a response away from the declared content-type as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="e490f-196">Параметр nosniff означает, что если сервер определяет содержимое как text/html, браузер будет обрабатывать его как text/html.</span><span class="sxs-lookup"><span data-stu-id="e490f-196">With the nosniff option, if the server says the content is text/html, the browser will render it as text/html.</span></span>

<span data-ttu-id="e490f-197">Измените файл httpd.conf.</span><span class="sxs-lookup"><span data-stu-id="e490f-197">Edit the httpd.conf file.</span></span>

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="e490f-198">Добавьте строку `Header set X-Content-Type-Options "nosniff"` и сохраните файл, а затем перезапустите Apache.</span><span class="sxs-lookup"><span data-stu-id="e490f-198">Add the line `Header set X-Content-Type-Options "nosniff"` and save the file, then restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="e490f-199">Балансировка нагрузки</span><span class="sxs-lookup"><span data-stu-id="e490f-199">Load Balancing</span></span> 

<span data-ttu-id="e490f-200">В этом примере показано, как установить и настроить Apache в CentOS 7 и Kestrel на том же компьютере.</span><span class="sxs-lookup"><span data-stu-id="e490f-200">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span>  <span data-ttu-id="e490f-201">Однако, чтобы исключить единую точку отказа, воспользуйтесь *mod_proxy_balancer* и измените VirtualHost для управления несколькими экземплярами веб-приложений за прокси-сервером Apache.</span><span class="sxs-lookup"><span data-stu-id="e490f-201">However, in order to not have a single point of failure; using *mod_proxy_balancer* and modifying the VirtualHost would allow for managing mutliple instances of the web applications behind the Apache proxy server.</span></span>

```bash
    sudo yum install mod_proxy_balancer
```

<span data-ttu-id="e490f-202">В файле конфигурации дополнительный экземпляр приложения `hellomvc` установлен и настроен для запуска на порту 5001, а раздел *Прокси-сервер* настроен с конфигурацией балансировки с двумя членами для балансировки нагрузки *byrequests*.</span><span class="sxs-lookup"><span data-stu-id="e490f-202">In the configuration file, an additional instance of the `hellomvc` app has been setup to run on port 5001 and the *Proxy* section has been set with a balancer configuration with two members to load balance *byrequests*.</span></span>

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

### <a name="rate-limits"></a><span data-ttu-id="e490f-203">Ограничения скорости</span><span class="sxs-lookup"><span data-stu-id="e490f-203">Rate Limits</span></span>
<span data-ttu-id="e490f-204">С помощью `mod_ratelimit` в составе модуля `htttpd` можно ограничить объем пропускной способности клиентов.</span><span class="sxs-lookup"><span data-stu-id="e490f-204">Using `mod_ratelimit`, which is included in the `htttpd` module you can limit the amount of bandwidth of clients.</span></span> 

```bash
    sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="e490f-205">В примере файла пропускная способность составляет 600 Кбит/с в корневой папке.</span><span class="sxs-lookup"><span data-stu-id="e490f-205">The example file limits bandwidth as 600 KB/sec under the root location.</span></span>

```text
    <IfModule mod_ratelimit.c>
        <Location />
            SetOutputFilter RATE_LIMIT
            SetEnv rate-limit 600
        </Location>
    </IfModule>
```
