---
title: Метапакет Microsoft.AspNetCore.App для ASP.NET Core 2.1 и более поздних версий
author: Rick-Anderson
description: Метапакет Microsoft.AspNetCore.App включает все поддерживаемые пакеты ASP.NET Core и Entity Framework Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 09/20/2017
uid: fundamentals/metapackage-app
ms.openlocfilehash: 95fd6b7e73cf325674f1c1e03f9eea88cbc1af13
ms.sourcegitcommit: f3538693a12cf55b7f124a6519677239170b7c43
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/28/2018
ms.locfileid: "43114779"
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

Следующий файл проекта ссылается на метапакет `Microsoft.AspNetCore.App` для ASP.NET Core и представляет стандартный шаблон ASP.NET Core 2.1:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" Version="2.1.1" />
  </ItemGroup>

</Project>
```

Указание номера версии метапакета `Microsoft.AspNetCore.App` в ссылке на него **не** гарантирует, что будет использоваться эта версия общей платформы. Например, пусть указана версия `2.1.1`, но установлена версия `2.1.3`. В этом случае приложение будет использовать версию `2.1.3`. Хотя это и не рекомендуется, можно отключить функцию выбора последней версии (для исправлений и (или) вспомогательных версий). Дополнительные сведения о процедуре выбора последней версии пакета см. [здесь](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).

## <a name="update-aspnet-core"></a>Обновление ASP.NET Core

[Метапакет](/dotnet/core/packages#metapackages) `Microsoft.AspNetCore.App` не является традиционным пакетом, который обновляется через NuGet. Аналогично `Microsoft.NETCore.App`, метапакет `Microsoft.AspNetCore.App` представляет собой общую среду выполнения, которая имеет особую семантику номеров версий, обрабатываемую за пределами NuGet. Дополнительную информацию см. в статье [Пакеты, метапакеты и платформы](/dotnet/core/packages).

Чтобы обновить ASP.NET Core, выполните следующие действия:

* На компьютерах разработчиков и серверах сборки: скачайте и установите [пакет SDK для .NET Core](https://www.microsoft.com/net/download).
* На серверах развертывания: скачайте и установите [среду выполнения .NET Core](https://www.microsoft.com/net/download).

 Приложения будут обновлены до последней установленной версии при перезапуске приложения. Номер версии `Microsoft.AspNetCore.App` в файле проекта обновлять не нужно. Дополнительные сведения см. в разделе [Накат платформозависимых приложений](/dotnet/core/versions/selection#framework-dependent-apps-roll-forward).

Если вы уже использовали `Microsoft.AspNetCore.All` в своем приложении, см. раздел [Переход от Microsoft.AspNetCore.All к Microsoft.AspNetCore.App](xref:fundamentals/metapackage#migrate).
