---
title: "Введение в ASP.NET Core MVC для Mac, Linux и Windows"
author: rick-anderson
description: "Начало работы с MVC ASP.NET Core и Visual Studio Code в операционных системах Mac, Linux и Windows"
manager: wpickett
ms.author: riande
ms.date: 07/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: 4771555b66f328a819f17a32eb3959f9ecf33d44
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="getting-started-with-aspnet-core-mvc--on-mac-linux-or-windows"></a>Начало работы с MVC ASP.NET Core в операционных системах Mac, Linux и Windows

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

В этом учебнике представлены основы построения веб-приложений MVC ASP.NET Core с помощью [Visual Studio Code](https://code.visualstudio.com) (VS Code). Для работы с этим руководством требуется знание VS Code. Дополнительные сведения см. в разделах [Начало работы с VS Code](https://code.visualstudio.com/docs) и [Справка по Visual Studio Code](#visual-studio-code-help). 

[!INCLUDE[consider RP](../../includes/razor.md)]

Существует 3 версии этого учебника:

* macOS: [Создание приложения ASP.NET Core MVC с помощью Visual Studio для Mac](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [Создание приложения ASP.NET Core MVC с помощью Visual Studio](xref:tutorials/first-mvc-app/start-mvc)
* macOS, Linux и Windows: [Создание приложения MVC ASP.NET Core с помощью Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc) 

## <a name="install-vs-code-and-net-core"></a>Установка VS Code и .NET Core

Для этого учебника требуется [пакет SDK для .NET Core 2.0.0](https://www.microsoft.com/net/core) или более поздней версии. См. [этот PDF-файл](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) для версии ASP.NET Core 1.1.

Установите следующие компоненты:

* [Пакет SDK .NET Core 2.0.0](https://www.microsoft.com/net/core) или более поздней версии.
* [Visual Studio Code.](https://code.visualstudio.com)
* Расширение VS Code [C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 

## <a name="create-a-web-app-with-dotnet"></a>Создание веб-приложения с dotnet

Из терминала выполните следующие команды:

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

Откройте папку *MvcMovie* в Visual Studio Code (VS Code) и выберите файл *Startup.cs*.

- Нажмите кнопку **Да** в **предупреждении** "Required assets to build and debug are missing from 'MvcMovie'. Add them?" (В MvcMovie отсутствуют необходимые ресурсы для сборки и отладки. Добавить их?)
- Щелкните **Восстановить** в **информационном** сообщении "There are unresolved dependencies" (Имеются неразрешенные зависимости).

![VS Code с предупреждением "Required assets to build and debug are missing from 'MvcMovie'.Add them?" (В MvcMovie отсутствуют необходимые ресурсы для сборки и отладки. Добавить их? "Больше не спрашивать", "Не сейчас", "Да", а также информационное сообщение "There are unresolved dependencies" (Имеются неразрешенные зависимости) — "Восстановить", "Закрыть"](../web-api-vsc/_static/vsc_restore.png)

Нажмите клавишу **отладки** (F5), чтобы выполнить сборку программы и запустить ее.

![Запуск приложения](../first-mvc-app/start-mvc/_static/1.png)

VS Code запускает веб-сервер [Kestrel](xref:fundamentals/servers/kestrel) и ваше приложение. Обратите внимание на то, что в адресной строке указывается `localhost:5000`, а не что-либо типа `example.com`. Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.

Шаблон по умолчанию включает рабочие ссылки **Домой, О программе** и **Контакты**. На представленном выше снимке экрана эти ссылки не отображаются. Если размер окна вашего браузера слишком мал, нажмите значок навигации, чтобы отобразить эти ссылки.

![Значок навигации в правом верхнем углу экрана](../first-mvc-app/start-mvc/_static/2.png)

В следующей части этого учебника мы поговорим об MVC и приступим к написанию кода.

## <a name="visual-studio-code-help"></a>Справка по Visual Studio Code

- [Начало работы](https://code.visualstudio.com/docs)
- [Отладка](https://code.visualstudio.com/docs/editor/debugging)
- [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [Сочетания клавиш](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [Сочетания клавиш для Mac](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [Сочетания клавиш для Linux](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [Сочетания клавиш для Windows](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

>[!div class="step-by-step"]
[Следующая статья — "Добавление контроллера"](adding-controller.md)
