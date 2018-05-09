---
title: Перенести из проверки подлинности членства в ASP.NET 2.0 удостоверению ASP.NET Core
author: isaac2004
description: Дополнительные сведения о миграции существующих приложений ASP.NET, с помощью проверки подлинности членства ASP.NET Core 2.0 Identity.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: f0d1099bfda01d036831350e0888ae3830ad3d58
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/08/2018
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a>Перенести из проверки подлинности членства в ASP.NET 2.0 удостоверению ASP.NET Core

Автор [Айзек Левин](https://isaaclevin.com) (Isaac Levin)

В этой статье демонстрируется миграция схемы базы данных для приложений ASP.NET, с помощью проверки подлинности членства ASP.NET Core 2.0 Identity.

> [!NOTE]
> Этот документ содержит шаги, необходимые для переноса схемы базы данных для приложения на основе членства в ASP.NET в схему базы данных, используемый для ASP.NET Core Identity. Дополнительные сведения о миграции с проверки подлинности на основе членства ASP.NET для ASP.NET Identity см. в разделе [провести миграцию существующего приложения с членства SQL в ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity). Дополнительные сведения об ASP.NET Core Identity см. в разделе [введение в ASP.NET Core удостоверения](xref:security/authentication/identity).

## <a name="review-of-membership-schema"></a>Проверка членства схемы

До ASP.NET 2.0 разработчики были заниматься созданием весь процесс проверки подлинности и авторизации для своих приложений. В ASP.NET 2.0 появилась членства, предоставления стандартного решения к обработке безопасности в приложениях ASP.NET. Теперь разработчики могли начальной загрузки схемы в базу данных SQL Server с [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) команды. После выполнения этой команды, следующие таблицы были созданы в базе данных.

  ![Таблицы членства](identity/_static/membership-tables.png)

Чтобы перенести существующие приложения ASP.NET Core 2.0 Identity, данные в этих таблицах необходимо перенести таблиц, используемых в новую схему удостоверений.

## <a name="aspnet-core-identity-20-schema"></a>Схема ASP.NET Core Identity 2.0

Соответствует ASP.NET Core 2.0 [удостоверение](/aspnet/identity/index) принцип, появившиеся в ASP.NET 4.5. Хотя общий принцип, реализация между платформами отличается, даже между версиями ASP.NET Core (см. [перенести проверку подлинности и удостоверение в ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).

Самый быстрый способ просмотра схемы для ASP.NET Core 2.0 Identity является создание нового приложения ASP.NET Core 2.0. Выполните следующие действия в Visual Studio 2017 г.

* Выберите **Файл** > **Создать** > **Проект**.
* Создайте новый **веб-приложения ASP.NET Core**и назовите проект *CoreIdentitySample*.
* Выберите в раскрывающемся списке **ASP.NET Core 2.0**, а затем **Веб-приложение**. Этот шаблон создает [страниц Razor](xref:mvc/razor-pages/index) приложения. Перед нажатием кнопки **ОК**, нажмите кнопку **изменить аутентификацию**.
* Выберите **отдельных учетных записей пользователей** для идентификаторов шаблонов. Наконец, нажмите кнопку **ОК**, затем **ОК**. Visual Studio создает проект с помощью шаблона ASP.NET Core Identity.

Использует удостоверения ASP.NET Core 2.0 [Entity Framework Core](/ef/core) для взаимодействия с базой данных, хранение данных проверки подлинности. Чтобы вновь созданного приложения для работы необходимо поддерживать базу данных для хранения этих данных. После создания нового приложения, то самый быстрый способ проверить схему в среде базы данных является создание базы данных, с помощью миграций Entity Framework. Этот процесс создает базу данных, либо локально или в других местах, который имитирует этой схемы. Предыдущий документации для получения дополнительной информации.

Чтобы создать базу данных со схемой ASP.NET Core Identity, запустите `Update-Database` в Visual Studio **консоль диспетчера пакетов** окна (PMC)&mdash;расположен по адресу **средства**  >  **Диспетчера пакетов NuGet** > **консоль диспетчера пакетов**. PMC поддерживает выполнение команды Entity Framework.

Команды Entity Framework использовать строку подключения для базы данных, указанной в *appsettings.json*. Следующая строка подключения связанного с базой данных на *localhost* с именем *asp net-core идентификаторов*. В этом параметре Entity Framework настроен на использование `DefaultConnection` строку подключения.

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
  }
}
```

Эта команда создает базы данных, указанной с помощью схемы и все данные, необходимые для инициализации приложения. На следующей иллюстрации изображена структура таблицы, создаваемой с описанные выше шаги.

   ![Удостоверение таблиц](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a>Перенос схемы

Существуют небольшие отличия в структуры таблиц и полей для членства и ASP.NET Core Identity. Шаблон отличается большей части для проверки подлинности и авторизации с приложениями ASP.NET и ASP.NET Core. Объекты ключей, по-прежнему используются с удостоверением, *пользователей* и *ролей*. Ниже приведены таблицы сопоставления для *пользователей*, *ролей*, и *UserRoles*.

### <a name="users"></a>Пользователи

| *Identity(AspNetUsers)* |   | *Membership(aspnet_Users/aspnet_Membership)* ||
| --- | --- | --- | --- | --- | --- |
| **Имя поля** | **Type**  |   **Имя поля** | **Type**  |
|`Id` | string | `aspnet_Users.UserId` | string
|`UserName` | string | `aspnet_Users.UserName` | string
|`Email` | string | `aspnet_Membership.Email` | string
|`NormalizedUserName` | string | `aspnet_Users.LoweredUserName` | string
|`NormalizedEmail` | string | `aspnet_Membership.LoweredEmail` | string
|`PhoneNumber` | string | `aspnet_Users.MobileAlias` | string
|`LockoutEnabled` | bit | `aspnet_Membership.IsLockedOut` | bit

> [!NOTE]
> Не все сопоставления полей напоминают отношения один к одному из членства в ASP.NET Core Identity. Приведенной выше таблице используется схема по умолчанию авторизованного пользователя и сопоставляет ее схемы ASP.NET Core Identity. Другие настраиваемые поля, которые были использованы для членства должны быть добавлены вручную. В этом сопоставлении файл сопоставления отсутствует пароли, как требования к паролям и соли пароль не проводить миграцию между двумя. **Рекомендуется оставить поле пароля как значения null и предлагать пользователям сбрасывать свои пароли.** В ASP.NET Identity Core `LockoutEnd` должно задаваться на некоторую дату в будущем, если пользователь заблокирован. Это показано в сценарии миграции.

### <a name="roles"></a>Роли

| *Identity(AspNetRoles)* |   | *Membership(aspnet_Roles)* ||
| --- | --- | --- | --- | --- | --- |
| **Имя поля** | **Type**  |   **Имя поля** | **Type**  |
|`Id` | string | `RoleId` | string
|`Name` | string | `RoleName` | string
|`NormalizedName` | string | `LoweredRoleName` | string

### <a name="user-roles"></a>Роли пользователей

| *Identity(AspNetUserRoles)* |   | *Membership(aspnet_UsersInRoles)* ||
| --- | --- | --- | --- | --- | --- |
| **Имя поля** | **Type**  |   **Имя поля** | **Type**  |
|`RoleId` | string | `RoleId` | string
|`UserId` | string | `UserId` | string

Ссылаться на предыдущем таблицы сопоставления при создании сценария миграции для *пользователей* и *ролей*. В следующем примере предполагается, что у вас есть две базы данных на сервере базы данных. Одна база данных содержит существующие членства ASP.NET схему и данные. Другие базы данных был создан с помощью действия, описанные ранее. Комментарии — это встроенные вместе для получения дополнительных сведений.

```sql
-- THIS SCRIPT NEEDS TO RUN FROM THE CONTEXT OF THE MEMBERSHIP DB
BEGIN TRANSACTION MigrateUsersAndRoles
use aspnetdb

-- INSERT USERS
INSERT INTO coreidentity.dbo.aspnetusers
            (id,
             username,
             normalizedusername,
             passwordhash,
             securitystamp,
             emailconfirmed,
             phonenumber,
             phonenumberconfirmed,
             twofactorenabled,
             lockoutend,
             lockoutenabled,
             accessfailedcount,
             email,
             normalizedemail)
SELECT aspnet_users.userid,
       aspnet_users.username,
       aspnet_users.loweredusername,
       --Creates an empty password since passwords don't map between the two schemas
       '',
       --Security Stamp is a token used to verify the state of an account and is subject to change at any time. It should be intialized as a new ID.
       NewID(),
       --EmailConfirmed is set when a new user is created and confirmed via email. Users must have this set during migration to ensure they're able to reset passwords.
       1,
       aspnet_users.mobilealias,
       CASE
         WHEN aspnet_Users.MobileAlias is null THEN 0
         ELSE 1
       END,
       --2-factor Auth likely wasn't setup in Membership for users, so setting as false.
       0,
       CASE
         --Setting lockout date to time in the future (1000 years)
         WHEN aspnet_membership.islockedout = 1 THEN Dateadd(year, 1000,
                                                     Sysutcdatetime())
         ELSE NULL
       END,
       aspnet_membership.islockedout,
       --AccessFailedAccount is used to track failed logins. This is stored in membership in multiple columns. Setting to 0 arbitrarily.
       0,
       aspnet_membership.email,
       aspnet_membership.loweredemail
FROM   aspnet_users
       LEFT OUTER JOIN aspnet_membership
                    ON aspnet_membership.applicationid =
                       aspnet_users.applicationid
                       AND aspnet_users.userid = aspnet_membership.userid
       LEFT OUTER JOIN coreidentity.dbo.aspnetusers
                    ON aspnet_membership.userid = aspnetusers.id
WHERE  aspnetusers.id IS NULL

-- INSERT ROLES
INSERT INTO coreIdentity.dbo.aspnetroles(id,name)
SELECT roleId,rolename
FROM aspnet_roles;

-- INSERT USER ROLES
INSERT INTO coreidentity.dbo.aspnetuserroles(userid,roleid)
SELECT userid,roleid
FROM aspnet_usersinroles;

IF @@ERROR <> 0
  BEGIN
    ROLLBACK TRANSACTION MigrateUsersAndRoles
    RETURN
  END

COMMIT TRANSACTION MigrateUsersAndRoles
```

После выполнения этого скрипта ASP.NET Core Identity приложения, созданного ранее заполняется авторизованных пользователей. Пользователи должны изменить свой пароль для входа.

> [!NOTE]
> Если система членства пользователей с именами пользователей, которые не соответствуют свой адрес электронной почты, требуются изменения в приложении, созданном ранее, чтобы решить эту проблему. Шаблон по умолчанию ожидает `UserName` и `Email` должны совпадать. В ситуациях, в которых они отличаются, процесс входа в систему необходимо изменить для использования `UserName` вместо `Email`.

В `PageModel` страницы входа, расположенный в *Pages\Account\Login.cshtml.cs*, удалите `[EmailAddress]` атрибута из *электронной почты* свойство. Переименуйте его в *UserName*. Это требует изменения везде, где `EmailAddress` упоминается, в *представление* и *PageModel*. Результат выглядит следующим образом:

 ![Фиксированный входа](identity/_static/fixed-login.png)

## <a name="next-steps"></a>Следующие шаги

В этом учебнике вы узнали, как перенести пользователей членства SQL с ASP.NET Core 2.0 Identity. Дополнительные сведения о ASP.NET Core Identity см. в разделе [Общие сведения об идентификации](xref:security/authentication/identity).
