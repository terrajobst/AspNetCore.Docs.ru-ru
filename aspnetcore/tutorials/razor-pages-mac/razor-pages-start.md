---
title: Начало работы с Razor Pages в ASP.NET Core в macOS с использованием Visual Studio для Mac
author: rick-anderson
description: Узнайте, как начать работу с Razor Pages в ASP.NET Core с использованием Visual Studio для Mac.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 07/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: 29eb11d0195f483b144394e505dd63fb6016161b
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252338"
---
# <a name="get-started-with-razor-pages-in-aspnet-core-on-macos-with-visual-studio-for-mac"></a>Начало работы с Razor Pages в ASP.NET Core в macOS с использованием Visual Studio для Mac

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

В этом учебнике приводятся основные сведения о веб-приложении Razor Pages в ASP.NET Core. Перед работой с этим учебником рекомендуем изучить статью [Введение в Razor Pages](xref:mvc/razor-pages/index). Razor Pages — это рекомендуемый способ создания пользовательского интерфейса для веб-приложений в ASP.NET Core.

## <a name="prerequisites"></a>Предварительные требования

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-a-razor-web-app"></a>Создание веб-приложения Razor

Из терминала выполните следующие команды:

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

Указанные выше команды используют [интерфейс командной строки .NET Core](https://docs.microsoft.com/dotnet/core/tools/dotnet) для создания и запуска проекта Razor Pages. Откройте в браузере страницу http://localhost:5000 для просмотра приложения.

![Домашняя или индексная страница](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a>Открытие проекта

Нажмите клавиши CTRL+C, чтобы завершить работу приложения.

В Visual Studio откройте меню **Файл > Открыть файл** и выберите файл *RazorPagesMovie.csproj*.

### <a name="launch-the-app"></a>Запуск приложения

В Visual Studio выберите **Выполнить > Запуск без отладки**, чтобы запустить приложение. Visual Studio запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `http://localhost:5000`.

В следующем учебнике мы добавим в проект модель.

> [!div class="step-by-step"]
> [Далее: добавление модели](xref:tutorials/razor-pages-mac/model)
