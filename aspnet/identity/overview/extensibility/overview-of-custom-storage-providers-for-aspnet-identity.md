---
uid: identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
title: "Обзор поставщиков пользовательского хранилища для ASP.NET Identity | Документы Microsoft"
author: tfitzmac
description: "ASP.NET Identity является расширяемой системой, что дает возможность создать собственный поставщик хранилища и вставьте его в приложение без переработки приложения..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2014
ms.topic: article
ms.assetid: 681a9204-462e-4260-9a0b-19f0644d6ad7
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: bbc1f6ef291eddd7488531943b146bb67ae7ee02
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/12/2018
---
<a name="overview-of-custom-storage-providers-for-aspnet-identity"></a>Обзор поставщиков пользовательского хранилища для ASP.NET Identity
====================
по [Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET Identity является расширяемой системой, что дает возможность создать собственный поставщик хранилища и вставьте его в приложение без переработки приложения. В этом разделе описывается создание поставщика хранилища, настроенные для ASP.NET Identity. Она охватывает важные понятия для создания собственного поставщика хранилища, но это не пошаговое руководство по реализации поставщика пользовательского хранилища.
> 
> Пример реализации поставщика пользовательского хранилища см. в разделе [реализации пользовательского поставщика хранилища удостоверений ASP.NET MySQL](implementing-a-custom-mysql-aspnet-identity-storage-provider.md).
> 
> В этом разделе был обновлен для удостоверения ASP.NET 2.0.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемая в этом учебнике
> 
> 
> - Visual Studio 2013 с обновлением 2
> - Удостоверение ASP.NET 2


## <a name="introduction"></a>Вступление

По умолчанию система идентификации ASP.NET хранит сведения о пользователе в базе данных SQL Server и использует Entity Framework Code First для создания базы данных. Для многих приложений этот способ работает хорошо. Тем не менее можно использовать другой тип механизм сохраняемости, например табличного хранилища Azure, или у вас уже таблиц базы данных со структурой сильно отличается от реализации по умолчанию. В любом случае можно указать настраиваемый поставщик для вашего механизм хранения и подключить этот поставщик в приложение.

Удостоверение ASP.NET включается по умолчанию во многих шаблонов Visual Studio 2013. Можно получить обновления для ASP.NET Identity через [пакет EntityFramework NuGet удостоверений Microsoft AspNet](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/).

Этот раздел включает следующие подразделы:

- [Описание архитектуры](#architecture)
- [Понять данные, которые хранятся](#data)
- [Создание уровня доступа к данным](#dal)
- [Настроить класс пользователя](#user)
- [Настроить хранилище пользователей](#userstore)
- [Настроить класс ролей](#role)
- [Настройка хранилища ролей](#rolestore)
- [Перенастройка приложения для использования нового поставщика хранилища](#reconfigure)
- [Другие реализации поставщиков пользовательского хранилища](#other)

<a id="architecture"></a>
## <a name="understand-the-architecture"></a>Описание архитектуры

ASP.NET Identity состоит из класса с именами менеджеров и хранилища. Диспетчеры — общие классы, которые разработчик приложения использует для выполнения операций, таких как создание пользователей, в системе ASP.NET Identity. Магазины являются более низкого уровня классы, определяющие, каким образом сущности, такие как пользователи и роли, сохраняются. Хранилищ тесно связана с механизм сохранения, но диспетчеры связаны с хранилищ это означает, что механизм сохранения можно заменить без нарушения всего приложения.

На следующей схеме показано, как веб-приложение взаимодействует с менеджеров и хранилищ взаимодействия с уровня доступа к данным.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image1.png)

Создание поставщика хранилища, настроенные для ASP.NET Identity, необходимо создать источник данных, уровень доступа к данным и хранилища классы, которые взаимодействуют с этот уровень доступа к данным. Можно продолжить, используя один и тот же диспетчер API-интерфейсы для выполнения операций с данными на пользователя, но теперь, когда данные сохраняются в другой системе.

Необязательно настраивать классы manager, так как при создании нового экземпляра UserManager или RoleManager вы предоставляете тип пользовательского класса, и передать экземпляр класса хранилище в качестве аргумента. Такой подход позволяет подключаются к существующей структуре настраиваемые классы. Вы увидите, как создать экземпляр UserManager и RoleManager с классами вашего настроенного хранилища в разделе [перенастроить приложение для использования нового поставщика хранилища](#reconfigure).

<a id="data"></a>
## <a name="understand-the-data-that-is-stored"></a>Понять данные, которые хранятся

Для реализации поставщика пользовательского хранилища, необходимо понимать типы данных при использовании ASP.NET Identity и решить, какие компоненты, необходимые для вашего приложения.

| Данные | Описание: |
| --- | --- |
| Пользователи | Зарегистрированным пользователям вашего веб-сайта. Включает в себя пользователя идентификатор и имя пользователя. Может включать хэшированный пароль, если пользователи будут входить с использованием учетных данных, которые относятся к веб-узла (а не с помощью учетных данных с внешних сайтов, таких как Facebook) и метку безопасности для указания ли что-либо изменения в учетных данных пользователя. Может также включать адрес электронной почты, телефонный номер, является ли двухфакторная проверка подлинности включена, текущее число неудачных попыток входа, и является ли учетная запись заблокирована. |
| Утверждения пользователей | Набор инструкций (или утверждения) о пользователе, которые представляют удостоверение пользователя. Можно включить большего выражения удостоверение пользователя, чем можно создать с помощью ролей. |
| Имена входа | Сведения о поставщике внешней проверки подлинности (например, Facebook) для использования при входе пользователя. |
| Роли | Группы авторизации для веб-узла. Содержит имя роли идентификатор и роли (например, «Admin» или «Employee»). |

<a id="dal"></a>
## <a name="create-the-data-access-layer"></a>Создание уровня доступа к данным

В этом разделе предполагается, что вы знакомы с механизм сохраняемости, который будет использоваться и как создать сущности для этого механизма. В этом разделе приводятся подробные сведения о создании репозитории или классов доступа к данным; Вместо этого он предоставляет некоторые рекомендации о проектные решения, которые нужно учитывать при работе с ASP.NET Identity.

У вас много свободу при проектировании хранилищ для настраиваемый поставщик хранилища. Необходимо создавать репозитории для компонентов, которые будут использоваться в приложении. Например если в приложении не используются роли, необходимо создание хранилища для роли или пользователя. Технологии и существующей инфраструктуры может потребоваться структуру, сильно отличается от реализации по умолчанию для ASP.NET Identity. В уровне доступа к данным предоставляют логику для работы со структурой свои хранилища.

Для реализации MySQL, репозиториев данные для удостоверения ASP.NET 2.0, см. [MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql).

На уровне доступа к данным предоставляют логику для сохранения данных из ASP.NET Identity для источника данных. Уровень доступа к данным для поставщика настраиваемого хранилища может включать следующие классы для хранения сведений о пользователе и роли.

| Класс | Описание: | Пример |
| --- | --- | --- |
| Контекст | Инкапсулирует сведения для подключения к вашей механизм сохранения и выполнения запросов. Этот класс является центральным элементом слоя доступа к данным. Классы данных потребуется экземпляр этого класса для выполнения своих операций. Также будет инициализировать хранилище классов с экземпляром этого класса. | [MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) |
| Хранилище пользователя | Хранит и извлекает сведения о пользователе (например, хэш имени и пароля пользователя). | [UserTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) |
| Роль хранилища | Хранит и извлекает сведения о роли (например, имя роли). | [RoleTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) |
| Хранилище объектов Userclaim | Сохраняет и извлекает информацию об утверждении пользователя (например, тип утверждения и значение). | [UserClaimsTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) |
| Хранилище объектов userlogin | Хранит и извлекает сведения об имени входа пользователя (например, внешний поставщик аутентификации). | [UserLoginsTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) |
| Объем хранилища UserRole | Хранит и извлекает пользователь назначен роли. | [UserRoleTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) |

Опять же необходимо только реализовать классы, которые будут использоваться в приложении.

В классы доступа к данным необходимо предоставить код для выполнения операций с данными для вашего конкретного сохраняемости механизма. Например в реализации MySQL UserTable класс содержит метод для вставки новой записи в таблице базы данных пользователей. Переменная с именем `_database` является экземпляром класса MySQLDatabase.

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample1.cs)]

После создания классов данных access, необходимо создать классы хранилища, вызывающие методы, определенные на уровне доступа к данным.

<a id="user"></a>
## <a name="customize-the-user-class"></a>Настроить класс пользователя

При реализации поставщика хранилища, необходимо создать класс пользователя, который эквивалентен [IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser(v=vs.108).aspx) класса в [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) пространство имен:

В примере ниже показан класс IdentityUser, необходимо создать и интерфейс для реализации этого класса.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image2.png)

[IUser&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx) интерфейс определяет свойства, которые пытается вызвать при выполнении запроса операции UserManager. Интерфейс содержит два свойства - идентификатор и имя пользователя. [IUser&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx) интерфейс позволяет указывать тип ключа для пользователя с помощью универсального **TKey** параметра. Тип идентификатора свойства совпадает со значением параметра TKey.

Также предоставляет платформу удостоверений [IUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.iuser(v=vs.108).aspx) интерфейса (без параметром универсального типа) при необходимости использовать строковое значение для ключа.

Класс IdentityUser реализует IUser и содержит дополнительные свойства или конструкторы для пользователей на веб-сайте. Пример класса IdentityUser, который использует целое число для ключа. Поле Id равно **int** в соответствии со значением универсального параметра. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample2.cs)]

 Для полной реализации, в разделе [IdentityUser (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs). 

<a id="userstore"></a>
## <a name="customize-the-user-store"></a>Настроить хранилище пользователей

Можно также создать UserStore класс, предоставляющий методы для всех операций с данными пользователя. Этот класс является эквивалентом [UserStore&lt;TUser&gt; ](https://msdn.microsoft.com/library/dn315446(v=vs.108).aspx) класса в [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) пространства имен. В классе UserStore реализовать [IUserStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx) и любые дополнительные интерфейсы. Выберите необязательные интерфейсы для реализации на основе функциональности, которую вы хотите предоставить в приложении.

На следующем рисунке UserStore класса, который необходимо создать и соответствующие интерфейсы.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image3.png)

Шаблон проекта по умолчанию в Visual Studio содержит код, который предполагает, что многие дополнительные интерфейсы, реализованные в хранилище пользователя. При использовании шаблона по умолчанию с хранилищем пользовательских реализовывать дополнительные интерфейсы в хранилище пользователя или изменить шаблон код больше не вызывать методы в интерфейсах, которые не реализована.

 Пример простого пользовательского класса хранилища. **TUser** универсальный параметр принимает тип пользовательского класса, который обычно является классом IdentityUser определенный. **TKey** универсальный параметр принимает тип ключа пользователя. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample3.cs)]

 В этом примере конструктор, который принимает параметр с именем *базы данных* типа ExampleDatabase является только показано, как передать в классе доступа к данным. Например в реализации MySQL этот конструктор использует параметр типа MySQLDatabase. 

В классе UserStore используется классов доступа к данным, которые созданы для выполнения операций. Например в реализации MySQL класс UserStore имеет метод CreateAsync, который использует экземпляр UserTable для вставки новой записи. **Вставить** метод **userTable** объект является тот же метод, который был показан в предыдущем разделе. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample4.cs)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>Интерфейсы для реализации при настройке хранилища пользователя

На следующем рисунке показана Дополнительные сведения о функциях, определенных в каждый интерфейс. Необязательные интерфейсы наследуются от IUserStore.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image4.png)

- **IUserStore**  
 [IUserStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613278(v=vs.108).aspx) интерфейс — только интерфейс, необходимо реализовать в хранилище пользователя. Он определяет методы для создания, обновления, удаления и получения пользователей.
- **IUserClaimStore**  
 [IUserClaimStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613265(v=vs.108).aspx) интерфейс определяет методы, необходимо реализовать в хранилище пользователя для включения заявок на доступ пользователя. Он содержит методы или добавления, удаления и получает утверждения пользователя.
- **IUserLoginStore**  
 [IUserLoginStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613272(v=vs.108).aspx) определяет методы, необходимо реализовать в хранилище пользователя для включения внешних поставщиков аутентификации. Содержит методы для добавления, удаления и извлечения имен входа пользователей и метод для извлечения на основе сведений имени входа пользователя.
- **IUserRoleStore**  
 [IUserRoleStore&lt;TKey, TUser&gt; ](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx) интерфейс определяет методы, необходимо реализовать в хранилище пользователя для сопоставления пользователей в роль. Содержит методы для добавления, удаления и извлечения роли пользователя, а также метод для проверки, если пользователь назначен роли.
- **IUserPasswordStore**  
 [IUserPasswordStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613273(v=vs.108).aspx) интерфейса определяются методы, необходимо реализовать в хранилище пользователя для сохранения паролей на основании хэша. Содержит методы для получения и задания хэшированный пароль и метод, который указывает, ли пользователь использовать пароль.
- **IUserSecurityStampStore**  
 [IUserSecurityStampStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613277(v=vs.108).aspx) интерфейса определяются методы, которые необходимо реализовать в хранилище пользователя для использования метку безопасности, указывающее, изменилось ли учетная запись пользователя . Это метка обновляется, когда пользователь изменяет пароль, или добавляет или удаляет имена входа. Содержит методы для получения и задания метку безопасности.
- **IUserTwoFactorStore**  
 [IUserTwoFactorStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613279(v=vs.108).aspx) интерфейса определяются методы, необходимо реализовать для реализации двухфакторная проверка подлинности. Содержит методы для получения и задания двухфакторная проверка подлинности включена ли для пользователя.
- **IUserPhoneNumberStore**  
 [IUserPhoneNumberStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613275(v=vs.108).aspx) интерфейса определяются методы, необходимо реализовать для хранения номеров телефонов пользователя. Содержит методы для получения и задания, номер телефона и подтверждается ли номер телефона.
- **IUserEmailStore**  
 [IUserEmailStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613143(v=vs.108).aspx) интерфейса определяются методы, необходимо реализовать для хранения адреса электронной почты пользователя. Содержит методы для получения и задания адреса электронной почты и подтверждается ли сообщение электронной почты.
- **IUserLockoutStore**  
 [IUserLockoutStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613271(v=vs.108).aspx) интерфейса определяются методы, необходимо реализовать для хранения сведений о блокировке учетной записи. Он содержит методы для получения текущее количество неудачных попыток доступа, получение и задание ли учетной записи могут быть заблокированы, получение и задание блокировка дату окончания, увеличивая число неудачных попыток и сброс числа неудачных попыток входа.
- **IQueryableUserStore**  
 [IQueryableUserStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613267(v=vs.108).aspx) интерфейс определяет элементы, необходимо реализовать для предоставления хранилища поддерживает запросы пользователя. Он содержит свойство, которое содержит поддерживает запросы пользователей.

 Реализуйте интерфейсы, которые необходимы в коде приложения; Например IUserClaimStore, IUserLoginStore, IUserRoleStore, IUserPasswordStore и IUserSecurityStampStore интерфейсы, как показано ниже. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample5.cs)]

Для полной реализации (включая все интерфейсы), в разделе [UserStore (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs).

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a>IdentityUserClaim, IdentityUserLogin и IdentityUserRole

Пространство имен Microsoft.AspNet.Identity.EntityFramework содержит реализации [IdentityUserClaim](https://msdn.microsoft.com/library/dn613250(v=vs.108).aspx), [IdentityUserLogin](https://msdn.microsoft.com/library/dn613251(v=vs.108).aspx), и [IdentityUserRole](https://msdn.microsoft.com/library/dn613252(v=vs.108).aspx) классы. Если вы используете эти функции, можно создавать собственные версии этих классов и определять свойства для вашего приложения. Тем не менее иногда бывает более эффективно не загружать эти сущности в память при выполнение основных операций (например, добавляя или удаляя утверждения пользователя). Вместо этого внутреннего хранилища классы могут выполнять эти операции непосредственно в источнике данных. Например метод UserStore.GetClaimsAsync() можно вызвать метод userClaimTable.FindByUserId(user. Метод ID) для выполнения запроса на который непосредственно таблицу и возвращает список утверждений.

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample6.cs)]

<a id="role"></a>
## <a name="customize-the-role-class"></a>Настроить класс ролей

При реализации поставщика хранилища, необходимо создать класс ролей, что эквивалентно [IdentityRole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityrole(v=vs.108).aspx) класса в [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) пространство имен:

В примере ниже показан класс IdentityRole, необходимо создать и интерфейс для реализации этого класса.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image5.png)

[IRole&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx) интерфейс определяет свойства, которые RoleManager пытается вызвать при выполнении запроса операции. Интерфейс содержит два свойства - идентификатор и имя. [IRole&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx) интерфейс позволяет указать тип ключа для роли через универсальный **TKey** параметра. Тип идентификатора свойства совпадает со значением параметра TKey.

Также предоставляет платформу удостоверений [IRole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.irole(v=vs.108).aspx) интерфейса (без параметром универсального типа) при необходимости использовать строковое значение для ключа.

Пример класса IdentityRole, который использует целое число для ключа. Поле «Идентификатор» имеет значение int в соответствии со значением универсального параметра. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample7.cs)]

 Для полной реализации, в разделе [IdentityRole (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs). 

<a id="rolestore"></a>
## <a name="customize-the-role-store"></a>Настройка хранилища ролей

Можно также создать RoleStore класс, предоставляющий методы для всех операций с данными на ролях. Этот класс является эквивалентом [RoleStore&lt;TRole&gt; ](https://msdn.microsoft.com/library/dn468181(v=vs.108).aspx) класса в пространстве имен Microsoft.ASP.NET.Identity.EntityFramework. В классе RoleStore реализовать [IRoleStore&lt;TRole, TKey&gt; ](https://msdn.microsoft.com/library/dn613266(v=vs.108).aspx) и при необходимости [IQueryableRoleStore&lt;TRole, TKey&gt; ](https://msdn.microsoft.com/library/dn613262(v=vs.108).aspx) интерфейс.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image6.png)

В следующем примере показан класс хранилища роли. Универсальный параметр TRole принимает тип класса роли, который обычно является определенным вами классом IdentityRole. Универсальный параметр TKey принимает тип ключа роли. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample8.cs)]

- **IRoleStore&lt;TRole&gt;**  
 [IRoleStore](https://msdn.microsoft.com/library/dn468195.aspx) интерфейс определяет методы для реализации в классе роли хранилища. Содержит методы для создания, обновления, удаления и получения ролей.
- **RoleStore&lt;TRole&gt;**  
 Чтобы настроить RoleStore, создайте класс, реализующий интерфейс IRoleStore. Имеется только для реализации этого класса, если хотите использовать ролей в вашей системе. Конструктор, который принимает параметр с именем *базы данных* типа ExampleDatabase является только показано, как передать в классе доступа к данным. Например в реализации MySQL этот конструктор использует параметр типа MySQLDatabase.  
  
 Для полной реализации, в разделе [RoleStore (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs) .

<a id="reconfigure"></a>
## <a name="reconfigure-application-to-use-new-storage-provider"></a>Перенастройка приложения для использования нового поставщика хранилища

Реализован новый поставщик хранилища. Теперь необходимо настроить приложение для использования этого поставщика хранилища. Если поставщик хранилища по умолчанию включены в проект, необходимо удалить поставщик по умолчанию и заменить ее с помощью поставщика.

### <a name="replace-default-storage-provider-in-mvc-project"></a>Замените поставщик хранилища по умолчанию в проект MVC

1. В **управление пакетами NuGet** окно, удалите **Microsoft ASP.NET Identity EntityFramework** пакета. Этот пакет можно найти путем поиска в установленные пакеты Identity.EntityFramework.  
    ![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image7.png)Требуется указывать также следует ли удалить Entity Framework. Если в других частях приложения оно не требуется, можно удалить ее.
2. В файле IdentityModels.cs в папке «Models», удалите или закомментируйте **ApplicationUser** и **ApplicationDbContext** классы. В MVC-приложении можно удалить весь файл IdentityModels.cs. В приложении Web Forms удаляется два класса, но убедитесь, что обеспечили вспомогательный класс, который также находится в файле IdentityModels.cs.
3. Если поставщик хранилища находится в отдельном проекте, добавьте ссылку на него в веб-приложении.
4. Замените все ссылки на `using Microsoft.AspNet.Identity.EntityFramework;` с с помощью инструкции для пространства имен поставщика хранилища.
5. В **Startup.Auth.cs** класса, измените **ConfigureAuth** метод для использования одного экземпляра соответствующий контекст. 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample9.cs?highlight=3)]
6. В приложении\_Начальная папка откройте **IdentityConfig.cs**. В классе ApplicationUserManager изменить **создать** метод для возврата диспетчера пользователей, который использует хранилище пользовательских. 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample10.cs?highlight=3)]
7. Замените все ссылки на **ApplicationUser** с **IdentityUser**.
8. Проект по умолчанию включает некоторые члены в класс пользователей, не определенные в интерфейсе IUser; Например, электронной почты, PasswordHash и GenerateUserIdentityAsync. Если класс user нет этих членов, реализовывать их или изменить код, который использует эти элементы.
9. Этот код для использования нового класса RoleStore измените при создании RoleManager все экземпляры.  

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample11.cs)]
10. Проект по умолчанию предназначен для пользовательского класса, который содержит строковое значение для ключа. Если класс user имеет другой тип ключа (например, целое число), необходимо изменить проект для работы с типом. В разделе [изменить первичный ключ для пользователей в ASP.NET Identity](change-primary-key-for-users-in-aspnet-identity.md).
11. При необходимости, добавьте строку подключения в файле Web.config.

<a id="other"></a>
## <a name="other-resources"></a>Другие источники

- Блог: [реализации ASP.NET Identity](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- Учебник и GIT кода: [поставщика удостоверений Simple.Data Asp.Net](http://designcoderelease.blogspot.co.uk/2015/03/simpledata-aspnet-identity-provider.html)
- Учебник:[установить основные учетные записи удостоверения и обращены внешних DB](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx). By [@xivSolutions](https://twitter.com/xivSolutions).
- Учебник по[: реализация поставщика хранилища ASP.NET Identity пользовательских MySQL](implementing-a-custom-mysql-aspnet-identity-storage-provider.md)
- [Сущности CodeFluent](http://blog.codefluententities.com/2014/04/30/asp-net-identity-v2-and-codefluent-entities/) по [SoftFluent](http://www.softfluent.com/)
- [Хранилище таблиц Azure](https://www.nuget.org/packages/accidentalfish.aspnet.identity.azure/) , Джеймс Randall @@.
- Хранилище таблиц Azure: [AspNet.Identity.TableStorage](https://github.com/stuartleeks/leeksnet.AspNet.Identity.TableStorage) по [ @stuartleeks ](https://twitter.com/stuartleeks).
- [CouchDB / Cloudant по Wertheim Дэниэла.](https://github.com/danielwertheim/mycouch.aspnet.identity)
- Эластичный поис[h: эластичной удостоверения](https://github.com/bmbsqd/elastic-identity) по Bombsquad AB.
- [MongoDB](http://www.nuget.org/packages/MongoDB.AspNet.Identity/) , Джонатан Sheely Джонатан Sheely.
- [NHibernate.AspNet.Identity](https://github.com/milesibastos/NHibernate.AspNet.Identity) по Antônio Milesi Bastos.
- [RavenDB](http://www.nuget.org/packages/AspNet.Identity.RavenDB/1.0.0) по [ @tourismgeek ](https://twitter.com/tourismgeek).
- [RavenDB.AspNet.Identity](https://github.com/ILMServices/RavenDB.AspNet.Identity) по [ILMServices](http://www.ilmservice.com/).
- Redis: [Redis.AspNet.Identity](https://github.com/aminjam/Redis.AspNet.Identity)
- Шаблоны T4 для формирования EF кода для пользовательского хранилища «database сначала»: [AspNet.Identity.EntityFramework](https://github.com/cbfrank/AspNet.Identity.EntityFramework)
