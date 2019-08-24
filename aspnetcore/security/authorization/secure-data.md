---
title: Создание приложения ASP.NET Core с помощью данных пользователя с помощью авторизации
author: rick-anderson
description: Узнайте, как создать приложение Razor Pages с помощью данных пользователя с помощью авторизации. Включает протокол HTTPS, проверка подлинности и безопасность, удостоверения ASP.NET Core.
ms.author: riande
ms.date: 12/18/2018
ms.custom: mvc, seodec18
uid: security/authorization/secure-data
ms.openlocfilehash: 225d0e3aa51745253d03e614b1c8568b3a6ba731
ms.sourcegitcommit: 983b31449fe398e6e922eb13e9eb6f4287ec91e8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/24/2019
ms.locfileid: "70017487"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="c1e01-104">Создание приложения ASP.NET Core с помощью данных пользователя с помощью авторизации</span><span class="sxs-lookup"><span data-stu-id="c1e01-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="c1e01-105">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Джо Одетт](https://twitter.com/joeaudette) (Joe Audette)</span><span class="sxs-lookup"><span data-stu-id="c1e01-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="c1e01-106">См. в разделе [этот PDF-ФАЙЛ](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) для версии ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="c1e01-106">See [this PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="c1e01-107">Версия ASP.NET Core 1.1 работы с этим руководством, [это](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) папки.</span><span class="sxs-lookup"><span data-stu-id="c1e01-107">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="c1e01-108">1\.1, пример ASP.NET Core находится в [примеры](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="c1e01-108">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="c1e01-109">См. в разделе [этот PDF-файл](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span><span class="sxs-lookup"><span data-stu-id="c1e01-109">See [this pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c1e01-110">Этом руководстве показано, как создать веб-приложение ASP.NET Core с помощью данных пользователя с помощью авторизации.</span><span class="sxs-lookup"><span data-stu-id="c1e01-110">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="c1e01-111">Отображается список контактов, прошедшие проверку подлинности (зарегистрированного) пользователи создали.</span><span class="sxs-lookup"><span data-stu-id="c1e01-111">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="c1e01-112">Существует три группы безопасности:</span><span class="sxs-lookup"><span data-stu-id="c1e01-112">There are three security groups:</span></span>

* <span data-ttu-id="c1e01-113">**Зарегистрированные пользователи** может просматривать все данные, утвержденных и может изменять и удалять свои собственные данные.</span><span class="sxs-lookup"><span data-stu-id="c1e01-113">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="c1e01-114">**Диспетчеры** можно утвердить или отклонить контактных данных.</span><span class="sxs-lookup"><span data-stu-id="c1e01-114">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="c1e01-115">Только утвержденные контакты отображаются для пользователей.</span><span class="sxs-lookup"><span data-stu-id="c1e01-115">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="c1e01-116">**Администраторы** можно утверждать или отклонять и изменить или удалить все данные.</span><span class="sxs-lookup"><span data-stu-id="c1e01-116">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="c1e01-117">Изображения в этом документе не полностью соответствуют последним шаблонам.</span><span class="sxs-lookup"><span data-stu-id="c1e01-117">The images in this document don't exactly match the latest templates.</span></span>

<span data-ttu-id="c1e01-118">На следующем рисунке, пользователь Рик (`rick@example.com`) выполнил вход.</span><span class="sxs-lookup"><span data-stu-id="c1e01-118">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="c1e01-119">Рик могут только просматривать утвержденные контакты и **изменить**/**удалить**/**Create New** ссылки на свои контакты.</span><span class="sxs-lookup"><span data-stu-id="c1e01-119">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="c1e01-120">Только последней записи, созданные Рик, отображает **изменить** и **удалить** ссылки.</span><span class="sxs-lookup"><span data-stu-id="c1e01-120">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="c1e01-121">Другие пользователи увидят последнюю запись, после менеджеру или администратору изменяет состояние на «Утверждено».</span><span class="sxs-lookup"><span data-stu-id="c1e01-121">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Снимок экрана, показывающий Рик входа](secure-data/_static/rick.png)

<span data-ttu-id="c1e01-123">На следующем рисунке `manager@contoso.com` вход выполнен и в роли руководителя:</span><span class="sxs-lookup"><span data-stu-id="c1e01-123">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![Снимок экрана, показывающий manager@contoso.com в системе](secure-data/_static/manager1.png)

<span data-ttu-id="c1e01-125">На следующем рисунке показана менеджеров Просмотр подробностей контакта:</span><span class="sxs-lookup"><span data-stu-id="c1e01-125">The following image shows the managers details view of a contact:</span></span>

![Представление руководителя контакта](secure-data/_static/manager.png)

<span data-ttu-id="c1e01-127">**Утвердить** и **Отклонить** кнопки отображаются только для менеджеров и администраторов.</span><span class="sxs-lookup"><span data-stu-id="c1e01-127">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="c1e01-128">На следующем рисунке `admin@contoso.com` вход выполнен и в роли администратора:</span><span class="sxs-lookup"><span data-stu-id="c1e01-128">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![Снимок экрана, показывающий admin@contoso.com в системе](secure-data/_static/admin.png)

<span data-ttu-id="c1e01-130">Администратор имеет все привилегии.</span><span class="sxs-lookup"><span data-stu-id="c1e01-130">The administrator has all privileges.</span></span> <span data-ttu-id="c1e01-131">Она может чтения, изменить или удалить любой контакт и изменить состояние контактов.</span><span class="sxs-lookup"><span data-stu-id="c1e01-131">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="c1e01-132">Приложение было создано [формирования шаблонов](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) следующие `Contact` модели:</span><span class="sxs-lookup"><span data-stu-id="c1e01-132">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="c1e01-133">Пример содержит следующие обработчики авторизации:</span><span class="sxs-lookup"><span data-stu-id="c1e01-133">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="c1e01-134">`ContactIsOwnerAuthorizationHandler`: Гарантирует, что пользователь может изменять только свои данные.</span><span class="sxs-lookup"><span data-stu-id="c1e01-134">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="c1e01-135">`ContactManagerAuthorizationHandler`: Позволяет руководителям утверждать или отклонять контакты.</span><span class="sxs-lookup"><span data-stu-id="c1e01-135">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="c1e01-136">`ContactAdministratorsAuthorizationHandler`: Позволяет администраторам утверждать или отклонять контакты, а также изменять или удалять контакты.</span><span class="sxs-lookup"><span data-stu-id="c1e01-136">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c1e01-137">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="c1e01-137">Prerequisites</span></span>

<span data-ttu-id="c1e01-138">Этот учебник является дополнительным.</span><span class="sxs-lookup"><span data-stu-id="c1e01-138">This tutorial is advanced.</span></span> <span data-ttu-id="c1e01-139">Вы должны быть знакомы с:</span><span class="sxs-lookup"><span data-stu-id="c1e01-139">You should be familiar with:</span></span>

* [<span data-ttu-id="c1e01-140">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c1e01-140">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="c1e01-141">Authentication</span><span class="sxs-lookup"><span data-stu-id="c1e01-141">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="c1e01-142">Подтверждение учетной записи и восстановление пароля</span><span class="sxs-lookup"><span data-stu-id="c1e01-142">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="c1e01-143">Авторизация</span><span class="sxs-lookup"><span data-stu-id="c1e01-143">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="c1e01-144">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c1e01-144">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="c1e01-145">Начальный и завершенное приложение</span><span class="sxs-lookup"><span data-stu-id="c1e01-145">The starter and completed app</span></span>

<span data-ttu-id="c1e01-146">[Скачайте](xref:index#how-to-download-a-sample) [завершения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) приложения.</span><span class="sxs-lookup"><span data-stu-id="c1e01-146">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="c1e01-147">[Тест](#test-the-completed-app) готовое приложение, поэтому вам ознакомиться с его возможностями безопасности.</span><span class="sxs-lookup"><span data-stu-id="c1e01-147">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="c1e01-148">Приложение начального уровня</span><span class="sxs-lookup"><span data-stu-id="c1e01-148">The starter app</span></span>

<span data-ttu-id="c1e01-149">[Скачайте](xref:index#how-to-download-a-sample) [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) приложения.</span><span class="sxs-lookup"><span data-stu-id="c1e01-149">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="c1e01-150">Запустите приложение, коснитесь **ContactManager** связать и убедитесь, создание, изменение и удаление контакта.</span><span class="sxs-lookup"><span data-stu-id="c1e01-150">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="c1e01-151">Обеспечение безопасности данных пользователя</span><span class="sxs-lookup"><span data-stu-id="c1e01-151">Secure user data</span></span>

<span data-ttu-id="c1e01-152">В следующих разделах есть все основные шаги по созданию приложения данные безопасности пользователей.</span><span class="sxs-lookup"><span data-stu-id="c1e01-152">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="c1e01-153">Могут оказаться полезными для ссылки на готового проекта.</span><span class="sxs-lookup"><span data-stu-id="c1e01-153">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="c1e01-154">Привязать контактные данные для пользователя</span><span class="sxs-lookup"><span data-stu-id="c1e01-154">Tie the contact data to the user</span></span>

<span data-ttu-id="c1e01-155">Использование ASP.NET [удостоверений](xref:security/authentication/identity) идентификатор пользователя, чтобы предоставить пользователям можно изменить свои данные, но не данные других пользователей.</span><span class="sxs-lookup"><span data-stu-id="c1e01-155">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="c1e01-156">Добавить `OwnerID` и `ContactStatus` для `Contact` модели:</span><span class="sxs-lookup"><span data-stu-id="c1e01-156">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final3/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="c1e01-157">`OwnerID` — Это идентификатор пользователя из `AspNetUser` в таблицу [удостоверений](xref:security/authentication/identity) базы данных.</span><span class="sxs-lookup"><span data-stu-id="c1e01-157">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="c1e01-158">`Status` Поля определяет, является ли контакт для просмотра обычными пользователями.</span><span class="sxs-lookup"><span data-stu-id="c1e01-158">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="c1e01-159">Создание новой миграции и обновления базы данных:</span><span class="sxs-lookup"><span data-stu-id="c1e01-159">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="c1e01-160">Добавление служб роли удостоверению</span><span class="sxs-lookup"><span data-stu-id="c1e01-160">Add Role services to Identity</span></span>

<span data-ttu-id="c1e01-161">Добавление [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) для добавления служб ролей:</span><span class="sxs-lookup"><span data-stu-id="c1e01-161">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet2&highlight=9)]

### <a name="require-authenticated-users"></a><span data-ttu-id="c1e01-162">Требовать от пользователей, прошедших проверку подлинности</span><span class="sxs-lookup"><span data-stu-id="c1e01-162">Require authenticated users</span></span>

<span data-ttu-id="c1e01-163">Задайте политику проверки подлинности по умолчанию, чтобы требовать проверку подлинности пользователей:</span><span class="sxs-lookup"><span data-stu-id="c1e01-163">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet&highlight=15-99)] 

 <span data-ttu-id="c1e01-164">Можно отказаться от проверки подлинности на уровне метода действия, контроллера или страницу Razor с `[AllowAnonymous]` атрибута.</span><span class="sxs-lookup"><span data-stu-id="c1e01-164">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="c1e01-165">Задание политики проверки подлинности по умолчанию требовать от пользователей пройти проверку подлинности защищает вновь добавленный Razor Pages и контроллеры.</span><span class="sxs-lookup"><span data-stu-id="c1e01-165">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="c1e01-166">Наличие требуется по умолчанию проверка подлинности безопаснее, чем полагаться на новые контроллеры и страницы Razor для включения `[Authorize]` атрибута.</span><span class="sxs-lookup"><span data-stu-id="c1e01-166">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="c1e01-167">Добавьте [allowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) на страницы индекса и конфиденциальности, чтобы анонимные пользователи могли получить сведения о сайте перед их регистрацией.</span><span class="sxs-lookup"><span data-stu-id="c1e01-167">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index and Privacy pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Index.cshtml.cs?highlight=1,7)]

### <a name="configure-the-test-account"></a><span data-ttu-id="c1e01-168">Настройка тестовой учетной записью</span><span class="sxs-lookup"><span data-stu-id="c1e01-168">Configure the test account</span></span>

<span data-ttu-id="c1e01-169">`SeedData` Класс создает две учетные записи: администратора и диспетчера.</span><span class="sxs-lookup"><span data-stu-id="c1e01-169">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="c1e01-170">Используйте [средство Secret Manager](xref:security/app-secrets) Установка пароля для этих учетных записей.</span><span class="sxs-lookup"><span data-stu-id="c1e01-170">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="c1e01-171">Задайте пароль из каталога проекта (каталог, содержащий *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="c1e01-171">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="c1e01-172">Если надежный пароль не указан, создается исключение при `SeedData.Initialize` вызывается.</span><span class="sxs-lookup"><span data-stu-id="c1e01-172">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="c1e01-173">Обновление `Main` используйте пароль теста:</span><span class="sxs-lookup"><span data-stu-id="c1e01-173">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final3/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="c1e01-174">Создание тестовых учетных записей и обновление контактов</span><span class="sxs-lookup"><span data-stu-id="c1e01-174">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="c1e01-175">Обновление `Initialize` метод в `SeedData` класса для создания тестовых учетных записей:</span><span class="sxs-lookup"><span data-stu-id="c1e01-175">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="c1e01-176">Добавьте идентификатор пользователя администратора и `ContactStatus` контактам.</span><span class="sxs-lookup"><span data-stu-id="c1e01-176">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="c1e01-177">Создать контактных лиц с «Отправлено» и одного «отклонено».</span><span class="sxs-lookup"><span data-stu-id="c1e01-177">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="c1e01-178">Добавьте идентификатор пользователя и состояние всех контактов.</span><span class="sxs-lookup"><span data-stu-id="c1e01-178">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="c1e01-179">Отображается только один контакт:</span><span class="sxs-lookup"><span data-stu-id="c1e01-179">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="c1e01-180">Создание владельца, manager и обработчики авторизации администратора</span><span class="sxs-lookup"><span data-stu-id="c1e01-180">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="c1e01-181">Создание `ContactIsOwnerAuthorizationHandler` в класс *авторизации* папки.</span><span class="sxs-lookup"><span data-stu-id="c1e01-181">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="c1e01-182">`ContactIsOwnerAuthorizationHandler` Проверяет, что пользователь, выполняющий ресурса, которому принадлежит ресурс.</span><span class="sxs-lookup"><span data-stu-id="c1e01-182">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="c1e01-183">`ContactIsOwnerAuthorizationHandler` Вызовы [контекста. Успешно](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) при связаться с владельцем текущего авторизованного пользователя.</span><span class="sxs-lookup"><span data-stu-id="c1e01-183">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="c1e01-184">Обработчики авторизации обычно:</span><span class="sxs-lookup"><span data-stu-id="c1e01-184">Authorization handlers generally:</span></span>

* <span data-ttu-id="c1e01-185">Вернуть `context.Succeed` при соблюдении требований.</span><span class="sxs-lookup"><span data-stu-id="c1e01-185">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="c1e01-186">Вернуть `Task.CompletedTask` Если требования не соблюдены.</span><span class="sxs-lookup"><span data-stu-id="c1e01-186">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="c1e01-187">`Task.CompletedTask`не является успешным или неудачным&mdash;, что позволяет запускать другие обработчики авторизации.</span><span class="sxs-lookup"><span data-stu-id="c1e01-187">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="c1e01-188">Если необходимо выполнить явную ошибку, возвратить [контекста. Сбой](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="c1e01-188">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="c1e01-189">Приложение позволяет контактные владельцам изменения или удаления и создания собственных данных.</span><span class="sxs-lookup"><span data-stu-id="c1e01-189">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="c1e01-190">`ContactIsOwnerAuthorizationHandler` не требуется для проверки операции, передаваемые в качестве параметра требование.</span><span class="sxs-lookup"><span data-stu-id="c1e01-190">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="c1e01-191">Создайте обработчик диспетчера авторизации</span><span class="sxs-lookup"><span data-stu-id="c1e01-191">Create a manager authorization handler</span></span>

<span data-ttu-id="c1e01-192">Создание `ContactManagerAuthorizationHandler` в класс *авторизации* папки.</span><span class="sxs-lookup"><span data-stu-id="c1e01-192">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="c1e01-193">`ContactManagerAuthorizationHandler` Проверяет пользователя, действующий от ресурса является руководителем.</span><span class="sxs-lookup"><span data-stu-id="c1e01-193">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="c1e01-194">Только менеджеры могли утверждать или отклонять изменения содержимого (новые или измененные).</span><span class="sxs-lookup"><span data-stu-id="c1e01-194">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="c1e01-195">Создайте обработчик авторизации администратора</span><span class="sxs-lookup"><span data-stu-id="c1e01-195">Create an administrator authorization handler</span></span>

<span data-ttu-id="c1e01-196">Создание `ContactAdministratorsAuthorizationHandler` в класс *авторизации* папки.</span><span class="sxs-lookup"><span data-stu-id="c1e01-196">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="c1e01-197">`ContactAdministratorsAuthorizationHandler` Проверяет действующий от ресурса пользователь является администратором.</span><span class="sxs-lookup"><span data-stu-id="c1e01-197">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="c1e01-198">Администратор может выполнить все операции.</span><span class="sxs-lookup"><span data-stu-id="c1e01-198">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="c1e01-199">Регистрировать обработчики авторизации</span><span class="sxs-lookup"><span data-stu-id="c1e01-199">Register the authorization handlers</span></span>

<span data-ttu-id="c1e01-200">Службы, с помощью Entity Framework Core должны быть зарегистрированы для [внедрения зависимостей](xref:fundamentals/dependency-injection) с помощью [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="c1e01-200">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="c1e01-201">`ContactIsOwnerAuthorizationHandler` Использует ASP.NET Core [удостоверений](xref:security/authentication/identity), которая опирается на Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c1e01-201">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="c1e01-202">Зарегистрировать обработчики с коллекцией, службы, поэтому они будут доступны для `ContactsController` через [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c1e01-202">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="c1e01-203">Добавьте следующий код в конец `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c1e01-203">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet_defaultPolicy&highlight=23-99)]

<span data-ttu-id="c1e01-204">`ContactAdministratorsAuthorizationHandler` и `ContactManagerAuthorizationHandler` добавляются как одноэлементных экземпляров.</span><span class="sxs-lookup"><span data-stu-id="c1e01-204">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="c1e01-205">Они одноэлементных экземпляров, так как они не используют EF и все сведения, необходимые в `Context` параметр `HandleRequirementAsync` метод.</span><span class="sxs-lookup"><span data-stu-id="c1e01-205">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="c1e01-206">Поддержка авторизации</span><span class="sxs-lookup"><span data-stu-id="c1e01-206">Support authorization</span></span>

<span data-ttu-id="c1e01-207">В этом разделе вы обновите страницы Razor Pages и добавьте класс требования к операции.</span><span class="sxs-lookup"><span data-stu-id="c1e01-207">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="c1e01-208">Просмотрите класс требования контактные операций</span><span class="sxs-lookup"><span data-stu-id="c1e01-208">Review the contact operations requirements class</span></span>

<span data-ttu-id="c1e01-209">Просмотрите `ContactOperations` класса.</span><span class="sxs-lookup"><span data-stu-id="c1e01-209">Review the `ContactOperations` class.</span></span> <span data-ttu-id="c1e01-210">Этот класс содержит требования к поддерживает приложения:</span><span class="sxs-lookup"><span data-stu-id="c1e01-210">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="c1e01-211">Создать базовый класс для страниц Razor контактов</span><span class="sxs-lookup"><span data-stu-id="c1e01-211">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="c1e01-212">Создайте базовый класс, который содержит службы, используемые в Razor Pages контакты.</span><span class="sxs-lookup"><span data-stu-id="c1e01-212">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="c1e01-213">Базовый класс помещает код инициализации в одном месте:</span><span class="sxs-lookup"><span data-stu-id="c1e01-213">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="c1e01-214">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="c1e01-214">The preceding code:</span></span>

* <span data-ttu-id="c1e01-215">Добавляет `IAuthorizationService` службы для доступа к обработчикам авторизации.</span><span class="sxs-lookup"><span data-stu-id="c1e01-215">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="c1e01-216">Добавляет удостоверение `UserManager` службы.</span><span class="sxs-lookup"><span data-stu-id="c1e01-216">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="c1e01-217">Добавьте `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="c1e01-217">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="c1e01-218">Обновление CreateModel</span><span class="sxs-lookup"><span data-stu-id="c1e01-218">Update the CreateModel</span></span>

<span data-ttu-id="c1e01-219">Измените конструктор модель страницы create для использования `DI_BasePageModel` базового класса:</span><span class="sxs-lookup"><span data-stu-id="c1e01-219">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="c1e01-220">Обновление `CreateModel.OnPostAsync` метод:</span><span class="sxs-lookup"><span data-stu-id="c1e01-220">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="c1e01-221">Добавьте идентификатор пользователя, `Contact` модели.</span><span class="sxs-lookup"><span data-stu-id="c1e01-221">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="c1e01-222">Вызовите обработчик авторизации, чтобы убедиться, что пользователь имеет разрешение на создание контактов.</span><span class="sxs-lookup"><span data-stu-id="c1e01-222">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="c1e01-223">Обновление IndexModel</span><span class="sxs-lookup"><span data-stu-id="c1e01-223">Update the IndexModel</span></span>

<span data-ttu-id="c1e01-224">Обновление `OnGetAsync` метод, поэтому отображаются только утвержденные контактов для обычных пользователей:</span><span class="sxs-lookup"><span data-stu-id="c1e01-224">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="c1e01-225">Обновление EditModel</span><span class="sxs-lookup"><span data-stu-id="c1e01-225">Update the EditModel</span></span>

<span data-ttu-id="c1e01-226">Добавление обработчика авторизации, чтобы убедиться, что пользователь является владельцем контакта.</span><span class="sxs-lookup"><span data-stu-id="c1e01-226">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="c1e01-227">Так как авторизации ресурсов проверяется, `[Authorize]` атрибут будет недостаточно.</span><span class="sxs-lookup"><span data-stu-id="c1e01-227">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="c1e01-228">Приложение не имеет доступа к ресурсу, при оценке атрибуты.</span><span class="sxs-lookup"><span data-stu-id="c1e01-228">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="c1e01-229">Авторизация на основе ресурсов должен быть явный.</span><span class="sxs-lookup"><span data-stu-id="c1e01-229">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="c1e01-230">Проверки должны выполняться, когда приложение имеет доступ к ресурсу, загрузив его в модели страниц или, загрузив его в сам обработчик.</span><span class="sxs-lookup"><span data-stu-id="c1e01-230">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="c1e01-231">Вы обращаетесь чаще всего ресурса, передавая ключ ресурса.</span><span class="sxs-lookup"><span data-stu-id="c1e01-231">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="c1e01-232">Обновление DeleteModel</span><span class="sxs-lookup"><span data-stu-id="c1e01-232">Update the DeleteModel</span></span>

<span data-ttu-id="c1e01-233">Обновите модель страницы delete на использование обработчика авторизации, чтобы убедиться, что пользователь имеет разрешение на удаление для контакта.</span><span class="sxs-lookup"><span data-stu-id="c1e01-233">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="c1e01-234">Внедрить службы авторизации в представления</span><span class="sxs-lookup"><span data-stu-id="c1e01-234">Inject the authorization service into the views</span></span>

<span data-ttu-id="c1e01-235">В настоящее время в пользовательском Интерфейсе показано изменение и удаление ссылок для контактов, которые не могут изменяться.</span><span class="sxs-lookup"><span data-stu-id="c1e01-235">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="c1e01-236">Вставьте службу авторизации в файл *pages/_ViewImports. cshtml* , чтобы она была доступна для всех представлений:</span><span class="sxs-lookup"><span data-stu-id="c1e01-236">Inject the authorization service in the *Pages/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="c1e01-237">Предыдущая разметка добавляет некоторые `using` инструкций.</span><span class="sxs-lookup"><span data-stu-id="c1e01-237">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="c1e01-238">Обновление **изменить** и **удалить** ссылок в *Pages/Contacts/Index.cshtml* , обработкой только для пользователей с соответствующими разрешениями:</span><span class="sxs-lookup"><span data-stu-id="c1e01-238">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="c1e01-239">Скрытие ссылок от пользователей, у которых нет разрешения на изменение данных не защитить приложения.</span><span class="sxs-lookup"><span data-stu-id="c1e01-239">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="c1e01-240">Скрытие ссылок делает приложение более удобным, отображая только допустимые ссылки.</span><span class="sxs-lookup"><span data-stu-id="c1e01-240">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="c1e01-241">Пользователи могут взломать созданный URL-адреса для вызова редактирования и удаления данных, которыми они не владеют.</span><span class="sxs-lookup"><span data-stu-id="c1e01-241">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="c1e01-242">На странице Razor или контроллер должен Принудительная проверка доступа для защиты данных.</span><span class="sxs-lookup"><span data-stu-id="c1e01-242">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="c1e01-243">Дополнительные сведения об обновлении</span><span class="sxs-lookup"><span data-stu-id="c1e01-243">Update Details</span></span>

<span data-ttu-id="c1e01-244">Обновление представления сведений, чтобы менеджеры могли утверждать или отклонять контактов:</span><span class="sxs-lookup"><span data-stu-id="c1e01-244">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="c1e01-245">Обновите модель страницы сведений:</span><span class="sxs-lookup"><span data-stu-id="c1e01-245">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="c1e01-246">Добавление или удаление пользователя к роли</span><span class="sxs-lookup"><span data-stu-id="c1e01-246">Add or remove a user to a role</span></span>

<span data-ttu-id="c1e01-247">См. в разделе [эту проблему](https://github.com/aspnet/AspNetCore.Docs/issues/8502) сведения о:</span><span class="sxs-lookup"><span data-stu-id="c1e01-247">See [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="c1e01-248">Удаление привилегий пользователя.</span><span class="sxs-lookup"><span data-stu-id="c1e01-248">Removing privileges from a user.</span></span> <span data-ttu-id="c1e01-249">Например, отзвука пользователя в приложении разговора.</span><span class="sxs-lookup"><span data-stu-id="c1e01-249">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="c1e01-250">Добавление прав к пользователю.</span><span class="sxs-lookup"><span data-stu-id="c1e01-250">Adding privileges to a user.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="c1e01-251">Тестирование завершенного приложения</span><span class="sxs-lookup"><span data-stu-id="c1e01-251">Test the completed app</span></span>

<span data-ttu-id="c1e01-252">Если вы еще не настроили пароль для учетных записей пользователей на основании добавляемых, использовать [средство Secret Manager](xref:security/app-secrets#secret-manager) Установка пароля:</span><span class="sxs-lookup"><span data-stu-id="c1e01-252">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="c1e01-253">Выберите надежный пароль: Используйте восемь и более символов и по крайней мере одну прописную букву, цифру и символ.</span><span class="sxs-lookup"><span data-stu-id="c1e01-253">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="c1e01-254">Например `Passw0rd!` требованиям надежный пароль.</span><span class="sxs-lookup"><span data-stu-id="c1e01-254">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="c1e01-255">Выполните следующую команду из папки проекта, где `<PW>` пароль:</span><span class="sxs-lookup"><span data-stu-id="c1e01-255">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```console
  dotnet user-secrets set SeedUserPW <PW>
  ```

<span data-ttu-id="c1e01-256">Если приложение имеет контактов:</span><span class="sxs-lookup"><span data-stu-id="c1e01-256">If the app has contacts:</span></span>

* <span data-ttu-id="c1e01-257">Удалить все записи в `Contact` таблицы.</span><span class="sxs-lookup"><span data-stu-id="c1e01-257">Delete all of the records in the `Contact` table.</span></span>
* <span data-ttu-id="c1e01-258">Перезапустите приложение, чтобы заполнить базу данных.</span><span class="sxs-lookup"><span data-stu-id="c1e01-258">Restart the app to seed the database.</span></span>

<span data-ttu-id="c1e01-259">Простой способ протестировать готовое приложение — запуск три различных браузеров (или инкогнито или InPrivate сеансов).</span><span class="sxs-lookup"><span data-stu-id="c1e01-259">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="c1e01-260">В один браузер, регистрация нового пользователя (например, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="c1e01-260">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="c1e01-261">Войдите в каждом браузере с другим пользователем.</span><span class="sxs-lookup"><span data-stu-id="c1e01-261">Sign in to each browser with a different user.</span></span> <span data-ttu-id="c1e01-262">Проверьте следующие операции:</span><span class="sxs-lookup"><span data-stu-id="c1e01-262">Verify the following operations:</span></span>

* <span data-ttu-id="c1e01-263">Зарегистрированные пользователи могут просматривать все утвержденные контактные данные.</span><span class="sxs-lookup"><span data-stu-id="c1e01-263">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="c1e01-264">Зарегистрированным пользователям может изменять и удалять свои собственные данные.</span><span class="sxs-lookup"><span data-stu-id="c1e01-264">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="c1e01-265">Руководители могут утверждать или отклонять контактных данных.</span><span class="sxs-lookup"><span data-stu-id="c1e01-265">Managers can approve/reject contact data.</span></span> <span data-ttu-id="c1e01-266">`Details` Просмотра показано **утвердить** и **Отклонить** кнопки.</span><span class="sxs-lookup"><span data-stu-id="c1e01-266">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="c1e01-267">Администраторы могут утверждать или отклонять и изменить или удалить все данные.</span><span class="sxs-lookup"><span data-stu-id="c1e01-267">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="c1e01-268">Пользовательская</span><span class="sxs-lookup"><span data-stu-id="c1e01-268">User</span></span>                | <span data-ttu-id="c1e01-269">Заполняется данными в приложении</span><span class="sxs-lookup"><span data-stu-id="c1e01-269">Seeded by the app</span></span> | <span data-ttu-id="c1e01-270">Параметры</span><span class="sxs-lookup"><span data-stu-id="c1e01-270">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="c1e01-271">Нет</span><span class="sxs-lookup"><span data-stu-id="c1e01-271">No</span></span>                | <span data-ttu-id="c1e01-272">Изменить или удалить данные принадлежат.</span><span class="sxs-lookup"><span data-stu-id="c1e01-272">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="c1e01-273">Да</span><span class="sxs-lookup"><span data-stu-id="c1e01-273">Yes</span></span>               | <span data-ttu-id="c1e01-274">Утверждать или отклонять и изменить или удалить данные принадлежат.</span><span class="sxs-lookup"><span data-stu-id="c1e01-274">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="c1e01-275">Да</span><span class="sxs-lookup"><span data-stu-id="c1e01-275">Yes</span></span>               | <span data-ttu-id="c1e01-276">Утверждать или отклонять и изменить или удалить все данные.</span><span class="sxs-lookup"><span data-stu-id="c1e01-276">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="c1e01-277">Создание контакта в браузере администратора.</span><span class="sxs-lookup"><span data-stu-id="c1e01-277">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="c1e01-278">Скопируйте URL-адрес для удаления и редактирование откуда обратитесь к администратору.</span><span class="sxs-lookup"><span data-stu-id="c1e01-278">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="c1e01-279">Вставьте эти ссылки в браузере пользователя теста, чтобы убедиться, что тестовый пользователь не может выполнять эти операции.</span><span class="sxs-lookup"><span data-stu-id="c1e01-279">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="c1e01-280">Создать приложение начального уровня</span><span class="sxs-lookup"><span data-stu-id="c1e01-280">Create the starter app</span></span>

* <span data-ttu-id="c1e01-281">Создание приложения Razor Pages с именем «ContactManager»</span><span class="sxs-lookup"><span data-stu-id="c1e01-281">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="c1e01-282">Создание приложения с **отдельным учетным записям пользователей**.</span><span class="sxs-lookup"><span data-stu-id="c1e01-282">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="c1e01-283">Назовите его «ContactManager», пространство имен соответствует пространству имен, используемые в образце.</span><span class="sxs-lookup"><span data-stu-id="c1e01-283">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="c1e01-284">`-uld` Указывает LocalDB вместо SQLite</span><span class="sxs-lookup"><span data-stu-id="c1e01-284">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="c1e01-285">Добавление *моделей/Contact. CS*:</span><span class="sxs-lookup"><span data-stu-id="c1e01-285">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="c1e01-286">Каркас `Contact` модели.</span><span class="sxs-lookup"><span data-stu-id="c1e01-286">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="c1e01-287">Создание первоначальной миграции и обновления базы данных:</span><span class="sxs-lookup"><span data-stu-id="c1e01-287">Create initial migration and update the database:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
  ```

<span data-ttu-id="c1e01-288">Если при выполнении `dotnet aspnet-codegenerator razorpage` команды возникла ошибка, см. [эту ошибку в GitHub](https://github.com/aspnet/Scaffolding/issues/984).</span><span class="sxs-lookup"><span data-stu-id="c1e01-288">If you experience a bug with the `dotnet aspnet-codegenerator razorpage` command, see [this GitHub issue](https://github.com/aspnet/Scaffolding/issues/984).</span></span>

* <span data-ttu-id="c1e01-289">Обновите привязку **ContactManager** в файле *pages/Shared/_layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="c1e01-289">Update the **ContactManager** anchor in the *Pages/Shared/_Layout.cshtml* file:</span></span>

 ```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Contacts/Index">ContactManager</a>
  ```

* <span data-ttu-id="c1e01-290">Протестируйте приложение, создание, изменение и удаление контакта</span><span class="sxs-lookup"><span data-stu-id="c1e01-290">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="c1e01-291">Заполнение базы данных</span><span class="sxs-lookup"><span data-stu-id="c1e01-291">Seed the database</span></span>

<span data-ttu-id="c1e01-292">Добавьте класс [сиддата](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) в папку *Data* :</span><span class="sxs-lookup"><span data-stu-id="c1e01-292">Add the [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) class to the *Data* folder:</span></span>

[!code-csharp[](secure-data/samples/starter3/Data/SeedData.cs)]

<span data-ttu-id="c1e01-293">Вызовите `SeedData.Initialize` из `Main`:</span><span class="sxs-lookup"><span data-stu-id="c1e01-293">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter3/Program.cs)]

<span data-ttu-id="c1e01-294">Проверьте, что приложение заполняется данными базы данных.</span><span class="sxs-lookup"><span data-stu-id="c1e01-294">Test that the app seeded the database.</span></span> <span data-ttu-id="c1e01-295">При возникновении любых строк в контакте DB, не запускается метод заполнения.</span><span class="sxs-lookup"><span data-stu-id="c1e01-295">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"

<span data-ttu-id="c1e01-296">Этом руководстве показано, как создать веб-приложение ASP.NET Core с помощью данных пользователя с помощью авторизации.</span><span class="sxs-lookup"><span data-stu-id="c1e01-296">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="c1e01-297">Отображается список контактов, прошедшие проверку подлинности (зарегистрированного) пользователи создали.</span><span class="sxs-lookup"><span data-stu-id="c1e01-297">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="c1e01-298">Существует три группы безопасности:</span><span class="sxs-lookup"><span data-stu-id="c1e01-298">There are three security groups:</span></span>

* <span data-ttu-id="c1e01-299">**Зарегистрированные пользователи** может просматривать все данные, утвержденных и может изменять и удалять свои собственные данные.</span><span class="sxs-lookup"><span data-stu-id="c1e01-299">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="c1e01-300">**Диспетчеры** можно утвердить или отклонить контактных данных.</span><span class="sxs-lookup"><span data-stu-id="c1e01-300">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="c1e01-301">Только утвержденные контакты отображаются для пользователей.</span><span class="sxs-lookup"><span data-stu-id="c1e01-301">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="c1e01-302">**Администраторы** можно утверждать или отклонять и изменить или удалить все данные.</span><span class="sxs-lookup"><span data-stu-id="c1e01-302">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="c1e01-303">На следующем рисунке, пользователь Рик (`rick@example.com`) выполнил вход.</span><span class="sxs-lookup"><span data-stu-id="c1e01-303">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="c1e01-304">Рик могут только просматривать утвержденные контакты и **изменить**/**удалить**/**Create New** ссылки на свои контакты.</span><span class="sxs-lookup"><span data-stu-id="c1e01-304">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="c1e01-305">Только последней записи, созданные Рик, отображает **изменить** и **удалить** ссылки.</span><span class="sxs-lookup"><span data-stu-id="c1e01-305">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="c1e01-306">Другие пользователи увидят последнюю запись, после менеджеру или администратору изменяет состояние на «Утверждено».</span><span class="sxs-lookup"><span data-stu-id="c1e01-306">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Снимок экрана, показывающий Рик входа](secure-data/_static/rick.png)

<span data-ttu-id="c1e01-308">На следующем рисунке `manager@contoso.com` вход выполнен и в роли руководителя:</span><span class="sxs-lookup"><span data-stu-id="c1e01-308">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![Снимок экрана, показывающий manager@contoso.com в системе](secure-data/_static/manager1.png)

<span data-ttu-id="c1e01-310">На следующем рисунке показана менеджеров Просмотр подробностей контакта:</span><span class="sxs-lookup"><span data-stu-id="c1e01-310">The following image shows the managers details view of a contact:</span></span>

![Представление руководителя контакта](secure-data/_static/manager.png)

<span data-ttu-id="c1e01-312">**Утвердить** и **Отклонить** кнопки отображаются только для менеджеров и администраторов.</span><span class="sxs-lookup"><span data-stu-id="c1e01-312">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="c1e01-313">На следующем рисунке `admin@contoso.com` вход выполнен и в роли администратора:</span><span class="sxs-lookup"><span data-stu-id="c1e01-313">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![Снимок экрана, показывающий admin@contoso.com в системе](secure-data/_static/admin.png)

<span data-ttu-id="c1e01-315">Администратор имеет все привилегии.</span><span class="sxs-lookup"><span data-stu-id="c1e01-315">The administrator has all privileges.</span></span> <span data-ttu-id="c1e01-316">Она может чтения, изменить или удалить любой контакт и изменить состояние контактов.</span><span class="sxs-lookup"><span data-stu-id="c1e01-316">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="c1e01-317">Приложение было создано [формирования шаблонов](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) следующие `Contact` модели:</span><span class="sxs-lookup"><span data-stu-id="c1e01-317">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="c1e01-318">Пример содержит следующие обработчики авторизации:</span><span class="sxs-lookup"><span data-stu-id="c1e01-318">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="c1e01-319">`ContactIsOwnerAuthorizationHandler`: Гарантирует, что пользователь может изменять только свои данные.</span><span class="sxs-lookup"><span data-stu-id="c1e01-319">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="c1e01-320">`ContactManagerAuthorizationHandler`: Позволяет руководителям утверждать или отклонять контакты.</span><span class="sxs-lookup"><span data-stu-id="c1e01-320">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="c1e01-321">`ContactAdministratorsAuthorizationHandler`: Позволяет администраторам утверждать или отклонять контакты, а также изменять или удалять контакты.</span><span class="sxs-lookup"><span data-stu-id="c1e01-321">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c1e01-322">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="c1e01-322">Prerequisites</span></span>

<span data-ttu-id="c1e01-323">Этот учебник является дополнительным.</span><span class="sxs-lookup"><span data-stu-id="c1e01-323">This tutorial is advanced.</span></span> <span data-ttu-id="c1e01-324">Вы должны быть знакомы с:</span><span class="sxs-lookup"><span data-stu-id="c1e01-324">You should be familiar with:</span></span>

* [<span data-ttu-id="c1e01-325">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c1e01-325">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="c1e01-326">Authentication</span><span class="sxs-lookup"><span data-stu-id="c1e01-326">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="c1e01-327">Подтверждение учетной записи и восстановление пароля</span><span class="sxs-lookup"><span data-stu-id="c1e01-327">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="c1e01-328">Авторизация</span><span class="sxs-lookup"><span data-stu-id="c1e01-328">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="c1e01-329">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c1e01-329">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="c1e01-330">Начальный и завершенное приложение</span><span class="sxs-lookup"><span data-stu-id="c1e01-330">The starter and completed app</span></span>

<span data-ttu-id="c1e01-331">[Скачайте](xref:index#how-to-download-a-sample) [завершения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) приложения.</span><span class="sxs-lookup"><span data-stu-id="c1e01-331">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="c1e01-332">[Тест](#test-the-completed-app) готовое приложение, поэтому вам ознакомиться с его возможностями безопасности.</span><span class="sxs-lookup"><span data-stu-id="c1e01-332">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="c1e01-333">Приложение начального уровня</span><span class="sxs-lookup"><span data-stu-id="c1e01-333">The starter app</span></span>

<span data-ttu-id="c1e01-334">[Скачайте](xref:index#how-to-download-a-sample) [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) приложения.</span><span class="sxs-lookup"><span data-stu-id="c1e01-334">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="c1e01-335">Запустите приложение, коснитесь **ContactManager** связать и убедитесь, создание, изменение и удаление контакта.</span><span class="sxs-lookup"><span data-stu-id="c1e01-335">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="c1e01-336">Обеспечение безопасности данных пользователя</span><span class="sxs-lookup"><span data-stu-id="c1e01-336">Secure user data</span></span>

<span data-ttu-id="c1e01-337">В следующих разделах есть все основные шаги по созданию приложения данные безопасности пользователей.</span><span class="sxs-lookup"><span data-stu-id="c1e01-337">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="c1e01-338">Могут оказаться полезными для ссылки на готового проекта.</span><span class="sxs-lookup"><span data-stu-id="c1e01-338">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="c1e01-339">Привязать контактные данные для пользователя</span><span class="sxs-lookup"><span data-stu-id="c1e01-339">Tie the contact data to the user</span></span>

<span data-ttu-id="c1e01-340">Использование ASP.NET [удостоверений](xref:security/authentication/identity) идентификатор пользователя, чтобы предоставить пользователям можно изменить свои данные, но не данные других пользователей.</span><span class="sxs-lookup"><span data-stu-id="c1e01-340">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="c1e01-341">Добавить `OwnerID` и `ContactStatus` для `Contact` модели:</span><span class="sxs-lookup"><span data-stu-id="c1e01-341">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="c1e01-342">`OwnerID` — Это идентификатор пользователя из `AspNetUser` в таблицу [удостоверений](xref:security/authentication/identity) базы данных.</span><span class="sxs-lookup"><span data-stu-id="c1e01-342">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="c1e01-343">`Status` Поля определяет, является ли контакт для просмотра обычными пользователями.</span><span class="sxs-lookup"><span data-stu-id="c1e01-343">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="c1e01-344">Создание новой миграции и обновления базы данных:</span><span class="sxs-lookup"><span data-stu-id="c1e01-344">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="c1e01-345">Добавление служб роли удостоверению</span><span class="sxs-lookup"><span data-stu-id="c1e01-345">Add Role services to Identity</span></span>

<span data-ttu-id="c1e01-346">Добавление [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) для добавления служб ролей:</span><span class="sxs-lookup"><span data-stu-id="c1e01-346">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a><span data-ttu-id="c1e01-347">Требовать от пользователей, прошедших проверку подлинности</span><span class="sxs-lookup"><span data-stu-id="c1e01-347">Require authenticated users</span></span>

<span data-ttu-id="c1e01-348">Задайте политику проверки подлинности по умолчанию, чтобы требовать проверку подлинности пользователей:</span><span class="sxs-lookup"><span data-stu-id="c1e01-348">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 <span data-ttu-id="c1e01-349">Можно отказаться от проверки подлинности на уровне метода действия, контроллера или страницу Razor с `[AllowAnonymous]` атрибута.</span><span class="sxs-lookup"><span data-stu-id="c1e01-349">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="c1e01-350">Задание политики проверки подлинности по умолчанию требовать от пользователей пройти проверку подлинности защищает вновь добавленный Razor Pages и контроллеры.</span><span class="sxs-lookup"><span data-stu-id="c1e01-350">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="c1e01-351">Наличие требуется по умолчанию проверка подлинности безопаснее, чем полагаться на новые контроллеры и страницы Razor для включения `[Authorize]` атрибута.</span><span class="sxs-lookup"><span data-stu-id="c1e01-351">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="c1e01-352">Добавить [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) в индекс, со сведениями и контактными страницы, чтобы анонимные пользователи могут получить сведения о веб-узле, прежде чем они были зарегистрированы.</span><span class="sxs-lookup"><span data-stu-id="c1e01-352">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="c1e01-353">Настройка тестовой учетной записью</span><span class="sxs-lookup"><span data-stu-id="c1e01-353">Configure the test account</span></span>

<span data-ttu-id="c1e01-354">`SeedData` Класс создает две учетные записи: администратора и диспетчера.</span><span class="sxs-lookup"><span data-stu-id="c1e01-354">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="c1e01-355">Используйте [средство Secret Manager](xref:security/app-secrets) Установка пароля для этих учетных записей.</span><span class="sxs-lookup"><span data-stu-id="c1e01-355">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="c1e01-356">Задайте пароль из каталога проекта (каталог, содержащий *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="c1e01-356">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="c1e01-357">Если надежный пароль не указан, создается исключение при `SeedData.Initialize` вызывается.</span><span class="sxs-lookup"><span data-stu-id="c1e01-357">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="c1e01-358">Обновление `Main` используйте пароль теста:</span><span class="sxs-lookup"><span data-stu-id="c1e01-358">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="c1e01-359">Создание тестовых учетных записей и обновление контактов</span><span class="sxs-lookup"><span data-stu-id="c1e01-359">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="c1e01-360">Обновление `Initialize` метод в `SeedData` класса для создания тестовых учетных записей:</span><span class="sxs-lookup"><span data-stu-id="c1e01-360">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="c1e01-361">Добавьте идентификатор пользователя администратора и `ContactStatus` контактам.</span><span class="sxs-lookup"><span data-stu-id="c1e01-361">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="c1e01-362">Создать контактных лиц с «Отправлено» и одного «отклонено».</span><span class="sxs-lookup"><span data-stu-id="c1e01-362">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="c1e01-363">Добавьте идентификатор пользователя и состояние всех контактов.</span><span class="sxs-lookup"><span data-stu-id="c1e01-363">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="c1e01-364">Отображается только один контакт:</span><span class="sxs-lookup"><span data-stu-id="c1e01-364">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="c1e01-365">Создание владельца, manager и обработчики авторизации администратора</span><span class="sxs-lookup"><span data-stu-id="c1e01-365">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="c1e01-366">Создание `ContactIsOwnerAuthorizationHandler` в класс *авторизации* папки.</span><span class="sxs-lookup"><span data-stu-id="c1e01-366">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="c1e01-367">`ContactIsOwnerAuthorizationHandler` Проверяет, что пользователь, выполняющий ресурса, которому принадлежит ресурс.</span><span class="sxs-lookup"><span data-stu-id="c1e01-367">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="c1e01-368">`ContactIsOwnerAuthorizationHandler` Вызовы [контекста. Успешно](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) при связаться с владельцем текущего авторизованного пользователя.</span><span class="sxs-lookup"><span data-stu-id="c1e01-368">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="c1e01-369">Обработчики авторизации обычно:</span><span class="sxs-lookup"><span data-stu-id="c1e01-369">Authorization handlers generally:</span></span>

* <span data-ttu-id="c1e01-370">Вернуть `context.Succeed` при соблюдении требований.</span><span class="sxs-lookup"><span data-stu-id="c1e01-370">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="c1e01-371">Вернуть `Task.CompletedTask` Если требования не соблюдены.</span><span class="sxs-lookup"><span data-stu-id="c1e01-371">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="c1e01-372">`Task.CompletedTask`не является успешным или неудачным&mdash;, что позволяет запускать другие обработчики авторизации.</span><span class="sxs-lookup"><span data-stu-id="c1e01-372">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="c1e01-373">Если необходимо выполнить явную ошибку, возвратить [контекста. Сбой](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="c1e01-373">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="c1e01-374">Приложение позволяет контактные владельцам изменения или удаления и создания собственных данных.</span><span class="sxs-lookup"><span data-stu-id="c1e01-374">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="c1e01-375">`ContactIsOwnerAuthorizationHandler` не требуется для проверки операции, передаваемые в качестве параметра требование.</span><span class="sxs-lookup"><span data-stu-id="c1e01-375">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="c1e01-376">Создайте обработчик диспетчера авторизации</span><span class="sxs-lookup"><span data-stu-id="c1e01-376">Create a manager authorization handler</span></span>

<span data-ttu-id="c1e01-377">Создание `ContactManagerAuthorizationHandler` в класс *авторизации* папки.</span><span class="sxs-lookup"><span data-stu-id="c1e01-377">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="c1e01-378">`ContactManagerAuthorizationHandler` Проверяет пользователя, действующий от ресурса является руководителем.</span><span class="sxs-lookup"><span data-stu-id="c1e01-378">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="c1e01-379">Только менеджеры могли утверждать или отклонять изменения содержимого (новые или измененные).</span><span class="sxs-lookup"><span data-stu-id="c1e01-379">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="c1e01-380">Создайте обработчик авторизации администратора</span><span class="sxs-lookup"><span data-stu-id="c1e01-380">Create an administrator authorization handler</span></span>

<span data-ttu-id="c1e01-381">Создание `ContactAdministratorsAuthorizationHandler` в класс *авторизации* папки.</span><span class="sxs-lookup"><span data-stu-id="c1e01-381">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="c1e01-382">`ContactAdministratorsAuthorizationHandler` Проверяет действующий от ресурса пользователь является администратором.</span><span class="sxs-lookup"><span data-stu-id="c1e01-382">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="c1e01-383">Администратор может выполнить все операции.</span><span class="sxs-lookup"><span data-stu-id="c1e01-383">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="c1e01-384">Регистрировать обработчики авторизации</span><span class="sxs-lookup"><span data-stu-id="c1e01-384">Register the authorization handlers</span></span>

<span data-ttu-id="c1e01-385">Службы, с помощью Entity Framework Core должны быть зарегистрированы для [внедрения зависимостей](xref:fundamentals/dependency-injection) с помощью [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="c1e01-385">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="c1e01-386">`ContactIsOwnerAuthorizationHandler` Использует ASP.NET Core [удостоверений](xref:security/authentication/identity), которая опирается на Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c1e01-386">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="c1e01-387">Зарегистрировать обработчики с коллекцией, службы, поэтому они будут доступны для `ContactsController` через [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c1e01-387">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="c1e01-388">Добавьте следующий код в конец `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c1e01-388">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

<span data-ttu-id="c1e01-389">`ContactAdministratorsAuthorizationHandler` и `ContactManagerAuthorizationHandler` добавляются как одноэлементных экземпляров.</span><span class="sxs-lookup"><span data-stu-id="c1e01-389">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="c1e01-390">Они одноэлементных экземпляров, так как они не используют EF и все сведения, необходимые в `Context` параметр `HandleRequirementAsync` метод.</span><span class="sxs-lookup"><span data-stu-id="c1e01-390">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="c1e01-391">Поддержка авторизации</span><span class="sxs-lookup"><span data-stu-id="c1e01-391">Support authorization</span></span>

<span data-ttu-id="c1e01-392">В этом разделе вы обновите страницы Razor Pages и добавьте класс требования к операции.</span><span class="sxs-lookup"><span data-stu-id="c1e01-392">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="c1e01-393">Просмотрите класс требования контактные операций</span><span class="sxs-lookup"><span data-stu-id="c1e01-393">Review the contact operations requirements class</span></span>

<span data-ttu-id="c1e01-394">Просмотрите `ContactOperations` класса.</span><span class="sxs-lookup"><span data-stu-id="c1e01-394">Review the `ContactOperations` class.</span></span> <span data-ttu-id="c1e01-395">Этот класс содержит требования к поддерживает приложения:</span><span class="sxs-lookup"><span data-stu-id="c1e01-395">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="c1e01-396">Создать базовый класс для страниц Razor контактов</span><span class="sxs-lookup"><span data-stu-id="c1e01-396">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="c1e01-397">Создайте базовый класс, который содержит службы, используемые в Razor Pages контакты.</span><span class="sxs-lookup"><span data-stu-id="c1e01-397">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="c1e01-398">Базовый класс помещает код инициализации в одном месте:</span><span class="sxs-lookup"><span data-stu-id="c1e01-398">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="c1e01-399">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="c1e01-399">The preceding code:</span></span>

* <span data-ttu-id="c1e01-400">Добавляет `IAuthorizationService` службы для доступа к обработчикам авторизации.</span><span class="sxs-lookup"><span data-stu-id="c1e01-400">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="c1e01-401">Добавляет удостоверение `UserManager` службы.</span><span class="sxs-lookup"><span data-stu-id="c1e01-401">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="c1e01-402">Добавьте `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="c1e01-402">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="c1e01-403">Обновление CreateModel</span><span class="sxs-lookup"><span data-stu-id="c1e01-403">Update the CreateModel</span></span>

<span data-ttu-id="c1e01-404">Измените конструктор модель страницы create для использования `DI_BasePageModel` базового класса:</span><span class="sxs-lookup"><span data-stu-id="c1e01-404">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="c1e01-405">Обновление `CreateModel.OnPostAsync` метод:</span><span class="sxs-lookup"><span data-stu-id="c1e01-405">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="c1e01-406">Добавьте идентификатор пользователя, `Contact` модели.</span><span class="sxs-lookup"><span data-stu-id="c1e01-406">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="c1e01-407">Вызовите обработчик авторизации, чтобы убедиться, что пользователь имеет разрешение на создание контактов.</span><span class="sxs-lookup"><span data-stu-id="c1e01-407">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="c1e01-408">Обновление IndexModel</span><span class="sxs-lookup"><span data-stu-id="c1e01-408">Update the IndexModel</span></span>

<span data-ttu-id="c1e01-409">Обновление `OnGetAsync` метод, поэтому отображаются только утвержденные контактов для обычных пользователей:</span><span class="sxs-lookup"><span data-stu-id="c1e01-409">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="c1e01-410">Обновление EditModel</span><span class="sxs-lookup"><span data-stu-id="c1e01-410">Update the EditModel</span></span>

<span data-ttu-id="c1e01-411">Добавление обработчика авторизации, чтобы убедиться, что пользователь является владельцем контакта.</span><span class="sxs-lookup"><span data-stu-id="c1e01-411">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="c1e01-412">Так как авторизации ресурсов проверяется, `[Authorize]` атрибут будет недостаточно.</span><span class="sxs-lookup"><span data-stu-id="c1e01-412">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="c1e01-413">Приложение не имеет доступа к ресурсу, при оценке атрибуты.</span><span class="sxs-lookup"><span data-stu-id="c1e01-413">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="c1e01-414">Авторизация на основе ресурсов должен быть явный.</span><span class="sxs-lookup"><span data-stu-id="c1e01-414">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="c1e01-415">Проверки должны выполняться, когда приложение имеет доступ к ресурсу, загрузив его в модели страниц или, загрузив его в сам обработчик.</span><span class="sxs-lookup"><span data-stu-id="c1e01-415">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="c1e01-416">Вы обращаетесь чаще всего ресурса, передавая ключ ресурса.</span><span class="sxs-lookup"><span data-stu-id="c1e01-416">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="c1e01-417">Обновление DeleteModel</span><span class="sxs-lookup"><span data-stu-id="c1e01-417">Update the DeleteModel</span></span>

<span data-ttu-id="c1e01-418">Обновите модель страницы delete на использование обработчика авторизации, чтобы убедиться, что пользователь имеет разрешение на удаление для контакта.</span><span class="sxs-lookup"><span data-stu-id="c1e01-418">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="c1e01-419">Внедрить службы авторизации в представления</span><span class="sxs-lookup"><span data-stu-id="c1e01-419">Inject the authorization service into the views</span></span>

<span data-ttu-id="c1e01-420">В настоящее время в пользовательском Интерфейсе показано изменение и удаление ссылок для контактов, которые не могут изменяться.</span><span class="sxs-lookup"><span data-stu-id="c1e01-420">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="c1e01-421">Внедрить службы авторизации в *Views/_ViewImports.cshtml* файл, чтобы он был доступен для всех представлений:</span><span class="sxs-lookup"><span data-stu-id="c1e01-421">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="c1e01-422">Предыдущая разметка добавляет некоторые `using` инструкций.</span><span class="sxs-lookup"><span data-stu-id="c1e01-422">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="c1e01-423">Обновление **изменить** и **удалить** ссылок в *Pages/Contacts/Index.cshtml* , обработкой только для пользователей с соответствующими разрешениями:</span><span class="sxs-lookup"><span data-stu-id="c1e01-423">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="c1e01-424">Скрытие ссылок от пользователей, у которых нет разрешения на изменение данных не защитить приложения.</span><span class="sxs-lookup"><span data-stu-id="c1e01-424">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="c1e01-425">Скрытие ссылок делает приложение более удобным, отображая только допустимые ссылки.</span><span class="sxs-lookup"><span data-stu-id="c1e01-425">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="c1e01-426">Пользователи могут взломать созданный URL-адреса для вызова редактирования и удаления данных, которыми они не владеют.</span><span class="sxs-lookup"><span data-stu-id="c1e01-426">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="c1e01-427">На странице Razor или контроллер должен Принудительная проверка доступа для защиты данных.</span><span class="sxs-lookup"><span data-stu-id="c1e01-427">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="c1e01-428">Дополнительные сведения об обновлении</span><span class="sxs-lookup"><span data-stu-id="c1e01-428">Update Details</span></span>

<span data-ttu-id="c1e01-429">Обновление представления сведений, чтобы менеджеры могли утверждать или отклонять контактов:</span><span class="sxs-lookup"><span data-stu-id="c1e01-429">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="c1e01-430">Обновите модель страницы сведений:</span><span class="sxs-lookup"><span data-stu-id="c1e01-430">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="c1e01-431">Добавление или удаление пользователя к роли</span><span class="sxs-lookup"><span data-stu-id="c1e01-431">Add or remove a user to a role</span></span>

<span data-ttu-id="c1e01-432">См. в разделе [эту проблему](https://github.com/aspnet/AspNetCore.Docs/issues/8502) сведения о:</span><span class="sxs-lookup"><span data-stu-id="c1e01-432">See [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="c1e01-433">Удаление привилегий пользователя.</span><span class="sxs-lookup"><span data-stu-id="c1e01-433">Removing privileges from a user.</span></span> <span data-ttu-id="c1e01-434">Например, отзвука пользователя в приложении разговора.</span><span class="sxs-lookup"><span data-stu-id="c1e01-434">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="c1e01-435">Добавление прав к пользователю.</span><span class="sxs-lookup"><span data-stu-id="c1e01-435">Adding privileges to a user.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="c1e01-436">Тестирование завершенного приложения</span><span class="sxs-lookup"><span data-stu-id="c1e01-436">Test the completed app</span></span>

<span data-ttu-id="c1e01-437">Если вы еще не настроили пароль для учетных записей пользователей на основании добавляемых, использовать [средство Secret Manager](xref:security/app-secrets#secret-manager) Установка пароля:</span><span class="sxs-lookup"><span data-stu-id="c1e01-437">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="c1e01-438">Выберите надежный пароль: Используйте восемь и более символов и по крайней мере одну прописную букву, цифру и символ.</span><span class="sxs-lookup"><span data-stu-id="c1e01-438">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="c1e01-439">Например `Passw0rd!` требованиям надежный пароль.</span><span class="sxs-lookup"><span data-stu-id="c1e01-439">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="c1e01-440">Выполните следующую команду из папки проекта, где `<PW>` пароль:</span><span class="sxs-lookup"><span data-stu-id="c1e01-440">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```console
  dotnet user-secrets set SeedUserPW <PW>
  ```

* <span data-ttu-id="c1e01-441">Удаление и обновление базы данных</span><span class="sxs-lookup"><span data-stu-id="c1e01-441">Drop and update the Database</span></span>

    ```console
     dotnet ef database drop -f
     dotnet ef database update  
     ```

* <span data-ttu-id="c1e01-442">Перезапустите приложение, чтобы заполнить базу данных.</span><span class="sxs-lookup"><span data-stu-id="c1e01-442">Restart the app to seed the database.</span></span>

<span data-ttu-id="c1e01-443">Простой способ протестировать готовое приложение — запуск три различных браузеров (или инкогнито или InPrivate сеансов).</span><span class="sxs-lookup"><span data-stu-id="c1e01-443">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="c1e01-444">В один браузер, регистрация нового пользователя (например, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="c1e01-444">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="c1e01-445">Войдите в каждом браузере с другим пользователем.</span><span class="sxs-lookup"><span data-stu-id="c1e01-445">Sign in to each browser with a different user.</span></span> <span data-ttu-id="c1e01-446">Проверьте следующие операции:</span><span class="sxs-lookup"><span data-stu-id="c1e01-446">Verify the following operations:</span></span>

* <span data-ttu-id="c1e01-447">Зарегистрированные пользователи могут просматривать все утвержденные контактные данные.</span><span class="sxs-lookup"><span data-stu-id="c1e01-447">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="c1e01-448">Зарегистрированным пользователям может изменять и удалять свои собственные данные.</span><span class="sxs-lookup"><span data-stu-id="c1e01-448">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="c1e01-449">Руководители могут утверждать или отклонять контактных данных.</span><span class="sxs-lookup"><span data-stu-id="c1e01-449">Managers can approve/reject contact data.</span></span> <span data-ttu-id="c1e01-450">`Details` Просмотра показано **утвердить** и **Отклонить** кнопки.</span><span class="sxs-lookup"><span data-stu-id="c1e01-450">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="c1e01-451">Администраторы могут утверждать или отклонять и изменить или удалить все данные.</span><span class="sxs-lookup"><span data-stu-id="c1e01-451">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="c1e01-452">Пользовательская</span><span class="sxs-lookup"><span data-stu-id="c1e01-452">User</span></span>                | <span data-ttu-id="c1e01-453">Заполняется данными в приложении</span><span class="sxs-lookup"><span data-stu-id="c1e01-453">Seeded by the app</span></span> | <span data-ttu-id="c1e01-454">Параметры</span><span class="sxs-lookup"><span data-stu-id="c1e01-454">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="c1e01-455">Нет</span><span class="sxs-lookup"><span data-stu-id="c1e01-455">No</span></span>                | <span data-ttu-id="c1e01-456">Изменить или удалить данные принадлежат.</span><span class="sxs-lookup"><span data-stu-id="c1e01-456">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="c1e01-457">Да</span><span class="sxs-lookup"><span data-stu-id="c1e01-457">Yes</span></span>               | <span data-ttu-id="c1e01-458">Утверждать или отклонять и изменить или удалить данные принадлежат.</span><span class="sxs-lookup"><span data-stu-id="c1e01-458">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="c1e01-459">Да</span><span class="sxs-lookup"><span data-stu-id="c1e01-459">Yes</span></span>               | <span data-ttu-id="c1e01-460">Утверждать или отклонять и изменить или удалить все данные.</span><span class="sxs-lookup"><span data-stu-id="c1e01-460">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="c1e01-461">Создание контакта в браузере администратора.</span><span class="sxs-lookup"><span data-stu-id="c1e01-461">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="c1e01-462">Скопируйте URL-адрес для удаления и редактирование откуда обратитесь к администратору.</span><span class="sxs-lookup"><span data-stu-id="c1e01-462">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="c1e01-463">Вставьте эти ссылки в браузере пользователя теста, чтобы убедиться, что тестовый пользователь не может выполнять эти операции.</span><span class="sxs-lookup"><span data-stu-id="c1e01-463">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="c1e01-464">Создать приложение начального уровня</span><span class="sxs-lookup"><span data-stu-id="c1e01-464">Create the starter app</span></span>

* <span data-ttu-id="c1e01-465">Создание приложения Razor Pages с именем «ContactManager»</span><span class="sxs-lookup"><span data-stu-id="c1e01-465">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="c1e01-466">Создание приложения с **отдельным учетным записям пользователей**.</span><span class="sxs-lookup"><span data-stu-id="c1e01-466">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="c1e01-467">Назовите его «ContactManager», пространство имен соответствует пространству имен, используемые в образце.</span><span class="sxs-lookup"><span data-stu-id="c1e01-467">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="c1e01-468">`-uld` Указывает LocalDB вместо SQLite</span><span class="sxs-lookup"><span data-stu-id="c1e01-468">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="c1e01-469">Добавление *моделей/Contact. CS*:</span><span class="sxs-lookup"><span data-stu-id="c1e01-469">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="c1e01-470">Каркас `Contact` модели.</span><span class="sxs-lookup"><span data-stu-id="c1e01-470">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="c1e01-471">Создание первоначальной миграции и обновления базы данных:</span><span class="sxs-lookup"><span data-stu-id="c1e01-471">Create initial migration and update the database:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
  dotnet ef database drop -f
  dotnet ef migrations add initial
  dotnet ef database update
  ```

* <span data-ttu-id="c1e01-472">Обновление **ContactManager** привязки в *Pages/_Layout.cshtml* файла:</span><span class="sxs-lookup"><span data-stu-id="c1e01-472">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

  ```cshtml
  <a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
  ```

* <span data-ttu-id="c1e01-473">Протестируйте приложение, создание, изменение и удаление контакта</span><span class="sxs-lookup"><span data-stu-id="c1e01-473">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="c1e01-474">Заполнение базы данных</span><span class="sxs-lookup"><span data-stu-id="c1e01-474">Seed the database</span></span>

<span data-ttu-id="c1e01-475">Добавить [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) класс *данных* папки.</span><span class="sxs-lookup"><span data-stu-id="c1e01-475">Add the [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) class to the *Data* folder.</span></span>

<span data-ttu-id="c1e01-476">Вызовите `SeedData.Initialize` из `Main`:</span><span class="sxs-lookup"><span data-stu-id="c1e01-476">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

<span data-ttu-id="c1e01-477">Проверьте, что приложение заполняется данными базы данных.</span><span class="sxs-lookup"><span data-stu-id="c1e01-477">Test that the app seeded the database.</span></span> <span data-ttu-id="c1e01-478">При возникновении любых строк в контакте DB, не запускается метод заполнения.</span><span class="sxs-lookup"><span data-stu-id="c1e01-478">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="c1e01-479">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="c1e01-479">Additional resources</span></span>

* [<span data-ttu-id="c1e01-480">Создание веб-приложения .NET Core с базой данных SQL в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="c1e01-480">Build a .NET Core and SQL Database web app in Azure App Service</span></span>](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* <span data-ttu-id="c1e01-481">[ASP.NET Core авторизация лаборатории](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="c1e01-481">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="c1e01-482">Это лабораторное занятие содержит более подробные сведения о функциях безопасности, представленных в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="c1e01-482">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* <xref:security/authorization/introduction>
* [<span data-ttu-id="c1e01-483">Пользовательская авторизация на основе политик</span><span class="sxs-lookup"><span data-stu-id="c1e01-483">Custom policy-based authorization</span></span>](xref:security/authorization/policies)
