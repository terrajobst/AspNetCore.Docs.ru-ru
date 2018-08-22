---
uid: identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
title: Общие сведения о пользовательских поставщиков хранилищ для ASP.NET Identity | Документация Майкрософт
author: tfitzmac
description: ASP.NET Identity является расширяемой системой, что дает возможность создать собственный поставщик хранилища и подключить его к приложению без переработки приложения...
ms.author: riande
ms.date: 10/13/2014
ms.assetid: 681a9204-462e-4260-9a0b-19f0644d6ad7
msc.legacyurl: /identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: c92084265ff821bcec25244195a3511b71714836
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837328"
---
<a name="overview-of-custom-storage-providers-for-aspnet-identity"></a>Общие сведения о пользовательских поставщиков хранилищ для ASP.NET Identity
====================
по [Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET Identity — это расширяемая система, что дает возможность создать собственный поставщик хранилища и подключить его к приложению без переработки приложения. В этом разделе описывается создание поставщика настраиваемого хранилища для ASP.NET Identity. Он содержит важные основные понятия для создания собственного поставщика хранилища, но это не пошаговое руководство по реализации поставщика пользовательского хранилища.
> 
> Пример реализации поставщика пользовательского хранилища, см. в разделе [реализации настраиваемого поставщика хранилища удостоверений ASP.NET MySQL](implementing-a-custom-mysql-aspnet-identity-storage-provider.md).
> 
> Этот раздел был обновлен для удостоверения ASP.NET 2.0.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемые в этом руководстве
> 
> 
> - Visual Studio 2013 с обновлением 2
> - ASP.NET Identity 2


## <a name="introduction"></a>Вступление

По умолчанию системы удостоверений ASP.NET хранит сведения о пользователе в базе данных SQL Server и использует Entity Framework Code First для создания базы данных. Для многих приложений этот способ работает хорошо. Тем не менее вы можете использовать другой тип механизма сохраняемости, например хранилище таблиц Azure, или у вас уже таблиц базы данных со структурой, сильно отличается от реализации по умолчанию. В любом случае можно написать настраиваемый поставщик для вашего механизма хранения и подключить этот поставщик в приложение.

Удостоверение ASP.NET включается по умолчанию во многих из шаблонов Visual Studio 2013. Вы можете получать обновления в ASP.NET Identity через [пакета EntityFramework NuGet удостоверений Microsoft AspNet](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/).

Этот раздел включает следующие подразделы:

- [Общие сведения об архитектуре](#architecture)
- [Интерпретация данных, которые хранятся](#data)
- [Создание уровня доступа к данным](#dal)
- [Настроить класс пользователя](#user)
- [Настроить хранилище пользователя](#userstore)
- [Настроить класс ролей](#role)
- [Настройка хранилища ролей](#rolestore)
- [Перенастройка приложения для использования нового поставщика хранилища](#reconfigure)
- [Другие реализации пользовательских поставщиков хранилищ](#other)

<a id="architecture"></a>
## <a name="understand-the-architecture"></a>Общие сведения об архитектуре

ASP.NET Identity состоит из классов, называемых диспетчеры и хранилищами. Высокоуровневые классы, которые разработчик приложения использует для выполнения операций, таких как создание пользователя в систему ASP.NET Identity используются такие диспетчеры. Магазины являются классы более низкого уровня, которые указывают, как сущности, такие как пользователи и роли, сохраняются. Магазины являются тесно связан с механизмом сохраняемости, но диспетчеры отделены от хранилища, то есть вы можете заменить механизм сохранения без нарушения работы всего приложения.

На следующей схеме показано, как веб-приложения взаимодействует с диспетчерами и хранилищ взаимодействовать с уровня доступа к данным.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image1.png)

Для создания поставщика хранилища, настроенного для ASP.NET Identity, необходимо создать источник данных, уровень доступа к данным и хранилища классы, которые взаимодействуют с этот уровень доступа к данным. Вы можете продолжать, использовать один и тот же диспетчер API-интерфейсы для выполнения операций с данными пользователя, но теперь, когда данные сохраняются в другой системе.

Не нужно настраивать классы manager, так как при создании нового экземпляра UserManager или RoleManager его указать тип класса пользователя и передайте экземпляр класса store в качестве аргумента. Такой подход позволяет подключить свои настраиваемые классы в существующую структуру. Вы увидите, как создать экземпляр UserManager и RoleManager с классов настраиваемого хранилища в разделе [перенастроить приложение для использования нового поставщика хранилища](#reconfigure).

<a id="data"></a>
## <a name="understand-the-data-that-is-stored"></a>Интерпретация данных, которые хранятся

Для реализации поставщика пользовательского хранилища, необходимо понять типы данных, используемых в ASP.NET Identity и решить, какие из функций, необходимые для вашего приложения.

| Данные | Описание: |
| --- | --- |
| Пользователи | Зарегистрированным пользователям вашего веб-сайта. Включает в себя имя пользователя идентификатор и пользователя. Может включать хэшированный пароль, если пользователи входят с помощью учетных данных, относящихся к веб-узла (а не с помощью учетных данных с внешних сайтов, таких как Facebook) и метка безопасности, чтобы указать, изменилось ли что-либо учетные данные пользователя. Может также включать адрес электронной почты, номер телефона, отвечает за включение двухфакторной проверки подлинности, текущее количество неудачных попыток входа, и является ли учетная запись заблокирована. |
| Утверждения пользователей | Набор инструкций (или утверждения) о пользователе, которые представляют удостоверение пользователя. Можно включить большего выражения удостоверения пользователя, чем можно достичь с помощью ролей. |
| Имена входа | Сведения о поставщике внешней проверки подлинности (например, Facebook) для использования при входе пользователя. |
| Роли | Группы авторизации для веб-узла. Включает в себя имя роли идентификатора и роли (например, «Администратор» или «Employee»). |

<a id="dal"></a>
## <a name="create-the-data-access-layer"></a>Создание уровня доступа к данным

В этом разделе предполагается, что вы знакомы с механизм сохраняемости, который вы собираетесь использовать и как создать сущности для этого механизма. В этом разделе представлены сведения о том, как создать репозитории или классы доступа к данным; Вместо этого он предоставляет ряд предложений о проектные решения, что необходимо сделать при работе с ASP.NET Identity.

У вас есть много свободу при разработке в репозитории для настраиваемый поставщик хранилища. Необходимо только создавать репозитории для функций, которые предполагается использовать в приложении. Например если вы не используете ролей в приложении, вы не обязательно для создания хранилища для роли или пользователя. Технологии и инфраструктура может потребоваться структуру, которая существенно отличается от реализации по умолчанию ASP.NET Identity. В уровне доступа к данным необходимо предоставить логику для работы со структурой ваших репозиториев.

Для реализации MySQL репозиториев данных для удостоверений ASP.NET 2.0, см. в разделе [MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql).

В уровень доступа к данным необходимо предоставить логику для сохранения данных из удостоверения ASP.NET к источнику данных. Уровень доступа к данным для поставщика настраиваемого хранилища может включать следующие классы для хранения сведений о пользователях и роли.

| Класс | Описание: | Пример |
| --- | --- | --- |
| Контекст | Инкапсулирует сведения для подключения к вашей механизм сохранения и выполнения запросов. Этот класс является центральным элементом слоя доступа к данным. Другие классы данных потребует экземпляр этого класса для выполнения своих операций. Также, будет инициализировать классы хранилища с экземпляром этого класса. | [MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) |
| Хранилище пользователя | Хранит и извлекает сведения о пользователе (например, хэш имени и пароля пользователя). | [UserTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) |
| Роль хранилища | Хранит и извлекает сведения о роли (например, имя роли). | [RoleTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) |
| Хранилище объектов Userclaim | Сохраняет и извлекает информацию об утверждении пользователя (например, тип утверждения и значение). | [UserClaimsTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) |
| Хранилище объектов userlogin | Хранит и извлекает данные для входа пользователя (например, внешний поставщик аутентификации). | [UserLoginsTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) |
| Объем хранилища UserRole | Хранит и извлекает какие роли, назначенные пользователю. | [UserRoleTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) |

Опять же достаточно для реализации классов, которые планируется использовать в приложении.

В классы доступа к данным необходимо предоставить код для выполнения операций с данными для вашего конкретного сохраняемости механизма. Например в реализации MySQL, UserTable класс содержит метод для вставки новой записи в таблице базы данных пользователей. Переменная с именем `_database` является экземпляром класса MySQLDatabase.

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample1.cs)]

После создания ваши классы доступа к данным, необходимо создать классы хранилища, которые вызывают конкретные методы в слое доступа к данным.

<a id="user"></a>
## <a name="customize-the-user-class"></a>Настроить класс пользователя

При реализации поставщика хранилища, необходимо создать класс пользователя, что эквивалентно [IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser(v=vs.108).aspx) в класс [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) пространство имен:

В примере ниже показан класс IdentityUser, который необходимо создать и интерфейс для реализации этого класса.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image2.png)

[IUser&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx) интерфейс определяет свойства, которые UserManager предпринимается попытка вызова при выполнении запрошены операции. Интерфейс содержит два свойства - идентификатор и имя пользователя. [IUser&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx) интерфейс позволяет указать тип ключа для пользователя с помощью универсального **TKey** параметра. Тип идентификатора свойства совпадает со значением параметра TKey.

Также предоставляет инфраструктуру идентификации [IUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.iuser(v=vs.108).aspx) интерфейс (без универсального параметра) Если вы хотите использовать строковое значение для ключа.

IdentityUser класс, реализующий IUser и содержит дополнительные свойства или конструкторы для пользователей на веб-сайт. В следующем примере показан класс IdentityUser, использующего целое число для ключа. Идентификатор поля задается **int** в соответствии с значением универсального параметра. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample2.cs)]

 Полную реализацию, см. в разделе [IdentityUser (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs). 

<a id="userstore"></a>
## <a name="customize-the-user-store"></a>Настроить хранилище пользователя

Можно также создать UserStore класс, предоставляющий методы для всех операций с данными пользователя. Этот класс эквивалентно [UserStore&lt;TUser&gt; ](https://msdn.microsoft.com/library/dn315446(v=vs.108).aspx) в класс [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) пространства имен. В классе UserStore реализации [IUserStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx) и любые дополнительные интерфейсы. Выберите дополнительные интерфейсы для реализации на основе функциональности, которую вы хотите предоставить в приложении.

На следующем рисунке необходимо создать класс UserStore и соответствующие интерфейсы.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image3.png)

Шаблон проекта по умолчанию в Visual Studio содержит код, который предполагается, что многие из дополнительного интерфейса были реализованы в хранилище пользователя. Если вы используете шаблон по умолчанию с помощью настраиваемого пользовательского хранилища, необходимо реализовывать дополнительные интерфейсы в вашем хранилище пользователей или изменять код шаблона больше не вызывать методы в интерфейсах, не реализованный.

 Приведенный ниже показан простой пользовательский класс хранилища. **TUser** универсальный параметр принимает тип класса пользователя, который обычно является класс IdentityUser, вы определили. **TKey** универсальный параметр принимает тип ключа пользователя. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample3.cs)]

 В этом примере конструктор, который принимает параметр с именем *базы данных* типа ExampleDatabase представлен только способ передачи в классе доступа данных. Например в реализации MySQL, этот конструктор принимает параметр типа MySQLDatabase. 

В классе UserStore используйте классы доступа к данным, которые созданы для выполнения операций. Например в реализации MySQL класс UserStore имеет метод CreateAsync, который использует экземпляр UserTable для вставки новой записи. **Вставить** метод **userTable** объект является тем же методом, который был показан в предыдущем разделе. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample4.cs)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>Интерфейсы для реализации при настройке хранилища пользователя

На следующем рисунке показаны Дополнительные сведения о функциональных возможностях, определенные в каждом интерфейсе. Все интерфейсы, необязательные наследовать от IUserStore.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image4.png)

- **IUserStore**  
  [IUserStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613278(v=vs.108).aspx) это единственный интерфейс, необходимо реализовать в вашем хранилище пользователей. Он определяет методы для создания, обновление, удаление и извлечение пользователей.
- **IUserClaimStore**  
  [IUserClaimStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613265(v=vs.108).aspx) интерфейс определяет методы, необходимо реализовать в вашем хранилище пользователей для включения заявок на доступ пользователя. Он содержит методы или добавления, удаления и получает утверждения пользователя.
- **IUserLoginStore**  
  [IUserLoginStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613272(v=vs.108).aspx) определяет методы, необходимо реализовать в вашем хранилище пользователей для включения внешних поставщиков проверки подлинности. Он содержит методы для добавления, удаления и получения учетных данных для входа и метод для извлечения на основе сведений имени входа пользователя.
- **IUserRoleStore**  
  [IUserRoleStore&lt;TKey, TUser&gt; ](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx) интерфейс определяет методы, необходимо реализовать в вашем хранилище пользователей для сопоставления пользователей в роль. Он содержит методы для добавления, удаления и получения ролей пользователя и метод для проверки, если пользователю назначена роль.
- **IUserPasswordStore**  
  [IUserPasswordStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613273(v=vs.108).aspx) интерфейс определяет методы, необходимо реализовать в вашем хранилище пользователей для сохранения хэшированные пароли. Он содержит методы для получения и установки хэшированный пароль и метод, который указывает, ли пользователь задать пароль.
- **IUserSecurityStampStore**  
  [IUserSecurityStampStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613277(v=vs.108).aspx) интерфейс определяет методы, необходимо реализовать в вашем хранилище пользователей использовать метку безопасности для, указывающее, изменилось ли сведения об учетной записи пользователя . Эта метка обновляется, когда пользователь изменяет пароль, или добавляет или удаляет имена входа. Он содержит методы для получения и установки метку безопасности.
- **IUserTwoFactorStore**  
  [IUserTwoFactorStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613279(v=vs.108).aspx) интерфейс определяет методы, необходимо реализовать для реализации двухфакторную проверку подлинности. Он содержит методы для получения и установки ли двухфакторная проверка подлинности включена для пользователя.
- **IUserPhoneNumberStore**  
  [IUserPhoneNumberStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613275(v=vs.108).aspx) интерфейс определяет методы, необходимо реализовать для хранения номеров телефонов пользователя. Он содержит методы для получения и установки, номер телефона и подтвержден ли номер телефона.
- **IUserEmailStore**  
  [IUserEmailStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613143(v=vs.108).aspx) интерфейс определяет методы, необходимо реализовать для хранения адреса электронной почты пользователя. Он содержит методы для получения и задания, адрес электронной почты и подтвержден ли адрес электронной почты.
- **IUserLockoutStore**  
  [IUserLockoutStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613271(v=vs.108).aspx) интерфейс определяет методы, необходимо реализовать для хранения сведений о блокировке учетной записи. Он содержит методы для получения текущее количество неудачных попыток доступа, получение и параметр учетной записи могут быть заблокированы, получение и установка блокировке Дата окончания, увеличивая число неудачных попыток и сброс число неудачных попыток входа.
- **IQueryableUserStore**  
  [IQueryableUserStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613267(v=vs.108).aspx) интерфейс определяет членов, необходимо реализовать для создания хранилища поддерживает запросы пользователя. Он содержит свойство, которое содержит поддерживает запросы пользователей.

  Реализовать интерфейсы, которые требуются в приложении; Например IUserClaimStore, IUserLoginStore, IUserRoleStore, IUserPasswordStore и IUserSecurityStampStore интерфейсы, как показано ниже. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample5.cs)]

Полная реализация (включая все интерфейсы), см. в разделе [UserStore (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs).

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a>IdentityUserClaim IdentityUserLogin и IdentityUserRole

Пространство имен Microsoft.AspNet.Identity.EntityFramework содержит реализации [IdentityUserClaim](https://msdn.microsoft.com/library/dn613250(v=vs.108).aspx), [IdentityUserLogin](https://msdn.microsoft.com/library/dn613251(v=vs.108).aspx), и [IdentityUserRole](https://msdn.microsoft.com/library/dn613252(v=vs.108).aspx) классы. Если вы используете эти функции, можно создать собственные версии этих классов и определить свойства для вашего приложения. Тем не менее иногда бывает более эффективно не загружать эти сущности в памяти при выполнении основных операций (например, добавление или удаление утверждения пользователя). Вместо этого классы хранилища базы данных могут выполнять эти операции непосредственно на источнике данных. Например метод UserStore.GetClaimsAsync() можно вызвать userClaimTable.FindByUserId(user. Метод ID) для выполнения запроса на который непосредственно таблицы и возвращает список утверждений.

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample6.cs)]

<a id="role"></a>
## <a name="customize-the-role-class"></a>Настроить класс ролей

При реализации поставщика хранилища, необходимо создать класс ролей, что эквивалентно [IdentityRole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityrole(v=vs.108).aspx) в класс [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) пространство имен:

В примере ниже показан класс IdentityRole, который необходимо создать и интерфейс для реализации этого класса.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image5.png)

[IRole&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx) интерфейс определяет свойства, которые RoleManager предпринимается попытка вызова при выполнении запрошены операции. Интерфейс содержит два свойства - идентификатор и имя. [IRole&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx) интерфейс позволяет указать тип ключа для роли через универсальный **TKey** параметра. Тип идентификатора свойства совпадает со значением параметра TKey.

Также предоставляет инфраструктуру идентификации [IRole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.irole(v=vs.108).aspx) интерфейс (без универсального параметра) Если вы хотите использовать строковое значение для ключа.

В следующем примере показан класс IdentityRole, использующего целое число для ключа. Поле идентификатора присваивается int в соответствии с значением универсального параметра. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample7.cs)]

 Полную реализацию, см. в разделе [IdentityRole (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs). 

<a id="rolestore"></a>
## <a name="customize-the-role-store"></a>Настройка хранилища ролей

Можно также создать RoleStore класс, предоставляющий методы для всех операций с данными для ролей. Этот класс эквивалентно [RoleStore&lt;TRole&gt; ](https://msdn.microsoft.com/library/dn468181(v=vs.108).aspx) класса в пространстве имен Microsoft.ASP.NET.Identity.EntityFramework. В классе RoleStore реализации [IRoleStore&lt;TRole, TKey&gt; ](https://msdn.microsoft.com/library/dn613266(v=vs.108).aspx) и при необходимости [IQueryableRoleStore&lt;TRole, TKey&gt; ](https://msdn.microsoft.com/library/dn613262(v=vs.108).aspx) интерфейс.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image6.png)

Пример класса роли хранилища. Универсальный параметр TRole принимает тип вашего класса роли, обычно это IdentityRole класса, который вы определили. Универсальный параметр TKey принимает тип ключа вашей роли. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample8.cs)]

- **IRoleStore&lt;TRole&gt;**  
  [IRoleStore](https://msdn.microsoft.com/library/dn468195.aspx) интерфейс определяет методы для реализации в классе роли хранилища. Он содержит методы для создания, обновления, удаления и извлечения ролей.
- **RoleStore&lt;TRole&gt;**  
  Чтобы настроить RoleStore, создайте класс, реализующий интерфейс IRoleStore. Достаточно для реализации этого класса, если используются роли в системе. Конструктор, который принимает параметр с именем *базы данных* типа ExampleDatabase представлен только способ передачи в классе доступа данных. Например в реализации MySQL, этот конструктор принимает параметр типа MySQLDatabase.  
  
  Полную реализацию, см. в разделе [RoleStore (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs) .

<a id="reconfigure"></a>
## <a name="reconfigure-application-to-use-new-storage-provider"></a>Перенастройка приложения для использования нового поставщика хранилища

Вы реализовали новый поставщику хранения. Теперь необходимо настроить приложение для использования этого поставщика хранилища. Если поставщик хранилища по умолчанию был включен в проект, необходимо удалить поставщик по умолчанию и заменить его с помощью поставщика.

### <a name="replace-default-storage-provider-in-mvc-project"></a>Замена поставщика хранилища по умолчанию в проекте MVC

1. В **управление пакетами NuGet** окно, удалите **Microsoft ASP.NET Identity EntityFramework** пакета. Этот пакет можно найти, выполнив поиск Identity.EntityFramework в установленные пакеты.  
    ![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image7.png) Вам потребуется указать, если требуется, чтобы удалить Entity Framework. Если не обязательно в других частях приложения, ее можно удалить.
2. В папке Models находится в файле IdentityModels.cs, удалите или закомментируйте **ApplicationUser** и **ApplicationDbContext** классы. В MVC-приложениях можно удалить весь файл IdentityModels.cs. В приложении Web Forms удалить эти два класса, но убедитесь, что хранить вспомогательный класс, который также находится в файле IdentityModels.cs.
3. Если поставщику хранения находится в отдельном проекте, добавьте ссылку на него в веб-приложения.
4. Замените все вхождения `using Microsoft.AspNet.Identity.EntityFramework;` с с помощью инструкции для пространства имен поставщика хранилища.
5. В **Startup.Auth.cs** измените **ConfigureAuth** метод для использования одного экземпляра соответствующий контекст. 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample9.cs?highlight=3)]
6. В приложении\_начала открытой вложенной папкой, **IdentityConfig.cs**. В классе ApplicationUserManager изменить **создать** метод для возврата руководитель пользователя, использующего хранилище пользовательских. 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample10.cs?highlight=3)]
7. Замените все вхождения **ApplicationUser** с **IdentityUser**.
8. Проект по умолчанию включает в себя некоторые члены в класс пользователя, не определенные в интерфейсе IUser; Например, адрес электронной почты, PasswordHash и GenerateUserIdentityAsync. Если ваш класс пользователя нет этих членов, необходимо их реализации или изменить код, который использует эти элементы.
9. Если вы создали все экземпляры RoleManager, измените этот код, чтобы использовать новый класс RoleStore.  

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample11.cs)]
10. По умолчанию проект предназначен для пользователя классом, имеющим строковое значение для ключа. Если ваш класс пользователя имеет другой тип ключа (например, целое число), необходимо изменить проект для работы с типом. См. в разделе [изменение первичного ключа для пользователей в ASP.NET Identity](change-primary-key-for-users-in-aspnet-identity.md).
11. При необходимости, добавьте строку подключения в файле Web.config.

<a id="other"></a>
## <a name="other-resources"></a>Другие источники

- Блог: [реализация ASP.NET Identity](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- Руководство и GIT код: [поставщика удостоверений Asp.Net Simple.Data](http://designcoderelease.blogspot.co.uk/2015/03/simpledata-aspnet-identity-provider.html)
- Учебник:[настройке базовых учетных записей удостоверений и направить в базу данных внешнего](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx). По [ @xivSolutions ](https://twitter.com/xivSolutions).
- Учебник[: реализация поставщика хранилища MySQL пользовательских ASP.NET Identity](implementing-a-custom-mysql-aspnet-identity-storage-provider.md)
- [Сущности CodeFluent](http://blog.codefluententities.com/2014/04/30/asp-net-identity-v2-and-codefluent-entities/) по [SoftFluent](http://www.softfluent.com/)
- [Хранилище таблиц Azure](https://www.nuget.org/packages/accidentalfish.aspnet.identity.azure/) , Джеймс Рэндалл.
- Хранилище таблиц Azure: [AspNet.Identity.TableStorage](https://github.com/stuartleeks/leeksnet.AspNet.Identity.TableStorage) по [ @stuartleeks ](https://twitter.com/stuartleeks).
- [CouchDB / Cloudant по Wertheim Дэниэл.](https://github.com/danielwertheim/mycouch.aspnet.identity)
- Эластичный кать[h: удостоверений эластичной](https://github.com/bmbsqd/elastic-identity) по Bombsquad AB.
- [MongoDB](http://www.nuget.org/packages/MongoDB.AspNet.Identity/) , Джонатан Sheely Джонатан Sheely.
- [NHibernate.AspNet.Identity](https://github.com/milesibastos/NHibernate.AspNet.Identity) по Antônio Milesi Bastos.
- [RavenDB](http://www.nuget.org/packages/AspNet.Identity.RavenDB/1.0.0) по [ @tourismgeek ](https://twitter.com/tourismgeek).
- [RavenDB.AspNet.Identity](https://github.com/ILMServices/RavenDB.AspNet.Identity) по [ILMServices](http://www.ilmservice.com/).
- Redis: [Redis.AspNet.Identity](https://github.com/aminjam/Redis.AspNet.Identity)
- Шаблоны T4 для создания EF кода для пользовательского хранилища базы данных «сначала»: [AspNet.Identity.EntityFramework](https://github.com/cbfrank/AspNet.Identity.EntityFramework)
