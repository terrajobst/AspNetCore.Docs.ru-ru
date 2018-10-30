---
title: Создание приложения ASP.NET Core с помощью данных пользователя с помощью авторизации
author: rick-anderson
description: Узнайте, как создать приложение Razor Pages с помощью данных пользователя с помощью авторизации. Включает протокол HTTPS, проверка подлинности и безопасность, удостоверения ASP.NET Core.
ms.author: riande
ms.date: 7/24/2018
uid: security/authorization/secure-data
ms.openlocfilehash: 786e83eb28722e83eea46def962593f58ad524e2
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244844"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a>Создание приложения ASP.NET Core с помощью данных пользователя с помощью авторизации

Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Джо Одетт](https://twitter.com/joeaudette) (Joe Audette)

::: moniker range="<= aspnetcore-1.1"

См. в разделе [этот PDF-ФАЙЛ](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) для версии ASP.NET Core MVC. Версия ASP.NET Core 1.1 работы с этим руководством, [это](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) папки. 1.1, пример ASP.NET Core находится в [примеры](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

См. в разделе [этот PDF-файл](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Этом руководстве показано, как создать веб-приложение ASP.NET Core с помощью данных пользователя с помощью авторизации. Отображается список контактов, прошедшие проверку подлинности (зарегистрированного) пользователи создали. Существует три группы безопасности:

* **Зарегистрированные пользователи** может просматривать все данные, утвержденных и может изменять и удалять свои собственные данные.
* **Диспетчеры** можно утвердить или отклонить контактных данных. Только утвержденные контакты отображаются для пользователей.
* **Администраторы** можно утверждать или отклонять и изменить или удалить все данные.

На следующем рисунке, пользователь Рик (`rick@example.com`) выполнил вход. Рик могут только просматривать утвержденные контакты и **изменить**/**удалить**/**Create New** ссылки на свои контакты. Только последней записи, созданные Рик, отображает **изменить** и **удалить** ссылки. Другие пользователи увидят последнюю запись, после менеджеру или администратору изменяет состояние на «Утверждено».

![изображение описанные выше](secure-data/_static/rick.png)

На следующем рисунке `manager@contoso.com` подписан в и в роли managers:

![изображение описанные выше](secure-data/_static/manager1.png)

На следующем рисунке показана менеджеров Просмотр подробностей контакта:

![изображение описанные выше](secure-data/_static/manager.png)

**Утвердить** и **Отклонить** кнопки отображаются только для менеджеров и администраторов.

На следующем рисунке `admin@contoso.com` подписан в и в роли "Администраторы":

![изображение описанные выше](secure-data/_static/admin.png)

Администратор имеет все привилегии. Она может чтения, изменить или удалить любой контакт и изменить состояние контактов.

Приложение было создано [формирования шаблонов](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) следующие `Contact` модели:

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet)]

Пример содержит следующие обработчики авторизации:

* `ContactIsOwnerAuthorizationHandler`: Гарантирует, что пользователь может изменить только свои данные.
* `ContactManagerAuthorizationHandler`: Менеджеры для утверждения или отклонения контактов.
* `ContactAdministratorsAuthorizationHandler`: Позволяет администраторам для утверждения или отклонения контакты и изменить или удалить контакты.

## <a name="prerequisites"></a>Предварительные требования

Этот учебник является дополнительным. Вы должны быть знакомы с:

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [Authentication](xref:security/authentication/index)
* [Подтверждение учетной записи и восстановление пароля](xref:security/authentication/accconfirm)
* [Авторизация](xref:security/authorization/index)
* [Entity Framework Core](xref:data/ef-mvc/intro)

::: moniker-end

::: moniker range="= aspnetcore-2.1"

В ASP.NET Core 2.1 `User.IsInRole` завершается сбоем при использовании `AddDefaultIdentity`. В этом руководстве используется `AddDefaultIdentity` и поэтому нуждается в предварительной версии ASP.NET Core 2.2 1 или более поздней версии. См. в разделе [проблема GitHub](https://github.com/aspnet/Identity/issues/1813#issuecomment-394543909) для обхода.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="the-starter-and-completed-app"></a>Начальный и завершенное приложение

[Скачайте](xref:index#how-to-download-a-sample) [завершения](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) приложения. [Тест](#test-the-completed-app) готовое приложение, поэтому вам ознакомиться с его возможностями безопасности.

### <a name="the-starter-app"></a>Приложение начального уровня

[Скачайте](xref:index#how-to-download-a-sample) [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) приложения.

Запустите приложение, коснитесь **ContactManager** связать и убедитесь, создание, изменение и удаление контакта.

## <a name="secure-user-data"></a>Обеспечение безопасности данных пользователя

В следующих разделах есть все основные шаги по созданию приложения данные безопасности пользователей. Могут оказаться полезными для ссылки на готового проекта.

### <a name="tie-the-contact-data-to-the-user"></a>Привязать контактные данные для пользователя

Использование ASP.NET [удостоверений](xref:security/authentication/identity) идентификатор пользователя, чтобы предоставить пользователям можно изменить свои данные, но не данные других пользователей. Добавить `OwnerID` и `ContactStatus` для `Contact` модели:

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

`OwnerID` — Это идентификатор пользователя из `AspNetUser` в таблицу [удостоверений](xref:security/authentication/identity) базы данных. `Status` Поля определяет, является ли контакт для просмотра обычными пользователями.

Создание новой миграции и обновления базы данных:

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a>Добавление служб роли удостоверению

Добавление [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) для добавления служб ролей:

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a>Требовать от пользователей, прошедших проверку подлинности

Задайте политику проверки подлинности по умолчанию, чтобы требовать проверку подлинности пользователей:

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 Можно отказаться от проверки подлинности на уровне метода действия, контроллера или страницу Razor с `[AllowAnonymous]` атрибута. Задание политики проверки подлинности по умолчанию требовать от пользователей пройти проверку подлинности защищает вновь добавленный Razor Pages и контроллеры. Наличие требуется по умолчанию проверка подлинности безопаснее, чем полагаться на новые контроллеры и страницы Razor для включения `[Authorize]` атрибута.

Добавить [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) в индекс, со сведениями и контактными страницы, чтобы анонимные пользователи могут получить сведения о веб-узле, прежде чем они были зарегистрированы.

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a>Настройка тестовой учетной записью

`SeedData` Класс создает две учетные записи: администратора и диспетчера. Используйте [средство Secret Manager](xref:security/app-secrets) Установка пароля для этих учетных записей. Задайте пароль из каталога проекта (каталог, содержащий *Program.cs*):

```console
dotnet user-secrets set SeedUserPW <PW>
```

Если надежный пароль не указан, создается исключение при `SeedData.Initialize` вызывается.

Обновление `Main` используйте пароль теста:

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a>Создание тестовых учетных записей и обновление контактов

Обновление `Initialize` метод в `SeedData` класса для создания тестовых учетных записей:

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

Добавьте идентификатор пользователя администратора и `ContactStatus` контактам. Создать контактных лиц с «Отправлено» и одного «отклонено». Добавьте идентификатор пользователя и состояние всех контактов. Отображается только один контакт:

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>Создание владельца, manager и обработчики авторизации администратора

Создание `ContactIsOwnerAuthorizationHandler` в класс *авторизации* папки. `ContactIsOwnerAuthorizationHandler` Проверяет, что пользователь, выполняющий ресурса, которому принадлежит ресурс.

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

`ContactIsOwnerAuthorizationHandler` Вызовы [контекста. Успешно](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) при связаться с владельцем текущего авторизованного пользователя. Обработчики авторизации обычно:

* Вернуть `context.Succeed` при соблюдении требований.
* Вернуть `Task.CompletedTask` Если требования не соблюдены. `Task.CompletedTask` — ни об успехе или неудаче&mdash;позволяет другим обработчикам авторизации для выполнения.

Если необходимо выполнить явную ошибку, возвратить [контекста. Сбой](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).

Приложение позволяет контактные владельцам изменения или удаления и создания собственных данных. `ContactIsOwnerAuthorizationHandler` не требуется для проверки операции, передаваемые в качестве параметра требование.

### <a name="create-a-manager-authorization-handler"></a>Создайте обработчик диспетчера авторизации

Создание `ContactManagerAuthorizationHandler` в класс *авторизации* папки. `ContactManagerAuthorizationHandler` Проверяет пользователя, действующий от ресурса является руководителем. Только менеджеры могли утверждать или отклонять изменения содержимого (новые или измененные).

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>Создайте обработчик авторизации администратора

Создание `ContactAdministratorsAuthorizationHandler` в класс *авторизации* папки. `ContactAdministratorsAuthorizationHandler` Проверяет действующий от ресурса пользователь является администратором. Администратор может выполнить все операции.

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>Регистрировать обработчики авторизации

Службы, с помощью Entity Framework Core должны быть зарегистрированы для [внедрения зависимостей](xref:fundamentals/dependency-injection) с помощью [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions). `ContactIsOwnerAuthorizationHandler` Использует ASP.NET Core [удостоверений](xref:security/authentication/identity), которая опирается на Entity Framework Core. Зарегистрировать обработчики с коллекцией, службы, поэтому они будут доступны для `ContactsController` через [внедрения зависимостей](xref:fundamentals/dependency-injection). Добавьте следующий код в конец `ConfigureServices`:

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

`ContactAdministratorsAuthorizationHandler` и `ContactManagerAuthorizationHandler` добавляются как одноэлементных экземпляров. Они одноэлементных экземпляров, так как они не используют EF и все сведения, необходимые в `Context` параметр `HandleRequirementAsync` метод.

## <a name="support-authorization"></a>Поддержка авторизации

В этом разделе вы обновите страницы Razor Pages и добавьте класс требования к операции.

### <a name="review-the-contact-operations-requirements-class"></a>Просмотрите класс требования контактные операций

Просмотрите `ContactOperations` класса. Этот класс содержит требования к поддерживает приложения:

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a>Создать базовый класс для страниц Razor контактов

Создайте базовый класс, который содержит службы, используемые в Razor Pages контакты. Базовый класс помещает код инициализации в одном месте:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

Предыдущий код:

* Добавляет `IAuthorizationService` службы для доступа к обработчикам авторизации.
* Добавляет удостоверение `UserManager` службы.
* Добавьте `ApplicationDbContext`.

### <a name="update-the-createmodel"></a>Обновление CreateModel

Измените конструктор модель страницы create для использования `DI_BasePageModel` базового класса:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

Обновление `CreateModel.OnPostAsync` метод:

* Добавьте идентификатор пользователя, `Contact` модели.
* Вызовите обработчик авторизации, чтобы убедиться, что пользователь имеет разрешение на создание контактов.

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a>Обновление IndexModel

Обновление `OnGetAsync` метод, поэтому отображаются только утвержденные контактов для обычных пользователей:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a>Обновление EditModel

Добавление обработчика авторизации, чтобы убедиться, что пользователь является владельцем контакта. Так как авторизации ресурсов проверяется, `[Authorize]` атрибут будет недостаточно. Приложение не имеет доступа к ресурсу, при оценке атрибуты. Авторизация на основе ресурсов должен быть явный. Проверки должны выполняться, когда приложение имеет доступ к ресурсу, загрузив его в модели страниц или, загрузив его в сам обработчик. Вы обращаетесь чаще всего ресурса, передавая ключ ресурса.

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a>Обновление DeleteModel

Обновите модель страницы delete на использование обработчика авторизации, чтобы убедиться, что пользователь имеет разрешение на удаление для контакта.

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a>Внедрить службы авторизации в представления

В настоящее время в пользовательском Интерфейсе показано изменение и удаление ссылок для контактов, которые не могут изменяться.

Внедрить службы авторизации в *Views/_ViewImports.cshtml* файл, чтобы он был доступен для всех представлений:

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

Предыдущая разметка добавляет некоторые `using` инструкций.

Обновление **изменить** и **удалить** ссылок в *Pages/Contacts/Index.cshtml* , обработкой только для пользователей с соответствующими разрешениями:

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> Скрытие ссылок от пользователей, у которых нет разрешения на изменение данных не защитить приложения. Скрытие ссылок делает приложение более удобным, отображая только допустимые ссылки. Пользователи могут взломать созданный URL-адреса для вызова редактирования и удаления данных, которыми они не владеют. На странице Razor или контроллер должен Принудительная проверка доступа для защиты данных.

### <a name="update-details"></a>Дополнительные сведения об обновлении

Обновление представления сведений, чтобы менеджеры могли утверждать или отклонять контактов:

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

Обновите модель страницы сведений:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a>Добавление или удаление пользователя к роли

См. в разделе [эту проблему](https://github.com/aspnet/Docs/issues/8502) сведения о:

* Удаление привилегий пользователя. Например, отключение звука пользователя в приложение чата.
* Добавление прав к пользователю.

## <a name="test-the-completed-app"></a>Тестирование завершенного приложения

Если приложение имеет контактов:

* Удалить все записи из `Contact` таблицы.
* Перезапустите приложение, чтобы заполнить базу данных.

Регистрация пользователя для просмотра контактов.

Простой способ протестировать готовое приложение — запуск три различных браузеров (или инкогнито или InPrivate версий). В один браузер, регистрация нового пользователя (например, `test@example.com`). Войдите в каждом браузере с другим пользователем. Проверьте следующие операции:

* Зарегистрированные пользователи могут просматривать утвержденные контактных данных.
* Зарегистрированным пользователям может изменять и удалять свои собственные данные.
* Менеджеры могли утверждать или отклонять контактные данные. `Details` Просмотра показано **утвердить** и **Отклонить** кнопки.
* Администраторы могут утверждать или отклонять и изменить или удалить все данные.

| Пользовательская| Параметры |
| ------------ | ---------|
| test@example.com | Может изменять и удалять данные принадлежат |
| manager@contoso.com | Можно утверждать или отклонять и изменить или удалить свои данные |
| admin@contoso.com | Можно изменить или удалить и утверждать или отклонять все данные|

Создание контакта в браузере администратора. Скопируйте URL-адрес для удаления и редактирование откуда обратитесь к администратору. Вставьте эти ссылки в браузере пользователя теста, чтобы убедиться, что тестовый пользователь не может выполнять эти операции.

## <a name="create-the-starter-app"></a>Создать приложение начального уровня

* Создание приложения Razor Pages с именем «ContactManager»
   * Создание приложения с **отдельным учетным записям пользователей**.
   * Назовите его «ContactManager», пространство имен соответствует пространству имен, используемые в образце.
   * `-uld` Указывает LocalDB вместо SQLite

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* Добавить *Models\Contact.cs*:

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* Каркас `Contact` модели.
* Создание первоначальной миграции и обновления базы данных:

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
```

* Обновление **ContactManager** привязки в *Pages/_Layout.cshtml* файла:

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* Протестируйте приложение, создание, изменение и удаление контакта

### <a name="seed-the-database"></a>Заполнение базы данных

Добавить [SeedData](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) класс *данных* папки.

Вызовите `SeedData.Initialize` из `Main`:

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

Проверьте, что приложение заполняется данными базы данных. При возникновении любых строк в контакте DB, не запускается метод заполнения.

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a>Дополнительные ресурсы

* [Создание веб-приложения .NET Core с базой данных SQL в службе приложений Azure](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* [ASP.NET Core авторизация лаборатории](https://github.com/blowdart/AspNetAuthorizationWorkshop). Это лабораторное занятие содержит более подробные сведения о функциях безопасности, представленных в этом руководстве.
* [Авторизация в ASP.NET Core: простая, ролей, на основе утверждений и пользовательская](xref:security/authorization/index)
* [Пользовательская авторизация на основе политик](xref:security/authorization/policies)

::: moniker-end
