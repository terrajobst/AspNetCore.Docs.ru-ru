---
title: Начало работы с Blazor
author: guardrex
description: Узнайте, как приступить к работе с Blazor путем создания и изменения проекта Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2019
uid: spa/blazor/get-started
ms.openlocfilehash: f46bd9af0f0762e794349d4e98de5c086a690d72
ms.sourcegitcommit: a1c43150ed46aa01572399e8aede50d4668745ca
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/21/2019
ms.locfileid: "58327233"
---
# <a name="get-started-with-blazor"></a>Начало работы с Blazor

Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Предварительные требования

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

Создание первого проекта Blazor в Visual Studio:

1. Установите последнюю версию [Blazor расширение](https://go.microsoft.com/fwlink/?linkid=870389) в Visual Studio Marketplace. Этот шаг делает доступным Blazor шаблоны для Visual Studio.
1. Шаблоны Blazor стали доступны для использования с .NET Core CLI, выполнив следующую команду в командной строке:

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```

1. Выберите **файл** > **новый проект** > **Web** > **веб-приложение ASP.NET Core**.
1. Убедитесь, что **.NET Core** и **ASP.NET Core 3.0** выбраны вверху.
1. Выберите шаблон **Blazor** и нажмите **ОК**.
1. Нажмите клавишу **F5** для запуска приложения.

Поздравляем! Вы только что выполнили свое первое приложение Blazor!

<!--

# [Visual Studio Code](#tab/visual-studio-code)

Prerequisites:

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

To create your first Blazor project in Visual Studio Code:

1. Execute the following command in a command shell:

   ```console
   dotnet new blazor -o WebApplication1
   ```

1. Open the *WebApplication1* folder in Visual Studio Code.

1. Visual Studio code offers to create assets to build and debug the app, which includes the *tasks.json* and *launch.json* files. Select **Yes** to add the assets.

1. Execute the app using the Visual Studio Code debugger.

1. In a browser, navigate to `https://localhost:5001`.

Congratulations! You just ran your first Blazor app!

# [Visual Studio for Mac](#tab/visual-studio-mac)

.NET Core 3.0 will be supported with Visual Studio for Mac version 8.0 or later. Visual Studio for Mac version 8.0 Preview isn't available at this time.

Use the [.NET Core CLI version of this topic](xref:razor-components/get-started?tabs=netcore-cli) on macOS.

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

To create your first project Blazor project in Visual Studio for Mac:

1. Select **File** > **New Solution** or **New Project**.
1. In the sidebar, select **.NET Core** > **App**.
1. Select **Blazor** and select **Next**.
1. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.
1. In the **Project Name** field, enter `WebApplication1`. Select **Create**.
1. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

Congratulations! You just ran your first Blazor app!
-->

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli/)

Предварительные требования

* [Предварительная версия .NET core SDK 3.0](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. Добавьте шаблоны Blazor, выполнив следующую команду в командной строке:

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```

1. Создайте свой первый проект Blazor в командной строке:

   ```console
   dotnet new blazor -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. В браузере перейдите на адрес `https://localhost:5001`.

Поздравляем! Вы только что выполнили свое первое приложение Blazor!

---

## <a name="blazor-project"></a>Blazor проекта

При запуске приложения, доступны несколько страницы из вкладок на боковой панели.

* Главная страница
* Счетчик
* Выборки данных

На странице Counter нажмите кнопку **Click me** (Щелкните здесь), чтобы увеличить значение счетчика без обновления страницы. Увеличение счетчика в веб-страницы обычно требует написания JavaScript, но Blazor предоставляет лучшую подход с использованием C#.

*Pages/Counter.cshtml*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.cshtml)]

Запрос `/counter` в браузере, в соответствии с `@page` директива сверху, компонент счетчика для отображения его содержимого. В выполняющейся в памяти представление дерева визуализации, можно использовать для обновления пользовательского интерфейса в гибкий и эффективный способ отображения компонентов.

Каждый раз **Click me** выбранной кнопки:

* `onclick` События.
* вызывается метод `IncrementCount` .
* `currentCount` Увеличивается.
* Компонент отображается снова.

Среда выполнения сравнивает новое содержимое для предыдущее содержимое и применяется измененное содержимое только для модели объектов документов (DOM).

Добавление компонента в другой компонент, используя синтаксис типа HTML. Параметры компонента указываются с помощью атрибутов или дочернего содержимого. Например, компонент счетчика можно добавить на домашнюю страницу приложения, добавив `<Counter />` элемент компонент индекса.

В *Pages/Index.cshtml*, замените компонент счетчика опроса Prompt компонента:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.cshtml?highlight=7)]

Запустите приложение. Домашняя страница имеет свой собственный счетчика.

Чтобы добавить параметр к компоненту счетчика, обновите компонента `@functions` блок:

* Добавьте свойство для `IncrementAmount` с `[Parameter]` атрибута.
* Изменение метод `IncrementCount`, чтобы он использовал `IncrementAmount` при увеличении значения `currentCount`.

*Pages/Counter.cshtml*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.cshtml?highlight=4,8)]

Укажите параметр `IncrementAmount` в элементе `<Counter>` для компонента домашней страницы, используя атрибут.

*Pages/Index.cshtml*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.cshtml)]

Запустите приложение. Домашняя страница имеет свой собственный счетчик, который увеличивается каждый раз, десять **Click me** выбранной кнопки.

## <a name="next-steps"></a>Следующие шаги

<xref:tutorials/first-razor-components-app>
