---
title: Введение в ASP.NET Core MVC для macOS, Linux и Windows
author: rick-anderson
description: Узнайте, как начать работу с MVC ASP.NET Core и Visual Studio Code в операционных системах macOS, Linux и Windows
ms.author: riande
ms.date: 07/07/2017
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: b25ee29541e345a3bf6700b67d992409c02b183a
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/31/2018
ms.locfileid: "36275279"
---
# <a name="introduction-to-aspnet-core-mvc-on-macos-linux-or-windows"></a>Введение в ASP.NET Core MVC для macOS, Linux и Windows

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

В этом учебнике представлены основы построения веб-приложений MVC ASP.NET Core с помощью [Visual Studio Code](https://code.visualstudio.com) (VS Code). Для работы с этим руководством требуется знание VS Code. Дополнительные сведения см. в разделах [Начало работы с VS Code](https://code.visualstudio.com/docs) и [Справка по Visual Studio Code](#visual-studio-code-help). 

[!INCLUDE [consider RP](../../includes/razor.md)]

Существует 3 версии этого учебника:

* macOS: [Создание приложения ASP.NET Core MVC с помощью Visual Studio для Mac](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [Создание приложения ASP.NET Core MVC с помощью Visual Studio](xref:tutorials/first-mvc-app/start-mvc)
* macOS, Linux и Windows: [Создание приложения MVC ASP.NET Core с помощью Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc) 

## <a name="prerequisites"></a>Предварительные требования

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

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

  - [Сочетания клавиш для macOS](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [Сочетания клавиш для Linux](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [Сочетания клавиш для Windows](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

> [!div class="step-by-step"]
> [Следующая статья — "Добавление контроллера"](adding-controller.md)
