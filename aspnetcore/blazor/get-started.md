---
title: Начало работы с Blazor ASP.NET Core
author: guardrex
description: Приступите к работе с Blazor, создав приложение Blazor с помощью предпочтительных средств.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/28/2019
no-loc:
- Blazor
- SignalR
uid: blazor/get-started
ms.openlocfilehash: bd33d874b3d6122f2ab820e9b147b0e62ba03a58
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78648634"
---
# <a name="get-started-with-aspnet-core-blazor"></a>Начало работы с MVC ASP.NET Core Blazor

Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Начало работы с Blazor:

1. Установите [пакет SDK для .NET Core 3.1](https://dotnet.microsoft.com/download/dotnet-core/3.1).

1. Дополнительно установите шаблон [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly).
   * Установите [пакет SDK для .NET Core 3.1 или более поздней версии (предварительная версия)](https://dotnet.microsoft.com/download/dotnet-core/3.1).
   * В командной оболочке выполните следующую команду. В период действия предварительной версии Blazor WebAssembly будет использоваться предварительная версия пакета [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/).

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.2.0-preview1.20073.1
   ```

1. Следуйте указаниям по выбору инструментов:

   # <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

   1\. Установите [Visual Studio 2019 16.4 или более поздней версии](https://visualstudio.microsoft.com/vs/preview/) с рабочей нагрузкой **ASP.NET и веб-разработка**.

   2\. Создайте новый проект.

   3\. Выберите **Приложение Blazor**. Выберите **Далее**.

   4\. В поле **Имя проекта** укажите имя проекта или оставьте имя по умолчанию. Убедитесь, что для проекта правильно указано существующее **расположение** или укажите новое. Выберите **Создать**.

   5\. Для работы с Blazor WebAssembly выберите шаблон **Приложение WebAssembly Blazor**. Для работы с Blazor Server выберите шаблон **Серверное приложение Blazor**. Выберите **Создать**. Сведения о двух моделях размещения Blazor, *Blazor Server* и *Blazor WebAssembly*, см. в разделе <xref:blazor/hosting-models>.

   6\. Нажмите клавишу **Ctrl**+**F5** для запуска приложения.

   > [!NOTE]
   > Если вы установили расширение Visual Studio Blazor для предыдущей предварительной версии ASP.NET Core Blazor (предварительная версия 6 или более ранняя версия), можно удалить расширение. Установки шаблонов Blazor в командной оболочке теперь достаточно для отображения шаблонов в Visual Studio.

   # <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

   1\. Установите [Visual Studio Code](https://code.visualstudio.com/).

   2\. Установите актуальную версию [расширения C# для Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).

   3\. Для работы с Blazor WebAssembly выполните следующую команду в командной оболочке:

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      Для работы с Blazor Server выполните следующую команду в командной оболочке:

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      Сведения о двух моделях размещения Blazor, *Blazor Server* и *Blazor WebAssembly*, см. в разделе <xref:blazor/hosting-models>.

   4\. Откройте папку *WebApplication1* в Visual Studio Code.

   5\. Для проекта Blazor Server среда IDE запрашивает добавление ресурсов для сборки и отладки проекта. Выберите ответ **Да**.

   6\. При использовании серверного приложения Blazor запустите приложение с помощью отладчика Visual Studio Code. Если используется приложение WebAssembly Blazor, выполните `dotnet run` из папки проекта приложения.

   7\. В браузере перейдите на адрес `https://localhost:5001`.

   # <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/visual-studio-mac)

   1\. Установите [Visual Studio для Mac](https://visualstudio.microsoft.com/vs/mac/).

   2\. Выберите **Файл** > **Создать решение** или создайте **новый проект**.

   3\. На боковой панели выберите **.NET Core** > **Приложение**.

   4\. Выберите шаблон **Серверное приложение Blazor**. В Visual Studio для Mac в данный момент доступен только шаблон Blazor Server. Для работы с Blazor WebAssembly следуйте инструкциям на вкладке **.NET Core CLI**. После выбора шаблона Blazor Server нажмите кнопку **Далее**. Сведения о двух моделях размещения Blazor, *Blazor Server* и *Blazor WebAssembly*, см. в разделе <xref:blazor/hosting-models>.

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   5\. Задайте для параметра **Целевая платформа** значение **.NET Core 3.1** и нажмите кнопку **Далее**.

   6\. В поле **Имя проекта** присвойте имя приложению `WebApplication1`. Выберите **Создать**.

   7\. Выберите **Выполнить** > **Запуск без отладки**, чтобы запустить приложение *без отладчика*. Запустите приложение с помощью кнопки **Начать отладку**, чтобы запустить приложение *с отладчиком*.

   При появлении подтверждения доверия к сертификату разработки подтвердите доверие, чтобы продолжить.

   # <a name="net-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli/)

   Для работы с Blazor WebAssembly выполните следующую команду в командной оболочке:

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Для работы с Blazor Server выполните следующую команду в командной оболочке:

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Сведения о двух моделях размещения Blazor, *Blazor Server* и *Blazor WebAssembly*, см. в разделе <xref:blazor/hosting-models>.

   В браузере перейдите на адрес `https://localhost:5001`.

   ---

На вкладках на боковой панели доступно несколько страниц:

* Главная страница
* Счетчик
* Выборка данных

На странице Counter нажмите кнопку **Click me** (Щелкните здесь), чтобы увеличить значение счетчика без обновления страницы. Обычно для увеличения значений счетчика на веб-странице требуется код JavaScript, однако с Blazor можно использовать C#.

*Pages/Counter.razor*:

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

Запрос `/counter` в браузере, как указано в директиве `@page` в верхней части, заставляет компонент `Counter` визуализировать свое содержимое. Компоненты преобразуются в представление дерева визуализации. Его затем можно использовать для гибкого и эффективного обновления пользовательского интерфейса.

Каждый раз при нажатии кнопки **Click me** (Щелкните здесь) выполняются следующие действия:

* Запускается событие `onclick`.
* вызывается метод `IncrementCount` .
* Увеличивается значение `currentCount`.
* Компонент визуализируется снова.

Среда выполнения сравнивает новое содержимое с предыдущим содержимым и применяет к модели DOM только измененное содержимое.

Добавьте компонент в другой компонент, используя синтаксис HTML. Например, добавьте компонент `Counter` на домашнюю страницу приложения, разместив элемент `<Counter />` внутри компонента `Index`.

*Pages/Index.razor*:

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

Запустите приложение. На домашней странице имеется собственный счетчик, предоставленный компонентом `Counter`.

Параметры компонента указываются с помощью атрибутов или [дочернего содержимого](xref:blazor/components#child-content), которые позволяют задавать свойства дочернего компонента. Чтобы добавить параметр в компонент `Counter`, обновите блок `@code`компонента:

* Добавьте открытое свойство для `IncrementAmount` с атрибутом `[Parameter]`.
* Изменение метод `IncrementCount`, чтобы он использовал `IncrementAmount` при увеличении значения `currentCount`.

*Pages/Counter.razor*:

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

Укажите `IncrementAmount` в элементе `<Counter>` для компонента `Index`, используя атрибут.

*Pages/Index.razor*:

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

Запустите приложение. Теперь у компонента `Index` будет собственный счетчик, который будет увеличиваться на десять при каждом нажатии кнопки **Click me** (Щелкните здесь). Компонент `Counter` (*Counter.razor*) в компоненте `/counter` продолжает увеличиваться на единицу.

## <a name="next-steps"></a>Следующие шаги

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:blazor/templates>
* <xref:signalr/introduction>
