---
title: Библиотеки классов Razor компонентов
author: guardrex
description: Узнайте, как компоненты могут быть включены в приложениях Razor компоненты из библиотеки внешнего компонента.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/14/2019
uid: razor-components/class-libraries
ms.openlocfilehash: 1064ad60d90af15af483ba9bca5ed85fb63c2924
ms.sourcegitcommit: 10e14b85490f064395e9b2f423d21e3c2d39ed8b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2019
ms.locfileid: "59515590"
---
# <a name="razor-components-class-libraries"></a>Библиотеки классов Razor компонентов

По [Тиммса Simon](https://github.com/stimms)

Компоненты могут использоваться совместно в библиотеках классов Razor проектов. Компоненты можно включить из:

* Другой проект в решении.
* Пакет NuGet.
* Библиотека .NET, на которую указывает ссылка.

Так же, как компоненты регулярные типы .NET, компоненты, предоставляемые в библиотеках классов Razor — это обычные сборки .NET.

Используйте `razorclasslib` шаблон (библиотека классов Razor) с [команды dotnet new](/dotnet/core/tools/dotnet-new) команды:

```console
dotnet new razorclasslib -o MyComponentLib1
```

Добавить файлы Razor компонентов (*.razor*) для библиотеки классов Razor.

Чтобы добавить в библиотеку в существующий проект, используйте [dotnet sln](/dotnet/core/tools/dotnet-sln) команды:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

```console
dotnet sln add .\MyComponentLib1
```

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

```console
dotnet add WebApplication1 reference MyComponentLib1
```

---

> [!NOTE]
> Библиотеки классов Razor не совместимы с приложениями Blazor в ASP.NET Core предварительной версии 3.
>
> Для создания компонентов библиотеки, который можно использовать совместно с Blazor и Razor компоненты приложения, используйте библиотеку классов Blazor, созданные `blazorlib` шаблона.
>
> Библиотеки классов Razor не поддерживают статических ресурсов в ASP.NET Core предварительной версии 3. С помощью библиотек компонентов `blazorlib` шаблон может включать статические файлы, такие как изображения, JavaScript и таблицы стилей. Во время сборки, статические файлы внедряются в сборку файл (*.dll*), позволяющий потребления компонентов не нужно беспокоиться о том, как включить их ресурсы. Все файлы, включенные в `content` directory помечаются как внедренный ресурс.

## <a name="consume-a-library-component"></a>Использует компонент библиотеки

Чтобы использовать компоненты, определенные в библиотеке в другом проекте [ @addTagHelper ](xref:mvc/views/tag-helpers/intro#add-helper-label) необходимо использовать директиву. Отдельные компоненты могут быть добавлены по имени.

Директивы общие выглядит следующим образом:

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<Component1 />
```

Например, следующая директива добавляет `Component1` из `MyComponentLib1`:

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1
```

Тем не менее, довольно часто для включения всех компонентов из сборки с помощью подстановочного знака (`*`):

```cshtml
@addTagHelper *, MyComponentLib1
```

`@addTagHelper` Директивы может быть включено в *_ViewImport.cshtml* для этих компонентов доступны для всего проекта или примененные к одной странице или набору страниц в папке. С помощью `@addTagHelper` директив на месте, компоненты библиотеки компонентов могут быть использованы как если бы они были в той же сборке, что и приложение.

## <a name="build-pack-and-ship-to-nuget"></a>Сборки, пакет и поставки для NuGet

Так как компонент библиотеки — стандартные библиотеки .NET, упаковки и их отправки в NuGet в качестве примеров, ничем не отличается от упаковки и доставки любую библиотеку в NuGet. Упаковка выполняется с помощью [dotnet pack](/dotnet/core/tools/dotnet-pack) команды:

```console
dotnet pack
```

Отправка пакета NuGet с помощью [публикации dotnet nuget](/dotnet/core/tools/dotnet-nuget-push) команды:

```console
dotnet nuget publish
```

При использовании `blazorlib` шаблона, статические ресурсы будут включены в пакет NuGet. Потребители библиотеки автоматически получать скриптов и таблиц стилей, чтобы потребители не требуется вручную установить ресурсы.
