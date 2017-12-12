---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: "ASP.NET Identity: С помощью MySQL хранилища с поставщиком MySQL EntityFramework (C#) | Документы Microsoft"
author: maumar
description: "Этот учебник показывает, как заменить механизм хранения данных по умолчанию для ASP.NET Identity EntityFramework (поставщик клиента SQL) с обеспечить MySQL..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/10/2013
ms.topic: article
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: ac254abcb756d048d159a9b67967a581f35ac871
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a><span data-ttu-id="bd24d-103">ASP.NET Identity: С помощью MySQL хранилища с поставщиком EntityFramework MySQL (C#)</span><span class="sxs-lookup"><span data-stu-id="bd24d-103">ASP.NET Identity: Using MySQL Storage with an EntityFramework MySQL Provider (C#)</span></span>
====================
<span data-ttu-id="bd24d-104">по [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Robert McMurray](https://github.com/rmcmurray)</span><span class="sxs-lookup"><span data-stu-id="bd24d-104">by [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Robert McMurray](https://github.com/rmcmurray)</span></span>

> <span data-ttu-id="bd24d-105">Этого учебника показано, как заменить механизм хранения данных по умолчанию для [ **ASP.NET Identity** ](introduction-to-aspnet-identity.md) с EntityFramework (поставщик клиента SQL) с поставщиком MySQL.</span><span class="sxs-lookup"><span data-stu-id="bd24d-105">This tutorial shows you how to replace the default data storage mechanism for [**ASP.NET Identity**](introduction-to-aspnet-identity.md) with EntityFramework (SQL client provider) with a MySQL provider.</span></span>


<span data-ttu-id="bd24d-106">В этом учебнике рассматриваются следующие темы:</span><span class="sxs-lookup"><span data-stu-id="bd24d-106">The following topics will be covered in this tutorial:</span></span>

- <span data-ttu-id="bd24d-107">Создание базы данных MySQL в Azure</span><span class="sxs-lookup"><span data-stu-id="bd24d-107">Creating a MySQL database on Azure</span></span>
- <span data-ttu-id="bd24d-108">Создание приложения MVC с помощью шаблона Visual Studio 2013 MVC</span><span class="sxs-lookup"><span data-stu-id="bd24d-108">Creating an MVC application using Visual Studio 2013 MVC template</span></span>
- <span data-ttu-id="bd24d-109">Настройка EntityFramework для работы с поставщиком базы данных MySQL</span><span class="sxs-lookup"><span data-stu-id="bd24d-109">Configuring EntityFramework to work with a MySQL database provider</span></span>
- <span data-ttu-id="bd24d-110">Запуск приложения для проверки результатов</span><span class="sxs-lookup"><span data-stu-id="bd24d-110">Running the application to verify the results</span></span>

<span data-ttu-id="bd24d-111">В конце этого учебника имеется приложения MVC с ASP.NET Identity сохранения, использующего базу данных MySQL, размещенной в Azure.</span><span class="sxs-lookup"><span data-stu-id="bd24d-111">At the end of this tutorial, you will have an MVC application with the ASP.NET Identity store that is using a MySQL database that is hosted in Azure.</span></span>

## <a name="creating-a-mysql-database-instance-on-azure"></a><span data-ttu-id="bd24d-112">Создание экземпляра базы данных MySQL в Azure</span><span class="sxs-lookup"><span data-stu-id="bd24d-112">Creating a MySQL database instance on Azure</span></span>

1. <span data-ttu-id="bd24d-113">Войдите на [портал Azure](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="bd24d-113">Log in to the [Azure Portal](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).</span></span>
2. <span data-ttu-id="bd24d-114">Нажмите кнопку **NEW** в нижней части страницы, а затем выберите **ХРАНИЛИЩА**:</span><span class="sxs-lookup"><span data-stu-id="bd24d-114">Click **NEW** at the bottom of the page, and then select **STORE**:</span></span>  
  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. <span data-ttu-id="bd24d-115">В **Выбор и надстройки** мастера выберите **базы данных MySQL ClearDB**и нажмите кнопку **Далее** стрелку в нижней части области:</span><span class="sxs-lookup"><span data-stu-id="bd24d-115">In the **Choose and Add-on** wizard, select **ClearDB MySQL Database**, and then click the **Next** arrow at the bottom of the frame:</span></span>  
  
 <span data-ttu-id="bd24d-116">[Щелкните следующие изображение, чтобы развернуть его.</span><span class="sxs-lookup"><span data-stu-id="bd24d-116">[Click the following image to expand it.</span></span> <span data-ttu-id="bd24d-117">]</span><span class="sxs-lookup"><span data-stu-id="bd24d-117">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. <span data-ttu-id="bd24d-118">Оставьте значение по умолчанию **Free** плана, измените **имя** для **IdentityMySQLDatabase**, выберите регион, ближайшего к вам и нажмите кнопку **Далее** стрелку в нижней части области:</span><span class="sxs-lookup"><span data-stu-id="bd24d-118">Keep the default **Free** plan, change the **NAME** to **IdentityMySQLDatabase**, select the region that is nearest to you, and then click the **Next** arrow at the bottom of the frame:</span></span>  
  
 <span data-ttu-id="bd24d-119">[Щелкните следующие изображение, чтобы развернуть его.</span><span class="sxs-lookup"><span data-stu-id="bd24d-119">[Click the following image to expand it.</span></span> <span data-ttu-id="bd24d-120">]</span><span class="sxs-lookup"><span data-stu-id="bd24d-120">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. <span data-ttu-id="bd24d-121">Нажмите кнопку **ПОКУПКИ** флажок для завершения создания базы данных.</span><span class="sxs-lookup"><span data-stu-id="bd24d-121">Click the **PURCHASE** checkmark to complete the database creation.</span></span>  
  
 <span data-ttu-id="bd24d-122">[Щелкните следующие изображение, чтобы развернуть его.</span><span class="sxs-lookup"><span data-stu-id="bd24d-122">[Click the following image to expand it.</span></span> <span data-ttu-id="bd24d-123">]</span><span class="sxs-lookup"><span data-stu-id="bd24d-123">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. <span data-ttu-id="bd24d-124">После создания базы данных можно было управлять из **надстройки** на портале управления.</span><span class="sxs-lookup"><span data-stu-id="bd24d-124">After your database has been created, you can manage it from the **ADD-ONS** tab in the management portal.</span></span> <span data-ttu-id="bd24d-125">Чтобы получить сведения о соединении для базы данных, щелкните **сведений о СОЕДИНЕНИИ** в нижней части страницы:</span><span class="sxs-lookup"><span data-stu-id="bd24d-125">To retrieve the connection information for your database, click **CONNECTION INFO** at the bottom of the page:</span></span>  
  
 <span data-ttu-id="bd24d-126">[Щелкните следующие изображение, чтобы развернуть его.</span><span class="sxs-lookup"><span data-stu-id="bd24d-126">[Click the following image to expand it.</span></span> <span data-ttu-id="bd24d-127">]</span><span class="sxs-lookup"><span data-stu-id="bd24d-127">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. <span data-ttu-id="bd24d-128">Скопируйте строку подключения, нажав кнопку "Копировать", **CONNECTIONSTRING** поля и сохраните его; эти сведения далее в этом учебнике будет использоваться для приложения MVC:</span><span class="sxs-lookup"><span data-stu-id="bd24d-128">Copy the connection string by clicking on the copy button by the **CONNECTIONSTRING** field and save it; you will use this information later in this tutorial for your MVC application:</span></span>  
  
 <span data-ttu-id="bd24d-129">[Щелкните следующие изображение, чтобы развернуть его.</span><span class="sxs-lookup"><span data-stu-id="bd24d-129">[Click the following image to expand it.</span></span> <span data-ttu-id="bd24d-130">]</span><span class="sxs-lookup"><span data-stu-id="bd24d-130">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a><span data-ttu-id="bd24d-131">Создав проект приложения MVC</span><span class="sxs-lookup"><span data-stu-id="bd24d-131">Creating an MVC application project</span></span>

<span data-ttu-id="bd24d-132">Для выполнения шагов в этом разделе учебника, сначала необходимо установить [Visual Studio Express 2013 для Web](https://go.microsoft.com/fwlink/?LinkId=299058) или [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="bd24d-132">To complete the steps in this section of the tutorial, you will first need to install [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="bd24d-133">После установки Visual Studio, выполните следующие действия, чтобы создать новый проект MVC-приложения:</span><span class="sxs-lookup"><span data-stu-id="bd24d-133">Once Visual Studio has been installed, use the following steps to create a new MVC application project:</span></span>

1. <span data-ttu-id="bd24d-134">Откройте Visual Studio 2103.</span><span class="sxs-lookup"><span data-stu-id="bd24d-134">Open Visual Studio 2103.</span></span>
2. <span data-ttu-id="bd24d-135">Нажмите кнопку **новый проект** из **запустить** страницы, или же можно нажать **файл** меню и затем **новый проект**:</span><span class="sxs-lookup"><span data-stu-id="bd24d-135">Click **New Project** from the **Start** page, or you can click the **File** menu and then **New Project**:</span></span>  
  
 <span data-ttu-id="bd24d-136">[Щелкните следующие изображение, чтобы развернуть его.</span><span class="sxs-lookup"><span data-stu-id="bd24d-136">[Click the following image to expand it.</span></span> <span data-ttu-id="bd24d-137">]</span><span class="sxs-lookup"><span data-stu-id="bd24d-137">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. <span data-ttu-id="bd24d-138">При **новый проект** диалоговое окно, разверните узел **Visual C#** в списке шаблонов выберите **Web**и выберите **веб-приложение ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="bd24d-138">When the **New Project** dialog box is displayed, expand **Visual C#** in the list of templates, then click **Web**, and select **ASP.NET Web Application**.</span></span> <span data-ttu-id="bd24d-139">Присвойте проекту имя **IdentityMySQLDemo** и нажмите кнопку **ОК**:</span><span class="sxs-lookup"><span data-stu-id="bd24d-139">Name your project **IdentityMySQLDemo** and then click **OK**:</span></span>  
  
 <span data-ttu-id="bd24d-140">[Щелкните следующие изображение, чтобы развернуть его.</span><span class="sxs-lookup"><span data-stu-id="bd24d-140">[Click the following image to expand it.</span></span> <span data-ttu-id="bd24d-141">]</span><span class="sxs-lookup"><span data-stu-id="bd24d-141">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. <span data-ttu-id="bd24d-142">В **новый проект ASP.NET** диалогового окна выберите **MVC** templatewith значения по умолчанию; при этом будут Настройка **индивидуальные учетные записи** в качестве метода проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="bd24d-142">In the **New ASP.NET Project** dialog, select the **MVC** templatewith the default options; this will configure **Individual User Accounts** as the authentication method.</span></span> <span data-ttu-id="bd24d-143">Нажмите кнопку **ОК**:</span><span class="sxs-lookup"><span data-stu-id="bd24d-143">Click **OK**:</span></span>  
  
 <span data-ttu-id="bd24d-144">[Щелкните следующие изображение, чтобы развернуть его.</span><span class="sxs-lookup"><span data-stu-id="bd24d-144">[Click the following image to expand it.</span></span> <span data-ttu-id="bd24d-145">]</span><span class="sxs-lookup"><span data-stu-id="bd24d-145">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a><span data-ttu-id="bd24d-146">Настройка EntityFramework для работы с базой данных MySQL</span><span class="sxs-lookup"><span data-stu-id="bd24d-146">Configure EntityFramework to work with a MySQL database</span></span>

### <a name="update-the-entity-framework-assembly-for-your-project"></a><span data-ttu-id="bd24d-147">Обновление сборки платформы Entity Framework для проекта</span><span class="sxs-lookup"><span data-stu-id="bd24d-147">Update the Entity Framework assembly for your project</span></span>

<span data-ttu-id="bd24d-148">Приложение MVC, который был создан из шаблона Visual Studio 2013 содержит ссылку на [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) пакета, но имеют были значительно обновления на эту сборку, с момента выпуска содержащие Повышение производительности.</span><span class="sxs-lookup"><span data-stu-id="bd24d-148">The MVC application that was created from the Visual Studio 2013 template contains a reference to the [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) package, but there have been updates to to that assembly since its release which contain significant performance improvements.</span></span> <span data-ttu-id="bd24d-149">Чтобы использовать эти последние обновления в приложение, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="bd24d-149">In order to use these latest updates in your application, use the following steps.</span></span>

1. <span data-ttu-id="bd24d-150">Откройте проект MVC в Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="bd24d-150">Open your MVC project in Visual Studio 2013.</span></span>
2. <span data-ttu-id="bd24d-151">Нажмите кнопку **средства**, нажмите кнопку **диспетчер пакетов библиотеки**, а затем нажмите кнопку **консоль диспетчера пакетов**:</span><span class="sxs-lookup"><span data-stu-id="bd24d-151">Click **TOOLS**, then click **Library Package Manager**, and then click **Package Manager Console**:</span></span>  
  
 <span data-ttu-id="bd24d-152">[Щелкните следующие изображение, чтобы развернуть его.</span><span class="sxs-lookup"><span data-stu-id="bd24d-152">[Click the following image to expand it.</span></span> <span data-ttu-id="bd24d-153">]</span><span class="sxs-lookup"><span data-stu-id="bd24d-153">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. <span data-ttu-id="bd24d-154">**Консоль диспетчера пакетов** появятся в нижней части окна Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bd24d-154">The **Package Manager Console** will appear in the bottom section of Visual Studio.</span></span> <span data-ttu-id="bd24d-155">Тип &quot; **EntityFramework пакет обновления** &quot; и нажмите клавишу ВВОД:</span><span class="sxs-lookup"><span data-stu-id="bd24d-155">Type &quot;**Update-Package EntityFramework**&quot; and press Enter:</span></span>  
  
 <span data-ttu-id="bd24d-156">[Щелкните следующие изображение, чтобы развернуть его.</span><span class="sxs-lookup"><span data-stu-id="bd24d-156">[Click the following image to expand it.</span></span> <span data-ttu-id="bd24d-157">]</span><span class="sxs-lookup"><span data-stu-id="bd24d-157">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a><span data-ttu-id="bd24d-158">Установка MySQL поставщика для EntityFramework</span><span class="sxs-lookup"><span data-stu-id="bd24d-158">Install the MySQL provider for EntityFramework</span></span>

<span data-ttu-id="bd24d-159">Чтобы EntityFramework для подключения к базе данных MySQL необходимо установить поставщик MySQL.</span><span class="sxs-lookup"><span data-stu-id="bd24d-159">In order for EntityFramework to connect to MySQL database, you need to install a MySQL provider.</span></span> <span data-ttu-id="bd24d-160">Чтобы сделать это, откройте **консоль диспетчера пакетов** и тип &quot; **MySql.Data.Entity Install-Package - Pre**&quot;, и нажмите клавишу ВВОД.</span><span class="sxs-lookup"><span data-stu-id="bd24d-160">To do so, open the **Package Manager Console** and type &quot;**Install-Package MySql.Data.Entity -Pre**&quot;, and then press Enter.</span></span>

> [!NOTE]
> <span data-ttu-id="bd24d-161">Это предварительная версия сборки, и таким образом, он может содержать ошибки.</span><span class="sxs-lookup"><span data-stu-id="bd24d-161">This is a pre-release version of the assembly, and as such it may contain bugs.</span></span> <span data-ttu-id="bd24d-162">Предварительная версия поставщика не следует использовать в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="bd24d-162">You should not use a pre-release version of the provider in production.</span></span>


<span data-ttu-id="bd24d-163">[Щелкните следующем рисунке, чтобы развернуть его.]</span><span class="sxs-lookup"><span data-stu-id="bd24d-163">[Click the following image to expand it.]</span></span>  
  
[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a><span data-ttu-id="bd24d-164">Внесение изменений конфигурации проекта в файл Web.config для приложения</span><span class="sxs-lookup"><span data-stu-id="bd24d-164">Making project configuration changes to the Web.config file for your application</span></span>

<span data-ttu-id="bd24d-165">В этом разделе вы настроите Entity Framework для использования поставщика MySQL, который был только что установлен зарегистрировать фабрику поставщика MySQL и добавить строки подключения из Azure.</span><span class="sxs-lookup"><span data-stu-id="bd24d-165">In this section you will configure the Entity Framework to use the MySQL provider that you just installed, register the MySQL provider factory, and add your connection string from Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="bd24d-166">Следующие примеры содержат конкретную версию сборки для MySql.Data.dll.</span><span class="sxs-lookup"><span data-stu-id="bd24d-166">The following examples contain a specific assembly version for MySql.Data.dll.</span></span> <span data-ttu-id="bd24d-167">При изменении версии сборки, необходимо изменить параметры соответствующих настроек с нужной версией.</span><span class="sxs-lookup"><span data-stu-id="bd24d-167">If the assembly version changes, you will need to modify the appropriate configuration settings with the correct version.</span></span>


1. <span data-ttu-id="bd24d-168">Откройте файл Web.config для проекта в Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="bd24d-168">Open the Web.config file for your project in Visual Studio 2013.</span></span>
2. <span data-ttu-id="bd24d-169">Найдите следующие параметры конфигурации, которые определяют поставщика базы данных по умолчанию и фабрики для платформы Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="bd24d-169">Locate the following configuration settings, which define the default database provider and factory for the Entity Framework:</span></span>

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. <span data-ttu-id="bd24d-170">Эти параметры конфигурации, замените следующую команду, настроенной для использования поставщика MySQL Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="bd24d-170">Replace those configuration settings with the following, which will configure the Entity Framework to use the MySQL provider:</span></span> 

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. <span data-ttu-id="bd24d-171">Найдите &lt;connectionStrings&gt; статьи и замените его следующим кодом, который будет определять строку подключения для базы данных MySQL, размещенной в Azure (Обратите внимание, что значение providerName также было изменено с исходное значение):</span><span class="sxs-lookup"><span data-stu-id="bd24d-171">Locate the &lt;connectionStrings&gt; section and replace it with the following code, which will define the connection string for your MySQL database that is hosted on Azure (note that providerName value has also been changed from the original):</span></span>

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a><span data-ttu-id="bd24d-172">Добавление пользовательского контекста MigrationHistory</span><span class="sxs-lookup"><span data-stu-id="bd24d-172">Adding custom MigrationHistory context</span></span>

<span data-ttu-id="bd24d-173">Entity Framework Code First использует **MigrationHistory** таблицы для отслеживания изменений модели и для обеспечения согласованности между концептуальной схемы и схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="bd24d-173">Entity Framework Code First uses a **MigrationHistory** table to keep track of model changes and to ensure the consistency between the database schema and conceptual schema.</span></span> <span data-ttu-id="bd24d-174">Тем не менее эта таблица не работает для MySQL по умолчанию, так как первичный ключ слишком велик.</span><span class="sxs-lookup"><span data-stu-id="bd24d-174">However, this table does not work for MySQL by default because the primary key is too large.</span></span> <span data-ttu-id="bd24d-175">Чтобы исправить эту ситуацию, необходимо уменьшить размер ключа для этой таблицы.</span><span class="sxs-lookup"><span data-stu-id="bd24d-175">To remedy this situation, you will need to shrink the key size for that table.</span></span> <span data-ttu-id="bd24d-176">Чтобы сделать это, выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="bd24d-176">To do so, use the following steps:</span></span>

1. <span data-ttu-id="bd24d-177">Сведения схемы для этой таблицы сохраняются в **HistoryContext**, который может быть изменен, как любой другой **DbContext**.</span><span class="sxs-lookup"><span data-stu-id="bd24d-177">The schema information for this table is captured in a **HistoryContext**, which can be modified as any other **DbContext**.</span></span> <span data-ttu-id="bd24d-178">Чтобы сделать это, добавьте новый файл класса с именем **MySqlHistoryContext.cs** в проект и замените его содержимое следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="bd24d-178">To do so, add a new class file named **MySqlHistoryContext.cs** to the project, and replace its contents with the following code:</span></span>

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. <span data-ttu-id="bd24d-179">Далее необходимо настроить использование измененного Entity Framework **HistoryContext**, а не по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="bd24d-179">Next you will need to configure Entity Framework to use the modified **HistoryContext**, rather than default one.</span></span> <span data-ttu-id="bd24d-180">Это можно сделать с помощью функции настройки на основе кода.</span><span class="sxs-lookup"><span data-stu-id="bd24d-180">This can be done by leveraging code-based configuration features.</span></span> <span data-ttu-id="bd24d-181">Чтобы сделать это, добавьте новый файл класса с именем **MySqlConfiguration.cs** в проект и заменить его содержимое:</span><span class="sxs-lookup"><span data-stu-id="bd24d-181">To do so, add new class file named **MySqlConfiguration.cs** to your project and replace its contents with:</span></span>

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a><span data-ttu-id="bd24d-182">Создание настраиваемого инициализатора для ApplicationDbContext EntityFramework</span><span class="sxs-lookup"><span data-stu-id="bd24d-182">Creating a custom EntityFramework initializer for ApplicationDbContext</span></span>

<span data-ttu-id="bd24d-183">Поставщик MySQL, представлено в этом учебнике не поддерживает миграции Entity Framework, вам потребуется использовать инициализаторы модели для подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="bd24d-183">The MySQL provider that is featured in this tutorial does not currently support Entity Framework migrations, so you will need to use model initializers in order to connect to the database.</span></span> <span data-ttu-id="bd24d-184">Поскольку этот учебник использует экземпляр MySQL в Azure, требуется необходимо создать пользовательский инициализатор Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="bd24d-184">Because this tutorial is using a MySQL instance on Azure, you will need need to create a custom Entity Framework initializer.</span></span>

> [!NOTE]
> <span data-ttu-id="bd24d-185">Этот шаг не является обязательным при подключении к экземпляру SQL Server на Azure или при использовании базы данных, который размещается на локальном компьютере.</span><span class="sxs-lookup"><span data-stu-id="bd24d-185">This step is not required if you are connecting to a SQL Server instance on Azure or if you are using a database that is hosted on premises.</span></span>


<span data-ttu-id="bd24d-186">Для создания пользовательского инициализатора Entity Framework для MySQL, выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="bd24d-186">To create a custom Entity Framework initializer for MySQL, use the following steps:</span></span>

1. <span data-ttu-id="bd24d-187">Добавьте новый файл класса с именем **MySqlInitializer.cs** в проект и заменить это содержимое следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="bd24d-187">Add a new class file named **MySqlInitializer.cs** to the project, and replace it's contents with the following code:</span></span> 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. <span data-ttu-id="bd24d-188">Откройте **IdentityModels.cs** файл проекта, который находится в **моделей** каталога и замените его содержимое следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="bd24d-188">Open the **IdentityModels.cs** file for your project, which is located in the **Models** directory, and replace it's contents with the following:</span></span> 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a><span data-ttu-id="bd24d-189">Запуск приложения и проверка базы данных</span><span class="sxs-lookup"><span data-stu-id="bd24d-189">Running the application and verifying the database</span></span>

<span data-ttu-id="bd24d-190">После завершения действия, описанные в предыдущих разделах, следует проверить базу данных.</span><span class="sxs-lookup"><span data-stu-id="bd24d-190">Once you have completed the steps in the preceding sections, you should test your database.</span></span> <span data-ttu-id="bd24d-191">Чтобы сделать это, выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="bd24d-191">To do so, use the following steps:</span></span>

1. <span data-ttu-id="bd24d-192">Нажмите клавишу **сочетание клавиш Ctrl + F5** для построения и запуска веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="bd24d-192">Press **Ctrl + F5** to build and run the web application.</span></span>
2. <span data-ttu-id="bd24d-193">Нажмите кнопку **зарегистрировать** вкладки в верхней части страницы:</span><span class="sxs-lookup"><span data-stu-id="bd24d-193">Click the **Register** tab on the top of the page:</span></span>  
  
 <span data-ttu-id="bd24d-194">[Щелкните следующие изображение, чтобы развернуть его.</span><span class="sxs-lookup"><span data-stu-id="bd24d-194">[Click the following image to expand it.</span></span> <span data-ttu-id="bd24d-195">]</span><span class="sxs-lookup"><span data-stu-id="bd24d-195">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. <span data-ttu-id="bd24d-196">Введите новое имя пользователя и пароль, а затем нажмите кнопку **зарегистрировать**:</span><span class="sxs-lookup"><span data-stu-id="bd24d-196">Enter a new user name and password, and then click **Register**:</span></span>  
  
 <span data-ttu-id="bd24d-197">[Щелкните следующие изображение, чтобы развернуть его.</span><span class="sxs-lookup"><span data-stu-id="bd24d-197">[Click the following image to expand it.</span></span> <span data-ttu-id="bd24d-198">]</span><span class="sxs-lookup"><span data-stu-id="bd24d-198">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. <span data-ttu-id="bd24d-199">На этом этапе ASP.NET Identity таблицы создаются в базе данных MySQL, а пользователь зарегистрирован и входа в приложение:</span><span class="sxs-lookup"><span data-stu-id="bd24d-199">At this point the ASP.NET Identity tables are created on the MySQL Database, and the user is registered and logged into the application:</span></span>  
  
 <span data-ttu-id="bd24d-200">[Щелкните следующие изображение, чтобы развернуть его.</span><span class="sxs-lookup"><span data-stu-id="bd24d-200">[Click the following image to expand it.</span></span> <span data-ttu-id="bd24d-201">]</span><span class="sxs-lookup"><span data-stu-id="bd24d-201">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a><span data-ttu-id="bd24d-202">Установка MySQL Workbench средства для проверки данных</span><span class="sxs-lookup"><span data-stu-id="bd24d-202">Installing MySQL Workbench tool to verify the data</span></span>

1. <span data-ttu-id="bd24d-203">Установка **MySQL Workbench** средства из [MySQL файлов для загрузки](http://dev.mysql.com/downloads/windows/installer/)</span><span class="sxs-lookup"><span data-stu-id="bd24d-203">Install the **MySQL Workbench** tool from the [MySQL downloads page](http://dev.mysql.com/downloads/windows/installer/)</span></span>
2. <span data-ttu-id="bd24d-204">В мастере установки: **Выбор компонентов** выберите **MySQL Workbench** под **приложений** раздела.</span><span class="sxs-lookup"><span data-stu-id="bd24d-204">In the installation wizard: **Feature Selection** tab, select **MySQL Workbench** under **applications** section.</span></span>
3. <span data-ttu-id="bd24d-205">Запустите приложение и добавьте новое подключение с помощью данных строки подключения из базы данных MySQL в Azure, созданное в приглашение этого учебника.</span><span class="sxs-lookup"><span data-stu-id="bd24d-205">Launch the app and add a new connection using the connection string data from the Azure MySQL database you created at the begging of this tutorial.</span></span>
4. <span data-ttu-id="bd24d-206">После подключения к проверки **ASP.NET Identity** таблицы, созданные на **IdentityMySQLDatabase.**</span><span class="sxs-lookup"><span data-stu-id="bd24d-206">After establishing the connection, inspect the **ASP.NET Identity** tables created on the **IdentityMySQLDatabase.**</span></span>
5. <span data-ttu-id="bd24d-207">Вы увидите, что все ASP.NET Identity необходимые таблицы создаются, как показано на рисунке ниже:</span><span class="sxs-lookup"><span data-stu-id="bd24d-207">You will see that all ASP.NET Identity required tables are created as shown in the image below:</span></span>  
  
 <span data-ttu-id="bd24d-208">[Щелкните следующие изображение, чтобы развернуть его.</span><span class="sxs-lookup"><span data-stu-id="bd24d-208">[Click the following image to expand it.</span></span> <span data-ttu-id="bd24d-209">]</span><span class="sxs-lookup"><span data-stu-id="bd24d-209">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. <span data-ttu-id="bd24d-210">Проверить **aspnetusers** таблицы для экземпляра на наличие записей, по мере регистрации новых пользователей.</span><span class="sxs-lookup"><span data-stu-id="bd24d-210">Inspect the **aspnetusers** table for instance to check for the entries as you register new users.</span></span>  
  
 <span data-ttu-id="bd24d-211">[Щелкните следующие изображение, чтобы развернуть его.</span><span class="sxs-lookup"><span data-stu-id="bd24d-211">[Click the following image to expand it.</span></span> <span data-ttu-id="bd24d-212">]</span><span class="sxs-lookup"><span data-stu-id="bd24d-212">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)
