---
title: Пользовательские поставщики хранилищ для ASP.NET Core Identity
author: ardalis
description: Узнайте, как настроить пользовательские поставщики хранилищ для ASP.NET Core Identity.
ms.author: riande
ms.date: 09/17/2018
uid: security/authentication/identity-custom-storage-providers
ms.openlocfilehash: db51c39cc700f93917f54c80adbfe7922ffcd67e
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011265"
---
# <a name="custom-storage-providers-for-aspnet-core-identity"></a>Пользовательские поставщики хранилищ для ASP.NET Core Identity

Автор: [Стив Смит](https://ardalis.com/) (Steve Smith)

Удостоверение ASP.NET Core является расширяемой системой, что дает возможность создать поставщика пользовательского хранилища и подключить его к своему приложению. В этом разделе описывается создание поставщика настраиваемого хранилища для ASP.NET Core Identity. Он содержит важные основные понятия для создания собственного поставщика хранилища, но не пошаговое руководство.

[Просмотреть или скачать образец с GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample).

## <a name="introduction"></a>Вступление

По умолчанию система удостоверения ASP.NET Core хранит сведения о пользователе в базе данных SQL Server с помощью Entity Framework Core. Для многих приложений этот способ работает хорошо. Тем не менее можно использовать механизм сохранения различных или схемы данных. Пример:

* Использовании [Azure Table Storage](https://docs.microsoft.com/azure/storage/) или другом хранилище данных.
* Таблицы базы данных иметь различную структуру. 
* Вы можете использовать подхода к доступа различных данных, такие как [Dapper](https://github.com/StackExchange/Dapper). 

В каждом из этих случаев можно написать настраиваемый поставщик для вашего механизма хранения и подключить этот поставщик в приложение.

Удостоверение ASP.NET Core включается в шаблоны проектов в Visual Studio с параметром «Учетные записи отдельных пользователей».

При использовании интерфейса командной строки .NET Core добавьте `-au Individual`:

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
```

## <a name="the-aspnet-core-identity-architecture"></a>Архитектура ASP.NET Core Identity

Удостоверение ASP.NET Core состоит из классов, называемых диспетчеры и хранилищами. *Диспетчеры* являются высокоуровневые классы, которые разработчик приложения использует для выполнения операций, таких как создание удостоверения пользователя. *Магазины* являются классами более низкого уровня, которые указывают, как сущности, такие как пользователи и роли, сохраняются. Выполните хранилищ [шаблон репозитория](xref:fundamentals/repository-pattern) и тесно связан с механизмом сохраняемости. Диспетчеры отделены от хранилища, что означает, что вы можете заменить механизм сохранения без изменения кода приложения (за исключением конфигурации).

В примере ниже показан как веб-приложения взаимодействует с руководителей, а хранилищ для взаимодействия с уровня доступа к данным.

![Приложения ASP.NET Core работать с диспетчерами (например, «UserManager», «RoleManager»). Диспетчеры работают с хранилищами (например, «UserStore») которые взаимодействуют с источником данных, с помощью библиотеки, например Entity Framework Core.](identity-custom-storage-providers/_static/identity-architecture-diagram.png)

Чтобы создать поставщика пользовательского хранилища, создайте источник данных, уровень доступа к данным и хранилища классы, которые взаимодействуют с этот уровень доступа к данным (зеленый и серые прямоугольники на схеме выше). Не нужно настраивать руководителей или код приложения, который взаимодействует с ними (см. описание поля синий).

При создании нового экземпляра `UserManager` или `RoleManager` укажите тип класса пользователя и передайте экземпляр класса store в качестве аргумента. Такой подход позволяет подключить свои настраиваемые классы в ASP.NET Core. 

[Перенастройка приложения для использования нового поставщика хранилища](#reconfigure-app-to-use-a-new-storage-provider) показано, как создать экземпляр `UserManager` и `RoleManager` настраиваемого хранилища.

## <a name="aspnet-core-identity-stores-data-types"></a>ASP.NET Core Identity хранит типы данных

[ASP.NET Core Identity](https://github.com/aspnet/identity) типы данных описаны в следующих разделах:

### <a name="users"></a>Пользователи

Зарегистрированным пользователям вашего веб-сайта. [IdentityUser](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuser) типа может расширять или использовать в качестве примера для пользовательского типа. Не нужно наследовать от определенного типа для реализации собственного решения хранилища пользовательских удостоверений.

### <a name="user-claims"></a>Утверждения пользователей

Набор инструкций (или [утверждений](/dotnet/api/system.security.claims.claim)) о пользователе, которые представляют удостоверение пользователя. Можно включить большего выражения удостоверения пользователя, чем можно достичь с помощью ролей.

### <a name="user-logins"></a>Имена входа

Сведения о поставщике внешней проверки подлинности (например, Facebook или учетную запись Майкрософт) для использования при входе пользователя. [Пример](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuserlogin)

### <a name="roles"></a>Роли

Группы авторизации для веб-узла. Включает в себя имя роли идентификатора и роли (например, «Администратор» или «Employee»). [Пример](/dotnet/api/microsoft.aspnet.identity.corecompat.identityrole)

## <a name="the-data-access-layer"></a>Уровень доступа к данным

В этом разделе предполагается, что вы знакомы с механизм сохраняемости, который вы собираетесь использовать и как создать сущности для этого механизма. В этом разделе не содержат сведения о том, как создать репозитории или классы доступа к данным; он предоставляет ряд предложений о проектные решения, при работе с ASP.NET Core Identity.

У вас много свободы при разработке уровня доступа к данным для поставщика настраиваемого хранилища. Необходимо только создать механизмов сохраняемости для функций, которые предполагается использовать в приложении. Например если роли не используются в приложении, не нужно создать хранилище для ролей или сопоставления пользователей и роли. Технологии и инфраструктура может потребоваться структуру, которая существенно отличается от реализации по умолчанию ASP.NET Core Identity. В уровне доступа к данным необходимо предоставить логику для работы со структурой вашей реализации хранилища.

Уровень доступа к данным содержит логику для сохранения данных из удостоверения ASP.NET Core с источником данных. Уровень доступа к данным для поставщика настраиваемого хранилища может включать следующие классы для хранения сведений о пользователях и роли.

### <a name="context-class"></a>Context - класс

Инкапсулирует сведения для подключения к вашей механизм сохранения и выполнения запросов. Несколько классов данных требует запуска экземпляра этого класса обычно предоставляются с помощью внедрения зависимостей. [Пример](/dotnet/api/microsoft.aspnet.identity.corecompat.identitydbcontext-1).

### <a name="user-storage"></a>Хранилище пользователя

Хранит и извлекает сведения о пользователе (например, хэш имени и пароля пользователя). [Пример](/dotnet/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="role-storage"></a>Роль хранилища

Хранит и извлекает сведения о роли (например, имя роли). [Пример](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1)

### <a name="userclaims-storage"></a>Хранилище объектов Userclaim

Сохраняет и извлекает информацию об утверждении пользователя (например, тип утверждения и значение). [Пример](/dotnet/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userlogins-storage"></a>Хранилище объектов userlogin

Хранит и извлекает данные для входа пользователя (например, внешний поставщик аутентификации). [Пример](/dotnet/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userrole-storage"></a>Объем хранилища UserRole

Хранит и извлекает, какие роли назначаются для пользователей. [Пример](/dotnet/api/microsoft.aspnet.identity.corecompat.userstore-1)

**Совет:** реализуют только классы, которые планируется использовать в приложении.

В классы доступа к данным предоставить код для выполнения операций с данными для вашего механизма сохраняемости. Например, в пределах пользовательского поставщика, возможно, следующий код, чтобы создать нового пользователя в *хранения* класса:

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs?name=createuser&highlight=7)]

Реализация логики для создания пользователя находится в `_usersTable.CreateAsync` метод, показанный ниже.

## <a name="customize-the-user-class"></a>Настроить класс пользователя

При реализации поставщика хранилища, создайте класс пользователя, что эквивалентно [IdentityUser класс](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuser).

Как минимум, необходимо включить класса user `Id` и `UserName` свойство.

`IdentityUser` Класс определяет свойства, `UserManager` вызовов при выполнении запрошенных операций. Тип по умолчанию `Id` свойство содержит строку, но может наследовать от `IdentityUser<TKey, TUserClaim, TUserRole, TUserLogin, TUserToken>` и указать другой тип. Платформа ожидает от реализации хранилища для обработки преобразований типа данных.

## <a name="customize-the-user-store"></a>Настроить хранилище пользователя

Создание `UserStore` класс, предоставляющий методы для всех операций с данными пользователя. Этот класс эквивалентно [UserStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.userstore-1) класса. В вашей `UserStore` , следует реализовать `IUserStore<TUser>` и необязательные интерфейсы, необходимые. Выбирается какие дополнительные интерфейсы для реализации, в зависимости от функциональных возможностей, предоставляемых в вашем приложении.

### <a name="optional-interfaces"></a>Необязательные интерфейсы

* [IUserRoleStore](/dotnet/api/microsoft.aspnetcore.identity.iuserrolestore-1)
* [IUserClaimStore](/dotnet/api/microsoft.aspnetcore.identity.iuserclaimstore-1)
* [IUserPasswordStore](/dotnet/api/microsoft.aspnetcore.identity.iuserpasswordstore-1)
* [IUserSecurityStampStore](/dotnet/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1)
* [IUserEmailStore](/dotnet/api/microsoft.aspnetcore.identity.iuseremailstore-1)
* [IPhoneNumberStore](/dotnet/api/microsoft.aspnetcore.identity.iphonenumberstore-1)
* [IQueryableUserStore](/dotnet/api/microsoft.aspnetcore.identity.iqueryableuserstore-1)
* [IUserLoginStore](/dotnet/api/microsoft.aspnetcore.identity.iuserloginstore-1)
* [IUserTwoFactorStore](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactorstore-1)
* [IUserLockoutStore](/dotnet/api/microsoft.aspnetcore.identity.iuserlockoutstore-1)

Необязательные интерфейсы наследуются от класса `IUserStore<TUser>`. Вы увидите хранить в частично реализованные тестового пользователя [пример приложения](https://github.com/aspnet/Docs/blob/master/aspnetcore/security/authentication/identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs).

В рамках `UserStore` , использовать классы доступа к данным, которые созданы для выполнения операций. Они передаются с использованием внедрения зависимостей. Например, в SQL Server с помощью Dapper реализации `UserStore` класс имеет `CreateAsync` метод, который использует экземпляр `DapperUsersTable` для вставки новой записи:

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/DapperUsersTable.cs?name=createuser&highlight=7)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>Интерфейсы для реализации при настройке хранилища пользователя

- **IUserStore**  
 [IUserStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserstore-1) это единственный интерфейс, необходимо реализовать в хранилище пользователя. Он определяет методы для создания, обновление, удаление и извлечение пользователей.
- **IUserClaimStore**  
 [IUserClaimStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserclaimstore-1) интерфейс определяет методы, реализуемые для включения заявок на доступ пользователя. Он содержит методы для добавления, удаления и получает утверждения пользователя.
- **IUserLoginStore**  
 [IUserLoginStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserloginstore-1) определяет методы реализации для включения внешних поставщиков проверки подлинности. Он содержит методы для добавления, удаления и получения учетных данных для входа и метод для извлечения на основе сведений имени входа пользователя.
- **IUserRoleStore**  
 [IUserRoleStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserrolestore-1) интерфейс определяет методы реализации для сопоставления пользователей в роль. Он содержит методы для добавления, удаления и получения ролей пользователя и метод для проверки, если пользователю назначена роль.
- **IUserPasswordStore**  
 [IUserPasswordStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserpasswordstore-1) интерфейс определяет методы реализации для сохранения хэшированные пароли. Он содержит методы для получения и установки хэшированный пароль и метод, который указывает, ли пользователь задать пароль.
- **IUserSecurityStampStore**  
 [IUserSecurityStampStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1) интерфейс определяет методы, которые можно реализовать, чтобы использовать метку безопасности для, указывающее, изменилось ли сведения об учетной записи пользователя. Эта метка обновляется, когда пользователь изменяет пароль, или добавляет или удаляет имена входа. Он содержит методы для получения и установки метку безопасности.
- **IUserTwoFactorStore**  
 [IUserTwoFactorStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactorstore-1) интерфейс определяет методы реализации для поддержки двухфакторной проверки подлинности. Он содержит методы для получения и установки ли двухфакторная проверка подлинности включена для пользователя.
- **IUserPhoneNumberStore**  
 [IUserPhoneNumberStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserphonenumberstore-1) интерфейс определяет методы, реализуемые для хранения номеров телефона пользователя. Он содержит методы для получения и установки, номер телефона и подтвержден ли номер телефона.
- **IUserEmailStore**  
 [IUserEmailStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuseremailstore-1) интерфейс определяет методы реализации для сохранения адресов электронной почты пользователя. Он содержит методы для получения и задания, адрес электронной почты и подтвержден ли адрес электронной почты.
- **IUserLockoutStore**  
 [IUserLockoutStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserlockoutstore-1) интерфейс определяет методы реализации для хранения сведений о блокировке учетной записи. Он содержит методы для отслеживания неудачных попыток доступа и блокировки.
- **IQueryableUserStore**  
 [IQueryableUserStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iqueryableuserstore-1) интерфейс определяет членов, реализовать для создания хранилища поддерживает запросы пользователя.

Только интерфейсы, которые необходимы реализовать в приложении. Пример:

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

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a>IdentityUserClaim IdentityUserLogin и IdentityUserRole

`Microsoft.AspNet.Identity.EntityFramework` Пространство имен содержит реализации [IdentityUserClaim](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserclaim-1), [IdentityUserLogin](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuserlogin), и [IdentityUserRole](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserrole-1) классы. Если вы используете эти функции, можно создать собственные версии этих классов и определить свойства для вашего приложения. Тем не менее иногда бывает более эффективно не загружать эти сущности в памяти при выполнении основных операций (например, добавление или удаление утверждения пользователя). Вместо этого классы хранилища базы данных могут выполнять эти операции непосредственно на источнике данных. Например `UserStore.GetClaimsAsync` можно вызвать метод `userClaimTable.FindByUserId(user.Id)` метод для выполнения запроса на, который непосредственно таблицы и возвращает список утверждений.

## <a name="customize-the-role-class"></a>Настроить класс ролей

При реализации роли поставщика хранилища, можно создать тип настраиваемой роли. Он не требуется реализовывать определенный интерфейс, но он должен иметь `Id` обычно он будет иметь `Name` свойство.

Ниже приведен пример класса роли.

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/ApplicationRole.cs)]

## <a name="customize-the-role-store"></a>Настройка хранилища ролей

Можно создать `RoleStore` класс, предоставляющий методы для всех операций с данными для ролей. Этот класс эквивалентно [RoleStore&lt;TRole&gt; ](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1) класса. В `RoleStore` реализовать класс, `IRoleStore<TRole>` и при необходимости `IQueryableRoleStore<TRole>` интерфейс.

- **IRoleStore&lt;TRole&gt;**  
 [IRoleStore&lt;TRole&gt; ](/dotnet/api/microsoft.aspnetcore.identity.irolestore-1) интерфейс определяет методы для реализации в классе роли хранилища. Он содержит методы для создания, обновления, удаления и получение ролей.
- **RoleStore&lt;TRole&gt;**  
 Чтобы настроить `RoleStore`, создайте класс, реализующий `IRoleStore<TRole>` интерфейс. 

## <a name="reconfigure-app-to-use-a-new-storage-provider"></a>Перенастройка приложения для использования нового поставщика хранилища

После реализации поставщика хранилища, вы настроите вашего приложения для его использования. Если приложение используется поставщик по умолчанию, его необходимо замените пользовательского поставщика.

1. Удалить `Microsoft.AspNetCore.EntityFramework.Identity` пакет NuGet.
1. Если поставщик хранилища находится в отдельном проекте или пакете, добавьте ссылку на него.
1. Замените все вхождения `Microsoft.AspNetCore.EntityFramework.Identity` с с помощью инструкции для пространства имен поставщика хранилища.
1. В `ConfigureServices` метод, изменение `AddIdentity` метод пользовательских типов. Можно создать собственные методы расширения для этой цели. См. в разделе [IdentityServiceCollectionExtensions](https://github.com/aspnet/Identity/blob/rel/1.1.0/src/Microsoft.AspNetCore.Identity/IdentityServiceCollectionExtensions.cs) пример.
1. При использовании роли, обновите `RoleManager` для использования вашей `RoleStore` класса.
1. Обновите строку соединения и учетные данные для конфигурации приложения.

Пример

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

- [Пользовательские поставщики хранилищ для ASP.NET 4.x Identity](/aspnet/identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity)
- [ASP.NET Core Identity](https://github.com/aspnet/identity) -этот репозиторий содержит ссылки на сообществом поставщиков хранилища.
