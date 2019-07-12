---
title: Создание приложения ASP.NET Core с помощью данных пользователя с помощью авторизации
author: rick-anderson
description: Узнайте, как создать приложение Razor Pages с помощью данных пользователя с помощью авторизации. Включает протокол HTTPS, проверка подлинности и безопасность, удостоверения ASP.NET Core.
ms.author: riande
ms.date: 12/18/2018
ms.custom: mvc, seodec18
uid: security/authorization/secure-data
ms.openlocfilehash: 222ae1d6212b838e5c70f831960fa23a9924a0ae
ms.sourcegitcommit: 7a40c56bf6a6aaa63a7ee83a2cac9b3a1d77555e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/12/2019
ms.locfileid: "67856140"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="56f48-104">Создание приложения ASP.NET Core с помощью данных пользователя с помощью авторизации</span><span class="sxs-lookup"><span data-stu-id="56f48-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="56f48-105">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Джо Одетт](https://twitter.com/joeaudette) (Joe Audette)</span><span class="sxs-lookup"><span data-stu-id="56f48-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="56f48-106">См. в разделе [этот PDF-ФАЙЛ](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) для версии ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="56f48-106">See [this PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="56f48-107">Версия ASP.NET Core 1.1 работы с этим руководством, [это](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) папки.</span><span class="sxs-lookup"><span data-stu-id="56f48-107">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="56f48-108">1\.1, пример ASP.NET Core находится в [примеры](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="56f48-108">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="56f48-109">См. в разделе [этот PDF-файл](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span><span class="sxs-lookup"><span data-stu-id="56f48-109">See [this pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="56f48-110">Этом руководстве показано, как создать веб-приложение ASP.NET Core с помощью данных пользователя с помощью авторизации.</span><span class="sxs-lookup"><span data-stu-id="56f48-110">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="56f48-111">Отображается список контактов, прошедшие проверку подлинности (зарегистрированного) пользователи создали.</span><span class="sxs-lookup"><span data-stu-id="56f48-111">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="56f48-112">Существует три группы безопасности:</span><span class="sxs-lookup"><span data-stu-id="56f48-112">There are three security groups:</span></span>

* <span data-ttu-id="56f48-113">**Зарегистрированные пользователи** может просматривать все данные, утвержденных и может изменять и удалять свои собственные данные.</span><span class="sxs-lookup"><span data-stu-id="56f48-113">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="56f48-114">**Диспетчеры** можно утвердить или отклонить контактных данных.</span><span class="sxs-lookup"><span data-stu-id="56f48-114">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="56f48-115">Только утвержденные контакты отображаются для пользователей.</span><span class="sxs-lookup"><span data-stu-id="56f48-115">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="56f48-116">**Администраторы** можно утверждать или отклонять и изменить или удалить все данные.</span><span class="sxs-lookup"><span data-stu-id="56f48-116">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="56f48-117">Изображения в этом документе не совпадать последние шаблоны.</span><span class="sxs-lookup"><span data-stu-id="56f48-117">The images in this document exactly don't match the latest templates.</span></span>

<span data-ttu-id="56f48-118">На следующем рисунке, пользователь Рик (`rick@example.com`) выполнил вход.</span><span class="sxs-lookup"><span data-stu-id="56f48-118">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="56f48-119">Рик могут только просматривать утвержденные контакты и **изменить**/**удалить**/**Create New** ссылки на свои контакты.</span><span class="sxs-lookup"><span data-stu-id="56f48-119">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="56f48-120">Только последней записи, созданные Рик, отображает **изменить** и **удалить** ссылки.</span><span class="sxs-lookup"><span data-stu-id="56f48-120">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="56f48-121">Другие пользователи увидят последнюю запись, после менеджеру или администратору изменяет состояние на «Утверждено».</span><span class="sxs-lookup"><span data-stu-id="56f48-121">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Снимок экрана, показывающий Рик входа](secure-data/_static/rick.png)

<span data-ttu-id="56f48-123">На следующем рисунке `manager@contoso.com` подписан в и в роль диспетчера:</span><span class="sxs-lookup"><span data-stu-id="56f48-123">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![Снимок экрана, показывающий manager@contoso.com в системе](secure-data/_static/manager1.png)

<span data-ttu-id="56f48-125">На следующем рисунке показана менеджеров Просмотр подробностей контакта:</span><span class="sxs-lookup"><span data-stu-id="56f48-125">The following image shows the managers details view of a contact:</span></span>

![Представление руководителя контакта](secure-data/_static/manager.png)

<span data-ttu-id="56f48-127">**Утвердить** и **Отклонить** кнопки отображаются только для менеджеров и администраторов.</span><span class="sxs-lookup"><span data-stu-id="56f48-127">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="56f48-128">На следующем рисунке `admin@contoso.com` подписан в и в роли администратора:</span><span class="sxs-lookup"><span data-stu-id="56f48-128">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![Снимок экрана, показывающий admin@contoso.com в системе](secure-data/_static/admin.png)

<span data-ttu-id="56f48-130">Администратор имеет все привилегии.</span><span class="sxs-lookup"><span data-stu-id="56f48-130">The administrator has all privileges.</span></span> <span data-ttu-id="56f48-131">Она может чтения, изменить или удалить любой контакт и изменить состояние контактов.</span><span class="sxs-lookup"><span data-stu-id="56f48-131">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="56f48-132">Приложение было создано [формирования шаблонов](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) следующие `Contact` модели:</span><span class="sxs-lookup"><span data-stu-id="56f48-132">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="56f48-133">Пример содержит следующие обработчики авторизации:</span><span class="sxs-lookup"><span data-stu-id="56f48-133">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="56f48-134">`ContactIsOwnerAuthorizationHandler`: Гарантирует, что пользователь может изменить только свои данные.</span><span class="sxs-lookup"><span data-stu-id="56f48-134">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="56f48-135">`ContactManagerAuthorizationHandler`: Менеджеры для утверждения или отклонения контактов.</span><span class="sxs-lookup"><span data-stu-id="56f48-135">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="56f48-136">`ContactAdministratorsAuthorizationHandler`: Администраторы могут утверждать или отклонять контакты и изменить или удалить контакты.</span><span class="sxs-lookup"><span data-stu-id="56f48-136">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="56f48-137">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="56f48-137">Prerequisites</span></span>

<span data-ttu-id="56f48-138">Этот учебник является дополнительным.</span><span class="sxs-lookup"><span data-stu-id="56f48-138">This tutorial is advanced.</span></span> <span data-ttu-id="56f48-139">Вы должны быть знакомы с:</span><span class="sxs-lookup"><span data-stu-id="56f48-139">You should be familiar with:</span></span>

* [<span data-ttu-id="56f48-140">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="56f48-140">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="56f48-141">Authentication</span><span class="sxs-lookup"><span data-stu-id="56f48-141">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="56f48-142">Подтверждение учетной записи и восстановление пароля</span><span class="sxs-lookup"><span data-stu-id="56f48-142">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="56f48-143">Авторизация</span><span class="sxs-lookup"><span data-stu-id="56f48-143">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="56f48-144">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="56f48-144">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="56f48-145">Начальный и завершенное приложение</span><span class="sxs-lookup"><span data-stu-id="56f48-145">The starter and completed app</span></span>

<span data-ttu-id="56f48-146">[Скачайте](xref:index#how-to-download-a-sample) [завершения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) приложения.</span><span class="sxs-lookup"><span data-stu-id="56f48-146">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="56f48-147">[Тест](#test-the-completed-app) готовое приложение, поэтому вам ознакомиться с его возможностями безопасности.</span><span class="sxs-lookup"><span data-stu-id="56f48-147">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="56f48-148">Приложение начального уровня</span><span class="sxs-lookup"><span data-stu-id="56f48-148">The starter app</span></span>

<span data-ttu-id="56f48-149">[Скачайте](xref:index#how-to-download-a-sample) [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) приложения.</span><span class="sxs-lookup"><span data-stu-id="56f48-149">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="56f48-150">Запустите приложение, коснитесь **ContactManager** связать и убедитесь, создание, изменение и удаление контакта.</span><span class="sxs-lookup"><span data-stu-id="56f48-150">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="56f48-151">Обеспечение безопасности данных пользователя</span><span class="sxs-lookup"><span data-stu-id="56f48-151">Secure user data</span></span>

<span data-ttu-id="56f48-152">В следующих разделах есть все основные шаги по созданию приложения данные безопасности пользователей.</span><span class="sxs-lookup"><span data-stu-id="56f48-152">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="56f48-153">Могут оказаться полезными для ссылки на готового проекта.</span><span class="sxs-lookup"><span data-stu-id="56f48-153">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="56f48-154">Привязать контактные данные для пользователя</span><span class="sxs-lookup"><span data-stu-id="56f48-154">Tie the contact data to the user</span></span>

<span data-ttu-id="56f48-155">Использование ASP.NET [удостоверений](xref:security/authentication/identity) идентификатор пользователя, чтобы предоставить пользователям можно изменить свои данные, но не данные других пользователей.</span><span class="sxs-lookup"><span data-stu-id="56f48-155">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="56f48-156">Добавить `OwnerID` и `ContactStatus` для `Contact` модели:</span><span class="sxs-lookup"><span data-stu-id="56f48-156">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final3/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="56f48-157">`OwnerID` — Это идентификатор пользователя из `AspNetUser` в таблицу [удостоверений](xref:security/authentication/identity) базы данных.</span><span class="sxs-lookup"><span data-stu-id="56f48-157">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="56f48-158">`Status` Поля определяет, является ли контакт для просмотра обычными пользователями.</span><span class="sxs-lookup"><span data-stu-id="56f48-158">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="56f48-159">Создание новой миграции и обновления базы данных:</span><span class="sxs-lookup"><span data-stu-id="56f48-159">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="56f48-160">Добавление служб роли удостоверению</span><span class="sxs-lookup"><span data-stu-id="56f48-160">Add Role services to Identity</span></span>

<span data-ttu-id="56f48-161">Добавление [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) для добавления служб ролей:</span><span class="sxs-lookup"><span data-stu-id="56f48-161">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet2&highlight=9)]

### <a name="require-authenticated-users"></a><span data-ttu-id="56f48-162">Требовать от пользователей, прошедших проверку подлинности</span><span class="sxs-lookup"><span data-stu-id="56f48-162">Require authenticated users</span></span>

<span data-ttu-id="56f48-163">Задайте политику проверки подлинности по умолчанию, чтобы требовать проверку подлинности пользователей:</span><span class="sxs-lookup"><span data-stu-id="56f48-163">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet&highlight=15-99)] 

 <span data-ttu-id="56f48-164">Можно отказаться от проверки подлинности на уровне метода действия, контроллера или страницу Razor с `[AllowAnonymous]` атрибута.</span><span class="sxs-lookup"><span data-stu-id="56f48-164">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="56f48-165">Задание политики проверки подлинности по умолчанию требовать от пользователей пройти проверку подлинности защищает вновь добавленный Razor Pages и контроллеры.</span><span class="sxs-lookup"><span data-stu-id="56f48-165">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="56f48-166">Наличие требуется по умолчанию проверка подлинности безопаснее, чем полагаться на новые контроллеры и страницы Razor для включения `[Authorize]` атрибута.</span><span class="sxs-lookup"><span data-stu-id="56f48-166">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="56f48-167">Добавить [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) на страницы индекса и конфиденциальности, чтобы анонимные пользователи могут получить сведения о веб-узле, прежде чем они были зарегистрированы.</span><span class="sxs-lookup"><span data-stu-id="56f48-167">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index and Privacy pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Index.cshtml.cs?highlight=1,7)]

### <a name="configure-the-test-account"></a><span data-ttu-id="56f48-168">Настройка тестовой учетной записью</span><span class="sxs-lookup"><span data-stu-id="56f48-168">Configure the test account</span></span>

<span data-ttu-id="56f48-169">`SeedData` Класс создает две учетные записи: администратора и диспетчера.</span><span class="sxs-lookup"><span data-stu-id="56f48-169">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="56f48-170">Используйте [средство Secret Manager](xref:security/app-secrets) Установка пароля для этих учетных записей.</span><span class="sxs-lookup"><span data-stu-id="56f48-170">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="56f48-171">Задайте пароль из каталога проекта (каталог, содержащий *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="56f48-171">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="56f48-172">Если надежный пароль не указан, создается исключение при `SeedData.Initialize` вызывается.</span><span class="sxs-lookup"><span data-stu-id="56f48-172">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="56f48-173">Обновление `Main` используйте пароль теста:</span><span class="sxs-lookup"><span data-stu-id="56f48-173">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final3/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="56f48-174">Создание тестовых учетных записей и обновление контактов</span><span class="sxs-lookup"><span data-stu-id="56f48-174">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="56f48-175">Обновление `Initialize` метод в `SeedData` класса для создания тестовых учетных записей:</span><span class="sxs-lookup"><span data-stu-id="56f48-175">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="56f48-176">Добавьте идентификатор пользователя администратора и `ContactStatus` контактам.</span><span class="sxs-lookup"><span data-stu-id="56f48-176">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="56f48-177">Создать контактных лиц с «Отправлено» и одного «отклонено».</span><span class="sxs-lookup"><span data-stu-id="56f48-177">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="56f48-178">Добавьте идентификатор пользователя и состояние всех контактов.</span><span class="sxs-lookup"><span data-stu-id="56f48-178">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="56f48-179">Отображается только один контакт:</span><span class="sxs-lookup"><span data-stu-id="56f48-179">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="56f48-180">Создание владельца, manager и обработчики авторизации администратора</span><span class="sxs-lookup"><span data-stu-id="56f48-180">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="56f48-181">Создание `ContactIsOwnerAuthorizationHandler` в класс *авторизации* папки.</span><span class="sxs-lookup"><span data-stu-id="56f48-181">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="56f48-182">`ContactIsOwnerAuthorizationHandler` Проверяет, что пользователь, выполняющий ресурса, которому принадлежит ресурс.</span><span class="sxs-lookup"><span data-stu-id="56f48-182">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="56f48-183">`ContactIsOwnerAuthorizationHandler` Вызовы [контекста. Успешно](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) при связаться с владельцем текущего авторизованного пользователя.</span><span class="sxs-lookup"><span data-stu-id="56f48-183">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="56f48-184">Обработчики авторизации обычно:</span><span class="sxs-lookup"><span data-stu-id="56f48-184">Authorization handlers generally:</span></span>

* <span data-ttu-id="56f48-185">Вернуть `context.Succeed` при соблюдении требований.</span><span class="sxs-lookup"><span data-stu-id="56f48-185">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="56f48-186">Вернуть `Task.CompletedTask` Если требования не соблюдены.</span><span class="sxs-lookup"><span data-stu-id="56f48-186">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="56f48-187">`Task.CompletedTask` не является об успехе или неудаче&mdash;позволяет другим обработчикам авторизации для выполнения.</span><span class="sxs-lookup"><span data-stu-id="56f48-187">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="56f48-188">Если необходимо выполнить явную ошибку, возвратить [контекста. Сбой](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="56f48-188">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="56f48-189">Приложение позволяет контактные владельцам изменения или удаления и создания собственных данных.</span><span class="sxs-lookup"><span data-stu-id="56f48-189">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="56f48-190">`ContactIsOwnerAuthorizationHandler` не требуется для проверки операции, передаваемые в качестве параметра требование.</span><span class="sxs-lookup"><span data-stu-id="56f48-190">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="56f48-191">Создайте обработчик диспетчера авторизации</span><span class="sxs-lookup"><span data-stu-id="56f48-191">Create a manager authorization handler</span></span>

<span data-ttu-id="56f48-192">Создание `ContactManagerAuthorizationHandler` в класс *авторизации* папки.</span><span class="sxs-lookup"><span data-stu-id="56f48-192">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="56f48-193">`ContactManagerAuthorizationHandler` Проверяет пользователя, действующий от ресурса является руководителем.</span><span class="sxs-lookup"><span data-stu-id="56f48-193">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="56f48-194">Только менеджеры могли утверждать или отклонять изменения содержимого (новые или измененные).</span><span class="sxs-lookup"><span data-stu-id="56f48-194">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="56f48-195">Создайте обработчик авторизации администратора</span><span class="sxs-lookup"><span data-stu-id="56f48-195">Create an administrator authorization handler</span></span>

<span data-ttu-id="56f48-196">Создание `ContactAdministratorsAuthorizationHandler` в класс *авторизации* папки.</span><span class="sxs-lookup"><span data-stu-id="56f48-196">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="56f48-197">`ContactAdministratorsAuthorizationHandler` Проверяет действующий от ресурса пользователь является администратором.</span><span class="sxs-lookup"><span data-stu-id="56f48-197">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="56f48-198">Администратор может выполнить все операции.</span><span class="sxs-lookup"><span data-stu-id="56f48-198">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="56f48-199">Регистрировать обработчики авторизации</span><span class="sxs-lookup"><span data-stu-id="56f48-199">Register the authorization handlers</span></span>

<span data-ttu-id="56f48-200">Службы, с помощью Entity Framework Core должны быть зарегистрированы для [внедрения зависимостей](xref:fundamentals/dependency-injection) с помощью [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="56f48-200">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="56f48-201">`ContactIsOwnerAuthorizationHandler` Использует ASP.NET Core [удостоверений](xref:security/authentication/identity), которая опирается на Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="56f48-201">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="56f48-202">Зарегистрировать обработчики с коллекцией, службы, поэтому они будут доступны для `ContactsController` через [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="56f48-202">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="56f48-203">Добавьте следующий код в конец `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="56f48-203">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet_defaultPolicy&highlight=23-99)]

<span data-ttu-id="56f48-204">`ContactAdministratorsAuthorizationHandler` и `ContactManagerAuthorizationHandler` добавляются как одноэлементных экземпляров.</span><span class="sxs-lookup"><span data-stu-id="56f48-204">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="56f48-205">Они одноэлементных экземпляров, так как они не используют EF и все сведения, необходимые в `Context` параметр `HandleRequirementAsync` метод.</span><span class="sxs-lookup"><span data-stu-id="56f48-205">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="56f48-206">Поддержка авторизации</span><span class="sxs-lookup"><span data-stu-id="56f48-206">Support authorization</span></span>

<span data-ttu-id="56f48-207">В этом разделе вы обновите страницы Razor Pages и добавьте класс требования к операции.</span><span class="sxs-lookup"><span data-stu-id="56f48-207">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="56f48-208">Просмотрите класс требования контактные операций</span><span class="sxs-lookup"><span data-stu-id="56f48-208">Review the contact operations requirements class</span></span>

<span data-ttu-id="56f48-209">Просмотрите `ContactOperations` класса.</span><span class="sxs-lookup"><span data-stu-id="56f48-209">Review the `ContactOperations` class.</span></span> <span data-ttu-id="56f48-210">Этот класс содержит требования к поддерживает приложения:</span><span class="sxs-lookup"><span data-stu-id="56f48-210">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="56f48-211">Создать базовый класс для страниц Razor контактов</span><span class="sxs-lookup"><span data-stu-id="56f48-211">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="56f48-212">Создайте базовый класс, который содержит службы, используемые в Razor Pages контакты.</span><span class="sxs-lookup"><span data-stu-id="56f48-212">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="56f48-213">Базовый класс помещает код инициализации в одном месте:</span><span class="sxs-lookup"><span data-stu-id="56f48-213">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="56f48-214">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="56f48-214">The preceding code:</span></span>

* <span data-ttu-id="56f48-215">Добавляет `IAuthorizationService` службы для доступа к обработчикам авторизации.</span><span class="sxs-lookup"><span data-stu-id="56f48-215">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="56f48-216">Добавляет удостоверение `UserManager` службы.</span><span class="sxs-lookup"><span data-stu-id="56f48-216">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="56f48-217">Добавьте `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="56f48-217">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="56f48-218">Обновление CreateModel</span><span class="sxs-lookup"><span data-stu-id="56f48-218">Update the CreateModel</span></span>

<span data-ttu-id="56f48-219">Измените конструктор модель страницы create для использования `DI_BasePageModel` базового класса:</span><span class="sxs-lookup"><span data-stu-id="56f48-219">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="56f48-220">Обновление `CreateModel.OnPostAsync` метод:</span><span class="sxs-lookup"><span data-stu-id="56f48-220">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="56f48-221">Добавьте идентификатор пользователя, `Contact` модели.</span><span class="sxs-lookup"><span data-stu-id="56f48-221">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="56f48-222">Вызовите обработчик авторизации, чтобы убедиться, что пользователь имеет разрешение на создание контактов.</span><span class="sxs-lookup"><span data-stu-id="56f48-222">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="56f48-223">Обновление IndexModel</span><span class="sxs-lookup"><span data-stu-id="56f48-223">Update the IndexModel</span></span>

<span data-ttu-id="56f48-224">Обновление `OnGetAsync` метод, поэтому отображаются только утвержденные контактов для обычных пользователей:</span><span class="sxs-lookup"><span data-stu-id="56f48-224">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="56f48-225">Обновление EditModel</span><span class="sxs-lookup"><span data-stu-id="56f48-225">Update the EditModel</span></span>

<span data-ttu-id="56f48-226">Добавление обработчика авторизации, чтобы убедиться, что пользователь является владельцем контакта.</span><span class="sxs-lookup"><span data-stu-id="56f48-226">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="56f48-227">Так как авторизации ресурсов проверяется, `[Authorize]` атрибут будет недостаточно.</span><span class="sxs-lookup"><span data-stu-id="56f48-227">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="56f48-228">Приложение не имеет доступа к ресурсу, при оценке атрибуты.</span><span class="sxs-lookup"><span data-stu-id="56f48-228">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="56f48-229">Авторизация на основе ресурсов должен быть явный.</span><span class="sxs-lookup"><span data-stu-id="56f48-229">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="56f48-230">Проверки должны выполняться, когда приложение имеет доступ к ресурсу, загрузив его в модели страниц или, загрузив его в сам обработчик.</span><span class="sxs-lookup"><span data-stu-id="56f48-230">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="56f48-231">Вы обращаетесь чаще всего ресурса, передавая ключ ресурса.</span><span class="sxs-lookup"><span data-stu-id="56f48-231">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="56f48-232">Обновление DeleteModel</span><span class="sxs-lookup"><span data-stu-id="56f48-232">Update the DeleteModel</span></span>

<span data-ttu-id="56f48-233">Обновите модель страницы delete на использование обработчика авторизации, чтобы убедиться, что пользователь имеет разрешение на удаление для контакта.</span><span class="sxs-lookup"><span data-stu-id="56f48-233">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="56f48-234">Внедрить службы авторизации в представления</span><span class="sxs-lookup"><span data-stu-id="56f48-234">Inject the authorization service into the views</span></span>

<span data-ttu-id="56f48-235">В настоящее время в пользовательском Интерфейсе показано изменение и удаление ссылок для контактов, которые не могут изменяться.</span><span class="sxs-lookup"><span data-stu-id="56f48-235">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="56f48-236">Внедрить службы авторизации в *Pages/_ViewImports.cshtml* файл, чтобы он был доступен для всех представлений:</span><span class="sxs-lookup"><span data-stu-id="56f48-236">Inject the authorization service in the *Pages/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="56f48-237">Предыдущая разметка добавляет некоторые `using` инструкций.</span><span class="sxs-lookup"><span data-stu-id="56f48-237">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="56f48-238">Обновление **изменить** и **удалить** ссылок в *Pages/Contacts/Index.cshtml* , обработкой только для пользователей с соответствующими разрешениями:</span><span class="sxs-lookup"><span data-stu-id="56f48-238">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="56f48-239">Скрытие ссылок от пользователей, у которых нет разрешения на изменение данных не защитить приложения.</span><span class="sxs-lookup"><span data-stu-id="56f48-239">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="56f48-240">Скрытие ссылок делает приложение более удобным, отображая только допустимые ссылки.</span><span class="sxs-lookup"><span data-stu-id="56f48-240">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="56f48-241">Пользователи могут взломать созданный URL-адреса для вызова редактирования и удаления данных, которыми они не владеют.</span><span class="sxs-lookup"><span data-stu-id="56f48-241">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="56f48-242">На странице Razor или контроллер должен Принудительная проверка доступа для защиты данных.</span><span class="sxs-lookup"><span data-stu-id="56f48-242">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="56f48-243">Дополнительные сведения об обновлении</span><span class="sxs-lookup"><span data-stu-id="56f48-243">Update Details</span></span>

<span data-ttu-id="56f48-244">Обновление представления сведений, чтобы менеджеры могли утверждать или отклонять контактов:</span><span class="sxs-lookup"><span data-stu-id="56f48-244">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="56f48-245">Обновите модель страницы сведений:</span><span class="sxs-lookup"><span data-stu-id="56f48-245">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="56f48-246">Добавление или удаление пользователя к роли</span><span class="sxs-lookup"><span data-stu-id="56f48-246">Add or remove a user to a role</span></span>

<span data-ttu-id="56f48-247">См. в разделе [эту проблему](https://github.com/aspnet/AspNetCore.Docs/issues/8502) сведения о:</span><span class="sxs-lookup"><span data-stu-id="56f48-247">See [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="56f48-248">Удаление привилегий пользователя.</span><span class="sxs-lookup"><span data-stu-id="56f48-248">Removing privileges from a user.</span></span> <span data-ttu-id="56f48-249">Например отключение пользователя в приложение чата.</span><span class="sxs-lookup"><span data-stu-id="56f48-249">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="56f48-250">Добавление прав к пользователю.</span><span class="sxs-lookup"><span data-stu-id="56f48-250">Adding privileges to a user.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="56f48-251">Тестирование завершенного приложения</span><span class="sxs-lookup"><span data-stu-id="56f48-251">Test the completed app</span></span>

<span data-ttu-id="56f48-252">Если вы еще не настроили пароль для учетных записей пользователей на основании добавляемых, использовать [средство Secret Manager](xref:security/app-secrets#secret-manager) Установка пароля:</span><span class="sxs-lookup"><span data-stu-id="56f48-252">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="56f48-253">Выберите надежный пароль. Использовать восемь или больше символов и хотя бы один символ верхнего регистра, чисел и символов.</span><span class="sxs-lookup"><span data-stu-id="56f48-253">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="56f48-254">Например `Passw0rd!` требованиям надежный пароль.</span><span class="sxs-lookup"><span data-stu-id="56f48-254">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="56f48-255">Выполните следующую команду из папки проекта, где `<PW>` пароль:</span><span class="sxs-lookup"><span data-stu-id="56f48-255">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```console
  dotnet user-secrets set SeedUserPW <PW>
  ```

<span data-ttu-id="56f48-256">Если приложение имеет контактов:</span><span class="sxs-lookup"><span data-stu-id="56f48-256">If the app has contacts:</span></span>

* <span data-ttu-id="56f48-257">Удалить все записи в `Contact` таблицы.</span><span class="sxs-lookup"><span data-stu-id="56f48-257">Delete all of the records in the `Contact` table.</span></span>
* <span data-ttu-id="56f48-258">Перезапустите приложение, чтобы заполнить базу данных.</span><span class="sxs-lookup"><span data-stu-id="56f48-258">Restart the app to seed the database.</span></span>

<span data-ttu-id="56f48-259">Простой способ протестировать готовое приложение — запуск три различных браузеров (или инкогнито или InPrivate сеансов).</span><span class="sxs-lookup"><span data-stu-id="56f48-259">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="56f48-260">В один браузер, регистрация нового пользователя (например, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="56f48-260">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="56f48-261">Войдите в каждом браузере с другим пользователем.</span><span class="sxs-lookup"><span data-stu-id="56f48-261">Sign in to each browser with a different user.</span></span> <span data-ttu-id="56f48-262">Проверьте следующие операции:</span><span class="sxs-lookup"><span data-stu-id="56f48-262">Verify the following operations:</span></span>

* <span data-ttu-id="56f48-263">Зарегистрированные пользователи могут просматривать все утвержденные контактные данные.</span><span class="sxs-lookup"><span data-stu-id="56f48-263">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="56f48-264">Зарегистрированным пользователям может изменять и удалять свои собственные данные.</span><span class="sxs-lookup"><span data-stu-id="56f48-264">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="56f48-265">Руководители могут утверждать или отклонять контактных данных.</span><span class="sxs-lookup"><span data-stu-id="56f48-265">Managers can approve/reject contact data.</span></span> <span data-ttu-id="56f48-266">`Details` Просмотра показано **утвердить** и **Отклонить** кнопки.</span><span class="sxs-lookup"><span data-stu-id="56f48-266">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="56f48-267">Администраторы могут утверждать или отклонять и изменить или удалить все данные.</span><span class="sxs-lookup"><span data-stu-id="56f48-267">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="56f48-268">Пользовательская</span><span class="sxs-lookup"><span data-stu-id="56f48-268">User</span></span>                | <span data-ttu-id="56f48-269">Заполняется данными в приложении</span><span class="sxs-lookup"><span data-stu-id="56f48-269">Seeded by the app</span></span> | <span data-ttu-id="56f48-270">Параметры</span><span class="sxs-lookup"><span data-stu-id="56f48-270">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="56f48-271">Нет</span><span class="sxs-lookup"><span data-stu-id="56f48-271">No</span></span>                | <span data-ttu-id="56f48-272">Изменить или удалить данные принадлежат.</span><span class="sxs-lookup"><span data-stu-id="56f48-272">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="56f48-273">Да</span><span class="sxs-lookup"><span data-stu-id="56f48-273">Yes</span></span>               | <span data-ttu-id="56f48-274">Утверждать или отклонять и изменить или удалить данные принадлежат.</span><span class="sxs-lookup"><span data-stu-id="56f48-274">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="56f48-275">Да</span><span class="sxs-lookup"><span data-stu-id="56f48-275">Yes</span></span>               | <span data-ttu-id="56f48-276">Утверждать или отклонять и изменить или удалить все данные.</span><span class="sxs-lookup"><span data-stu-id="56f48-276">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="56f48-277">Создание контакта в браузере администратора.</span><span class="sxs-lookup"><span data-stu-id="56f48-277">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="56f48-278">Скопируйте URL-адрес для удаления и редактирование откуда обратитесь к администратору.</span><span class="sxs-lookup"><span data-stu-id="56f48-278">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="56f48-279">Вставьте эти ссылки в браузере пользователя теста, чтобы убедиться, что тестовый пользователь не может выполнять эти операции.</span><span class="sxs-lookup"><span data-stu-id="56f48-279">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="56f48-280">Создать приложение начального уровня</span><span class="sxs-lookup"><span data-stu-id="56f48-280">Create the starter app</span></span>

* <span data-ttu-id="56f48-281">Создание приложения Razor Pages с именем «ContactManager»</span><span class="sxs-lookup"><span data-stu-id="56f48-281">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="56f48-282">Создание приложения с **отдельным учетным записям пользователей**.</span><span class="sxs-lookup"><span data-stu-id="56f48-282">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="56f48-283">Назовите его «ContactManager», пространство имен соответствует пространству имен, используемые в образце.</span><span class="sxs-lookup"><span data-stu-id="56f48-283">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="56f48-284">`-uld` Указывает LocalDB вместо SQLite</span><span class="sxs-lookup"><span data-stu-id="56f48-284">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="56f48-285">Добавить *Models/Contact.cs*:</span><span class="sxs-lookup"><span data-stu-id="56f48-285">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="56f48-286">Каркас `Contact` модели.</span><span class="sxs-lookup"><span data-stu-id="56f48-286">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="56f48-287">Создание первоначальной миграции и обновления базы данных:</span><span class="sxs-lookup"><span data-stu-id="56f48-287">Create initial migration and update the database:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
  ```

<span data-ttu-id="56f48-288">При возникновении ошибки с `dotnet aspnet-codegenerator razorpage` команды, см. в разделе [проблема GitHub](https://github.com/aspnet/Scaffolding/issues/984).</span><span class="sxs-lookup"><span data-stu-id="56f48-288">If you experience a bug with the `dotnet aspnet-codegenerator razorpage` command, see [this GitHub issue](https://github.com/aspnet/Scaffolding/issues/984).</span></span>

* <span data-ttu-id="56f48-289">Обновление **ContactManager** привязки в *Pages/Shared/_Layout.cshtml* файла:</span><span class="sxs-lookup"><span data-stu-id="56f48-289">Update the **ContactManager** anchor in the *Pages/Shared/_Layout.cshtml* file:</span></span>

 ```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Contacts/Index">ContactManager</a>
  ```

* <span data-ttu-id="56f48-290">Протестируйте приложение, создание, изменение и удаление контакта</span><span class="sxs-lookup"><span data-stu-id="56f48-290">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="56f48-291">Заполнение базы данных</span><span class="sxs-lookup"><span data-stu-id="56f48-291">Seed the database</span></span>

<span data-ttu-id="56f48-292">Добавить [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) класс *данных* папку:</span><span class="sxs-lookup"><span data-stu-id="56f48-292">Add the [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) class to the *Data* folder:</span></span>

[!code-csharp[](secure-data/samples/starter3/Data/SeedData.cs)]

<span data-ttu-id="56f48-293">Вызовите `SeedData.Initialize` из `Main`:</span><span class="sxs-lookup"><span data-stu-id="56f48-293">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter3/Program.cs)]

<span data-ttu-id="56f48-294">Проверьте, что приложение заполняется данными базы данных.</span><span class="sxs-lookup"><span data-stu-id="56f48-294">Test that the app seeded the database.</span></span> <span data-ttu-id="56f48-295">При возникновении любых строк в контакте DB, не запускается метод заполнения.</span><span class="sxs-lookup"><span data-stu-id="56f48-295">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"

<span data-ttu-id="56f48-296">Этом руководстве показано, как создать веб-приложение ASP.NET Core с помощью данных пользователя с помощью авторизации.</span><span class="sxs-lookup"><span data-stu-id="56f48-296">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="56f48-297">Отображается список контактов, прошедшие проверку подлинности (зарегистрированного) пользователи создали.</span><span class="sxs-lookup"><span data-stu-id="56f48-297">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="56f48-298">Существует три группы безопасности:</span><span class="sxs-lookup"><span data-stu-id="56f48-298">There are three security groups:</span></span>

* <span data-ttu-id="56f48-299">**Зарегистрированные пользователи** может просматривать все данные, утвержденных и может изменять и удалять свои собственные данные.</span><span class="sxs-lookup"><span data-stu-id="56f48-299">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="56f48-300">**Диспетчеры** можно утвердить или отклонить контактных данных.</span><span class="sxs-lookup"><span data-stu-id="56f48-300">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="56f48-301">Только утвержденные контакты отображаются для пользователей.</span><span class="sxs-lookup"><span data-stu-id="56f48-301">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="56f48-302">**Администраторы** можно утверждать или отклонять и изменить или удалить все данные.</span><span class="sxs-lookup"><span data-stu-id="56f48-302">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="56f48-303">На следующем рисунке, пользователь Рик (`rick@example.com`) выполнил вход.</span><span class="sxs-lookup"><span data-stu-id="56f48-303">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="56f48-304">Рик могут только просматривать утвержденные контакты и **изменить**/**удалить**/**Create New** ссылки на свои контакты.</span><span class="sxs-lookup"><span data-stu-id="56f48-304">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="56f48-305">Только последней записи, созданные Рик, отображает **изменить** и **удалить** ссылки.</span><span class="sxs-lookup"><span data-stu-id="56f48-305">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="56f48-306">Другие пользователи увидят последнюю запись, после менеджеру или администратору изменяет состояние на «Утверждено».</span><span class="sxs-lookup"><span data-stu-id="56f48-306">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Снимок экрана, показывающий Рик входа](secure-data/_static/rick.png)

<span data-ttu-id="56f48-308">На следующем рисунке `manager@contoso.com` подписан в и в роль диспетчера:</span><span class="sxs-lookup"><span data-stu-id="56f48-308">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![Снимок экрана, показывающий manager@contoso.com в системе](secure-data/_static/manager1.png)

<span data-ttu-id="56f48-310">На следующем рисунке показана менеджеров Просмотр подробностей контакта:</span><span class="sxs-lookup"><span data-stu-id="56f48-310">The following image shows the managers details view of a contact:</span></span>

![Представление руководителя контакта](secure-data/_static/manager.png)

<span data-ttu-id="56f48-312">**Утвердить** и **Отклонить** кнопки отображаются только для менеджеров и администраторов.</span><span class="sxs-lookup"><span data-stu-id="56f48-312">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="56f48-313">На следующем рисунке `admin@contoso.com` подписан в и в роли администратора:</span><span class="sxs-lookup"><span data-stu-id="56f48-313">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![Снимок экрана, показывающий admin@contoso.com в системе](secure-data/_static/admin.png)

<span data-ttu-id="56f48-315">Администратор имеет все привилегии.</span><span class="sxs-lookup"><span data-stu-id="56f48-315">The administrator has all privileges.</span></span> <span data-ttu-id="56f48-316">Она может чтения, изменить или удалить любой контакт и изменить состояние контактов.</span><span class="sxs-lookup"><span data-stu-id="56f48-316">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="56f48-317">Приложение было создано [формирования шаблонов](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) следующие `Contact` модели:</span><span class="sxs-lookup"><span data-stu-id="56f48-317">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="56f48-318">Пример содержит следующие обработчики авторизации:</span><span class="sxs-lookup"><span data-stu-id="56f48-318">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="56f48-319">`ContactIsOwnerAuthorizationHandler`: Гарантирует, что пользователь может изменить только свои данные.</span><span class="sxs-lookup"><span data-stu-id="56f48-319">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="56f48-320">`ContactManagerAuthorizationHandler`: Менеджеры для утверждения или отклонения контактов.</span><span class="sxs-lookup"><span data-stu-id="56f48-320">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="56f48-321">`ContactAdministratorsAuthorizationHandler`: Администраторы могут утверждать или отклонять контакты и изменить или удалить контакты.</span><span class="sxs-lookup"><span data-stu-id="56f48-321">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="56f48-322">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="56f48-322">Prerequisites</span></span>

<span data-ttu-id="56f48-323">Этот учебник является дополнительным.</span><span class="sxs-lookup"><span data-stu-id="56f48-323">This tutorial is advanced.</span></span> <span data-ttu-id="56f48-324">Вы должны быть знакомы с:</span><span class="sxs-lookup"><span data-stu-id="56f48-324">You should be familiar with:</span></span>

* [<span data-ttu-id="56f48-325">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="56f48-325">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="56f48-326">Authentication</span><span class="sxs-lookup"><span data-stu-id="56f48-326">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="56f48-327">Подтверждение учетной записи и восстановление пароля</span><span class="sxs-lookup"><span data-stu-id="56f48-327">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="56f48-328">Авторизация</span><span class="sxs-lookup"><span data-stu-id="56f48-328">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="56f48-329">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="56f48-329">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="56f48-330">Начальный и завершенное приложение</span><span class="sxs-lookup"><span data-stu-id="56f48-330">The starter and completed app</span></span>

<span data-ttu-id="56f48-331">[Скачайте](xref:index#how-to-download-a-sample) [завершения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) приложения.</span><span class="sxs-lookup"><span data-stu-id="56f48-331">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="56f48-332">[Тест](#test-the-completed-app) готовое приложение, поэтому вам ознакомиться с его возможностями безопасности.</span><span class="sxs-lookup"><span data-stu-id="56f48-332">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="56f48-333">Приложение начального уровня</span><span class="sxs-lookup"><span data-stu-id="56f48-333">The starter app</span></span>

<span data-ttu-id="56f48-334">[Скачайте](xref:index#how-to-download-a-sample) [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) приложения.</span><span class="sxs-lookup"><span data-stu-id="56f48-334">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="56f48-335">Запустите приложение, коснитесь **ContactManager** связать и убедитесь, создание, изменение и удаление контакта.</span><span class="sxs-lookup"><span data-stu-id="56f48-335">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="56f48-336">Обеспечение безопасности данных пользователя</span><span class="sxs-lookup"><span data-stu-id="56f48-336">Secure user data</span></span>

<span data-ttu-id="56f48-337">В следующих разделах есть все основные шаги по созданию приложения данные безопасности пользователей.</span><span class="sxs-lookup"><span data-stu-id="56f48-337">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="56f48-338">Могут оказаться полезными для ссылки на готового проекта.</span><span class="sxs-lookup"><span data-stu-id="56f48-338">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="56f48-339">Привязать контактные данные для пользователя</span><span class="sxs-lookup"><span data-stu-id="56f48-339">Tie the contact data to the user</span></span>

<span data-ttu-id="56f48-340">Использование ASP.NET [удостоверений](xref:security/authentication/identity) идентификатор пользователя, чтобы предоставить пользователям можно изменить свои данные, но не данные других пользователей.</span><span class="sxs-lookup"><span data-stu-id="56f48-340">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="56f48-341">Добавить `OwnerID` и `ContactStatus` для `Contact` модели:</span><span class="sxs-lookup"><span data-stu-id="56f48-341">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="56f48-342">`OwnerID` — Это идентификатор пользователя из `AspNetUser` в таблицу [удостоверений](xref:security/authentication/identity) базы данных.</span><span class="sxs-lookup"><span data-stu-id="56f48-342">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="56f48-343">`Status` Поля определяет, является ли контакт для просмотра обычными пользователями.</span><span class="sxs-lookup"><span data-stu-id="56f48-343">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="56f48-344">Создание новой миграции и обновления базы данных:</span><span class="sxs-lookup"><span data-stu-id="56f48-344">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="56f48-345">Добавление служб роли удостоверению</span><span class="sxs-lookup"><span data-stu-id="56f48-345">Add Role services to Identity</span></span>

<span data-ttu-id="56f48-346">Добавление [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) для добавления служб ролей:</span><span class="sxs-lookup"><span data-stu-id="56f48-346">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a><span data-ttu-id="56f48-347">Требовать от пользователей, прошедших проверку подлинности</span><span class="sxs-lookup"><span data-stu-id="56f48-347">Require authenticated users</span></span>

<span data-ttu-id="56f48-348">Задайте политику проверки подлинности по умолчанию, чтобы требовать проверку подлинности пользователей:</span><span class="sxs-lookup"><span data-stu-id="56f48-348">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 <span data-ttu-id="56f48-349">Можно отказаться от проверки подлинности на уровне метода действия, контроллера или страницу Razor с `[AllowAnonymous]` атрибута.</span><span class="sxs-lookup"><span data-stu-id="56f48-349">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="56f48-350">Задание политики проверки подлинности по умолчанию требовать от пользователей пройти проверку подлинности защищает вновь добавленный Razor Pages и контроллеры.</span><span class="sxs-lookup"><span data-stu-id="56f48-350">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="56f48-351">Наличие требуется по умолчанию проверка подлинности безопаснее, чем полагаться на новые контроллеры и страницы Razor для включения `[Authorize]` атрибута.</span><span class="sxs-lookup"><span data-stu-id="56f48-351">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="56f48-352">Добавить [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) в индекс, со сведениями и контактными страницы, чтобы анонимные пользователи могут получить сведения о веб-узле, прежде чем они были зарегистрированы.</span><span class="sxs-lookup"><span data-stu-id="56f48-352">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="56f48-353">Настройка тестовой учетной записью</span><span class="sxs-lookup"><span data-stu-id="56f48-353">Configure the test account</span></span>

<span data-ttu-id="56f48-354">`SeedData` Класс создает две учетные записи: администратора и диспетчера.</span><span class="sxs-lookup"><span data-stu-id="56f48-354">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="56f48-355">Используйте [средство Secret Manager](xref:security/app-secrets) Установка пароля для этих учетных записей.</span><span class="sxs-lookup"><span data-stu-id="56f48-355">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="56f48-356">Задайте пароль из каталога проекта (каталог, содержащий *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="56f48-356">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="56f48-357">Если надежный пароль не указан, создается исключение при `SeedData.Initialize` вызывается.</span><span class="sxs-lookup"><span data-stu-id="56f48-357">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="56f48-358">Обновление `Main` используйте пароль теста:</span><span class="sxs-lookup"><span data-stu-id="56f48-358">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="56f48-359">Создание тестовых учетных записей и обновление контактов</span><span class="sxs-lookup"><span data-stu-id="56f48-359">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="56f48-360">Обновление `Initialize` метод в `SeedData` класса для создания тестовых учетных записей:</span><span class="sxs-lookup"><span data-stu-id="56f48-360">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="56f48-361">Добавьте идентификатор пользователя администратора и `ContactStatus` контактам.</span><span class="sxs-lookup"><span data-stu-id="56f48-361">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="56f48-362">Создать контактных лиц с «Отправлено» и одного «отклонено».</span><span class="sxs-lookup"><span data-stu-id="56f48-362">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="56f48-363">Добавьте идентификатор пользователя и состояние всех контактов.</span><span class="sxs-lookup"><span data-stu-id="56f48-363">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="56f48-364">Отображается только один контакт:</span><span class="sxs-lookup"><span data-stu-id="56f48-364">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="56f48-365">Создание владельца, manager и обработчики авторизации администратора</span><span class="sxs-lookup"><span data-stu-id="56f48-365">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="56f48-366">Создание `ContactIsOwnerAuthorizationHandler` в класс *авторизации* папки.</span><span class="sxs-lookup"><span data-stu-id="56f48-366">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="56f48-367">`ContactIsOwnerAuthorizationHandler` Проверяет, что пользователь, выполняющий ресурса, которому принадлежит ресурс.</span><span class="sxs-lookup"><span data-stu-id="56f48-367">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="56f48-368">`ContactIsOwnerAuthorizationHandler` Вызовы [контекста. Успешно](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) при связаться с владельцем текущего авторизованного пользователя.</span><span class="sxs-lookup"><span data-stu-id="56f48-368">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="56f48-369">Обработчики авторизации обычно:</span><span class="sxs-lookup"><span data-stu-id="56f48-369">Authorization handlers generally:</span></span>

* <span data-ttu-id="56f48-370">Вернуть `context.Succeed` при соблюдении требований.</span><span class="sxs-lookup"><span data-stu-id="56f48-370">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="56f48-371">Вернуть `Task.CompletedTask` Если требования не соблюдены.</span><span class="sxs-lookup"><span data-stu-id="56f48-371">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="56f48-372">`Task.CompletedTask` не является об успехе или неудаче&mdash;позволяет другим обработчикам авторизации для выполнения.</span><span class="sxs-lookup"><span data-stu-id="56f48-372">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="56f48-373">Если необходимо выполнить явную ошибку, возвратить [контекста. Сбой](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="56f48-373">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="56f48-374">Приложение позволяет контактные владельцам изменения или удаления и создания собственных данных.</span><span class="sxs-lookup"><span data-stu-id="56f48-374">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="56f48-375">`ContactIsOwnerAuthorizationHandler` не требуется для проверки операции, передаваемые в качестве параметра требование.</span><span class="sxs-lookup"><span data-stu-id="56f48-375">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="56f48-376">Создайте обработчик диспетчера авторизации</span><span class="sxs-lookup"><span data-stu-id="56f48-376">Create a manager authorization handler</span></span>

<span data-ttu-id="56f48-377">Создание `ContactManagerAuthorizationHandler` в класс *авторизации* папки.</span><span class="sxs-lookup"><span data-stu-id="56f48-377">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="56f48-378">`ContactManagerAuthorizationHandler` Проверяет пользователя, действующий от ресурса является руководителем.</span><span class="sxs-lookup"><span data-stu-id="56f48-378">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="56f48-379">Только менеджеры могли утверждать или отклонять изменения содержимого (новые или измененные).</span><span class="sxs-lookup"><span data-stu-id="56f48-379">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="56f48-380">Создайте обработчик авторизации администратора</span><span class="sxs-lookup"><span data-stu-id="56f48-380">Create an administrator authorization handler</span></span>

<span data-ttu-id="56f48-381">Создание `ContactAdministratorsAuthorizationHandler` в класс *авторизации* папки.</span><span class="sxs-lookup"><span data-stu-id="56f48-381">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="56f48-382">`ContactAdministratorsAuthorizationHandler` Проверяет действующий от ресурса пользователь является администратором.</span><span class="sxs-lookup"><span data-stu-id="56f48-382">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="56f48-383">Администратор может выполнить все операции.</span><span class="sxs-lookup"><span data-stu-id="56f48-383">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="56f48-384">Регистрировать обработчики авторизации</span><span class="sxs-lookup"><span data-stu-id="56f48-384">Register the authorization handlers</span></span>

<span data-ttu-id="56f48-385">Службы, с помощью Entity Framework Core должны быть зарегистрированы для [внедрения зависимостей](xref:fundamentals/dependency-injection) с помощью [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="56f48-385">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="56f48-386">`ContactIsOwnerAuthorizationHandler` Использует ASP.NET Core [удостоверений](xref:security/authentication/identity), которая опирается на Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="56f48-386">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="56f48-387">Зарегистрировать обработчики с коллекцией, службы, поэтому они будут доступны для `ContactsController` через [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="56f48-387">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="56f48-388">Добавьте следующий код в конец `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="56f48-388">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

<span data-ttu-id="56f48-389">`ContactAdministratorsAuthorizationHandler` и `ContactManagerAuthorizationHandler` добавляются как одноэлементных экземпляров.</span><span class="sxs-lookup"><span data-stu-id="56f48-389">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="56f48-390">Они одноэлементных экземпляров, так как они не используют EF и все сведения, необходимые в `Context` параметр `HandleRequirementAsync` метод.</span><span class="sxs-lookup"><span data-stu-id="56f48-390">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="56f48-391">Поддержка авторизации</span><span class="sxs-lookup"><span data-stu-id="56f48-391">Support authorization</span></span>

<span data-ttu-id="56f48-392">В этом разделе вы обновите страницы Razor Pages и добавьте класс требования к операции.</span><span class="sxs-lookup"><span data-stu-id="56f48-392">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="56f48-393">Просмотрите класс требования контактные операций</span><span class="sxs-lookup"><span data-stu-id="56f48-393">Review the contact operations requirements class</span></span>

<span data-ttu-id="56f48-394">Просмотрите `ContactOperations` класса.</span><span class="sxs-lookup"><span data-stu-id="56f48-394">Review the `ContactOperations` class.</span></span> <span data-ttu-id="56f48-395">Этот класс содержит требования к поддерживает приложения:</span><span class="sxs-lookup"><span data-stu-id="56f48-395">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="56f48-396">Создать базовый класс для страниц Razor контактов</span><span class="sxs-lookup"><span data-stu-id="56f48-396">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="56f48-397">Создайте базовый класс, который содержит службы, используемые в Razor Pages контакты.</span><span class="sxs-lookup"><span data-stu-id="56f48-397">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="56f48-398">Базовый класс помещает код инициализации в одном месте:</span><span class="sxs-lookup"><span data-stu-id="56f48-398">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="56f48-399">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="56f48-399">The preceding code:</span></span>

* <span data-ttu-id="56f48-400">Добавляет `IAuthorizationService` службы для доступа к обработчикам авторизации.</span><span class="sxs-lookup"><span data-stu-id="56f48-400">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="56f48-401">Добавляет удостоверение `UserManager` службы.</span><span class="sxs-lookup"><span data-stu-id="56f48-401">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="56f48-402">Добавьте `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="56f48-402">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="56f48-403">Обновление CreateModel</span><span class="sxs-lookup"><span data-stu-id="56f48-403">Update the CreateModel</span></span>

<span data-ttu-id="56f48-404">Измените конструктор модель страницы create для использования `DI_BasePageModel` базового класса:</span><span class="sxs-lookup"><span data-stu-id="56f48-404">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="56f48-405">Обновление `CreateModel.OnPostAsync` метод:</span><span class="sxs-lookup"><span data-stu-id="56f48-405">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="56f48-406">Добавьте идентификатор пользователя, `Contact` модели.</span><span class="sxs-lookup"><span data-stu-id="56f48-406">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="56f48-407">Вызовите обработчик авторизации, чтобы убедиться, что пользователь имеет разрешение на создание контактов.</span><span class="sxs-lookup"><span data-stu-id="56f48-407">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="56f48-408">Обновление IndexModel</span><span class="sxs-lookup"><span data-stu-id="56f48-408">Update the IndexModel</span></span>

<span data-ttu-id="56f48-409">Обновление `OnGetAsync` метод, поэтому отображаются только утвержденные контактов для обычных пользователей:</span><span class="sxs-lookup"><span data-stu-id="56f48-409">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="56f48-410">Обновление EditModel</span><span class="sxs-lookup"><span data-stu-id="56f48-410">Update the EditModel</span></span>

<span data-ttu-id="56f48-411">Добавление обработчика авторизации, чтобы убедиться, что пользователь является владельцем контакта.</span><span class="sxs-lookup"><span data-stu-id="56f48-411">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="56f48-412">Так как авторизации ресурсов проверяется, `[Authorize]` атрибут будет недостаточно.</span><span class="sxs-lookup"><span data-stu-id="56f48-412">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="56f48-413">Приложение не имеет доступа к ресурсу, при оценке атрибуты.</span><span class="sxs-lookup"><span data-stu-id="56f48-413">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="56f48-414">Авторизация на основе ресурсов должен быть явный.</span><span class="sxs-lookup"><span data-stu-id="56f48-414">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="56f48-415">Проверки должны выполняться, когда приложение имеет доступ к ресурсу, загрузив его в модели страниц или, загрузив его в сам обработчик.</span><span class="sxs-lookup"><span data-stu-id="56f48-415">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="56f48-416">Вы обращаетесь чаще всего ресурса, передавая ключ ресурса.</span><span class="sxs-lookup"><span data-stu-id="56f48-416">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="56f48-417">Обновление DeleteModel</span><span class="sxs-lookup"><span data-stu-id="56f48-417">Update the DeleteModel</span></span>

<span data-ttu-id="56f48-418">Обновите модель страницы delete на использование обработчика авторизации, чтобы убедиться, что пользователь имеет разрешение на удаление для контакта.</span><span class="sxs-lookup"><span data-stu-id="56f48-418">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="56f48-419">Внедрить службы авторизации в представления</span><span class="sxs-lookup"><span data-stu-id="56f48-419">Inject the authorization service into the views</span></span>

<span data-ttu-id="56f48-420">В настоящее время в пользовательском Интерфейсе показано изменение и удаление ссылок для контактов, которые не могут изменяться.</span><span class="sxs-lookup"><span data-stu-id="56f48-420">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="56f48-421">Внедрить службы авторизации в *Views/_ViewImports.cshtml* файл, чтобы он был доступен для всех представлений:</span><span class="sxs-lookup"><span data-stu-id="56f48-421">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="56f48-422">Предыдущая разметка добавляет некоторые `using` инструкций.</span><span class="sxs-lookup"><span data-stu-id="56f48-422">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="56f48-423">Обновление **изменить** и **удалить** ссылок в *Pages/Contacts/Index.cshtml* , обработкой только для пользователей с соответствующими разрешениями:</span><span class="sxs-lookup"><span data-stu-id="56f48-423">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="56f48-424">Скрытие ссылок от пользователей, у которых нет разрешения на изменение данных не защитить приложения.</span><span class="sxs-lookup"><span data-stu-id="56f48-424">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="56f48-425">Скрытие ссылок делает приложение более удобным, отображая только допустимые ссылки.</span><span class="sxs-lookup"><span data-stu-id="56f48-425">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="56f48-426">Пользователи могут взломать созданный URL-адреса для вызова редактирования и удаления данных, которыми они не владеют.</span><span class="sxs-lookup"><span data-stu-id="56f48-426">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="56f48-427">На странице Razor или контроллер должен Принудительная проверка доступа для защиты данных.</span><span class="sxs-lookup"><span data-stu-id="56f48-427">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="56f48-428">Дополнительные сведения об обновлении</span><span class="sxs-lookup"><span data-stu-id="56f48-428">Update Details</span></span>

<span data-ttu-id="56f48-429">Обновление представления сведений, чтобы менеджеры могли утверждать или отклонять контактов:</span><span class="sxs-lookup"><span data-stu-id="56f48-429">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="56f48-430">Обновите модель страницы сведений:</span><span class="sxs-lookup"><span data-stu-id="56f48-430">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="56f48-431">Добавление или удаление пользователя к роли</span><span class="sxs-lookup"><span data-stu-id="56f48-431">Add or remove a user to a role</span></span>

<span data-ttu-id="56f48-432">См. в разделе [эту проблему](https://github.com/aspnet/AspNetCore.Docs/issues/8502) сведения о:</span><span class="sxs-lookup"><span data-stu-id="56f48-432">See [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="56f48-433">Удаление привилегий пользователя.</span><span class="sxs-lookup"><span data-stu-id="56f48-433">Removing privileges from a user.</span></span> <span data-ttu-id="56f48-434">Например отключение пользователя в приложение чата.</span><span class="sxs-lookup"><span data-stu-id="56f48-434">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="56f48-435">Добавление прав к пользователю.</span><span class="sxs-lookup"><span data-stu-id="56f48-435">Adding privileges to a user.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="56f48-436">Тестирование завершенного приложения</span><span class="sxs-lookup"><span data-stu-id="56f48-436">Test the completed app</span></span>

<span data-ttu-id="56f48-437">Если вы еще не настроили пароль для учетных записей пользователей на основании добавляемых, использовать [средство Secret Manager](xref:security/app-secrets#secret-manager) Установка пароля:</span><span class="sxs-lookup"><span data-stu-id="56f48-437">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="56f48-438">Выберите надежный пароль. Использовать восемь или больше символов и хотя бы один символ верхнего регистра, чисел и символов.</span><span class="sxs-lookup"><span data-stu-id="56f48-438">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="56f48-439">Например `Passw0rd!` требованиям надежный пароль.</span><span class="sxs-lookup"><span data-stu-id="56f48-439">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="56f48-440">Выполните следующую команду из папки проекта, где `<PW>` пароль:</span><span class="sxs-lookup"><span data-stu-id="56f48-440">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```console
  dotnet user-secrets set SeedUserPW <PW>
  ```

* <span data-ttu-id="56f48-441">Удаление и обновление базы данных</span><span class="sxs-lookup"><span data-stu-id="56f48-441">Drop and update the database</span></span>
    ```console
     dotnet ef database drop -f
     dotnet ef database update  
```

* <span data-ttu-id="56f48-442">Перезапустите приложение, чтобы заполнить базу данных.</span><span class="sxs-lookup"><span data-stu-id="56f48-442">Restart the app to seed the database.</span></span>

<span data-ttu-id="56f48-443">Простой способ протестировать готовое приложение — запуск три различных браузеров (или инкогнито или InPrivate сеансов).</span><span class="sxs-lookup"><span data-stu-id="56f48-443">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="56f48-444">В один браузер, регистрация нового пользователя (например, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="56f48-444">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="56f48-445">Войдите в каждом браузере с другим пользователем.</span><span class="sxs-lookup"><span data-stu-id="56f48-445">Sign in to each browser with a different user.</span></span> <span data-ttu-id="56f48-446">Проверьте следующие операции:</span><span class="sxs-lookup"><span data-stu-id="56f48-446">Verify the following operations:</span></span>

* <span data-ttu-id="56f48-447">Зарегистрированные пользователи могут просматривать все утвержденные контактные данные.</span><span class="sxs-lookup"><span data-stu-id="56f48-447">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="56f48-448">Зарегистрированным пользователям может изменять и удалять свои собственные данные.</span><span class="sxs-lookup"><span data-stu-id="56f48-448">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="56f48-449">Руководители могут утверждать или отклонять контактных данных.</span><span class="sxs-lookup"><span data-stu-id="56f48-449">Managers can approve/reject contact data.</span></span> <span data-ttu-id="56f48-450">`Details` Просмотра показано **утвердить** и **Отклонить** кнопки.</span><span class="sxs-lookup"><span data-stu-id="56f48-450">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="56f48-451">Администраторы могут утверждать или отклонять и изменить или удалить все данные.</span><span class="sxs-lookup"><span data-stu-id="56f48-451">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="56f48-452">Пользовательская</span><span class="sxs-lookup"><span data-stu-id="56f48-452">User</span></span>                | <span data-ttu-id="56f48-453">Заполняется данными в приложении</span><span class="sxs-lookup"><span data-stu-id="56f48-453">Seeded by the app</span></span> | <span data-ttu-id="56f48-454">Параметры</span><span class="sxs-lookup"><span data-stu-id="56f48-454">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="56f48-455">Нет</span><span class="sxs-lookup"><span data-stu-id="56f48-455">No</span></span>                | <span data-ttu-id="56f48-456">Изменить или удалить данные принадлежат.</span><span class="sxs-lookup"><span data-stu-id="56f48-456">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="56f48-457">Да</span><span class="sxs-lookup"><span data-stu-id="56f48-457">Yes</span></span>               | <span data-ttu-id="56f48-458">Утверждать или отклонять и изменить или удалить данные принадлежат.</span><span class="sxs-lookup"><span data-stu-id="56f48-458">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="56f48-459">Да</span><span class="sxs-lookup"><span data-stu-id="56f48-459">Yes</span></span>               | <span data-ttu-id="56f48-460">Утверждать или отклонять и изменить или удалить все данные.</span><span class="sxs-lookup"><span data-stu-id="56f48-460">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="56f48-461">Создание контакта в браузере администратора.</span><span class="sxs-lookup"><span data-stu-id="56f48-461">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="56f48-462">Скопируйте URL-адрес для удаления и редактирование откуда обратитесь к администратору.</span><span class="sxs-lookup"><span data-stu-id="56f48-462">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="56f48-463">Вставьте эти ссылки в браузере пользователя теста, чтобы убедиться, что тестовый пользователь не может выполнять эти операции.</span><span class="sxs-lookup"><span data-stu-id="56f48-463">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="56f48-464">Создать приложение начального уровня</span><span class="sxs-lookup"><span data-stu-id="56f48-464">Create the starter app</span></span>

* <span data-ttu-id="56f48-465">Создание приложения Razor Pages с именем «ContactManager»</span><span class="sxs-lookup"><span data-stu-id="56f48-465">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="56f48-466">Создание приложения с **отдельным учетным записям пользователей**.</span><span class="sxs-lookup"><span data-stu-id="56f48-466">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="56f48-467">Назовите его «ContactManager», пространство имен соответствует пространству имен, используемые в образце.</span><span class="sxs-lookup"><span data-stu-id="56f48-467">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="56f48-468">`-uld` Указывает LocalDB вместо SQLite</span><span class="sxs-lookup"><span data-stu-id="56f48-468">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="56f48-469">Добавить *Models/Contact.cs*:</span><span class="sxs-lookup"><span data-stu-id="56f48-469">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="56f48-470">Каркас `Contact` модели.</span><span class="sxs-lookup"><span data-stu-id="56f48-470">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="56f48-471">Создание первоначальной миграции и обновления базы данных:</span><span class="sxs-lookup"><span data-stu-id="56f48-471">Create initial migration and update the database:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
  dotnet ef database drop -f
  dotnet ef migrations add initial
  dotnet ef database update
  ```

* <span data-ttu-id="56f48-472">Обновление **ContactManager** привязки в *Pages/_Layout.cshtml* файла:</span><span class="sxs-lookup"><span data-stu-id="56f48-472">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

  ```cshtml
  <a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
  ```

* <span data-ttu-id="56f48-473">Протестируйте приложение, создание, изменение и удаление контакта</span><span class="sxs-lookup"><span data-stu-id="56f48-473">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="56f48-474">Заполнение базы данных</span><span class="sxs-lookup"><span data-stu-id="56f48-474">Seed the database</span></span>

<span data-ttu-id="56f48-475">Добавить [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) класс *данных* папки.</span><span class="sxs-lookup"><span data-stu-id="56f48-475">Add the [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) class to the *Data* folder.</span></span>

<span data-ttu-id="56f48-476">Вызовите `SeedData.Initialize` из `Main`:</span><span class="sxs-lookup"><span data-stu-id="56f48-476">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

<span data-ttu-id="56f48-477">Проверьте, что приложение заполняется данными базы данных.</span><span class="sxs-lookup"><span data-stu-id="56f48-477">Test that the app seeded the database.</span></span> <span data-ttu-id="56f48-478">При возникновении любых строк в контакте DB, не запускается метод заполнения.</span><span class="sxs-lookup"><span data-stu-id="56f48-478">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="56f48-479">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="56f48-479">Additional resources</span></span>

* [<span data-ttu-id="56f48-480">Создание веб-приложения .NET Core с базой данных SQL в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="56f48-480">Build a .NET Core and SQL Database web app in Azure App Service</span></span>](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* <span data-ttu-id="56f48-481">[ASP.NET Core авторизация лаборатории](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="56f48-481">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="56f48-482">Это лабораторное занятие содержит более подробные сведения о функциях безопасности, представленных в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="56f48-482">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* <xref:security/authorization/introduction>
* [<span data-ttu-id="56f48-483">Пользовательская авторизация на основе политик</span><span class="sxs-lookup"><span data-stu-id="56f48-483">Custom policy-based authorization</span></span>](xref:security/authorization/policies)
