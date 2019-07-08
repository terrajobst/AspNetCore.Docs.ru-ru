---
title: Каркас удостоверений в проектах ASP.NET Core
author: rick-anderson
description: Узнайте, как сформировать шаблон удостоверений в проекте ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/authentication/scaffold-identity
ms.openlocfilehash: 9e784864ee78bdfb76dab7748f3fb81728b9c289
ms.sourcegitcommit: 91cc1f07ef178ab709ea42f8b3a10399c970496e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2019
ms.locfileid: "67622771"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a>Каркас удостоверений в проектах ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

ASP.NET Core 2.1 и более поздних предоставляет [удостоверения ASP.NET Core](xref:security/authentication/identity) как [библиотеки классов Razor](xref:razor-pages/ui-class). Приложения, включающие удостоверение можно применить шаблон выборочно Добавление исходного кода, содержащегося в библиотеке класса Identity Razor (RCL). Вы можете создать исходный код, чтобы изменить код и тем самым изменить поведение. Например, вы можете указать шаблону создать код, используемый при регистрации. Созданный код имеет приоритет над тем же кодом в RCL для идентификаторов. Чтобы получить полный контроль над пользовательского интерфейса и не используйте значение по умолчанию RCL, см. в разделе [создать полное удостоверение пользовательского интерфейса источник](#full).

Приложения, которые **не** включения проверки подлинности можно применить шаблон для добавления пакета RCL удостоверений. Вы можете выбрать, какой код идентификатора будет создан.

Несмотря на то, что шаблон создает большую часть необходимый код, необходимо обновить проект так, чтобы завершить процесс. Этом документе описываются шаги, необходимые для завершения обновления удостоверений формирования шаблонов.

При запуске шаблон удостоверений *ScaffoldingReadme.txt* файл создается в каталоге проекта. *ScaffoldingReadme.txt* файл содержит общие инструкции, на что нужно для завершения обновления удостоверений формирования шаблонов. Этот документ содержит более подробные инструкции, чем *ScaffoldingReadme.txt* файла.

Мы рекомендуем использовать систему управления версиями, показаны различия в файл и дает возможность отката изменений. Какие изменения после выполнения шаблон удостоверений.

> [!NOTE]
> При использовании служб требуются [двухфакторная проверка подлинности](xref:security/authentication/identity-enable-qrcodes), [учетной записи пароль и Подтверждение восстановления](xref:security/authentication/accconfirm)и другие функции безопасности с удостоверением. Службы или службы заглушки не появляется, если формирование шаблонов удостоверений. Службы, чтобы включить эти функции должны быть добавлены вручную. Например, см. в разделе [требуют подтверждение по электронной почте](xref:security/authentication/accconfirm#require-email-confirmation).

## <a name="scaffold-identity-into-an-empty-project"></a>Каркас identity в пустой проект

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

Добавьте следующие выделенные вызовы `Startup` класса:

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a>Удостоверение шаблона в проект Razor без существующей авторизации

<!--
set projNam=RPnoAuth
set projType=webapp
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

Удостоверение настроено в *Areas/Identity/IdentityHostingStartup.cs*. Дополнительные сведения см. в разделе [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a>Миграция, UseAuthentication и макет

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a>Включить проверку подлинности

В `Configure` метод `Startup` вызовите [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) после `UseStaticFiles`:

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a>Изменение макета

Необязательные: Добавьте имя входа частичного (`_LoginPartial`) с файлами макетов:

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a>Удостоверение шаблона в проект Razor без авторизации

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
Некоторые параметры идентификаторов настраиваются в *Areas/Identity/IdentityHostingStartup.cs*. Дополнительные сведения см. в разделе [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a>Каркас удостоверений в проекте MVC без существующей авторизации

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

Необязательные: Добавьте имя входа частичного (`_LoginPartial`) для *Views/Shared/_Layout.cshtml* файла:

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* Переместить *Pages/Shared/_LoginPartial.cshtml* файл *Views/Shared/_LoginPartial.cshtml*

Удостоверение настроено в *Areas/Identity/IdentityHostingStartup.cs*. Дополнительные сведения см. в разделе IHostingStartup.

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

Вызовите [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) после `UseStaticFiles`:

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a>Каркас удостоверений в проекте MVC с авторизацией

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

Удалить *страниц/Shared* папки и файлы в этой папке.

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a>Создать источник полное удостоверение пользовательского интерфейса

Чтобы сохранить полный контроль удостоверение пользовательского интерфейса, запустите шаблон удостоверений и выберите **переопределить все файлы**.

Следующий выделенный код показывает изменения для замены стандартного пользовательского интерфейса удостоверения с удостоверением в веб-приложение ASP.NET Core 2.1. Может потребоваться сделать это для полного управления удостоверение пользовательского интерфейса.

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

По умолчанию удостоверение будет заменен в следующий код:

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

В следующем коде наборы [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), и [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

Зарегистрировать `IEmailSender` реализации, например:

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Изменения в код проверки подлинности для ASP.NET Core 2.1 и более поздних версий](xref:migration/20_21#changes-to-authentication-code)
