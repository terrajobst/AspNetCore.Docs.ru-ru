---
title: "Создание приложения ASP.NET Core пользовательскими данными, защищенных авторизации"
author: rick-anderson
description: "Узнайте, как для создания приложения страниц Razor с данными пользователя, защищен авторизации. Включает SSL, проверки подлинности, безопасность ASP.NET Core Identity."
manager: wpickett
ms.author: riande
ms.date: 01/24/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/secure-data
ms.openlocfilehash: 6333082a2b2b4f6d3f1ce2afc600b4203a0f5dca
ms.sourcegitcommit: 7a87d66cf1d01febe6635c7306f2f679434901d1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/03/2018
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="37334-104">Создание приложения ASP.NET Core пользовательскими данными, защищенных авторизации</span><span class="sxs-lookup"><span data-stu-id="37334-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="37334-105">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Джо Одетт](https://twitter.com/joeaudette) (Joe Audette)</span><span class="sxs-lookup"><span data-stu-id="37334-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="37334-106">Этого учебника показано, как создать веб-приложение ASP.NET Core пользовательскими данными, защищенных авторизации.</span><span class="sxs-lookup"><span data-stu-id="37334-106">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="37334-107">Отображается список контактов, прошедшие проверку подлинности (зарегистрированные) пользователи создали.</span><span class="sxs-lookup"><span data-stu-id="37334-107">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="37334-108">Существует три группы безопасности:</span><span class="sxs-lookup"><span data-stu-id="37334-108">There are three security groups:</span></span>

* <span data-ttu-id="37334-109">**Зарегистрированные пользователи** можно просмотреть все утвержденные данные и можно изменить или удалить свои собственные данные.</span><span class="sxs-lookup"><span data-stu-id="37334-109">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="37334-110">**Диспетчеры** можно утвердить или отклонить контактных данных.</span><span class="sxs-lookup"><span data-stu-id="37334-110">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="37334-111">Только утвержденные контакты отображаются для пользователей.</span><span class="sxs-lookup"><span data-stu-id="37334-111">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="37334-112">**Администраторы** можно утвердить или отклонить и изменить или удалить все данные.</span><span class="sxs-lookup"><span data-stu-id="37334-112">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="37334-113">На следующем рисунке пользователь Рик (`rick@example.com`) вход в систему.</span><span class="sxs-lookup"><span data-stu-id="37334-113">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="37334-114">Рик могут только просматривать утвержденные контакты и **изменить**/**удаление**/**создать новый** ссылки для его контактов.</span><span class="sxs-lookup"><span data-stu-id="37334-114">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="37334-115">Последней записи, созданные Рик отображает **изменить** и **удалить** ссылки.</span><span class="sxs-lookup"><span data-stu-id="37334-115">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="37334-116">Пользователи не смогут просмотреть последнюю запись пока руководитель или администратор меняет состояние на «Утверждено».</span><span class="sxs-lookup"><span data-stu-id="37334-116">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![изображение описывается предшествующий](secure-data/_static/rick.png)

<span data-ttu-id="37334-118">На следующем рисунке `manager@contoso.com` входит в и диспетчеры роль:</span><span class="sxs-lookup"><span data-stu-id="37334-118">In the following image, `manager@contoso.com` is signed in and in the managers role:</span></span>

![изображение описывается предшествующий](secure-data/_static/manager1.png)

<span data-ttu-id="37334-120">На следующем рисунке показана менеджеров Просмотр подробностей контакта:</span><span class="sxs-lookup"><span data-stu-id="37334-120">The following image shows the managers details view of a contact:</span></span>

![изображение описывается предшествующий](secure-data/_static/manager.png)

<span data-ttu-id="37334-122">**Утвердить** и **Отклонить** кнопки отображаются только для менеджеров и администраторов.</span><span class="sxs-lookup"><span data-stu-id="37334-122">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="37334-123">На следующем рисунке `admin@contoso.com` входит в и в роли "Администраторы":</span><span class="sxs-lookup"><span data-stu-id="37334-123">In the following image, `admin@contoso.com` is signed in and in the administrators role:</span></span>

![изображение описывается предшествующий](secure-data/_static/admin.png)

<span data-ttu-id="37334-125">Администратор имеет все права доступа.</span><span class="sxs-lookup"><span data-stu-id="37334-125">The administrator has all privileges.</span></span> <span data-ttu-id="37334-126">Она может чтения, изменить или удалить любой контакт и изменить состояние контактов.</span><span class="sxs-lookup"><span data-stu-id="37334-126">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="37334-127">Приложение создано с [формирование шаблонов](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) следующие `Contact` модели:</span><span class="sxs-lookup"><span data-stu-id="37334-127">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) the following `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="37334-128">Пример содержит следующие обработчики авторизации:</span><span class="sxs-lookup"><span data-stu-id="37334-128">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="37334-129">`ContactIsOwnerAuthorizationHandler`: Гарантирует, что пользователь может редактировать только свои данные.</span><span class="sxs-lookup"><span data-stu-id="37334-129">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="37334-130">`ContactManagerAuthorizationHandler`: Менеджеры утверждать или отклонять контакты.</span><span class="sxs-lookup"><span data-stu-id="37334-130">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="37334-131">`ContactAdministratorsAuthorizationHandler`-Позволяет администраторам утвердить или отклонить контактов и изменение/удаление контактов.</span><span class="sxs-lookup"><span data-stu-id="37334-131">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="37334-132">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="37334-132">Prerequisites</span></span>

<span data-ttu-id="37334-133">Этот учебник является дополнительным.</span><span class="sxs-lookup"><span data-stu-id="37334-133">This tutorial is advanced.</span></span> <span data-ttu-id="37334-134">Вы должны быть знакомы с:</span><span class="sxs-lookup"><span data-stu-id="37334-134">You should be familiar with:</span></span>

* [<span data-ttu-id="37334-135">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="37334-135">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="37334-136">Проверка подлинности</span><span class="sxs-lookup"><span data-stu-id="37334-136">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="37334-137">Подтверждение учетной записи и восстановление пароля</span><span class="sxs-lookup"><span data-stu-id="37334-137">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="37334-138">Авторизация</span><span class="sxs-lookup"><span data-stu-id="37334-138">Authorization</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="37334-139">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="37334-139">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="37334-140">В разделе [этот файл PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) для версии ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="37334-140">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="37334-141">Версия ASP.NET Core 1.1 этого учебника, [это](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) папки.</span><span class="sxs-lookup"><span data-stu-id="37334-141">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="37334-142">1.1, ASP.NET Core образец находится в [образцы](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="37334-142">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="37334-143">Начальная и завершенное приложение</span><span class="sxs-lookup"><span data-stu-id="37334-143">The starter and completed app</span></span>

<span data-ttu-id="37334-144">[Загрузить](xref:tutorials/index#how-to-download-a-sample) [завершения](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) приложения.</span><span class="sxs-lookup"><span data-stu-id="37334-144">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app.</span></span> <span data-ttu-id="37334-145">[Тест](#test-the-completed-app) завершенное приложение, чтобы ознакомиться с функциями безопасности.</span><span class="sxs-lookup"><span data-stu-id="37334-145">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="37334-146">Начального уровня приложения</span><span class="sxs-lookup"><span data-stu-id="37334-146">The starter app</span></span>

<span data-ttu-id="37334-147">[Загрузить](xref:tutorials/index#how-to-download-a-sample) [начальный](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) приложения.</span><span class="sxs-lookup"><span data-stu-id="37334-147">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span></span>

<span data-ttu-id="37334-148">Запуск приложения, коснитесь **ContactManager** ссылку и убедитесь, создание, изменение и удаление контакта.</span><span class="sxs-lookup"><span data-stu-id="37334-148">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="37334-149">Защитить данные пользователя</span><span class="sxs-lookup"><span data-stu-id="37334-149">Secure user data</span></span>

<span data-ttu-id="37334-150">Следующие разделы имеют основных шагов для создания приложения данных безопасности пользователей.</span><span class="sxs-lookup"><span data-stu-id="37334-150">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="37334-151">Может быть полезен для обращения к завершенного проекта.</span><span class="sxs-lookup"><span data-stu-id="37334-151">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="37334-152">Связать контактные данные для пользователя</span><span class="sxs-lookup"><span data-stu-id="37334-152">Tie the contact data to the user</span></span>

<span data-ttu-id="37334-153">Используйте ASP.NET [удостоверение](xref:security/authentication/identity) идентификатор пользователя, чтобы предоставить пользователям можно изменить свои данные, но не данные других пользователей.</span><span class="sxs-lookup"><span data-stu-id="37334-153">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="37334-154">Добавить `OwnerID` и `ContactStatus` для `Contact` модели:</span><span class="sxs-lookup"><span data-stu-id="37334-154">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="37334-155">`OwnerID`идентификатор пользователя из `AspNetUser` в таблицу [удостоверение](xref:security/authentication/identity) базы данных.</span><span class="sxs-lookup"><span data-stu-id="37334-155">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="37334-156">`Status` Поле определяет, является ли контакт для просмотра обычными пользователями.</span><span class="sxs-lookup"><span data-stu-id="37334-156">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="37334-157">Создание нового миграции и обновления базы данных:</span><span class="sxs-lookup"><span data-stu-id="37334-157">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="require-ssl-and-authenticated-users"></a><span data-ttu-id="37334-158">Требовать SSL и проверку подлинности пользователей</span><span class="sxs-lookup"><span data-stu-id="37334-158">Require SSL and authenticated users</span></span>

<span data-ttu-id="37334-159">Добавить [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) для `Startup`:</span><span class="sxs-lookup"><span data-stu-id="37334-159">Add [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) to `Startup`:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_env)]

<span data-ttu-id="37334-160">В `ConfigureServices` метод *файла Startup.cs* файл, добавьте [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) фильтр авторизации:</span><span class="sxs-lookup"><span data-stu-id="37334-160">In the `ConfigureServices` method of the *Startup.cs* file, add the [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) authorization filter:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_SSL&highlight=19-999)]

<span data-ttu-id="37334-161">Если вы используете Visual Studio, протокол SSL включен.</span><span class="sxs-lookup"><span data-stu-id="37334-161">If you're using Visual Studio, enable SSL.</span></span>

<span data-ttu-id="37334-162">Для перенаправления HTTP-запросы HTTPS, в разделе [по промежуточного слоя перезаписи URL-адрес](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="37334-162">To redirect HTTP requests to HTTPS, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="37334-163">Если с помощью кода Visual Studio или тестирование на локальном платформу, которая не включает тестового сертификата для SSL.</span><span class="sxs-lookup"><span data-stu-id="37334-163">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for SSL:</span></span>

  <span data-ttu-id="37334-164">Задать `"LocalTest:skipSSL": true` в *appsettings. Developement.JSON* файл.</span><span class="sxs-lookup"><span data-stu-id="37334-164">Set `"LocalTest:skipSSL": true` in the *appsettings.Developement.json* file.</span></span>

### <a name="require-authenticated-users"></a><span data-ttu-id="37334-165">Требовать проверку подлинности пользователей</span><span class="sxs-lookup"><span data-stu-id="37334-165">Require authenticated users</span></span>

<span data-ttu-id="37334-166">Задайте политику проверки подлинности по умолчанию, чтобы требовать проверку подлинности пользователей.</span><span class="sxs-lookup"><span data-stu-id="37334-166">Set the default authentication policy to require users to be authenticated.</span></span> <span data-ttu-id="37334-167">Можно отключить проверку подлинности на уровне метода действия, контроллера или страница Razor с `[AllowAnonymous]` атрибута.</span><span class="sxs-lookup"><span data-stu-id="37334-167">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="37334-168">Задание политики проверки подлинности по умолчанию, чтобы требовать проверку подлинности пользователей защищает вновь добавленных страниц Razor и контроллеров.</span><span class="sxs-lookup"><span data-stu-id="37334-168">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="37334-169">Наличие более безопасна, чем новых контроллеров и страниц Razor для включения проверки подлинности, предусмотренного по умолчанию `[Authorize]` атрибута.</span><span class="sxs-lookup"><span data-stu-id="37334-169">Having authentication required by default is safer than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span> <span data-ttu-id="37334-170">Добавьте следующий код в `ConfigureServices` метод *файла Startup.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="37334-170">Add the following to the `ConfigureServices` method of the *Startup.cs* file:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_defaultPolicy&highlight=31-999)]

<span data-ttu-id="37334-171">Добавить [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) индекс страницы об и контактов, анонимные пользователи могут получить сведения о веб-сайте, прежде чем они зарегистрировать.</span><span class="sxs-lookup"><span data-stu-id="37334-171">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span> 

[!code-csharp[Main](secure-data/samples/final2/Pages/Index.cshtml.cs?name=snippet&highlight=2)]

<span data-ttu-id="37334-172">Добавить `[AllowAnonymous]` для [LoginModel и RegisterModel](https://github.com/aspnet/templating/issues/238).</span><span class="sxs-lookup"><span data-stu-id="37334-172">Add `[AllowAnonymous]` to the [LoginModel and RegisterModel](https://github.com/aspnet/templating/issues/238).</span></span>

### <a name="configure-the-test-account"></a><span data-ttu-id="37334-173">Настройка тестовой учетной записи</span><span class="sxs-lookup"><span data-stu-id="37334-173">Configure the test account</span></span>

<span data-ttu-id="37334-174">`SeedData` Класс создает две учетные записи: администратора и диспетчера.</span><span class="sxs-lookup"><span data-stu-id="37334-174">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="37334-175">Используйте [секрет диспетчера](xref:security/app-secrets) , чтобы задать пароль для этих учетных записей.</span><span class="sxs-lookup"><span data-stu-id="37334-175">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="37334-176">Задать пароль из каталога проекта (каталог, содержащий *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="37334-176">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="37334-177">Обновление `Main` использовать пароль теста:</span><span class="sxs-lookup"><span data-stu-id="37334-177">Update `Main` to use the test password:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="37334-178">Создание тестовых учетных записей и обновить контакты</span><span class="sxs-lookup"><span data-stu-id="37334-178">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="37334-179">Обновление `Initialize` метод `SeedData` класса, чтобы создать тестовые учетные записи:</span><span class="sxs-lookup"><span data-stu-id="37334-179">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="37334-180">Добавьте идентификатор пользователя администратора и `ContactStatus` контактам.</span><span class="sxs-lookup"><span data-stu-id="37334-180">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="37334-181">Создать контактов «Отправлено» и одного «отклонено».</span><span class="sxs-lookup"><span data-stu-id="37334-181">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="37334-182">Добавьте идентификатор пользователя и состояние для всех контактов.</span><span class="sxs-lookup"><span data-stu-id="37334-182">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="37334-183">Показан только один контакт.</span><span class="sxs-lookup"><span data-stu-id="37334-183">Only one contact is shown:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="37334-184">Создать владельца, диспетчер и обработчики авторизации администратора</span><span class="sxs-lookup"><span data-stu-id="37334-184">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="37334-185">Создание `ContactIsOwnerAuthorizationHandler` класса в *авторизации* папки.</span><span class="sxs-lookup"><span data-stu-id="37334-185">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="37334-186">`ContactIsOwnerAuthorizationHandler` Проверяет, что пользователь, действующий от ресурса, которому принадлежит ресурс.</span><span class="sxs-lookup"><span data-stu-id="37334-186">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="37334-187">`ContactIsOwnerAuthorizationHandler` Вызовы [контекста. Успешно](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) Если контактные владельцем текущего пользователя, прошедшего проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="37334-187">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="37334-188">Обработчики авторизации обычно:</span><span class="sxs-lookup"><span data-stu-id="37334-188">Authorization handlers generally:</span></span>

* <span data-ttu-id="37334-189">Возвращает `context.Succeed` при выполнение требований.</span><span class="sxs-lookup"><span data-stu-id="37334-189">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="37334-190">Возвращает `Task.CompletedTask` Если требования не выполнены.</span><span class="sxs-lookup"><span data-stu-id="37334-190">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="37334-191">`Task.CompletedTask`— ни об успешном или неуспешном&mdash;она позволяет другим обработчикам авторизации для выполнения.</span><span class="sxs-lookup"><span data-stu-id="37334-191">`Task.CompletedTask` is neither success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="37334-192">Если необходимо выполнить явную ошибку, возвратить [контекста. Сбой](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="37334-192">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="37334-193">Данное приложение позволяет контакта владельцев, изменение или удаление и создание собственных данных.</span><span class="sxs-lookup"><span data-stu-id="37334-193">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="37334-194">`ContactIsOwnerAuthorizationHandler`не требуется для операции, передаваемые в качестве параметра требование проверки.</span><span class="sxs-lookup"><span data-stu-id="37334-194">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="37334-195">Создание обработчика диспетчера авторизации</span><span class="sxs-lookup"><span data-stu-id="37334-195">Create a manager authorization handler</span></span>

<span data-ttu-id="37334-196">Создание `ContactManagerAuthorizationHandler` класса в *авторизации* папки.</span><span class="sxs-lookup"><span data-stu-id="37334-196">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="37334-197">`ContactManagerAuthorizationHandler` Проверяет пользователя, действующий от ресурса является менеджером.</span><span class="sxs-lookup"><span data-stu-id="37334-197">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="37334-198">Данные только менеджеров можно утвердить или отклонить изменения содержимого (новые или измененные).</span><span class="sxs-lookup"><span data-stu-id="37334-198">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="37334-199">Создайте обработчик авторизации администратора</span><span class="sxs-lookup"><span data-stu-id="37334-199">Create an administrator authorization handler</span></span>

<span data-ttu-id="37334-200">Создание `ContactAdministratorsAuthorizationHandler` класса в *авторизации* папки.</span><span class="sxs-lookup"><span data-stu-id="37334-200">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="37334-201">`ContactAdministratorsAuthorizationHandler` Проверяет, действует для ресурса пользователь является администратором.</span><span class="sxs-lookup"><span data-stu-id="37334-201">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="37334-202">Администратор может выполнять все операции.</span><span class="sxs-lookup"><span data-stu-id="37334-202">Administrator can do all operations.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="37334-203">Регистрировать обработчики авторизации</span><span class="sxs-lookup"><span data-stu-id="37334-203">Register the authorization handlers</span></span>

<span data-ttu-id="37334-204">С помощью Entity Framework Core Services должны быть зарегистрированы для [внедрения зависимостей](xref:fundamentals/dependency-injection) с помощью [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="37334-204">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="37334-205">`ContactIsOwnerAuthorizationHandler` Использует ASP.NET Core [удостоверения](xref:security/authentication/identity), которые построены на Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="37334-205">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="37334-206">Зарегистрировать обработчики с коллекцией служб, они будут доступны для `ContactsController` через [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="37334-206">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="37334-207">Добавьте следующий код в конец `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="37334-207">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=ConfigureServices&highlight=41-999)]

<span data-ttu-id="37334-208">`ContactAdministratorsAuthorizationHandler`и `ContactManagerAuthorizationHandler` добавляются как одноэлементных кортежей.</span><span class="sxs-lookup"><span data-stu-id="37334-208">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="37334-209">Они единственных экземпляров, так как они не используют EF и все сведения, необходимые возможности `Context` параметр `HandleRequirementAsync` метода.</span><span class="sxs-lookup"><span data-stu-id="37334-209">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="37334-210">Поддержка авторизации</span><span class="sxs-lookup"><span data-stu-id="37334-210">Support authorization</span></span>

<span data-ttu-id="37334-211">В этом разделе обновления страниц Razor и добавления класса требований операции.</span><span class="sxs-lookup"><span data-stu-id="37334-211">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="37334-212">Просмотрите требования класс контакта operations</span><span class="sxs-lookup"><span data-stu-id="37334-212">Review the contact operations requirements class</span></span>

<span data-ttu-id="37334-213">Просмотрите `ContactOperations` класса.</span><span class="sxs-lookup"><span data-stu-id="37334-213">Review the `ContactOperations` class.</span></span> <span data-ttu-id="37334-214">Этот класс содержит требования поддерживает приложения:</span><span class="sxs-lookup"><span data-stu-id="37334-214">This class contains the requirements the app supports:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-razor-pages"></a><span data-ttu-id="37334-215">Создать базовый класс для страниц Razor</span><span class="sxs-lookup"><span data-stu-id="37334-215">Create a base class for the Razor Pages</span></span>

<span data-ttu-id="37334-216">Создайте базовый класс, который содержит службы, используемые в контактах страниц Razor.</span><span class="sxs-lookup"><span data-stu-id="37334-216">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="37334-217">Базовый класс используется этот код инициализации в одном месте:</span><span class="sxs-lookup"><span data-stu-id="37334-217">The base class puts that initialization code in one location:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="37334-218">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="37334-218">The preceding code:</span></span>

* <span data-ttu-id="37334-219">Добавляет `IAuthorizationService` для доступа к обработчикам авторизации.</span><span class="sxs-lookup"><span data-stu-id="37334-219">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="37334-220">Добавляет идентификатор `UserManager` службы.</span><span class="sxs-lookup"><span data-stu-id="37334-220">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="37334-221">Добавьте `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="37334-221">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="37334-222">Обновление CreateModel</span><span class="sxs-lookup"><span data-stu-id="37334-222">Update the CreateModel</span></span>

<span data-ttu-id="37334-223">Измените конструктор модели создать страницу для использования `DI_BasePageModel` базового класса:</span><span class="sxs-lookup"><span data-stu-id="37334-223">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="37334-224">Обновление `CreateModel.OnPostAsync` метода:</span><span class="sxs-lookup"><span data-stu-id="37334-224">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="37334-225">Добавьте идентификатор пользователя, который `Contact` модели.</span><span class="sxs-lookup"><span data-stu-id="37334-225">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="37334-226">Вызывает обработчик авторизации, чтобы убедиться, что пользователь имеет разрешение на создание контактов.</span><span class="sxs-lookup"><span data-stu-id="37334-226">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="37334-227">Обновление IndexModel</span><span class="sxs-lookup"><span data-stu-id="37334-227">Update the IndexModel</span></span>

<span data-ttu-id="37334-228">Обновление `OnGetAsync` метод только утвержденные контакты отображаются обычным пользователям:</span><span class="sxs-lookup"><span data-stu-id="37334-228">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="37334-229">Обновление EditModel</span><span class="sxs-lookup"><span data-stu-id="37334-229">Update the EditModel</span></span>

<span data-ttu-id="37334-230">Добавьте обработчик авторизации, чтобы убедиться, что пользователь является владельцем контакта.</span><span class="sxs-lookup"><span data-stu-id="37334-230">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="37334-231">Поскольку проверки авторизации ресурсов `[Authorize]` атрибута недостаточно.</span><span class="sxs-lookup"><span data-stu-id="37334-231">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="37334-232">Приложение не имеет доступа к ресурсу, при оценке атрибутов.</span><span class="sxs-lookup"><span data-stu-id="37334-232">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="37334-233">Авторизация на основе ресурсов должно быть принудительной.</span><span class="sxs-lookup"><span data-stu-id="37334-233">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="37334-234">После приложение имеет доступ к ресурсу, загрузив его в модель страницы или, загрузив его в сам обработчик должен выполняться проверки.</span><span class="sxs-lookup"><span data-stu-id="37334-234">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="37334-235">Частом обращении к ресурсу, передавая ключ ресурса.</span><span class="sxs-lookup"><span data-stu-id="37334-235">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="37334-236">Обновление DeleteModel</span><span class="sxs-lookup"><span data-stu-id="37334-236">Update the DeleteModel</span></span>

<span data-ttu-id="37334-237">Обновление модели страницы delete на использование обработчика авторизации, чтобы убедиться, что пользователь имеет разрешение на удаление контакта.</span><span class="sxs-lookup"><span data-stu-id="37334-237">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="37334-238">Внедрить службы авторизации в представления</span><span class="sxs-lookup"><span data-stu-id="37334-238">Inject the authorization service into the views</span></span>

<span data-ttu-id="37334-239">В настоящее время показано пользовательского интерфейса, изменение и удаление ссылки на данные, которые не может быть изменено пользователем.</span><span class="sxs-lookup"><span data-stu-id="37334-239">Currently, the UI shows edit and delete links for data the user can't modify.</span></span> <span data-ttu-id="37334-240">Пользовательский Интерфейс фиксируется путем применения авторизации обработчика представлений.</span><span class="sxs-lookup"><span data-stu-id="37334-240">The UI is fixed by applying the authorization handler to the views.</span></span>

<span data-ttu-id="37334-241">Запустить службу авторизации в *Views/_ViewImports.cshtml* файл, чтобы сделать ее доступной для всех представлений:</span><span class="sxs-lookup"><span data-stu-id="37334-241">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[Main](secure-data/samples/final2/Pages/_ViewImports.cshtml?highlight=6-9)]

<span data-ttu-id="37334-242">Добавляет предыдущей разметки некоторые `using` инструкции.</span><span class="sxs-lookup"><span data-stu-id="37334-242">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="37334-243">Обновление **изменить** и **удалить** связывает *Pages/Contacts/Index.cshtml* , только отображаются для пользователей с соответствующими разрешениями:</span><span class="sxs-lookup"><span data-stu-id="37334-243">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[Main](secure-data/samples/final2/Pages/Contacts/Index.cshtml?highlight=34-36,64-999)]

> [!WARNING]
> <span data-ttu-id="37334-244">Скрытие ссылок от пользователей, у которых нет разрешения на изменение данных не защищать приложения.</span><span class="sxs-lookup"><span data-stu-id="37334-244">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="37334-245">Скрытие ссылки позволяет сделать приложение более удобной для пользователей, отображая только допустимые ссылки.</span><span class="sxs-lookup"><span data-stu-id="37334-245">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="37334-246">Пользователи могут hack созданные URL-адреса, вызывать изменение и удаление операций с данными, которыми они не владеют.</span><span class="sxs-lookup"><span data-stu-id="37334-246">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="37334-247">Страница Razor или контроллер должен Принудительная проверка доступа для защиты данных.</span><span class="sxs-lookup"><span data-stu-id="37334-247">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="37334-248">Дополнительные сведения об обновлении</span><span class="sxs-lookup"><span data-stu-id="37334-248">Update Details</span></span>

<span data-ttu-id="37334-249">Обновление представления сведений, чтобы менеджеры могли утверждать или отклонять контакты:</span><span class="sxs-lookup"><span data-stu-id="37334-249">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[Main](secure-data/samples/final2/Pages/Contacts/Details.cshtml?range=48-999)]

<span data-ttu-id="37334-250">Обновите модель страницы сведений:</span><span class="sxs-lookup"><span data-stu-id="37334-250">Update the details page model:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="test-the-completed-app"></a><span data-ttu-id="37334-251">Тестирование завершенного приложения</span><span class="sxs-lookup"><span data-stu-id="37334-251">Test the completed app</span></span>

<span data-ttu-id="37334-252">Если с помощью кода Visual Studio или тестирование на локальном платформу, которая не включает тестового сертификата для SSL.</span><span class="sxs-lookup"><span data-stu-id="37334-252">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for SSL:</span></span>

* <span data-ttu-id="37334-253">Задать `"LocalTest:skipSSL": true` в *appsettings. Developement.JSON* файл, чтобы пропустить требование SSL.</span><span class="sxs-lookup"><span data-stu-id="37334-253">Set `"LocalTest:skipSSL": true` in the *appsettings.Developement.json* file to skip the SSL requirement.</span></span> <span data-ttu-id="37334-254">Пропустить SSL только на компьютере разработки.</span><span class="sxs-lookup"><span data-stu-id="37334-254">Skip SSL only on a development machine.</span></span>

<span data-ttu-id="37334-255">Если приложение имеет контактов:</span><span class="sxs-lookup"><span data-stu-id="37334-255">If the app has contacts:</span></span>

* <span data-ttu-id="37334-256">Удалить все записи из `Contact` таблицы.</span><span class="sxs-lookup"><span data-stu-id="37334-256">Delete all the records in the `Contact` table.</span></span>
* <span data-ttu-id="37334-257">Перезапустите приложение для инициализации базы данных.</span><span class="sxs-lookup"><span data-stu-id="37334-257">Restart the app to seed the database.</span></span>

<span data-ttu-id="37334-258">Для просмотра контактов регистрации пользователя.</span><span class="sxs-lookup"><span data-stu-id="37334-258">Register a user for browsing the contacts.</span></span>

<span data-ttu-id="37334-259">Простой способ тестирования завершенное приложение — запуск три разных браузерах (или версий браузера или InPrivate).</span><span class="sxs-lookup"><span data-stu-id="37334-259">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="37334-260">В одном браузере регистрации нового пользователя (например, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="37334-260">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="37334-261">Вход для каждого веб-обозревателя с другим пользователем.</span><span class="sxs-lookup"><span data-stu-id="37334-261">Sign in to each browser with a different user.</span></span> <span data-ttu-id="37334-262">Проверьте следующие операции:</span><span class="sxs-lookup"><span data-stu-id="37334-262">Verify the following operations:</span></span>

* <span data-ttu-id="37334-263">Зарегистрированные пользователи могут просматривать утвержденные контактных данных.</span><span class="sxs-lookup"><span data-stu-id="37334-263">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="37334-264">Зарегистрированные пользователи могут изменить или удалить свои собственные данные.</span><span class="sxs-lookup"><span data-stu-id="37334-264">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="37334-265">Менеджеры могли утверждать или отклонять контактные данные.</span><span class="sxs-lookup"><span data-stu-id="37334-265">Managers can approve or reject contact data.</span></span> <span data-ttu-id="37334-266">`Details` Представлении отображаются **утвердить** и **Отклонить** кнопки.</span><span class="sxs-lookup"><span data-stu-id="37334-266">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="37334-267">Администраторы могут утвердить или отклонить и изменить или удалить все данные.</span><span class="sxs-lookup"><span data-stu-id="37334-267">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="37334-268">Пользовательская</span><span class="sxs-lookup"><span data-stu-id="37334-268">User</span></span>| <span data-ttu-id="37334-269">Параметры</span><span class="sxs-lookup"><span data-stu-id="37334-269">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="37334-270">Можно изменить или удалить собственных данных</span><span class="sxs-lookup"><span data-stu-id="37334-270">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="37334-271">Можно утвердить или отклонить и изменить или удалить принадлежат данные</span><span class="sxs-lookup"><span data-stu-id="37334-271">Can approve/reject and edit/delete own data</span></span> |
| admin@contoso.com | <span data-ttu-id="37334-272">Можно изменить или удалить и утвердить или отклонить все данные</span><span class="sxs-lookup"><span data-stu-id="37334-272">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="37334-273">Создание контакта в браузере администратора.</span><span class="sxs-lookup"><span data-stu-id="37334-273">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="37334-274">Скопируйте URL-адрес для удаления и изменения из обратитесь к администратору.</span><span class="sxs-lookup"><span data-stu-id="37334-274">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="37334-275">Вставьте эти ссылки в браузер тестового пользователя, чтобы убедиться, что тестовый пользователь не может выполнять эти операции.</span><span class="sxs-lookup"><span data-stu-id="37334-275">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="37334-276">Создание начального уровня приложения</span><span class="sxs-lookup"><span data-stu-id="37334-276">Create the starter app</span></span>

* <span data-ttu-id="37334-277">Создание страниц Razor приложения с именем «ContactManager»</span><span class="sxs-lookup"><span data-stu-id="37334-277">Create a Razor Pages app named "ContactManager"</span></span>

  * <span data-ttu-id="37334-278">Создание приложения с **отдельных учетных записей пользователей**.</span><span class="sxs-lookup"><span data-stu-id="37334-278">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="37334-279">Присвойте ему имя «ContactManager», пространства имен соответствует пространству имен, используемые в образце.</span><span class="sxs-lookup"><span data-stu-id="37334-279">Name it "ContactManager" so your namespace matches the namespace used in the sample.</span></span>

  ```console
  dotnet new razor -o ContactManager -au Individual -uld
  ```

  * <span data-ttu-id="37334-280">`-uld`Указывает LocalDB вместо SQLite</span><span class="sxs-lookup"><span data-stu-id="37334-280">`-uld` specifies LocalDB instead of SQLite</span></span>

* <span data-ttu-id="37334-281">Добавьте следующие `Contact` модели:</span><span class="sxs-lookup"><span data-stu-id="37334-281">Add the following `Contact` model:</span></span>

  [!code-csharp[Main](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="37334-282">Представление формирования `Contact` модели:</span><span class="sxs-lookup"><span data-stu-id="37334-282">Scaffold the `Contact` model:</span></span>

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
```

* <span data-ttu-id="37334-283">Обновление **ContactManager** привязки в *Pages/_Layout.cshtml* файла:</span><span class="sxs-lookup"><span data-stu-id="37334-283">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* <span data-ttu-id="37334-284">Формировать первоначальной миграции и обновления базы данных:</span><span class="sxs-lookup"><span data-stu-id="37334-284">Scaffold the initial migration and update the database:</span></span>

```console
dotnet ef migrations add initial
dotnet ef database update
```

* <span data-ttu-id="37334-285">Протестируйте приложение, создание, изменение и удаление контакта</span><span class="sxs-lookup"><span data-stu-id="37334-285">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="37334-286">Заполнение базы данных</span><span class="sxs-lookup"><span data-stu-id="37334-286">Seed the database</span></span>

<span data-ttu-id="37334-287">Добавить `SeedData` класса *данные* папки.</span><span class="sxs-lookup"><span data-stu-id="37334-287">Add the `SeedData` class to the *Data* folder.</span></span> <span data-ttu-id="37334-288">Если загруженный образца можно скопировать *SeedData.cs* файл *данные* папки начальный проект.</span><span class="sxs-lookup"><span data-stu-id="37334-288">If you've downloaded the sample, you can copy the *SeedData.cs* file to the *Data* folder of the starter project.</span></span>

<span data-ttu-id="37334-289">Вызовите `SeedData.Initialize` из `Main`:</span><span class="sxs-lookup"><span data-stu-id="37334-289">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[Main](secure-data/samples/starter2/Program.cs?name=snippet)]

<span data-ttu-id="37334-290">Тест заполняется, приложение базы данных.</span><span class="sxs-lookup"><span data-stu-id="37334-290">Test that the app seeded the database.</span></span> <span data-ttu-id="37334-291">Если все строки в контакте DB, метод инициализации не запускается.</span><span class="sxs-lookup"><span data-stu-id="37334-291">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="37334-292">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="37334-292">Additional resources</span></span>

* <span data-ttu-id="37334-293">[Лаборатории авторизации ASP.NET Core](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="37334-293">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="37334-294">Эта лаборатория приведены более подробные сведения о средствах безопасности, представленные в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="37334-294">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="37334-295">Авторизация в ASP.NET Core: простой, роли, на основе утверждений, а также пользовательские</span><span class="sxs-lookup"><span data-stu-id="37334-295">Authorization in ASP.NET Core: Simple, role, claims-based, and custom</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="37334-296">Пользовательская авторизация на основе политик</span><span class="sxs-lookup"><span data-stu-id="37334-296">Custom policy-based authorization</span></span>](xref:security/authorization/policies)
