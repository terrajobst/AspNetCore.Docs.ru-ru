---
title: Создание приложения ASP.NET Core с помощью данных пользователя с помощью авторизации
author: rick-anderson
description: Узнайте, как создать приложение Razor Pages с помощью данных пользователя с помощью авторизации. Включает протокол HTTPS, проверка подлинности и безопасность, удостоверения ASP.NET Core.
ms.author: riande
ms.date: 7/24/2018
uid: security/authorization/secure-data
ms.openlocfilehash: 71b7855958b530b8bac32843a8d1e7db0113ffd9
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912635"
---
::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="71c77-104">См. в разделе [этот PDF-ФАЙЛ](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) для версии ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="71c77-104">See [this PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="71c77-105">Версия ASP.NET Core 1.1 работы с этим руководством, [это](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) папки.</span><span class="sxs-lookup"><span data-stu-id="71c77-105">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="71c77-106">1.1, пример ASP.NET Core находится в [примеры](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="71c77-106">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="71c77-107">См. в разделе [этот PDF-файл](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_July16_18.pdf)</span><span class="sxs-lookup"><span data-stu-id="71c77-107">See [this pdf](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_July16_18.pdf)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="71c77-108">Создание приложения ASP.NET Core с помощью данных пользователя с помощью авторизации</span><span class="sxs-lookup"><span data-stu-id="71c77-108">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="71c77-109">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Джо Одетт](https://twitter.com/joeaudette) (Joe Audette)</span><span class="sxs-lookup"><span data-stu-id="71c77-109">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="71c77-110">Этом руководстве показано, как создать веб-приложение ASP.NET Core с помощью данных пользователя с помощью авторизации.</span><span class="sxs-lookup"><span data-stu-id="71c77-110">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="71c77-111">Отображается список контактов, прошедшие проверку подлинности (зарегистрированного) пользователи создали.</span><span class="sxs-lookup"><span data-stu-id="71c77-111">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="71c77-112">Существует три группы безопасности:</span><span class="sxs-lookup"><span data-stu-id="71c77-112">There are three security groups:</span></span>

* <span data-ttu-id="71c77-113">**Зарегистрированные пользователи** может просматривать все данные, утвержденных и может изменять и удалять свои собственные данные.</span><span class="sxs-lookup"><span data-stu-id="71c77-113">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="71c77-114">**Диспетчеры** можно утвердить или отклонить контактных данных.</span><span class="sxs-lookup"><span data-stu-id="71c77-114">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="71c77-115">Только утвержденные контакты отображаются для пользователей.</span><span class="sxs-lookup"><span data-stu-id="71c77-115">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="71c77-116">**Администраторы** можно утверждать или отклонять и изменить или удалить все данные.</span><span class="sxs-lookup"><span data-stu-id="71c77-116">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="71c77-117">На следующем рисунке, пользователь Рик (`rick@example.com`) выполнил вход.</span><span class="sxs-lookup"><span data-stu-id="71c77-117">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="71c77-118">Рик могут только просматривать утвержденные контакты и **изменить**/**удалить**/**Create New** ссылки на свои контакты.</span><span class="sxs-lookup"><span data-stu-id="71c77-118">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="71c77-119">Только последней записи, созданные Рик, отображает **изменить** и **удалить** ссылки.</span><span class="sxs-lookup"><span data-stu-id="71c77-119">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="71c77-120">Другие пользователи увидят последнюю запись, после менеджеру или администратору изменяет состояние на «Утверждено».</span><span class="sxs-lookup"><span data-stu-id="71c77-120">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![изображение описанные выше](secure-data/_static/rick.png)

<span data-ttu-id="71c77-122">На следующем рисунке `manager@contoso.com` подписан в и в роли managers:</span><span class="sxs-lookup"><span data-stu-id="71c77-122">In the following image, `manager@contoso.com` is signed in and in the managers role:</span></span>

![изображение описанные выше](secure-data/_static/manager1.png)

<span data-ttu-id="71c77-124">На следующем рисунке показана менеджеров Просмотр подробностей контакта:</span><span class="sxs-lookup"><span data-stu-id="71c77-124">The following image shows the managers details view of a contact:</span></span>

![изображение описанные выше](secure-data/_static/manager.png)

<span data-ttu-id="71c77-126">**Утвердить** и **Отклонить** кнопки отображаются только для менеджеров и администраторов.</span><span class="sxs-lookup"><span data-stu-id="71c77-126">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="71c77-127">На следующем рисунке `admin@contoso.com` подписан в и в роли "Администраторы":</span><span class="sxs-lookup"><span data-stu-id="71c77-127">In the following image, `admin@contoso.com` is signed in and in the administrators role:</span></span>

![изображение описанные выше](secure-data/_static/admin.png)

<span data-ttu-id="71c77-129">Администратор имеет все привилегии.</span><span class="sxs-lookup"><span data-stu-id="71c77-129">The administrator has all privileges.</span></span> <span data-ttu-id="71c77-130">Она может чтения, изменить или удалить любой контакт и изменить состояние контактов.</span><span class="sxs-lookup"><span data-stu-id="71c77-130">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="71c77-131">Приложение было создано [формирования шаблонов](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) следующие `Contact` модели:</span><span class="sxs-lookup"><span data-stu-id="71c77-131">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet)]

<span data-ttu-id="71c77-132">Пример содержит следующие обработчики авторизации:</span><span class="sxs-lookup"><span data-stu-id="71c77-132">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="71c77-133">`ContactIsOwnerAuthorizationHandler`: Гарантирует, что пользователь может изменить только свои данные.</span><span class="sxs-lookup"><span data-stu-id="71c77-133">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="71c77-134">`ContactManagerAuthorizationHandler`: Менеджеры для утверждения или отклонения контактов.</span><span class="sxs-lookup"><span data-stu-id="71c77-134">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="71c77-135">`ContactAdministratorsAuthorizationHandler`: Позволяет администраторам для утверждения или отклонения контакты и изменить или удалить контакты.</span><span class="sxs-lookup"><span data-stu-id="71c77-135">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="71c77-136">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="71c77-136">Prerequisites</span></span>

<span data-ttu-id="71c77-137">Этот учебник является дополнительным.</span><span class="sxs-lookup"><span data-stu-id="71c77-137">This tutorial is advanced.</span></span> <span data-ttu-id="71c77-138">Вы должны быть знакомы с:</span><span class="sxs-lookup"><span data-stu-id="71c77-138">You should be familiar with:</span></span>

* [<span data-ttu-id="71c77-139">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="71c77-139">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="71c77-140">Authentication</span><span class="sxs-lookup"><span data-stu-id="71c77-140">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="71c77-141">Подтверждение учетной записи и восстановление пароля</span><span class="sxs-lookup"><span data-stu-id="71c77-141">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="71c77-142">Авторизация</span><span class="sxs-lookup"><span data-stu-id="71c77-142">Authorization</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="71c77-143">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="71c77-143">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="71c77-144">В ASP.NET Core 2.1 `User.IsInRole` завершается сбоем при использовании `AddDefaultIdentity`.</span><span class="sxs-lookup"><span data-stu-id="71c77-144">In ASP.NET Core 2.1, `User.IsInRole` fails when using `AddDefaultIdentity`.</span></span> <span data-ttu-id="71c77-145">В этом руководстве используется `AddDefaultIdentity` и поэтому нуждается в предварительной версии ASP.NET Core 2.2 1 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="71c77-145">This tutorial uses `AddDefaultIdentity` and therefore requires ASP.NET Core 2.2 preview 1 or later.</span></span> <span data-ttu-id="71c77-146">См. в разделе [проблема GitHub](https://github.com/aspnet/Identity/issues/1813#issuecomment-394543909) для обхода.</span><span class="sxs-lookup"><span data-stu-id="71c77-146">See [this GitHub issue](https://github.com/aspnet/Identity/issues/1813#issuecomment-394543909) for a work-around.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="71c77-147">Начальный и завершенное приложение</span><span class="sxs-lookup"><span data-stu-id="71c77-147">The starter and completed app</span></span>

<span data-ttu-id="71c77-148">[Скачайте](xref:tutorials/index#how-to-download-a-sample) [завершения](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) приложения.</span><span class="sxs-lookup"><span data-stu-id="71c77-148">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app.</span></span> <span data-ttu-id="71c77-149">[Тест](#test-the-completed-app) готовое приложение, поэтому вам ознакомиться с его возможностями безопасности.</span><span class="sxs-lookup"><span data-stu-id="71c77-149">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="71c77-150">Приложение начального уровня</span><span class="sxs-lookup"><span data-stu-id="71c77-150">The starter app</span></span>

<span data-ttu-id="71c77-151">[Скачайте](xref:tutorials/index#how-to-download-a-sample) [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) приложения.</span><span class="sxs-lookup"><span data-stu-id="71c77-151">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span></span>

<span data-ttu-id="71c77-152">Запустите приложение, коснитесь **ContactManager** связать и убедитесь, создание, изменение и удаление контакта.</span><span class="sxs-lookup"><span data-stu-id="71c77-152">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="71c77-153">Обеспечение безопасности данных пользователя</span><span class="sxs-lookup"><span data-stu-id="71c77-153">Secure user data</span></span>

<span data-ttu-id="71c77-154">В следующих разделах есть все основные шаги по созданию приложения данные безопасности пользователей.</span><span class="sxs-lookup"><span data-stu-id="71c77-154">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="71c77-155">Могут оказаться полезными для ссылки на готового проекта.</span><span class="sxs-lookup"><span data-stu-id="71c77-155">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="71c77-156">Привязать контактные данные для пользователя</span><span class="sxs-lookup"><span data-stu-id="71c77-156">Tie the contact data to the user</span></span>

<span data-ttu-id="71c77-157">Использование ASP.NET [удостоверений](xref:security/authentication/identity) идентификатор пользователя, чтобы предоставить пользователям можно изменить свои данные, но не данные других пользователей.</span><span class="sxs-lookup"><span data-stu-id="71c77-157">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="71c77-158">Добавить `OwnerID` и `ContactStatus` для `Contact` модели:</span><span class="sxs-lookup"><span data-stu-id="71c77-158">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="71c77-159">`OwnerID` — Это идентификатор пользователя из `AspNetUser` в таблицу [удостоверений](xref:security/authentication/identity) базы данных.</span><span class="sxs-lookup"><span data-stu-id="71c77-159">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="71c77-160">`Status` Поля определяет, является ли контакт для просмотра обычными пользователями.</span><span class="sxs-lookup"><span data-stu-id="71c77-160">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="71c77-161">Создание новой миграции и обновления базы данных:</span><span class="sxs-lookup"><span data-stu-id="71c77-161">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="71c77-162">Добавление служб роли удостоверению</span><span class="sxs-lookup"><span data-stu-id="71c77-162">Add Role services to Identity</span></span>

<span data-ttu-id="71c77-163">Добавление [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) для добавления служб ролей:</span><span class="sxs-lookup"><span data-stu-id="71c77-163">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a><span data-ttu-id="71c77-164">Требовать от пользователей, прошедших проверку подлинности</span><span class="sxs-lookup"><span data-stu-id="71c77-164">Require authenticated users</span></span>

<span data-ttu-id="71c77-165">Задайте политику проверки подлинности по умолчанию, чтобы требовать проверку подлинности пользователей:</span><span class="sxs-lookup"><span data-stu-id="71c77-165">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 <span data-ttu-id="71c77-166">Можно отказаться от проверки подлинности на уровне метода действия, контроллера или страницу Razor с `[AllowAnonymous]` атрибута.</span><span class="sxs-lookup"><span data-stu-id="71c77-166">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="71c77-167">Задание политики проверки подлинности по умолчанию требовать от пользователей пройти проверку подлинности защищает вновь добавленный Razor Pages и контроллеры.</span><span class="sxs-lookup"><span data-stu-id="71c77-167">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="71c77-168">Наличие требуется по умолчанию проверка подлинности безопаснее, чем полагаться на новые контроллеры и страницы Razor для включения `[Authorize]` атрибута.</span><span class="sxs-lookup"><span data-stu-id="71c77-168">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="71c77-169">Добавить [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) в индекс, со сведениями и контактными страницы, чтобы анонимные пользователи могут получить сведения о веб-узле, прежде чем они были зарегистрированы.</span><span class="sxs-lookup"><span data-stu-id="71c77-169">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="71c77-170">Настройка тестовой учетной записью</span><span class="sxs-lookup"><span data-stu-id="71c77-170">Configure the test account</span></span>

<span data-ttu-id="71c77-171">`SeedData` Класс создает две учетные записи: администратора и диспетчера.</span><span class="sxs-lookup"><span data-stu-id="71c77-171">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="71c77-172">Используйте [средство Secret Manager](xref:security/app-secrets) Установка пароля для этих учетных записей.</span><span class="sxs-lookup"><span data-stu-id="71c77-172">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="71c77-173">Задайте пароль из каталога проекта (каталог, содержащий *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="71c77-173">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="71c77-174">Если надежный пароль не указан, создается исключение при `SeedData.Initialize` вызывается.</span><span class="sxs-lookup"><span data-stu-id="71c77-174">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="71c77-175">Обновление `Main` используйте пароль теста:</span><span class="sxs-lookup"><span data-stu-id="71c77-175">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="71c77-176">Создание тестовых учетных записей и обновление контактов</span><span class="sxs-lookup"><span data-stu-id="71c77-176">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="71c77-177">Обновление `Initialize` метод в `SeedData` класса для создания тестовых учетных записей:</span><span class="sxs-lookup"><span data-stu-id="71c77-177">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="71c77-178">Добавьте идентификатор пользователя администратора и `ContactStatus` контактам.</span><span class="sxs-lookup"><span data-stu-id="71c77-178">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="71c77-179">Создать контактных лиц с «Отправлено» и одного «отклонено».</span><span class="sxs-lookup"><span data-stu-id="71c77-179">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="71c77-180">Добавьте идентификатор пользователя и состояние всех контактов.</span><span class="sxs-lookup"><span data-stu-id="71c77-180">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="71c77-181">Отображается только один контакт:</span><span class="sxs-lookup"><span data-stu-id="71c77-181">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="71c77-182">Создание владельца, manager и обработчики авторизации администратора</span><span class="sxs-lookup"><span data-stu-id="71c77-182">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="71c77-183">Создание `ContactIsOwnerAuthorizationHandler` в класс *авторизации* папки.</span><span class="sxs-lookup"><span data-stu-id="71c77-183">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="71c77-184">`ContactIsOwnerAuthorizationHandler` Проверяет, что пользователь, выполняющий ресурса, которому принадлежит ресурс.</span><span class="sxs-lookup"><span data-stu-id="71c77-184">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="71c77-185">`ContactIsOwnerAuthorizationHandler` Вызовы [контекста. Успешно](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) при связаться с владельцем текущего авторизованного пользователя.</span><span class="sxs-lookup"><span data-stu-id="71c77-185">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="71c77-186">Обработчики авторизации обычно:</span><span class="sxs-lookup"><span data-stu-id="71c77-186">Authorization handlers generally:</span></span>

* <span data-ttu-id="71c77-187">Вернуть `context.Succeed` при соблюдении требований.</span><span class="sxs-lookup"><span data-stu-id="71c77-187">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="71c77-188">Вернуть `Task.CompletedTask` Если требования не соблюдены.</span><span class="sxs-lookup"><span data-stu-id="71c77-188">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="71c77-189">`Task.CompletedTask` — ни об успехе или неудаче&mdash;позволяет другим обработчикам авторизации для выполнения.</span><span class="sxs-lookup"><span data-stu-id="71c77-189">`Task.CompletedTask` is neither success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="71c77-190">Если необходимо выполнить явную ошибку, возвратить [контекста. Сбой](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="71c77-190">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="71c77-191">Приложение позволяет контактные владельцам изменения или удаления и создания собственных данных.</span><span class="sxs-lookup"><span data-stu-id="71c77-191">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="71c77-192">`ContactIsOwnerAuthorizationHandler` не требуется для проверки операции, передаваемые в качестве параметра требование.</span><span class="sxs-lookup"><span data-stu-id="71c77-192">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="71c77-193">Создайте обработчик диспетчера авторизации</span><span class="sxs-lookup"><span data-stu-id="71c77-193">Create a manager authorization handler</span></span>

<span data-ttu-id="71c77-194">Создание `ContactManagerAuthorizationHandler` в класс *авторизации* папки.</span><span class="sxs-lookup"><span data-stu-id="71c77-194">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="71c77-195">`ContactManagerAuthorizationHandler` Проверяет пользователя, действующий от ресурса является руководителем.</span><span class="sxs-lookup"><span data-stu-id="71c77-195">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="71c77-196">Только менеджеры могли утверждать или отклонять изменения содержимого (новые или измененные).</span><span class="sxs-lookup"><span data-stu-id="71c77-196">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="71c77-197">Создайте обработчик авторизации администратора</span><span class="sxs-lookup"><span data-stu-id="71c77-197">Create an administrator authorization handler</span></span>

<span data-ttu-id="71c77-198">Создание `ContactAdministratorsAuthorizationHandler` в класс *авторизации* папки.</span><span class="sxs-lookup"><span data-stu-id="71c77-198">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="71c77-199">`ContactAdministratorsAuthorizationHandler` Проверяет действующий от ресурса пользователь является администратором.</span><span class="sxs-lookup"><span data-stu-id="71c77-199">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="71c77-200">Администратор может выполнить все операции.</span><span class="sxs-lookup"><span data-stu-id="71c77-200">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="71c77-201">Регистрировать обработчики авторизации</span><span class="sxs-lookup"><span data-stu-id="71c77-201">Register the authorization handlers</span></span>

<span data-ttu-id="71c77-202">Службы, с помощью Entity Framework Core должны быть зарегистрированы для [внедрения зависимостей](xref:fundamentals/dependency-injection) с помощью [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="71c77-202">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="71c77-203">`ContactIsOwnerAuthorizationHandler` Использует ASP.NET Core [удостоверений](xref:security/authentication/identity), которая опирается на Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="71c77-203">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="71c77-204">Зарегистрировать обработчики с коллекцией, службы, поэтому они будут доступны для `ContactsController` через [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="71c77-204">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="71c77-205">Добавьте следующий код в конец `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="71c77-205">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

<span data-ttu-id="71c77-206">`ContactAdministratorsAuthorizationHandler` и `ContactManagerAuthorizationHandler` добавляются как одноэлементных экземпляров.</span><span class="sxs-lookup"><span data-stu-id="71c77-206">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="71c77-207">Они одноэлементных экземпляров, так как они не используют EF и все сведения, необходимые в `Context` параметр `HandleRequirementAsync` метод.</span><span class="sxs-lookup"><span data-stu-id="71c77-207">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="71c77-208">Поддержка авторизации</span><span class="sxs-lookup"><span data-stu-id="71c77-208">Support authorization</span></span>

<span data-ttu-id="71c77-209">В этом разделе вы обновите страницы Razor Pages и добавьте класс требования к операции.</span><span class="sxs-lookup"><span data-stu-id="71c77-209">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="71c77-210">Просмотрите класс требования контактные операций</span><span class="sxs-lookup"><span data-stu-id="71c77-210">Review the contact operations requirements class</span></span>

<span data-ttu-id="71c77-211">Просмотрите `ContactOperations` класса.</span><span class="sxs-lookup"><span data-stu-id="71c77-211">Review the `ContactOperations` class.</span></span> <span data-ttu-id="71c77-212">Этот класс содержит требования к поддерживает приложения:</span><span class="sxs-lookup"><span data-stu-id="71c77-212">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="71c77-213">Создать базовый класс для страниц Razor контактов</span><span class="sxs-lookup"><span data-stu-id="71c77-213">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="71c77-214">Создайте базовый класс, который содержит службы, используемые в Razor Pages контакты.</span><span class="sxs-lookup"><span data-stu-id="71c77-214">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="71c77-215">Базовый класс помещает код инициализации в одном месте:</span><span class="sxs-lookup"><span data-stu-id="71c77-215">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="71c77-216">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="71c77-216">The preceding code:</span></span>

* <span data-ttu-id="71c77-217">Добавляет `IAuthorizationService` службы для доступа к обработчикам авторизации.</span><span class="sxs-lookup"><span data-stu-id="71c77-217">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="71c77-218">Добавляет удостоверение `UserManager` службы.</span><span class="sxs-lookup"><span data-stu-id="71c77-218">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="71c77-219">Добавьте `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="71c77-219">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="71c77-220">Обновление CreateModel</span><span class="sxs-lookup"><span data-stu-id="71c77-220">Update the CreateModel</span></span>

<span data-ttu-id="71c77-221">Измените конструктор модель страницы create для использования `DI_BasePageModel` базового класса:</span><span class="sxs-lookup"><span data-stu-id="71c77-221">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="71c77-222">Обновление `CreateModel.OnPostAsync` метод:</span><span class="sxs-lookup"><span data-stu-id="71c77-222">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="71c77-223">Добавьте идентификатор пользователя, `Contact` модели.</span><span class="sxs-lookup"><span data-stu-id="71c77-223">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="71c77-224">Вызовите обработчик авторизации, чтобы убедиться, что пользователь имеет разрешение на создание контактов.</span><span class="sxs-lookup"><span data-stu-id="71c77-224">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="71c77-225">Обновление IndexModel</span><span class="sxs-lookup"><span data-stu-id="71c77-225">Update the IndexModel</span></span>

<span data-ttu-id="71c77-226">Обновление `OnGetAsync` метод, поэтому отображаются только утвержденные контактов для обычных пользователей:</span><span class="sxs-lookup"><span data-stu-id="71c77-226">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="71c77-227">Обновление EditModel</span><span class="sxs-lookup"><span data-stu-id="71c77-227">Update the EditModel</span></span>

<span data-ttu-id="71c77-228">Добавление обработчика авторизации, чтобы убедиться, что пользователь является владельцем контакта.</span><span class="sxs-lookup"><span data-stu-id="71c77-228">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="71c77-229">Так как авторизации ресурсов проверяется, `[Authorize]` атрибут будет недостаточно.</span><span class="sxs-lookup"><span data-stu-id="71c77-229">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="71c77-230">Приложение не имеет доступа к ресурсу, при оценке атрибуты.</span><span class="sxs-lookup"><span data-stu-id="71c77-230">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="71c77-231">Авторизация на основе ресурсов должен быть явный.</span><span class="sxs-lookup"><span data-stu-id="71c77-231">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="71c77-232">Проверки должны выполняться, когда приложение имеет доступ к ресурсу, загрузив его в модели страниц или, загрузив его в сам обработчик.</span><span class="sxs-lookup"><span data-stu-id="71c77-232">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="71c77-233">Вы обращаетесь чаще всего ресурса, передавая ключ ресурса.</span><span class="sxs-lookup"><span data-stu-id="71c77-233">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="71c77-234">Обновление DeleteModel</span><span class="sxs-lookup"><span data-stu-id="71c77-234">Update the DeleteModel</span></span>

<span data-ttu-id="71c77-235">Обновите модель страницы delete на использование обработчика авторизации, чтобы убедиться, что пользователь имеет разрешение на удаление для контакта.</span><span class="sxs-lookup"><span data-stu-id="71c77-235">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="71c77-236">Внедрить службы авторизации в представления</span><span class="sxs-lookup"><span data-stu-id="71c77-236">Inject the authorization service into the views</span></span>

<span data-ttu-id="71c77-237">В настоящее время в пользовательском Интерфейсе показано изменение и удаление ссылок для контактов, которые не могут изменяться.</span><span class="sxs-lookup"><span data-stu-id="71c77-237">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="71c77-238">Внедрить службы авторизации в *Views/_ViewImports.cshtml* файл, чтобы он был доступен для всех представлений:</span><span class="sxs-lookup"><span data-stu-id="71c77-238">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="71c77-239">Предыдущая разметка добавляет некоторые `using` инструкций.</span><span class="sxs-lookup"><span data-stu-id="71c77-239">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="71c77-240">Обновление **изменить** и **удалить** ссылок в *Pages/Contacts/Index.cshtml* , обработкой только для пользователей с соответствующими разрешениями:</span><span class="sxs-lookup"><span data-stu-id="71c77-240">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="71c77-241">Скрытие ссылок от пользователей, у которых нет разрешения на изменение данных не защитить приложения.</span><span class="sxs-lookup"><span data-stu-id="71c77-241">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="71c77-242">Скрытие ссылок делает приложение более удобным, отображая только допустимые ссылки.</span><span class="sxs-lookup"><span data-stu-id="71c77-242">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="71c77-243">Пользователи могут взломать созданный URL-адреса для вызова редактирования и удаления данных, которыми они не владеют.</span><span class="sxs-lookup"><span data-stu-id="71c77-243">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="71c77-244">На странице Razor или контроллер должен Принудительная проверка доступа для защиты данных.</span><span class="sxs-lookup"><span data-stu-id="71c77-244">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="71c77-245">Дополнительные сведения об обновлении</span><span class="sxs-lookup"><span data-stu-id="71c77-245">Update Details</span></span>

<span data-ttu-id="71c77-246">Обновление представления сведений, чтобы менеджеры могли утверждать или отклонять контактов:</span><span class="sxs-lookup"><span data-stu-id="71c77-246">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="71c77-247">Обновите модель страницы сведений:</span><span class="sxs-lookup"><span data-stu-id="71c77-247">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="71c77-248">Добавление или удаление пользователя к роли</span><span class="sxs-lookup"><span data-stu-id="71c77-248">Add or remove a user to a role</span></span>

<span data-ttu-id="71c77-249">См. в разделе [эту проблему](https://github.com/aspnet/Docs/issues/8502) сведения о:</span><span class="sxs-lookup"><span data-stu-id="71c77-249">See [this issue](https://github.com/aspnet/Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="71c77-250">Удаление привилегий пользователя.</span><span class="sxs-lookup"><span data-stu-id="71c77-250">Removing privileges from a user.</span></span> <span data-ttu-id="71c77-251">Например, отключение звука пользователя в приложение чата.</span><span class="sxs-lookup"><span data-stu-id="71c77-251">For example muting a user in a chat app.</span></span>
* <span data-ttu-id="71c77-252">Добавление прав к пользователю.</span><span class="sxs-lookup"><span data-stu-id="71c77-252">Adding privileges to a user.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="71c77-253">Тестирование завершенного приложения</span><span class="sxs-lookup"><span data-stu-id="71c77-253">Test the completed app</span></span>

<span data-ttu-id="71c77-254">Если приложение имеет контактов:</span><span class="sxs-lookup"><span data-stu-id="71c77-254">If the app has contacts:</span></span>

* <span data-ttu-id="71c77-255">Удалить все записи из `Contact` таблицы.</span><span class="sxs-lookup"><span data-stu-id="71c77-255">Delete all the records in the `Contact` table.</span></span>
* <span data-ttu-id="71c77-256">Перезапустите приложение, чтобы заполнить базу данных.</span><span class="sxs-lookup"><span data-stu-id="71c77-256">Restart the app to seed the database.</span></span>

<span data-ttu-id="71c77-257">Регистрация пользователя для просмотра контактов.</span><span class="sxs-lookup"><span data-stu-id="71c77-257">Register a user for browsing the contacts.</span></span>

<span data-ttu-id="71c77-258">Простой способ протестировать готовое приложение — запуск три различных браузеров (или инкогнито или InPrivate версий).</span><span class="sxs-lookup"><span data-stu-id="71c77-258">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="71c77-259">В один браузер, регистрация нового пользователя (например, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="71c77-259">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="71c77-260">Войдите в каждом браузере с другим пользователем.</span><span class="sxs-lookup"><span data-stu-id="71c77-260">Sign in to each browser with a different user.</span></span> <span data-ttu-id="71c77-261">Проверьте следующие операции:</span><span class="sxs-lookup"><span data-stu-id="71c77-261">Verify the following operations:</span></span>

* <span data-ttu-id="71c77-262">Зарегистрированные пользователи могут просматривать утвержденные контактных данных.</span><span class="sxs-lookup"><span data-stu-id="71c77-262">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="71c77-263">Зарегистрированным пользователям может изменять и удалять свои собственные данные.</span><span class="sxs-lookup"><span data-stu-id="71c77-263">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="71c77-264">Менеджеры могли утверждать или отклонять контактные данные.</span><span class="sxs-lookup"><span data-stu-id="71c77-264">Managers can approve or reject contact data.</span></span> <span data-ttu-id="71c77-265">`Details` Просмотра показано **утвердить** и **Отклонить** кнопки.</span><span class="sxs-lookup"><span data-stu-id="71c77-265">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="71c77-266">Администраторы могут утверждать или отклонять и изменить или удалить все данные.</span><span class="sxs-lookup"><span data-stu-id="71c77-266">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="71c77-267">Пользовательская</span><span class="sxs-lookup"><span data-stu-id="71c77-267">User</span></span>| <span data-ttu-id="71c77-268">Параметры</span><span class="sxs-lookup"><span data-stu-id="71c77-268">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="71c77-269">Может изменять и удалять данные принадлежат</span><span class="sxs-lookup"><span data-stu-id="71c77-269">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="71c77-270">Можно утверждать или отклонять и изменить или удалить свои данные</span><span class="sxs-lookup"><span data-stu-id="71c77-270">Can approve/reject and edit/delete own data</span></span> |
| admin@contoso.com | <span data-ttu-id="71c77-271">Можно изменить или удалить и утверждать или отклонять все данные</span><span class="sxs-lookup"><span data-stu-id="71c77-271">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="71c77-272">Создание контакта в браузере администратора.</span><span class="sxs-lookup"><span data-stu-id="71c77-272">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="71c77-273">Скопируйте URL-адрес для удаления и редактирование откуда обратитесь к администратору.</span><span class="sxs-lookup"><span data-stu-id="71c77-273">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="71c77-274">Вставьте эти ссылки в браузере пользователя теста, чтобы убедиться, что тестовый пользователь не может выполнять эти операции.</span><span class="sxs-lookup"><span data-stu-id="71c77-274">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="71c77-275">Создать приложение начального уровня</span><span class="sxs-lookup"><span data-stu-id="71c77-275">Create the starter app</span></span>

* <span data-ttu-id="71c77-276">Создание приложения Razor Pages с именем «ContactManager»</span><span class="sxs-lookup"><span data-stu-id="71c77-276">Create a Razor Pages app named "ContactManager"</span></span>
   * <span data-ttu-id="71c77-277">Создание приложения с **отдельным учетным записям пользователей**.</span><span class="sxs-lookup"><span data-stu-id="71c77-277">Create the app with **Individual User Accounts**.</span></span>
   * <span data-ttu-id="71c77-278">Назовите его «ContactManager», пространство имен соответствует пространству имен, используемые в образце.</span><span class="sxs-lookup"><span data-stu-id="71c77-278">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
   * <span data-ttu-id="71c77-279">`-uld` Указывает LocalDB вместо SQLite</span><span class="sxs-lookup"><span data-stu-id="71c77-279">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="71c77-280">Добавить *Models\Contact.cs*:</span><span class="sxs-lookup"><span data-stu-id="71c77-280">Add *Models\Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="71c77-281">Каркас `Contact` модели.</span><span class="sxs-lookup"><span data-stu-id="71c77-281">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="71c77-282">Создание первоначальной миграции и обновления базы данных:</span><span class="sxs-lookup"><span data-stu-id="71c77-282">Create initial migration and update the database:</span></span>

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
```

* <span data-ttu-id="71c77-283">Обновление **ContactManager** привязки в *Pages/_Layout.cshtml* файла:</span><span class="sxs-lookup"><span data-stu-id="71c77-283">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* <span data-ttu-id="71c77-284">Протестируйте приложение, создание, изменение и удаление контакта</span><span class="sxs-lookup"><span data-stu-id="71c77-284">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="71c77-285">Заполнение базы данных</span><span class="sxs-lookup"><span data-stu-id="71c77-285">Seed the database</span></span>

<span data-ttu-id="71c77-286">Добавить [SeedData](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) класс *данных* папки.</span><span class="sxs-lookup"><span data-stu-id="71c77-286">Add the [SeedData](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) class to the *Data* folder.</span></span>

<span data-ttu-id="71c77-287">Вызовите `SeedData.Initialize` из `Main`:</span><span class="sxs-lookup"><span data-stu-id="71c77-287">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

<span data-ttu-id="71c77-288">Проверьте, что приложение заполняется данными базы данных.</span><span class="sxs-lookup"><span data-stu-id="71c77-288">Test that the app seeded the database.</span></span> <span data-ttu-id="71c77-289">При возникновении любых строк в контакте DB, не запускается метод заполнения.</span><span class="sxs-lookup"><span data-stu-id="71c77-289">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="71c77-290">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="71c77-290">Additional resources</span></span>

* [<span data-ttu-id="71c77-291">Создание веб-приложения .NET Core с базой данных SQL в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="71c77-291">Build a .NET Core and SQL Database web app in Azure App Service</span></span>](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* <span data-ttu-id="71c77-292">[ASP.NET Core авторизация лаборатории](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="71c77-292">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="71c77-293">Это лабораторное занятие содержит более подробные сведения о функциях безопасности, представленных в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="71c77-293">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="71c77-294">Авторизация в ASP.NET Core: простая, ролей, на основе утверждений и пользовательская</span><span class="sxs-lookup"><span data-stu-id="71c77-294">Authorization in ASP.NET Core: Simple, role, claims-based, and custom</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="71c77-295">Пользовательская авторизация на основе политик</span><span class="sxs-lookup"><span data-stu-id="71c77-295">Custom policy-based authorization</span></span>](xref:security/authorization/policies)

::: moniker-end
