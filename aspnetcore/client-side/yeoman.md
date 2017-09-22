---
title: "Построение проектов с Yeoman в ASP.NET Core"
author: spboyer
description: "В этой статье описывается создание веб-приложения, с помощью Yeoman ASP.NET Core генератора на macOS."
keywords: "ASP.NET Core, Yeoman, кроссплатформенный, ё aspnet"
ms.author: spboyer
manager: wpickett
ms.date: 07/05/2017
ms.topic: article
ms.assetid: fda0c2a8-1743-4505-be1a-7f8ceeef8647
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/yeoman
ms.openlocfilehash: d7411c1635e9fef2857f9a03e7310224ee8d7344
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-building-projects-with-yeoman-in-aspnet-core"></a>Введение в построение проектов с Yeoman в ASP.NET Core

[Yeoman](http://yeoman.io/) — это система формирования шаблонов проекта для создания различных типов приложений. Yeoman генератор для ASP.NET Core содержит множество шаблонов проекта для запуска нового веб-узла, MVC или консольное приложение.

## <a name="install-nodejs-npm-and-yeoman"></a>Установка Node.js и npm, Yeoman

### <a name="prerequisites"></a>Предварительные требования

Node.js и npm являются обязательными для Yeoman. Загрузить [Node.js](https://nodejs.org/). Программа установки включает [Node.js](https://nodejs.org/) и [npm](https://www.npmjs.com/). Bower также является обязательным для установки платформы пользовательского интерфейса, такие как начальной загрузки.

Чтобы установить Yeoman и Bower, выполните следующую команду:

```console
npm install -g yo bower
```

>[!Note]
>Если появляется ошибка `npm ERR! Please try running this command again as root/Administrator.` на macOS, выполните следующие команды, используя [sudo](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/sudo.8.html):`sudo npm install -g yo bower`

Из командной строки установите генератор ASP.NET:

```console
npm install -g generator-aspnet
```

> [!NOTE]
> Если появляется ошибка разрешение, выполните команду под `sudo` как описано выше.

`–g` Флаг глобально, устанавливает генератор, чтобы его можно использовать из любого пути.

## <a name="create-an-aspnet-app"></a>Создание приложения ASP.NET

Выполните генератор Yeoman под управлением ASP.NET.

```console
yo aspnet
```

Генератор отображает меню. Стрелка вниз до **Web приложения, основные [без членства и авторизации]** проекта, а затем коснитесь **ввод**:

![Окно командной строки: тип приложений вы хотите создать? Меню типов приложений](yeoman/_static/yeoman-yo-aspnet.png)

Выберите программу начальной загрузки как платформа пользовательского интерфейса и коснитесь **ввод**.

Используйте "**MyWebApp**" для имени приложения, а затем коснитесь **ввод**.

Yeoman будет формировать проекта и его вспомогательные файлы. Предлагаемые дальнейшие действия также предоставляются в виде команды.

![Окно командной строки: имя приложения ASP.NET? Командная строка](yeoman/_static/yeoman-yo-aspnet-created.png)

[Генератор](https://www.npmjs.com/package/generator-aspnet) создает ASP.NET Core проекты, которые могут быть загружены в коде Visual Studio, Visual Studio или запустите из командной строки.

## <a name="restore-build-and-run"></a>Восстановление, сборка и выполнение

Выполните предлагаемые команд, изменив каталоги для `MyWebApp` каталога. Затем запустите `dotnet restore`.

![командное окно](yeoman/_static/dotnet-restore.png)

Построение и запуск приложения с использованием `dotnet build` и `dotnet run`:

![командное окно](yeoman/_static/dotnet-build-run.png)

На этом этапе можно перейти к показано тестирование приложения ASP.NET Core только что созданный URL-адрес.

## <a name="client-side-packages"></a>Пакеты на стороне клиента

Интерфейсный ресурсы предоставляются с помощью шаблонов из Yeoman с помощью генератора [Bower](xref:client-side/bower) диспетчер клиентского пакета, добавление *bower.json* и *.bowerrc* файлы для восстановления клиентских пакетов, с помощью Bower.

[BundlerMinifier](xref:client-side/bundling-and-minification) компонент также включается по умолчанию для облегчения объединения (объединение) и минификацию CSS, JavaScript и HTML.

## <a name="building-and-running-from-visual-studio"></a>Построение и запуск из Visual Studio

Можно загрузить созданный веб-проекта ASP.NET Core непосредственно в Visual Studio, построения и запуска проекта из него. Следуйте приведенным выше инструкциям для формировать с помощью Yeoman нового приложения ASP.NET Core. На этот раз выберите **веб-приложение** из меню и имя приложения `MyWebApp`.

Запустите Visual Studio. В меню "файл" выберите ‣ Открыть решение или проект.

В диалоговом окне Открытие проекта перейдите на вкладку *.csproj* файла, выберите его и нажмите кнопку **откройте** кнопки. В обозревателе решений щелкните проект должно быть похоже на снимке экрана ниже.

![Файлы и папки из нового проекта в обозревателе решений](yeoman/_static/yeoman-solution.png)

Yeoman scaffolds веб-приложение MVC, дополненный и стороне сервера и клиента поддержка построения. Серверные зависимости перечислены в разделе **зависимости/NuGet** узла и зависимости на стороне клиента в **зависимости и Bower** обозревателя решений. Зависимости, автоматически восстанавливаются при загрузке проекта.

![В дереве обозревателя решений в узле зависимости в папку Bower открыт список его зависимостей.](yeoman/_static/yeoman-loading-dependencies.png)

После восстановления всех зависимостей нажмите **F5** для запуска проекта. Домашняя страница по умолчанию отображается в браузере.

![Открыть в Microsoft Edge веб-приложения](yeoman/_static/yeoman-home-page.png)

## <a name="restoring-building-and-hosting-from-a-command-line"></a>Восстановление, построение и размещение из командной строки

Можно подготовить и размещения веб-приложения с помощью .NET Core CLI.

В командной строке измените текущий каталог на папку, содержащую проект (то есть к папке, содержащей *.csproj* файла):

```console
cd src\MyWebApp
```

Восстановление зависимости пакета NuGet для проекта.

```console
dotnet restore
```

Запуск приложения:

```console
dotnet run
```

Кросс платформенных [Kestrel](xref:fundamentals/servers/kestrel) начинается веб-сервер прослушивает порт 5000.

Откройте веб-браузер и перейдите к `http://localhost:5000`.

![Открыть в Microsoft Edge веб-приложения](yeoman/_static/yeoman-home-page_5000.png)

## <a name="adding-to-your-project-with-sub-generators"></a>Добавление в проект с генераторами sub

С помощью Yeoman [sub генераторы](https://github.com/omnisharp/generator-aspnet), можно добавить любую `nuget.config` или `web.config` после создания проекта. Например выполните следующую команду из каталога, в котором следует создать файл:

```console
yo aspnet:nugetconfig
```

Результатом является файл конфигурации NuGet с именем `nuget.config` со следующим содержимым:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
 <packageSources>
    <!--To inherit the global NuGet package sources remove the <clear/> line below -->
    <clear />
 </packageSources>
</configuration>
```

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Серверы (Kestrel и WebListener)](xref:fundamentals/servers/index)
* [Основы](xref:fundamentals/index)
