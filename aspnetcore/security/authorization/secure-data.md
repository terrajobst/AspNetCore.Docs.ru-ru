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
ms.openlocfilehash: 000b14ddc1adb56c029d3da8ab0754215403ba79
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/13/2017
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="1404e-103">Создание приложения ASP.NET Core пользовательскими данными, защищенных авторизации</span><span class="sxs-lookup"><span data-stu-id="1404e-103">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="1404e-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Джо Одетт](https://twitter.com/joeaudette) (Joe Audette)</span><span class="sxs-lookup"><span data-stu-id="1404e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="1404e-105">Этого учебника показано, как создавать веб-приложения с данными пользователя, защищен авторизации.</span><span class="sxs-lookup"><span data-stu-id="1404e-105">This tutorial shows how to create a web app with user data protected by authorization.</span></span> <span data-ttu-id="1404e-106">Отображается список контактов, прошедшие проверку подлинности (зарегистрированные) пользователи создали.</span><span class="sxs-lookup"><span data-stu-id="1404e-106">It  displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="1404e-107">Существует три группы безопасности:</span><span class="sxs-lookup"><span data-stu-id="1404e-107">There are three security groups:</span></span>

* <span data-ttu-id="1404e-108">Зарегистрированные пользователи могут просматривать утвержденные контактных данных.</span><span class="sxs-lookup"><span data-stu-id="1404e-108">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="1404e-109">Зарегистрированные пользователи могут изменить или удалить свои собственные данные.</span><span class="sxs-lookup"><span data-stu-id="1404e-109">Registered users can edit/delete their own data.</span></span> 
* <span data-ttu-id="1404e-110">Менеджеры могли утверждать или отклонять контактные данные.</span><span class="sxs-lookup"><span data-stu-id="1404e-110">Managers can approve or reject contact data.</span></span> <span data-ttu-id="1404e-111">Только утвержденные контакты отображаются для пользователей.</span><span class="sxs-lookup"><span data-stu-id="1404e-111">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="1404e-112">Администраторы могут утвердить или отклонить и изменить или удалить все данные.</span><span class="sxs-lookup"><span data-stu-id="1404e-112">Administrators can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="1404e-113">На следующем рисунке пользователь Рик (`rick@example.com`) вход в систему.</span><span class="sxs-lookup"><span data-stu-id="1404e-113">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="1404e-114">Пользователь Рик может только представление утвержденные контакты и изменить или удалить его контактов.</span><span class="sxs-lookup"><span data-stu-id="1404e-114">User Rick can only view approved contacts and edit/delete his contacts.</span></span> <span data-ttu-id="1404e-115">Последней записи, созданные Рик, отображает изменение и удаление связей</span><span class="sxs-lookup"><span data-stu-id="1404e-115">Only the last record, created by Rick, displays edit and delete links</span></span>

![изображения, описанных выше](secure-data/_static/rick.png)

<span data-ttu-id="1404e-117">На следующем рисунке `manager@contoso.com` входит в и диспетчеры роль.</span><span class="sxs-lookup"><span data-stu-id="1404e-117">In the following image, `manager@contoso.com` is signed in and in the managers role.</span></span> 

![изображения, описанных выше](secure-data/_static/manager1.png)

<span data-ttu-id="1404e-119">На следующем рисунке менеджеров представление сведений для контакта.</span><span class="sxs-lookup"><span data-stu-id="1404e-119">The following image shows the  managers details view of a contact.</span></span>

![изображения, описанных выше](secure-data/_static/manager.png)

<span data-ttu-id="1404e-121">Только администраторы и диспетчеры имеют утверждение и отклонение кнопок.</span><span class="sxs-lookup"><span data-stu-id="1404e-121">Only managers and administrators have the approve and reject buttons.</span></span>

<span data-ttu-id="1404e-122">На следующем рисунке `admin@contoso.com` входит в и в роли администратора.</span><span class="sxs-lookup"><span data-stu-id="1404e-122">In the following image, `admin@contoso.com` is signed in and in the administrator’s role.</span></span> 

![изображения, описанных выше](secure-data/_static/admin.png)

<span data-ttu-id="1404e-124">Администратор имеет все права доступа.</span><span class="sxs-lookup"><span data-stu-id="1404e-124">The administrator has all privileges.</span></span> <span data-ttu-id="1404e-125">Она может чтения, изменить или удалить любой контакт и изменить состояние контактов.</span><span class="sxs-lookup"><span data-stu-id="1404e-125">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="1404e-126">Приложение создано с [формирование шаблонов](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) следующие `Contact` модели:</span><span class="sxs-lookup"><span data-stu-id="1404e-126">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller)  the following `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="1404e-127">Объект `ContactIsOwnerAuthorizationHandler` обработки авторизации гарантирует, что пользователь может редактировать только свои данные.</span><span class="sxs-lookup"><span data-stu-id="1404e-127">A `ContactIsOwnerAuthorizationHandler` authorization handler ensures that a user can only edit their data.</span></span> <span data-ttu-id="1404e-128">Объект `ContactManagerAuthorizationHandler` обработки авторизации менеджеры могут утверждать или отклонять контакты.</span><span class="sxs-lookup"><span data-stu-id="1404e-128">A `ContactManagerAuthorizationHandler` authorization handler allows managers to approve or reject contacts.</span></span>  <span data-ttu-id="1404e-129">Объект `ContactAdministratorsAuthorizationHandler` обработки авторизации позволяет администраторам утверждать или отклонять контакты и изменение/удаление контактов.</span><span class="sxs-lookup"><span data-stu-id="1404e-129">A `ContactAdministratorsAuthorizationHandler` authorization handler allows administrators to approve or reject contacts and to edit/delete contacts.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="1404e-130">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="1404e-130">Prerequisites</span></span>

<span data-ttu-id="1404e-131">Это не начало учебника.</span><span class="sxs-lookup"><span data-stu-id="1404e-131">This is not a beginning tutorial.</span></span> <span data-ttu-id="1404e-132">Вы должны быть знакомы с:</span><span class="sxs-lookup"><span data-stu-id="1404e-132">You should be familiar with:</span></span>

* [<span data-ttu-id="1404e-133">Основные ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="1404e-133">ASP.NET Core MVC</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="1404e-134">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="1404e-134">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="1404e-135">Начальная и завершенное приложение</span><span class="sxs-lookup"><span data-stu-id="1404e-135">The starter and completed app</span></span>

<span data-ttu-id="1404e-136">[Загрузить](xref:tutorials/index#how-to-download-a-sample) [завершения](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final) приложения.</span><span class="sxs-lookup"><span data-stu-id="1404e-136">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final) app.</span></span> <span data-ttu-id="1404e-137">[Тест](#test-the-completed-app) завершенное приложение, чтобы ознакомиться с функциями безопасности.</span><span class="sxs-lookup"><span data-stu-id="1404e-137">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span> 

### <a name="the-starter-app"></a><span data-ttu-id="1404e-138">Начального уровня приложения</span><span class="sxs-lookup"><span data-stu-id="1404e-138">The starter app</span></span>

<span data-ttu-id="1404e-139">Полезно для сравнения кода с помощью полного примера.</span><span class="sxs-lookup"><span data-stu-id="1404e-139">It's helpful to compare your code with the completed sample.</span></span>

<span data-ttu-id="1404e-140">[Загрузить](xref:tutorials/index#how-to-download-a-sample) [начальный](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter) приложения.</span><span class="sxs-lookup"><span data-stu-id="1404e-140">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter) app.</span></span> 

<span data-ttu-id="1404e-141">В разделе [создания начального приложения](#create-the-starter-app) Если вы хотите создать ее с нуля.</span><span class="sxs-lookup"><span data-stu-id="1404e-141">See [Create the starter app](#create-the-starter-app) if you'd like to create it from scratch.</span></span>

<span data-ttu-id="1404e-142">Обновление базы данных:</span><span class="sxs-lookup"><span data-stu-id="1404e-142">Update the database:</span></span>

```none
   dotnet ef database update
```

<span data-ttu-id="1404e-143">Запуск приложения, коснитесь **ContactManager** ссылку и убедитесь, создание, изменение и удаление контакта.</span><span class="sxs-lookup"><span data-stu-id="1404e-143">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

<span data-ttu-id="1404e-144">Этот учебник содержит основных шагов для создания приложения данных безопасности пользователей.</span><span class="sxs-lookup"><span data-stu-id="1404e-144">This tutorial has all the major steps to create the secure user data app.</span></span> <span data-ttu-id="1404e-145">Может быть полезен для обращения к завершенного проекта.</span><span class="sxs-lookup"><span data-stu-id="1404e-145">You may find it helpful to refer to the completed project.</span></span>

## <a name="modify-the-app-to-secure-user-data"></a><span data-ttu-id="1404e-146">Изменение приложения для защиты данных пользователя</span><span class="sxs-lookup"><span data-stu-id="1404e-146">Modify the app to secure user data</span></span>

<span data-ttu-id="1404e-147">Следующие разделы имеют основных шагов для создания приложения данных безопасности пользователей.</span><span class="sxs-lookup"><span data-stu-id="1404e-147">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="1404e-148">Может быть полезен для обращения к завершенного проекта.</span><span class="sxs-lookup"><span data-stu-id="1404e-148">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="1404e-149">Связать контактные данные для пользователя</span><span class="sxs-lookup"><span data-stu-id="1404e-149">Tie the contact data to the user</span></span>

<span data-ttu-id="1404e-150">Используйте ASP.NET [удостоверение](xref:security/authentication/identity) идентификатор пользователя, чтобы предоставить пользователям можно изменить свои данные, но не данные других пользователей.</span><span class="sxs-lookup"><span data-stu-id="1404e-150">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="1404e-151">Добавить `OwnerID` для `Contact` модели:</span><span class="sxs-lookup"><span data-stu-id="1404e-151">Add `OwnerID` to the `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/final/Models/Contact.cs?name=snippet1&highlight=5-6,16-)]

<span data-ttu-id="1404e-152">`OwnerID`идентификатор пользователя из `AspNetUser` в таблицу [удостоверение](xref:security/authentication/identity) базы данных.</span><span class="sxs-lookup"><span data-stu-id="1404e-152">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="1404e-153">`Status` Поле определяет, является ли контакт для просмотра обычными пользователями.</span><span class="sxs-lookup"><span data-stu-id="1404e-153">The `Status` field determines if a contact is viewable by general users.</span></span> 

<span data-ttu-id="1404e-154">Сформировать новый миграции и обновления базы данных:</span><span class="sxs-lookup"><span data-stu-id="1404e-154">Scaffold a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
 ```

### <a name="require-ssl-and-authenticated-users"></a><span data-ttu-id="1404e-155">Требовать SSL и проверку подлинности пользователей</span><span class="sxs-lookup"><span data-stu-id="1404e-155">Require SSL and authenticated users</span></span>

<span data-ttu-id="1404e-156">В `ConfigureServices` метод *файла Startup.cs* файл, добавьте [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) фильтр авторизации:</span><span class="sxs-lookup"><span data-stu-id="1404e-156">In the `ConfigureServices` method of the *Startup.cs* file, add the [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) authorization filter:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_SSL&highlight=1)]

<span data-ttu-id="1404e-157">Для перенаправления HTTP-запросы HTTPS, в разделе [по промежуточного слоя перезаписи URL-адрес](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="1404e-157">To redirect HTTP requests to HTTPS, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="1404e-158">Если вы используете Visual Studio Code или тестирование на локальной платформе, которая не включает тестового сертификата для SSL:</span><span class="sxs-lookup"><span data-stu-id="1404e-158">If you are using Visual Studio Code or testing on local platform that doesn't include a test certificate for SSL:</span></span>

- <span data-ttu-id="1404e-159">Задать `"LocalTest:skipSSL": true` в *appsettings.json* файла.</span><span class="sxs-lookup"><span data-stu-id="1404e-159">Set `"LocalTest:skipSSL": true` in the *appsettings.json* file.</span></span>

### <a name="require-authenticated-users"></a><span data-ttu-id="1404e-160">Требовать проверку подлинности пользователей</span><span class="sxs-lookup"><span data-stu-id="1404e-160">Require authenticated users</span></span>

<span data-ttu-id="1404e-161">Задайте политику проверки подлинности по умолчанию, чтобы требовать проверку подлинности пользователей.</span><span class="sxs-lookup"><span data-stu-id="1404e-161">Set the default authentication policy to require users to be authenticated.</span></span> <span data-ttu-id="1404e-162">Можно отключить проверку подлинности на контроллеру или методу действия с `[AllowAnonymous]` атрибута.</span><span class="sxs-lookup"><span data-stu-id="1404e-162">You can opt out of authentication at the controller or action method with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="1404e-163">Таким образом, любые новые контроллеры добавлен автоматически требуется проверка подлинности, которая является более безопасным, чем полагаться на новые контроллеры для включения `[Authorize]` атрибута.</span><span class="sxs-lookup"><span data-stu-id="1404e-163">With this approach, any new controllers added will automatically require authentication, which is safer than relying on new controllers to include the `[Authorize]` attribute.</span></span> <span data-ttu-id="1404e-164">Добавьте следующий код в `ConfigureServices` метод *файла Startup.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="1404e-164">Add the following to  the `ConfigureServices` method of the *Startup.cs* file:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_defaultPolicy)]

<span data-ttu-id="1404e-165">Добавить `[AllowAnonymous]` , анонимные пользователи могут получить сведения о веб-сайте, прежде чем они зарегистрировать контроллер home.</span><span class="sxs-lookup"><span data-stu-id="1404e-165">Add `[AllowAnonymous]` to the home controller so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/HomeController.cs?name=snippet1&highlight=2,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="1404e-166">Настройка тестовой учетной записи</span><span class="sxs-lookup"><span data-stu-id="1404e-166">Configure the test account</span></span>

<span data-ttu-id="1404e-167">`SeedData` Класс создает две учетные записи, администратора и диспетчера.</span><span class="sxs-lookup"><span data-stu-id="1404e-167">The `SeedData` class creates two accounts,  administrator and manager.</span></span> <span data-ttu-id="1404e-168">Используйте [секрет диспетчера](xref:security/app-secrets) , чтобы задать пароль для этих учетных записей.</span><span class="sxs-lookup"><span data-stu-id="1404e-168">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="1404e-169">Это можно сделать из каталога проекта (каталог, содержащий *Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="1404e-169">Do this from the project directory (the directory containing *Program.cs*).</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="1404e-170">Обновление `Configure` использовать пароль теста:</span><span class="sxs-lookup"><span data-stu-id="1404e-170">Update `Configure` to use the test password:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=Configure&highlight=19-21)]

<span data-ttu-id="1404e-171">Добавьте идентификатор пользователя администратора и `Status = ContactStatus.Approved` контактам.</span><span class="sxs-lookup"><span data-stu-id="1404e-171">Add the administrator user ID and `Status = ContactStatus.Approved` to the contacts.</span></span> <span data-ttu-id="1404e-172">Отображается только один контакт, добавьте все контакты идентификатор пользователя:</span><span class="sxs-lookup"><span data-stu-id="1404e-172">Only one contact is shown, add the user ID to all contacts:</span></span>

[!code-csharp[Main](secure-data/samples/final/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="1404e-173">Создать владельца, диспетчер и обработчики авторизации администратора</span><span class="sxs-lookup"><span data-stu-id="1404e-173">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="1404e-174">Создание `ContactIsOwnerAuthorizationHandler` класса в *авторизации* папки.</span><span class="sxs-lookup"><span data-stu-id="1404e-174">Create a `ContactIsOwnerAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="1404e-175">`ContactIsOwnerAuthorizationHandler` Проверит пользователя, действующий от ресурса, которому принадлежит ресурс.</span><span class="sxs-lookup"><span data-stu-id="1404e-175">The `ContactIsOwnerAuthorizationHandler` will verify the user acting on the resource owns the resource.</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="1404e-176">`ContactIsOwnerAuthorizationHandler` Вызовы `context.Succeed` Если контактные владельцем текущего пользователя, прошедшего проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="1404e-176">The `ContactIsOwnerAuthorizationHandler` calls `context.Succeed` if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="1404e-177">Обработчики авторизации обычно возвращают `context.Succeed` при выполнение требований.</span><span class="sxs-lookup"><span data-stu-id="1404e-177">Authorization handlers generally return `context.Succeed` when the requirements are met.</span></span> <span data-ttu-id="1404e-178">Они возвращают `Task.FromResult(0)` Если требования не выполнены.</span><span class="sxs-lookup"><span data-stu-id="1404e-178">They return `Task.FromResult(0)` when requirements are not met.</span></span> <span data-ttu-id="1404e-179">`Task.FromResult(0)`— ни успех или неудача, она позволяет другому обработчику авторизации для выполнения.</span><span class="sxs-lookup"><span data-stu-id="1404e-179">`Task.FromResult(0)` is neither success or failure, it allows other authorization handler to run.</span></span> <span data-ttu-id="1404e-180">Если необходимо выполнить явную ошибку, возвратить `context.Fail()`.</span><span class="sxs-lookup"><span data-stu-id="1404e-180">If you need to explicitly fail, return `context.Fail()`.</span></span>

<span data-ttu-id="1404e-181">Мы даем контакта владельцев, изменить или удалить свои собственные данные, поэтому мы не нужно проверять операции, передаваемые в качестве параметра требование.</span><span class="sxs-lookup"><span data-stu-id="1404e-181">We allow contact owners to edit/delete their own data, so we don't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="1404e-182">Создание обработчика диспетчера авторизации</span><span class="sxs-lookup"><span data-stu-id="1404e-182">Create a manager authorization handler</span></span>

<span data-ttu-id="1404e-183">Создание `ContactManagerAuthorizationHandler` класса в *авторизации* папки.</span><span class="sxs-lookup"><span data-stu-id="1404e-183">Create a `ContactManagerAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="1404e-184">`ContactManagerAuthorizationHandler` Проверит пользователя, действующий от ресурса является менеджером.</span><span class="sxs-lookup"><span data-stu-id="1404e-184">The `ContactManagerAuthorizationHandler` will verify the user acting on the resource is a manager.</span></span> <span data-ttu-id="1404e-185">Данные только менеджеров можно утвердить или отклонить изменения содержимого (новые или измененные).</span><span class="sxs-lookup"><span data-stu-id="1404e-185">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="1404e-186">Создайте обработчик авторизации администратора</span><span class="sxs-lookup"><span data-stu-id="1404e-186">Create an administrator authorization handler</span></span>

<span data-ttu-id="1404e-187">Создание `ContactAdministratorsAuthorizationHandler` класса в *авторизации* папки.</span><span class="sxs-lookup"><span data-stu-id="1404e-187">Create a `ContactAdministratorsAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="1404e-188">`ContactAdministratorsAuthorizationHandler` Проверит действует для ресурса пользователь является администратором.</span><span class="sxs-lookup"><span data-stu-id="1404e-188">The `ContactAdministratorsAuthorizationHandler` will verify the user acting on the resource is a administrator.</span></span> <span data-ttu-id="1404e-189">Администратор может выполнять все операции.</span><span class="sxs-lookup"><span data-stu-id="1404e-189">Administrator can do all operations.</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="1404e-190">Регистрировать обработчики авторизации</span><span class="sxs-lookup"><span data-stu-id="1404e-190">Register the authorization handlers</span></span>

<span data-ttu-id="1404e-191">С помощью Entity Framework Core Services должны быть зарегистрированы для [внедрения зависимостей](xref:fundamentals/dependency-injection) с помощью [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="1404e-191">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="1404e-192">`ContactIsOwnerAuthorizationHandler` Использует ASP.NET Core [удостоверения](xref:security/authentication/identity), которые построены на Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="1404e-192">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="1404e-193">Зарегистрировать обработчики с коллекцией служб, они будут доступны для `ContactsController` через [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="1404e-193">Register the handlers with the service collection so they will be available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="1404e-194">Добавьте следующий код в конец `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="1404e-194">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=AuthorizationHandlers)]

<span data-ttu-id="1404e-195">`ContactAdministratorsAuthorizationHandler`и `ContactManagerAuthorizationHandler` добавляются как одноэлементных кортежей.</span><span class="sxs-lookup"><span data-stu-id="1404e-195">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="1404e-196">Они являются единственных экземпляров, так как они не используют EF и все сведения, необходимые в `Context` параметр `HandleRequirementAsync` метода.</span><span class="sxs-lookup"><span data-stu-id="1404e-196">They are singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

<span data-ttu-id="1404e-197">Полный `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="1404e-197">The complete `ConfigureServices`:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=ConfigureServices)]

## <a name="update-the-code-to-support-authorization"></a><span data-ttu-id="1404e-198">Обновление кода для поддержки авторизации</span><span class="sxs-lookup"><span data-stu-id="1404e-198">Update the code to support authorization</span></span>

<span data-ttu-id="1404e-199">В этом разделе обновления контроллера и представлений и добавить класс требований операции.</span><span class="sxs-lookup"><span data-stu-id="1404e-199">In this section, you update the controller and views and add an operations requirements class.</span></span>

### <a name="update-the-contacts-controller"></a><span data-ttu-id="1404e-200">Обновление контроллера контактов</span><span class="sxs-lookup"><span data-stu-id="1404e-200">Update the Contacts controller</span></span>

<span data-ttu-id="1404e-201">Обновление `ContactsController` конструктор:</span><span class="sxs-lookup"><span data-stu-id="1404e-201">Update the `ContactsController` constructor:</span></span>

* <span data-ttu-id="1404e-202">Добавить `IAuthorizationService` службы для доступа к обработчикам авторизации.</span><span class="sxs-lookup"><span data-stu-id="1404e-202">Add the `IAuthorizationService` service to  access to the authorization handlers.</span></span> 
* <span data-ttu-id="1404e-203">Добавить `Identity` `UserManager` службы:</span><span class="sxs-lookup"><span data-stu-id="1404e-203">Add the `Identity` `UserManager` service:</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_ContactsControllerCtor)]

### <a name="add-a-contact-operations-requirements-class"></a><span data-ttu-id="1404e-204">Добавьте класс требования операции контактов</span><span class="sxs-lookup"><span data-stu-id="1404e-204">Add a contact operations requirements class</span></span>

<span data-ttu-id="1404e-205">Добавить `ContactOperationsRequirements` класса *авторизации* папки.</span><span class="sxs-lookup"><span data-stu-id="1404e-205">Add the `ContactOperationsRequirements` class to the *Authorization* folder.</span></span> <span data-ttu-id="1404e-206">Этот класс содержит требованиям наших поддерживает приложения:</span><span class="sxs-lookup"><span data-stu-id="1404e-206">This class  contain the requirements our app supports:</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]

### <a name="update-create"></a><span data-ttu-id="1404e-207">Создание обновления</span><span class="sxs-lookup"><span data-stu-id="1404e-207">Update Create</span></span>

<span data-ttu-id="1404e-208">Обновление `HTTP POST Create` метода:</span><span class="sxs-lookup"><span data-stu-id="1404e-208">Update the `HTTP POST Create` method to:</span></span>

* <span data-ttu-id="1404e-209">Добавьте идентификатор пользователя, который `Contact` модели.</span><span class="sxs-lookup"><span data-stu-id="1404e-209">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="1404e-210">Вызывает обработчик авторизации, чтобы убедиться, что пользователь является владельцем контакта.</span><span class="sxs-lookup"><span data-stu-id="1404e-210">Call the authorization handler to verify the user owns the contact.</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Create)]

### <a name="update-edit"></a><span data-ttu-id="1404e-211">Изменение обновления</span><span class="sxs-lookup"><span data-stu-id="1404e-211">Update Edit</span></span>

<span data-ttu-id="1404e-212">Обновлять `Edit` методов на использование обработчика авторизации, чтобы убедиться, что пользователь является владельцем контакта.</span><span class="sxs-lookup"><span data-stu-id="1404e-212">Update both `Edit` methods to use the authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="1404e-213">Так как мы занимаемся авторизации ресурсов не может использовать `[Authorize]` атрибута.</span><span class="sxs-lookup"><span data-stu-id="1404e-213">Because we are performing resource authorization we cannot use the `[Authorize]` attribute.</span></span> <span data-ttu-id="1404e-214">При оценке атрибуты нас нет доступа к ресурсу.</span><span class="sxs-lookup"><span data-stu-id="1404e-214">We don't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="1404e-215">Авторизация на основе ресурсов должно быть принудительной.</span><span class="sxs-lookup"><span data-stu-id="1404e-215">Resource based authorization must be imperative.</span></span> <span data-ttu-id="1404e-216">После получения доступа к ресурсу, загрузив его в нашем контроллера или его загрузке в сам обработчик должен выполняться проверки.</span><span class="sxs-lookup"><span data-stu-id="1404e-216">Checks must be performed once we have access to the resource, either by loading it in our controller, or by loading it within the handler itself.</span></span> <span data-ttu-id="1404e-217">Часто будет доступ к ресурсу, передавая ключ ресурса.</span><span class="sxs-lookup"><span data-stu-id="1404e-217">Frequently you will access the resource by passing in the resource key.</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Edit)]

### <a name="update-the-delete-method"></a><span data-ttu-id="1404e-218">Метод удаления обновления</span><span class="sxs-lookup"><span data-stu-id="1404e-218">Update the Delete method</span></span>

<span data-ttu-id="1404e-219">Обновлять `Delete` методов на использование обработчика авторизации, чтобы убедиться, что пользователь является владельцем контакта.</span><span class="sxs-lookup"><span data-stu-id="1404e-219">Update both `Delete` methods to use the authorization handler to verify the user owns the contact.</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Delete)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="1404e-220">Внедрить службы авторизации в представления</span><span class="sxs-lookup"><span data-stu-id="1404e-220">Inject the authorization service into the views</span></span>

<span data-ttu-id="1404e-221">В настоящее время показано пользовательского интерфейса, изменение и удаление ссылки на данные, которые не может быть изменено пользователем.</span><span class="sxs-lookup"><span data-stu-id="1404e-221">Currently the UI shows edit and delete links for data the user cannot modify.</span></span> <span data-ttu-id="1404e-222">Мы исправим это путем применения авторизации обработчика представлений.</span><span class="sxs-lookup"><span data-stu-id="1404e-222">We'll fix that by applying the authorization handler to the views.</span></span>

<span data-ttu-id="1404e-223">Запустить службу авторизации в *Views/_ViewImports.cshtml* файл, он будет доступен для всех представлений:</span><span class="sxs-lookup"><span data-stu-id="1404e-223">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it will be available to all views:</span></span>

[!code-html[Main](secure-data/samples/final/Views/_ViewImports.cshtml)]

<span data-ttu-id="1404e-224">Обновление *Views/Contacts/Index.cshtml* представления Razor, чтобы отображались только редактирование удалить ссылки для пользователей, которые можно изменить или удалить контакт.</span><span class="sxs-lookup"><span data-stu-id="1404e-224">Update the *Views/Contacts/Index.cshtml* Razor view to only display the edit and delete links for users who can edit/delete the contact.</span></span>

<span data-ttu-id="1404e-225">Добавить`@using ContactManager.Authorization;`</span><span class="sxs-lookup"><span data-stu-id="1404e-225">Add `@using ContactManager.Authorization;`</span></span>

<span data-ttu-id="1404e-226">Обновление `Edit` и `Delete` ссылки, они отображаются только пользователи с разрешением на изменение и удаление контакта.</span><span class="sxs-lookup"><span data-stu-id="1404e-226">Update the `Edit` and `Delete` links so they are only rendered for users with permission to edit and delete the contact.</span></span>

[!code-html[Main](secure-data/samples/final/Views/Contacts/Index.cshtml?range=63-84)]

<span data-ttu-id="1404e-227">Предупреждение: Скрытие ссылок от пользователей, у которых нет разрешения на изменение и удаление данных не обеспечивает безопасность приложения.</span><span class="sxs-lookup"><span data-stu-id="1404e-227">Warning: Hiding links from users that do not have permission to edit or delete data does not secure the app.</span></span> <span data-ttu-id="1404e-228">Скрытие ссылки делает приложение пользователя более понятное, отображая только допустимые ссылки.</span><span class="sxs-lookup"><span data-stu-id="1404e-228">Hiding links makes the app more user friendly by displaying only valid links.</span></span> <span data-ttu-id="1404e-229">Пользователи могут hack созданные URL-адреса, вызывать изменение и удаление операций с данными, которыми они не владеют.</span><span class="sxs-lookup"><span data-stu-id="1404e-229">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span>  <span data-ttu-id="1404e-230">Контроллер должен повторять проверку доступа для обеспечения безопасности.</span><span class="sxs-lookup"><span data-stu-id="1404e-230">The controller must repeat the access checks to be secure.</span></span>

### <a name="update-the-details-view"></a><span data-ttu-id="1404e-231">Обновить представление сведений</span><span class="sxs-lookup"><span data-stu-id="1404e-231">Update the Details view</span></span>

<span data-ttu-id="1404e-232">Обновление представления сведений, чтобы менеджеры могли утверждать или отклонять контакты:</span><span class="sxs-lookup"><span data-stu-id="1404e-232">Update the details view so managers can approve or reject contacts:</span></span>

[!code-html[Main](secure-data/samples/final/Views/Contacts/Details.cshtml?range=53-)]

## <a name="test-the-completed-app"></a><span data-ttu-id="1404e-233">Тестирование завершенного приложения</span><span class="sxs-lookup"><span data-stu-id="1404e-233">Test the completed app</span></span>

<span data-ttu-id="1404e-234">Если вы используете Visual Studio Code или тестирование на локальной платформе, которая не включает тестового сертификата для SSL:</span><span class="sxs-lookup"><span data-stu-id="1404e-234">If you are using Visual Studio Code or testing on local platform that doesn't include a test certificate for SSL:</span></span>

- <span data-ttu-id="1404e-235">Задать `"LocalTest:skipSSL": true` в *appsettings.json* файла.</span><span class="sxs-lookup"><span data-stu-id="1404e-235">Set `"LocalTest:skipSSL": true` in the *appsettings.json* file.</span></span>

<span data-ttu-id="1404e-236">Если вы запускали приложение и контактов, удалите все записи в `Contact` таблице и перезапустить приложение для инициализации базы данных.</span><span class="sxs-lookup"><span data-stu-id="1404e-236">If you have run the app and have contacts, delete all the records in the `Contact` table and restart the app to seed the database.</span></span> <span data-ttu-id="1404e-237">Если вы используете Visual Studio, необходимо закрыть и перезапустить IIS Express для инициализации базы данных.</span><span class="sxs-lookup"><span data-stu-id="1404e-237">If you are using Visual Studio, you need to exit and restart IIS Express to seed the database.</span></span>

<span data-ttu-id="1404e-238">Регистрация пользователя для поиска контактов.</span><span class="sxs-lookup"><span data-stu-id="1404e-238">Register a user to browse the contacts.</span></span>

<span data-ttu-id="1404e-239">Простой способ тестирования завершенное приложение — запуск три разных браузерах (или версий браузера или InPrivate).</span><span class="sxs-lookup"><span data-stu-id="1404e-239">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="1404e-240">В одном браузере регистрации нового пользователя, например, `test@example.com`.</span><span class="sxs-lookup"><span data-stu-id="1404e-240">In one browser, register a new user, for example, `test@example.com`.</span></span> <span data-ttu-id="1404e-241">Вход для каждого веб-обозревателя с другим пользователем.</span><span class="sxs-lookup"><span data-stu-id="1404e-241">Sign in to each browser with a different user.</span></span> <span data-ttu-id="1404e-242">Проверьте следующее.</span><span class="sxs-lookup"><span data-stu-id="1404e-242">Verify the following:</span></span>

* <span data-ttu-id="1404e-243">Зарегистрированные пользователи могут просматривать утвержденные контактных данных.</span><span class="sxs-lookup"><span data-stu-id="1404e-243">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="1404e-244">Зарегистрированные пользователи могут изменить или удалить свои собственные данные.</span><span class="sxs-lookup"><span data-stu-id="1404e-244">Registered users can edit/delete their own data.</span></span> 
* <span data-ttu-id="1404e-245">Менеджеры могли утверждать или отклонять контактные данные.</span><span class="sxs-lookup"><span data-stu-id="1404e-245">Managers can approve or reject contact data.</span></span> <span data-ttu-id="1404e-246">`Details` Представлении отображаются **утвердить** и **Отклонить** кнопки.</span><span class="sxs-lookup"><span data-stu-id="1404e-246">The `Details` view shows **Approve** and **Reject** buttons.</span></span> 
* <span data-ttu-id="1404e-247">Администраторы могут утвердить или отклонить и изменить или удалить все данные.</span><span class="sxs-lookup"><span data-stu-id="1404e-247">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="1404e-248">Пользователь</span><span class="sxs-lookup"><span data-stu-id="1404e-248">User</span></span>| <span data-ttu-id="1404e-249">Параметры</span><span class="sxs-lookup"><span data-stu-id="1404e-249">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="1404e-250">Можно изменить или удалить собственных данных</span><span class="sxs-lookup"><span data-stu-id="1404e-250">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="1404e-251">Можно утвердить или отклонить и изменить или удалить принадлежат данные</span><span class="sxs-lookup"><span data-stu-id="1404e-251">Can approve/reject and edit/delete own data</span></span>  |
| admin@contoso.com | <span data-ttu-id="1404e-252">Можно изменить или удалить и утвердить или отклонить все данные</span><span class="sxs-lookup"><span data-stu-id="1404e-252">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="1404e-253">Создание контакта в браузере администраторов.</span><span class="sxs-lookup"><span data-stu-id="1404e-253">Create a contact in the administrators browser.</span></span> <span data-ttu-id="1404e-254">Скопируйте URL-адрес для удаления и изменения из обратитесь к администратору.</span><span class="sxs-lookup"><span data-stu-id="1404e-254">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="1404e-255">Вставьте эти ссылки в браузер тестового пользователя, чтобы убедиться, что тестовый пользователь не может выполнять эти операции.</span><span class="sxs-lookup"><span data-stu-id="1404e-255">Paste these links into the test user's browser to verify the test user cannot perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="1404e-256">Создание начального уровня приложения</span><span class="sxs-lookup"><span data-stu-id="1404e-256">Create the starter app</span></span>

<span data-ttu-id="1404e-257">Выполните следующие действия для создания начального приложения.</span><span class="sxs-lookup"><span data-stu-id="1404e-257">Follow these instructions to create the starter app.</span></span>

* <span data-ttu-id="1404e-258">Создание **веб-приложения ASP.NET Core** с помощью [2017 г. Visual Studio](https://www.visualstudio.com/) с именем «ContactManager»</span><span class="sxs-lookup"><span data-stu-id="1404e-258">Create an **ASP.NET Core Web Application** using [Visual Studio 2017](https://www.visualstudio.com/) named "ContactManager"</span></span>

  * <span data-ttu-id="1404e-259">Создание приложения с **отдельных учетных записей пользователей**.</span><span class="sxs-lookup"><span data-stu-id="1404e-259">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="1404e-260">Присвойте ему имя «ContactManager», пространство имен будет соответствовать использование пространства имен в образце.</span><span class="sxs-lookup"><span data-stu-id="1404e-260">Name it "ContactManager" so your namespace will match the namespace use in the sample.</span></span>

* <span data-ttu-id="1404e-261">Добавьте следующие `Contact` модели:</span><span class="sxs-lookup"><span data-stu-id="1404e-261">Add the following `Contact` model:</span></span>

  [!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="1404e-262">Представление формирования `Contact` модели с помощью Entity Framework Core и `ApplicationDbContext` контекста данных.</span><span class="sxs-lookup"><span data-stu-id="1404e-262">Scaffold the `Contact` model using Entity Framework Core and the `ApplicationDbContext` data context.</span></span> <span data-ttu-id="1404e-263">Примите значения по умолчанию формирование шаблонов.</span><span class="sxs-lookup"><span data-stu-id="1404e-263">Accept all the scaffolding defaults.</span></span> <span data-ttu-id="1404e-264">С помощью `ApplicationDbContext` для контекста данных класс помещает в таблице контактов [удостоверение](xref:security/authentication/identity) базы данных.</span><span class="sxs-lookup"><span data-stu-id="1404e-264">Using `ApplicationDbContext` for the data context class  puts the contact table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="1404e-265">В разделе [Добавление модели](xref:tutorials/first-mvc-app/adding-model) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="1404e-265">See [Adding a model](xref:tutorials/first-mvc-app/adding-model) for more information.</span></span>

* <span data-ttu-id="1404e-266">Обновление **ContactManager** привязки в *Views/Shared/_Layout.cshtml* файл из `asp-controller="Home"` для `asp-controller="Contacts"` таким образом коснувшись **ContactManager** ссылку вызывает контроллер контактов.</span><span class="sxs-lookup"><span data-stu-id="1404e-266">Update the **ContactManager** anchor in the *Views/Shared/_Layout.cshtml* file from `asp-controller="Home"` to `asp-controller="Contacts"` so tapping the **ContactManager** link will invoke the Contacts controller.</span></span> <span data-ttu-id="1404e-267">Исходной разметке:</span><span class="sxs-lookup"><span data-stu-id="1404e-267">The original markup:</span></span>

```html
   <a asp-area="" asp-controller="Home" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

<span data-ttu-id="1404e-268">Обновленная разметка:</span><span class="sxs-lookup"><span data-stu-id="1404e-268">The updated markup:</span></span>

```html
   <a asp-area="" asp-controller="Contacts" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

* <span data-ttu-id="1404e-269">Формировать первоначальной миграции и обновления базы данных</span><span class="sxs-lookup"><span data-stu-id="1404e-269">Scaffold the initial migration and update the database</span></span>

```none
   dotnet ef migrations add initial
   dotnet ef database update
   ```

* <span data-ttu-id="1404e-270">Протестируйте приложение, создание, изменение и удаление контакта</span><span class="sxs-lookup"><span data-stu-id="1404e-270">Test the app by creating, editing and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="1404e-271">Заполнение базы данных</span><span class="sxs-lookup"><span data-stu-id="1404e-271">Seed the database</span></span>

<span data-ttu-id="1404e-272">Добавить `SeedData` класса *данные* папки.</span><span class="sxs-lookup"><span data-stu-id="1404e-272">Add the `SeedData` class to the *Data* folder.</span></span> <span data-ttu-id="1404e-273">Если загруженный образца можно скопировать *SeedData.cs* файл *данные* папки начальный проект.</span><span class="sxs-lookup"><span data-stu-id="1404e-273">If you've downloaded the sample, you can copy the *SeedData.cs* file to the *Data* folder of the starter project.</span></span>

[!code-csharp[Main](secure-data/samples/starter/Data/SeedData.cs)]

<span data-ttu-id="1404e-274">Добавьте выделенный код в конец `Configure` метод в *файла Startup.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="1404e-274">Add the highlighted code to the end of the `Configure` method in the *Startup.cs* file:</span></span>

[!code-csharp[Main](secure-data/samples/starter/Startup.cs?name=Configure&highlight=28-)]

<span data-ttu-id="1404e-275">Тест заполняется, приложение базы данных.</span><span class="sxs-lookup"><span data-stu-id="1404e-275">Test that the app seeded the database.</span></span> <span data-ttu-id="1404e-276">Seed-метод не выполняется, если все строки в контакт DB.</span><span class="sxs-lookup"><span data-stu-id="1404e-276">The seed method does not run if there are any rows in the contact DB.</span></span>

### <a name="create-a-class-used-in-the-tutorial"></a><span data-ttu-id="1404e-277">Создайте класс, используемый в этом учебнике</span><span class="sxs-lookup"><span data-stu-id="1404e-277">Create a class used in the tutorial</span></span>

* <span data-ttu-id="1404e-278">Создайте папку с именем *авторизации*.</span><span class="sxs-lookup"><span data-stu-id="1404e-278">Create a folder named *Authorization*.</span></span>
* <span data-ttu-id="1404e-279">Копировать *Authorization\ContactOperations.cs* файла из загрузки завершенного проекта или скопируйте следующий код:</span><span class="sxs-lookup"><span data-stu-id="1404e-279">Copy the *Authorization\ContactOperations.cs* file from the completed project download, or copy the following code:</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="1404e-280">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="1404e-280">Additional resources</span></span>

* <span data-ttu-id="1404e-281">[Лаборатории авторизации ASP.NET Core](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="1404e-281">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="1404e-282">Эта лаборатория приведены более подробные сведения о средствах безопасности, представленные в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="1404e-282">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="1404e-283">Авторизация в ASP.NET Core: простой, роли, на основе утверждений и пользовательских</span><span class="sxs-lookup"><span data-stu-id="1404e-283">Authorization in ASP.NET Core : Simple, role, claims-based and custom</span></span>](index.md)
* [<span data-ttu-id="1404e-284">Пользовательская авторизация на основе политик</span><span class="sxs-lookup"><span data-stu-id="1404e-284">Custom Policy-Based Authorization</span></span>](policies.md)
