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
ms.openlocfilehash: 0b67d4aef198aa418b54fb92db76d331ffa2785a
ms.sourcegitcommit: 0d6f151e69c159d776ed0142773279e645edbc0a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2018
ms.locfileid: "35415037"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a>Создание приложения ASP.NET Core пользовательскими данными, защищенных авторизации

Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Джо Одетт](https://twitter.com/joeaudette) (Joe Audette)

Этого учебника показано, как создать веб-приложение ASP.NET Core пользовательскими данными, защищенных авторизации. Отображается список контактов, прошедшие проверку подлинности (зарегистрированные) пользователи создали. Существует три группы безопасности:

* **Зарегистрированные пользователи** можно просмотреть все утвержденные данные и можно изменить или удалить свои собственные данные.
* **Диспетчеры** можно утвердить или отклонить контактных данных. Только утвержденные контакты отображаются для пользователей.
* **Администраторы** можно утвердить или отклонить и изменить или удалить все данные.

На следующем рисунке пользователь Рик (`rick@example.com`) вход в систему. Рик могут только просматривать утвержденные контакты и **изменить**/**удаление**/**создать новый** ссылки для его контактов. Последней записи, созданные Рик отображает **изменить** и **удалить** ссылки. Пользователи не смогут просмотреть последнюю запись пока руководитель или администратор меняет состояние на «Утверждено».

![изображение описывается предшествующий](secure-data/_static/rick.png)

На следующем рисунке `manager@contoso.com` входит в и диспетчеры роль:

![изображение описывается предшествующий](secure-data/_static/manager1.png)

На следующем рисунке показана менеджеров Просмотр подробностей контакта:

![изображение описывается предшествующий](secure-data/_static/manager.png)

**Утвердить** и **Отклонить** кнопки отображаются только для менеджеров и администраторов.

На следующем рисунке `admin@contoso.com` входит в и в роли "Администраторы":

![изображение описывается предшествующий](secure-data/_static/admin.png)

Администратор имеет все права доступа. Она может чтения, изменить или удалить любой контакт и изменить состояние контактов.

Приложение создано с [формирование шаблонов](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) следующие `Contact` модели:

[!code-csharp[](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

Пример содержит следующие обработчики авторизации:

* `ContactIsOwnerAuthorizationHandler`: Гарантирует, что пользователь может редактировать только свои данные.
* `ContactManagerAuthorizationHandler`: Менеджеры утверждать или отклонять контакты.
* `ContactAdministratorsAuthorizationHandler`-Позволяет администраторам утвердить или отклонить контактов и изменение/удаление контактов.

## <a name="prerequisites"></a>Предварительные требования

Этот учебник является дополнительным. Вы должны быть знакомы с:

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [Проверка подлинности](xref:security/authentication/index)
* [Подтверждение учетной записи и восстановление пароля](xref:security/authentication/accconfirm)
* [Авторизация](xref:security/authorization/index)
* [Entity Framework Core](xref:data/ef-mvc/intro)

В разделе [этот файл PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) для версии ASP.NET Core MVC. Версия ASP.NET Core 1.1 этого учебника, [это](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) папки. 1.1, ASP.NET Core образец находится в [образцы](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).

## <a name="the-starter-and-completed-app"></a>Начальная и завершенное приложение

[Загрузить](xref:tutorials/index#how-to-download-a-sample) [завершения](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) приложения. [Тест](#test-the-completed-app) завершенное приложение, чтобы ознакомиться с функциями безопасности.

### <a name="the-starter-app"></a>Начального уровня приложения

[Загрузить](xref:tutorials/index#how-to-download-a-sample) [начальный](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) приложения.

Запуск приложения, коснитесь **ContactManager** ссылку и убедитесь, создание, изменение и удаление контакта.

## <a name="secure-user-data"></a>Защитить данные пользователя

Следующие разделы имеют основных шагов для создания приложения данных безопасности пользователей. Может быть полезен для обращения к завершенного проекта.

### <a name="tie-the-contact-data-to-the-user"></a>Связать контактные данные для пользователя

Используйте ASP.NET [удостоверение](xref:security/authentication/identity) идентификатор пользователя, чтобы предоставить пользователям можно изменить свои данные, но не данные других пользователей. Добавить `OwnerID` и `ContactStatus` для `Contact` модели:

[!code-csharp[](secure-data/samples/final2/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

`OwnerID` идентификатор пользователя из `AspNetUser` в таблицу [удостоверение](xref:security/authentication/identity) базы данных. `Status` Поле определяет, является ли контакт для просмотра обычными пользователями.

Создание нового миграции и обновления базы данных:

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="require-https-and-authenticated-users"></a>Требовать HTTPS и проверку подлинности пользователей

Добавить [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) для `Startup`:

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_env)]

В `ConfigureServices` метод *файла Startup.cs* файл, добавьте [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) фильтр авторизации:

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_SSL&highlight=10-999)]

Если вы используете Visual Studio, необходимо включите HTTPS.

Для перенаправления HTTP-запросы HTTPS, в разделе [по промежуточного слоя перезаписи URL-адрес](xref:fundamentals/url-rewriting). Если с помощью кода Visual Studio или тестирование на локальном платформу, которая не включает тестовый сертификат для использования протокола HTTPS.

  Задать `"LocalTest:skipHTTPS": true` в *appsettings. Developement.JSON* файл.

### <a name="require-authenticated-users"></a>Требовать проверку подлинности пользователей

Задайте политику проверки подлинности по умолчанию, чтобы требовать проверку подлинности пользователей. Можно отключить проверку подлинности на уровне метода действия, контроллера или страница Razor с `[AllowAnonymous]` атрибута. Задание политики проверки подлинности по умолчанию, чтобы требовать проверку подлинности пользователей защищает вновь добавленных страниц Razor и контроллеров. Наличие более безопасна, чем новых контроллеров и страниц Razor для включения проверки подлинности, предусмотренного по умолчанию `[Authorize]` атрибута. 

С требованием проверки подлинности все пользователи [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) и [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage?view=aspnetcore-2.0) вызовы не являются обязательными.

Обновление `ConfigureServices` со следующими изменениями:

* Закомментируйте `AuthorizeFolder` и `AuthorizePage`.
* Задайте политику проверки подлинности по умолчанию, чтобы требовать проверку подлинности пользователей.

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_defaultPolicy&highlight=23-27,31-999)]

Добавить [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) индекс страницы об и контактов, анонимные пользователи могут получить сведения о веб-сайте, прежде чем они зарегистрировать. 

[!code-csharp[](secure-data/samples/final2/Pages/Index.cshtml.cs?name=snippet&highlight=2)]

Добавить `[AllowAnonymous]` для [LoginModel и RegisterModel](https://github.com/aspnet/templating/issues/238).

### <a name="configure-the-test-account"></a>Настройка тестовой учетной записи

`SeedData` Класс создает две учетные записи: администратора и диспетчера. Используйте [секрет диспетчера](xref:security/app-secrets) , чтобы задать пароль для этих учетных записей. Задать пароль из каталога проекта (каталог, содержащий *Program.cs*):

```console
dotnet user-secrets set SeedUserPW <PW>
```

Обновление `Main` использовать пароль теста:

[!code-csharp[](secure-data/samples/final2/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a>Создание тестовых учетных записей и обновить контакты

Обновление `Initialize` метод `SeedData` класса, чтобы создать тестовые учетные записи:

[!code-csharp[](secure-data/samples/final2/Data/SeedData.cs?name=snippet_Initialize)]

Добавьте идентификатор пользователя администратора и `ContactStatus` контактам. Создать контактов «Отправлено» и одного «отклонено». Добавьте идентификатор пользователя и состояние для всех контактов. Показан только один контакт.

[!code-csharp[](secure-data/samples/final2/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>Создать владельца, диспетчер и обработчики авторизации администратора

Создание `ContactIsOwnerAuthorizationHandler` класса в *авторизации* папки. `ContactIsOwnerAuthorizationHandler` Проверяет, что пользователь, действующий от ресурса, которому принадлежит ресурс.

[!code-csharp[](secure-data/samples/final2/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

`ContactIsOwnerAuthorizationHandler` Вызовы [контекста. Успешно](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) Если контактные владельцем текущего пользователя, прошедшего проверку подлинности. Обработчики авторизации обычно:

* Возвращает `context.Succeed` при выполнение требований.
* Возвращает `Task.CompletedTask` Если требования не выполнены. `Task.CompletedTask` — ни об успешном или неуспешном&mdash;она позволяет другим обработчикам авторизации для выполнения.

Если необходимо выполнить явную ошибку, возвратить [контекста. Сбой](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).

Данное приложение позволяет контакта владельцев, изменение или удаление и создание собственных данных. `ContactIsOwnerAuthorizationHandler` не требуется для операции, передаваемые в качестве параметра требование проверки.

### <a name="create-a-manager-authorization-handler"></a>Создание обработчика диспетчера авторизации

Создание `ContactManagerAuthorizationHandler` класса в *авторизации* папки. `ContactManagerAuthorizationHandler` Проверяет пользователя, действующий от ресурса является менеджером. Данные только менеджеров можно утвердить или отклонить изменения содержимого (новые или измененные).

[!code-csharp[](secure-data/samples/final2/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>Создайте обработчик авторизации администратора

Создание `ContactAdministratorsAuthorizationHandler` класса в *авторизации* папки. `ContactAdministratorsAuthorizationHandler` Проверяет, действует для ресурса пользователь является администратором. Администратор может выполнять все операции.

[!code-csharp[](secure-data/samples/final2/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>Регистрировать обработчики авторизации

С помощью Entity Framework Core Services должны быть зарегистрированы для [внедрения зависимостей](xref:fundamentals/dependency-injection) с помощью [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions). `ContactIsOwnerAuthorizationHandler` Использует ASP.NET Core [удостоверения](xref:security/authentication/identity), которые построены на Entity Framework Core. Зарегистрировать обработчики с коллекцией служб, они будут доступны для `ContactsController` через [внедрения зависимостей](xref:fundamentals/dependency-injection). Добавьте следующий код в конец `ConfigureServices`:

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=ConfigureServices&highlight=41-999)]

`ContactAdministratorsAuthorizationHandler` и `ContactManagerAuthorizationHandler` добавляются как одноэлементных кортежей. Они единственных экземпляров, так как они не используют EF и все сведения, необходимые возможности `Context` параметр `HandleRequirementAsync` метода.

## <a name="support-authorization"></a>Поддержка авторизации

В этом разделе обновления страниц Razor и добавления класса требований операции.

### <a name="review-the-contact-operations-requirements-class"></a>Просмотрите требования класс контакта operations

Просмотрите `ContactOperations` класса. Этот класс содержит требования поддерживает приложения:

[!code-csharp[](secure-data/samples/final2/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-razor-pages"></a>Создать базовый класс для страниц Razor

Создайте базовый класс, который содержит службы, используемые в контактах страниц Razor. Базовый класс используется этот код инициализации в одном месте:

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/DI_BasePageModel.cs)]

Предыдущий код:

* Добавляет `IAuthorizationService` для доступа к обработчикам авторизации.
* Добавляет идентификатор `UserManager` службы.
* Добавьте `ApplicationDbContext`.

### <a name="update-the-createmodel"></a>Обновление CreateModel

Измените конструктор модели создать страницу для использования `DI_BasePageModel` базового класса:

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

Обновление `CreateModel.OnPostAsync` метода:

* Добавьте идентификатор пользователя, который `Contact` модели.
* Вызывает обработчик авторизации, чтобы убедиться, что пользователь имеет разрешение на создание контактов.

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a>Обновление IndexModel

Обновление `OnGetAsync` метод только утвержденные контакты отображаются обычным пользователям:

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a>Обновление EditModel

Добавьте обработчик авторизации, чтобы убедиться, что пользователь является владельцем контакта. Поскольку проверки авторизации ресурсов `[Authorize]` атрибута недостаточно. Приложение не имеет доступа к ресурсу, при оценке атрибутов. Авторизация на основе ресурсов должно быть принудительной. После приложение имеет доступ к ресурсу, загрузив его в модель страницы или, загрузив его в сам обработчик должен выполняться проверки. Частом обращении к ресурсу, передавая ключ ресурса.

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a>Обновление DeleteModel

Обновление модели страницы delete на использование обработчика авторизации, чтобы убедиться, что пользователь имеет разрешение на удаление контакта.

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a>Внедрить службы авторизации в представления

В настоящее время показано пользовательского интерфейса, изменение и удаление ссылки на данные, которые не может быть изменено пользователем. Пользовательский Интерфейс фиксируется путем применения авторизации обработчика представлений.

Запустить службу авторизации в *Views/_ViewImports.cshtml* файл, чтобы сделать ее доступной для всех представлений:

[!code-cshtml[](secure-data/samples/final2/Pages/_ViewImports.cshtml?highlight=6-9)]

Добавляет предыдущей разметки некоторые `using` инструкции.

Обновление **изменить** и **удалить** связывает *Pages/Contacts/Index.cshtml* , только отображаются для пользователей с соответствующими разрешениями:

[!code-cshtml[](secure-data/samples/final2/Pages/Contacts/Index.cshtml?highlight=34-36,64-999)]

> [!WARNING]
> Скрытие ссылок от пользователей, у которых нет разрешения на изменение данных не защищать приложения. Скрытие ссылки позволяет сделать приложение более удобной для пользователей, отображая только допустимые ссылки. Пользователи могут hack созданные URL-адреса, вызывать изменение и удаление операций с данными, которыми они не владеют. Страница Razor или контроллер должен Принудительная проверка доступа для защиты данных.

### <a name="update-details"></a>Дополнительные сведения об обновлении

Обновление представления сведений, чтобы менеджеры могли утверждать или отклонять контакты:

[!code-cshtml[](secure-data/samples/final2/Pages/Contacts/Details.cshtml?range=48-999)]

Обновите модель страницы сведений:

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="test-the-completed-app"></a>Тестирование завершенного приложения

Если с помощью кода Visual Studio или тестирование на локальном платформу, которая не включает тестовый сертификат для использования протокола HTTPS.

* Задать `"LocalTest:skipHTTPS": true` в *appsettings. Developement.JSON* файл, чтобы пропустить обязательное использование протокола HTTPS. Пропустить HTTPS только на компьютере разработки.

Если приложение имеет контактов:

* Удалить все записи из `Contact` таблицы.
* Перезапустите приложение для инициализации базы данных.

Для просмотра контактов регистрации пользователя.

Простой способ тестирования завершенное приложение — запуск три разных браузерах (или версий браузера или InPrivate). В одном браузере регистрации нового пользователя (например, `test@example.com`). Вход для каждого веб-обозревателя с другим пользователем. Проверьте следующие операции:

* Зарегистрированные пользователи могут просматривать утвержденные контактных данных.
* Зарегистрированные пользователи могут изменить или удалить свои собственные данные.
* Менеджеры могли утверждать или отклонять контактные данные. `Details` Представлении отображаются **утвердить** и **Отклонить** кнопки.
* Администраторы могут утвердить или отклонить и изменить или удалить все данные.

| Пользовательская| Параметры |
| ------------ | ---------|
| test@example.com | Можно изменить или удалить собственных данных |
| manager@contoso.com | Можно утвердить или отклонить и изменить или удалить принадлежат данные |
| admin@contoso.com | Можно изменить или удалить и утвердить или отклонить все данные|

Создание контакта в браузере администратора. Скопируйте URL-адрес для удаления и изменения из обратитесь к администратору. Вставьте эти ссылки в браузер тестового пользователя, чтобы убедиться, что тестовый пользователь не может выполнять эти операции.

## <a name="create-the-starter-app"></a>Создание начального уровня приложения

* Создание страниц Razor приложения с именем «ContactManager»

  * Создание приложения с **отдельных учетных записей пользователей**.
  * Присвойте ему имя «ContactManager», пространства имен соответствует пространству имен, используемые в образце.

::: moniker range=">= aspnetcore-2.1"

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

  [!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

  ```console
  dotnet new razor -o ContactManager -au Individual -uld
  ```

::: moniker-end

  * `-uld` Указывает LocalDB вместо SQLite

* Добавьте следующие `Contact` модели:

  [!code-csharp[](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

* Представление формирования `Contact` модели:

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
```

* Обновление **ContactManager** привязки в *Pages/_Layout.cshtml* файла:

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* Формировать первоначальной миграции и обновления базы данных:

```console
dotnet ef migrations add initial
dotnet ef database update
```

* Протестируйте приложение, создание, изменение и удаление контакта

### <a name="seed-the-database"></a>Заполнение базы данных

Добавить `SeedData` класса *данные* папки. Если загруженный образца можно скопировать *SeedData.cs* файл *данные* папки начальный проект.

Вызовите `SeedData.Initialize` из `Main`:

[!code-csharp[](secure-data/samples/starter2/Program.cs?name=snippet)]

Тест заполняется, приложение базы данных. Если все строки в контакте DB, метод инициализации не запускается.

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a>Дополнительные ресурсы

* [Лаборатории авторизации ASP.NET Core](https://github.com/blowdart/AspNetAuthorizationWorkshop). Эта лаборатория приведены более подробные сведения о средствах безопасности, представленные в этом учебнике.
* [Авторизация в ASP.NET Core: простой, роли, на основе утверждений, а также пользовательские](xref:security/authorization/index)
* [Пользовательская авторизация на основе политик](xref:security/authorization/policies)
