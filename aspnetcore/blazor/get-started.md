---
title: Начало работы с ASP.NET Core Блазор
author: guardrex
description: Приступите к работе с Блазор, создав приложение Блазор с любыми инструментами по своему усмотрению.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/07/2019
uid: blazor/get-started
ms.openlocfilehash: b5043c7e4549800c1ab49bc37dd8f3568975d4aa
ms.sourcegitcommit: 67116718dc33a7a01696d41af38590fdbb58e014
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/07/2019
ms.locfileid: "73799231"
---
# <a name="get-started-with-aspnet-core-blazor"></a>Начало работы с ASP.NET Core Блазор

Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Начало работы с Блазор:

::: moniker range=">= aspnetcore-3.1"

1. Установите [пакет SDK для предварительной версии .NET Core 3,1](https://dotnet.microsoft.com/download/dotnet-core/3.1).

1. Установите шаблон [блазор-Assembly](xref:blazor/hosting-models#blazor-webassembly) , выполнив следующую команду в командной оболочке. Пакет [Microsoft. AspNetCore. блазор. templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) имеет предварительную версию, а блазор уже находится в предварительной версии.

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview2.19528.8
   ```

1. Следуйте указаниям по выбору инструментов:

   # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

   1 \. Установите [Visual Studio 16,4 Preview 2 или более поздней версии](https://visualstudio.microsoft.com/vs/preview/) с рабочей нагрузкой **ASP.NET и веб-разработка** .

   2 \. Создайте новый проект.

   3 \. Выберите **приложение блазор**. Выберите **Далее**.

   4 \. В поле **Имя проекта** укажите имя проекта или оставьте имя по умолчанию. Убедитесь, что запись **расположения** указана правильно, или укажите расположение для проекта. Выберите **Создать**.

   5 \. Для интерфейса Блазор можно выбрать шаблон **приложения блазор для сборки** . Для удобства работы с сервером Блазор выберите шаблон **серверное приложение блазор** . Выберите **Создать**. Сведения о двух Блазор моделях размещения, *Блазор Server* и *блазор Assembly*см. в разделе <xref:blazor/hosting-models>.

   6 \. Нажмите клавишу **Ctrl**+**F5** для запуска приложения.

   > [!NOTE]
   > Если вы установили расширение Visual Studio Блазор для предыдущей предварительной версии ASP.NET Core Блазор (Предварительная версия 6 или более ранняя версия), вы можете удалить расширение. Установка шаблонов Блазор в командной оболочке теперь достаточно для отображения шаблонов в Visual Studio.

   # <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code)

   1 \. Установите [Visual Studio Code](https://code.visualstudio.com/).

   2 \. Установите последнюю версию [ C# для расширения Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).

   3 \. Для работы с Блазор-сборками выполните следующую команду в командной оболочке:

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      Для работы с сервером Блазор выполните следующую команду в командной оболочке:

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      Сведения о двух Блазор моделях размещения, *Блазор Server* и *блазор Assembly*см. в разделе <xref:blazor/hosting-models>.

   4 \. Откройте папку *WebApplication1* в Visual Studio Code.

   5 \. Для проекта сервера Блазор среда IDE запрашивает добавление ресурсов для сборки и отладки проекта. Выберите ответ **Да**.

   6 \. При использовании серверного приложения Блазор запустите приложение с помощью отладчика Visual Studio Code. Если используется Блазор приложение сборки, выполните `dotnet run` из папки проекта приложения.

   7 \. В браузере перейдите на адрес `https://localhost:5001`.

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1\. Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2\. Select **File** > **New Solution** or **New Project**.

   3\. In the sidebar, select **.NET Core** > **App**.

   4\. For a Blazor Server experience, select the **Blazor Server App** template. For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.

   5\. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6\. In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7\. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli/)

   Для работы с Блазор-сборками выполните следующие команды в командной оболочке:

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Для работы с сервером Блазор выполните следующие команды в командной оболочке:

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Сведения о двух Блазор моделях размещения, *Блазор Server* и *блазор Assembly*см. в разделе <xref:blazor/hosting-models>.

   В браузере перейдите на адрес `https://localhost:5001`.

   ---

::: moniker-end

::: moniker range="< aspnetcore-3.1"

1. Установите последнюю версию [пакета SDK для .NET Core 3,0](https://dotnet.microsoft.com/download/dotnet-core/3.0) .

1. При необходимости установите шаблон [блазор-Assembly](xref:blazor/hosting-models#blazor-webassembly) , установив [пакет SDK для .NET Core 3,1 Preview](https://dotnet.microsoft.com/download/dotnet-core/3.1) , а затем выполнив следующую команду в командной оболочке:

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview2.19528.8
   ```

1. Следуйте указаниям по выбору инструментов:

   # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

   1 \. Установите последнюю версию [Visual Studio](https://visualstudio.com/vs/) с рабочей нагрузкой **ASP.NET и веб-разработка** .

   2 \. При необходимости установите [Visual Studio 16,4 Preview 2 или более поздней версии](https://visualstudio.microsoft.com/vs/preview/) с рабочей нагрузкой **ASP.NET и веб-разработка** для разработки приложений блазор.

   3 \. Создайте новый проект.

   4 \. Выберите **приложение блазор**. Выберите **Далее**.

   5 \. В поле **Имя проекта** укажите имя проекта или оставьте имя по умолчанию. Убедитесь, что запись **расположения** указана правильно, или укажите расположение для проекта. Выберите **Создать**.

   6 \. Для интерфейса Блазор можно выбрать шаблон **приложения блазор для сборки** . Для удобства работы с сервером Блазор выберите шаблон **серверное приложение блазор** . Выберите **Создать**. Сведения о двух Блазор моделях размещения, *Блазор Server* и *блазор Assembly*см. в разделе <xref:blazor/hosting-models>.

   7 \. Нажмите клавишу **F5** для запуска приложения.

   > [!NOTE]
   > Если вы установили расширение Visual Studio Блазор для предыдущей предварительной версии ASP.NET Core Блазор (Предварительная версия 6 или более ранняя версия), вы можете удалить расширение. Установка шаблонов Блазор в командной оболочке теперь достаточно для отображения шаблонов в Visual Studio.

   # <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code)

   1 \. Установите [Visual Studio Code](https://code.visualstudio.com/).

   2 \. Установите последнюю версию [ C# для расширения Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).

   3 \. Для работы с Блазор-сборками выполните следующую команду в командной оболочке:

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      Для работы с сервером Блазор выполните следующую команду в командной оболочке:

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      Сведения о двух Блазор моделях размещения, *Блазор Server* и *блазор Assembly*см. в разделе <xref:blazor/hosting-models>.

   4 \. Откройте папку *WebApplication1* в Visual Studio Code.

   5 \. Для проекта сервера Блазор среда IDE запрашивает добавление ресурсов для сборки и отладки проекта. Выберите ответ **Да**.

   6 \. При использовании серверного приложения Блазор запустите приложение с помощью отладчика Visual Studio Code. Если используется Блазор приложение сборки, выполните `dotnet run` из папки проекта приложения.

   7 \. В браузере перейдите на адрес `https://localhost:5001`.

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1\. Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2\. Select **File** > **New Solution** or **New Project**.

   3\. In the sidebar, select **.NET Core** > **App**.

   4\. For a Blazor Server experience, select the **Blazor Server App** template. For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.

   5\. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6\. In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7\. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli/)

   Для работы с Блазор-сборками выполните следующие команды в командной оболочке:

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Для работы с сервером Блазор выполните следующие команды в командной оболочке:

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Сведения о двух Блазор моделях размещения, *Блазор Server* и *блазор Assembly*см. в разделе <xref:blazor/hosting-models>.

   В браузере перейдите на адрес `https://localhost:5001`.

   ---

::: moniker-end

Из вкладок боковой панели доступны несколько страниц:

* Главная страница
* Счетчик
* Выборка данных

На странице Counter нажмите кнопку **Click me** (Щелкните здесь), чтобы увеличить значение счетчика без обновления страницы. Для увеличения счетчика на веб-странице обычно требуется написание кода JavaScript, но с Блазор можно C#использовать.

*Pages/Counter.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

Запрос `/counter` в браузере, как указано в директиве `@page` в верхней части, приводит к тому, что компонент `Counter` отображает его содержимое. Компоненты подготавливаются к просмотру в памяти представления дерева подготовки, которое затем может использоваться для обновления пользовательского интерфейса в гибком и удобном виде.

При каждом выборе кнопки « **Click Me** »:

* Будет вызвано событие `onclick`.
* вызывается метод `IncrementCount` .
* Значение `currentCount` увеличивается.
* Компонент снова готовится к просмотру.

Среда выполнения сравнивает новое содержимое с предыдущим содержимым и применяет только измененное содержимое к модель DOM (DOM).

Добавьте компонент в другой компонент с помощью синтаксиса HTML. Например, добавьте компонент `Counter` на домашнюю страницу приложения, добавив элемент `<Counter />` в компонент `Index`.

*Pages/Index.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

Запустите приложение. Домашняя страница имеет собственный счетчик, предоставляемый компонентом `Counter`.

Параметры компонента указываются с помощью атрибутов или [дочернего содержимого](xref:blazor/components#child-content), которые позволяют задавать свойства дочернего компонента. Чтобы добавить параметр в компонент `Counter`, обновите блок `@code` компонента:

* Добавьте открытое свойство для `IncrementAmount` с атрибутом `[Parameter]`.
* Изменение метод `IncrementCount`, чтобы он использовал `IncrementAmount` при увеличении значения `currentCount`.

*Pages/Counter.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

Укажите `IncrementAmount` в элементе `<Counter>` компонента `Index` с помощью атрибута.

*Pages/Index.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

Запустите приложение. Компонент `Index` имеет собственный счетчик, который увеличивается на десять каждый раз при выборе кнопки " **нажать** ". Компонент `Counter` (*Counter. Razor*) в `/counter` продолжит приращение на единицу.

## <a name="next-steps"></a>Следующие шаги

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:signalr/introduction>
