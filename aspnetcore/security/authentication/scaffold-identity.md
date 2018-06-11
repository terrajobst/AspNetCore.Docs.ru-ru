---
title: Удостоверение формирования шаблонов в проектах ASP.NET Core
author: rick-anderson
description: Дополнительные сведения о формировать удостоверения в проекте ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 5/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/scaffold-identity
ms.openlocfilehash: e7a2cf3633ed48a0d2030739cdc092441fcae2ff
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252039"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a>Удостоверение формирования шаблонов в проектах ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

Предоставляет ASP.NET Core 2.1 и более поздние версии [ASP.NET Core Identity](xref:security/authentication/identity) как [библиотеки классов Razor](xref:mvc/razor-pages/ui-class). Приложения, включающие удостоверение можно применить scaffolder выборочно Добавление исходного кода, содержащегося в библиотеке класса Razor идентификаторов (RCL). Можно создать исходный код, чтобы можно было изменить код и изменить поведение. Например можно указать scaffolder для создания кода, используемое при регистрации. Созданный код имеет приоритет над один и тот же код в RCL удостоверений.

Приложения, которые **не** включения проверки подлинности можно применить scaffolder Добавление RCL удостоверение пакета. У вас есть возможность выбрать удостоверение код должен быть создан.

Несмотря на то, что scaffolder создает большую часть необходимый код, необходимо обновить проект, чтобы завершить процесс. В этом документе объясняется шаги, необходимые для завершения обновления удостоверения формирования шаблонов.

При запуске удостоверения scaffolder *ScaffoldingReadme.txt* файл создается в каталоге проекта. *ScaffoldingReadme.txt* файл содержит общие инструкции, на что требуется для завершения обновления удостоверения формирования шаблонов. Этот документ содержит более подробные инструкции, чем *ScaffoldingReadme.txt* файла.

Мы рекомендуем использовать систему управления версиями, показаны различия в файл и дает возможность отката изменений. Проверьте изменения после выполнения scaffolder удостоверений.

## <a name="scaffold-identity-into-an-empty-project"></a>Удостоверение формирования шаблонов в пустой проект

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

Добавьте следующий выделенный вызовы `Startup` класса:

[!code-csharp[Main](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a>Удостоверение формирования шаблонов в проект Razor без существующей авторизации

<!--
set projNam=RPnoAuth
set projType=razor
set version=2.1.0

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v %version%
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

Удостоверение настраивается в *Areas/Identity/IdentityHostingStartup.cs*. Дополнительные сведения см. в разделе [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a>Миграция, UseAuthentication и макет

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

В `Configure` метод `Startup` вызовите [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) после `UseStaticFiles`:

[!code-csharp[Main](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a>Изменение макета

Необязательно: Добавьте имя входа частичного (`_LoginPartial`) файл макета:

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a>Удостоверение формирования шаблонов в проект Razor с авторизации

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files Account.Register

[!INCLUDE[](~/includes/webapp-alias-notice.md)]
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
Некоторые параметры идентификаторов настраиваются в *Areas/Identity/IdentityHostingStartup.cs*. Дополнительные сведения см. в разделе [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a>Удостоверение формирования шаблонов в проект MVC без существующей авторизации

<!--
set projNam=MvcNoAuth
set projType=mvc
set version=2.1.0

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v %version%
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

Необязательно: Добавьте имя входа частичного (`_LoginPartial`) для *Views/Shared/_Layout.cshtml* файла:

[!code-html[Main](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* Переместить *Pages/Shared/_LoginPartial.cshtml* файл *Views/Shared/_LoginPartial.cshtml*

Удостоверение настраивается в *Areas/Identity/IdentityHostingStartup.cs*. Дополнительные сведения см. в разделе IHostingStartup.

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

Вызовите [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) после `UseStaticFiles`:

[!code-csharp[Main](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a>Удостоверение формирования шаблонов в проект MVC с авторизации

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

Удалить *страниц/Общие* папки и файлы в этой папке.
