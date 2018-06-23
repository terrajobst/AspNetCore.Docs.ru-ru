---
uid: identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
title: Миграция существующего веб-сайта из SQL членства в ASP.NET Identity | Документы Microsoft
author: Rick-Anderson
description: Этот учебник демонстрирует действия по переносу существующих веб-приложения с именем пользователя и роли данных, созданных с помощью членства SQL для нового удостоверения ASP.NET s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/19/2014
ms.topic: article
ms.assetid: 220d3d75-16b2-4240-beae-a5b534f06419
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 1766c11dabec3931ec2bfc4ae2e15332427d7855
ms.sourcegitcommit: e22097b84d26a812cd1380a6b2d12c93e522c125
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/22/2018
ms.locfileid: "36314017"
---
<a name="migrating-an-existing-website-from-sql-membership-to-aspnet-identity"></a>Миграция существующего веб-сайта из SQL членства в ASP.NET Identity
====================
по [Рик Андерсон](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)

> Этот учебник демонстрирует действия по переносу существующих веб-приложения с именем пользователя и роли данных, созданный с помощью членства SQL в новой системе ASP.NET Identity. Этот подход включает изменение существующей схемы базы данных к одному требуется ASP.NET Identity и обработчик в старые и новые классы, к нему. После после переноса базы данных применять этот подход, будущие обновления удостоверение будет обрабатываться без усилий.


В этом учебнике мы займет шаблона веб-приложения (Web Forms) создан с помощью Visual Studio 2010 для создания данных пользователя и роли. Затем мы будем использовать скрипты SQL для миграции существующей базы данных для таблицы, необходимые для системы удостоверений. Затем мы установить необходимые пакеты NuGet и добавить новые страницы управления учетной записи, которые используют системы удостоверений для управления членством. Для проверки миграции пользователей, созданных с помощью членства SQL должно быть разрешение на вход в и новые пользователи должны иметь возможность регистрации. Можно найти полный пример [здесь](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/). См. также [миграция из членства в ASP.NET в ASP.NET Identity](http://travis.io/blog/2015/03/24/migrate-from-aspnet-membership-to-aspnet-identity.html).

## <a name="getting-started"></a>Начало работы

### <a name="creating-an-application-with-sql-membership"></a>Создание приложения с членством SQL

1. Нам нужно начать с существующего приложения, который использует членства SQL и данных пользователя и роли. В данной статье давайте создадим веб-приложения в Visual Studio 2010.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.jpg)
2. С помощью средства настройки ASP.NET, создайте 2 пользователя: **oldAdminUser** и **oldUser.**

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.jpg)
3. Создайте роль с именем Admin и добавить «oldAdminUser» в качестве пользователя с данной ролью.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.png)
4. Создайте раздел администратора сайта с Default.aspx. Задайте тег авторизации в файле web.config для включения доступа только для пользователей в роли администратора. Дополнительные сведения можно найти здесь [https://www.asp.net/web-forms/tutorials/security/roles/role-based-authorization-cs](../../../web-forms/overview/older-versions-security/roles/role-based-authorization-cs.md)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.png)
5. Просмотр базы данных в обозревателе серверов для понимания таблицы, созданные система членства SQL. Данные входа в систему пользователя хранится в aspnet\_пользователей и aspnet\_таблицы членства, во время хранения данных роли в aspnet\_таблицу Roles. Сведения о том, какие пользователи хранятся в какой роли в aspnet\_UsersInRoles таблицы. Для управления участия достаточно перенести данные в таблицах в системе ASP.NET Identity.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.png)

### <a name="migrating-to-visual-studio-2013"></a>Миграция в Visual Studio 2013

1. Установка Visual Studio Express 2013 для Web или Visual Studio 2013 вместе с [последние обновления](https://www.microsoft.com/download/details.aspx?id=44921).
2. Откройте проект выше в установленной версии Visual Studio. Если SQL Server Express не установлен на компьютере, запрос отображается при открытии проекта, так как использует строку подключения SQL Express. Можно либо установить SQL Express или как обойти изменить строку подключения с LocalDb. В этой статье мы изменим ее в LocalDb.
3. Откройте файл web.config и измените строку подключения из. SQLExpess для v11.0 (LocalDb). Удалите "User Instance = true" в строке подключения.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.jpg)
4. Откройте обозреватель серверов и проверьте схему и данные таблицы можно контролировать.
5. Система идентификации ASP.NET работает с версией 4.5 или более поздней версии платформы. Перенацелить приложение для версии 4.5 или более поздней версии.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image5.png)

    Постройте проект, чтобы убедиться, что отсутствуют ошибки.

### <a name="installing-the-nuget-packages"></a>Установка пакетов Nuget

1. В обозревателе решений щелкните правой кнопкой мыши проект &gt; **управление пакетами NuGet**. В поле поиска введите «Asp.net Identity». Выберите пакет, в списке результатов и нажмите кнопку установить. Примите лицензионное соглашение с помощью кнопки «Принимаю». Обратите внимание, что этот пакет будут установлены пакеты зависимостей: EntityFramework и Microsoft ASP.NET Identity Core. Аналогичным образом установите следующие пакеты (Если вы не хотите включить в журнал OAuth, пропустите последние 4 пакеты OWIN):

   - Microsoft.AspNet.Identity.Owin
   - Microsoft.Owin.Host.SystemWeb
   - Microsoft.Owin.Security.Facebook
   - Microsoft.Owin.Security.Google
   - Microsoft.Owin.Security.MicrosoftAccount
   - Microsoft.Owin.Security.Twitter

     ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image6.png)

### <a name="migrate-database-to-the-new-identity-system"></a>Перенести базу данных в новой системе удостоверений

Следующим шагом является миграция существующей базы данных для схемы, необходимой для системы ASP.NET Identity. Для достижения запустим SQL скрипт она имеет набор команд для создания новых таблиц и переноса существующих сведений о пользователе в новые таблицы. Файл сценария можно найти [здесь](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/Migrations.sql).

В этом примере зависит этот файл сценария. Если создана схема для таблиц с помощью членства SQL пользовательские или изменить скрипты необходимость соответствующим образом изменить.

### <a name="how-to-generate-the-sql-script-for-schema-migration"></a>Создание скрипта SQL для переноса схемы

Для ASP.NET Identity классы для работы без дополнительной настройки существующих пользователей с данными нам необходимо для переноса схемы базы данных с тем, необходимые для ASP.NET Identity. Это можно сделать путем добавления новых таблиц и копирование существующие данные в этих таблицах. По умолчанию ASP.NET Identity использует EntityFramework для сопоставления классов модели удостоверения в базе данных для хранения или извлечения информации. Эти классы модели реализовывать интерфейсы идентификаторов основных компонентов, определение и объектов роли пользователя. Таблицы и столбцы в базе данных основаны на этих классов модели. Классы модели EntityFramework v2.1.0 удостоверений и их свойства, как определено ниже

| **IdentityUser** | **Type** | **IdentityRole** | **IdentityUserRole** | **IdentityUserLogin** | **IdentityUserClaim** |
| --- | --- | --- | --- | --- | --- |
| Идентификатор | string | Идентификатор | RoleId | ProviderKey | Идентификатор |
| Имя пользователя | string | name | Идентификатор пользователя | Идентификатор пользователя | ClaimType |
| PasswordHash | string |  |  | LoginProvider | ClaimValue |
| SecurityStamp | string |  |  |  | Пользователь\_идентификатор |
| Адрес эл. почты | string |  |  |  |  |
| EmailConfirmed | bool |  |  |  |  |
| PhoneNumber | string |  |  |  |  |
| PhoneNumberConfirmed | bool |  |  |  |  |
| LockoutEnabled | bool |  |  |  |  |
| LockoutEndDate | DateTime |  |  |  |  |
| Счетчик AccessFailedCount | int |  |  |  |  |

Мы должны иметь таблиц для каждой из этих моделей с столбцы, соответствующие свойства. Сопоставление между классами и таблиц, определенное в `OnModelCreating` метод `IdentityDBContext`. Этот процесс известен как метод fluent API конфигурации и Дополнительные сведения можно найти [здесь](https://msdn.microsoft.com/data/jj591617.aspx). Конфигурация для классов является описанным ниже

| **Класс** | **Таблица** | **первичный ключ** | **внешний ключ** |
| --- | --- | --- | --- |
| IdentityUser | AspnetUsers | Идентификатор |  |
| IdentityRole | AspnetRoles | Идентификатор |  |
| IdentityUserRole | AspnetUserRole | UserId + RoleId | Пользователь\_идентификатор -&gt;AspnetUsers RoleId -&gt;AspnetRoles |
| IdentityUserLogin | AspnetUserLogins | ProviderKey + UserId + LoginProvider | UserId -&gt;AspnetUsers |
| IdentityUserClaim | AspnetUserClaims | Идентификатор | Пользователь\_идентификатор -&gt;AspnetUsers |

Эта информация позволяет создавать инструкции SQL для создания новых таблиц. Мы записи каждой инструкции по отдельности или сформировать весь скрипт с помощью команд EntityFramework PowerShell, которые затем можно изменить при необходимости. Для этого откройте VS **консоль диспетчера пакетов** из **представление** или **средства** меню

- Выполните команду «Enable-Migrations», чтобы включить EntityFramework миграции.
- Выполните команду «Add-migration начальным», создающий код начальной настройки, чтобы создать базу данных в C# или Visual Basic.
- Последний шаг заключается в запуске «Update-Database — сценарий «команду, которая формирует скрипт SQL, основанных на классах модели.

[!INCLUDE[](../../../includes/identity/alter-command-exception.md)]

Этот скрипт создания базы данных можно использовать как начало, где мы будем внесения дополнительных изменений для добавления новых столбцов и копирования данных. Преимущество этого — мы создаем `_MigrationHistory` таблицу, которая используется для изменения схемы базы данных при модели классы изменений для будущих версий выпусков Identity EntityFramework. 

Сведения о членстве пользователя SQL имел другие свойства в дополнение к функциям в класс модели удостоверения пользователя а именно электронной почты, попыток ввода пароля, Дата последнего входа, Дата последнего ожидания блокировки и т. д. Это полезные сведения, и мы предлагаем переносятся системы удостоверений. Это можно сделать, добавив дополнительные свойства пользователя модели и сопоставления их обратно к столбцам таблицы в базе данных. Это можно сделать путем добавления класса, который наследуется от класса `IdentityUser` модели. Мы можно добавлять свойства к этот настраиваемый класс и изменить скрипт SQL для добавления соответствующих столбцов при создании таблицы. Код для этого класса описан далее в статье. Скрипт SQL для создания `AspnetUsers` таблицы после добавления новых свойств будет

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample1.sql)]

Далее нам нужно скопировать информацию из существующего членства базы данных SQL в новые таблицы для удостоверения. Это можно сделать с помощью SQL путем копирования данных непосредственно из одной таблицы в другую. Чтобы добавить данные в строках таблицы, мы используем `INSERT INTO [Table]` построения. Для копирования из другой таблицы, можно использовать `INSERT INTO` инструкции вместе с `SELECT` инструкции. Получить все сведения о пользователе, нам нужно запросить *aspnet\_пользователей* и *aspnet\_членства* таблиц и скопируйте данные на *AspNetUsers*таблицы. Мы используем `INSERT INTO` и `SELECT` вместе с `JOIN` и `LEFT OUTER JOIN` инструкции. Дополнительные сведения о запросе и копирования данных между таблицами, см. [это](https://technet.microsoft.com/library/ms190750%28v=sql.105%29.aspx) ссылку. Кроме того таблицы AspnetUserLogins и AspnetUserClaims пусты начинается с, так как нет сведений в членства SQL, это сопоставляется по умолчанию. Только копируемых данных предназначен для пользователей и ролей. Для проекта, созданного в предыдущих шагах будет SQL-запрос, чтобы скопировать данные в таблицу пользователей

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample2.sql)]

В приведенном выше инструкции SQL, сведения о каждом пользователе из *aspnet\_пользователей* и *aspnet\_членства* таблицы копируется в столбцах  *AspnetUsers* таблицы. Единственное изменение, здесь является при копировании пароль. Поскольку алгоритм шифрования паролей в членства SQL «PasswordSalt» и «PasswordFormat», мы копируем, слишком вместе с хэшированный пароль, чтобы он может использоваться для расшифровки пароля по идентификатору. Это описывается далее в статье, когда подключение hasher пользовательского пароля. 

В этом примере зависит этот файл сценария. Для приложений, которые имеют дополнительные таблицы подобный подход для добавления дополнительных свойств в класс модели пользователя и сопоставить их со столбцами в таблице AspnetUsers может следовать разработчики. Чтобы запустить скрипт,

1. Откройте обозреватель серверов. Разверните подключение «ApplicationServices» для отображения таблиц. Щелкните правой кнопкой мыши узел таблицы и выберите параметр «Создать запрос»

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image7.png)
2. В окне запроса скопируйте и вставьте весь скрипт SQL из файла Migrations.sql. Выполнение файла скрипта, нажав кнопку со стрелкой «Выполнить».

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.jpg)

    Обновление окна обозревателя серверов. Пять новые таблицы создаются в базе данных.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image8.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image9.png)

    Ниже приведен как данные в таблицы членства SQL сопоставляются с новой системы удостоверений.

    ASPNET\_роли--&gt; AspNetRoles

    ASP\_netUsers и asp\_netMembership--&gt; AspNetUsers

    ASPNET\_UserInRoles--&gt; AspNetUserRoles

    Как описано в предыдущем разделе, то AspNetUserClaims и AspNetUserLogins таблицы пусты. Поле «Дискриминатора» в таблице пользователь ASPNET должно соответствовать имени класса модели, которая определяется как следующий шаг. Также PasswordHash столбец находится в форме "зашифрованный пароль | соли пароль | формат пароля". Это позволяет использовать специальную логику шифрования членства SQL, чтобы можно было повторно использовать старый пароль. Который описан далее в этой статье.

### <a name="creating-models-and-membership-pages"></a>Создание моделей и членство страниц

Как упоминалось ранее, функция удостоверений использует Entity Framework, связавшись с базы данных для хранения учетных записей по умолчанию. Для работы с существующие данные в таблице, необходимо создать классы модели, которые сопоставляются с таблицами и соединить их в системы удостоверений. Как часть контракта удостоверений классы модели должен реализовывать интерфейсы, определенные в библиотеке dll Identity.Core или расширить существующую реализацию этих интерфейсов, доступных в Microsoft.AspNet.Identity.EntityFramework.

В нашем примере AspNetRoles, AspNetUserClaims, AspNetLogins и AspNetUserRole таблицы имеют столбцы, которые похожи на существующую реализацию системы удостоверений. Поэтому мы можно повторно использовать существующие классы для сопоставления с этими таблицами. Пользователь ASPNET таблица содержит некоторые дополнительные столбцы, которые используются для хранения дополнительных сведений из таблицы членства SQL. Это могут быть сопоставлены, создав класс модели, расширить существующую реализацию «IdentityUser» и добавить дополнительные свойства.

1. Модели создать папку в проекте и добавьте класс пользователя. Имя класса должно соответствовать данные, добавленные в столбце «Дискриминатора» таблицы «AspnetUsers».

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image10.png)

    Класс пользователя должен расширять класс IdentityUser в *Microsoft.AspNet.Identity.EntityFramework* dll. Объявления свойств в классе, которые сопоставляются со столбцами пользователь ASPNET. Свойства идентификатора, имени пользователя, PasswordHash и SecurityStamp определены в IdentityUser и включены. Ниже приведен код для пользовательского класса, который имеет те же свойства

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample3.cs)]
2. Класс Entity Framework DbContext требуется для сохранения данных в моделях обратно в таблицы и получения данных из таблицы для заполнения модели. *Microsoft.AspNet.Identity.EntityFramework* dll определяет класс IdentityDbContext, который взаимодействует с таблицами Identity для получения и хранения данных. IdentityDbContext&lt;tuser&gt; принимает класс «TUser», который может быть любой класс, расширяющий класс IdentityUser.

    Создайте новый класс, расширяющий IdentityDbContext в папке «Модели», передача в классе 'User', созданной на шаге 1 ApplicationDBContext

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample4.cs)]
3. Управление пользователями в новой системе удостоверения выполняется с помощью UserManager&lt;tuser&gt; класса, определенного в *Microsoft.AspNet.Identity.EntityFramework* dll. Необходимо создать пользовательский класс, который расширяет UserManager, передав в классе 'User', созданной на шаге 1.

    В папке «Models» создайте новый класс, расширяющий UserManager UserManager&lt;пользователя&gt;

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample5.cs)]
4. Пароли пользователей приложения, зашифрованы и хранятся в базе данных. Алгоритм шифрования, используемый в членства SQL отличается от того, в новой системе удостоверений. Повторное использование старых паролей, нам нужно выборочно расшифровки паролей при входе с помощью алгоритма членства SQL при использовании в удостоверении алгоритм шифрования для новых пользователей старого пользователям.

    Класс UserManager имеет свойство «PasswordHasher», который хранит экземпляр класса, который реализует интерфейс «IPasswordHasher». Используется для шифрования и расшифровки паролей во время проверки подлинности пользовательских транзакций. В классе UserManager, определенный на шаге 3, создайте новый класс SQLPasswordHasher и скопируйте ниже кода.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample6.cs)]

    Устраните ошибки компиляции путем импорта пространства имен System.Text и System.Security.Cryptography.

    Метод EncodePassword шифрует пароль согласно реализации шифрования членства SQL по умолчанию. Это будет взято из System.Web dll. Если старое приложение используется пользовательская реализация затем должно быть отражено здесь. Необходимо определить два других метода *HashPassword* и *VerifyHashedPassword* , использующих *EncodePassword* метод хэша пароля или проверить, обычный текст пароль с одного существующим в базе данных.

    Система членства SQL используется PasswordHash, PasswordSalt и PasswordFormat для хэширования пароля, введенного пользователем при регистрации или изменить свой пароль. Во время миграции всех трех полях хранятся в столбце PasswordHash в таблице пользователь ASPNET, разделенных "|" символов. Если пользователь вошел в систему и пароль содержит следующие поля, мы используем шифрования членства SQL для проверки пароля; в противном случае мы используем шифрования по умолчанию для системы удостоверений, чтобы проверить пароль. Таким образом старый пользователям не нужно будет изменять свои пароли, после переноса приложения.
5. Объявите конструктор для класса UserManager и передать ее как SQLPasswordHasher свойству в конструктор.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample7.cs)]

### <a name="create-new-account-management-pages"></a>Создать новую учетную запись страницы управления

Следующим шагом в процессе переноса является добавление учетной записи управления страниц, которые будут пользователи могут регистрировать и войдите в систему. Старой учетной записи страниц из членства SQL используйте элементы управления, которые не работают с новой системы удостоверений. Чтобы добавить нового пользователя страницы управления учебнике по этой ссылке [ https://www.asp.net/identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project ](../getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project.md) начиная с шага «Добавление веб-формы для регистрации пользователей для приложения», так как мы уже созданы проект и добавлены NuGet пакеты.

Необходимо внести некоторые изменения для образца для работы с проектом, которая у нас есть здесь.

- Register.aspx.cs и Login.aspx.cs кода используйте классы `UserManager` из идентификаторов пакетов для создания пользователя. В этом примере используется UserManager, добавленные в папку Models, выполнив действия, приведенные выше.
- Класс пользователя, созданные вместо IdentityUser Register.aspx.cs и Login.aspx.cs коде программной части классы. Это подключит в нашего пользовательского класса в систему удостоверений.
- Часть, для создания базы данных могут быть пропущены.
- Разработчику необходимо задать идентификатор приложения для нового пользователя для сопоставления кода текущего приложения. Это можно сделать, запросив ApplicationId для этого приложения, перед созданием объекта-пользователя в классе Register.aspx.cs и ее настройка перед созданием пользователя. 

    Пример

    Определение метода в Register.aspx.cs страницу, чтобы запросить aspnet\_приложений таблицы и получение идентификатора приложения в соответствии с именем приложения

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample8.cs)]

    Теперь установить это объекта пользователя

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample9.cs)]

Используйте старое имя пользователя и пароль для входа в систему существующего пользователя. Страница регистрации используется для создания нового пользователя. Также убедитесь, что пользователи являются в ролях должным образом.

Перенос системы удостоверений помогает пользователю добавить Open Authentication (OAuth) для приложения. См. образец [здесь](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/) содержит включенным OAuth.

## <a name="next-steps"></a>Следующие шаги

В этом учебнике мы показали способов переноса пользователей членства SQL с ASP.NET Identity, но мы не порта данных профиля. В следующем уроке мы будем в перенос данных профиля из членства SQL до новой системы удостоверений.

Можно оставить отзывы в нижней части этой статьи.

*Благодаря Tom Dykstra и Рик Андерсон для просмотра статьи.*
