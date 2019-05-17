---
title: Метапакет Microsoft.AspNetCore.App для ASP.NET Core 2.1 и последующих версий
author: Rick-Anderson
description: Метапакет Microsoft.AspNetCore.App включает все поддерживаемые пакеты ASP.NET Core и Entity Framework Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 04/21/2019
uid: fundamentals/metapackage-app
ms.openlocfilehash: 913e3d83fbf1af7ea995a88202f86c60b359a7e2
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/06/2019
ms.locfileid: "65085655"
---
# <a name="microsoftaspnetcoreapp-metapackage-for-aspnet-core-21-or-later"></a>Метапакет Microsoft.AspNetCore.App для ASP.NET Core 2.1 и последующих версий

Для этой функции нужна платформа ASP.NET Core 2.1 или последующей версии для .NET Core 2.1 или последующей версии.

[Метапакет](/dotnet/core/packages#metapackages) [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) для ASP.NET Core:

* Не включает сторонних зависимостей, за исключением [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/), [Remotion.Linq](https://www.nuget.org/packages/Remotion.Linq/) и [IX-Async](https://www.nuget.org/packages/System.Interactive.Async/). Эти зависимости необходимы для обеспечения работы основных возможностей платформ.
* Включает все поддерживаемые командой ASP.NET Core пакеты, за исключением тех, которые содержат зависимости сторонних разработчиков (кроме указанных выше).
* Включает все поддерживаемые командой Entity Framework Core пакеты, за исключением тех, которые содержат зависимости сторонних разработчиков (кроме указанных выше).

В пакет `Microsoft.AspNetCore.App` входят все компоненты ASP.NET Core 2.1 и более поздних версий, а также Entity Framework Core 2.1 и более поздних версий. Этот пакет по умолчанию используется для шаблонов проектов, предназначенных для ASP.NET Core 2.1 и более поздних версий. Для использования пакета `Microsoft.AspNetCore.App` рекомендуется использовать приложения, предназначенные для ASP.NET Core 2.1 и более поздних версий, а также для Entity Framework Core 2.1 и более поздних версий.

Номер версии метапакета `Microsoft.AspNetCore.App` соответствует минимальной версии ASP.NET Core и версии Entity Framework Core.

Метапакет `Microsoft.AspNetCore.App` предоставляет возможность ограничения по версиям, которые защищают приложение:

* Если включен пакет, который имеет транзитивную (не прямую) зависимость от пакета в `Microsoft.AspNetCore.App`, и эти номера версий не совпадают, NuGet выдаст ошибку.
* Другие пакеты, добавленные в ваше приложение не могут изменять версию пакетов, включенных в `Microsoft.AspNetCore.App`.
* Согласованность версий гарантирует надежности работы. Метапакет `Microsoft.AspNetCore.App` был разработан, чтобы предотвратить использование непроверенных сочетаний версий связанных компонентов в одном приложении.

Приложения, использующие метапакет `Microsoft.AspNetCore.App`, автоматически получают все преимущества общей платформы ASP.NET Core. При использовании метапакета `Microsoft.AspNetCore.App` с приложением не развертываются **никакие** ресурсы из указанных по ссылке пакетов NuGet ASP.NET Core &mdash;: общая платформа .ASP.NET Core уже содержит эти ресурсы. Для сокращения времени запуска приложения ресурсы в общей платформе подвергаются предварительной компиляции. Дополнительную информацию см. в этой публикации об [общей платформе](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).

Следующий файл проекта ссылается на метапакет `Microsoft.AspNetCore.App` для ASP.NET Core и представляет стандартный шаблон ASP.NET Core 2.1:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
  </ItemGroup>

</Project>
```

Предыдущая разметка представляет типичный шаблон ASP.NET Core 2.1 и более поздних версий. Он не задает номер версии в ссылке на пакет `Microsoft.AspNetCore.App`. Если версия не указана, она задается [неявно](https://github.com/dotnet/core/blob/master/release-notes/1.0/sdk/1.0-rc3-implicit-package-refs.md) пакетом SDK, то есть пакетом `Microsoft.NET.Sdk.Web`. Рекомендуется использовать неявное указание версии через пакет SDK, а не задавать номер версии явно в ссылке на пакет. Если у вас возникли вопросы по этому подходу, оставьте комментарий на GitHub в [обсуждении неявного указания версий Microsoft.AspNetCore.App](https://github.com/aspnet/AspNetCore.Docs/issues/6430).

Для переносимых приложений при неявном указании версии устанавливается значение `major.minor.0`. Механизм выбора последней общей платформы будет запускать приложение на последней совместимой версии среди установленных общих платформ. Чтобы гарантировать, что используется одна и та же версия при разработке, тестировании и эксплуатации, убедитесь, что установлена одинаковая версия общей платформы во всех средах. Для автономных приложений неявный номер версии общей платформы, включенной в установленный пакет SDK, устанавливается в значение `major.minor.patch`.

Указание номера версии метапакета `Microsoft.AspNetCore.App` в ссылке на него **не** гарантирует, что будет выбрана эта версия общей платформы. Например, пусть указана версия "2.1.1", но установлена версия "2.1.3". В этом случае приложение будет использовать версию "2.1.3". Хотя это не рекомендуется, можно отключить функцию выбора последней версии (для исправлений и (или) вспомогательных версий). Дополнительную информацию см. в статье о [выборе последней версии на узле .NET](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).

::: moniker range="= aspnetcore-2.1"

Для `<Project Sdk` нужно установить значение `Microsoft.NET.Sdk.Web`, чтобы использовать неявную версию `Microsoft.AspNetCore.App`. При использовании `<Project Sdk="Microsoft.NET.Sdk">` (без `.Web` в конце) происходит следующее:

* Создается такое предупреждение:

  *Предупреждение NU1604. Зависимость проекта Microsoft.AspNetCore.App не содержит включенную нижнюю границу. Включите нижнюю границу в версию зависимости, чтобы гарантировать согласованные результаты восстановления.*

* Это известная проблема с пакетом SDK для NET Core 2.1.

::: moniker-end

<a name="update"></a>

## <a name="update-aspnet-core"></a>Обновление ASP.NET Core

[Метапакет](/dotnet/core/packages#metapackages) `Microsoft.AspNetCore.App` не является традиционным пакетом, который обновляется через NuGet. Аналогично `Microsoft.NETCore.App`, метапакет `Microsoft.AspNetCore.App` представляет собой общую среду выполнения, которая имеет особую семантику номеров версий, обрабатываемую за пределами NuGet. Дополнительную информацию см. в статье [Пакеты, метапакеты и платформы](/dotnet/core/packages).

Чтобы обновить ASP.NET Core, выполните следующие действия:

* На компьютерах разработки и серверах сборки выполните следующее: Скачайте и установите [пакет SDK для .NET Core](https://www.microsoft.com/net/download).
* На серверах развертывания выполните следующее: Скачайте и установите [среду выполнения .NET Core](https://www.microsoft.com/net/download).

 Приложения будут обновлены до последней установленной версии при перезапуске приложения. Номер версии `Microsoft.AspNetCore.App` в файле проекта обновлять не нужно. Дополнительные сведения см. в разделе [Накат платформозависимых приложений](/dotnet/core/versions/selection#framework-dependent-apps-roll-forward).

Если вы уже использовали `Microsoft.AspNetCore.All` в своем приложении, см. раздел [Переход от Microsoft.AspNetCore.All к Microsoft.AspNetCore.App](xref:fundamentals/metapackage#migrate).
