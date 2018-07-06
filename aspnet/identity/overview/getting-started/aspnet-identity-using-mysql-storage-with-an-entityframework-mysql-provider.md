---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: 'ASP.NET Identity: Использование хранилища MySQL с помощью поставщика EntityFramework MySQL (C#) | Документация Майкрософт'
author: maumar
description: Этом руководстве показано, как замените механизм хранения данных по умолчанию для ASP.NET Identity EntityFramework (поставщик клиента SQL) — обеспечить MySQL...
ms.author: aspnetcontent
ms.date: 12/10/2013
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: 13cb1d9a862429bfb4e20a8d9700fc17eb721c6b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812679"
---
<a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a><span data-ttu-id="abc94-103">ASP.NET Identity: Использование хранилища MySQL с помощью поставщика EntityFramework MySQL (C#)</span><span class="sxs-lookup"><span data-stu-id="abc94-103">ASP.NET Identity: Using MySQL Storage with an EntityFramework MySQL Provider (C#)</span></span>
====================
<span data-ttu-id="abc94-104">по [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [(Robert McMurray)](https://github.com/rmcmurray)</span><span class="sxs-lookup"><span data-stu-id="abc94-104">by [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Robert McMurray](https://github.com/rmcmurray)</span></span>

> <span data-ttu-id="abc94-105">Этом руководстве показано, как заменить механизм хранения данных по умолчанию для [ **ASP.NET Identity** ](introduction-to-aspnet-identity.md) с EntityFramework (поставщик клиента SQL) с поставщиком MySQL.</span><span class="sxs-lookup"><span data-stu-id="abc94-105">This tutorial shows you how to replace the default data storage mechanism for [**ASP.NET Identity**](introduction-to-aspnet-identity.md) with EntityFramework (SQL client provider) with a MySQL provider.</span></span>


<span data-ttu-id="abc94-106">В этом руководстве рассматриваются следующие темы:</span><span class="sxs-lookup"><span data-stu-id="abc94-106">The following topics will be covered in this tutorial:</span></span>

- <span data-ttu-id="abc94-107">Создание базы данных MySQL в Azure</span><span class="sxs-lookup"><span data-stu-id="abc94-107">Creating a MySQL database on Azure</span></span>
- <span data-ttu-id="abc94-108">Создание приложения MVC с помощью шаблона Visual Studio 2013 MVC</span><span class="sxs-lookup"><span data-stu-id="abc94-108">Creating an MVC application using Visual Studio 2013 MVC template</span></span>
- <span data-ttu-id="abc94-109">Настройка EntityFramework, чтобы в сотрудничестве с поставщиком базы данных MySQL</span><span class="sxs-lookup"><span data-stu-id="abc94-109">Configuring EntityFramework to work with a MySQL database provider</span></span>
- <span data-ttu-id="abc94-110">Запуск приложения для проверки результатов</span><span class="sxs-lookup"><span data-stu-id="abc94-110">Running the application to verify the results</span></span>

<span data-ttu-id="abc94-111">В конце этого руководства необходимо хранить приложения MVC с удостоверением ASP.NET, использующего базу данных MySQL, размещенной в Azure.</span><span class="sxs-lookup"><span data-stu-id="abc94-111">At the end of this tutorial, you will have an MVC application with the ASP.NET Identity store that is using a MySQL database that is hosted in Azure.</span></span>

## <a name="creating-a-mysql-database-instance-on-azure"></a><span data-ttu-id="abc94-112">Создание экземпляра базы данных MySQL в Azure</span><span class="sxs-lookup"><span data-stu-id="abc94-112">Creating a MySQL database instance on Azure</span></span>

1. <span data-ttu-id="abc94-113">Войдите на [портал Azure](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="abc94-113">Log in to the [Azure Portal](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).</span></span>
2. <span data-ttu-id="abc94-114">Нажмите кнопку **NEW** в нижней части страницы, а затем выберите **ХРАНИЛИЩЕ**:</span><span class="sxs-lookup"><span data-stu-id="abc94-114">Click **NEW** at the bottom of the page, and then select **STORE**:</span></span>  
  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. <span data-ttu-id="abc94-115">В **Выбор и надстройки** мастера выберите **база данных ClearDB MySQL**, а затем нажмите кнопку **Далее** стрелку в нижней части окна:</span><span class="sxs-lookup"><span data-stu-id="abc94-115">In the **Choose and Add-on** wizard, select **ClearDB MySQL Database**, and then click the **Next** arrow at the bottom of the frame:</span></span>  
  
   <span data-ttu-id="abc94-116">[Щелкните следующее изображение, чтобы развернуть его.</span><span class="sxs-lookup"><span data-stu-id="abc94-116">[Click the following image to expand it.</span></span> <span data-ttu-id="abc94-117">]</span><span class="sxs-lookup"><span data-stu-id="abc94-117">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. <span data-ttu-id="abc94-118">Сохраните значение по умолчанию **бесплатный** план, изменить **имя** для **IdentityMySQLDatabase**, выберите регион, который находится ближе всего к вам и нажмите кнопку **Далее** стрелку в нижней части окна:</span><span class="sxs-lookup"><span data-stu-id="abc94-118">Keep the default **Free** plan, change the **NAME** to **IdentityMySQLDatabase**, select the region that is nearest to you, and then click the **Next** arrow at the bottom of the frame:</span></span>  
  
   <span data-ttu-id="abc94-119">[Щелкните следующее изображение, чтобы развернуть его.</span><span class="sxs-lookup"><span data-stu-id="abc94-119">[Click the following image to expand it.</span></span> <span data-ttu-id="abc94-120">]</span><span class="sxs-lookup"><span data-stu-id="abc94-120">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. <span data-ttu-id="abc94-121">Нажмите кнопку **ПОКУПКИ** флажок, чтобы завершить создание базы данных.</span><span class="sxs-lookup"><span data-stu-id="abc94-121">Click the **PURCHASE** checkmark to complete the database creation.</span></span>  
  
   <span data-ttu-id="abc94-122">[Щелкните следующее изображение, чтобы развернуть его.</span><span class="sxs-lookup"><span data-stu-id="abc94-122">[Click the following image to expand it.</span></span> <span data-ttu-id="abc94-123">]</span><span class="sxs-lookup"><span data-stu-id="abc94-123">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. <span data-ttu-id="abc94-124">После создания базы данных, можно было управлять из **надстройки** вкладка на портале управления.</span><span class="sxs-lookup"><span data-stu-id="abc94-124">After your database has been created, you can manage it from the **ADD-ONS** tab in the management portal.</span></span> <span data-ttu-id="abc94-125">Чтобы получить сведения о соединении для базы данных, щелкните **сведения о ПОДКЛЮЧЕНИИ** в нижней части страницы:</span><span class="sxs-lookup"><span data-stu-id="abc94-125">To retrieve the connection information for your database, click **CONNECTION INFO** at the bottom of the page:</span></span>  
  
   <span data-ttu-id="abc94-126">[Щелкните следующее изображение, чтобы развернуть его.</span><span class="sxs-lookup"><span data-stu-id="abc94-126">[Click the following image to expand it.</span></span> <span data-ttu-id="abc94-127">]</span><span class="sxs-lookup"><span data-stu-id="abc94-127">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. <span data-ttu-id="abc94-128">Скопируйте строку подключения, нажав кнопку "Копировать", **CONNECTIONSTRING** поле и сохраните его; эти сведения далее в этом учебнике будет использоваться для своего приложения MVC:</span><span class="sxs-lookup"><span data-stu-id="abc94-128">Copy the connection string by clicking on the copy button by the **CONNECTIONSTRING** field and save it; you will use this information later in this tutorial for your MVC application:</span></span>  
  
   <span data-ttu-id="abc94-129">[Щелкните следующее изображение, чтобы развернуть его.</span><span class="sxs-lookup"><span data-stu-id="abc94-129">[Click the following image to expand it.</span></span> <span data-ttu-id="abc94-130">]</span><span class="sxs-lookup"><span data-stu-id="abc94-130">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a><span data-ttu-id="abc94-131">Создав проект приложения MVC</span><span class="sxs-lookup"><span data-stu-id="abc94-131">Creating an MVC application project</span></span>

<span data-ttu-id="abc94-132">Чтобы выполнить действия, описанные в этом разделе руководства, необходимо сначала установить [Visual Studio Express 2013 для Web](https://go.microsoft.com/fwlink/?LinkId=299058) или [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="abc94-132">To complete the steps in this section of the tutorial, you will first need to install [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="abc94-133">После установки Visual Studio, следуйте инструкциям ниже, чтобы создать новый проект приложения MVC:</span><span class="sxs-lookup"><span data-stu-id="abc94-133">Once Visual Studio has been installed, use the following steps to create a new MVC application project:</span></span>

1. <span data-ttu-id="abc94-134">Откройте Visual Studio 2103.</span><span class="sxs-lookup"><span data-stu-id="abc94-134">Open Visual Studio 2103.</span></span>
2. <span data-ttu-id="abc94-135">Нажмите кнопку **новый проект** из **запустить** страницы, или же можно нажать **файл** меню и затем **новый проект**:</span><span class="sxs-lookup"><span data-stu-id="abc94-135">Click **New Project** from the **Start** page, or you can click the **File** menu and then **New Project**:</span></span>  
  
   <span data-ttu-id="abc94-136">[Щелкните следующее изображение, чтобы развернуть его.</span><span class="sxs-lookup"><span data-stu-id="abc94-136">[Click the following image to expand it.</span></span> <span data-ttu-id="abc94-137">]</span><span class="sxs-lookup"><span data-stu-id="abc94-137">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. <span data-ttu-id="abc94-138">При **новый проект** диалоговое окно отображается, разверните **Visual C#** в списке шаблонов выберите **Web**и выберите **веб-приложение ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="abc94-138">When the **New Project** dialog box is displayed, expand **Visual C#** in the list of templates, then click **Web**, and select **ASP.NET Web Application**.</span></span> <span data-ttu-id="abc94-139">Присвойте проекту имя **IdentityMySQLDemo** и нажмите кнопку **ОК**:</span><span class="sxs-lookup"><span data-stu-id="abc94-139">Name your project **IdentityMySQLDemo** and then click **OK**:</span></span>  
  
   <span data-ttu-id="abc94-140">[Щелкните следующее изображение, чтобы развернуть его.</span><span class="sxs-lookup"><span data-stu-id="abc94-140">[Click the following image to expand it.</span></span> <span data-ttu-id="abc94-141">]</span><span class="sxs-lookup"><span data-stu-id="abc94-141">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. <span data-ttu-id="abc94-142">В **новый проект ASP.NET** диалоговом окне выберите **MVC** templatewith значения по умолчанию; значение может настроить **учетные записи отдельных пользователей** метод проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="abc94-142">In the **New ASP.NET Project** dialog, select the **MVC** templatewith the default options; this will configure **Individual User Accounts** as the authentication method.</span></span> <span data-ttu-id="abc94-143">Нажмите кнопку **ОК**:</span><span class="sxs-lookup"><span data-stu-id="abc94-143">Click **OK**:</span></span>  
  
   <span data-ttu-id="abc94-144">[Щелкните следующее изображение, чтобы развернуть его.</span><span class="sxs-lookup"><span data-stu-id="abc94-144">[Click the following image to expand it.</span></span> <span data-ttu-id="abc94-145">]</span><span class="sxs-lookup"><span data-stu-id="abc94-145">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a><span data-ttu-id="abc94-146">Настройка EntityFramework, чтобы работать с базой данных MySQL</span><span class="sxs-lookup"><span data-stu-id="abc94-146">Configure EntityFramework to work with a MySQL database</span></span>

### <a name="update-the-entity-framework-assembly-for-your-project"></a><span data-ttu-id="abc94-147">Обновление сборки платформы Entity Framework для проекта</span><span class="sxs-lookup"><span data-stu-id="abc94-147">Update the Entity Framework assembly for your project</span></span>

<span data-ttu-id="abc94-148">Приложение MVC, который был создан из шаблона Visual Studio 2013 содержит ссылку на [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) пакета, но должно было на эту сборку, с момента выпуска обновлений, которые содержат значительные Повышение производительности.</span><span class="sxs-lookup"><span data-stu-id="abc94-148">The MVC application that was created from the Visual Studio 2013 template contains a reference to the [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) package, but there have been updates to that assembly since its release which contain significant performance improvements.</span></span> <span data-ttu-id="abc94-149">Чтобы использовать эти последние обновления в приложении, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="abc94-149">In order to use these latest updates in your application, use the following steps.</span></span>

1. <span data-ttu-id="abc94-150">Откройте проект MVC в Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="abc94-150">Open your MVC project in Visual Studio 2013.</span></span>
2. <span data-ttu-id="abc94-151">Нажмите кнопку **средства**, нажмите кнопку **диспетчер пакетов библиотеки**, а затем нажмите кнопку **консоль диспетчера пакетов**:</span><span class="sxs-lookup"><span data-stu-id="abc94-151">Click **TOOLS**, then click **Library Package Manager**, and then click **Package Manager Console**:</span></span>  
  
   <span data-ttu-id="abc94-152">[Щелкните следующее изображение, чтобы развернуть его.</span><span class="sxs-lookup"><span data-stu-id="abc94-152">[Click the following image to expand it.</span></span> <span data-ttu-id="abc94-153">]</span><span class="sxs-lookup"><span data-stu-id="abc94-153">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. <span data-ttu-id="abc94-154">**Консоль диспетчера пакетов** будет отображаться в нижней части Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="abc94-154">The **Package Manager Console** will appear in the bottom section of Visual Studio.</span></span> <span data-ttu-id="abc94-155">Тип &quot; **EntityFramework Update-Package** &quot; и нажмите клавишу ВВОД:</span><span class="sxs-lookup"><span data-stu-id="abc94-155">Type &quot;**Update-Package EntityFramework**&quot; and press Enter:</span></span>  
  
   <span data-ttu-id="abc94-156">[Щелкните следующее изображение, чтобы развернуть его.</span><span class="sxs-lookup"><span data-stu-id="abc94-156">[Click the following image to expand it.</span></span> <span data-ttu-id="abc94-157">]</span><span class="sxs-lookup"><span data-stu-id="abc94-157">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a><span data-ttu-id="abc94-158">Установите поставщик MySQL для EntityFramework</span><span class="sxs-lookup"><span data-stu-id="abc94-158">Install the MySQL provider for EntityFramework</span></span>

<span data-ttu-id="abc94-159">Чтобы EntityFramework, чтобы подключиться к базе данных MySQL необходимо установить поставщик MySQL.</span><span class="sxs-lookup"><span data-stu-id="abc94-159">In order for EntityFramework to connect to MySQL database, you need to install a MySQL provider.</span></span> <span data-ttu-id="abc94-160">Чтобы сделать это, откройте **консоль диспетчера пакетов** и тип &quot; **MySql.Data.Entity Install-Package - Pre**&quot;, и нажмите клавишу ВВОД.</span><span class="sxs-lookup"><span data-stu-id="abc94-160">To do so, open the **Package Manager Console** and type &quot;**Install-Package MySql.Data.Entity -Pre**&quot;, and then press Enter.</span></span>

> [!NOTE]
> <span data-ttu-id="abc94-161">Это предварительная версия сборки, и таким образом, он может содержать ошибок.</span><span class="sxs-lookup"><span data-stu-id="abc94-161">This is a pre-release version of the assembly, and as such it may contain bugs.</span></span> <span data-ttu-id="abc94-162">Предварительная версия поставщика не следует использовать в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="abc94-162">You should not use a pre-release version of the provider in production.</span></span>


<span data-ttu-id="abc94-163">[Щелкните следующее изображение, чтобы развернуть его.]</span><span class="sxs-lookup"><span data-stu-id="abc94-163">[Click the following image to expand it.]</span></span>  
  
[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a><span data-ttu-id="abc94-164">Изменения конфигурации проекта в файл Web.config для приложения</span><span class="sxs-lookup"><span data-stu-id="abc94-164">Making project configuration changes to the Web.config file for your application</span></span>

<span data-ttu-id="abc94-165">В этом разделе вы настроите Entity Framework для использования поставщика MySQL, который вы только что установили зарегистрировать фабрику поставщика MySQL и добавить строку подключения из Azure.</span><span class="sxs-lookup"><span data-stu-id="abc94-165">In this section you will configure the Entity Framework to use the MySQL provider that you just installed, register the MySQL provider factory, and add your connection string from Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="abc94-166">Следующие примеры содержат конкретную версию сборки для MySql.Data.dll.</span><span class="sxs-lookup"><span data-stu-id="abc94-166">The following examples contain a specific assembly version for MySql.Data.dll.</span></span> <span data-ttu-id="abc94-167">При изменении версии сборки, необходимо изменить соответствующие параметры конфигурации с нужной версией.</span><span class="sxs-lookup"><span data-stu-id="abc94-167">If the assembly version changes, you will need to modify the appropriate configuration settings with the correct version.</span></span>


1. <span data-ttu-id="abc94-168">Откройте файл Web.config для проекта в Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="abc94-168">Open the Web.config file for your project in Visual Studio 2013.</span></span>
2. <span data-ttu-id="abc94-169">Найдите следующие параметры конфигурации, которые определяют поставщик базы данных по умолчанию и фабрики для Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="abc94-169">Locate the following configuration settings, which define the default database provider and factory for the Entity Framework:</span></span>

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. <span data-ttu-id="abc94-170">Замените эти параметры конфигурации следующим образом, который сконфигурирует Entity Framework для использования поставщика MySQL:</span><span class="sxs-lookup"><span data-stu-id="abc94-170">Replace those configuration settings with the following, which will configure the Entity Framework to use the MySQL provider:</span></span> 

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. <span data-ttu-id="abc94-171">Найдите &lt;connectionStrings&gt; разделе и замените его следующим кодом, который будет определять строку подключения для базы данных MySQL, размещенной в Azure (Обратите внимание, что значение providerName также было изменено с исходное значение):</span><span class="sxs-lookup"><span data-stu-id="abc94-171">Locate the &lt;connectionStrings&gt; section and replace it with the following code, which will define the connection string for your MySQL database that is hosted on Azure (note that providerName value has also been changed from the original):</span></span>

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a><span data-ttu-id="abc94-172">Добавление пользовательского контекста MigrationHistory</span><span class="sxs-lookup"><span data-stu-id="abc94-172">Adding custom MigrationHistory context</span></span>

<span data-ttu-id="abc94-173">Использует Entity Framework Code First **MigrationHistory** таблицы для отслеживания изменений модели и для обеспечения согласованности между концептуальной схеме и схеме базы данных.</span><span class="sxs-lookup"><span data-stu-id="abc94-173">Entity Framework Code First uses a **MigrationHistory** table to keep track of model changes and to ensure the consistency between the database schema and conceptual schema.</span></span> <span data-ttu-id="abc94-174">Тем не менее эта таблица не работает для MySQL по умолчанию из-за слишком большого первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="abc94-174">However, this table does not work for MySQL by default because the primary key is too large.</span></span> <span data-ttu-id="abc94-175">Чтобы исправить эту ситуацию, необходимо уменьшить размер ключа для этой таблицы.</span><span class="sxs-lookup"><span data-stu-id="abc94-175">To remedy this situation, you will need to shrink the key size for that table.</span></span> <span data-ttu-id="abc94-176">Чтобы сделать это, следуйте инструкциям ниже:</span><span class="sxs-lookup"><span data-stu-id="abc94-176">To do so, use the following steps:</span></span>

1. <span data-ttu-id="abc94-177">Сведения о схеме для этой таблицы был захвачен в **HistoryContext**, который может быть изменен, как любой другой **DbContext**.</span><span class="sxs-lookup"><span data-stu-id="abc94-177">The schema information for this table is captured in a **HistoryContext**, which can be modified as any other **DbContext**.</span></span> <span data-ttu-id="abc94-178">Чтобы сделать это, добавьте новый файл класса с именем **MySqlHistoryContext.cs** в проект и замените его содержимое следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="abc94-178">To do so, add a new class file named **MySqlHistoryContext.cs** to the project, and replace its contents with the following code:</span></span>

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. <span data-ttu-id="abc94-179">Далее необходимо настроить Entity Framework использовать измененный **HistoryContext**, а не по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="abc94-179">Next you will need to configure Entity Framework to use the modified **HistoryContext**, rather than default one.</span></span> <span data-ttu-id="abc94-180">Это можно сделать путем использования функций, конфигурация на основе кода.</span><span class="sxs-lookup"><span data-stu-id="abc94-180">This can be done by leveraging code-based configuration features.</span></span> <span data-ttu-id="abc94-181">Чтобы сделать это, добавьте новый файл класса с именем **MySqlConfiguration.cs** в проект и замените его содержимое с помощью:</span><span class="sxs-lookup"><span data-stu-id="abc94-181">To do so, add new class file named **MySqlConfiguration.cs** to your project and replace its contents with:</span></span>

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a><span data-ttu-id="abc94-182">Создание настраиваемого инициализатора EntityFramework для ApplicationDbContext</span><span class="sxs-lookup"><span data-stu-id="abc94-182">Creating a custom EntityFramework initializer for ApplicationDbContext</span></span>

<span data-ttu-id="abc94-183">Поставщик MySQL, представленную в этом учебнике не поддерживает миграций Entity Framework, поэтому вам нужно будет использовать инициализаторы модели для подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="abc94-183">The MySQL provider that is featured in this tutorial does not currently support Entity Framework migrations, so you will need to use model initializers in order to connect to the database.</span></span> <span data-ttu-id="abc94-184">Так как в этом руководстве используется экземпляр MySQL в Azure, необходимо будет создать пользовательский инициализатор Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="abc94-184">Because this tutorial is using a MySQL instance on Azure, you will need to create a custom Entity Framework initializer.</span></span>

> [!NOTE]
> <span data-ttu-id="abc94-185">Этот шаг не является обязательным, если вы подключаетесь к экземпляру SQL Server в Azure или если вы используете базу данных, размещенной на локальном компьютере.</span><span class="sxs-lookup"><span data-stu-id="abc94-185">This step is not required if you are connecting to a SQL Server instance on Azure or if you are using a database that is hosted on premises.</span></span>


<span data-ttu-id="abc94-186">Чтобы создать пользовательский инициализатор Entity Framework для MySQL, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="abc94-186">To create a custom Entity Framework initializer for MySQL, use the following steps:</span></span>

1. <span data-ttu-id="abc94-187">Добавьте новый файл класса с именем **MySqlInitializer.cs** к проекту и заменить это содержимое следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="abc94-187">Add a new class file named **MySqlInitializer.cs** to the project, and replace it's contents with the following code:</span></span> 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. <span data-ttu-id="abc94-188">Откройте **IdentityModels.cs** файл проекта, который находится в **моделей** directory и замените его содержимое следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="abc94-188">Open the **IdentityModels.cs** file for your project, which is located in the **Models** directory, and replace it's contents with the following:</span></span> 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a><span data-ttu-id="abc94-189">Запуск приложения и проверка базы данных</span><span class="sxs-lookup"><span data-stu-id="abc94-189">Running the application and verifying the database</span></span>

<span data-ttu-id="abc94-190">После завершения действия, описанные в предыдущих разделах, следует протестировать базы данных.</span><span class="sxs-lookup"><span data-stu-id="abc94-190">Once you have completed the steps in the preceding sections, you should test your database.</span></span> <span data-ttu-id="abc94-191">Чтобы сделать это, следуйте инструкциям ниже:</span><span class="sxs-lookup"><span data-stu-id="abc94-191">To do so, use the following steps:</span></span>

1. <span data-ttu-id="abc94-192">Нажмите клавишу **Ctrl + F5** для сборки и запуска веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="abc94-192">Press **Ctrl + F5** to build and run the web application.</span></span>
2. <span data-ttu-id="abc94-193">Нажмите кнопку **зарегистрировать** вкладки в верхней части страницы:</span><span class="sxs-lookup"><span data-stu-id="abc94-193">Click the **Register** tab on the top of the page:</span></span>  
  
   <span data-ttu-id="abc94-194">[Щелкните следующее изображение, чтобы развернуть его.</span><span class="sxs-lookup"><span data-stu-id="abc94-194">[Click the following image to expand it.</span></span> <span data-ttu-id="abc94-195">]</span><span class="sxs-lookup"><span data-stu-id="abc94-195">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. <span data-ttu-id="abc94-196">Введите новое имя пользователя и пароль, а затем нажмите кнопку **зарегистрировать**:</span><span class="sxs-lookup"><span data-stu-id="abc94-196">Enter a new user name and password, and then click **Register**:</span></span>  
  
   <span data-ttu-id="abc94-197">[Щелкните следующее изображение, чтобы развернуть его.</span><span class="sxs-lookup"><span data-stu-id="abc94-197">[Click the following image to expand it.</span></span> <span data-ttu-id="abc94-198">]</span><span class="sxs-lookup"><span data-stu-id="abc94-198">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. <span data-ttu-id="abc94-199">На этом этапе ASP.NET Identity таблицы создаются в базе данных MySQL и регистрации и входа в приложение пользователя:</span><span class="sxs-lookup"><span data-stu-id="abc94-199">At this point the ASP.NET Identity tables are created on the MySQL Database, and the user is registered and logged into the application:</span></span>  
  
   <span data-ttu-id="abc94-200">[Щелкните следующее изображение, чтобы развернуть его.</span><span class="sxs-lookup"><span data-stu-id="abc94-200">[Click the following image to expand it.</span></span> <span data-ttu-id="abc94-201">]</span><span class="sxs-lookup"><span data-stu-id="abc94-201">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a><span data-ttu-id="abc94-202">Установка средства MySQL Workbench, чтобы проверить данные</span><span class="sxs-lookup"><span data-stu-id="abc94-202">Installing MySQL Workbench tool to verify the data</span></span>

1. <span data-ttu-id="abc94-203">Установка **MySQL Workbench** средство из [страницу загрузок MySQL](http://dev.mysql.com/downloads/windows/installer/)</span><span class="sxs-lookup"><span data-stu-id="abc94-203">Install the **MySQL Workbench** tool from the [MySQL downloads page](http://dev.mysql.com/downloads/windows/installer/)</span></span>
2. <span data-ttu-id="abc94-204">В мастере установки: **Выбор компонентов** выберите **MySQL Workbench** под **приложений** раздел.</span><span class="sxs-lookup"><span data-stu-id="abc94-204">In the installation wizard: **Feature Selection** tab, select **MySQL Workbench** under **applications** section.</span></span>
3. <span data-ttu-id="abc94-205">Запустите приложение и добавить новое подключение с помощью подключения строковые данные из базы данных Azure MySQL на приглашение этого учебника.</span><span class="sxs-lookup"><span data-stu-id="abc94-205">Launch the app and add a new connection using the connection string data from the Azure MySQL database you created at the begging of this tutorial.</span></span>
4. <span data-ttu-id="abc94-206">После установления соединения следует проверять **ASP.NET Identity** таблицы, созданные на **IdentityMySQLDatabase.**</span><span class="sxs-lookup"><span data-stu-id="abc94-206">After establishing the connection, inspect the **ASP.NET Identity** tables created on the **IdentityMySQLDatabase.**</span></span>
5. <span data-ttu-id="abc94-207">Вы увидите, что все удостоверения ASP.NET необходимые таблицы создаются в том случае, как показано на рисунке ниже:</span><span class="sxs-lookup"><span data-stu-id="abc94-207">You will see that all ASP.NET Identity required tables are created as shown in the image below:</span></span>  
  
   <span data-ttu-id="abc94-208">[Щелкните следующее изображение, чтобы развернуть его.</span><span class="sxs-lookup"><span data-stu-id="abc94-208">[Click the following image to expand it.</span></span> <span data-ttu-id="abc94-209">]</span><span class="sxs-lookup"><span data-stu-id="abc94-209">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. <span data-ttu-id="abc94-210">Проверьте **aspnetusers** таблицы для экземпляра на наличие записей по мере регистрации новых пользователей.</span><span class="sxs-lookup"><span data-stu-id="abc94-210">Inspect the **aspnetusers** table for instance to check for the entries as you register new users.</span></span>  
  
   <span data-ttu-id="abc94-211">[Щелкните следующее изображение, чтобы развернуть его.</span><span class="sxs-lookup"><span data-stu-id="abc94-211">[Click the following image to expand it.</span></span> <span data-ttu-id="abc94-212">]</span><span class="sxs-lookup"><span data-stu-id="abc94-212">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)
