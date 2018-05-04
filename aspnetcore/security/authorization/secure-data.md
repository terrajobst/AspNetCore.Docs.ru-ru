---
title: Создание приложения ASP.NET Core пользовательскими данными, защищенных авторизации
author: rick-anderson
description: Узнайте, как для создания приложения страниц Razor с данными пользователя, защищен авторизации. Включает в себя HTTPS, проверки подлинности, безопасность ASP.NET Core Identity.
manager: wpickett
ms.author: riande
ms.date: 01/24/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/secure-data
ms.openlocfilehash: e42f299efcae7c6a0e3d20b157c591eed98c99d0
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="489bc-104">Создание приложения ASP.NET Core пользовательскими данными, защищенных авторизации</span><span class="sxs-lookup"><span data-stu-id="489bc-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="489bc-105">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Джо Одетт](https://twitter.com/joeaudette) (Joe Audette)</span><span class="sxs-lookup"><span data-stu-id="489bc-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="489bc-106">Этого учебника показано, как создать веб-приложение ASP.NET Core пользовательскими данными, защищенных авторизации.</span><span class="sxs-lookup"><span data-stu-id="489bc-106">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="489bc-107">Отображается список контактов, прошедшие проверку подлинности (зарегистрированные) пользователи создали.</span><span class="sxs-lookup"><span data-stu-id="489bc-107">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="489bc-108">Существует три группы безопасности:</span><span class="sxs-lookup"><span data-stu-id="489bc-108">There are three security groups:</span></span>

* <span data-ttu-id="489bc-109">**Зарегистрированные пользователи** можно просмотреть все утвержденные данные и можно изменить или удалить свои собственные данные.</span><span class="sxs-lookup"><span data-stu-id="489bc-109">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="489bc-110">**Диспетчеры** можно утвердить или отклонить контактных данных.</span><span class="sxs-lookup"><span data-stu-id="489bc-110">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="489bc-111">Только утвержденные контакты отображаются для пользователей.</span><span class="sxs-lookup"><span data-stu-id="489bc-111">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="489bc-112">**Администраторы** можно утвердить или отклонить и изменить или удалить все данные.</span><span class="sxs-lookup"><span data-stu-id="489bc-112">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="489bc-113">На следующем рисунке пользователь Рик (`rick@example.com`) вход в систему.</span><span class="sxs-lookup"><span data-stu-id="489bc-113">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="489bc-114">Рик могут только просматривать утвержденные контакты и **изменить**/**удаление**/**создать новый** ссылки для его контактов.</span><span class="sxs-lookup"><span data-stu-id="489bc-114">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="489bc-115">Последней записи, созданные Рик отображает **изменить** и **удалить** ссылки.</span><span class="sxs-lookup"><span data-stu-id="489bc-115">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="489bc-116">Пользователи не смогут просмотреть последнюю запись пока руководитель или администратор меняет состояние на «Утверждено».</span><span class="sxs-lookup"><span data-stu-id="489bc-116">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![изображение описывается предшествующий](secure-data/_static/rick.png)

<span data-ttu-id="489bc-118">На следующем рисунке `manager@contoso.com` входит в и диспетчеры роль:</span><span class="sxs-lookup"><span data-stu-id="489bc-118">In the following image, `manager@contoso.com` is signed in and in the managers role:</span></span>

![изображение описывается предшествующий](secure-data/_static/manager1.png)

<span data-ttu-id="489bc-120">На следующем рисунке показана менеджеров Просмотр подробностей контакта:</span><span class="sxs-lookup"><span data-stu-id="489bc-120">The following image shows the managers details view of a contact:</span></span>

![изображение описывается предшествующий](secure-data/_static/manager.png)

<span data-ttu-id="489bc-122">**Утвердить** и **Отклонить** кнопки отображаются только для менеджеров и администраторов.</span><span class="sxs-lookup"><span data-stu-id="489bc-122">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="489bc-123">На следующем рисунке `admin@contoso.com` входит в и в роли "Администраторы":</span><span class="sxs-lookup"><span data-stu-id="489bc-123">In the following image, `admin@contoso.com` is signed in and in the administrators role:</span></span>

![изображение описывается предшествующий](secure-data/_static/admin.png)

<span data-ttu-id="489bc-125">Администратор имеет все права доступа.</span><span class="sxs-lookup"><span data-stu-id="489bc-125">The administrator has all privileges.</span></span> <span data-ttu-id="489bc-126">Она может чтения, изменить или удалить любой контакт и изменить состояние контактов.</span><span class="sxs-lookup"><span data-stu-id="489bc-126">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="489bc-127">Приложение создано с [формирование шаблонов](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) следующие `Contact` модели:</span><span class="sxs-lookup"><span data-stu-id="489bc-127">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="489bc-128">Пример содержит следующие обработчики авторизации:</span><span class="sxs-lookup"><span data-stu-id="489bc-128">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="489bc-129">`ContactIsOwnerAuthorizationHandler`: Гарантирует, что пользователь может редактировать только свои данные.</span><span class="sxs-lookup"><span data-stu-id="489bc-129">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="489bc-130">`ContactManagerAuthorizationHandler`: Менеджеры утверждать или отклонять контакты.</span><span class="sxs-lookup"><span data-stu-id="489bc-130">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="489bc-131">`ContactAdministratorsAuthorizationHandler`-Позволяет администраторам утвердить или отклонить контактов и изменение/удаление контактов.</span><span class="sxs-lookup"><span data-stu-id="489bc-131">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="489bc-132">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="489bc-132">Prerequisites</span></span>

<span data-ttu-id="489bc-133">Этот учебник является дополнительным.</span><span class="sxs-lookup"><span data-stu-id="489bc-133">This tutorial is advanced.</span></span> <span data-ttu-id="489bc-134">Вы должны быть знакомы с:</span><span class="sxs-lookup"><span data-stu-id="489bc-134">You should be familiar with:</span></span>

* [<span data-ttu-id="489bc-135">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="489bc-135">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="489bc-136">Проверка подлинности</span><span class="sxs-lookup"><span data-stu-id="489bc-136">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="489bc-137">Подтверждение учетной записи и восстановление пароля</span><span class="sxs-lookup"><span data-stu-id="489bc-137">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="489bc-138">Авторизация</span><span class="sxs-lookup"><span data-stu-id="489bc-138">Authorization</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="489bc-139">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="489bc-139">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="489bc-140">В разделе [этот файл PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) для версии ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="489bc-140">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="489bc-141">Версия ASP.NET Core 1.1 этого учебника, [это](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) папки.</span><span class="sxs-lookup"><span data-stu-id="489bc-141">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="489bc-142">1.1, ASP.NET Core образец находится в [образцы](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="489bc-142">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="489bc-143">Начальная и завершенное приложение</span><span class="sxs-lookup"><span data-stu-id="489bc-143">The starter and completed app</span></span>

<span data-ttu-id="489bc-144">[Загрузить](xref:tutorials/index#how-to-download-a-sample) [завершения](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) приложения.</span><span class="sxs-lookup"><span data-stu-id="489bc-144">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app.</span></span> <span data-ttu-id="489bc-145">[Тест](#test-the-completed-app) завершенное приложение, чтобы ознакомиться с функциями безопасности.</span><span class="sxs-lookup"><span data-stu-id="489bc-145">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="489bc-146">Начального уровня приложения</span><span class="sxs-lookup"><span data-stu-id="489bc-146">The starter app</span></span>

<span data-ttu-id="489bc-147">[Загрузить](xref:tutorials/index#how-to-download-a-sample) [начальный](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) приложения.</span><span class="sxs-lookup"><span data-stu-id="489bc-147">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span></span>

<span data-ttu-id="489bc-148">Запуск приложения, коснитесь **ContactManager** ссылку и убедитесь, создание, изменение и удаление контакта.</span><span class="sxs-lookup"><span data-stu-id="489bc-148">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="489bc-149">Защитить данные пользователя</span><span class="sxs-lookup"><span data-stu-id="489bc-149">Secure user data</span></span>

<span data-ttu-id="489bc-150">Следующие разделы имеют основных шагов для создания приложения данных безопасности пользователей.</span><span class="sxs-lookup"><span data-stu-id="489bc-150">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="489bc-151">Может быть полезен для обращения к завершенного проекта.</span><span class="sxs-lookup"><span data-stu-id="489bc-151">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="489bc-152">Связать контактные данные для пользователя</span><span class="sxs-lookup"><span data-stu-id="489bc-152">Tie the contact data to the user</span></span>

<span data-ttu-id="489bc-153">Используйте ASP.NET [удостоверение](xref:security/authentication/identity) идентификатор пользователя, чтобы предоставить пользователям можно изменить свои данные, но не данные других пользователей.</span><span class="sxs-lookup"><span data-stu-id="489bc-153">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="489bc-154">Добавить `OwnerID` и `ContactStatus` для `Contact` модели:</span><span class="sxs-lookup"><span data-stu-id="489bc-154">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="489bc-155">`OwnerID` идентификатор пользователя из `AspNetUser` в таблицу [удостоверение](xref:security/authentication/identity) базы данных.</span><span class="sxs-lookup"><span data-stu-id="489bc-155">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="489bc-156">`Status` Поле определяет, является ли контакт для просмотра обычными пользователями.</span><span class="sxs-lookup"><span data-stu-id="489bc-156">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="489bc-157">Создание нового миграции и обновления базы данных:</span><span class="sxs-lookup"><span data-stu-id="489bc-157">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="require-https-and-authenticated-users"></a><span data-ttu-id="489bc-158">Требовать HTTPS и проверку подлинности пользователей</span><span class="sxs-lookup"><span data-stu-id="489bc-158">Require HTTPS and authenticated users</span></span>

<span data-ttu-id="489bc-159">Добавить [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) для `Startup`:</span><span class="sxs-lookup"><span data-stu-id="489bc-159">Add [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) to `Startup`:</span></span>

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_env)]

<span data-ttu-id="489bc-160">В `ConfigureServices` метод *файла Startup.cs* файл, добавьте [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) фильтр авторизации:</span><span class="sxs-lookup"><span data-stu-id="489bc-160">In the `ConfigureServices` method of the *Startup.cs* file, add the [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) authorization filter:</span></span>

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_SSL&highlight=10-999)]

<span data-ttu-id="489bc-161">Если вы используете Visual Studio, необходимо включите HTTPS.</span><span class="sxs-lookup"><span data-stu-id="489bc-161">If you're using Visual Studio, enable HTTPS.</span></span>

<span data-ttu-id="489bc-162">Для перенаправления HTTP-запросы HTTPS, в разделе [по промежуточного слоя перезаписи URL-адрес](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="489bc-162">To redirect HTTP requests to HTTPS, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="489bc-163">Если с помощью кода Visual Studio или тестирование на локальном платформу, которая не включает тестовый сертификат для использования протокола HTTPS.</span><span class="sxs-lookup"><span data-stu-id="489bc-163">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for HTTPS:</span></span>

  <span data-ttu-id="489bc-164">Задать `"LocalTest:skipSSL": true` в *appsettings. Developement.JSON* файл.</span><span class="sxs-lookup"><span data-stu-id="489bc-164">Set `"LocalTest:skipSSL": true` in the *appsettings.Developement.json* file.</span></span>

### <a name="require-authenticated-users"></a><span data-ttu-id="489bc-165">Требовать проверку подлинности пользователей</span><span class="sxs-lookup"><span data-stu-id="489bc-165">Require authenticated users</span></span>

<span data-ttu-id="489bc-166">Задайте политику проверки подлинности по умолчанию, чтобы требовать проверку подлинности пользователей.</span><span class="sxs-lookup"><span data-stu-id="489bc-166">Set the default authentication policy to require users to be authenticated.</span></span> <span data-ttu-id="489bc-167">Можно отключить проверку подлинности на уровне метода действия, контроллера или страница Razor с `[AllowAnonymous]` атрибута.</span><span class="sxs-lookup"><span data-stu-id="489bc-167">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="489bc-168">Задание политики проверки подлинности по умолчанию, чтобы требовать проверку подлинности пользователей защищает вновь добавленных страниц Razor и контроллеров.</span><span class="sxs-lookup"><span data-stu-id="489bc-168">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="489bc-169">Наличие более безопасна, чем новых контроллеров и страниц Razor для включения проверки подлинности, предусмотренного по умолчанию `[Authorize]` атрибута.</span><span class="sxs-lookup"><span data-stu-id="489bc-169">Having authentication required by default is safer than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span> 

<span data-ttu-id="489bc-170">С требованием проверки подлинности все пользователи [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) и [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage?view=aspnetcore-2.0) вызовы не являются обязательными.</span><span class="sxs-lookup"><span data-stu-id="489bc-170">With the requirement of all users authenticated, the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) and [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage?view=aspnetcore-2.0) calls are not required.</span></span>

<span data-ttu-id="489bc-171">Обновление `ConfigureServices` со следующими изменениями:</span><span class="sxs-lookup"><span data-stu-id="489bc-171">Update `ConfigureServices` with the following changes:</span></span>

* <span data-ttu-id="489bc-172">Закомментируйте `AuthorizeFolder` и `AuthorizePage`.</span><span class="sxs-lookup"><span data-stu-id="489bc-172">Comment out `AuthorizeFolder` and `AuthorizePage`.</span></span>
* <span data-ttu-id="489bc-173">Задайте политику проверки подлинности по умолчанию, чтобы требовать проверку подлинности пользователей.</span><span class="sxs-lookup"><span data-stu-id="489bc-173">Set the default authentication policy to require users to be authenticated.</span></span>

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_defaultPolicy&highlight=23-27,31-999)]

<span data-ttu-id="489bc-174">Добавить [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) индекс страницы об и контактов, анонимные пользователи могут получить сведения о веб-сайте, прежде чем они зарегистрировать.</span><span class="sxs-lookup"><span data-stu-id="489bc-174">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span> 

[!code-csharp[](secure-data/samples/final2/Pages/Index.cshtml.cs?name=snippet&highlight=2)]

<span data-ttu-id="489bc-175">Добавить `[AllowAnonymous]` для [LoginModel и RegisterModel](https://github.com/aspnet/templating/issues/238).</span><span class="sxs-lookup"><span data-stu-id="489bc-175">Add `[AllowAnonymous]` to the [LoginModel and RegisterModel](https://github.com/aspnet/templating/issues/238).</span></span>

### <a name="configure-the-test-account"></a><span data-ttu-id="489bc-176">Настройка тестовой учетной записи</span><span class="sxs-lookup"><span data-stu-id="489bc-176">Configure the test account</span></span>

<span data-ttu-id="489bc-177">`SeedData` Класс создает две учетные записи: администратора и диспетчера.</span><span class="sxs-lookup"><span data-stu-id="489bc-177">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="489bc-178">Используйте [секрет диспетчера](xref:security/app-secrets) , чтобы задать пароль для этих учетных записей.</span><span class="sxs-lookup"><span data-stu-id="489bc-178">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="489bc-179">Задать пароль из каталога проекта (каталог, содержащий *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="489bc-179">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="489bc-180">Обновление `Main` использовать пароль теста:</span><span class="sxs-lookup"><span data-stu-id="489bc-180">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="489bc-181">Создание тестовых учетных записей и обновить контакты</span><span class="sxs-lookup"><span data-stu-id="489bc-181">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="489bc-182">Обновление `Initialize` метод `SeedData` класса, чтобы создать тестовые учетные записи:</span><span class="sxs-lookup"><span data-stu-id="489bc-182">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="489bc-183">Добавьте идентификатор пользователя администратора и `ContactStatus` контактам.</span><span class="sxs-lookup"><span data-stu-id="489bc-183">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="489bc-184">Создать контактов «Отправлено» и одного «отклонено».</span><span class="sxs-lookup"><span data-stu-id="489bc-184">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="489bc-185">Добавьте идентификатор пользователя и состояние для всех контактов.</span><span class="sxs-lookup"><span data-stu-id="489bc-185">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="489bc-186">Показан только один контакт.</span><span class="sxs-lookup"><span data-stu-id="489bc-186">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="489bc-187">Создать владельца, диспетчер и обработчики авторизации администратора</span><span class="sxs-lookup"><span data-stu-id="489bc-187">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="489bc-188">Создание `ContactIsOwnerAuthorizationHandler` класса в *авторизации* папки.</span><span class="sxs-lookup"><span data-stu-id="489bc-188">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="489bc-189">`ContactIsOwnerAuthorizationHandler` Проверяет, что пользователь, действующий от ресурса, которому принадлежит ресурс.</span><span class="sxs-lookup"><span data-stu-id="489bc-189">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="489bc-190">`ContactIsOwnerAuthorizationHandler` Вызовы [контекста. Успешно](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) Если контактные владельцем текущего пользователя, прошедшего проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="489bc-190">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="489bc-191">Обработчики авторизации обычно:</span><span class="sxs-lookup"><span data-stu-id="489bc-191">Authorization handlers generally:</span></span>

* <span data-ttu-id="489bc-192">Возвращает `context.Succeed` при выполнение требований.</span><span class="sxs-lookup"><span data-stu-id="489bc-192">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="489bc-193">Возвращает `Task.CompletedTask` Если требования не выполнены.</span><span class="sxs-lookup"><span data-stu-id="489bc-193">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="489bc-194">`Task.CompletedTask` — ни об успешном или неуспешном&mdash;она позволяет другим обработчикам авторизации для выполнения.</span><span class="sxs-lookup"><span data-stu-id="489bc-194">`Task.CompletedTask` is neither success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="489bc-195">Если необходимо выполнить явную ошибку, возвратить [контекста. Сбой](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="489bc-195">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="489bc-196">Данное приложение позволяет контакта владельцев, изменение или удаление и создание собственных данных.</span><span class="sxs-lookup"><span data-stu-id="489bc-196">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="489bc-197">`ContactIsOwnerAuthorizationHandler` не требуется для операции, передаваемые в качестве параметра требование проверки.</span><span class="sxs-lookup"><span data-stu-id="489bc-197">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="489bc-198">Создание обработчика диспетчера авторизации</span><span class="sxs-lookup"><span data-stu-id="489bc-198">Create a manager authorization handler</span></span>

<span data-ttu-id="489bc-199">Создание `ContactManagerAuthorizationHandler` класса в *авторизации* папки.</span><span class="sxs-lookup"><span data-stu-id="489bc-199">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="489bc-200">`ContactManagerAuthorizationHandler` Проверяет пользователя, действующий от ресурса является менеджером.</span><span class="sxs-lookup"><span data-stu-id="489bc-200">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="489bc-201">Данные только менеджеров можно утвердить или отклонить изменения содержимого (новые или измененные).</span><span class="sxs-lookup"><span data-stu-id="489bc-201">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="489bc-202">Создайте обработчик авторизации администратора</span><span class="sxs-lookup"><span data-stu-id="489bc-202">Create an administrator authorization handler</span></span>

<span data-ttu-id="489bc-203">Создание `ContactAdministratorsAuthorizationHandler` класса в *авторизации* папки.</span><span class="sxs-lookup"><span data-stu-id="489bc-203">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="489bc-204">`ContactAdministratorsAuthorizationHandler` Проверяет, действует для ресурса пользователь является администратором.</span><span class="sxs-lookup"><span data-stu-id="489bc-204">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="489bc-205">Администратор может выполнять все операции.</span><span class="sxs-lookup"><span data-stu-id="489bc-205">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="489bc-206">Регистрировать обработчики авторизации</span><span class="sxs-lookup"><span data-stu-id="489bc-206">Register the authorization handlers</span></span>

<span data-ttu-id="489bc-207">С помощью Entity Framework Core Services должны быть зарегистрированы для [внедрения зависимостей](xref:fundamentals/dependency-injection) с помощью [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="489bc-207">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="489bc-208">`ContactIsOwnerAuthorizationHandler` Использует ASP.NET Core [удостоверения](xref:security/authentication/identity), которые построены на Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="489bc-208">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="489bc-209">Зарегистрировать обработчики с коллекцией служб, они будут доступны для `ContactsController` через [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="489bc-209">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="489bc-210">Добавьте следующий код в конец `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="489bc-210">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=ConfigureServices&highlight=41-999)]

<span data-ttu-id="489bc-211">`ContactAdministratorsAuthorizationHandler` и `ContactManagerAuthorizationHandler` добавляются как одноэлементных кортежей.</span><span class="sxs-lookup"><span data-stu-id="489bc-211">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="489bc-212">Они единственных экземпляров, так как они не используют EF и все сведения, необходимые возможности `Context` параметр `HandleRequirementAsync` метода.</span><span class="sxs-lookup"><span data-stu-id="489bc-212">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="489bc-213">Поддержка авторизации</span><span class="sxs-lookup"><span data-stu-id="489bc-213">Support authorization</span></span>

<span data-ttu-id="489bc-214">В этом разделе обновления страниц Razor и добавления класса требований операции.</span><span class="sxs-lookup"><span data-stu-id="489bc-214">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="489bc-215">Просмотрите требования класс контакта operations</span><span class="sxs-lookup"><span data-stu-id="489bc-215">Review the contact operations requirements class</span></span>

<span data-ttu-id="489bc-216">Просмотрите `ContactOperations` класса.</span><span class="sxs-lookup"><span data-stu-id="489bc-216">Review the `ContactOperations` class.</span></span> <span data-ttu-id="489bc-217">Этот класс содержит требования поддерживает приложения:</span><span class="sxs-lookup"><span data-stu-id="489bc-217">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-razor-pages"></a><span data-ttu-id="489bc-218">Создать базовый класс для страниц Razor</span><span class="sxs-lookup"><span data-stu-id="489bc-218">Create a base class for the Razor Pages</span></span>

<span data-ttu-id="489bc-219">Создайте базовый класс, который содержит службы, используемые в контактах страниц Razor.</span><span class="sxs-lookup"><span data-stu-id="489bc-219">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="489bc-220">Базовый класс используется этот код инициализации в одном месте:</span><span class="sxs-lookup"><span data-stu-id="489bc-220">The base class puts that initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="489bc-221">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="489bc-221">The preceding code:</span></span>

* <span data-ttu-id="489bc-222">Добавляет `IAuthorizationService` для доступа к обработчикам авторизации.</span><span class="sxs-lookup"><span data-stu-id="489bc-222">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="489bc-223">Добавляет идентификатор `UserManager` службы.</span><span class="sxs-lookup"><span data-stu-id="489bc-223">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="489bc-224">Добавьте `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="489bc-224">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="489bc-225">Обновление CreateModel</span><span class="sxs-lookup"><span data-stu-id="489bc-225">Update the CreateModel</span></span>

<span data-ttu-id="489bc-226">Измените конструктор модели создать страницу для использования `DI_BasePageModel` базового класса:</span><span class="sxs-lookup"><span data-stu-id="489bc-226">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="489bc-227">Обновление `CreateModel.OnPostAsync` метода:</span><span class="sxs-lookup"><span data-stu-id="489bc-227">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="489bc-228">Добавьте идентификатор пользователя, который `Contact` модели.</span><span class="sxs-lookup"><span data-stu-id="489bc-228">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="489bc-229">Вызывает обработчик авторизации, чтобы убедиться, что пользователь имеет разрешение на создание контактов.</span><span class="sxs-lookup"><span data-stu-id="489bc-229">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="489bc-230">Обновление IndexModel</span><span class="sxs-lookup"><span data-stu-id="489bc-230">Update the IndexModel</span></span>

<span data-ttu-id="489bc-231">Обновление `OnGetAsync` метод только утвержденные контакты отображаются обычным пользователям:</span><span class="sxs-lookup"><span data-stu-id="489bc-231">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="489bc-232">Обновление EditModel</span><span class="sxs-lookup"><span data-stu-id="489bc-232">Update the EditModel</span></span>

<span data-ttu-id="489bc-233">Добавьте обработчик авторизации, чтобы убедиться, что пользователь является владельцем контакта.</span><span class="sxs-lookup"><span data-stu-id="489bc-233">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="489bc-234">Поскольку проверки авторизации ресурсов `[Authorize]` атрибута недостаточно.</span><span class="sxs-lookup"><span data-stu-id="489bc-234">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="489bc-235">Приложение не имеет доступа к ресурсу, при оценке атрибутов.</span><span class="sxs-lookup"><span data-stu-id="489bc-235">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="489bc-236">Авторизация на основе ресурсов должно быть принудительной.</span><span class="sxs-lookup"><span data-stu-id="489bc-236">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="489bc-237">После приложение имеет доступ к ресурсу, загрузив его в модель страницы или, загрузив его в сам обработчик должен выполняться проверки.</span><span class="sxs-lookup"><span data-stu-id="489bc-237">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="489bc-238">Частом обращении к ресурсу, передавая ключ ресурса.</span><span class="sxs-lookup"><span data-stu-id="489bc-238">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="489bc-239">Обновление DeleteModel</span><span class="sxs-lookup"><span data-stu-id="489bc-239">Update the DeleteModel</span></span>

<span data-ttu-id="489bc-240">Обновление модели страницы delete на использование обработчика авторизации, чтобы убедиться, что пользователь имеет разрешение на удаление контакта.</span><span class="sxs-lookup"><span data-stu-id="489bc-240">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="489bc-241">Внедрить службы авторизации в представления</span><span class="sxs-lookup"><span data-stu-id="489bc-241">Inject the authorization service into the views</span></span>

<span data-ttu-id="489bc-242">В настоящее время показано пользовательского интерфейса, изменение и удаление ссылки на данные, которые не может быть изменено пользователем.</span><span class="sxs-lookup"><span data-stu-id="489bc-242">Currently, the UI shows edit and delete links for data the user can't modify.</span></span> <span data-ttu-id="489bc-243">Пользовательский Интерфейс фиксируется путем применения авторизации обработчика представлений.</span><span class="sxs-lookup"><span data-stu-id="489bc-243">The UI is fixed by applying the authorization handler to the views.</span></span>

<span data-ttu-id="489bc-244">Запустить службу авторизации в *Views/_ViewImports.cshtml* файл, чтобы сделать ее доступной для всех представлений:</span><span class="sxs-lookup"><span data-stu-id="489bc-244">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2/Pages/_ViewImports.cshtml?highlight=6-9)]

<span data-ttu-id="489bc-245">Добавляет предыдущей разметки некоторые `using` инструкции.</span><span class="sxs-lookup"><span data-stu-id="489bc-245">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="489bc-246">Обновление **изменить** и **удалить** связывает *Pages/Contacts/Index.cshtml* , только отображаются для пользователей с соответствующими разрешениями:</span><span class="sxs-lookup"><span data-stu-id="489bc-246">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2/Pages/Contacts/Index.cshtml?highlight=34-36,64-999)]

> [!WARNING]
> <span data-ttu-id="489bc-247">Скрытие ссылок от пользователей, у которых нет разрешения на изменение данных не защищать приложения.</span><span class="sxs-lookup"><span data-stu-id="489bc-247">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="489bc-248">Скрытие ссылки позволяет сделать приложение более удобной для пользователей, отображая только допустимые ссылки.</span><span class="sxs-lookup"><span data-stu-id="489bc-248">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="489bc-249">Пользователи могут hack созданные URL-адреса, вызывать изменение и удаление операций с данными, которыми они не владеют.</span><span class="sxs-lookup"><span data-stu-id="489bc-249">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="489bc-250">Страница Razor или контроллер должен Принудительная проверка доступа для защиты данных.</span><span class="sxs-lookup"><span data-stu-id="489bc-250">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="489bc-251">Дополнительные сведения об обновлении</span><span class="sxs-lookup"><span data-stu-id="489bc-251">Update Details</span></span>

<span data-ttu-id="489bc-252">Обновление представления сведений, чтобы менеджеры могли утверждать или отклонять контакты:</span><span class="sxs-lookup"><span data-stu-id="489bc-252">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2/Pages/Contacts/Details.cshtml?range=48-999)]

<span data-ttu-id="489bc-253">Обновите модель страницы сведений:</span><span class="sxs-lookup"><span data-stu-id="489bc-253">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="test-the-completed-app"></a><span data-ttu-id="489bc-254">Тестирование завершенного приложения</span><span class="sxs-lookup"><span data-stu-id="489bc-254">Test the completed app</span></span>

<span data-ttu-id="489bc-255">Если с помощью кода Visual Studio или тестирование на локальном платформу, которая не включает тестовый сертификат для использования протокола HTTPS.</span><span class="sxs-lookup"><span data-stu-id="489bc-255">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for HTTPS:</span></span>

* <span data-ttu-id="489bc-256">Задать `"LocalTest:skipSSL": true` в *appsettings. Developement.JSON* файл, чтобы пропустить обязательное использование протокола HTTPS.</span><span class="sxs-lookup"><span data-stu-id="489bc-256">Set `"LocalTest:skipSSL": true` in the *appsettings.Developement.json* file to skip the HTTPS requirement.</span></span> <span data-ttu-id="489bc-257">Пропустить HTTPS только на компьютере разработки.</span><span class="sxs-lookup"><span data-stu-id="489bc-257">Skip HTTPS only on a development machine.</span></span>

<span data-ttu-id="489bc-258">Если приложение имеет контактов:</span><span class="sxs-lookup"><span data-stu-id="489bc-258">If the app has contacts:</span></span>

* <span data-ttu-id="489bc-259">Удалить все записи из `Contact` таблицы.</span><span class="sxs-lookup"><span data-stu-id="489bc-259">Delete all the records in the `Contact` table.</span></span>
* <span data-ttu-id="489bc-260">Перезапустите приложение для инициализации базы данных.</span><span class="sxs-lookup"><span data-stu-id="489bc-260">Restart the app to seed the database.</span></span>

<span data-ttu-id="489bc-261">Для просмотра контактов регистрации пользователя.</span><span class="sxs-lookup"><span data-stu-id="489bc-261">Register a user for browsing the contacts.</span></span>

<span data-ttu-id="489bc-262">Простой способ тестирования завершенное приложение — запуск три разных браузерах (или версий браузера или InPrivate).</span><span class="sxs-lookup"><span data-stu-id="489bc-262">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="489bc-263">В одном браузере регистрации нового пользователя (например, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="489bc-263">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="489bc-264">Вход для каждого веб-обозревателя с другим пользователем.</span><span class="sxs-lookup"><span data-stu-id="489bc-264">Sign in to each browser with a different user.</span></span> <span data-ttu-id="489bc-265">Проверьте следующие операции:</span><span class="sxs-lookup"><span data-stu-id="489bc-265">Verify the following operations:</span></span>

* <span data-ttu-id="489bc-266">Зарегистрированные пользователи могут просматривать утвержденные контактных данных.</span><span class="sxs-lookup"><span data-stu-id="489bc-266">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="489bc-267">Зарегистрированные пользователи могут изменить или удалить свои собственные данные.</span><span class="sxs-lookup"><span data-stu-id="489bc-267">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="489bc-268">Менеджеры могли утверждать или отклонять контактные данные.</span><span class="sxs-lookup"><span data-stu-id="489bc-268">Managers can approve or reject contact data.</span></span> <span data-ttu-id="489bc-269">`Details` Представлении отображаются **утвердить** и **Отклонить** кнопки.</span><span class="sxs-lookup"><span data-stu-id="489bc-269">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="489bc-270">Администраторы могут утвердить или отклонить и изменить или удалить все данные.</span><span class="sxs-lookup"><span data-stu-id="489bc-270">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="489bc-271">Пользовательская</span><span class="sxs-lookup"><span data-stu-id="489bc-271">User</span></span>| <span data-ttu-id="489bc-272">Параметры</span><span class="sxs-lookup"><span data-stu-id="489bc-272">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="489bc-273">Можно изменить или удалить собственных данных</span><span class="sxs-lookup"><span data-stu-id="489bc-273">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="489bc-274">Можно утвердить или отклонить и изменить или удалить принадлежат данные</span><span class="sxs-lookup"><span data-stu-id="489bc-274">Can approve/reject and edit/delete own data</span></span> |
| admin@contoso.com | <span data-ttu-id="489bc-275">Можно изменить или удалить и утвердить или отклонить все данные</span><span class="sxs-lookup"><span data-stu-id="489bc-275">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="489bc-276">Создание контакта в браузере администратора.</span><span class="sxs-lookup"><span data-stu-id="489bc-276">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="489bc-277">Скопируйте URL-адрес для удаления и изменения из обратитесь к администратору.</span><span class="sxs-lookup"><span data-stu-id="489bc-277">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="489bc-278">Вставьте эти ссылки в браузер тестового пользователя, чтобы убедиться, что тестовый пользователь не может выполнять эти операции.</span><span class="sxs-lookup"><span data-stu-id="489bc-278">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="489bc-279">Создание начального уровня приложения</span><span class="sxs-lookup"><span data-stu-id="489bc-279">Create the starter app</span></span>

* <span data-ttu-id="489bc-280">Создание страниц Razor приложения с именем «ContactManager»</span><span class="sxs-lookup"><span data-stu-id="489bc-280">Create a Razor Pages app named "ContactManager"</span></span>

  * <span data-ttu-id="489bc-281">Создание приложения с **отдельных учетных записей пользователей**.</span><span class="sxs-lookup"><span data-stu-id="489bc-281">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="489bc-282">Присвойте ему имя «ContactManager», пространства имен соответствует пространству имен, используемые в образце.</span><span class="sxs-lookup"><span data-stu-id="489bc-282">Name it "ContactManager" so your namespace matches the namespace used in the sample.</span></span>

  ```console
  dotnet new razor -o ContactManager -au Individual -uld
  ```

  * <span data-ttu-id="489bc-283">`-uld` Указывает LocalDB вместо SQLite</span><span class="sxs-lookup"><span data-stu-id="489bc-283">`-uld` specifies LocalDB instead of SQLite</span></span>

* <span data-ttu-id="489bc-284">Добавьте следующие `Contact` модели:</span><span class="sxs-lookup"><span data-stu-id="489bc-284">Add the following `Contact` model:</span></span>

  [!code-csharp[](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="489bc-285">Представление формирования `Contact` модели:</span><span class="sxs-lookup"><span data-stu-id="489bc-285">Scaffold the `Contact` model:</span></span>

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
```

* <span data-ttu-id="489bc-286">Обновление **ContactManager** привязки в *Pages/_Layout.cshtml* файла:</span><span class="sxs-lookup"><span data-stu-id="489bc-286">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* <span data-ttu-id="489bc-287">Формировать первоначальной миграции и обновления базы данных:</span><span class="sxs-lookup"><span data-stu-id="489bc-287">Scaffold the initial migration and update the database:</span></span>

```console
dotnet ef migrations add initial
dotnet ef database update
```

* <span data-ttu-id="489bc-288">Протестируйте приложение, создание, изменение и удаление контакта</span><span class="sxs-lookup"><span data-stu-id="489bc-288">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="489bc-289">Заполнение базы данных</span><span class="sxs-lookup"><span data-stu-id="489bc-289">Seed the database</span></span>

<span data-ttu-id="489bc-290">Добавить `SeedData` класса *данные* папки.</span><span class="sxs-lookup"><span data-stu-id="489bc-290">Add the `SeedData` class to the *Data* folder.</span></span> <span data-ttu-id="489bc-291">Если загруженный образца можно скопировать *SeedData.cs* файл *данные* папки начальный проект.</span><span class="sxs-lookup"><span data-stu-id="489bc-291">If you've downloaded the sample, you can copy the *SeedData.cs* file to the *Data* folder of the starter project.</span></span>

<span data-ttu-id="489bc-292">Вызовите `SeedData.Initialize` из `Main`:</span><span class="sxs-lookup"><span data-stu-id="489bc-292">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2/Program.cs?name=snippet)]

<span data-ttu-id="489bc-293">Тест заполняется, приложение базы данных.</span><span class="sxs-lookup"><span data-stu-id="489bc-293">Test that the app seeded the database.</span></span> <span data-ttu-id="489bc-294">Если все строки в контакте DB, метод инициализации не запускается.</span><span class="sxs-lookup"><span data-stu-id="489bc-294">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="489bc-295">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="489bc-295">Additional resources</span></span>

* <span data-ttu-id="489bc-296">[Лаборатории авторизации ASP.NET Core](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="489bc-296">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="489bc-297">Эта лаборатория приведены более подробные сведения о средствах безопасности, представленные в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="489bc-297">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="489bc-298">Авторизация в ASP.NET Core: простой, роли, на основе утверждений, а также пользовательские</span><span class="sxs-lookup"><span data-stu-id="489bc-298">Authorization in ASP.NET Core: Simple, role, claims-based, and custom</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="489bc-299">Пользовательская авторизация на основе политик</span><span class="sxs-lookup"><span data-stu-id="489bc-299">Custom policy-based authorization</span></span>](xref:security/authorization/policies)
