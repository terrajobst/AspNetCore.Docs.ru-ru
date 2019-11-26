---
title: Создание приложения ASP.NET Core с помощью данных пользователя с помощью авторизации
author: rick-anderson
description: Узнайте, как создать приложение Razor Pages с помощью данных пользователя с помощью авторизации. Включает протокол HTTPS, проверка подлинности и безопасность, удостоверения ASP.NET Core.
ms.author: riande
ms.date: 12/18/2018
ms.custom: mvc, seodec18
uid: security/authorization/secure-data
ms.openlocfilehash: 65c72d4dd457f85451796c5713bedebafec7a7de
ms.sourcegitcommit: 8157e5a351f49aeef3769f7d38b787b4386aad5f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2019
ms.locfileid: "74239838"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a>Создание приложения ASP.NET Core с помощью данных пользователя с помощью авторизации

Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Джо Одетт](https://twitter.com/joeaudette) (Joe Audette)

::: moniker range="<= aspnetcore-1.1"

См. [этот PDF-файл](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) для ASP.NET Core версии MVC. Версия ASP.NET Core 1,1 этого учебника находится в [этой](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) папке. Образец 1,1 ASP.NET Core приведен [в примерах.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2)

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Просмотреть [этот PDF-файл](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

Этом руководстве показано, как создать веб-приложение ASP.NET Core с помощью данных пользователя с помощью авторизации. Отображается список контактов, прошедшие проверку подлинности (зарегистрированного) пользователи создали. Существует три группы безопасности:

* **Зарегистрированные пользователи** могут просматривать все утвержденные данные, а также изменять и удалять свои данные.
* **Руководители** могут утверждать или отклонять контактные данные. Только утвержденные контакты отображаются для пользователей.
* **Администраторы** могут утверждать, отклонять и изменять и удалять любые данные.

Изображения в этом документе не полностью соответствуют последним шаблонам.

На следующем рисунке пользователь Рик (`rick@example.com`) вошел в. Рик может просматривать только утвержденные контакты и **изменять**/**Удалить**/**создавать новые** ссылки для своих контактов. Ссылки **Edit** и **Delete** отображаются только в последней записи, созданной Рик. Другие пользователи увидят последнюю запись, после менеджеру или администратору изменяет состояние на «Утверждено».

![Снимок экрана, показывающий Рик входа](secure-data/_static/rick.png)

На следующем рисунке `manager@contoso.com` вход в и в роли руководителя:

![Снимок экрана, показывающий manager@contoso.com выполнен вход](secure-data/_static/manager1.png)

На следующем рисунке показана менеджеров Просмотр подробностей контакта:

![Представление руководителя контакта](secure-data/_static/manager.png)

Кнопки **утвердить** и **отклонить** отображаются только для руководителей и администраторов.

На следующем рисунке `admin@contoso.com` вход выполнен и в роли администратора:

![Снимок экрана, показывающий admin@contoso.com выполнен вход](secure-data/_static/admin.png)

Администратор имеет все привилегии. Она может чтения, изменить или удалить любой контакт и изменить состояние контактов.

Приложение было создано с помощью [формирования шаблонов](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) следующей модели `Contact`:

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

Пример содержит следующие обработчики авторизации:

* `ContactIsOwnerAuthorizationHandler`: гарантирует, что пользователь сможет изменять только свои данные.
* `ContactManagerAuthorizationHandler`: позволяет руководителям утверждать или отклонять контакты.
* `ContactAdministratorsAuthorizationHandler`: позволяет администраторам утверждать или отклонять контакты, а также изменять или удалять контакты.

## <a name="prerequisites"></a>Предварительные требования

Этот учебник является дополнительным. Вы должны быть знакомы с:

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [Проверка подлинности](xref:security/authentication/identity)
* [Подтверждение учетной записи и восстановление пароля](xref:security/authentication/accconfirm)
* [Авторизация](xref:security/authorization/introduction)
* [Entity Framework Core](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a>Начальный и завершенное приложение

[Скачайте](xref:index#how-to-download-a-sample) [готовое](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) приложение. [Протестируйте](#test-the-completed-app) готовое приложение, чтобы ознакомиться с его функциями безопасности.

### <a name="the-starter-app"></a>Приложение начального уровня

[Скачайте](xref:index#how-to-download-a-sample) [Начальное](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) приложение.

Запустите приложение, коснитесь ссылки **ContactManager** и убедитесь, что вы можете создать, изменить и удалить контакт.

## <a name="secure-user-data"></a>Обеспечение безопасности данных пользователя

В следующих разделах есть все основные шаги по созданию приложения данные безопасности пользователей. Могут оказаться полезными для ссылки на готового проекта.

### <a name="tie-the-contact-data-to-the-user"></a>Привязать контактные данные для пользователя

Используйте идентификатор пользователя [удостоверения](xref:security/authentication/identity) ASP.NET, чтобы убедиться, что пользователи могут изменять данные, но не данные других пользователей. Добавьте `OwnerID` и `ContactStatus` в модель `Contact`:

[!code-csharp[](secure-data/samples/final3/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

`OwnerID` — это идентификатор пользователя из таблицы `AspNetUser` в базе данных [удостоверений](xref:security/authentication/identity) . Поле `Status` определяет, можно ли просматривать контакт обычными пользователями.

Создание новой миграции и обновления базы данных:

```dotnetcli
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a>Добавление служб роли удостоверению

Добавление [аддролес](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) для добавления служб ролей:

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet2&highlight=9)]

### <a name="require-authenticated-users"></a>Требовать от пользователей, прошедших проверку подлинности

Задайте политику проверки подлинности по умолчанию, чтобы требовать проверку подлинности пользователей:

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet&highlight=15-99)] 

 Можно отказаться от проверки подлинности на уровне страницы Razor, контроллера или метода действия с помощью атрибута `[AllowAnonymous]`. Задание политики проверки подлинности по умолчанию требовать от пользователей пройти проверку подлинности защищает вновь добавленный Razor Pages и контроллеры. Наличие проверки подлинности по умолчанию более безопасна, чем использование новых контроллеров и Razor Pages включением атрибута `[Authorize]`.

Добавьте [allowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) на страницы индекса и конфиденциальности, чтобы анонимные пользователи могли получить сведения о сайте перед их регистрацией.

[!code-csharp[](secure-data/samples/final3/Pages/Index.cshtml.cs?highlight=1,7)]

### <a name="configure-the-test-account"></a>Настройка тестовой учетной записью

Класс `SeedData` создает две учетные записи: Administrator и Manager. Используйте [средство диспетчера секретов](xref:security/app-secrets) , чтобы задать пароль для этих учетных записей. Задайте пароль из каталога проекта (каталога, содержащего *Program.CS*):

```dotnetcli
dotnet user-secrets set SeedUserPW <PW>
```

Если надежный пароль не указан, при вызове `SeedData.Initialize` возникает исключение.

Обновите `Main`, чтобы использовать тестовый пароль:

[!code-csharp[](secure-data/samples/final3/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a>Создание тестовых учетных записей и обновление контактов

Обновите метод `Initialize` в классе `SeedData`, чтобы создать тестовые учетные записи:

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet_Initialize)]

Добавьте в контакты идентификатор пользователя и `ContactStatus` администратора. Создать контактных лиц с «Отправлено» и одного «отклонено». Добавьте идентификатор пользователя и состояние всех контактов. Отображается только один контакт:

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>Создание владельца, manager и обработчики авторизации администратора

Создайте класс `ContactIsOwnerAuthorizationHandler` в папке *authorization* . `ContactIsOwnerAuthorizationHandler` проверяет, принадлежит ли ресурс пользователю, работающему с ресурсом.

[!code-csharp[](secure-data/samples/final3/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

`ContactIsOwnerAuthorizationHandler` вызывает [контекст. Считается успешной](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) , если текущий пользователь, прошедший проверку подлинности, является владельцем контакта. Обработчики авторизации обычно:

* Возврат `context.Succeed` при соблюдении требований.
* Возвращает `Task.CompletedTask`, если требования не выполнены. `Task.CompletedTask` не является успешным или неудачным&mdash;он позволяет запускать другие обработчики авторизации.

Если необходимо явное завершение, возвращаемый [контекст. Не пройдено](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).

Приложение позволяет контактные владельцам изменения или удаления и создания собственных данных. `ContactIsOwnerAuthorizationHandler` не требуется проверять операцию, переданную в параметре требования.

### <a name="create-a-manager-authorization-handler"></a>Создайте обработчик диспетчера авторизации

Создайте класс `ContactManagerAuthorizationHandler` в папке *authorization* . `ContactManagerAuthorizationHandler` проверяет, является ли пользователь, действующий для ресурса, диспетчером. Только менеджеры могли утверждать или отклонять изменения содержимого (новые или измененные).

[!code-csharp[](secure-data/samples/final3/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>Создайте обработчик авторизации администратора

Создайте класс `ContactAdministratorsAuthorizationHandler` в папке *authorization* . `ContactAdministratorsAuthorizationHandler` проверяет, является ли пользователь, действующий для ресурса, администратором. Администратор может выполнить все операции.

[!code-csharp[](secure-data/samples/final3/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>Регистрировать обработчики авторизации

Службы, использующие Entity Framework Core, должны быть зарегистрированы для [внедрения зависимостей](xref:fundamentals/dependency-injection) с помощью [аддскопед](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions). В `ContactIsOwnerAuthorizationHandler` используется [удостоверение](xref:security/authentication/identity)ASP.NET Core, созданное на основе Entity Framework Core. Зарегистрируйте обработчики в коллекции служб, чтобы они были доступны `ContactsController` посредством [внедрения зависимостей](xref:fundamentals/dependency-injection). Добавьте следующий код в конец `ConfigureServices`:

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet_defaultPolicy&highlight=23-99)]

`ContactAdministratorsAuthorizationHandler` и `ContactManagerAuthorizationHandler` добавляются в виде Singleton. Они являются Singleton-классом, поскольку не используют EF, а вся необходимая информация находится в параметре `Context` метода `HandleRequirementAsync`.

## <a name="support-authorization"></a>Поддержка авторизации

В этом разделе вы обновите страницы Razor Pages и добавьте класс требования к операции.

### <a name="review-the-contact-operations-requirements-class"></a>Просмотрите класс требования контактные операций

Изучите класс `ContactOperations`. Этот класс содержит требования к поддерживает приложения:

[!code-csharp[](secure-data/samples/final3/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a>Создать базовый класс для страниц Razor контактов

Создайте базовый класс, который содержит службы, используемые в Razor Pages контакты. Базовый класс помещает код инициализации в одном месте:

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/DI_BasePageModel.cs)]

Предыдущий код:

* Добавляет службу `IAuthorizationService` для доступа к обработчикам авторизации.
* Добавляет службу удостоверений `UserManager`.
* Добавьте `ApplicationDbContext`.

### <a name="update-the-createmodel"></a>Обновление CreateModel

Обновите конструктор модели страницы создания, чтобы использовать базовый класс `DI_BasePageModel`:

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

Обновите метод `CreateModel.OnPostAsync` следующим образом:

* Добавьте идентификатор пользователя в модель `Contact`.
* Вызовите обработчик авторизации, чтобы убедиться, что пользователь имеет разрешение на создание контактов.

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a>Обновление IndexModel

Обновите метод `OnGetAsync`, чтобы только утвержденные контакты отображались для обычных пользователей:

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a>Обновление EditModel

Добавление обработчика авторизации, чтобы убедиться, что пользователь является владельцем контакта. Так как авторизация ресурса проверяется, атрибут `[Authorize]` недостаточно. Приложение не имеет доступа к ресурсу, при оценке атрибуты. Авторизация на основе ресурсов должен быть явный. Проверки должны выполняться, когда приложение имеет доступ к ресурсу, загрузив его в модели страниц или, загрузив его в сам обработчик. Вы обращаетесь чаще всего ресурса, передавая ключ ресурса.

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a>Обновление DeleteModel

Обновите модель страницы delete на использование обработчика авторизации, чтобы убедиться, что пользователь имеет разрешение на удаление для контакта.

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a>Внедрить службы авторизации в представления

В настоящее время в пользовательском Интерфейсе показано изменение и удаление ссылок для контактов, которые не могут изменяться.

Вставьте службу авторизации в файл *pages/_ViewImports. cshtml* , чтобы он был доступен для всех представлений:

[!code-cshtml[](secure-data/samples/final3/Pages/_ViewImports.cshtml?highlight=6-99)]

Предыдущая разметка добавляет несколько инструкций `using`.

Обновите ссылки **Edit** и **Delete** в *pages/contacts/index. cshtml* , чтобы они отображались только для пользователей с соответствующими разрешениями:

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> Скрытие ссылок от пользователей, у которых нет разрешения на изменение данных не защитить приложения. Скрытие ссылок делает приложение более удобным, отображая только допустимые ссылки. Пользователи могут взломать созданный URL-адреса для вызова редактирования и удаления данных, которыми они не владеют. На странице Razor или контроллер должен Принудительная проверка доступа для защиты данных.

### <a name="update-details"></a>Дополнительные сведения об обновлении

Обновление представления сведений, чтобы менеджеры могли утверждать или отклонять контактов:

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Details.cshtml?name=snippet)]

Обновите модель страницы сведений:

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a>Добавление или удаление пользователя к роли

Сведения об [этой ошибке](https://github.com/aspnet/AspNetCore.Docs/issues/8502) см. в следующих статьях:

* Удаление привилегий пользователя. Например, отзвука пользователя в приложении разговора.
* Добавление прав к пользователю.

<a name="challenge"></a>

## <a name="differences-between-challenge-and-forbid"></a>Различия между запросом и запретом

Это приложение задает политику по умолчанию, [требующую проверки подлинности пользователей](#require-authenticated-users). Следующий код позволяет анонимным пользователям. Анонимным пользователям разрешено показывать различия между запросом и запретом.

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Details2.cshtml.cs?name=snippet)]

В приведенном выше коде:

* Если пользователь **не** прошел проверку подлинности, возвращается `ChallengeResult`. При возвращении `ChallengeResult` пользователь перенаправляется на страницу входа.
* Если пользователь прошел проверку подлинности, но не авторизован, возвращается `ForbidResult`. При возвращении `ForbidResult` пользователь перенаправляется на страницу отказ в доступе.

## <a name="test-the-completed-app"></a>Тестирование завершенного приложения

Если вы еще не установили пароль для заполненных учетных записей пользователей, используйте [средство диспетчера секретов](xref:security/app-secrets#secret-manager) , чтобы задать пароль:

* Выберите надежный пароль: используйте восемь или дополнительные символы хотя бы один символ верхнего регистра, число и символ. Например, `Passw0rd!` соответствует требованиям к надежному паролю.
* Выполните следующую команду из папки проекта, где `<PW>` — это пароль:

  ```dotnetcli
  dotnet user-secrets set SeedUserPW <PW>
  ```

Если приложение имеет контактов:

* Удалите все записи в таблице `Contact`.
* Перезапустите приложение, чтобы заполнить базу данных.

Простой способ протестировать готовое приложение — запуск три различных браузеров (или инкогнито или InPrivate сеансов). В одном браузере Зарегистрируйте нового пользователя (например, `test@example.com`). Войдите в каждом браузере с другим пользователем. Проверьте следующие операции:

* Зарегистрированные пользователи могут просматривать все утвержденные контактные данные.
* Зарегистрированным пользователям может изменять и удалять свои собственные данные.
* Руководители могут утверждать или отклонять контактных данных. В представлении `Details` отображаются кнопки **утверждения** и **отклонения** .
* Администраторы могут утверждать или отклонять и изменить или удалить все данные.

| Пользовательская                | Заполняется данными в приложении | Параметры                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | Нет                | Изменить или удалить данные принадлежат.                |
| manager@contoso.com | Да               | Утверждать или отклонять и изменить или удалить данные принадлежат. |
| admin@contoso.com   | Да               | Утверждать или отклонять и изменить или удалить все данные. |

Создание контакта в браузере администратора. Скопируйте URL-адрес для удаления и редактирование откуда обратитесь к администратору. Вставьте эти ссылки в браузере пользователя теста, чтобы убедиться, что тестовый пользователь не может выполнять эти операции.

## <a name="create-the-starter-app"></a>Создать приложение начального уровня

* Создание приложения Razor Pages с именем «ContactManager»
  * Создайте приложение с **учетными записями отдельных пользователей**.
  * Назовите его «ContactManager», пространство имен соответствует пространству имен, используемые в образце.
  * `-uld` указывает LocalDB вместо SQLite

  ```dotnetcli
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* Добавление *моделей/Contact. CS*:

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* Формирование шаблона модели `Contact`.
* Создание первоначальной миграции и обновления базы данных:

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
```

Если при выполнении команды `dotnet aspnet-codegenerator razorpage` возникла ошибка, см. [эту ошибку в GitHub](https://github.com/aspnet/Scaffolding/issues/984).

* Обновите привязку **ContactManager** в файле *pages/shared/_layout. cshtml* :

 ```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Contacts/Index">ContactManager</a>
  ```

* Протестируйте приложение, создание, изменение и удаление контакта

### <a name="seed-the-database"></a>Заполнение базы данных

Добавьте класс [сиддата](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) в папку *Data* :

[!code-csharp[](secure-data/samples/starter3/Data/SeedData.cs)]

Вызовите `SeedData.Initialize` из `Main`:

[!code-csharp[](secure-data/samples/starter3/Program.cs)]

Проверьте, что приложение заполняется данными базы данных. При возникновении любых строк в контакте DB, не запускается метод заполнения.

::: moniker-end

::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"

Этом руководстве показано, как создать веб-приложение ASP.NET Core с помощью данных пользователя с помощью авторизации. Отображается список контактов, прошедшие проверку подлинности (зарегистрированного) пользователи создали. Существует три группы безопасности:

* **Зарегистрированные пользователи** могут просматривать все утвержденные данные, а также изменять и удалять свои данные.
* **Руководители** могут утверждать или отклонять контактные данные. Только утвержденные контакты отображаются для пользователей.
* **Администраторы** могут утверждать, отклонять и изменять и удалять любые данные.

На следующем рисунке пользователь Рик (`rick@example.com`) вошел в. Рик может просматривать только утвержденные контакты и **изменять**/**Удалить**/**создавать новые** ссылки для своих контактов. Ссылки **Edit** и **Delete** отображаются только в последней записи, созданной Рик. Другие пользователи увидят последнюю запись, после менеджеру или администратору изменяет состояние на «Утверждено».

![Снимок экрана, показывающий Рик входа](secure-data/_static/rick.png)

На следующем рисунке `manager@contoso.com` вход в и в роли руководителя:

![Снимок экрана, показывающий manager@contoso.com выполнен вход](secure-data/_static/manager1.png)

На следующем рисунке показана менеджеров Просмотр подробностей контакта:

![Представление руководителя контакта](secure-data/_static/manager.png)

Кнопки **утвердить** и **отклонить** отображаются только для руководителей и администраторов.

На следующем рисунке `admin@contoso.com` вход выполнен и в роли администратора:

![Снимок экрана, показывающий admin@contoso.com выполнен вход](secure-data/_static/admin.png)

Администратор имеет все привилегии. Она может чтения, изменить или удалить любой контакт и изменить состояние контактов.

Приложение было создано с помощью [формирования шаблонов](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) следующей модели `Contact`:

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

Пример содержит следующие обработчики авторизации:

* `ContactIsOwnerAuthorizationHandler`: гарантирует, что пользователь сможет изменять только свои данные.
* `ContactManagerAuthorizationHandler`: позволяет руководителям утверждать или отклонять контакты.
* `ContactAdministratorsAuthorizationHandler`: позволяет администраторам утверждать или отклонять контакты, а также изменять или удалять контакты.

## <a name="prerequisites"></a>Предварительные требования

Этот учебник является дополнительным. Вы должны быть знакомы с:

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [Проверка подлинности](xref:security/authentication/identity)
* [Подтверждение учетной записи и восстановление пароля](xref:security/authentication/accconfirm)
* [Авторизация](xref:security/authorization/introduction)
* [Entity Framework Core](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a>Начальный и завершенное приложение

[Скачайте](xref:index#how-to-download-a-sample) [готовое](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) приложение. [Протестируйте](#test-the-completed-app) готовое приложение, чтобы ознакомиться с его функциями безопасности.

### <a name="the-starter-app"></a>Приложение начального уровня

[Скачайте](xref:index#how-to-download-a-sample) [Начальное](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) приложение.

Запустите приложение, коснитесь ссылки **ContactManager** и убедитесь, что вы можете создать, изменить и удалить контакт.

## <a name="secure-user-data"></a>Обеспечение безопасности данных пользователя

В следующих разделах есть все основные шаги по созданию приложения данные безопасности пользователей. Могут оказаться полезными для ссылки на готового проекта.

### <a name="tie-the-contact-data-to-the-user"></a>Привязать контактные данные для пользователя

Используйте идентификатор пользователя [удостоверения](xref:security/authentication/identity) ASP.NET, чтобы убедиться, что пользователи могут изменять данные, но не данные других пользователей. Добавьте `OwnerID` и `ContactStatus` в модель `Contact`:

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

`OwnerID` — это идентификатор пользователя из таблицы `AspNetUser` в базе данных [удостоверений](xref:security/authentication/identity) . Поле `Status` определяет, можно ли просматривать контакт обычными пользователями.

Создание новой миграции и обновления базы данных:

```dotnetcli
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a>Добавление служб роли удостоверению

Добавление [аддролес](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) для добавления служб ролей:

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a>Требовать от пользователей, прошедших проверку подлинности

Задайте политику проверки подлинности по умолчанию, чтобы требовать проверку подлинности пользователей:

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 Можно отказаться от проверки подлинности на уровне страницы Razor, контроллера или метода действия с помощью атрибута `[AllowAnonymous]`. Задание политики проверки подлинности по умолчанию требовать от пользователей пройти проверку подлинности защищает вновь добавленный Razor Pages и контроллеры. Наличие проверки подлинности по умолчанию более безопасна, чем использование новых контроллеров и Razor Pages включением атрибута `[Authorize]`.

Добавьте [allowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) в индекс, About и Contact Pages, чтобы анонимные пользователи могли получить сведения о сайте перед регистрацией.

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a>Настройка тестовой учетной записью

Класс `SeedData` создает две учетные записи: Administrator и Manager. Используйте [средство диспетчера секретов](xref:security/app-secrets) , чтобы задать пароль для этих учетных записей. Задайте пароль из каталога проекта (каталога, содержащего *Program.CS*):

```dotnetcli
dotnet user-secrets set SeedUserPW <PW>
```

Если надежный пароль не указан, при вызове `SeedData.Initialize` возникает исключение.

Обновите `Main`, чтобы использовать тестовый пароль:

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a>Создание тестовых учетных записей и обновление контактов

Обновите метод `Initialize` в классе `SeedData`, чтобы создать тестовые учетные записи:

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

Добавьте в контакты идентификатор пользователя и `ContactStatus` администратора. Создать контактных лиц с «Отправлено» и одного «отклонено». Добавьте идентификатор пользователя и состояние всех контактов. Отображается только один контакт:

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>Создание владельца, manager и обработчики авторизации администратора

Создайте папку *авторизации* и создайте в ней класс `ContactIsOwnerAuthorizationHandler`. `ContactIsOwnerAuthorizationHandler` проверяет, принадлежит ли ресурс пользователю, работающему с ресурсом.

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

`ContactIsOwnerAuthorizationHandler` вызывает [контекст. Считается успешной](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) , если текущий пользователь, прошедший проверку подлинности, является владельцем контакта. Обработчики авторизации обычно:

* Возврат `context.Succeed` при соблюдении требований.
* Возвращает `Task.CompletedTask`, если требования не выполнены. `Task.CompletedTask` не является успешным или неудачным&mdash;он позволяет запускать другие обработчики авторизации.

Если необходимо явное завершение, возвращаемый [контекст. Не пройдено](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).

Приложение позволяет контактные владельцам изменения или удаления и создания собственных данных. `ContactIsOwnerAuthorizationHandler` не требуется проверять операцию, переданную в параметре требования.

### <a name="create-a-manager-authorization-handler"></a>Создайте обработчик диспетчера авторизации

Создайте класс `ContactManagerAuthorizationHandler` в папке *authorization* . `ContactManagerAuthorizationHandler` проверяет, является ли пользователь, действующий для ресурса, диспетчером. Только менеджеры могли утверждать или отклонять изменения содержимого (новые или измененные).

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>Создайте обработчик авторизации администратора

Создайте класс `ContactAdministratorsAuthorizationHandler` в папке *authorization* . `ContactAdministratorsAuthorizationHandler` проверяет, является ли пользователь, действующий для ресурса, администратором. Администратор может выполнить все операции.

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>Регистрировать обработчики авторизации

Службы, использующие Entity Framework Core, должны быть зарегистрированы для [внедрения зависимостей](xref:fundamentals/dependency-injection) с помощью [аддскопед](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions). В `ContactIsOwnerAuthorizationHandler` используется [удостоверение](xref:security/authentication/identity)ASP.NET Core, созданное на основе Entity Framework Core. Зарегистрируйте обработчики в коллекции служб, чтобы они были доступны `ContactsController` посредством [внедрения зависимостей](xref:fundamentals/dependency-injection). Добавьте следующий код в конец `ConfigureServices`:

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

`ContactAdministratorsAuthorizationHandler` и `ContactManagerAuthorizationHandler` добавляются в виде Singleton. Они являются Singleton-классом, поскольку не используют EF, а вся необходимая информация находится в параметре `Context` метода `HandleRequirementAsync`.

## <a name="support-authorization"></a>Поддержка авторизации

В этом разделе вы обновите страницы Razor Pages и добавьте класс требования к операции.

### <a name="review-the-contact-operations-requirements-class"></a>Просмотрите класс требования контактные операций

Изучите класс `ContactOperations`. Этот класс содержит требования к поддерживает приложения:

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a>Создать базовый класс для страниц Razor контактов

Создайте базовый класс, который содержит службы, используемые в Razor Pages контакты. Базовый класс помещает код инициализации в одном месте:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

Предыдущий код:

* Добавляет службу `IAuthorizationService` для доступа к обработчикам авторизации.
* Добавляет службу удостоверений `UserManager`.
* Добавьте `ApplicationDbContext`.

### <a name="update-the-createmodel"></a>Обновление CreateModel

Обновите конструктор модели страницы создания, чтобы использовать базовый класс `DI_BasePageModel`:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

Обновите метод `CreateModel.OnPostAsync` следующим образом:

* Добавьте идентификатор пользователя в модель `Contact`.
* Вызовите обработчик авторизации, чтобы убедиться, что пользователь имеет разрешение на создание контактов.

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a>Обновление IndexModel

Обновите метод `OnGetAsync`, чтобы только утвержденные контакты отображались для обычных пользователей:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a>Обновление EditModel

Добавление обработчика авторизации, чтобы убедиться, что пользователь является владельцем контакта. Так как авторизация ресурса проверяется, атрибут `[Authorize]` недостаточно. Приложение не имеет доступа к ресурсу, при оценке атрибуты. Авторизация на основе ресурсов должен быть явный. Проверки должны выполняться, когда приложение имеет доступ к ресурсу, загрузив его в модели страниц или, загрузив его в сам обработчик. Вы обращаетесь чаще всего ресурса, передавая ключ ресурса.

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a>Обновление DeleteModel

Обновите модель страницы delete на использование обработчика авторизации, чтобы убедиться, что пользователь имеет разрешение на удаление для контакта.

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a>Внедрить службы авторизации в представления

В настоящее время в пользовательском Интерфейсе показано изменение и удаление ссылок для контактов, которые не могут изменяться.

Вставьте службу авторизации в файл *views/_ViewImports. cshtml* , чтобы она была доступна для всех представлений:

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

Предыдущая разметка добавляет несколько инструкций `using`.

Обновите ссылки **Edit** и **Delete** в *pages/contacts/index. cshtml* , чтобы они отображались только для пользователей с соответствующими разрешениями:

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> Скрытие ссылок от пользователей, у которых нет разрешения на изменение данных не защитить приложения. Скрытие ссылок делает приложение более удобным, отображая только допустимые ссылки. Пользователи могут взломать созданный URL-адреса для вызова редактирования и удаления данных, которыми они не владеют. На странице Razor или контроллер должен Принудительная проверка доступа для защиты данных.

### <a name="update-details"></a>Дополнительные сведения об обновлении

Обновление представления сведений, чтобы менеджеры могли утверждать или отклонять контактов:

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

Обновите модель страницы сведений:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a>Добавление или удаление пользователя к роли

Сведения об [этой ошибке](https://github.com/aspnet/AspNetCore.Docs/issues/8502) см. в следующих статьях:

* Удаление привилегий пользователя. Например, отзвука пользователя в приложении разговора.
* Добавление прав к пользователю.

## <a name="test-the-completed-app"></a>Тестирование завершенного приложения

Если вы еще не установили пароль для заполненных учетных записей пользователей, используйте [средство диспетчера секретов](xref:security/app-secrets#secret-manager) , чтобы задать пароль:

* Выберите надежный пароль: используйте восемь или дополнительные символы хотя бы один символ верхнего регистра, число и символ. Например, `Passw0rd!` соответствует требованиям к надежному паролю.
* Выполните следующую команду из папки проекта, где `<PW>` — это пароль:

  ```dotnetcli
  dotnet user-secrets set SeedUserPW <PW>
  ```

* Удаление и обновление базы данных

  ```dotnetcli
  dotnet ef database drop -f
  dotnet ef database update  
  ```

* Перезапустите приложение, чтобы заполнить базу данных.

Простой способ протестировать готовое приложение — запуск три различных браузеров (или инкогнито или InPrivate сеансов). В одном браузере Зарегистрируйте нового пользователя (например, `test@example.com`). Войдите в каждом браузере с другим пользователем. Проверьте следующие операции:

* Зарегистрированные пользователи могут просматривать все утвержденные контактные данные.
* Зарегистрированным пользователям может изменять и удалять свои собственные данные.
* Руководители могут утверждать или отклонять контактных данных. В представлении `Details` отображаются кнопки **утверждения** и **отклонения** .
* Администраторы могут утверждать или отклонять и изменить или удалить все данные.

| Пользовательская                | Заполняется данными в приложении | Параметры                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | Нет                | Изменить или удалить данные принадлежат.                |
| manager@contoso.com | Да               | Утверждать или отклонять и изменить или удалить данные принадлежат. |
| admin@contoso.com   | Да               | Утверждать или отклонять и изменить или удалить все данные. |

Создание контакта в браузере администратора. Скопируйте URL-адрес для удаления и редактирование откуда обратитесь к администратору. Вставьте эти ссылки в браузере пользователя теста, чтобы убедиться, что тестовый пользователь не может выполнять эти операции.

## <a name="create-the-starter-app"></a>Создать приложение начального уровня

* Создание приложения Razor Pages с именем «ContactManager»
  * Создайте приложение с **учетными записями отдельных пользователей**.
  * Назовите его «ContactManager», пространство имен соответствует пространству имен, используемые в образце.
  * `-uld` указывает LocalDB вместо SQLite

  ```dotnetcli
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* Добавление *моделей/Contact. CS*:

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* Формирование шаблона модели `Contact`.
* Создание первоначальной миграции и обновления базы данных:

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
  dotnet ef database drop -f
  dotnet ef migrations add initial
  dotnet ef database update
  ```

* Обновите привязку **ContactManager** в файле *pages/_layout. cshtml* :

  ```cshtml
  <a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
  ```

* Протестируйте приложение, создание, изменение и удаление контакта

### <a name="seed-the-database"></a>Заполнение базы данных

Добавьте класс [сиддата](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) в папку *Data* .

Вызовите `SeedData.Initialize` из `Main`:

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

Проверьте, что приложение заполняется данными базы данных. При возникновении любых строк в контакте DB, не запускается метод заполнения.

::: moniker-end

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a>Дополнительные ресурсы

* [Создание веб-приложения .NET Core и базы данных SQL в службе приложений Azure](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* [ASP.NET Core лаборатории авторизации](https://github.com/blowdart/AspNetAuthorizationWorkshop). Это лабораторное занятие содержит более подробные сведения о функциях безопасности, представленных в этом руководстве.
* <xref:security/authorization/introduction>
* [Пользовательская авторизация на основе политик](xref:security/authorization/policies)
