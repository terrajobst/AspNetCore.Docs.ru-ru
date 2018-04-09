---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: 'ASP.NET Identity: С помощью MySQL хранилища с поставщиком MySQL EntityFramework (C#) | Документы Microsoft'
author: maumar
description: Этот учебник показывает, как заменить механизм хранения данных по умолчанию для ASP.NET Identity EntityFramework (поставщик клиента SQL) с обеспечить MySQL...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/10/2013
ms.topic: article
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: 6018b4f62f95f9abffece536f345d7a16d052aac
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a>ASP.NET Identity: С помощью MySQL хранилища с поставщиком EntityFramework MySQL (C#)
====================
по [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Robert McMurray](https://github.com/rmcmurray)

> Этого учебника показано, как заменить механизм хранения данных по умолчанию для [ **ASP.NET Identity** ](introduction-to-aspnet-identity.md) с EntityFramework (поставщик клиента SQL) с поставщиком MySQL.


В этом учебнике рассматриваются следующие темы:

- Создание базы данных MySQL в Azure
- Создание приложения MVC с помощью шаблона Visual Studio 2013 MVC
- Настройка EntityFramework для работы с поставщиком базы данных MySQL
- Запуск приложения для проверки результатов

В конце этого учебника имеется приложения MVC с ASP.NET Identity сохранения, использующего базу данных MySQL, размещенной в Azure.

## <a name="creating-a-mysql-database-instance-on-azure"></a>Создание экземпляра базы данных MySQL в Azure

1. Войдите на [портал Azure](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).
2. Нажмите кнопку **NEW** в нижней части страницы, а затем выберите **ХРАНИЛИЩА**:  
  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. В **Выбор и надстройки** мастера выберите **базы данных MySQL ClearDB**и нажмите кнопку **Далее** стрелку в нижней части области:  
  
   [Щелкните следующие изображение, чтобы развернуть его. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. Оставьте значение по умолчанию **Free** плана, измените **имя** для **IdentityMySQLDatabase**, выберите регион, ближайшего к вам и нажмите кнопку **Далее** стрелку в нижней части области:  
  
   [Щелкните следующие изображение, чтобы развернуть его. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. Нажмите кнопку **ПОКУПКИ** флажок для завершения создания базы данных.  
  
   [Щелкните следующие изображение, чтобы развернуть его. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. После создания базы данных можно было управлять из **надстройки** на портале управления. Чтобы получить сведения о соединении для базы данных, щелкните **сведений о СОЕДИНЕНИИ** в нижней части страницы:  
  
   [Щелкните следующие изображение, чтобы развернуть его. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. Скопируйте строку подключения, нажав кнопку "Копировать", **CONNECTIONSTRING** поля и сохраните его; эти сведения далее в этом учебнике будет использоваться для приложения MVC:  
  
   [Щелкните следующие изображение, чтобы развернуть его. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a>Создав проект приложения MVC

Для выполнения шагов в этом разделе учебника, сначала необходимо установить [Visual Studio Express 2013 для Web](https://go.microsoft.com/fwlink/?LinkId=299058) или [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). После установки Visual Studio, выполните следующие действия, чтобы создать новый проект MVC-приложения:

1. Откройте Visual Studio 2103.
2. Нажмите кнопку **новый проект** из **запустить** страницы, или же можно нажать **файл** меню и затем **новый проект**:  
  
   [Щелкните следующие изображение, чтобы развернуть его. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. При **новый проект** диалоговое окно, разверните узел **Visual C#** в списке шаблонов выберите **Web**и выберите **веб-приложение ASP.NET**. Присвойте проекту имя **IdentityMySQLDemo** и нажмите кнопку **ОК**:  
  
   [Щелкните следующие изображение, чтобы развернуть его. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. В **новый проект ASP.NET** диалогового окна выберите **MVC** templatewith значения по умолчанию; при этом будут Настройка **индивидуальные учетные записи** в качестве метода проверки подлинности. Нажмите кнопку **ОК**:  
  
   [Щелкните следующие изображение, чтобы развернуть его. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a>Настройка EntityFramework для работы с базой данных MySQL

### <a name="update-the-entity-framework-assembly-for-your-project"></a>Обновление сборки платформы Entity Framework для проекта

Приложение MVC, который был создан из шаблона Visual Studio 2013 содержит ссылку на [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) пакета, но имеют были на эту сборку, с момента выпуска обновлений, которые содержат значительные Повышение производительности. Чтобы использовать эти последние обновления в приложение, выполните следующие действия.

1. Откройте проект MVC в Visual Studio 2013.
2. Нажмите кнопку **средства**, нажмите кнопку **диспетчер пакетов библиотеки**, а затем нажмите кнопку **консоль диспетчера пакетов**:  
  
   [Щелкните следующие изображение, чтобы развернуть его. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. **Консоль диспетчера пакетов** появятся в нижней части окна Visual Studio. Тип &quot; **EntityFramework пакет обновления** &quot; и нажмите клавишу ВВОД:  
  
   [Щелкните следующие изображение, чтобы развернуть его. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a>Установка MySQL поставщика для EntityFramework

Чтобы EntityFramework для подключения к базе данных MySQL необходимо установить поставщик MySQL. Чтобы сделать это, откройте **консоль диспетчера пакетов** и тип &quot; **MySql.Data.Entity Install-Package - Pre**&quot;, и нажмите клавишу ВВОД.

> [!NOTE]
> Это предварительная версия сборки, и таким образом, он может содержать ошибки. Предварительная версия поставщика не следует использовать в рабочей среде.


[Щелкните следующем рисунке, чтобы развернуть его.]  
  
[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a>Внесение изменений конфигурации проекта в файл Web.config для приложения

В этом разделе вы настроите Entity Framework для использования поставщика MySQL, который был только что установлен зарегистрировать фабрику поставщика MySQL и добавить строки подключения из Azure.

> [!NOTE]
> Следующие примеры содержат конкретную версию сборки для MySql.Data.dll. При изменении версии сборки, необходимо изменить параметры соответствующих настроек с нужной версией.


1. Откройте файл Web.config для проекта в Visual Studio 2013.
2. Найдите следующие параметры конфигурации, которые определяют поставщика базы данных по умолчанию и фабрики для платформы Entity Framework:

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. Эти параметры конфигурации, замените следующую команду, настроенной для использования поставщика MySQL Entity Framework: 

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. Найдите &lt;connectionStrings&gt; статьи и замените его следующим кодом, который будет определять строку подключения для базы данных MySQL, размещенной в Azure (Обратите внимание, что значение providerName также было изменено с исходное значение):

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a>Добавление пользовательского контекста MigrationHistory

Entity Framework Code First использует **MigrationHistory** таблицы для отслеживания изменений модели и для обеспечения согласованности между концептуальной схемы и схемы базы данных. Тем не менее эта таблица не работает для MySQL по умолчанию, так как первичный ключ слишком велик. Чтобы исправить эту ситуацию, необходимо уменьшить размер ключа для этой таблицы. Чтобы сделать это, выполните следующие действия:

1. Сведения схемы для этой таблицы сохраняются в **HistoryContext**, который может быть изменен, как любой другой **DbContext**. Чтобы сделать это, добавьте новый файл класса с именем **MySqlHistoryContext.cs** в проект и замените его содержимое следующим кодом:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. Далее необходимо настроить использование измененного Entity Framework **HistoryContext**, а не по умолчанию. Это можно сделать с помощью функции настройки на основе кода. Чтобы сделать это, добавьте новый файл класса с именем **MySqlConfiguration.cs** в проект и заменить его содержимое:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a>Создание настраиваемого инициализатора для ApplicationDbContext EntityFramework

Поставщик MySQL, представлено в этом учебнике не поддерживает миграции Entity Framework, вам потребуется использовать инициализаторы модели для подключения к базе данных. Так как этот учебник использует экземпляр MySQL в Azure, необходимо создать настраиваемого инициализатора Entity Framework.

> [!NOTE]
> Этот шаг не является обязательным при подключении к экземпляру SQL Server на Azure или при использовании базы данных, который размещается на локальном компьютере.


Для создания пользовательского инициализатора Entity Framework для MySQL, выполните следующие действия:

1. Добавьте новый файл класса с именем **MySqlInitializer.cs** в проект и заменить это содержимое следующим кодом: 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. Откройте **IdentityModels.cs** файл проекта, который находится в **моделей** каталога и замените его содержимое следующим кодом: 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a>Запуск приложения и проверка базы данных

После завершения действия, описанные в предыдущих разделах, следует проверить базу данных. Чтобы сделать это, выполните следующие действия:

1. Нажмите клавишу **сочетание клавиш Ctrl + F5** для построения и запуска веб-приложения.
2. Нажмите кнопку **зарегистрировать** вкладки в верхней части страницы:  
  
   [Щелкните следующие изображение, чтобы развернуть его. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. Введите новое имя пользователя и пароль, а затем нажмите кнопку **зарегистрировать**:  
  
   [Щелкните следующие изображение, чтобы развернуть его. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. На этом этапе ASP.NET Identity таблицы создаются в базе данных MySQL, а пользователь зарегистрирован и входа в приложение:  
  
   [Щелкните следующие изображение, чтобы развернуть его. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a>Установка MySQL Workbench средства для проверки данных

1. Установка **MySQL Workbench** средства из [MySQL файлов для загрузки](http://dev.mysql.com/downloads/windows/installer/)
2. В мастере установки: **Выбор компонентов** выберите **MySQL Workbench** под **приложений** раздела.
3. Запустите приложение и добавьте новое подключение с помощью данных строки подключения из базы данных MySQL в Azure, созданное в приглашение этого учебника.
4. После подключения к проверки **ASP.NET Identity** таблицы, созданные на **IdentityMySQLDatabase.**
5. Вы увидите, что все ASP.NET Identity необходимые таблицы создаются, как показано на рисунке ниже:  
  
   [Щелкните следующие изображение, чтобы развернуть его. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. Проверить **aspnetusers** таблицы для экземпляра на наличие записей, по мере регистрации новых пользователей.  
  
   [Щелкните следующие изображение, чтобы развернуть его. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)
