---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: Определение класса запуска OWIN | Документы Microsoft
author: Praburaj
description: Этого учебника показано, как настроить класс запуска OWIN, который загружается. Дополнительные сведения о OWIN в разделе Обзор Katana проекта. Этот учебник был...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: 33d2745b24387419e5614c62c2d46948427b242a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870054"
---
<a name="owin-startup-class-detection"></a><span data-ttu-id="0874a-105">Определение класса запуска OWIN</span><span class="sxs-lookup"><span data-stu-id="0874a-105">OWIN Startup Class Detection</span></span>
====================
<span data-ttu-id="0874a-106">по [Praburaj Thiagarajan](https://github.com/Praburaj), [Рик Андерсон](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="0874a-106">by [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="0874a-107">Этого учебника показано, как настроить класс запуска OWIN, который загружается.</span><span class="sxs-lookup"><span data-stu-id="0874a-107">This tutorial shows how to configure which OWIN startup class is loaded.</span></span> <span data-ttu-id="0874a-108">Дополнительные сведения о OWIN см. в разделе [Обзор проекта Katana](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="0874a-108">For more information on OWIN, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="0874a-109">Это руководство было написано с Рик Андерсон ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan и Говард Дайеркинг ( [ @howard \_Дайеркинг](https://twitter.com/howard_dierking) ).</span><span class="sxs-lookup"><span data-stu-id="0874a-109">This tutorial was written by Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan, and Howard Dierking ( [@howard\_dierking](https://twitter.com/howard_dierking) ).</span></span>
> 
> ## <a name="prerequisites"></a><span data-ttu-id="0874a-110">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="0874a-110">Prerequisites</span></span>
> 
> [<span data-ttu-id="0874a-111">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="0874a-111">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)


## <a name="owin-startup-class-detection"></a><span data-ttu-id="0874a-112">Определение класса запуска OWIN</span><span class="sxs-lookup"><span data-stu-id="0874a-112">OWIN Startup Class Detection</span></span>

 <span data-ttu-id="0874a-113">Каждое приложение OWIN имеет класс запуска, где указываются компоненты для конвейера приложения.</span><span class="sxs-lookup"><span data-stu-id="0874a-113">Every OWIN Application has a startup class where you specify components for the application pipeline.</span></span> <span data-ttu-id="0874a-114">Существуют различные способы запуска класса могут подключаться со средой выполнения, в зависимости от модели размещения выберите (OwinHost, IIS и IIS Express).</span><span class="sxs-lookup"><span data-stu-id="0874a-114">There are different ways you can connect your startup class with the runtime, depending on the hosting model you choose (OwinHost, IIS, and IIS-Express).</span></span> <span data-ttu-id="0874a-115">Класс startup, показанные в этом учебнике используется в каждое приложение размещения.</span><span class="sxs-lookup"><span data-stu-id="0874a-115">The startup class shown in this tutorial can be used in every hosting application.</span></span> <span data-ttu-id="0874a-116">Класс startup подключиться с размещения среды выполнения, используя одну из этих подходы:</span><span class="sxs-lookup"><span data-stu-id="0874a-116">You connect the startup class with the hosting runtime using one of the these approaches:</span></span>  

1. <span data-ttu-id="0874a-117">**Соглашение об именовании**: Katana ищет класс с именем `Startup` в пространстве имен, имя сборки или глобального пространства имен.</span><span class="sxs-lookup"><span data-stu-id="0874a-117">**Naming Convention**: Katana looks for a class named `Startup` in namespace matching the assembly name or the global namespace.</span></span>
2. <span data-ttu-id="0874a-118">**Атрибут OwinStartup**: именно этот подход, укажите класс startup большинство разработчиков будет выполняться.</span><span class="sxs-lookup"><span data-stu-id="0874a-118">**OwinStartup Attribute**: This is the approach most developers will take to specify the startup class.</span></span> <span data-ttu-id="0874a-119">Следующий атрибут будет присвоено класс startup `TestStartup` класса в `StartupDemo` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="0874a-119">The following attribute will set the startup class to the `TestStartup` class in the `StartupDemo` namespace.</span></span> 

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

   <span data-ttu-id="0874a-120">`OwinStartup` Атрибут замещает соглашение об именовании.</span><span class="sxs-lookup"><span data-stu-id="0874a-120">The `OwinStartup` attribute overrides the naming convention.</span></span> <span data-ttu-id="0874a-121">С помощью этого атрибута также можно указать понятное имя, однако с помощью понятного имени требует также использования `appSetting` элемент в файле конфигурации.</span><span class="sxs-lookup"><span data-stu-id="0874a-121">You can also specify a friendly name with this attribute, however, using a friendly name requires you to also use the `appSetting` element in the configuration file.</span></span>
3. <span data-ttu-id="0874a-122">**В файле конфигурации элемент appSetting**: `appSetting` переопределяет `OwinStartup` атрибут и соглашение об именовании.</span><span class="sxs-lookup"><span data-stu-id="0874a-122">**The appSetting element in the Configuration file**: The `appSetting` element overrides the `OwinStartup` attribute and naming convention.</span></span> <span data-ttu-id="0874a-123">Можно иметь несколько классов запуска (каждый из которых использует `OwinStartup` атрибут) и настройте класс, который при запуске будет загружаться в файл конфигурации с помощью разметки следующего вида:</span><span class="sxs-lookup"><span data-stu-id="0874a-123">You can have multiple startup classes (each using an `OwinStartup` attribute) and configure which startup class will be loaded in a configuration file using markup similar to the following:</span></span>  

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

   <span data-ttu-id="0874a-124">Также можно использовать следующий раздел, явным образом указывающий запуска класса и сборки:</span><span class="sxs-lookup"><span data-stu-id="0874a-124">The following key, which explicitly specifies the startup class and assembly can also be used:</span></span> 

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

   <span data-ttu-id="0874a-125">Следующий XML-код в файле конфигурации задает имя класса понятное запуска `ProductionConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="0874a-125">The following XML in the configuration file specifies a friendly startup class name of `ProductionConfiguration`.</span></span>  

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

   <span data-ttu-id="0874a-126">Выше разметки должен использоваться со следующими `OwinStartup` атрибут, который указывает понятное имя и вызывает `ProductionStartup2` для выполнения.</span><span class="sxs-lookup"><span data-stu-id="0874a-126">The above markup must be used with the following `OwinStartup` attribute which specifies a friendly name and causes the `ProductionStartup2` class to run.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. <span data-ttu-id="0874a-127">Чтобы отключить обнаружение запуска OWIN добавьте `appSetting owin:AutomaticAppStartup` со значением `"false"` в файле web.config.</span><span class="sxs-lookup"><span data-stu-id="0874a-127">To disable OWIN startup discovery add the `appSetting owin:AutomaticAppStartup` with a value of `"false"` in the web.config file.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a><span data-ttu-id="0874a-128">Создать веб-приложение ASP.NET с помощью запуска OWIN</span><span class="sxs-lookup"><span data-stu-id="0874a-128">Create an ASP.NET Web App using OWIN Startup</span></span>

1. <span data-ttu-id="0874a-129">Создайте пустое веб-приложение Asp.Net и назовите его **StartupDemo**.</span><span class="sxs-lookup"><span data-stu-id="0874a-129">Create an empty Asp.Net web application and name it **StartupDemo**.</span></span> <span data-ttu-id="0874a-130">-Установить `Microsoft.Owin.Host.SystemWeb` с помощью диспетчера пакетов NuGet.</span><span class="sxs-lookup"><span data-stu-id="0874a-130">- Install `Microsoft.Owin.Host.SystemWeb` using the NuGet package manager.</span></span> <span data-ttu-id="0874a-131">Из **средства** последовательно выберите пункты **диспетчер пакетов библиотеки**, а затем **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="0874a-131">From the **Tools** menu, select **Library Package Manager**, and then **Package Manager Console**.</span></span> <span data-ttu-id="0874a-132">Введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="0874a-132">Enter the following command:</span></span>  

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
2. <span data-ttu-id="0874a-133">Добавьте класс запуска OWIN.</span><span class="sxs-lookup"><span data-stu-id="0874a-133">Add an OWIN startup class.</span></span> <span data-ttu-id="0874a-134">В Visual Studio 2013 правой кнопкой мыши проект и выберите **добавить класс**. — в **Добавление нового элемента** диалогового окна введите *OWIN* в поле поиска и измените имя на файле Startup.cs, затем **добавить**.</span><span class="sxs-lookup"><span data-stu-id="0874a-134">In Visual Studio 2013 right click the project and select **Add Class**.- In the **Add New Item** dialog box, enter *OWIN* in the search field, and change the name to Startup.cs, and then click **Add**.</span></span>  
  
     ![](owin-startup-class-detection/_static/image1.png)   
  
   <span data-ttu-id="0874a-135">В следующий раз, необходимо добавить *класс запуска Owin*, будет доступен из **добавить** меню.</span><span class="sxs-lookup"><span data-stu-id="0874a-135">The next time you want to add an *Owin Startup class*, it will be in available from the **Add** menu.</span></span>  
   
     ![](owin-startup-class-detection/_static/image2.png)  
  
   <span data-ttu-id="0874a-136">Кроме того, щелкните правой кнопкой мыши проект и выберите **добавить**, а затем выберите **новый элемент**и выберите **запуска Owin класса**.</span><span class="sxs-lookup"><span data-stu-id="0874a-136">Alternatively, you can right click the project and select **Add**, then select **New Item**, and then select the **Owin Startup class**.</span></span>  
  
     ![](owin-startup-class-detection/_static/image3.png)  
  
- <span data-ttu-id="0874a-137">Замените код, созданный в *файла Startup.cs* файл со следующим:</span><span class="sxs-lookup"><span data-stu-id="0874a-137">Replace the generated code in the *Startup.cs* file with the following:</span></span>  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]
  
  <span data-ttu-id="0874a-138">`app.Use` Лямбда-выражение используется для регистрации компонента указанного по промежуточного слоя в конвейере OWIN.</span><span class="sxs-lookup"><span data-stu-id="0874a-138">The `app.Use` lambda expression is used to register the specified middleware component to the OWIN pipeline.</span></span> <span data-ttu-id="0874a-139">В этом случае настройка ведения журнала входящих запросов перед ответом на входящего запроса.</span><span class="sxs-lookup"><span data-stu-id="0874a-139">In this case we are setting up logging of incoming requests before responding to the incoming request.</span></span> <span data-ttu-id="0874a-140">`next` Параметр является делегатом ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [задачи](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) к следующему компоненту в конвейере.</span><span class="sxs-lookup"><span data-stu-id="0874a-140">The `next` parameter is the delegate ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [Task](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) to the next component in the pipeline.</span></span> <span data-ttu-id="0874a-141">`app.Run` Лямбда-выражение подключается конвейера на входящие запросы и предоставляет механизм ответа.</span><span class="sxs-lookup"><span data-stu-id="0874a-141">The `app.Run` lambda expression hooks up the pipeline to incoming requests and provides the response mechanism.</span></span>
     > [!NOTE]
     > <span data-ttu-id="0874a-142">В приведенном выше коде мы закомментирован `OwinStartup` атрибут и мы полагаетесь на соглашение выполнения класс с именем `Startup` .-клавишу ***F5*** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="0874a-142">In the code above we have commented out the `OwinStartup` attribute and we're relying on the convention of running the class named `Startup` .- Press ***F5*** to run the application.</span></span> <span data-ttu-id="0874a-143">Попадание обновления несколько раз.</span><span class="sxs-lookup"><span data-stu-id="0874a-143">Hit refresh a few times.</span></span>  
  
    ![](owin-startup-class-detection/_static/image4.png)  
  <span data-ttu-id="0874a-144">Примечание: Значение, показанное в образы в этом учебнике не будет соответствовать число см.</span><span class="sxs-lookup"><span data-stu-id="0874a-144">Note: The number shown in the images in this tutorial will not match the number you see.</span></span> <span data-ttu-id="0874a-145">Строка миллисекунды используется для отображения новый ответ при обновлении страницы.</span><span class="sxs-lookup"><span data-stu-id="0874a-145">The millisecond string is used to show a new response when you refresh the page.</span></span>  
  <span data-ttu-id="0874a-146">Можно просмотреть информацию трассировки в **вывода** окна.</span><span class="sxs-lookup"><span data-stu-id="0874a-146">You can see the trace information in the **Output** window.</span></span>  
  
    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a><span data-ttu-id="0874a-147">Добавить дополнительные классы запуска</span><span class="sxs-lookup"><span data-stu-id="0874a-147">Add More Startup Classes</span></span>

<span data-ttu-id="0874a-148">В этом разделе мы добавим другого класса при запуске.</span><span class="sxs-lookup"><span data-stu-id="0874a-148">In this section we'll add another Startup class.</span></span> <span data-ttu-id="0874a-149">Можно добавить несколько класс запуска OWIN в приложение.</span><span class="sxs-lookup"><span data-stu-id="0874a-149">You can add multiple OWIN startup class to your application.</span></span> <span data-ttu-id="0874a-150">Например можно создать классы запуска для разработки, тестирования и эксплуатации.</span><span class="sxs-lookup"><span data-stu-id="0874a-150">For example, you might want to create startup classes for development, testing and production.</span></span>

1. <span data-ttu-id="0874a-151">Создайте новый класс запуска OWIN и назовите его `ProductionStartup`.</span><span class="sxs-lookup"><span data-stu-id="0874a-151">Create a new OWIN Startup class and name it `ProductionStartup`.</span></span>
2. <span data-ttu-id="0874a-152">Замените сгенерированный код следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="0874a-152">Replace the generated code with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. <span data-ttu-id="0874a-153">Нажмите клавишу F5 элемента управления, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="0874a-153">Press Control F5 to run the app.</span></span> <span data-ttu-id="0874a-154">`OwinStartup` Атрибут указывает, запущена класс startup производства.</span><span class="sxs-lookup"><span data-stu-id="0874a-154">The `OwinStartup` attribute specifies the production startup class is run.</span></span>  
  
    ![](owin-startup-class-detection/_static/image6.png)
4. <span data-ttu-id="0874a-155">Создайте еще один класс запуска OWIN и назовите его `TestStartup`.</span><span class="sxs-lookup"><span data-stu-id="0874a-155">Create another OWIN Startup class and name it `TestStartup`.</span></span>
5. <span data-ttu-id="0874a-156">Замените сгенерированный код следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="0874a-156">Replace the generated code with the following:</span></span>  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

   <span data-ttu-id="0874a-157">`OwinStartup` Выше перегрузка атрибут задает `TestingConfiguration` как *понятное* класса при запуске.</span><span class="sxs-lookup"><span data-stu-id="0874a-157">The `OwinStartup` attribute overload above specifies `TestingConfiguration` as the *friendly* name of the Startup class.</span></span>
6. <span data-ttu-id="0874a-158">Откройте *web.config* и добавьте ключ запуска приложения OWIN, который указывает понятное имя класса при запуске файла:</span><span class="sxs-lookup"><span data-stu-id="0874a-158">Open the *web.config* file and add the OWIN App startup key which specifies the friendly name of the Startup class:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. <span data-ttu-id="0874a-159">Нажмите клавишу F5 элемента управления, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="0874a-159">Press Control F5 to run the app.</span></span> <span data-ttu-id="0874a-160">Элемент параметры приложения принимает высокий приоритет и тестового запуска конфигурации.</span><span class="sxs-lookup"><span data-stu-id="0874a-160">The app settings element takes precedent, and the test configuration is run.</span></span>  
  
    ![](owin-startup-class-detection/_static/image7.png)
8. <span data-ttu-id="0874a-161">Удалить *понятное* имя из `OwinStartup` атрибута в `TestStartup` класса.</span><span class="sxs-lookup"><span data-stu-id="0874a-161">Remove the *friendly* name from the `OwinStartup` attribute in the `TestStartup` class.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. <span data-ttu-id="0874a-162">Замените на ключ запуска приложения OWIN в *web.config* файл со следующим:</span><span class="sxs-lookup"><span data-stu-id="0874a-162">Replace the OWIN App startup key in the *web.config* file with the following:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. <span data-ttu-id="0874a-163">Восстановить `OwinStartup` атрибута в каждом классе в код атрибута по умолчанию, созданную Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="0874a-163">Revert the `OwinStartup` attribute in each class to the default attribute code generated by Visual Studio:</span></span>  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

    <span data-ttu-id="0874a-164">Каждый ключ запуска приложения OWIN ниже вызовет производственного класса для выполнения.</span><span class="sxs-lookup"><span data-stu-id="0874a-164">Each of the OWIN App startup keys below will cause the production class to run.</span></span> 

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

    <span data-ttu-id="0874a-165">Ключ последнего запуска указывает метод конфигурации запуска.</span><span class="sxs-lookup"><span data-stu-id="0874a-165">The last startup key specifies the startup configuration method.</span></span> <span data-ttu-id="0874a-166">Следующий ключ запуска приложения OWIN позволяет изменить имя класса конфигурации `MyConfiguration` .</span><span class="sxs-lookup"><span data-stu-id="0874a-166">The following OWIN App startup key allows you to change the name of the configuration class to `MyConfiguration` .</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a><span data-ttu-id="0874a-167">С помощью Owinhost.exe</span><span class="sxs-lookup"><span data-stu-id="0874a-167">Using Owinhost.exe</span></span>

1. <span data-ttu-id="0874a-168">Замените файл Web.config следующую разметку:</span><span class="sxs-lookup"><span data-stu-id="0874a-168">Replace the Web.config file with the following markup:</span></span>  

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

   <span data-ttu-id="0874a-169">Ключ последнего wins, поэтому в данном случае `TestStartup` указано.</span><span class="sxs-lookup"><span data-stu-id="0874a-169">The last key wins, so in this case `TestStartup` is specified.</span></span>
2. <span data-ttu-id="0874a-170">Установка Owinhost из PMC:</span><span class="sxs-lookup"><span data-stu-id="0874a-170">Install Owinhost from the PMC:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. <span data-ttu-id="0874a-171">Перейдите в папку приложения (папку, содержащую *Web.config* файл) и в командную строку и введите:</span><span class="sxs-lookup"><span data-stu-id="0874a-171">Navigate to the application folder (the folder containing the *Web.config* file) and in a command prompt and type:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

   <span data-ttu-id="0874a-172">Командное окно будет показано следующее:</span><span class="sxs-lookup"><span data-stu-id="0874a-172">The command window will show:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. <span data-ttu-id="0874a-173">Запустить браузер с URL-адрес `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="0874a-173">Launch a browser with the URL `http://localhost:5000/`.</span></span>  
  
    ![](owin-startup-class-detection/_static/image8.png)  
  
   <span data-ttu-id="0874a-174">OwinHost соблюдаться соглашения запуска, перечисленных выше.</span><span class="sxs-lookup"><span data-stu-id="0874a-174">OwinHost honored the startup conventions listed above.</span></span>
5. <span data-ttu-id="0874a-175">В окне командной строки нажмите клавишу ВВОД, чтобы закрыть OwinHost.</span><span class="sxs-lookup"><span data-stu-id="0874a-175">In the command window, press Enter to exit OwinHost.</span></span>
6. <span data-ttu-id="0874a-176">В `ProductionStartup` класса, добавьте следующий атрибут OwinStartup, который указывает понятное имя *ProductionConfiguration*.</span><span class="sxs-lookup"><span data-stu-id="0874a-176">In the `ProductionStartup` class, add the following OwinStartup attribute which specifies a friendly name of *ProductionConfiguration*.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. <span data-ttu-id="0874a-177">В командную строку и введите:</span><span class="sxs-lookup"><span data-stu-id="0874a-177">In the command prompt and type:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

   <span data-ttu-id="0874a-178">Класс запуска рабочей загрузки.</span><span class="sxs-lookup"><span data-stu-id="0874a-178">The Production startup class is loaded.</span></span>  
    ![](owin-startup-class-detection/_static/image9.png)  
   <span data-ttu-id="0874a-179">Наше приложение имеет несколько классов запуска, а в этом примере имеют отсроченные загружаемый класс запуска до времени выполнения.</span><span class="sxs-lookup"><span data-stu-id="0874a-179">Our application has multiple startup classes, and in this example we have deferred which startup class to load until runtime.</span></span>
8. <span data-ttu-id="0874a-180">Проверьте следующие параметры запуска среды выполнения:</span><span class="sxs-lookup"><span data-stu-id="0874a-180">Test the following runtime startup options:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
