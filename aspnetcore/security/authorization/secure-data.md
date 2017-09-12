---
title: "Создание приложения ASP.NET Core пользовательскими данными, защищенных авторизации"
author: rick-anderson
keywords: "ASP.NET Core, MVC, авторизации, ролей, безопасность, администратор"
ms.author: riande
manager: wpickett
ms.date: 05/22/2017
ms.topic: article
ms.assetid: abeb2f8e-dfbf-4398-a04c-338a613a65bc
ms.technology: aspnet
ms.prod: aspnet-core
uid: security/authorization/secure-data
ms.openlocfilehash: db05ffb585022c3d9512d32da28c54788f97129c
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2017
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="a0af0-103">Создание приложения ASP.NET Core пользовательскими данными, защищенных авторизации</span><span class="sxs-lookup"><span data-stu-id="a0af0-103">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="a0af0-104">По [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="a0af0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="a0af0-105">Этого учебника показано, как создавать веб-приложения с данными пользователя, защищен авторизации.</span><span class="sxs-lookup"><span data-stu-id="a0af0-105">This tutorial shows how to create a web app with user data protected by authorization.</span></span> <span data-ttu-id="a0af0-106">Отображается список контактов, прошедшие проверку подлинности (зарегистрированные) пользователи создали.</span><span class="sxs-lookup"><span data-stu-id="a0af0-106">It  displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="a0af0-107">Существует три группы безопасности:</span><span class="sxs-lookup"><span data-stu-id="a0af0-107">There are three security groups:</span></span>

* <span data-ttu-id="a0af0-108">Зарегистрированные пользователи могут просматривать утвержденные контактных данных.</span><span class="sxs-lookup"><span data-stu-id="a0af0-108">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="a0af0-109">Зарегистрированные пользователи могут изменить или удалить свои собственные данные.</span><span class="sxs-lookup"><span data-stu-id="a0af0-109">Registered users can edit/delete their own data.</span></span> 
* <span data-ttu-id="a0af0-110">Менеджеры могли утверждать или отклонять контактные данные.</span><span class="sxs-lookup"><span data-stu-id="a0af0-110">Managers can approve or reject contact data.</span></span> <span data-ttu-id="a0af0-111">Только утвержденные контакты отображаются для пользователей.</span><span class="sxs-lookup"><span data-stu-id="a0af0-111">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="a0af0-112">Администраторы могут утвердить или отклонить и изменить или удалить все данные.</span><span class="sxs-lookup"><span data-stu-id="a0af0-112">Administrators can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="a0af0-113">На следующем рисунке пользователь Рик (`rick@example.com`) вход в систему.</span><span class="sxs-lookup"><span data-stu-id="a0af0-113">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="a0af0-114">Пользователь Рик может только представление утвержденные контакты и изменить или удалить его контактов.</span><span class="sxs-lookup"><span data-stu-id="a0af0-114">User Rick can only view approved contacts and edit/delete his contacts.</span></span> <span data-ttu-id="a0af0-115">Последней записи, созданные Рик, отображает изменение и удаление связей</span><span class="sxs-lookup"><span data-stu-id="a0af0-115">Only the last record, created by Rick, displays edit and delete links</span></span>

![изображения, описанных выше](secure-data/_static/rick.png)

<span data-ttu-id="a0af0-117">На следующем рисунке `manager@contoso.com` входит в и диспетчеры роль.</span><span class="sxs-lookup"><span data-stu-id="a0af0-117">In the following image, `manager@contoso.com` is signed in and in the managers role.</span></span> 

![изображения, описанных выше](secure-data/_static/manager1.png)

<span data-ttu-id="a0af0-119">На следующем рисунке менеджеров представление сведений для контакта.</span><span class="sxs-lookup"><span data-stu-id="a0af0-119">The following image shows the  managers details view of a contact.</span></span>

![изображения, описанных выше](secure-data/_static/manager.png)

<span data-ttu-id="a0af0-121">Только администраторы и диспетчеры имеют утверждение и отклонение кнопок.</span><span class="sxs-lookup"><span data-stu-id="a0af0-121">Only managers and administrators have the approve and reject buttons.</span></span>

<span data-ttu-id="a0af0-122">На следующем рисунке `admin@contoso.com` входит в и в роли администратора.</span><span class="sxs-lookup"><span data-stu-id="a0af0-122">In the following image, `admin@contoso.com` is signed in and in the administrator’s role.</span></span> 

![изображения, описанных выше](secure-data/_static/admin.png)

<span data-ttu-id="a0af0-124">Администратор имеет все права доступа.</span><span class="sxs-lookup"><span data-stu-id="a0af0-124">The administrator has all privileges.</span></span> <span data-ttu-id="a0af0-125">Она может чтения, изменить или удалить любой контакт и изменить состояние контактов.</span><span class="sxs-lookup"><span data-stu-id="a0af0-125">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="a0af0-126">Приложение создано с [формирование шаблонов](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) следующие `Contact` модели:</span><span class="sxs-lookup"><span data-stu-id="a0af0-126">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller)  the following `Contact` model:</span></span>

<span data-ttu-id="a0af0-127">[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="a0af0-127">[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]</span></span>

<span data-ttu-id="a0af0-128">Объект `ContactIsOwnerAuthorizationHandler` обработки авторизации гарантирует, что пользователь может редактировать только свои данные.</span><span class="sxs-lookup"><span data-stu-id="a0af0-128">A `ContactIsOwnerAuthorizationHandler` authorization handler ensures that a user can only edit their data.</span></span> <span data-ttu-id="a0af0-129">Объект `ContactManagerAuthorizationHandler` обработки авторизации менеджеры могут утверждать или отклонять контакты.</span><span class="sxs-lookup"><span data-stu-id="a0af0-129">A `ContactManagerAuthorizationHandler` authorization handler allows managers to approve or reject contacts.</span></span>  <span data-ttu-id="a0af0-130">Объект `ContactAdministratorsAuthorizationHandler` обработки авторизации позволяет администраторам утверждать или отклонять контакты и изменение/удаление контактов.</span><span class="sxs-lookup"><span data-stu-id="a0af0-130">A `ContactAdministratorsAuthorizationHandler` authorization handler allows administrators to approve or reject contacts and to edit/delete contacts.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="a0af0-131">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="a0af0-131">Prerequisites</span></span>

<span data-ttu-id="a0af0-132">Это не начало учебника.</span><span class="sxs-lookup"><span data-stu-id="a0af0-132">This is not a beginning tutorial.</span></span> <span data-ttu-id="a0af0-133">Вы должны быть знакомы с:</span><span class="sxs-lookup"><span data-stu-id="a0af0-133">You should be familiar with:</span></span>

* [<span data-ttu-id="a0af0-134">Основные ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="a0af0-134">ASP.NET Core MVC</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="a0af0-135">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="a0af0-135">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="a0af0-136">Начальная и завершенное приложение</span><span class="sxs-lookup"><span data-stu-id="a0af0-136">The starter and completed app</span></span>

<span data-ttu-id="a0af0-137">[Загрузить](xref:tutorials/index#how-to-download-a-sample) [завершения](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final) приложения.</span><span class="sxs-lookup"><span data-stu-id="a0af0-137">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final) app.</span></span> <span data-ttu-id="a0af0-138">[Тест](#test-the-completed-app) завершенное приложение, чтобы ознакомиться с функциями безопасности.</span><span class="sxs-lookup"><span data-stu-id="a0af0-138">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span> 

### <a name="the-starter-app"></a><span data-ttu-id="a0af0-139">Начального уровня приложения</span><span class="sxs-lookup"><span data-stu-id="a0af0-139">The starter app</span></span>

<span data-ttu-id="a0af0-140">Полезно для сравнения кода с помощью полного примера.</span><span class="sxs-lookup"><span data-stu-id="a0af0-140">It's helpful to compare your code with the completed sample.</span></span>

<span data-ttu-id="a0af0-141">[Загрузить](xref:tutorials/index#how-to-download-a-sample) [начальный](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter) приложения.</span><span class="sxs-lookup"><span data-stu-id="a0af0-141">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter) app.</span></span> 

<span data-ttu-id="a0af0-142">В разделе [создания начального приложения](#create-the-starter-app) Если вы хотите создать ее с нуля.</span><span class="sxs-lookup"><span data-stu-id="a0af0-142">See [Create the starter app](#create-the-starter-app) if you'd like to create it from scratch.</span></span>

<span data-ttu-id="a0af0-143">Обновление базы данных:</span><span class="sxs-lookup"><span data-stu-id="a0af0-143">Update the database:</span></span>

```none
   dotnet ef database update
```

<span data-ttu-id="a0af0-144">Запуск приложения, коснитесь **ContactManager** ссылку и убедитесь, создание, изменение и удаление контакта.</span><span class="sxs-lookup"><span data-stu-id="a0af0-144">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

<span data-ttu-id="a0af0-145">Этот учебник содержит основных шагов для создания приложения данных безопасности пользователей.</span><span class="sxs-lookup"><span data-stu-id="a0af0-145">This tutorial has all the major steps to create the secure user data app.</span></span> <span data-ttu-id="a0af0-146">Может быть полезен для обращения к завершенного проекта.</span><span class="sxs-lookup"><span data-stu-id="a0af0-146">You may find it helpful to refer to the completed project.</span></span>

## <a name="modify-the-app-to-secure-user-data"></a><span data-ttu-id="a0af0-147">Изменение приложения для защиты данных пользователя</span><span class="sxs-lookup"><span data-stu-id="a0af0-147">Modify the app to secure user data</span></span>

<span data-ttu-id="a0af0-148">Следующие разделы имеют основных шагов для создания приложения данных безопасности пользователей.</span><span class="sxs-lookup"><span data-stu-id="a0af0-148">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="a0af0-149">Может быть полезен для обращения к завершенного проекта.</span><span class="sxs-lookup"><span data-stu-id="a0af0-149">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="a0af0-150">Связать контактные данные для пользователя</span><span class="sxs-lookup"><span data-stu-id="a0af0-150">Tie the contact data to the user</span></span>

<span data-ttu-id="a0af0-151">Используйте ASP.NET [удостоверение](xref:security/authentication/identity) идентификатор пользователя, чтобы предоставить пользователям можно изменить свои данные, но не данные других пользователей.</span><span class="sxs-lookup"><span data-stu-id="a0af0-151">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="a0af0-152">Добавить `OwnerID` для `Contact` модели:</span><span class="sxs-lookup"><span data-stu-id="a0af0-152">Add `OwnerID` to the `Contact` model:</span></span>

<span data-ttu-id="a0af0-153">[!code-csharp[Main](secure-data/samples/final/Models/Contact.cs?name=snippet1&highlight=5-6,16-)]</span><span class="sxs-lookup"><span data-stu-id="a0af0-153">[!code-csharp[Main](secure-data/samples/final/Models/Contact.cs?name=snippet1&highlight=5-6,16-)]</span></span>

<span data-ttu-id="a0af0-154">`OwnerID`идентификатор пользователя из `AspNetUser` в таблицу [удостоверение](xref:security/authentication/identity) базы данных.</span><span class="sxs-lookup"><span data-stu-id="a0af0-154">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="a0af0-155">`Status` Поле определяет, является ли контакт для просмотра обычными пользователями.</span><span class="sxs-lookup"><span data-stu-id="a0af0-155">The `Status` field determines if a contact is viewable by general users.</span></span> 

<span data-ttu-id="a0af0-156">Сформировать новый миграции и обновления базы данных:</span><span class="sxs-lookup"><span data-stu-id="a0af0-156">Scaffold a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
 ```

### <a name="require-ssl-and-authenticated-users"></a><span data-ttu-id="a0af0-157">Требовать SSL и проверку подлинности пользователей</span><span class="sxs-lookup"><span data-stu-id="a0af0-157">Require SSL and authenticated users</span></span>

<span data-ttu-id="a0af0-158">В `ConfigureServices` метод *файла Startup.cs* файл, добавьте [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api) фильтр авторизации:</span><span class="sxs-lookup"><span data-stu-id="a0af0-158">In the `ConfigureServices` method of the *Startup.cs* file, add the [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api) authorization filter:</span></span>

<span data-ttu-id="a0af0-159">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_SSL&highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="a0af0-159">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_SSL&highlight=1)]</span></span>

<span data-ttu-id="a0af0-160">Если вы используете Visual Studio, см. раздел [настроить IIS Express для SSL или HTTPS](xref:security/enforcing-ssl#set-up-iis-express-for-sslhttps).</span><span class="sxs-lookup"><span data-stu-id="a0af0-160">If you're using Visual Studio, see [Set up IIS Express for SSL/HTTPS](xref:security/enforcing-ssl#set-up-iis-express-for-sslhttps).</span></span> <span data-ttu-id="a0af0-161">Для перенаправления HTTP-запросы HTTPS, в разделе [по промежуточного слоя перезаписи URL-адрес](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="a0af0-161">To redirect HTTP requests to HTTPS, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="a0af0-162">Если вы используете Visual Studio Code или тестирование на локальной платформе, которая не включает тестового сертификата для SSL:</span><span class="sxs-lookup"><span data-stu-id="a0af0-162">If you are using Visual Studio Code or testing on local platform that doesn't include a test certificate for SSL:</span></span>

- <span data-ttu-id="a0af0-163">Задать `"LocalTest:skipSSL": true` в *appsettings.json* файла.</span><span class="sxs-lookup"><span data-stu-id="a0af0-163">Set `"LocalTest:skipSSL": true` in the *appsettings.json* file.</span></span>

### <a name="require-authenticated-users"></a><span data-ttu-id="a0af0-164">Требовать проверку подлинности пользователей</span><span class="sxs-lookup"><span data-stu-id="a0af0-164">Require authenticated users</span></span>

<span data-ttu-id="a0af0-165">Задайте политику проверки подлинности по умолчанию, чтобы требовать проверку подлинности пользователей.</span><span class="sxs-lookup"><span data-stu-id="a0af0-165">Set the default authentication policy to require users to be authenticated.</span></span> <span data-ttu-id="a0af0-166">Можно отключить проверку подлинности на контроллеру или методу действия с `[AllowAnonymous]` атрибута.</span><span class="sxs-lookup"><span data-stu-id="a0af0-166">You can opt out of authentication at the controller or action method with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="a0af0-167">Таким образом, любые новые контроллеры добавлен автоматически требуется проверка подлинности, которая является более безопасным, чем полагаться на новые контроллеры для включения `[Authorize]` атрибута.</span><span class="sxs-lookup"><span data-stu-id="a0af0-167">With this approach, any new controllers added will automatically require authentication, which is safer than relying on new controllers to include the `[Authorize]` attribute.</span></span> <span data-ttu-id="a0af0-168">Добавьте следующий код в `ConfigureServices` метод *файла Startup.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="a0af0-168">Add the following to  the `ConfigureServices` method of the *Startup.cs* file:</span></span>

<span data-ttu-id="a0af0-169">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_defaultPolicy)]</span><span class="sxs-lookup"><span data-stu-id="a0af0-169">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_defaultPolicy)]</span></span>

<span data-ttu-id="a0af0-170">Добавить `[AllowAnonymous]` , анонимные пользователи могут получить сведения о веб-сайте, прежде чем они зарегистрировать контроллер home.</span><span class="sxs-lookup"><span data-stu-id="a0af0-170">Add `[AllowAnonymous]` to the home controller so anonymous users can get information about the site before they register.</span></span>

<span data-ttu-id="a0af0-171">[!code-csharp[Main](secure-data/samples/final/Controllers/HomeController.cs?name=snippet1&highlight=2,6)]</span><span class="sxs-lookup"><span data-stu-id="a0af0-171">[!code-csharp[Main](secure-data/samples/final/Controllers/HomeController.cs?name=snippet1&highlight=2,6)]</span></span>

### <a name="configure-the-test-account"></a><span data-ttu-id="a0af0-172">Настройка тестовой учетной записи</span><span class="sxs-lookup"><span data-stu-id="a0af0-172">Configure the test account</span></span>

<span data-ttu-id="a0af0-173">`SeedData` Класс создает две учетные записи, администратора и диспетчера.</span><span class="sxs-lookup"><span data-stu-id="a0af0-173">The `SeedData` class creates two accounts,  administrator and manager.</span></span> <span data-ttu-id="a0af0-174">Используйте [секрет диспетчера](xref:security/app-secrets) , чтобы задать пароль для этих учетных записей.</span><span class="sxs-lookup"><span data-stu-id="a0af0-174">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="a0af0-175">Это можно сделать из каталога проекта (каталог, содержащий *Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="a0af0-175">Do this from the project directory (the directory containing *Program.cs*).</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="a0af0-176">Обновление `Configure` использовать пароль теста:</span><span class="sxs-lookup"><span data-stu-id="a0af0-176">Update `Configure` to use the test password:</span></span>

<span data-ttu-id="a0af0-177">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=Configure&highlight=19-21)]</span><span class="sxs-lookup"><span data-stu-id="a0af0-177">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=Configure&highlight=19-21)]</span></span>

<span data-ttu-id="a0af0-178">Добавьте идентификатор пользователя администратора и `Status = ContactStatus.Approved` контактам.</span><span class="sxs-lookup"><span data-stu-id="a0af0-178">Add the administrator user ID and `Status = ContactStatus.Approved` to the contacts.</span></span> <span data-ttu-id="a0af0-179">Отображается только один контакт, добавьте все контакты идентификатор пользователя:</span><span class="sxs-lookup"><span data-stu-id="a0af0-179">Only one contact is shown, add the user ID to all contacts:</span></span>

<span data-ttu-id="a0af0-180">[!code-csharp[Main](secure-data/samples/final/Data/SeedData.cs?name=snippet1&highlight=17,18)]</span><span class="sxs-lookup"><span data-stu-id="a0af0-180">[!code-csharp[Main](secure-data/samples/final/Data/SeedData.cs?name=snippet1&highlight=17,18)]</span></span>

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="a0af0-181">Создать владельца, диспетчер и обработчики авторизации администратора</span><span class="sxs-lookup"><span data-stu-id="a0af0-181">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="a0af0-182">Создание `ContactIsOwnerAuthorizationHandler` класса в *авторизации* папки.</span><span class="sxs-lookup"><span data-stu-id="a0af0-182">Create a `ContactIsOwnerAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="a0af0-183">`ContactIsOwnerAuthorizationHandler` Проверит пользователя, действующий от ресурса, которому принадлежит ресурс.</span><span class="sxs-lookup"><span data-stu-id="a0af0-183">The `ContactIsOwnerAuthorizationHandler` will verify the user acting on the resource owns the resource.</span></span>

<span data-ttu-id="a0af0-184">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactIsOwnerAuthorizationHandler.cs)]</span><span class="sxs-lookup"><span data-stu-id="a0af0-184">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactIsOwnerAuthorizationHandler.cs)]</span></span>

<span data-ttu-id="a0af0-185">`ContactIsOwnerAuthorizationHandler` Вызовы `context.Succeed` Если контактные владельцем текущего пользователя, прошедшего проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="a0af0-185">The `ContactIsOwnerAuthorizationHandler` calls `context.Succeed` if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="a0af0-186">Обработчики авторизации обычно возвращают `context.Succeed` при выполнение требований.</span><span class="sxs-lookup"><span data-stu-id="a0af0-186">Authorization handlers generally return `context.Succeed` when the requirements are met.</span></span> <span data-ttu-id="a0af0-187">Они возвращают `Task.FromResult(0)` Если требования не выполнены.</span><span class="sxs-lookup"><span data-stu-id="a0af0-187">They return `Task.FromResult(0)` when requirements are not met.</span></span> <span data-ttu-id="a0af0-188">`Task.FromResult(0)`— ни успех или неудача, она позволяет другому обработчику авторизации для выполнения.</span><span class="sxs-lookup"><span data-stu-id="a0af0-188">`Task.FromResult(0)` is neither success or failure, it allows other authorization handler to run.</span></span> <span data-ttu-id="a0af0-189">Если необходимо выполнить явную ошибку, возвратить `context.Fail()`.</span><span class="sxs-lookup"><span data-stu-id="a0af0-189">If you need to explicitly fail, return `context.Fail()`.</span></span>

<span data-ttu-id="a0af0-190">Мы даем контакта владельцев, изменить или удалить свои собственные данные, поэтому мы не нужно проверять операции, передаваемые в качестве параметра требование.</span><span class="sxs-lookup"><span data-stu-id="a0af0-190">We allow contact owners to edit/delete their own data, so we don't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="a0af0-191">Создание обработчика диспетчера авторизации</span><span class="sxs-lookup"><span data-stu-id="a0af0-191">Create a manager authorization handler</span></span>

<span data-ttu-id="a0af0-192">Создание `ContactManagerAuthorizationHandler` класса в *авторизации* папки.</span><span class="sxs-lookup"><span data-stu-id="a0af0-192">Create a `ContactManagerAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="a0af0-193">`ContactManagerAuthorizationHandler` Проверит пользователя, действующий от ресурса является менеджером.</span><span class="sxs-lookup"><span data-stu-id="a0af0-193">The `ContactManagerAuthorizationHandler` will verify the user acting on the resource is a manager.</span></span> <span data-ttu-id="a0af0-194">Данные только менеджеров можно утвердить или отклонить изменения содержимого (новые или измененные).</span><span class="sxs-lookup"><span data-stu-id="a0af0-194">Only managers can approve or reject content changes (new or changed).</span></span>

<span data-ttu-id="a0af0-195">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactManagerAuthorizationHandler.cs)]</span><span class="sxs-lookup"><span data-stu-id="a0af0-195">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactManagerAuthorizationHandler.cs)]</span></span>

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="a0af0-196">Создайте обработчик авторизации администратора</span><span class="sxs-lookup"><span data-stu-id="a0af0-196">Create an administrator authorization handler</span></span>

<span data-ttu-id="a0af0-197">Создание `ContactAdministratorsAuthorizationHandler` класса в *авторизации* папки.</span><span class="sxs-lookup"><span data-stu-id="a0af0-197">Create a `ContactAdministratorsAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="a0af0-198">`ContactAdministratorsAuthorizationHandler` Проверит действует для ресурса пользователь является администратором.</span><span class="sxs-lookup"><span data-stu-id="a0af0-198">The `ContactAdministratorsAuthorizationHandler` will verify the user acting on the resource is a administrator.</span></span> <span data-ttu-id="a0af0-199">Администратор может выполнять все операции.</span><span class="sxs-lookup"><span data-stu-id="a0af0-199">Administrator can do all operations.</span></span>

<span data-ttu-id="a0af0-200">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactAdministratorsAuthorizationHandler.cs)]</span><span class="sxs-lookup"><span data-stu-id="a0af0-200">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactAdministratorsAuthorizationHandler.cs)]</span></span>

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="a0af0-201">Регистрировать обработчики авторизации</span><span class="sxs-lookup"><span data-stu-id="a0af0-201">Register the authorization handlers</span></span>

<span data-ttu-id="a0af0-202">С помощью Entity Framework Core Services должны быть зарегистрированы для [внедрения зависимостей](xref:fundamentals/dependency-injection) с помощью [AddScoped](https://docs.microsoft.com/aspnet/core/api).</span><span class="sxs-lookup"><span data-stu-id="a0af0-202">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](https://docs.microsoft.com/aspnet/core/api).</span></span> <span data-ttu-id="a0af0-203">`ContactIsOwnerAuthorizationHandler` Использует ASP.NET Core [удостоверения](xref:security/authentication/identity), которые построены на Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="a0af0-203">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="a0af0-204">Зарегистрировать обработчики с коллекцией служб, они будут доступны для `ContactsController` через [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a0af0-204">Register the handlers with the service collection so they will be available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="a0af0-205">Добавьте следующий код в конец `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="a0af0-205">Add the following code to the end of `ConfigureServices`:</span></span>

<span data-ttu-id="a0af0-206">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=AuthorizationHandlers)]</span><span class="sxs-lookup"><span data-stu-id="a0af0-206">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=AuthorizationHandlers)]</span></span>

<span data-ttu-id="a0af0-207">`ContactAdministratorsAuthorizationHandler`и `ContactManagerAuthorizationHandler` добавляются как одноэлементных кортежей.</span><span class="sxs-lookup"><span data-stu-id="a0af0-207">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="a0af0-208">Они являются единственных экземпляров, так как они не используют EF и все сведения, необходимые в `Context` параметр `HandleRequirementAsync` метода.</span><span class="sxs-lookup"><span data-stu-id="a0af0-208">They are singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

<span data-ttu-id="a0af0-209">Полный `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="a0af0-209">The complete `ConfigureServices`:</span></span>

<span data-ttu-id="a0af0-210">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=ConfigureServices)]</span><span class="sxs-lookup"><span data-stu-id="a0af0-210">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=ConfigureServices)]</span></span>

## <a name="update-the-code-to-support-authorization"></a><span data-ttu-id="a0af0-211">Обновление кода для поддержки авторизации</span><span class="sxs-lookup"><span data-stu-id="a0af0-211">Update the code to support authorization</span></span>

<span data-ttu-id="a0af0-212">В этом разделе обновления контроллера и представлений и добавить класс требований операции.</span><span class="sxs-lookup"><span data-stu-id="a0af0-212">In this section, you update the controller and views and add an operations requirements class.</span></span>

### <a name="update-the-contacts-controller"></a><span data-ttu-id="a0af0-213">Обновление контроллера контактов</span><span class="sxs-lookup"><span data-stu-id="a0af0-213">Update the Contacts controller</span></span>

<span data-ttu-id="a0af0-214">Обновление `ContactsController` конструктор:</span><span class="sxs-lookup"><span data-stu-id="a0af0-214">Update the `ContactsController` constructor:</span></span>

* <span data-ttu-id="a0af0-215">Добавить `IAuthorizationService` службы для доступа к обработчикам авторизации.</span><span class="sxs-lookup"><span data-stu-id="a0af0-215">Add the `IAuthorizationService` service to  access to the authorization handlers.</span></span> 
* <span data-ttu-id="a0af0-216">Добавить `Identity` `UserManager` службы:</span><span class="sxs-lookup"><span data-stu-id="a0af0-216">Add the `Identity` `UserManager` service:</span></span>

<span data-ttu-id="a0af0-217">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_ContactsControllerCtor)]</span><span class="sxs-lookup"><span data-stu-id="a0af0-217">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_ContactsControllerCtor)]</span></span>

### <a name="add-a-contact-operations-requirements-class"></a><span data-ttu-id="a0af0-218">Добавьте класс требования операции контактов</span><span class="sxs-lookup"><span data-stu-id="a0af0-218">Add a contact operations requirements class</span></span>

<span data-ttu-id="a0af0-219">Добавить `ContactOperationsRequirements` класса *авторизации* папки.</span><span class="sxs-lookup"><span data-stu-id="a0af0-219">Add the `ContactOperationsRequirements` class to the *Authorization* folder.</span></span> <span data-ttu-id="a0af0-220">Этот класс содержит требованиям наших поддерживает приложения:</span><span class="sxs-lookup"><span data-stu-id="a0af0-220">This class  contain the requirements our app supports:</span></span>

<span data-ttu-id="a0af0-221">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]</span><span class="sxs-lookup"><span data-stu-id="a0af0-221">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]</span></span>

### <a name="update-create"></a><span data-ttu-id="a0af0-222">Создание обновления</span><span class="sxs-lookup"><span data-stu-id="a0af0-222">Update Create</span></span>

<span data-ttu-id="a0af0-223">Обновление `HTTP POST Create` метода:</span><span class="sxs-lookup"><span data-stu-id="a0af0-223">Update the `HTTP POST Create` method to:</span></span>

* <span data-ttu-id="a0af0-224">Добавьте идентификатор пользователя, который `Contact` модели.</span><span class="sxs-lookup"><span data-stu-id="a0af0-224">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="a0af0-225">Вызывает обработчик авторизации, чтобы убедиться, что пользователь является владельцем контакта.</span><span class="sxs-lookup"><span data-stu-id="a0af0-225">Call the authorization handler to verify the user owns the contact.</span></span>

<span data-ttu-id="a0af0-226">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="a0af0-226">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Create)]</span></span>

### <a name="update-edit"></a><span data-ttu-id="a0af0-227">Изменение обновления</span><span class="sxs-lookup"><span data-stu-id="a0af0-227">Update Edit</span></span>

<span data-ttu-id="a0af0-228">Обновлять `Edit` методов на использование обработчика авторизации, чтобы убедиться, что пользователь является владельцем контакта.</span><span class="sxs-lookup"><span data-stu-id="a0af0-228">Update both `Edit` methods to use the authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="a0af0-229">Так как мы занимаемся авторизации ресурсов не может использовать `[Authorize]` атрибута.</span><span class="sxs-lookup"><span data-stu-id="a0af0-229">Because we are performing resource authorization we cannot use the `[Authorize]` attribute.</span></span> <span data-ttu-id="a0af0-230">При оценке атрибуты нас нет доступа к ресурсу.</span><span class="sxs-lookup"><span data-stu-id="a0af0-230">We don't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="a0af0-231">Авторизация на основе ресурсов должно быть принудительной.</span><span class="sxs-lookup"><span data-stu-id="a0af0-231">Resource based authorization must be imperative.</span></span> <span data-ttu-id="a0af0-232">После получения доступа к ресурсу, загрузив его в нашем контроллера или его загрузке в сам обработчик должен выполняться проверки.</span><span class="sxs-lookup"><span data-stu-id="a0af0-232">Checks must be performed once we have access to the resource, either by loading it in our controller, or by loading it within the handler itself.</span></span> <span data-ttu-id="a0af0-233">Часто будет доступ к ресурсу, передавая ключ ресурса.</span><span class="sxs-lookup"><span data-stu-id="a0af0-233">Frequently you will access the resource by passing in the resource key.</span></span>

<span data-ttu-id="a0af0-234">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Edit)]</span><span class="sxs-lookup"><span data-stu-id="a0af0-234">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Edit)]</span></span>

### <a name="update-the-delete-method"></a><span data-ttu-id="a0af0-235">Метод удаления обновления</span><span class="sxs-lookup"><span data-stu-id="a0af0-235">Update the Delete method</span></span>

<span data-ttu-id="a0af0-236">Обновлять `Delete` методов на использование обработчика авторизации, чтобы убедиться, что пользователь является владельцем контакта.</span><span class="sxs-lookup"><span data-stu-id="a0af0-236">Update both `Delete` methods to use the authorization handler to verify the user owns the contact.</span></span>

<span data-ttu-id="a0af0-237">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Delete)]</span><span class="sxs-lookup"><span data-stu-id="a0af0-237">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Delete)]</span></span>

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="a0af0-238">Внедрить службы авторизации в представления</span><span class="sxs-lookup"><span data-stu-id="a0af0-238">Inject the authorization service into the views</span></span>

<span data-ttu-id="a0af0-239">В настоящее время показано пользовательского интерфейса, изменение и удаление ссылки на данные, которые не может быть изменено пользователем.</span><span class="sxs-lookup"><span data-stu-id="a0af0-239">Currently the UI shows edit and delete links for data the user cannot modify.</span></span> <span data-ttu-id="a0af0-240">Мы исправим это путем применения авторизации обработчика представлений.</span><span class="sxs-lookup"><span data-stu-id="a0af0-240">We'll fix that by applying the authorization handler to the views.</span></span>

<span data-ttu-id="a0af0-241">Запустить службу авторизации в *Views/_ViewImports.cshtml* файл, он будет доступен для всех представлений:</span><span class="sxs-lookup"><span data-stu-id="a0af0-241">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it will be available to all views:</span></span>

<span data-ttu-id="a0af0-242">[!code-html[Main](secure-data/samples/final/Views/_ViewImports.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="a0af0-242">[!code-html[Main](secure-data/samples/final/Views/_ViewImports.cshtml)]</span></span>

<span data-ttu-id="a0af0-243">Обновление *Views/Contacts/Index.cshtml* представления Razor, чтобы отображались только редактирование удалить ссылки для пользователей, которые можно изменить или удалить контакт.</span><span class="sxs-lookup"><span data-stu-id="a0af0-243">Update the *Views/Contacts/Index.cshtml* Razor view to only display the edit and delete links for users who can edit/delete the contact.</span></span>

<span data-ttu-id="a0af0-244">Добавить`@using ContactManager.Authorization;`</span><span class="sxs-lookup"><span data-stu-id="a0af0-244">Add `@using ContactManager.Authorization;`</span></span>

<span data-ttu-id="a0af0-245">Обновление `Edit` и `Delete` ссылки, они отображаются только пользователи с разрешением на изменение и удаление контакта.</span><span class="sxs-lookup"><span data-stu-id="a0af0-245">Update the `Edit` and `Delete` links so they are only rendered for users with permission to edit and delete the contact.</span></span>

<span data-ttu-id="a0af0-246">[!code-html[Main](secure-data/samples/final/Views/Contacts/Index.cshtml?range=63-84)]</span><span class="sxs-lookup"><span data-stu-id="a0af0-246">[!code-html[Main](secure-data/samples/final/Views/Contacts/Index.cshtml?range=63-84)]</span></span>

<span data-ttu-id="a0af0-247">Предупреждение: Скрытие ссылок от пользователей, у которых нет разрешения на изменение и удаление данных не обеспечивает безопасность приложения.</span><span class="sxs-lookup"><span data-stu-id="a0af0-247">Warning: Hiding links from users that do not have permission to edit or delete data does not secure the app.</span></span> <span data-ttu-id="a0af0-248">Скрытие ссылки делает приложение пользователя более понятное, отображая только допустимые ссылки.</span><span class="sxs-lookup"><span data-stu-id="a0af0-248">Hiding links makes the app more user friendly by displaying only valid links.</span></span> <span data-ttu-id="a0af0-249">Пользователи могут hack созданные URL-адреса, вызывать изменение и удаление операций с данными, которыми они не владеют.</span><span class="sxs-lookup"><span data-stu-id="a0af0-249">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span>  <span data-ttu-id="a0af0-250">Контроллер должен повторять проверку доступа для обеспечения безопасности.</span><span class="sxs-lookup"><span data-stu-id="a0af0-250">The controller must repeat the access checks to be secure.</span></span>

### <a name="update-the-details-view"></a><span data-ttu-id="a0af0-251">Обновить представление сведений</span><span class="sxs-lookup"><span data-stu-id="a0af0-251">Update the Details view</span></span>

<span data-ttu-id="a0af0-252">Обновление представления сведений, чтобы менеджеры могли утверждать или отклонять контакты:</span><span class="sxs-lookup"><span data-stu-id="a0af0-252">Update the details view so managers can approve or reject contacts:</span></span>

<span data-ttu-id="a0af0-253">[!code-html[Main](secure-data/samples/final/Views/Contacts/Details.cshtml?range=53-)]</span><span class="sxs-lookup"><span data-stu-id="a0af0-253">[!code-html[Main](secure-data/samples/final/Views/Contacts/Details.cshtml?range=53-)]</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="a0af0-254">Тестирование завершенного приложения</span><span class="sxs-lookup"><span data-stu-id="a0af0-254">Test the completed app</span></span>

<span data-ttu-id="a0af0-255">Если вы используете Visual Studio Code или тестирование на локальной платформе, которая не включает тестового сертификата для SSL:</span><span class="sxs-lookup"><span data-stu-id="a0af0-255">If you are using Visual Studio Code or testing on local platform that doesn't include a test certificate for SSL:</span></span>

- <span data-ttu-id="a0af0-256">Задать `"LocalTest:skipSSL": true` в *appsettings.json* файла.</span><span class="sxs-lookup"><span data-stu-id="a0af0-256">Set `"LocalTest:skipSSL": true` in the *appsettings.json* file.</span></span>

<span data-ttu-id="a0af0-257">Если вы запускали приложение и контактов, удалите все записи в `Contact` таблице и перезапустить приложение для инициализации базы данных.</span><span class="sxs-lookup"><span data-stu-id="a0af0-257">If you have run the app and have contacts, delete all the records in the `Contact` table and restart the app to seed the database.</span></span> <span data-ttu-id="a0af0-258">Если вы используете Visual Studio, необходимо закрыть и перезапустить IIS Express для инициализации базы данных.</span><span class="sxs-lookup"><span data-stu-id="a0af0-258">If you are using Visual Studio, you need to exit and restart IIS Express to seed the database.</span></span>

<span data-ttu-id="a0af0-259">Регистрация пользователя для поиска контактов.</span><span class="sxs-lookup"><span data-stu-id="a0af0-259">Register a user to browse the contacts.</span></span>

<span data-ttu-id="a0af0-260">Простой способ тестирования завершенное приложение — запуск три разных браузерах (или версий браузера или InPrivate).</span><span class="sxs-lookup"><span data-stu-id="a0af0-260">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="a0af0-261">В одном браузере регистрации нового пользователя, например, `test@example.com`.</span><span class="sxs-lookup"><span data-stu-id="a0af0-261">In one browser, register a new user, for example, `test@example.com`.</span></span> <span data-ttu-id="a0af0-262">Вход для каждого веб-обозревателя с другим пользователем.</span><span class="sxs-lookup"><span data-stu-id="a0af0-262">Sign in to each browser with a different user.</span></span> <span data-ttu-id="a0af0-263">Проверьте следующее.</span><span class="sxs-lookup"><span data-stu-id="a0af0-263">Verify the following:</span></span>

* <span data-ttu-id="a0af0-264">Зарегистрированные пользователи могут просматривать утвержденные контактных данных.</span><span class="sxs-lookup"><span data-stu-id="a0af0-264">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="a0af0-265">Зарегистрированные пользователи могут изменить или удалить свои собственные данные.</span><span class="sxs-lookup"><span data-stu-id="a0af0-265">Registered users can edit/delete their own data.</span></span> 
* <span data-ttu-id="a0af0-266">Менеджеры могли утверждать или отклонять контактные данные.</span><span class="sxs-lookup"><span data-stu-id="a0af0-266">Managers can approve or reject contact data.</span></span> <span data-ttu-id="a0af0-267">`Details` Представлении отображаются **утвердить** и **Отклонить** кнопки.</span><span class="sxs-lookup"><span data-stu-id="a0af0-267">The `Details` view shows **Approve** and **Reject** buttons.</span></span> 
* <span data-ttu-id="a0af0-268">Администраторы могут утвердить или отклонить и изменить или удалить все данные.</span><span class="sxs-lookup"><span data-stu-id="a0af0-268">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="a0af0-269">Пользователь</span><span class="sxs-lookup"><span data-stu-id="a0af0-269">User</span></span>| <span data-ttu-id="a0af0-270">Параметры</span><span class="sxs-lookup"><span data-stu-id="a0af0-270">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="a0af0-271">Можно изменить или удалить собственных данных</span><span class="sxs-lookup"><span data-stu-id="a0af0-271">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="a0af0-272">Можно утвердить или отклонить и изменить или удалить принадлежат данные</span><span class="sxs-lookup"><span data-stu-id="a0af0-272">Can approve/reject and edit/delete own data</span></span>  |
| admin@contoso.com | <span data-ttu-id="a0af0-273">Можно изменить или удалить и утвердить или отклонить все данные</span><span class="sxs-lookup"><span data-stu-id="a0af0-273">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="a0af0-274">Создание контакта в браузере администраторов.</span><span class="sxs-lookup"><span data-stu-id="a0af0-274">Create a contact in the administrators browser.</span></span> <span data-ttu-id="a0af0-275">Скопируйте URL-адрес для удаления и изменения из обратитесь к администратору.</span><span class="sxs-lookup"><span data-stu-id="a0af0-275">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="a0af0-276">Вставьте эти ссылки в браузер тестового пользователя, чтобы убедиться, что тестовый пользователь не может выполнять эти операции.</span><span class="sxs-lookup"><span data-stu-id="a0af0-276">Paste these links into the test user's browser to verify the test user cannot perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="a0af0-277">Создание начального уровня приложения</span><span class="sxs-lookup"><span data-stu-id="a0af0-277">Create the starter app</span></span>

<span data-ttu-id="a0af0-278">Выполните следующие действия для создания начального приложения.</span><span class="sxs-lookup"><span data-stu-id="a0af0-278">Follow these instructions to create the starter app.</span></span>

* <span data-ttu-id="a0af0-279">Создание **веб-приложения ASP.NET Core** с помощью [2017 г. Visual Studio](https://www.visualstudio.com/) с именем «ContactManager»</span><span class="sxs-lookup"><span data-stu-id="a0af0-279">Create an **ASP.NET Core Web Application** using [Visual Studio 2017](https://www.visualstudio.com/) named "ContactManager"</span></span>

  * <span data-ttu-id="a0af0-280">Создание приложения с **отдельных учетных записей пользователей**.</span><span class="sxs-lookup"><span data-stu-id="a0af0-280">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="a0af0-281">Присвойте ему имя «ContactManager», пространство имен будет соответствовать использование пространства имен в образце.</span><span class="sxs-lookup"><span data-stu-id="a0af0-281">Name it "ContactManager" so your namespace will match the namespace use in the sample.</span></span>

* <span data-ttu-id="a0af0-282">Добавьте следующие `Contact` модели:</span><span class="sxs-lookup"><span data-stu-id="a0af0-282">Add the following `Contact` model:</span></span>

  <span data-ttu-id="a0af0-283">[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="a0af0-283">[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]</span></span>

* <span data-ttu-id="a0af0-284">Представление формирования `Contact` модели с помощью Entity Framework Core и `ApplicationDbContext` контекста данных.</span><span class="sxs-lookup"><span data-stu-id="a0af0-284">Scaffold the `Contact` model using Entity Framework Core and the `ApplicationDbContext` data context.</span></span> <span data-ttu-id="a0af0-285">Примите значения по умолчанию формирование шаблонов.</span><span class="sxs-lookup"><span data-stu-id="a0af0-285">Accept all the scaffolding defaults.</span></span> <span data-ttu-id="a0af0-286">С помощью `ApplicationDbContext` для контекста данных класс помещает в таблице контактов [удостоверение](xref:security/authentication/identity) базы данных.</span><span class="sxs-lookup"><span data-stu-id="a0af0-286">Using `ApplicationDbContext` for the data context class  puts the contact table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="a0af0-287">В разделе [Добавление модели](xref:tutorials/first-mvc-app/adding-model) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="a0af0-287">See [Adding a model](xref:tutorials/first-mvc-app/adding-model) for more information.</span></span>

* <span data-ttu-id="a0af0-288">Обновление **ContactManager** привязки в *Views/Shared/_Layout.cshtml* файл из `asp-controller="Home"` для `asp-controller="Contacts"` таким образом коснувшись **ContactManager** ссылку вызывает контроллер контактов.</span><span class="sxs-lookup"><span data-stu-id="a0af0-288">Update the **ContactManager** anchor in the *Views/Shared/_Layout.cshtml* file from `asp-controller="Home"` to `asp-controller="Contacts"` so tapping the **ContactManager** link will invoke the Contacts controller.</span></span> <span data-ttu-id="a0af0-289">Исходной разметке:</span><span class="sxs-lookup"><span data-stu-id="a0af0-289">The original markup:</span></span>

```html
   <a asp-area="" asp-controller="Home" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

<span data-ttu-id="a0af0-290">Обновленная разметка:</span><span class="sxs-lookup"><span data-stu-id="a0af0-290">The updated markup:</span></span>

```html
   <a asp-area="" asp-controller="Contacts" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

* <span data-ttu-id="a0af0-291">Формировать первоначальной миграции и обновления базы данных</span><span class="sxs-lookup"><span data-stu-id="a0af0-291">Scaffold the initial migration and update the database</span></span>

```none
   dotnet ef migrations add initial
   dotnet ef database update
   ```

* <span data-ttu-id="a0af0-292">Протестируйте приложение, создание, изменение и удаление контакта</span><span class="sxs-lookup"><span data-stu-id="a0af0-292">Test the app by creating, editing and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="a0af0-293">Начальное значение базы данных</span><span class="sxs-lookup"><span data-stu-id="a0af0-293">Seed the database</span></span>

<span data-ttu-id="a0af0-294">Добавить `SeedData` класса *данные* папки.</span><span class="sxs-lookup"><span data-stu-id="a0af0-294">Add the `SeedData` class to the *Data* folder.</span></span> <span data-ttu-id="a0af0-295">Если загруженный образца можно скопировать *SeedData.cs* файл *данные* папки начальный проект.</span><span class="sxs-lookup"><span data-stu-id="a0af0-295">If you've downloaded the sample, you can copy the *SeedData.cs* file to the *Data* folder of the starter project.</span></span>

<span data-ttu-id="a0af0-296">[!code-csharp[Main](secure-data/samples/starter/Data/SeedData.cs)]</span><span class="sxs-lookup"><span data-stu-id="a0af0-296">[!code-csharp[Main](secure-data/samples/starter/Data/SeedData.cs)]</span></span>

<span data-ttu-id="a0af0-297">Добавьте выделенный код в конец `Configure` метод в *файла Startup.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="a0af0-297">Add the highlighted code to the end of the `Configure` method in the *Startup.cs* file:</span></span>

<span data-ttu-id="a0af0-298">[!code-csharp[Main](secure-data/samples/starter/Startup.cs?name=Configure&highlight=28-)]</span><span class="sxs-lookup"><span data-stu-id="a0af0-298">[!code-csharp[Main](secure-data/samples/starter/Startup.cs?name=Configure&highlight=28-)]</span></span>

<span data-ttu-id="a0af0-299">Тест заполняется, приложение базы данных.</span><span class="sxs-lookup"><span data-stu-id="a0af0-299">Test that the app seeded the database.</span></span> <span data-ttu-id="a0af0-300">Seed-метод не выполняется, если все строки в контакт DB.</span><span class="sxs-lookup"><span data-stu-id="a0af0-300">The seed method does not run if there are any rows in the contact DB.</span></span>

### <a name="create-a-class-used-in-the-tutorial"></a><span data-ttu-id="a0af0-301">Создайте класс, используемый в этом учебнике</span><span class="sxs-lookup"><span data-stu-id="a0af0-301">Create a class used in the tutorial</span></span>

* <span data-ttu-id="a0af0-302">Создайте папку с именем *авторизации*.</span><span class="sxs-lookup"><span data-stu-id="a0af0-302">Create a folder named *Authorization*.</span></span>
* <span data-ttu-id="a0af0-303">Копировать *Authorization\ContactOperations.cs* файла из загрузки завершенного проекта или скопируйте следующий код:</span><span class="sxs-lookup"><span data-stu-id="a0af0-303">Copy the *Authorization\ContactOperations.cs* file from the completed project download, or copy the following code:</span></span>

<span data-ttu-id="a0af0-304">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]</span><span class="sxs-lookup"><span data-stu-id="a0af0-304">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]</span></span>

<a name=secure-data-add-resources-label></a>

### <a name="additional-resources"></a><span data-ttu-id="a0af0-305">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="a0af0-305">Additional resources</span></span>

* <span data-ttu-id="a0af0-306">[Лаборатории авторизации ASP.NET Core](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="a0af0-306">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="a0af0-307">Эта лаборатория приведены более подробные сведения о средствах безопасности, представленные в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="a0af0-307">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="a0af0-308">Авторизация в ASP.NET Core: простой, роли, на основе утверждений и пользовательских</span><span class="sxs-lookup"><span data-stu-id="a0af0-308">Authorization in ASP.NET Core : Simple, role, claims-based and custom</span></span>](index.md)
* [<span data-ttu-id="a0af0-309">Пользовательская авторизация на основе политик</span><span class="sxs-lookup"><span data-stu-id="a0af0-309">Custom Policy-Based Authorization</span></span>](policies.md)
