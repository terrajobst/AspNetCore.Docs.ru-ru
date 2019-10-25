---
title: Удостоверение шаблона в ASP.NET Core проектах
author: rick-anderson
description: Узнайте, как сформировать удостоверение в проекте ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/authentication/scaffold-identity
ms.openlocfilehash: ca2046563281efc3c1cd8f4fec73fe4f8d3fbdda
ms.sourcegitcommit: 383017d7060a6d58f6a79cf4d7335d5b4b6c5659
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/23/2019
ms.locfileid: "72816107"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a>Удостоверение шаблона в ASP.NET Core проектах

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT)

ASP.NET Core 2,1 и более поздних версий предоставляет [ASP.NET Core удостоверение](xref:security/authentication/identity) как [библиотеку классов Razor](xref:razor-pages/ui-class). Приложения, включающие удостоверение, могут применять шаблон для выборочного добавления исходного кода, содержащегося в библиотеке классов Razor класса Identity (РКЛ). Вы можете создать исходный код, чтобы изменить код и тем самым изменить поведение. Например, вы можете указать шаблону создать код, используемый при регистрации. Созданный код имеет приоритет над тем же кодом в RCL для идентификаторов. Чтобы получить полный контроль над пользовательским интерфейсом и не использовать РКЛ по умолчанию, см. раздел [Создание полного идентификатора пользовательского интерфейса идентификации](#full).

Приложения, которые **не** включают проверку подлинности, могут применять механизм формирования шаблонов для добавления пакета РКЛ Identity. Вы можете выбрать, какой код идентификатора будет создан.

Хотя этот механизм создает большую часть необходимого кода, необходимо обновить проект, чтобы завершить процесс. В этом документе объясняются шаги, необходимые для завершения обновления формирования шаблонов удостоверений.

При запуске механизма формирования идентификаторов в каталоге проекта создается файл *скаффолдингреадме. txt* . Файл *скаффолдингреадме. txt* содержит общие инструкции о том, что необходимо для завершения обновления формирования шаблонов удостоверений. Этот документ содержит более полные инструкции, чем файл *скаффолдингреадме. txt* .

Рекомендуется использовать систему управления версиями, которая показывает различия в файлах и позволяет вносить изменения. Проверьте изменения после запуска шаблона удостоверений.

> [!NOTE]
> Службы необходимы при использовании [двухфакторной проверки подлинности](xref:security/authentication/identity-enable-qrcodes), [подтверждения учетной записи и восстановления пароля](xref:security/authentication/accconfirm), а также других функций безопасности с удостоверениями. Службы или заглушки служб не создаются при формировании удостоверений формирования шаблонов. Службы для включения этих функций необходимо добавить вручную. Например, см. статью [требование подтверждения по электронной почте](xref:security/authentication/accconfirm#require-email-confirmation).

## <a name="scaffold-identity-into-an-empty-project"></a>Удостоверение шаблона в пустой проект

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

Добавьте следующие выделенные вызовы в класс `Startup`:

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a>Удостоверение шаблона в проекте Razor без существующей авторизации

<!--  Updated for 3.0
set projNam=RPnoAuth
set projType=webapp

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
dotnet add package Microsoft.AspNetCore.Identity.UI
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

Удостоверение настраивается в *области/Identity/идентитихостингстартуп. CS*. Дополнительные сведения см. в разделе [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a>Миграция, Усеаусентикатион и макет

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a>Включить проверку подлинности

В методе `Configure` класса `Startup` вызовите [усеаусентикатион](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) после `UseStaticFiles`:

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a>Изменения макета

Необязательно: Добавьте часть имени для входа partial (`_LoginPartial`) в файл макета:

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a>Удостоверение шаблона в проекте Razor с авторизацией

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
Некоторые параметры удостоверения настраиваются в *области/Identity/идентитихостингстартуп. CS*. Дополнительные сведения см. в разделе [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a>Удостоверение шаблона в проекте MVC без существующей авторизации

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

Необязательно: Добавьте часть имени для входа partial (`_LoginPartial`) в файл *views/Shared/_layout. cshtml* :

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* Перемещение файла *pages/Shared/_LoginPartial. cshtml* в *views/Shared/_LoginPartial. cshtml*

Удостоверение настраивается в *области/Identity/идентитихостингстартуп. CS*. Дополнительные сведения см. в разделе IHostingStartup.

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

Вызовите [усеаусентикатион](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) после `UseStaticFiles`:

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a>Удостоверение шаблона в проекте MVC с авторизацией

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext  --files "Account.Login;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

Удалите папки *pages/Shared* и файлы в этой папке.

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a>Создание полного источника идентификатора пользовательского интерфейса

Чтобы обеспечить полный контроль над пользовательским интерфейсом удостоверений, запустите механизм формирования удостоверений и выберите **переопределить все файлы**.

В следующем выделенном коде показаны изменения, которые заменяют пользовательский интерфейс удостоверения по умолчанию удостоверением в веб-приложении ASP.NET Core 2,1. Это может потребоваться для полного управления пользовательским интерфейсом идентификации.

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

Удостоверение по умолчанию заменяется в следующем коде:

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

Следующий код задает [логинпас](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [логаутпас](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)и [акцессдениедпас](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

Зарегистрируйте реализацию `IEmailSender`, например:

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

<!--
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
-->
## <a name="disable-register-page"></a>Отключить страницу регистрации

Чтобы отключить регистрацию пользователей, выполните следующие действия.

* Удостоверение шаблона. Включить учетную запись. регистрация, учетная запись. Login и Account. Регистерконфирматион. Пример:

  ```dotnetcli
   dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
  ```

* Обновите *области/идентификаторы/страницы/учетная запись/Register. cshtml. CS* , чтобы пользователи не могли зарегистрироваться из этой конечной точки:

  [!code-csharp[](scaffold-identity/sample/Register.cshtml.cs?name=snippet)]

* Обновите *области, идентификаторы, страницы, учетную запись или Register. cshtml* , чтобы они были совместимы с предыдущими изменениями:

  [!code-cshtml[](scaffold-identity/sample/Register.cshtml)]

* Закомментируйте или удалите ссылку регистрации из *областей, удостоверений, страниц, учетной записи или имени входа. cshtml*

```cshtml
@*
<p>
    <a asp-page="./Register" asp-route-returnUrl="@Model.ReturnUrl">Register as a new user</a>
</p>
*@
```

* Обновите страницу *области, удостоверение, страницы/учетная запись или регистерконфирматион* .

  * Удалите код и ссылки из файла CSHTML.
  * Удалите код подтверждения из `PageModel`:

  ```csharp
   [AllowAnonymous]
    public class RegisterConfirmationModel : PageModel
    {
        public IActionResult OnGet()
        {  
            return Page();
        }
    }
  ```
  
### <a name="use-another-app-to-add-users"></a>Использование другого приложения для добавления пользователей

Предоставьте механизм для добавления пользователей за пределами веб-приложения. Ниже перечислены параметры для добавления пользователей.

* Выделенное административное веб-приложение.
* Консольное приложение.

В следующем примере кода один из подходов к добавлению пользователей:

* Список пользователей считывается в память.
* Для каждого пользователя создается надежный уникальный пароль.
* Пользователь будет добавлен в базу данных удостоверений.
* Пользователь получает уведомления и сообщил об изменении пароля.

[!code-csharp[](scaffold-identity/consoleAddUser/Program.cs?name=snippet)]

В следующем коде показано, как добавить пользователя:

[!code-csharp[](scaffold-identity/consoleAddUser/Data/SeedData.cs?name=snippet)]

Подобный подход можно выполнить для рабочих сценариев.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Изменения кода проверки подлинности на ASP.NET Core 2,1 и более поздних версий](xref:migration/20_21#changes-to-authentication-code)
