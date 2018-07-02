---
title: Метапакет Microsoft.AspNetCore.App для ASP.NET Core 2.1 и более поздних версий
author: Rick-Anderson
description: Метапакет Microsoft.AspNetCore.App включает все поддерживаемые пакеты ASP.NET Core и Entity Framework Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 09/20/2017
uid: fundamentals/metapackage-app
ms.openlocfilehash: 4840d0a7536b1e9d8da835690b285ac2074967f5
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277476"
---
# <a name="microsoftaspnetcoreapp-metapackage-for-aspnet-core-21"></a>Метапакет Microsoft.AspNetCore.App для ASP.NET Core 2.1

Для этой функции нужен ASP.NET Core 2.1 или более поздней версии, нацеленный на .NET Core 2.1 или более поздней версии.

[Метапакет](/dotnet/core/packages#metapackages) [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) для ASP.NET Core:

* Не включает сторонних зависимостей, за исключением [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/), [Remotion.Linq](https://www.nuget.org/packages/Remotion.Linq/) и [IX-Async](https://www.nuget.org/packages/System.Interactive.Async/). Эти зависимости необходимы для обеспечения работы основных возможностей платформ.
* Включает все поддерживаемые командой ASP.NET Core пакеты, за исключением тех, которые содержат зависимости сторонних разработчиков (кроме указанных выше).
* Включает все поддерживаемые командой Entity Framework Core пакеты, за исключением тех, которые содержат зависимости сторонних разработчиков (кроме указанных выше).

В пакет `Microsoft.AspNetCore.App` входят все компоненты ASP.NET Core 2.1 и более поздних версий, а также Entity Framework Core 2.1 и более поздних версий. Этот пакет по умолчанию используется для шаблонов проектов, предназначенных для ASP.NET Core 2.1 и более поздних версий. Для использования пакета `Microsoft.AspNetCore.App` рекомендуется использовать приложения, предназначенные для ASP.NET Core 2.1 и более поздних версий, а также для Entity Framework Core 2.1 и более поздних версий.

Номер версии метапакета `Microsoft.AspNetCore.App` представляет версию ASP.NET Core и версию Entity Framework Core.

Метапакет `Microsoft.AspNetCore.App` предоставляет возможность ограничения по версиям, которые защищают приложение:

* Если включен пакет, который имеет транзитивную (не прямую) зависимость от пакета в `Microsoft.AspNetCore.App`, и эти номера версий не совпадают, NuGet выдаст ошибку.
* Другие пакеты, добавленные в ваше приложение не могут изменять версию пакетов, включенных в `Microsoft.AspNetCore.App`.
* Согласованность версий гарантирует надежности работы. Метапакет `Microsoft.AspNetCore.App` был разработан, чтобы предотвратить использование непроверенных сочетаний версий связанных компонентов в одном приложении.

Приложения, использующие метапакет `Microsoft.AspNetCore.App`, автоматически получают все преимущества общей платформы ASP.NET Core. При использовании метапакета `Microsoft.AspNetCore.App` с приложением не развертываются **никакие** ресурсы из указанных по ссылке пакетов NuGet ASP.NET Core &mdash;: общая платформа .ASP.NET Core уже содержит эти ресурсы. Для сокращения времени запуска приложения ресурсы в общей платформе подвергаются предварительной компиляции. Дополнительные сведения об "общей платформе" см. в статье [Упаковка дистрибутивов .NET Core](/dotnet/core/build/distribution-packaging).

Следующий файл проекта *.csproj* ссылается на метапакет `Microsoft.AspNetCore.App` для ASP.NET Core:

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

Предыдущая разметка представляет типичный шаблон ASP.NET Core 2.1 и более поздних версий. Он не задает номер версии в ссылке на пакет `Microsoft.AspNetCore.App`. Если версия не указана, она задается [неявно](https://github.com/dotnet/core/blob/master/release-notes/1.0/sdk/1.0-rc3-implicit-package-refs.md) пакетом SDK, то есть пакетом `Microsoft.NET.Sdk.Web`. Рекомендуется использовать неявное указание версии через пакет SDK, а не задавать номер версии явно в ссылке на пакет. Вы можете оставить комментарий на GitHub в [обсуждении неявного указания версий Microsoft.AspNetCore.App](https://github.com/aspnet/Docs/issues/6430).

Для переносимых приложений при неявном указании версии устанавливается значение `major.minor.0`. Механизм выбора последней общей платформы будет запускать приложение на последней совместимой версии среди установленных общих платформ. Чтобы гарантировать, что используется одна и та же версия при разработке, тестировании и эксплуатации, убедитесь, что установлена одинаковая версия общей платформы во всех средах. Для автономных приложений неявный номер версии общей платформы, включенной в установленный пакет SDK, устанавливается в значение `major.minor.patch`.

Указание номера версии метапакета `Microsoft.AspNetCore.App` в ссылке на него **не** гарантирует, что будет выбрана эта версия общей платформы. Например, пусть указана версия "2.1.1", но установлена версия "2.1.3". В этом случае приложение будет использовать версию "2.1.3". Хотя это не рекомендуется, можно отключить функцию выбора последней версии (для исправлений и (или) вспомогательных версий). Дополнительную информацию см. в статье о [выборе последней версии на узле .NET](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).

[Метапакет](/dotnet/core/packages#metapackages) `Microsoft.AspNetCore.App` не является традиционным пакетом, который обновляется через NuGet. Аналогично `Microsoft.NETCore.App`, метапакет `Microsoft.AspNetCore.App` представляет собой общую среду выполнения, которая имеет особую семантику номеров версий, обрабатываемую за пределами NuGet. Дополнительную информацию см. в статье [Пакеты, метапакеты и платформы](/dotnet/core/packages).

Для `<Project Sdk` нужно установить значение `Microsoft.NET.Sdk.Web`, чтобы использовать неявную версию `Microsoft.AspNetCore.App`.  Когда используется `<Project Sdk="Microsoft.NET.Sdk">`, создаются следующие предупреждения:

*Предупреждение NU1604. Зависимость проекта Microsoft.AspNetCore.App не содержит включенную нижнюю границу. Включите нижнюю границу в версию зависимости, чтобы гарантировать согласованные результаты восстановления.*

*Предупреждение NU1602. [Имя проекта] не предоставляет включенную нижнюю границу для зависимости Microsoft.AspNetCore.App. Было разрешено приблизительное лучшее соответствие Microsoft.AspNetCore.App 2.1.0.*

Если вы уже использовали `Microsoft.AspNetCore.All` в своем приложении, см. раздел [Переход от Microsoft.AspNetCore.All к Microsoft.AspNetCore.App](xref:fundamentals/metapackage#migrate).
