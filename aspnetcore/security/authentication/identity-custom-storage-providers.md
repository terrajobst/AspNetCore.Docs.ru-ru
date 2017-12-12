---
title: "Поставщики пользовательского хранилища для ASP.NET Core Identity | Документы Microsoft"
author: ardalis
description: "Сведения о настройке поставщиков пользовательского хранилища для ASP.NET Core Identity."
keywords: "Поставщики ASP.NET Core, удостоверение, пользовательского хранилища"
ms.author: riande
manager: wpickett
ms.date: 05/24/2017
ms.topic: article
ms.assetid: b2ace545-ecf6-4664-b31e-b65bd4a6b025
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-custom-storage-providers
ms.openlocfilehash: 687ca96be5121502e816bdc856e17dcd5923fe05
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="custom-storage-providers-for-aspnet-core-identity"></a>Поставщики пользовательского хранилища для ASP.NET Core Identity

По [Стив Смит](https://ardalis.com/)

Удостоверение ASP.NET Core является расширяемой системой, что дает возможность создать поставщика пользовательского хранилища и подключите его к приложению. В этом разделе описывается создание поставщика хранилища, настроенные для ASP.NET Core Identity. Рассматриваются важные понятия для создания собственного поставщика хранилища, но не пошагового руководства.

[Просмотреть или загрузить пример из GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample).

## <a name="introduction"></a>Вступление

По умолчанию система ASP.NET Core Identity сохраняет сведения о пользователе в базе данных SQL Server с использованием Entity Framework Core. Для многих приложений этот способ работает хорошо. Тем не менее можно использовать механизм сохранения различных или схемы данных. Пример:

* Вы используете [табличного хранилища Azure](https://docs.microsoft.com/azure/storage/) или другом хранилище данных.
* Таблицы базы данных имеют другую структуру. 
* Вы можете использовать подхода доступа различных данных, такие как [Dapper](https://github.com/StackExchange/Dapper). 

В каждом из этих случаев можно указать настраиваемый поставщик для вашего механизм хранения и подключить этот поставщик в приложение.

Удостоверение ASP.NET Core включается в шаблонах проектов в Visual Studio с помощью параметра «Отдельных учетных записей пользователей».

При использовании .NET Core CLI, добавить `-au Individual`:

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
```

## <a name="the-aspnet-core-identity-architecture"></a>Архитектура ASP.NET Core Identity

ASP.NET Core Identity состоит из класса с именами менеджеров и хранилища. *Диспетчеры* являются классами высокого уровня, который разработчик приложения использует для выполнения операций, таких как создание идентификатора пользователя. *Магазины* являются классами более низкого уровня, укажите, каким образом сущности, такие как пользователи и роли, сохраняются. Выполните хранилищ [шаблон репозитория](http://deviq.com/repository-pattern/) и тесно связана с механизм сохраняемости. Диспетчеры связаны с хранилищами, что означает, что механизм сохранения можно заменить без изменения кода приложения (за исключением конфигурации).

На следующей диаграмме показан как веб-приложение взаимодействует с руководителей, а хранилищ для взаимодействия с уровня доступа к данным.

![Приложения ASP.NET Core, работают с диспетчерами (например, 'UserManager', 'RoleManager'). Диспетчеры работают с хранилищами (например, «UserStore») которые взаимодействуют с источником данных с помощью библиотеки, такие как Entity Framework Core.](identity-custom-storage-providers/_static/identity-architecture-diagram.png)

Чтобы создать поставщика пользовательского хранилища, создайте источник данных, уровень доступа к данным и хранилища классы, которые взаимодействуют с этот уровень доступа к данным (зеленый серые поля и на схеме выше). Нет необходимости настраивать руководителей или коде приложения, который взаимодействует с ними (синие прямоугольники выше).

При создании нового экземпляра `UserManager` или `RoleManager` укажите тип класса пользователя и передать экземпляр класса хранилище в качестве аргумента. Такой подход позволяет подключить пользовательские классы ASP.NET Core. 

[Перенастройка приложения для использования нового поставщика хранилища](#reconfigure-app-to-use-new-storage-provider) показано, как создать экземпляр `UserManager` и `RoleManager` настроенного хранилища.

## <a name="aspnet-core-identity-stores-data-types"></a>ASP.NET Core удостоверения хранит типы данных

[ASP.NET Core Identity](https://github.com/aspnet/identity) типы данных описаны в следующих разделах:

### <a name="users"></a>Users

Зарегистрированным пользователям вашего веб-сайта. [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser) типа может расширенные или использовать в качестве примера для пользовательского типа. Необходимо наследовать от определенного типа для реализации решения хранилища пользовательское удостоверение.

### <a name="user-claims"></a>Утверждения пользователей

Набор инструкций (или [утверждений](https://docs.microsoft.com//dotnet/api/system.security.claims.claim)) о пользователе, которые представляют удостоверение пользователя. Можно включить большего выражения удостоверение пользователя, чем можно создать с помощью ролей.

### <a name="user-logins"></a>Имена входа

Сведения о поставщике внешней проверки подлинности (например, Facebook или учетную запись Майкрософт) для использования при входе пользователя. [Пример](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuserlogin)

### <a name="roles"></a>Роли

Группы авторизации для веб-узла. Содержит имя роли идентификатор и роли (например, «Admin» или «Employee»). [Пример](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityrole)

## <a name="the-data-access-layer"></a>Уровень доступа к данным

В этом разделе предполагается, что вы знакомы с механизм сохраняемости, который будет использоваться и как создать сущности для этого механизма. В этом разделе приводятся подробные сведения о создании репозитории или классов доступа к данным; он предоставляет некоторые рекомендации о проектные решения, при работе с ASP.NET Core Identity.

У вас много свободу при проектировании доступа к данным для поставщика настраиваемого хранилища. Необходимо создать механизмов сохраняемости для компонентов, которые будут использоваться в приложении. Например если роли не используются в приложении, необходимо создать хранилище для ролей или назначения пользователей и роли. Технологии и существующей инфраструктуры может потребоваться структуру, сильно отличается от реализации по умолчанию для ASP.NET Core Identity. В уровне доступа к данным предоставляют логику для работы со структурой хранилища реализации.

Уровень доступа к данным предоставляет логику для сохранения данных из ASP.NET Core Identity к источнику данных. Уровень доступа к данным для поставщика настраиваемого хранилища может включать следующие классы для хранения сведений о пользователе и роли.

### <a name="context-class"></a>Context - класс

Инкапсулирует сведения для подключения к вашей механизм сохранения и выполнения запросов. Несколько классов данных требуют экземпляр этого класса обычно предоставляются через внедрения зависимостей. [Пример](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identitydbcontext-1).

### <a name="user-storage"></a>Хранилище пользователя

Хранит и извлекает сведения о пользователе (например, хэш имени и пароля пользователя). [Пример](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="role-storage"></a>Роль хранилища

Хранит и извлекает сведения о роли (например, имя роли). [Пример](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1)

### <a name="userclaims-storage"></a>Хранилище объектов Userclaim

Сохраняет и извлекает информацию об утверждении пользователя (например, тип утверждения и значение). [Пример](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userlogins-storage"></a>Хранилище объектов userlogin

Хранит и извлекает сведения об имени входа пользователя (например, внешний поставщик аутентификации). [Пример](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userrole-storage"></a>Объем хранилища UserRole

Хранит и извлекает назначенных ролей для пользователей. [Пример](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

**Совет:** реализовать только классы, которые будут использоваться в приложении.

В классы доступа к данным предоставляют код для выполнения операций с данными для вашего механизма сохраняемости. Например, в пределах пользовательского поставщика может иметь следующий код, чтобы создать нового пользователя в *хранения* класса:

[!code-csharp[Main](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs?name=createuser&highlight=7)]

Логика реализации для создания пользователя находится в ``_usersTable.CreateAsync`` метод, показанный ниже.

## <a name="customize-the-user-class"></a>Настроить класс пользователя

При реализации поставщика хранилища, создание пользовательского класса, который эквивалентен [ `IdentityUser` класса](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser).

Как минимум, необходимо включить класс user `Id` и `UserName` свойства.

`IdentityUser` Класс определяет свойства, ``UserManager`` вызовов при выполнении запрошенным операциям. Тип по умолчанию `Id` свойство является строкой, но может наследовать от `IdentityUser<TKey, TUserClaim, TUserRole, TUserLogin, TUserToken>` и указать другой тип. Платформа ожидает, что реализация хранилища для обработки преобразований типа данных.

## <a name="customize-the-user-store"></a>Настроить хранилище пользователей

Создайте `UserStore` класс, предоставляющий методы для всех операций с данными пользователя. Этот класс является эквивалентом [UserStore<TUser> ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.userstore-1) класса. В вашей `UserStore` , реализуйте интерфейс `IUserStore<TUser>` и необязательные интерфейсы, необходимые. Выберите необязательные интерфейсы для реализации на основании функциональные возможности, предоставляемые в приложении.

### <a name="optional-interfaces"></a>Необязательные интерфейсы

- IUserRoleStore https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserrolestore-1
- IUserClaimStore https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserclaimstore-1
- IUserPasswordStore https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserpasswordstore-1
- IUserSecurityStampStore<!-- make these all links and remove / -->
- IUserEmailStore
- IPhoneNumberStore
- IQueryableUserStore
- IUserLoginStore
- IUserTwoFactorStore
- IUserLockoutStore

Необязательные интерфейсы наследуются от класса `IUserStore`. Вы увидите хранения пользователя частично реализовано образец [здесь](https://github.com/aspnet/Docs/blob/master/aspnetcore/security/authentication/identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs).

В пределах `UserStore` , использовать классов доступа к данным, которые созданы для выполнения операций. Они передаются в с помощью внедрения зависимости. Например, в SQL Server с Dapper реализацию `UserStore` класс имеет `CreateAsync` метода, использующего экземпляр `DapperUsersTable` для вставки новой записи:

[!code-csharp[Main](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/DapperUsersTable.cs?name=createuser&highlight=7)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>Интерфейсы для реализации при настройке хранилища пользователя

- **IUserStore**  
 [IUserStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserstore-1) интерфейс — только интерфейс, необходимо реализовать в хранилище пользователя. Он определяет методы для создания, обновления, удаления и получения пользователей.
- **IUserClaimStore**  
 [IUserClaimStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserclaimstore-1) интерфейса определяются методы, реализуемые для включения заявок на доступ пользователя. Содержит методы для добавления, удаления и получает утверждения пользователя.
- **IUserLoginStore**  
 [IUserLoginStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserloginstore-1) определяет методы, реализуемые для включения внешних поставщиков аутентификации. Содержит методы для добавления, удаления и извлечения имен входа пользователей и метод для извлечения на основе сведений имени входа пользователя.
- **IUserRoleStore**  
 [IUserRoleStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserrolestore-1) интерфейса определяются методы, которые можно реализовать, чтобы сопоставить пользователя с ролью. Содержит методы для добавления, удаления и извлечения роли пользователя, а также метод для проверки, если пользователь назначен роли.
- **IUserPasswordStore**  
 [IUserPasswordStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserpasswordstore-1) интерфейса определяются методы, которые можно реализовать для сохранения Хешированные пароли. Содержит методы для получения и задания хэшированный пароль и метод, который указывает, ли пользователь использовать пароль.
- **IUserSecurityStampStore**  
 [IUserSecurityStampStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1) интерфейса определяются методы, которые реализуют использовать метку безопасности для, указывающее, изменилось ли учетная запись пользователя. Это метка обновляется, когда пользователь изменяет пароль, или добавляет или удаляет имена входа. Содержит методы для получения и задания метку безопасности.
- **IUserTwoFactorStore**  
 [IUserTwoFactorStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iusertwofactorstore-1) интерфейса определяются методы, реализуемые для поддержки двухфакторная проверка подлинности. Содержит методы для получения и задания двухфакторная проверка подлинности включена ли для пользователя.
- **IUserPhoneNumberStore**  
 [IUserPhoneNumberStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserphonenumberstore-1) интерфейса определяются методы, которые реализуют для хранения номеров телефонов пользователя. Содержит методы для получения и задания, номер телефона и подтверждается ли номер телефона.
- **IUserEmailStore**  
 [IUserEmailStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuseremailstore-1) интерфейса определяются методы, реализуемые для хранения адреса электронной почты пользователя. Содержит методы для получения и задания адреса электронной почты и подтверждается ли сообщение электронной почты.
- **IUserLockoutStore**  
 [IUserLockoutStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserlockoutstore-1) интерфейса определяются методы, реализуемые для хранения сведений о блокировке учетной записи. Содержит методы для отслеживания неудачных попыток доступа и блокировки.
- **IQueryableUserStore**  
 [IQueryableUserStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iqueryableuserstore-1) интерфейс определяет реализуют члены для предоставления хранилища поддерживает запросы пользователя.

Реализация интерфейсов, которые необходимы в приложении. Пример:

```csharp
public class UserStore : IUserStore<IdentityUser>,
                         IUserClaimStore<IdentityUser>,
                         IUserLoginStore<IdentityUser>,
                         IUserRoleStore<IdentityUser>,
                         IUserPasswordStore<IdentityUser>,
                         IUserSecurityStampStore<IdentityUser>
{
    // interface implementations not shown
}
```

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a>IdentityUserClaim, IdentityUserLogin и IdentityUserRole

``Microsoft.AspNet.Identity.EntityFramework`` Пространство имен содержит реализации [IdentityUserClaim](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserclaim-1), [IdentityUserLogin](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuserlogin), и [IdentityUserRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserrole-1) классы. Если вы используете эти функции, можно создавать собственные версии этих классов и определять свойства для вашего приложения. Тем не менее иногда бывает более эффективно не загружать эти сущности в память при выполнение основных операций (например, добавляя или удаляя утверждения пользователя). Вместо этого внутреннего хранилища классы могут выполнять эти операции непосредственно в источнике данных. Например ``UserStore.GetClaimsAsync`` можно вызвать метод ``userClaimTable.FindByUserId(user.Id)`` выполнения запроса на метод, который непосредственно таблицы и возвращают список утверждений.

## <a name="customize-the-role-class"></a>Настроить класс ролей

При реализации поставщика хранилища роли, можно создать тип пользовательской роли. Он не нужен реализовывал определенный интерфейс, но он должен иметь `Id` и обычно имеет `Name` свойства.

Ниже приведен пример класса роли.

[!code-csharp[Main](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/ApplicationRole.cs)]

## <a name="customize-the-role-store"></a>Настройка хранилища ролей

Можно создать ``RoleStore`` класс, предоставляющий методы для всех операций с данными на ролях. Этот класс является эквивалентом [RoleStore<TRole> ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1) класса. В `RoleStore` реализовать класс, ``IRoleStore<TRole>`` и при необходимости ``IQueryableRoleStore<TRole>`` интерфейса.

- **IRoleStore&lt;TRole&gt;**  
 [IRoleStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.irolestore-1) интерфейса определяются методы реализации в классе хранилище роли. Содержит методы для создания, обновления, удаления и получения ролей.
- **RoleStore&lt;TRole&gt;**  
 Чтобы настроить `RoleStore`, создайте класс, реализующий `IRoleStore` интерфейса. 

## <a name="reconfigure-app-to-use-new-storage-provider"></a>Перенастройка приложения для использования нового поставщика хранилища

После реализации поставщика хранилища, вы настраиваете приложение для его использования. Если приложение используется поставщик по умолчанию, замените его настраиваемого поставщика.

1. Удалить `Microsoft.AspNetCore.EntityFramework.Identity` пакет NuGet.
1. Если поставщик хранилища находится в отдельный проект или пакет, добавьте ссылку на него.
1. Замените все ссылки на `Microsoft.AspNetCore.EntityFramework.Identity` с с помощью инструкции для пространства имен поставщика хранилища.
1. В ``ConfigureServices`` метод изменение `AddIdentity` метод для использования пользовательских типов. Можно создать собственные методы расширения для этой цели. В разделе [IdentityServiceCollectionExtensions](https://github.com/aspnet/Identity/blob/rel/1.1.0/src/Microsoft.AspNetCore.Identity/IdentityServiceCollectionExtensions.cs) в качестве примера.
1. При использовании ролей обновление `RoleManager` для использования вашей `RoleStore` класса.
1. Обновите строку соединения и учетные данные для конфигурации приложения.

Пример.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add identity types
    services.AddIdentity<ApplicationUser, ApplicationRole>()
        .AddDefaultTokenProviders();

    // Identity Services
    services.AddTransient<IUserStore<ApplicationUser>, CustomUserStore>();
    services.AddTransient<IRoleStore<ApplicationRole>, CustomRoleStore>();
    string connectionString = Configuration.GetConnectionString("DefaultConnection");
    services.AddTransient<SqlConnection>(e => new SqlConnection(connectionString));
    services.AddTransient<DapperUsersTable>();

    // additional configuration
}
```

## <a name="references"></a>Ссылки

- [Поставщики пользовательского хранилища для ASP.NET Identity](https://docs.microsoft.com/aspnet/identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity)
- [ASP.NET Core Identity](https://github.com/aspnet/identity) -этот репозиторий содержит ссылки на сообщества сохраняется поставщиков хранилища.
