---
title: Библиотеки классов компонентов Razor ASP.NET Core
author: guardrex
description: Узнайте, как компоненты можно включать в Blazor приложения из библиотеки внешних компонентов.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- Blazor
uid: blazor/class-libraries
ms.openlocfilehash: 6bac007e3e1d046d6b16a3a0be6dc5976b99b766
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/09/2019
ms.locfileid: "74943879"
---
# <a name="aspnet-core-razor-components-class-libraries"></a>Библиотеки классов компонентов Razor ASP.NET Core

По [Simon Тиммс](https://github.com/stimms)

Компоненты можно совместно использовать в [библиотеке классов Razor (РКЛ)](xref:razor-pages/ui-class) в разных проектах. *Библиотеку классов компонентов Razor* можно включать в:

* Другой проект в решении.
* Пакет NuGet.
* Упоминаемая библиотека .NET.

Так же как и компоненты — обычные типы .NET, компоненты, предоставляемые РКЛ, являются нормальными сборками .NET.

## <a name="create-an-rcl"></a>Создание РКЛ

Следуйте указаниям в статье <xref:blazor/get-started>, чтобы настроить среду для Blazor.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Создайте новый проект.
1. Выберите пункт **Библиотека классов Razor**. Выберите **Далее**.
1. В диалоговом окне **Создание новой библиотеки классов Razor** выберите **создать**.
1. В поле **Имя проекта** укажите имя проекта или оставьте имя по умолчанию. В примерах этого раздела используется имя проекта `MyComponentLib1`. Выберите **Создать**.
1. Добавьте РКЛ в решение:
   1. Щелкните решение правой кнопкой мыши. Выберите **добавить** > **существующий проект**.
   1. Перейдите к файлу проекта РКЛ.
   1. Выберите файл проекта РКЛ ( *. csproj*).
1. Добавьте ссылку на РКЛ из приложения:
   1. Щелкните проект приложения правой кнопкой мыши. Выберите **добавить** > **ссылку**.
   1. Выберите проект РКЛ. Нажмите кнопку **ОК**.

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

1. Используйте шаблон **библиотеки классов Razor** (`razorclasslib`) с командой [DotNet New](/dotnet/core/tools/dotnet-new) в командной оболочке. В следующем примере создается РКЛ с именем `MyComponentLib1`. Папка, содержащая `MyComponentLib1`, создается автоматически при выполнении команды:

   ```dotnetcli
   dotnet new razorclasslib -o MyComponentLib1
   ```

1. Чтобы добавить библиотеку в существующий проект, используйте команду [DotNet Add Reference](/dotnet/core/tools/dotnet-add-reference) в командной оболочке. В следующем примере РКЛ добавляется в приложение. Выполните следующую команду из папки проекта приложения, указав путь к библиотеке:

   ```dotnetcli
   dotnet add reference {PATH TO LIBRARY}
   ```

---

## <a name="consume-a-library-component"></a>Использование компонента библиотеки

Для использования компонентов, определенных в библиотеке в другом проекте, используйте один из следующих подходов:

* Используйте полное имя типа с пространством имен.
* Используйте директиву [\@Razor в](xref:mvc/views/razor#using) синтаксисе. Отдельные компоненты можно добавлять по имени.

В следующих примерах `MyComponentLib1` — это библиотека компонентов, содержащая компонент `SalesReport`.

На компонент `SalesReport` можно ссылаться, используя полное имя типа с пространством имен:

```razor
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLib1.SalesReport />
```

На этот компонент также можно ссылаться, если библиотека входит в область с директивой `@using`:

```razor
@using MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

Включите директиву `@using MyComponentLib1` в файл верхнего уровня *_Import. Razor* , чтобы сделать компоненты библиотеки доступными для всего проекта. Добавьте директиву в файл *_Import. Razor* на любом уровне, чтобы применить пространство имен к одной странице или набору страниц в папке.

## <a name="build-pack-and-ship-to-nuget"></a>Сборка, Упаковка и доставка в NuGet

Так как библиотеки компонентов являются стандартными библиотеками .NET, их упаковка и доставка в NuGet не отличается от упаковки и доставки любых библиотек в NuGet. Упаковка выполняется с помощью команды [DotNet Pack](/dotnet/core/tools/dotnet-pack) в командной оболочке:

```dotnetcli
dotnet pack
```

Отправьте пакет в NuGet с помощью команды [DotNet NuGet Push](/dotnet/core/tools/dotnet-nuget-push) в командной оболочке.

## <a name="create-a-razor-components-class-library-with-static-assets"></a>Создание библиотеки классов компонентов Razor со статическими ресурсами

РКЛ может включать статические ресурсы. Статические ресурсы доступны для любого приложения, использующего библиотеку. Для получения дополнительной информации см. <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:razor-pages/ui-class>
