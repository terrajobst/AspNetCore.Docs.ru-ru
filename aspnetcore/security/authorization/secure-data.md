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
ms.openlocfilehash: 54a737f140a8434035e5fc5abfefa458fdb69321
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/22/2017
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a>Создание приложения ASP.NET Core пользовательскими данными, защищенных авторизации

Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Джо Одетт](https://twitter.com/joeaudette) (Joe Audette)

Этого учебника показано, как создавать веб-приложения с данными пользователя, защищен авторизации. Отображается список контактов, прошедшие проверку подлинности (зарегистрированные) пользователи создали. Существует три группы безопасности:

* Зарегистрированные пользователи могут просматривать утвержденные контактных данных.
* Зарегистрированные пользователи могут изменить или удалить свои собственные данные. 
* Менеджеры могли утверждать или отклонять контактные данные. Только утвержденные контакты отображаются для пользователей.
* Администраторы могут утвердить или отклонить и изменить или удалить все данные.

На следующем рисунке пользователь Рик (`rick@example.com`) вход в систему. Пользователь Рик может только представление утвержденные контакты и изменить или удалить его контактов. Последней записи, созданные Рик, отображает изменение и удаление связей

![изображения, описанных выше](secure-data/_static/rick.png)

На следующем рисунке `manager@contoso.com` входит в и диспетчеры роль. 

![изображения, описанных выше](secure-data/_static/manager1.png)

На следующем рисунке менеджеров представление сведений для контакта.

![изображения, описанных выше](secure-data/_static/manager.png)

Только администраторы и диспетчеры имеют утверждение и отклонение кнопок.

На следующем рисунке `admin@contoso.com` входит в и в роли администратора. 

![изображения, описанных выше](secure-data/_static/admin.png)

Администратор имеет все права доступа. Она может чтения, изменить или удалить любой контакт и изменить состояние контактов.

Приложение создано с [формирование шаблонов](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) следующие `Contact` модели:

[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]

Объект `ContactIsOwnerAuthorizationHandler` обработки авторизации гарантирует, что пользователь может редактировать только свои данные. Объект `ContactManagerAuthorizationHandler` обработки авторизации менеджеры могут утверждать или отклонять контакты.  Объект `ContactAdministratorsAuthorizationHandler` обработки авторизации позволяет администраторам утверждать или отклонять контакты и изменение/удаление контактов. 

## <a name="prerequisites"></a>Предварительные требования

Это не начало учебника. Вы должны быть знакомы с:

* [Основные ASP.NET MVC](xref:tutorials/first-mvc-app/start-mvc)
* [Entity Framework Core](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a>Начальная и завершенное приложение

[Загрузить](xref:tutorials/index#how-to-download-a-sample) [завершения](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final) приложения. [Тест](#test-the-completed-app) завершенное приложение, чтобы ознакомиться с функциями безопасности. 

### <a name="the-starter-app"></a>Начального уровня приложения

Полезно для сравнения кода с помощью полного примера.

[Загрузить](xref:tutorials/index#how-to-download-a-sample) [начальный](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter) приложения. 

В разделе [создания начального приложения](#create-the-starter-app) Если вы хотите создать ее с нуля.

Обновление базы данных:

```none
   dotnet ef database update
```

Запуск приложения, коснитесь **ContactManager** ссылку и убедитесь, создание, изменение и удаление контакта.

Этот учебник содержит основных шагов для создания приложения данных безопасности пользователей. Может быть полезен для обращения к завершенного проекта.

## <a name="modify-the-app-to-secure-user-data"></a>Изменение приложения для защиты данных пользователя

Следующие разделы имеют основных шагов для создания приложения данных безопасности пользователей. Может быть полезен для обращения к завершенного проекта.

### <a name="tie-the-contact-data-to-the-user"></a>Связать контактные данные для пользователя

Используйте ASP.NET [удостоверение](xref:security/authentication/identity) идентификатор пользователя, чтобы предоставить пользователям можно изменить свои данные, но не данные других пользователей. Добавить `OwnerID` для `Contact` модели:

[!code-csharp[Main](secure-data/samples/final/Models/Contact.cs?name=snippet1&highlight=5-6,16-)]

`OwnerID`идентификатор пользователя из `AspNetUser` в таблицу [удостоверение](xref:security/authentication/identity) базы данных. `Status` Поле определяет, является ли контакт для просмотра обычными пользователями. 

Сформировать новый миграции и обновления базы данных:

```console
dotnet ef migrations add userID_Status
dotnet ef database update
 ```

### <a name="require-ssl-and-authenticated-users"></a>Требовать SSL и проверку подлинности пользователей

В `ConfigureServices` метод *файла Startup.cs* файл, добавьте [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) фильтр авторизации:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_SSL&highlight=1)]

Если вы используете Visual Studio, см. раздел [настроить IIS Express для SSL или HTTPS](xref:security/enforcing-ssl#set-up-iis-express-for-sslhttps). Для перенаправления HTTP-запросы HTTPS, в разделе [по промежуточного слоя перезаписи URL-адрес](xref:fundamentals/url-rewriting). Если вы используете Visual Studio Code или тестирование на локальной платформе, которая не включает тестового сертификата для SSL:

- Задать `"LocalTest:skipSSL": true` в *appsettings.json* файла.

### <a name="require-authenticated-users"></a>Требовать проверку подлинности пользователей

Задайте политику проверки подлинности по умолчанию, чтобы требовать проверку подлинности пользователей. Можно отключить проверку подлинности на контроллеру или методу действия с `[AllowAnonymous]` атрибута. Таким образом, любые новые контроллеры добавлен автоматически требуется проверка подлинности, которая является более безопасным, чем полагаться на новые контроллеры для включения `[Authorize]` атрибута. Добавьте следующий код в `ConfigureServices` метод *файла Startup.cs* файла:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_defaultPolicy)]

Добавить `[AllowAnonymous]` , анонимные пользователи могут получить сведения о веб-сайте, прежде чем они зарегистрировать контроллер home.

[!code-csharp[Main](secure-data/samples/final/Controllers/HomeController.cs?name=snippet1&highlight=2,6)]

### <a name="configure-the-test-account"></a>Настройка тестовой учетной записи

`SeedData` Класс создает две учетные записи, администратора и диспетчера. Используйте [секрет диспетчера](xref:security/app-secrets) , чтобы задать пароль для этих учетных записей. Это можно сделать из каталога проекта (каталог, содержащий *Program.cs*).

```console
dotnet user-secrets set SeedUserPW <PW>
```

Обновление `Configure` использовать пароль теста:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=Configure&highlight=19-21)]

Добавьте идентификатор пользователя администратора и `Status = ContactStatus.Approved` контактам. Отображается только один контакт, добавьте все контакты идентификатор пользователя:

[!code-csharp[Main](secure-data/samples/final/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>Создать владельца, диспетчер и обработчики авторизации администратора

Создание `ContactIsOwnerAuthorizationHandler` класса в *авторизации* папки. `ContactIsOwnerAuthorizationHandler` Проверит пользователя, действующий от ресурса, которому принадлежит ресурс.

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

`ContactIsOwnerAuthorizationHandler` Вызовы `context.Succeed` Если контактные владельцем текущего пользователя, прошедшего проверку подлинности. Обработчики авторизации обычно возвращают `context.Succeed` при выполнение требований. Они возвращают `Task.FromResult(0)` Если требования не выполнены. `Task.FromResult(0)`— ни успех или неудача, она позволяет другому обработчику авторизации для выполнения. Если необходимо выполнить явную ошибку, возвратить `context.Fail()`.

Мы даем контакта владельцев, изменить или удалить свои собственные данные, поэтому мы не нужно проверять операции, передаваемые в качестве параметра требование.

### <a name="create-a-manager-authorization-handler"></a>Создание обработчика диспетчера авторизации

Создание `ContactManagerAuthorizationHandler` класса в *авторизации* папки. `ContactManagerAuthorizationHandler` Проверит пользователя, действующий от ресурса является менеджером. Данные только менеджеров можно утвердить или отклонить изменения содержимого (новые или измененные).

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>Создайте обработчик авторизации администратора

Создание `ContactAdministratorsAuthorizationHandler` класса в *авторизации* папки. `ContactAdministratorsAuthorizationHandler` Проверит действует для ресурса пользователь является администратором. Администратор может выполнять все операции.

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>Регистрировать обработчики авторизации

С помощью Entity Framework Core Services должны быть зарегистрированы для [внедрения зависимостей](xref:fundamentals/dependency-injection) с помощью [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions). `ContactIsOwnerAuthorizationHandler` Использует ASP.NET Core [удостоверения](xref:security/authentication/identity), которые построены на Entity Framework Core. Зарегистрировать обработчики с коллекцией служб, они будут доступны для `ContactsController` через [внедрения зависимостей](xref:fundamentals/dependency-injection). Добавьте следующий код в конец `ConfigureServices`:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=AuthorizationHandlers)]

`ContactAdministratorsAuthorizationHandler`и `ContactManagerAuthorizationHandler` добавляются как одноэлементных кортежей. Они являются единственных экземпляров, так как они не используют EF и все сведения, необходимые в `Context` параметр `HandleRequirementAsync` метода.

Полный `ConfigureServices`:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=ConfigureServices)]

## <a name="update-the-code-to-support-authorization"></a>Обновление кода для поддержки авторизации

В этом разделе обновления контроллера и представлений и добавить класс требований операции.

### <a name="update-the-contacts-controller"></a>Обновление контроллера контактов

Обновление `ContactsController` конструктор:

* Добавить `IAuthorizationService` службы для доступа к обработчикам авторизации. 
* Добавить `Identity` `UserManager` службы:

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_ContactsControllerCtor)]

### <a name="add-a-contact-operations-requirements-class"></a>Добавьте класс требования операции контактов

Добавить `ContactOperationsRequirements` класса *авторизации* папки. Этот класс содержит требованиям наших поддерживает приложения:

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]

### <a name="update-create"></a>Создание обновления

Обновление `HTTP POST Create` метода:

* Добавьте идентификатор пользователя, который `Contact` модели.
* Вызывает обработчик авторизации, чтобы убедиться, что пользователь является владельцем контакта.

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Create)]

### <a name="update-edit"></a>Изменение обновления

Обновлять `Edit` методов на использование обработчика авторизации, чтобы убедиться, что пользователь является владельцем контакта. Так как мы занимаемся авторизации ресурсов не может использовать `[Authorize]` атрибута. При оценке атрибуты нас нет доступа к ресурсу. Авторизация на основе ресурсов должно быть принудительной. После получения доступа к ресурсу, загрузив его в нашем контроллера или его загрузке в сам обработчик должен выполняться проверки. Часто будет доступ к ресурсу, передавая ключ ресурса.

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Edit)]

### <a name="update-the-delete-method"></a>Метод удаления обновления

Обновлять `Delete` методов на использование обработчика авторизации, чтобы убедиться, что пользователь является владельцем контакта.

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Delete)]

## <a name="inject-the-authorization-service-into-the-views"></a>Внедрить службы авторизации в представления

В настоящее время показано пользовательского интерфейса, изменение и удаление ссылки на данные, которые не может быть изменено пользователем. Мы исправим это путем применения авторизации обработчика представлений.

Запустить службу авторизации в *Views/_ViewImports.cshtml* файл, он будет доступен для всех представлений:

[!code-html[Main](secure-data/samples/final/Views/_ViewImports.cshtml)]

Обновление *Views/Contacts/Index.cshtml* представления Razor, чтобы отображались только редактирование удалить ссылки для пользователей, которые можно изменить или удалить контакт.

Добавить`@using ContactManager.Authorization;`

Обновление `Edit` и `Delete` ссылки, они отображаются только пользователи с разрешением на изменение и удаление контакта.

[!code-html[Main](secure-data/samples/final/Views/Contacts/Index.cshtml?range=63-84)]

Предупреждение: Скрытие ссылок от пользователей, у которых нет разрешения на изменение и удаление данных не обеспечивает безопасность приложения. Скрытие ссылки делает приложение пользователя более понятное, отображая только допустимые ссылки. Пользователи могут hack созданные URL-адреса, вызывать изменение и удаление операций с данными, которыми они не владеют.  Контроллер должен повторять проверку доступа для обеспечения безопасности.

### <a name="update-the-details-view"></a>Обновить представление сведений

Обновление представления сведений, чтобы менеджеры могли утверждать или отклонять контакты:

[!code-html[Main](secure-data/samples/final/Views/Contacts/Details.cshtml?range=53-)]

## <a name="test-the-completed-app"></a>Тестирование завершенного приложения

Если вы используете Visual Studio Code или тестирование на локальной платформе, которая не включает тестового сертификата для SSL:

- Задать `"LocalTest:skipSSL": true` в *appsettings.json* файла.

Если вы запускали приложение и контактов, удалите все записи в `Contact` таблице и перезапустить приложение для инициализации базы данных. Если вы используете Visual Studio, необходимо закрыть и перезапустить IIS Express для инициализации базы данных.

Регистрация пользователя для поиска контактов.

Простой способ тестирования завершенное приложение — запуск три разных браузерах (или версий браузера или InPrivate). В одном браузере регистрации нового пользователя, например, `test@example.com`. Вход для каждого веб-обозревателя с другим пользователем. Проверьте следующее.

* Зарегистрированные пользователи могут просматривать утвержденные контактных данных.
* Зарегистрированные пользователи могут изменить или удалить свои собственные данные. 
* Менеджеры могли утверждать или отклонять контактные данные. `Details` Представлении отображаются **утвердить** и **Отклонить** кнопки. 
* Администраторы могут утвердить или отклонить и изменить или удалить все данные.

| Пользователь| Параметры |
| ------------ | ---------|
| test@example.com | Можно изменить или удалить собственных данных |
| manager@contoso.com | Можно утвердить или отклонить и изменить или удалить принадлежат данные  |
| admin@contoso.com | Можно изменить или удалить и утвердить или отклонить все данные|

Создание контакта в браузере администраторов. Скопируйте URL-адрес для удаления и изменения из обратитесь к администратору. Вставьте эти ссылки в браузер тестового пользователя, чтобы убедиться, что тестовый пользователь не может выполнять эти операции.

## <a name="create-the-starter-app"></a>Создание начального уровня приложения

Выполните следующие действия для создания начального приложения.

* Создание **веб-приложения ASP.NET Core** с помощью [2017 г. Visual Studio](https://www.visualstudio.com/) с именем «ContactManager»

  * Создание приложения с **отдельных учетных записей пользователей**.
  * Присвойте ему имя «ContactManager», пространство имен будет соответствовать использование пространства имен в образце.

* Добавьте следующие `Contact` модели:

  [!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]

* Представление формирования `Contact` модели с помощью Entity Framework Core и `ApplicationDbContext` контекста данных. Примите значения по умолчанию формирование шаблонов. С помощью `ApplicationDbContext` для контекста данных класс помещает в таблице контактов [удостоверение](xref:security/authentication/identity) базы данных. В разделе [Добавление модели](xref:tutorials/first-mvc-app/adding-model) для получения дополнительной информации.

* Обновление **ContactManager** привязки в *Views/Shared/_Layout.cshtml* файл из `asp-controller="Home"` для `asp-controller="Contacts"` таким образом коснувшись **ContactManager** ссылку вызывает контроллер контактов. Исходной разметке:

```html
   <a asp-area="" asp-controller="Home" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

Обновленная разметка:

```html
   <a asp-area="" asp-controller="Contacts" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

* Формировать первоначальной миграции и обновления базы данных

```none
   dotnet ef migrations add initial
   dotnet ef database update
   ```

* Протестируйте приложение, создание, изменение и удаление контакта

### <a name="seed-the-database"></a>Заполнение базы данных

Добавить `SeedData` класса *данные* папки. Если загруженный образца можно скопировать *SeedData.cs* файл *данные* папки начальный проект.

[!code-csharp[Main](secure-data/samples/starter/Data/SeedData.cs)]

Добавьте выделенный код в конец `Configure` метод в *файла Startup.cs* файла:

[!code-csharp[Main](secure-data/samples/starter/Startup.cs?name=Configure&highlight=28-)]

Тест заполняется, приложение базы данных. Seed-метод не выполняется, если все строки в контакт DB.

### <a name="create-a-class-used-in-the-tutorial"></a>Создайте класс, используемый в этом учебнике

* Создайте папку с именем *авторизации*.
* Копировать *Authorization\ContactOperations.cs* файла из загрузки завершенного проекта или скопируйте следующий код:

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]

<a name=secure-data-add-resources-label></a>

### <a name="additional-resources"></a>Дополнительные ресурсы

* [Лаборатории авторизации ASP.NET Core](https://github.com/blowdart/AspNetAuthorizationWorkshop). Эта лаборатория приведены более подробные сведения о средствах безопасности, представленные в этом учебнике.
* [Авторизация в ASP.NET Core: простой, роли, на основе утверждений и пользовательских](index.md)
* [Пользовательская авторизация на основе политик](policies.md)
