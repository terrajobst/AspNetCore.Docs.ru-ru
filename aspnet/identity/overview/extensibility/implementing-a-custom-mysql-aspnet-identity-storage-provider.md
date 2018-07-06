---
uid: identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
title: Реализация поставщика хранилища MySQL пользовательского удостоверения ASP.NET | Документация Майкрософт
author: raquelsa
description: ASP.NET Identity является расширяемой системой, что дает возможность создать собственный поставщик хранилища и подключить его к приложению без переработки приложения...
ms.author: aspnetcontent
ms.date: 05/22/2015
ms.assetid: 248f5fe7-39ba-40ea-ab1e-71a69b0bd649
msc.legacyurl: /identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
msc.type: authoredcontent
ms.openlocfilehash: e5784991a95dcff0db38d87707bc83555da716df
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805679"
---
<a name="implementing-a-custom-mysql-aspnet-identity-storage-provider"></a>Реализация поставщика хранилища MySQL пользовательских ASP.NET Identity
====================
по [Raquel Soares De Almeida](https://github.com/raquelsa), [Suhas Joshi](https://github.com/suhasj), [Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET Identity — это расширяемая система, что дает возможность создать собственный поставщик хранилища и подключить его к приложению без переработки приложения. В этом разделе описывается создание поставщика хранилища MySQL для ASP.NET Identity. Сведения о создании пользовательские поставщики хранилищ, см. в разделе [Обзор из пользовательские поставщики хранилищ для ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).
> 
> Для работы с этим учебником необходимо иметь Visual Studio 2013 с обновлением 2.
> 
> Этом руководстве выполняются следующие действия:
> 
> - Показано, как создать экземпляр базы данных MySQL в Azure.
> - В этой статье демонстрируется использование клиентского средства MySQL (MySQL Workbench) для создания таблиц и управления вашей удаленной базы данных в Azure.
> - Показано, как заменить значение по умолчанию реализации хранилища ASP.NET Identity наших собственную реализацию на проект приложения MVC.
> 
> Этот учебник был первоначально написан Майклом Raquel Soares De Almeida и Рик Андерсон ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ). Пример проекта был обновлен для идентификации 2.0 по Suhas Joshi. Раздел был обновлен для идентификации 2.0, Tom FitzMacken.


## <a name="download-completed-project"></a>Процент загрузки проекта

В конце этого руководства вы получите проект MVC-приложения с ASP.NET Identity, работать с базой данных MySQL, размещенной в Azure.

Можно загрузить завершенного поставщика хранилища MySQL в [AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/).

## <a name="the-steps-you-will-perform"></a>Шаги, которые будут выполнены

В этом учебнике вы научитесь:

1. Создание базы данных MySQL в Azure
2. Создание таблиц ASP.NET Identity в MySQL
3. Создание приложения MVC и настроить его для использования поставщика MySQL
4. Запуск приложения

В этом разделе не рассматривается архитектура ASP.NET Identity и решения, которые необходимо принять при реализации поставщика хранилища клиента. Дополнительные сведения см. в разделе [Обзор из пользовательские поставщики хранилищ для ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).

## <a name="review-mysql-storage-provider-classes"></a>Просмотрите классы поставщика хранилища MySQL

Прежде чем углубиться в процесс создания поставщика хранилища MySQL, давайте взглянем на классы, входящие в состав поставщика хранилища. Вам потребуется, которые управляют операциями базы данных и классы, которые вызываются из приложения для управления пользователями и ролями.

### <a name="storage-classes"></a>Классы хранения

- [IdentityUser](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs) -содержит свойства для пользователя.
- [UserStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs) -содержит операции добавления, обновления или получения пользователей.
- [IdentityRole](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs) -содержит свойства для ролей.
- [RoleStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs) -содержит операции добавления, удаления, обновления и получения ролей.

### <a name="data-access-layer-classes"></a>Классы слой доступа к данным

Например классы слой доступа к данным содержат инструкции SQL для работы с таблицами; Тем не менее в коде можно использовать объектно реляционного сопоставления (ORM), например Entity Framework или NHibernate. В частности приложение может наблюдаться низкая производительность без ORM, включающий отложенную загрузку и кэширование объектов. Дополнительные сведения см. в разделе [ASP.NET 2.0 удостоверений без Entity Framework?](https://aspnetidentity.codeplex.com/discussions/561828)

- [MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) -содержит подключения к базе данных MySQL и методы для выполнения операций с базой данных. UserStore и RoleStore создаются с экземпляром этого класса.
- [RoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) -содержит операции базы данных для таблицы, которая хранит ролей.
- [UserClaimsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) -содержит операции базы данных для таблицы, которая хранит утверждения пользователей.
- [UserLoginsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) -содержит операции базы данных для таблицы, которая сохраняет данные для входа пользователя.
- [UserRoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) -содержит операции базы данных для таблицы, которая хранит, какие пользователи назначены какие роли.
- [UserTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) -содержит операции базы данных для таблицы, которая сохраняет пользователей.

## <a name="create-a-mysql-database-instance-on-azure"></a>Создание экземпляра базы данных MySQL в Azure

1. Войдите на [портал Azure](https://manage.windowsazure.com/).
2. Нажмите кнопку **+ создать** в нижней части страницы, а затем выберите **ХРАНИЛИЩЕ**.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.png)
3. В **Выбор и надстройки** мастера выберите **база данных ClearDB MySQL** и щелкните стрелку в правом нижнем углу диалогового окна.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.png)
4. Сохраните значение по умолчанию **бесплатный** планирования и изменить **имя** для **IdentityMySQLDatabase**. Выберите ближайшее к вам регион и щелкните стрелку «Далее».  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.png)
5. Нажмите кнопку с галочкой, чтобы завершить создание базы данных.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image4.png)
6. После создания базы данных, можно было управлять из **надстройки** вкладка на портале управления.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image5.png)
7. Подключение к базе данных можно получить, щелкнув **сведения о ПОДКЛЮЧЕНИИ** в нижней части страницы.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image6.png)
8. Скопируйте строку подключения, нажав кнопку "Копировать" и сохраните его, чтобы использовать позже в приложении MVC.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image7.png)

## <a name="create-the-aspnet-identity-tables-in-a-mysql-database"></a>Создание таблиц ASP.NET Identity в базе данных MySQL

### <a name="install-mysql-workbench-tool-to-connect-and-manage-mysql-database"></a>Установите средство MySQL Workbench для подключения и управления базой данных MySQL

1. Установка **MySQL Workbench** средство из [страницу загрузок MySQL](http://dev.mysql.com/downloads/windows/installer/)
2. Запустите приложение и добавить щелкните на **MySQLConnections +** кнопку, чтобы добавить новое подключение. Используйте данные строки соединения, которые копируются из базы данных MySQL в Azure, созданную ранее в этом учебнике.
3. После установления подключения, откройте новую **запроса** вкладка; вставьте команды из [MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql) в запрос и выполните его, чтобы создать таблицы базы данных.
4. Теперь у вас есть все удостоверения ASP.NET необходимые таблицы, созданные в базе данных MySQL, размещенной в Azure, как показано ниже.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.jpg)

## <a name="create-an-mvc-application-project-from-template-and-configure-it-to-use-mysql-provider"></a>Создайте проект приложения MVC на основе шаблона и настроить его для использования поставщика MySQL

При необходимости установите [Visual Studio Express 2013 для Web](https://go.microsoft.com/fwlink/?LinkId=299058) или [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) с обновлением 2.

### <a name="download-the-aspnetidentitymysql-project-from-codeplex"></a>Скачайте проект ASP.NET.Identity.MySQL веб-сайте CodePlex

1. Перейдите к URL-адрес репозитория в [AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/).
2. Скачайте исходный код.
3. Извлеките ZIP-файл в локальную папку.
4. Откройте решение AspNet.Identity.MySQL и постройте его.

### <a name="create-a-new-mvc-application-project-from-template"></a>Создайте новый проект MVC-приложения на основе шаблона

1. Щелкните правой кнопкой мыши **AspNet.Identity.MySQL** решения и **добавить**, **нового проекта**
2. В **Добавление нового проекта** окна выберите **Visual C#** слева, затем **Web** , а затем выберите **веб-приложение ASP.NET**. Присвойте проекту имя **IdentityMySQLDemo**; и нажмите кнопку ОК.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.jpg)
3. В **новый проект ASP.NET** диалоговое окно, выберите шаблон MVC с параметрами по умолчанию (включающий **учетные записи отдельных пользователей** как метод проверки подлинности) и нажмите кнопку **ОК** .![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.jpg)
4. В обозревателе решений щелкните правой кнопкой мыши проект IdentityMySQLDemo и выберите **управление пакетами NuGet**. В диалоговом поле поиска текста, введите **Identity.EntityFramework**. Выберите этот пакет в списке результатов и нажмите кнопку **удаления**. Вам будет предложено удалить EntityFramework пакета зависимостей. Щелкните Да, мы больше не будет этого пакета в это приложение.
5. Правой кнопкой мыши проект IdentityMySQLDemo, выберите **добавить**, **ссылку, решения, проекты;** выберите AspNet.Identity.MySQL проекта и нажмите кнопку **ОК**.
6. В проекте IdentityMySQLDemo замените все вхождения  
    `using Microsoft.AspNet.Identity.EntityFramework;`  
   на  
     `using AspNet.Identity.MySQL;`
7. В IdentityModels.cs, задайте **ApplicationDbContext** для наследования от **MySqlDatabase** и включает конструктор, которая принимает один параметр с именем подключения.  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample1.cs)]
8. Откройте файл IdentityConfig.cs. В **ApplicationUserManager.Create** метод, replace, создание экземпляров UserManager следующим кодом:  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample2.cs)]
9. Откройте файл web.config и замените строку DefaultConnection эту запись, заменив выделенные значения в строке подключения базы данных MySQL, созданной на предыдущих шагах:  

    [!code-xml[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample3.xml?highlight=2)]

## <a name="run-the-app-and-connect-to-the-mysql-db"></a>Запустите приложение и подключения к базе данных MySQL

1. Щелкните правой кнопкой мыши **IdentityMySQLDemo** проекта и выберите **Назначить запускаемым проектом**
2. Нажмите клавишу **Ctrl + F5** для сборки и запуска приложения.
3. Щелкните **зарегистрировать** вкладки в верхней части страницы.
4. Введите новое имя пользователя и пароль и нажмите кнопку на **зарегистрировать**.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image8.png)
5. Новый пользователь теперь регистрации и входа.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image9.png)
6. Вернитесь в средство MySQL Workbench и проверять **IdentityMySQLDatabase** содержимое таблицы. Проверьте таблицу пользователей для записи, по мере регистрации новых пользователей.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image10.png)

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения о том, как включить другие методы проверки подлинности для этого приложения см. [Создание приложения ASP.NET MVC 5 с использованием Facebook и Google OAuth2 и OpenID единого входа](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

Чтобы узнать, как интегрировать в DB с помощью OAuth и Настройка ролей для ограничения доступа пользователей к приложению, см. в разделе [развертывание безопасного приложения ASP.NET MVC с членством, OAuth и базой данных SQL в Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).
