---
title: "Начало работы с Razor Pages в ASP.NET Core с Visual Studio Code"
author: rick-anderson
description: "Начало работы с Razor Pages в ASP.NET Core с использованием Visual Studio Code"
keywords: "ASP.NET Core,Razor Pages,формирование шаблонов,Entity Framework Core,EF,EF Core,база данных,mac,macOS,Visual Studio Code,Code"
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: 1b9dff14fa98314601fa44aa229aef6b73bb79d0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-code"></a>Начало работы с Razor Pages в ASP.NET Core с Visual Studio Code

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT)

В этом учебнике приводятся основные сведения о веб-приложении Razor Pages в ASP.NET Core. Перед работой с этим учебником рекомендуем изучить статью [Введение в Razor Pages](xref:mvc/razor-pages/index). Razor Pages — это рекомендуемый способ создания пользовательского интерфейса для веб-приложений в ASP.NET Core.

## <a name="prerequisites"></a>Предварительные требования

Установите следующие компоненты:

* [пакет SDK для .NET Core 2.0.0](https://www.microsoft.com/net/core) или более поздней версии;
* [Visual Studio Code.](https://code.visualstudio.com)
* Расширение VS Code [C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 

## <a name="create-a-razor-web-app"></a>Создание веб-приложения Razor

Из терминала выполните следующие команды.

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

Указанные выше команды используют [интерфейс командной строки .NET Core](https://docs.microsoft.com/dotnet/core/tools/dotnet) для создания и запуска проекта Razor Pages. Откройте в браузере страницу http://localhost:5000 для просмотра приложения.

![Домашняя или индексная страница](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a>Открытие проекта

Нажмите клавиши CTRL+C, чтобы завершить работу приложения.

В Visual Studio Code (VS Code) откройте меню **Файл > Открыть папку** и выберите папку *RazorPagesMovie*.

- Нажмите кнопку **Да** в **предупреждении** "Required assets to build and debug are missing from 'TodoApi'.Add them?" (В TodoApi отсутствуют необходимые ресурсы для сборки и отладки. Добавить их?).
- Щелкните **Восстановить** в **информационном** сообщении "There are unresolved dependencies" (Имеются неразрешенные зависимости).

### <a name="launch-the-app"></a>Запуск приложения

Нажмите клавиши CTRL+F5, чтобы запустить приложение без отладки. Либо в меню **Отладка** выберите пункт **Запуск без отладки**.

В следующем учебнике мы добавим в проект модель. 

>[!div class="step-by-step"]
[Далее: добавление модели](xref:tutorials/razor-pages-vsc/model)  
