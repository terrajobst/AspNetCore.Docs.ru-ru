---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: 'ASP.NET Identity: Использование хранилища MySQL с помощью поставщика EntityFramework MySQL (C#) | Документация Майкрософт'
author: maumar
description: Этом руководстве показано, как замените механизм хранения данных по умолчанию для ASP.NET Identity EntityFramework (поставщик клиента SQL) — обеспечить MySQL...
ms.author: riande
ms.date: 12/10/2013
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: 3856b40b31a3deb6ad1c6c5d2cd678e183f012d7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837349"
---
<a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a>ASP.NET Identity: Использование хранилища MySQL с помощью поставщика EntityFramework MySQL (C#)
====================
по [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [(Robert McMurray)](https://github.com/rmcmurray)

> Этом руководстве показано, как заменить механизм хранения данных по умолчанию для [ **ASP.NET Identity** ](introduction-to-aspnet-identity.md) с EntityFramework (поставщик клиента SQL) с поставщиком MySQL.


В этом руководстве рассматриваются следующие темы:

- Создание базы данных MySQL в Azure
- Создание приложения MVC с помощью шаблона Visual Studio 2013 MVC
- Настройка EntityFramework, чтобы в сотрудничестве с поставщиком базы данных MySQL
- Запуск приложения для проверки результатов

В конце этого руководства необходимо хранить приложения MVC с удостоверением ASP.NET, использующего базу данных MySQL, размещенной в Azure.

## <a name="creating-a-mysql-database-instance-on-azure"></a>Создание экземпляра базы данных MySQL в Azure

1. Войдите на [портал Azure](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).
2. Нажмите кнопку **NEW** в нижней части страницы, а затем выберите **ХРАНИЛИЩЕ**:  
  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. В **Выбор и надстройки** мастера выберите **база данных ClearDB MySQL**, а затем нажмите кнопку **Далее** стрелку в нижней части окна:  
  
   [Щелкните следующее изображение, чтобы развернуть его. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. Сохраните значение по умолчанию **бесплатный** план, изменить **имя** для **IdentityMySQLDatabase**, выберите регион, который находится ближе всего к вам и нажмите кнопку **Далее** стрелку в нижней части окна:  
  
   [Щелкните следующее изображение, чтобы развернуть его. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. Нажмите кнопку **ПОКУПКИ** флажок, чтобы завершить создание базы данных.  
  
   [Щелкните следующее изображение, чтобы развернуть его. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. После создания базы данных, можно было управлять из **надстройки** вкладка на портале управления. Чтобы получить сведения о соединении для базы данных, щелкните **сведения о ПОДКЛЮЧЕНИИ** в нижней части страницы:  
  
   [Щелкните следующее изображение, чтобы развернуть его. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. Скопируйте строку подключения, нажав кнопку "Копировать", **CONNECTIONSTRING** поле и сохраните его; эти сведения далее в этом учебнике будет использоваться для своего приложения MVC:  
  
   [Щелкните следующее изображение, чтобы развернуть его. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a>Создав проект приложения MVC

Чтобы выполнить действия, описанные в этом разделе руководства, необходимо сначала установить [Visual Studio Express 2013 для Web](https://go.microsoft.com/fwlink/?LinkId=299058) или [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). После установки Visual Studio, следуйте инструкциям ниже, чтобы создать новый проект приложения MVC:

1. Откройте Visual Studio 2103.
2. Нажмите кнопку **новый проект** из **запустить** страницы, или же можно нажать **файл** меню и затем **новый проект**:  
  
   [Щелкните следующее изображение, чтобы развернуть его. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. При **новый проект** диалоговое окно отображается, разверните **Visual C#** в списке шаблонов выберите **Web**и выберите **веб-приложение ASP.NET**. Присвойте проекту имя **IdentityMySQLDemo** и нажмите кнопку **ОК**:  
  
   [Щелкните следующее изображение, чтобы развернуть его. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. В **новый проект ASP.NET** диалоговом окне выберите **MVC** templatewith значения по умолчанию; значение может настроить **учетные записи отдельных пользователей** метод проверки подлинности. Нажмите кнопку **ОК**.  
  
   [Щелкните следующее изображение, чтобы развернуть его. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a>Настройка EntityFramework, чтобы работать с базой данных MySQL

### <a name="update-the-entity-framework-assembly-for-your-project"></a>Обновление сборки платформы Entity Framework для проекта

Приложение MVC, который был создан из шаблона Visual Studio 2013 содержит ссылку на [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) пакета, но должно было на эту сборку, с момента выпуска обновлений, которые содержат значительные Повышение производительности. Чтобы использовать эти последние обновления в приложении, выполните следующие действия.

1. Откройте проект MVC в Visual Studio 2013.
2. Нажмите кнопку **средства**, нажмите кнопку **диспетчер пакетов библиотеки**, а затем нажмите кнопку **консоль диспетчера пакетов**:  
  
   [Щелкните следующее изображение, чтобы развернуть его. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. **Консоль диспетчера пакетов** будет отображаться в нижней части Visual Studio. Тип &quot; **EntityFramework Update-Package** &quot; и нажмите клавишу ВВОД:  
  
   [Щелкните следующее изображение, чтобы развернуть его. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a>Установите поставщик MySQL для EntityFramework

Чтобы EntityFramework, чтобы подключиться к базе данных MySQL необходимо установить поставщик MySQL. Чтобы сделать это, откройте **консоль диспетчера пакетов** и тип &quot; **MySql.Data.Entity Install-Package - Pre**&quot;, и нажмите клавишу ВВОД.

> [!NOTE]
> Это предварительная версия сборки, и таким образом, он может содержать ошибок. Предварительная версия поставщика не следует использовать в рабочей среде.


[Щелкните следующее изображение, чтобы развернуть его.]  
  
[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a>Изменения конфигурации проекта в файл Web.config для приложения

В этом разделе вы настроите Entity Framework для использования поставщика MySQL, который вы только что установили зарегистрировать фабрику поставщика MySQL и добавить строку подключения из Azure.

> [!NOTE]
> Следующие примеры содержат конкретную версию сборки для MySql.Data.dll. При изменении версии сборки, необходимо изменить соответствующие параметры конфигурации с нужной версией.


1. Откройте файл Web.config для проекта в Visual Studio 2013.
2. Найдите следующие параметры конфигурации, которые определяют поставщик базы данных по умолчанию и фабрики для Entity Framework:

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. Замените эти параметры конфигурации следующим образом, который сконфигурирует Entity Framework для использования поставщика MySQL: 

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. Найдите &lt;connectionStrings&gt; разделе и замените его следующим кодом, который будет определять строку подключения для базы данных MySQL, размещенной в Azure (Обратите внимание, что значение providerName также было изменено с исходное значение):

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a>Добавление пользовательского контекста MigrationHistory

Использует Entity Framework Code First **MigrationHistory** таблицы для отслеживания изменений модели и для обеспечения согласованности между концептуальной схеме и схеме базы данных. Тем не менее эта таблица не работает для MySQL по умолчанию из-за слишком большого первичный ключ. Чтобы исправить эту ситуацию, необходимо уменьшить размер ключа для этой таблицы. Чтобы сделать это, следуйте инструкциям ниже:

1. Сведения о схеме для этой таблицы был захвачен в **HistoryContext**, который может быть изменен, как любой другой **DbContext**. Чтобы сделать это, добавьте новый файл класса с именем **MySqlHistoryContext.cs** в проект и замените его содержимое следующим кодом:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. Далее необходимо настроить Entity Framework использовать измененный **HistoryContext**, а не по умолчанию. Это можно сделать путем использования функций, конфигурация на основе кода. Чтобы сделать это, добавьте новый файл класса с именем **MySqlConfiguration.cs** в проект и замените его содержимое с помощью:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a>Создание настраиваемого инициализатора EntityFramework для ApplicationDbContext

Поставщик MySQL, представленную в этом учебнике не поддерживает миграций Entity Framework, поэтому вам нужно будет использовать инициализаторы модели для подключения к базе данных. Так как в этом руководстве используется экземпляр MySQL в Azure, необходимо будет создать пользовательский инициализатор Entity Framework.

> [!NOTE]
> Этот шаг не является обязательным, если вы подключаетесь к экземпляру SQL Server в Azure или если вы используете базу данных, размещенной на локальном компьютере.


Чтобы создать пользовательский инициализатор Entity Framework для MySQL, сделайте следующее:

1. Добавьте новый файл класса с именем **MySqlInitializer.cs** к проекту и заменить это содержимое следующим кодом: 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. Откройте **IdentityModels.cs** файл проекта, который находится в **моделей** directory и замените его содержимое следующим кодом: 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a>Запуск приложения и проверка базы данных

После завершения действия, описанные в предыдущих разделах, следует протестировать базы данных. Чтобы сделать это, следуйте инструкциям ниже:

1. Нажмите клавишу **Ctrl + F5** для сборки и запуска веб-приложения.
2. Нажмите кнопку **зарегистрировать** вкладки в верхней части страницы:  
  
   [Щелкните следующее изображение, чтобы развернуть его. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. Введите новое имя пользователя и пароль, а затем нажмите кнопку **зарегистрировать**:  
  
   [Щелкните следующее изображение, чтобы развернуть его. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. На этом этапе ASP.NET Identity таблицы создаются в базе данных MySQL и регистрации и входа в приложение пользователя:  
  
   [Щелкните следующее изображение, чтобы развернуть его. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a>Установка средства MySQL Workbench, чтобы проверить данные

1. Установка **MySQL Workbench** средство из [страницу загрузок MySQL](http://dev.mysql.com/downloads/windows/installer/)
2. В мастере установки: **Выбор компонентов** выберите **MySQL Workbench** под **приложений** раздел.
3. Запустите приложение и добавить новое подключение с помощью подключения строковые данные из базы данных Azure MySQL на приглашение этого учебника.
4. После установления соединения следует проверять **ASP.NET Identity** таблицы, созданные на **IdentityMySQLDatabase.**
5. Вы увидите, что все удостоверения ASP.NET необходимые таблицы создаются в том случае, как показано на рисунке ниже:  
  
   [Щелкните следующее изображение, чтобы развернуть его. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. Проверьте **aspnetusers** таблицы для экземпляра на наличие записей по мере регистрации новых пользователей.  
  
   [Щелкните следующее изображение, чтобы развернуть его. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)
