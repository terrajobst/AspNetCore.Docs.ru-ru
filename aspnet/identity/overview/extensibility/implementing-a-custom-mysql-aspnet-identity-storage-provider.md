---
uid: identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
title: Реализация поставщика хранилища пользовательских MySQL ASP.NET Identity | Документы Microsoft
author: raquelsa
description: ASP.NET Identity является расширяемой системой, что дает возможность создать собственный поставщик хранилища и вставьте его в приложение без переработки приложения...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: 248f5fe7-39ba-40ea-ab1e-71a69b0bd649
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
msc.type: authoredcontent
ms.openlocfilehash: d843b31e011fe520aad6cfdab0beca2d12477f12
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="implementing-a-custom-mysql-aspnet-identity-storage-provider"></a>Реализация поставщика хранилища ASP.NET Identity пользовательских MySQL
====================
по [Raquel Soares De Almeida](https://github.com/raquelsa), [Suhas Joshi](https://github.com/suhasj), [Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET Identity является расширяемой системой, что дает возможность создать собственный поставщик хранилища и вставьте его в приложение без переработки приложения. В этом разделе описывается создание поставщика хранилища MySQL для ASP.NET Identity. Обзор создания поставщиков пользовательского хранилища см. в разделе [Обзор из пользовательских поставщиков хранилища для ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).
> 
> Для работы с этим учебником требуются Visual Studio 2013 с обновлением 2.
> 
> Этот учебник выполняет следующие действия:
> 
> - Показано, как создать экземпляр базы данных MySQL в Azure.
> - Показано, как использовать MySQL клиентского средства (MySQL Workbench) для создания таблиц и управления удаленной базы данных в Azure.
> - Показано, как заменить значение по умолчанию реализация хранилища ASP.NET Identity реализацию пользовательского на проект MVC-приложения.
> 
> Этот учебник изначально был записан с Raquel Soares De Almeida и Рик Андерсон ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ). Пример проекта был обновлен для идентификации 2.0 по Suhas Joshi. Раздел был обновлен для идентификации 2.0, Tom FitzMacken.


## <a name="download-completed-project"></a>Процент загрузки проекта

В конце этого учебника вы получите проект MVC-приложения с ASP.NET Identity, работать с базой данных MySQL, размещенные в Azure.

Можно загрузить поставщик хранилища завершенного MySQL в [AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/).

## <a name="the-steps-you-will-perform"></a>Действия, которое будет выполнять

В этом учебнике будет:

1. Создание базы данных MySQL в Azure
2. Создание таблиц ASP.NET Identity в MySQL
3. Создание приложения MVC и настройте его для использования поставщика MySQL
4. Запуск приложения

В этом разделе описывается архитектура ASP.NET Identity и решений, которые необходимо принять при реализации поставщика хранилища клиента. Эти сведения в разделе [Обзор из пользовательских поставщиков хранилища для ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).

## <a name="review-mysql-storage-provider-classes"></a>Просмотрите классы поставщиков для хранения данных MySQL

Перед обращением к инструкции по созданию поставщика хранилища MySQL, давайте взглянем на классы, входящие в состав поставщика хранилища. Необходимо будет классы, которые управляют операциями базы данных и классы, которые вызываются из приложения для управления пользователями и ролями.

### <a name="storage-classes"></a>Классы хранения

- [IdentityUser](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs) -содержит свойства для пользователя.
- [UserStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs) -содержит операции для добавления, обновления или получение пользователей.
- [IdentityRole](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs) -содержит свойства для ролей.
- [RoleStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs) -содержит операции для добавления, удаления, обновления и получения ролей.

### <a name="data-access-layer-classes"></a>Классы слой доступа к данным

В этом примере классов слой доступа к данным содержит инструкции SQL для работы с таблицами; Однако в коде можно использовать объектно реляционного сопоставления (ORM), такие как Entity Framework и NHibernate. В частности приложение может наблюдаться низкая производительность без ORM, включающий отложенную загрузку и кэширование объекта. Дополнительные сведения см. в разделе [ASP.NET 2.0 удостоверений без Entity Framework?](https://aspnetidentity.codeplex.com/discussions/561828)

- [MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) -содержит подключения к базе данных MySQL и методы для операций базы данных. UserStore и RoleStore создаются с помощью экземпляра этого класса.
- [RoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) -содержит операции с базой данных для таблицы, которая хранит ролей.
- [UserClaimsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) -содержит операции с базой данных для таблицы, которая хранит утверждения пользователей.
- [UserLoginsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) -содержит операции с базой данных для таблицы, в которой хранятся сведения об имени входа пользователя.
- [UserRoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) -содержит операции с базой данных для таблицы, которая содержит пользователей, которым назначены роли, к которым.
- [UserTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) -содержит операции для таблицы, которая содержит пользователей с базой данных.

## <a name="create-a-mysql-database-instance-on-azure"></a>Создайте экземпляр базы данных MySQL в Azure

1. Войдите на [портал Azure](https://manage.windowsazure.com/).
2. Нажмите кнопку **+ создать** в нижней части страницы, а затем выберите **ХРАНИЛИЩЕ**.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.png)
3. В **Выбор и надстройки** мастера выберите **базы данных MySQL ClearDB** и щелкните стрелку в правом нижнем углу диалогового окна.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.png)
4. Оставьте значение по умолчанию **Free** планирования и изменить **имя** для **IdentityMySQLDatabase**. Выберите ближайший регион и щелкните стрелку «Далее».  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.png)
5. Установите флажок для завершения создания базы данных.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image4.png)
6. После создания базы данных можно было управлять из **надстройки** на портале управления.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image5.png)
7. Соединение с базой данных можно получить, щелкнув **сведений о СОЕДИНЕНИИ** в нижней части страницы.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image6.png)
8. Скопируйте строку подключения, нажав кнопку "Копировать" и сохраните его, чтобы можно было использовать позже в приложении MVC.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image7.png)

## <a name="create-the-aspnet-identity-tables-in-a-mysql-database"></a>Создание таблиц ASP.NET Identity в базе данных MySQL

### <a name="install-mysql-workbench-tool-to-connect-and-manage-mysql-database"></a>Установите средство MySQL Workbench для подключения и управления базой данных MySQL

1. Установка **MySQL Workbench** средства из [MySQL файлов для загрузки](http://dev.mysql.com/downloads/windows/installer/)
2. Запустите приложение и добавить, выберите на **MySQLConnections +** кнопку, чтобы добавить новое подключение. Используйте данные строки соединения, которые копируются из базы данных MySQL в Azure, созданный ранее в этом учебнике.
3. После установления соединения, откройте новый **запроса** вкладка; команды из вставки [MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql) в окно запроса и выполните его для создания таблиц базы данных.
4. Теперь у вас есть все ASP.NET Identity необходимые таблицы, созданные в базе данных MySQL, размещенные в Azure, как показано ниже.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.jpg)

## <a name="create-an-mvc-application-project-from-template-and-configure-it-to-use-mysql-provider"></a>Создать проект MVC-приложения на основе шаблона и настроить его для использования поставщика MySQL

При необходимости установите [Visual Studio Express 2013 для Web](https://go.microsoft.com/fwlink/?LinkId=299058) или [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) с обновлением 2.

### <a name="download-the-aspnetidentitymysql-project-from-codeplex"></a>Загрузите проект ASP.NET.Identity.MySQL из CodePlex

1. Перейдите к URL-адрес репозитория в [AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/).
2. Загрузка исходного кода.
3. Извлеките ZIP-файл в локальную папку.
4. Откройте решение AspNet.Identity.MySQL и постройте его.

### <a name="create-a-new-mvc-application-project-from-template"></a>Создайте новый проект MVC-приложения на основе шаблона

1. Щелкните правой кнопкой мыши **AspNet.Identity.MySQL** решения и **добавить**, **нового проекта**
2. В **Добавление нового проекта** окна выберите **Visual C#** слева, затем **Web** , а затем выберите **веб-приложение ASP.NET**. Присвойте проекту имя **IdentityMySQLDemo**; и нажмите кнопку ОК.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.jpg)
3. В **новый проект ASP.NET** диалоговое окно, выберите шаблон MVC с параметрами по умолчанию (включающий **отдельных учетных записей пользователей** как метод проверки подлинности) и нажмите кнопку **ОК** .![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.jpg)
4. В обозревателе решений щелкните правой кнопкой мыши проект IdentityMySQLDemo и выберите **управление пакетами NuGet**. В диалоговом окне поле текст поиска, введите **Identity.EntityFramework**. Выберите этот пакет в списке результатов и выберите **удаления**. Вам будет предложено удаления зависимостей пакета EntityFramework. Щелкните Да, мы больше не будет этого пакета в этом приложении.
5. Щелкните правой кнопкой мыши проект IdentityMySQLDemo, выберите **добавить**, **ссылку, решения, проекты;** выберите AspNet.Identity.MySQL проекта и нажмите кнопку **ОК**.
6. В проекте IdentityMySQLDemo замените все вхождения  
    `using Microsoft.AspNet.Identity.EntityFramework;`  
   на  
     `using AspNet.Identity.MySQL;`
7. В IdentityModels.cs, задайте **ApplicationDbContext** для наследования от **MySqlDatabase** и включает конструктор, который принимает один параметр с именем подключения.  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample1.cs)]
8. Откройте файл IdentityConfig.cs. В **ApplicationUserManager.Create** метод, replace, при создании экземпляра UserManager следующим кодом:  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample2.cs)]
9. Откройте файл web.config и замените строку, DefaultConnection эту запись, заменив значения выделенная строка подключения базы данных MySQL, созданный на предыдущих шагах.  

    [!code-xml[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample3.xml?highlight=2)]

## <a name="run-the-app-and-connect-to-the-mysql-db"></a>Запустите приложение и подключитесь к базе данных MySQL

1. Щелкните правой кнопкой мыши **IdentityMySQLDemo** проект и выберите **Назначить запускаемым проектом**
2. Нажмите клавишу **сочетание клавиш Ctrl + F5** для построения и запуска приложения.
3. Щелкните **зарегистрировать** вкладки в верхней части страницы.
4. Введите новое имя пользователя и пароль и нажмите кнопку на **зарегистрировать**.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image8.png)
5. Новый пользователь теперь зарегистрирован и вход.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image9.png)
6. Вернитесь в средство MySQL Workbench и проверять **IdentityMySQLDatabase** содержимое таблицы. Проверьте таблицу пользователей для операций по мере регистрации новых пользователей.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image10.png)

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения о включении других методов проверки подлинности на это приложение посвящены [создать приложение ASP.NET MVC 5 с Facebook и Google OAuth2 и входа OpenID](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

Чтобы узнать, как интегрировать базы данных с помощью OAuth и настроить роли, чтобы ограничить доступ пользователей к приложению, см. [развернуть приложение для защиты ASP.NET MVC 5 с членством, OAuth и базы данных SQL на Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).
