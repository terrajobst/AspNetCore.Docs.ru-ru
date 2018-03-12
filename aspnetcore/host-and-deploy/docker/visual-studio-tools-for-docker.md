---
title: "Использование средств Visual Studio для Docker с ASP.NET Core"
author: spboyer
description: "Сведения о том, как упаковать в контейнер приложение ASP.NET Core с помощью средств Visual Studio 2017 и Docker для Windows."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/12/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/docker/visual-studio-tools-for-docker
ms.openlocfilehash: b2a3c369a22d50fcefdb96914f5bf84bfafab7cb
ms.sourcegitcommit: 6fa546140575b3eb279eabae12d9acad966f70e0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="visual-studio-tools-for-docker-with-aspnet-core"></a>Использование средств Visual Studio для Docker с ASP.NET Core

[Visual Studio 2017 г](https://www.visualstudio.com/) поддерживает построение, отладка и запуск приложений, предназначенных для .NET Core контейнерного ASP.NET Core. Поддерживаются контейнеры Windows и Linux.

## <a name="prerequisites"></a>Предварительные требования

* [Visual Studio 2017](https://www.visualstudio.com/) с рабочей нагрузкой **Кроссплатформенная разработка .NET Core**
* [Docker для Windows](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a>Установка и настройка

Перед установкой Docker ознакомьтесь с разделом [Docker для Windows: что следует знать перед установкой](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install), а затем установите [Docker для Windows](https://docs.docker.com/docker-for-windows/install/).

Нужно настроить **[Общие диски](https://docs.docker.com/docker-for-windows/#shared-drives)** в Docker для Windows, чтобы обеспечить поддержку сопоставления тома и отладки. Щелкните правой кнопкой мыши значок области уведомлений Docker, выберите **параметры...** и выберите **общих дисков**. Выберите диск, где хранятся файлы в Docker. Выберите **применить**.

![Общие диски](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

> [!TIP]
> Версии Visual Studio 2017 г 15.6 и более поздние версии приглашение по **общих дисков** не настроены.

## <a name="add-docker-support-to-an-app"></a>Добавление поддержки Docker в приложение

Чтобы добавить поддержку Docker в проекте ASP.NET Core, проекта должны быть предназначены для .NET Core. Поддерживаются контейнерами Windows и Linux.

При добавлении поддержки Docker в проект, выберите Windows или Linux контейнера. Узел Docker должен работать на контейнерах такого же типа. Чтобы изменить тип контейнера для работающего экземпляра Docker, щелкните правой кнопкой мыши значок Docker в области уведомлений и выберите **Переключение на контейнеры Windows** или **Переключение на контейнеры Linux**.

### <a name="new-app"></a>Новое приложение

При создании приложения с помощью шаблонов проектов **веб-приложения ASP.NET Core** установите флажок **Enable Docker Support** (Включение поддержки Docker):

![Флажок "Enable Docker Support" (Включение поддержки Docker)](visual-studio-tools-for-docker/_static/enable-docker-support-checkbox.png)

Если требуемая версия .NET framework — .NET Core **ОС** раскрывающегося списка позволяет выбирать тип контейнера.

### <a name="existing-app"></a>Существующее приложение

Средства Visual Studio для Docker не поддерживают добавление Docker в существующий проект ASP.NET Core, ориентированный на .NET Framework. Для проектов ASP.NET Core, ориентированных на .NET Core, добавить поддержку Docker через средства можно двумя способами. Откройте проект в Visual Studio и выберите один из следующих параметров:

* Выберите пункт **Поддержка Docker** в меню **Проект**.
* В обозревателе решений щелкните проект правой кнопкой мыши и выберите пункт **Добавить** > **Поддержка Docker**.

## <a name="docker-assets-overview"></a>Обзор ресурсов Docker

Средства Visual Studio для Docker добавляют в решение проект *docker-compose* со следующим содержимым:

* *.dockerignore*. Содержит список шаблонов файлов и каталогов для исключения при создании контекста сборки.
* *docker-compose.yml*. Базовый файл [Docker Compose](https://docs.docker.com/compose/overview/), который служит для определения коллекции образов, сборка и запуск которых будут выполняться с помощью команд `docker-compose build` и `docker-compose run`, соответственно.
* *docker compose.override.yml*. Дополнительный файл, считываемый Docker Compose и содержащий переопределения конфигурации для служб. Visual Studio выполняет `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` для объединения этих файлов.

*Dockerfile* с инструкциями по созданию окончательного образа Docker добавляется в корень проекта. См. [справочник по Dockerfile](https://docs.docker.com/engine/reference/builder/) для получения сведений о других доступных в нем командах. Этот конкретный *Dockerfile* использует [многоэтапную сборку](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) из четырех раздельных именованных этапов:

[!code-dockerfile[](visual-studio-tools-for-docker/samples/HelloDockerTools/HelloDockerTools/Dockerfile?highlight=1,5,14,17)]

*Dockerfile* основан на образе [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore). Этот базовый образ включает пакеты NuGet платформы ASP.NET Core, которые были предварительно скомпилированы JIT-компилятором для повышения производительности запуска.

*Docker compose.yml* файл содержит имя образа, который создается при выполнении проекта:

[!code-yaml[](visual-studio-tools-for-docker/samples/HelloDockerTools/docker-compose.yml?highlight=5)]

В предыдущем примере `image: hellodockertools` создает образ `hellodockertools:dev`, когда приложение выполняется в режиме **отладки**. Образ `hellodockertools:latest` создается, когда приложение запускается в режиме **выпуска**.

Префикс имени изображения с [Docker Hub](https://hub.docker.com/) имя пользователя (например, `dockerhubusername/hellodockertools`), если изображение будет помещено в реестре. Кроме того, изменить имя изображения, чтобы включить URL-адрес частного реестра (например, `privateregistry.domain.com/hellodockertools`) в зависимости от конфигурации.

## <a name="debug"></a>Отладка

Выберите пункт **Docker** в раскрывающемся списке отладки на панели инструментов, чтобы начать отладку приложения. Представление **Docker** окна **Выходные данные** показывает следующие выполненные действия:

* *Microsoft/aspnetcore* получена образа среды выполнения (если еще не в кэше).
* *Aspnetcore/microsoft построения* компиляции и публикации изображение получается (если еще не в кэше).
* Переменной среды *ASPNETCORE_ENVIRONMENT* присвоено значение `Development` внутри контейнера.
* Порт 80 открыт и сопоставлен с динамически назначаемым портом для localhost. Порт определяется узлом Docker и может запрашиваться с помощью команды `docker ps`.
* Приложения копируется в контейнер.
* Браузер по умолчанию запускается с помощью отладчика, присоединенных к контейнеру с помощью динамически назначаемый порт. 

Полученный образ Docker — *разработки* образа приложения с *microsoft/aspnetcore* изображения как базового образа. Выполните команду `docker images` в окне **Консоль диспетчера пакетов** (PMC). Отображаются изображения на компьютере.

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                f8f9d6c923e2        About an hour ago   391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        15 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> Изображение разработки не имеет содержимое приложения, как **отладки** конфигурации используют монтирование томов для обеспечения взаимодействия с интерактивным. Чтобы отправить образ, используйте конфигурацию **выпуска**.

Выполните команду `docker ps` в PMC. Обратите внимание, что приложение выполняется с помощью контейнера:

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   21 seconds ago      Up 19 seconds       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="edit-and-continue"></a>Изменение и продолжение

Изменения статических полей и представлений Razor применяются автоматически без необходимости выполнять этап компиляции. Внесите изменение, сохраните его и перезагрузите страницу в браузере, чтобы увидеть обновление.  

Для внесения изменений в файлы кода требуются компиляция и перезапуск Kestrel в контейнере. После внесения изменения нажмите клавиши CTRL+F5, чтобы выполнить процесс и запустить приложение в контейнере. Контейнер Docker не перестроен или остановлена. Выполните команду `docker ps` в PMC. Обратите внимание, что исходный контейнер все еще выполняется, как и 10 минут назад:

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   10 minutes ago      Up 10 minutes       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="publish-docker-images"></a>Публикация образов Docker

После завершения цикла разработки и отладки приложения, Visual Studio Tools для Docker помочь при создании образа производственного приложения. Выберите в раскрывающемся списке конфигурации значение **Выпуск** и выполните сборку приложения. Инструментарий создает изображение с *последние* тег, который можно отправить частного реестра или Docker Hub. 

Выполните команду `docker images` PMC, чтобы просмотреть список образов:

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                4cb1fca533f0        19 seconds ago      391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        16 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> Команда `docker images` возвращает промежуточные образы с именами репозитория и тегами, обозначенными как *\<none>* (не указаны выше). Эти неименованные образы создаются *Dockerfile* [многоэтапной сборки](https://docs.docker.com/engine/userguide/eng-image/multistage-build/). Они повышают эффективность сборки окончательного образа &mdash; при изменениях перестраиваются только необходимые слои. Когда промежуточных образов больше не нужны, удалите их с помощью [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) команды.

Рабочий образ или образ выпуска может быть меньше по размеру, чем образ *dev*. В результате сопоставления по тома отладчика и приложения были запущены на локальном компьютере, а не внутри контейнера. Образ с тегом *latest* упаковал код приложения, необходимый для запуска приложения на хост-компьютере. Таким образом разностный файл имеет размер кода приложения.
