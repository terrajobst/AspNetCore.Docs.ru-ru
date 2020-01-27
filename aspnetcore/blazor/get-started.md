---
title: Начало работы с Blazor ASP.NET Core
author: guardrex
description: Приступайте к работе с Blazor, создав Blazor приложение с любыми инструментами по своему усмотрению.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/get-started
ms.openlocfilehash: 642881b5400a70a99f6e7e262d2a2f1038389ce7
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726846"
---
# <a name="get-started-with-aspnet-core-opno-locblazor"></a>Начало работы с Blazor ASP.NET Core

Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Начало работы с Blazor:

1. Установите [пакет SDK для .NET Core 3,1](https://dotnet.microsoft.com/download/dotnet-core/3.1).

1. При необходимости установите шаблон [Blazor сборки](xref:blazor/hosting-models#blazor-webassembly) :
   * Установите [пакет SDK для .NET Core 3,1 или более поздней версии (Предварительная версия)](https://dotnet.microsoft.com/download/dotnet-core/3.1).
   * Выполните следующую команду в командной оболочке. Объект [Microsoft. AspNetCore.Blazor. Пакет шаблонов](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) имеет предварительную версию, тогда как Blazor сборка доступна в предварительной версии.

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview4.19579.2
   ```

1. Следуйте указаниям по выбору инструментов:

   # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

   1 \. Установите [Visual Studio 2019 версии 16,4 или более поздней](https://visualstudio.microsoft.com/vs/preview/) с рабочей нагрузкой **ASP.NET и веб-разработка** .

   2 \. Создайте новый проект.

   3 \. Выберите **Blazor приложение**. Выберите **Далее**.

   4 \. В поле **Имя проекта** укажите имя проекта или оставьте имя по умолчанию. Убедитесь, что запись **расположения** указана правильно, или укажите расположение для проекта. Выберите **Создать**.

   5 \. Для Blazor процесса сборки выберите шаблон **приложенияBlazor сборки** . Для Blazor работы с сервером выберите шаблон **серверное приложениеBlazor** . Выберите **Создать**. Сведения о двух моделях размещения Blazor, *Blazor сервере* и *Blazor сборки*см. в разделе <xref:blazor/hosting-models>.

   6 \. Нажмите клавишу **Ctrl**+**F5** для запуска приложения.

   > [!NOTE]
   > Если вы установили расширение Blazor Visual Studio для предыдущей предварительной версии ASP.NET Core Blazor (Предварительная версия 6 или более ранней версии), вы можете удалить расширение. Установка шаблонов Blazor в командной оболочке теперь достаточно для отображения шаблонов в Visual Studio.

   # <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code)

   1 \. Установите [Visual Studio Code](https://code.visualstudio.com/).

   2 \. Установите последнюю версию [ C# для расширения Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).

   3 \. Для Blazor процесса сборки выполните следующую команду в командной оболочке:

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      Для работы с Blazor Server выполните следующую команду в командной оболочке:

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      Сведения о двух моделях размещения Blazor, *Blazor сервере* и *Blazor сборки*см. в разделе <xref:blazor/hosting-models>.

   4 \. Откройте папку *WebApplication1* в Visual Studio Code.

   5 \. Для проекта Blazor Server Среда IDE запрашивает добавление ресурсов для сборки и отладки проекта. Выберите ответ **Да**.

   6 \. При использовании серверного приложения Blazor запустите приложение с помощью отладчика Visual Studio Code. При использовании Blazor приложения сборки выполните `dotnet run` из папки проекта приложения.

   7 \. В браузере перейдите на адрес `https://localhost:5001`.

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio для Mac](#tab/visual-studio-mac)

   1 \. Установите [Visual Studio для Mac](https://visualstudio.microsoft.com/vs/mac/).

   2 \. Выберите **файл** > **создать решение** или создать **Новый проект**.

   3 \. На боковой панели выберите > **приложение** **.NET Core** .

   4 \. Выберите шаблон **серверное приложениеBlazor** . В настоящее время в Visual Studio для Mac доступен только шаблон Blazor Server. Для Blazor процесса сборки следуйте инструкциям на вкладке **.NET Core CLI** . Выбрав шаблон Blazor Server, нажмите кнопку **Далее**. Сведения о двух моделях размещения Blazor, *Blazor сервере* и *Blazor сборки*см. в разделе <xref:blazor/hosting-models>.

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   5 \. Задайте для параметра **Целевая платформа** значение **.NET Core 3,1** и нажмите кнопку **Далее**.

   6 \. В поле **имя проекта** Назовите приложение `WebApplication1`. Выберите **Создать**.

   7 \. Выберите **выполнить** > **выполнить без отладки** , чтобы запустить приложение *без отладчика*. Запустите приложение с помощью программы " **начать отладку** ", чтобы запустить приложение *с помощью отладчика*.

   Если появится запрос на доверие к сертификату разработки, то следует доверять сертификату и продолжить.

   # <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli/)

   Для Blazor процесса сборки выполните следующие команды в командной оболочке:

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Для работы с Blazor Server выполните следующие команды в командной оболочке:

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Сведения о двух моделях размещения Blazor, *Blazor сервере* и *Blazor сборки*см. в разделе <xref:blazor/hosting-models>.

   В браузере перейдите на адрес `https://localhost:5001`.

   ---

Из вкладок боковой панели доступны несколько страниц:

* Домашняя страница
* Счетчик
* Выборка данных

На странице Counter нажмите кнопку **Click me** (Щелкните здесь), чтобы увеличить значение счетчика без обновления страницы. Увеличение счетчика на веб-странице обычно требует написания JavaScript, но с Blazor можно использовать C#.

*Pages/Counter.razor*:

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

Запрос `/counter` в браузере, как указано в директиве `@page` в верхней части, заставляет компонент `Counter` визуализировать свое содержимое. Компоненты подготавливаются к просмотру в памяти представления дерева подготовки, которое затем может использоваться для обновления пользовательского интерфейса в гибком и удобном виде.

При каждом выборе кнопки « **Click Me** »:

* Запускается событие `onclick`.
* Вызывается метод `IncrementCount`.
* `currentCount` увеличивается.
* Компонент снова готовится к просмотру.

Среда выполнения сравнивает новое содержимое с предыдущим содержимым и применяет только измененное содержимое к модель DOM (DOM).

Добавьте компонент в другой компонент с помощью синтаксиса HTML. Например, добавьте `Counter` компонент на домашнюю страницу приложения, добавив элемент `<Counter />` в компонент `Index`.

*Pages/Index.razor*:

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

Запустите приложение. Домашняя страница имеет собственный счетчик, предоставляемый компонентом `Counter`.

Параметры компонента указываются с помощью атрибутов или [дочернего содержимого](xref:blazor/components#child-content), которые позволяют задавать свойства дочернего компонента. Чтобы добавить параметр в компонент `Counter`, обновите блок `@code` компонента:

* Добавьте открытое свойство для `IncrementAmount` с атрибутом `[Parameter]`.
* Изменение метод `IncrementCount`, чтобы он использовал `IncrementAmount` при увеличении значения `currentCount`.

*Pages/Counter.razor*:

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

Укажите `IncrementAmount` в элементе `<Counter>` компонента `Index` с помощью атрибута.

*Pages/Index.razor*:

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

Запустите приложение. Компонент `Index` имеет собственный счетчик, который увеличивается на десять каждый раз при выборе кнопки " **нажать** ". Компонент `Counter` (*Counter. Razor*) в `/counter` по-своему увеличивается на единицу.

## <a name="next-steps"></a>Следующие шаги

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:blazor/templates>
* <xref:signalr/introduction>
