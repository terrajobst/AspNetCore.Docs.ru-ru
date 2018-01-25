---
uid: mvc/overview/deployment/docker
title: "Перенос приложений ASP.NET MVC в контейнеры Windows"
description: "Узнайте, как запустить существующее приложение ASP.NET MVC в контейнере Windows Docker"
keywords: Windows Containers,Docker,ASP.NET MVC
author: BillWagner
ms.author: wiwagn
ms.date: 02/01/2017
ms.topic: article
ms.prod: .net-framework
ms.technology: dotnet-mvc
ms.devlang: dotnet
ms.assetid: c9f1d52c-b4bd-4b5d-b7f9-8f9ceaf778c4
ms.openlocfilehash: badc1c9b10ac27c3d876e3331c855a9d5904d27d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
# <a name="migrating-aspnet-mvc-applications-to-windows-containers"></a>Перенос приложений ASP.NET MVC в контейнеры Windows

При запуске существующего приложения на основе .NET Framework в контейнере Windows не требуется вносить изменения в приложение. Чтобы запустить приложение в контейнере Windows, создайте образ Docker с приложением и запустите контейнер. В этом разделе показано, как получить существующее [приложение ASP.NET MVC](http://www.asp.net/mvc) и развернуть его в контейнере Windows.

Начните с существующего приложения ASP.NET MVC и выполните сборку опубликованных ресурсов с помощью Visual Studio. Docker используется для создания образа, который содержит и запускает ваше приложение. Вы перейдете на сайт, запущенный в контейнере Windows, и проверите, работает ли приложение.

Для понимания этой статьи требуется базовое знакомство с Docker. Сведения о Docker см. в разделе [Общие сведения о Docker](https://docs.docker.com/engine/understanding-docker/).

Приложение, которое будет работать в контейнере — это простой веб-сайт, который случайным образом отвечает на вопросы. Это базовое приложение MVC без проверки подлинности и хранилища базы данных. Оно позволяет сосредоточиться на перемещении веб-уровня в контейнер. В дальнейших темах показано, как перемещать постоянное хранилище и управлять им в контейнерных приложениях.

Перемещение приложения состоит из следующих действий.

1. [Создание задачи публикации для сборки ресурсов образа](#publish-script).
1. [Создание образа Docker, в котором будет запускаться приложение](#build-the-image).
1. [Запуск контейнера Docker, в котором будет запускаться образ](#start-a-container).
1. [Проверка приложения с помощью браузера](#verify-in-the-browser).

[Готовое приложение](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator) находится на GitHub.

## <a name="prerequisites"></a>Предварительные требования

Компьютер разработки должен быть запущен

- [Юбилейное обновление Windows 10](https://www.microsoft.com/software-download/windows10/) (или выше) либо [Windows Server 2016](https://www.microsoft.com/cloud-platform/windows-server) (или выше).
- [Docker для Windows](https://docs.docker.com/docker-for-windows/) — версия Stable 1.13.0 или 1.12 Beta 26 (или более поздние)
- [Visual Studio 2017 г](https://www.visualstudio.com/visual-studio-homepage-vs.aspx).

> [!IMPORTANT]
> При использовании Windows Server 2016 выполните инструкции по [развертыванию узла контейнеров в Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/deployment/deployment).

После установки и запуска Docker щелкните правой кнопкой мыши значок на панели задач и выберите **Переключиться на контейнеры Windows**. Это нужно для запуска образов Docker на основе Windows. На выполнение этой команды требуется несколько секунд:

![Контейнер Windows][windows-container]

## <a name="publish-script"></a>Публикация скрипта

Соберите вместе все ресурсы, которые нужно загрузить в образ Docker. Для создания профиля публикации приложения в Visual Studio есть команда **Опубликовать**. В этом профиле все ресурсы будут собраны в одном дереве каталогов, которое далее в этом учебнике вам нужно будет скопировать в свой целевой образ.

**Этапы публикации**

1. Щелкните правой кнопкой мыши веб-проект в Visual Studio и выберите **Publish** (Публикация).
1. Нажмите кнопку **Пользовательский профиль**, а затем выберите **Файловая система** в качестве метода.
1. Выберите каталог. По правилам загруженный образец использует `bin\Release\PublishOutput`.

![Публикация: подключение][publish-connection]

Откройте раздел **Параметры публикации файла** на вкладке **Параметры**. Выберите **Precompile during publishing** (Предварительная компиляция во время публикации). Эта оптимизация означает, что представления не будут компилироваться в контейнере Docker, вместо этого будут копироваться предварительно скомпилированные представления.

![Публикация: параметры][publish-settings]

Нажмите кнопку **Publish** (Публикация), и Visual Studio скопирует все необходимые ресурсы в целевую папку.

## <a name="build-the-image"></a>Сборка образа

Определение образа Docker в Dockerfile. Dockerfile содержит инструкции для базового образа, дополнительные компоненты, приложение, которое нужно запустить, и другие образы конфигурации.  Dockerfile содержит входные параметры для команды `docker build`, которая создает образ.

Вы создадите образ на основе образа `microsft/aspnet`, расположенного на [Docker Hub](https://hub.docker.com/r/microsoft/aspnet/).
Базовый образ `microsoft/aspnet` — это образ Windows Server. Он содержит Windows Server Core, IIS и ASP.NET 4.6.2. При запуске этого образа в контейнере он автоматически использует IIS и установленные веб-сайты.

Dockerfile, который создает образ, выглядит следующим образом:

```console
# The `FROM` instruction specifies the base image. You are
# extending the `microsoft/aspnet` image.

FROM microsoft/aspnet

# The final instruction copies the site you published earlier into the container.
COPY ./bin/Release/PublishOutput/ /inetpub/wwwroot
```

В Dockerfile нет команды `ENTRYPOINT`. Она не нужна. При запуске Windows Server со службами IIS, процесс IIS очень точки входа, который настроен для запуска в базовом образе aspnet.

Выполните команду сборки Docker, чтобы создать образ, который запускает приложение ASP.NET. Чтобы сделать это, откройте окно PowerShell в каталоге проекта и введите следующую команду в каталоге решения:

```console
docker build -t mvcrandomanswers .
```

Эта команда создаст новый образ с помощью инструкций в Dockerfile, именования (-t тегов) изображение в виде mvcrandomanswers. Это может включать получение базового образа из [Docker Hub](http://hub.docker.com). После этого в образ будет добавлено приложение.

После выполнения этой команды можно выполнить команду `docker images` для просмотра сведений о новом образе:

```console
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
mvcrandomanswers              latest              86838648aab6        2 minutes ago       10.1 GB
```

Идентификатор IMAGE ID на вашем компьютере будет другим. Теперь запустим приложение.

## <a name="start-a-container"></a>Запуск контейнера

Чтобы запустить контейнер, нужно выполнить следующую команду `docker run`:

```console
docker run -d --name randomanswers mvcrandomanswers
```

Аргумент `-d` предписывает Docker запустить образ в отсоединенном режиме. Это значит, что образ Docker запускается в отрыве от текущей оболочки.

Во многих примерах docker может появиться -p для сопоставления портов контейнером и узлом. Изображение aspnet по умолчанию уже настроен контейнер для прослушивания порта 80 и предоставлять к нему. 

Аргумент `--name randomanswers` содержит имя запущенного контейнера. Это имя можно использовать вместо идентификатора контейнера в большинстве команд.

`mvcrandomanswers` — это имя запускаемого образа.

## <a name="verify-in-the-browser"></a>Проверка в браузере

> [!NOTE]
> В текущем выпуске контейнера Windows не удается просмотреть `http://localhost`.
> Это связано с известным поведением в WinNAT. В будущем проблема будет устранена. А пока этого не произошло, необходимо использовать IP-адрес контейнера.

После запуска контейнера найдите его IP-адрес. В этом случае вы сможете подключиться к запущенному контейнеру из браузера:

```console
docker inspect -f "{{ .NetworkSettings.Networks.nat.IPAddress }}" randomanswers
172.31.194.61
```

Соединиться с IPv4-адресом, запущенного контейнера `http://172.31.194.61` в приведенном примере. Введите этот URL-адрес в адресной строке браузера, и вы увидите работающий сайт.

> [!NOTE]
> Некоторые программы VPN или прокси-серверы могут препятствовать переходу на ваш узел.
> Их можно временно отключить, чтобы убедиться, что контейнер работает.

Каталог образцов на GitHub содержит [Сценарий PowerShell](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator/run.ps1), который выполняет эти команды за вас. Откройте окно PowerShell, перейдите в каталог решения и введите команду:

```console
./run.ps1
```

Она создает образ, отображает список образов на компьютере, запускает контейнер и отображает IP-адрес для этого контейнера.

Чтобы остановить контейнер, выполните команду `docker
stop`:

```console
docker stop randomanswers
```

Чтобы удалить контейнер, выполните команду `docker rm`:

```console
docker rm randomanswers
```

[windows-container]: media/aspnetmvc/SwitchContainer.png "Переключение на контейнер Windows"
[publish-connection]: media/aspnetmvc/PublishConnection.png "Публикация в файловой системе"
[publish-settings]: media/aspnetmvc/PublishSettings.png "Публикация: параметры"
