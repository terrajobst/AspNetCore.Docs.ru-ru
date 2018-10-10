---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: Определение класса запуска OWIN | Документация Майкрософт
author: Praburaj
description: Этом руководстве демонстрируется настройка загружается какой класс запуска OWIN. Дополнительные сведения о OWIN см. в разделе Обзор проекта Katana. Этот учебник был...
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: 4e753187f1caae646402712c2abc28856ae71a79
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910711"
---
<a name="owin-startup-class-detection"></a><span data-ttu-id="80988-105">Определение класса запуска OWIN</span><span class="sxs-lookup"><span data-stu-id="80988-105">OWIN Startup Class Detection</span></span>
====================
<span data-ttu-id="80988-106">по [Praburaj Thiagarajan](https://github.com/Praburaj), [Рик Андерсон]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="80988-106">by [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> <span data-ttu-id="80988-107">Этом руководстве демонстрируется настройка загружается какой класс запуска OWIN.</span><span class="sxs-lookup"><span data-stu-id="80988-107">This tutorial shows how to configure which OWIN startup class is loaded.</span></span> <span data-ttu-id="80988-108">Дополнительные сведения о OWIN, см. в разделе [Обзор проекта Katana](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="80988-108">For more information on OWIN, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="80988-109">Это руководство было написано с Рик Андерсон ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan и Говард Дайеркинг ( [ @howard \_Дайеркинг](https://twitter.com/howard_dierking) ).</span><span class="sxs-lookup"><span data-stu-id="80988-109">This tutorial was written by Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan, and Howard Dierking ( [@howard\_dierking](https://twitter.com/howard_dierking) ).</span></span>
>
> ## <a name="prerequisites"></a><span data-ttu-id="80988-110">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="80988-110">Prerequisites</span></span>
>
> [<span data-ttu-id="80988-111">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="80988-111">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)


## <a name="owin-startup-class-detection"></a><span data-ttu-id="80988-112">Определение класса запуска OWIN</span><span class="sxs-lookup"><span data-stu-id="80988-112">OWIN Startup Class Detection</span></span>

 <span data-ttu-id="80988-113">Каждое приложение OWIN имеет класс запуска, где указываются компоненты конвейера приложения.</span><span class="sxs-lookup"><span data-stu-id="80988-113">Every OWIN Application has a startup class where you specify components for the application pipeline.</span></span> <span data-ttu-id="80988-114">Существует несколько способов, вы можете подключиться классе запуска в среде выполнения, в зависимости от модели размещения выберите (OwinHost, IIS и IIS Express).</span><span class="sxs-lookup"><span data-stu-id="80988-114">There are different ways you can connect your startup class with the runtime, depending on the hosting model you choose (OwinHost, IIS, and IIS-Express).</span></span> <span data-ttu-id="80988-115">Класс startup, в этом учебнике используется в каждое приложение размещения.</span><span class="sxs-lookup"><span data-stu-id="80988-115">The startup class shown in this tutorial can be used in every hosting application.</span></span> <span data-ttu-id="80988-116">Класс startup для соединения с размещения среды выполнения с помощью, один из этих приближается к:</span><span class="sxs-lookup"><span data-stu-id="80988-116">You connect the startup class with the hosting runtime using one of the these approaches:</span></span>

1. <span data-ttu-id="80988-117">**Соглашение об именовании**: Katana ищет класс с именем `Startup` в пространстве имен, имя сборки или глобальное пространство имен.</span><span class="sxs-lookup"><span data-stu-id="80988-117">**Naming Convention**: Katana looks for a class named `Startup` in namespace matching the assembly name or the global namespace.</span></span>
2. <span data-ttu-id="80988-118">**Атрибут OwinStartup**: именно этот подход, большинство разработчиков займет для указания класса startup.</span><span class="sxs-lookup"><span data-stu-id="80988-118">**OwinStartup Attribute**: This is the approach most developers will take to specify the startup class.</span></span> <span data-ttu-id="80988-119">Следующий атрибут будет присвоено класс startup `TestStartup` в класс `StartupDemo` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="80988-119">The following attribute will set the startup class to the `TestStartup` class in the `StartupDemo` namespace.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

   <span data-ttu-id="80988-120">`OwinStartup` Атрибут переопределяет соглашение об именовании.</span><span class="sxs-lookup"><span data-stu-id="80988-120">The `OwinStartup` attribute overrides the naming convention.</span></span> <span data-ttu-id="80988-121">Также можно указать понятное имя с этим атрибутом, тем не менее, используя понятное имя требует также использования `appSetting` элемент в файле конфигурации.</span><span class="sxs-lookup"><span data-stu-id="80988-121">You can also specify a friendly name with this attribute, however, using a friendly name requires you to also use the `appSetting` element in the configuration file.</span></span>
3. <span data-ttu-id="80988-122">**В файле конфигурации элемент appSetting**: `appSetting` переопределяет `OwinStartup` атрибут и соглашение об именовании.</span><span class="sxs-lookup"><span data-stu-id="80988-122">**The appSetting element in the Configuration file**: The `appSetting` element overrides the `OwinStartup` attribute and naming convention.</span></span> <span data-ttu-id="80988-123">Можно иметь несколько классов запуска (каждый с помощью `OwinStartup` атрибут) и настройки, какой класс запуска будут загружены в файл конфигурации, используя разметку, аналогичную следующей:</span><span class="sxs-lookup"><span data-stu-id="80988-123">You can have multiple startup classes (each using an `OwinStartup` attribute) and configure which startup class will be loaded in a configuration file using markup similar to the following:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

   <span data-ttu-id="80988-124">Также можно использовать следующий раздел, который явно указывает класс startup и сборки:</span><span class="sxs-lookup"><span data-stu-id="80988-124">The following key, which explicitly specifies the startup class and assembly can also be used:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

   <span data-ttu-id="80988-125">Следующий код XML в файле конфигурации задает имя класса понятное запуска `ProductionConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="80988-125">The following XML in the configuration file specifies a friendly startup class name of `ProductionConfiguration`.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

   <span data-ttu-id="80988-126">Приведенная выше разметка должен использоваться со следующими `OwinStartup` атрибут, который задает понятное имя и вызывает `ProductionStartup2` класса для запуска.</span><span class="sxs-lookup"><span data-stu-id="80988-126">The above markup must be used with the following `OwinStartup` attribute which specifies a friendly name and causes the `ProductionStartup2` class to run.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. <span data-ttu-id="80988-127">Чтобы отключить обнаружение запуска OWIN добавьте `appSetting owin:AutomaticAppStartup` со значением `"false"` в файле web.config.</span><span class="sxs-lookup"><span data-stu-id="80988-127">To disable OWIN startup discovery add the `appSetting owin:AutomaticAppStartup` with a value of `"false"` in the web.config file.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a><span data-ttu-id="80988-128">Создать веб-приложение ASP.NET, с помощью запуска OWIN</span><span class="sxs-lookup"><span data-stu-id="80988-128">Create an ASP.NET Web App using OWIN Startup</span></span>

1. <span data-ttu-id="80988-129">Создайте пустое веб-приложение Asp.Net и назовите его **StartupDemo**.</span><span class="sxs-lookup"><span data-stu-id="80988-129">Create an empty Asp.Net web application and name it **StartupDemo**.</span></span> <span data-ttu-id="80988-130">-Установить `Microsoft.Owin.Host.SystemWeb` с помощью диспетчера пакетов NuGet.</span><span class="sxs-lookup"><span data-stu-id="80988-130">- Install `Microsoft.Owin.Host.SystemWeb` using the NuGet package manager.</span></span> <span data-ttu-id="80988-131">Из **средства** меню, выберите **диспетчер пакетов NuGet**, а затем **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="80988-131">From the **Tools** menu, select **NuGet Package Manager**, and then **Package Manager Console**.</span></span> <span data-ttu-id="80988-132">Введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="80988-132">Enter the following command:</span></span>

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
2. <span data-ttu-id="80988-133">Добавьте класс запуска OWIN.</span><span class="sxs-lookup"><span data-stu-id="80988-133">Add an OWIN startup class.</span></span> <span data-ttu-id="80988-134">В Visual Studio 2013 правой кнопкой мыши проект и выберите **Добавление класса**. — в **Добавление нового элемента** диалогового окна введите *OWIN* в поле поиска и измените имя на Startup.cs, а затем нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="80988-134">In Visual Studio 2013 right click the project and select **Add Class**.- In the **Add New Item** dialog box, enter *OWIN* in the search field, and change the name to Startup.cs, and then click **Add**.</span></span>

     ![](owin-startup-class-detection/_static/image1.png)

   <span data-ttu-id="80988-135">В следующий раз, вы хотите добавить *класс запуска Owin*, будет доступен из **добавить** меню.</span><span class="sxs-lookup"><span data-stu-id="80988-135">The next time you want to add an *Owin Startup class*, it will be in available from the **Add** menu.</span></span>

     ![](owin-startup-class-detection/_static/image2.png)

   <span data-ttu-id="80988-136">Кроме того, можно правой кнопкой мыши проект и выберите **добавить**, а затем выберите **новый элемент**, а затем выберите **класс запуска Owin**.</span><span class="sxs-lookup"><span data-stu-id="80988-136">Alternatively, you can right click the project and select **Add**, then select **New Item**, and then select the **Owin Startup class**.</span></span>

     ![](owin-startup-class-detection/_static/image3.png)

- <span data-ttu-id="80988-137">Замените сгенерированный код в *Startup.cs* файл со следующими:</span><span class="sxs-lookup"><span data-stu-id="80988-137">Replace the generated code in the *Startup.cs* file with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]

  <span data-ttu-id="80988-138">`app.Use` Лямбда-выражение используется для регистрации компонента указанного по промежуточного слоя в конвейер OWIN.</span><span class="sxs-lookup"><span data-stu-id="80988-138">The `app.Use` lambda expression is used to register the specified middleware component to the OWIN pipeline.</span></span> <span data-ttu-id="80988-139">В этом случае мы собираемся настроить ведение журнала входящих запросов, прежде чем отвечать на входящий запрос.</span><span class="sxs-lookup"><span data-stu-id="80988-139">In this case we are setting up logging of incoming requests before responding to the incoming request.</span></span> <span data-ttu-id="80988-140">`next` Параметр является делегатом ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [задачи](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) к следующему компоненту в конвейере.</span><span class="sxs-lookup"><span data-stu-id="80988-140">The `next` parameter is the delegate ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [Task](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) to the next component in the pipeline.</span></span> <span data-ttu-id="80988-141">`app.Run` Лямбда-выражение подключает конвейер для входящих запросов и предоставляет механизм ответа.</span><span class="sxs-lookup"><span data-stu-id="80988-141">The `app.Run` lambda expression hooks up the pipeline to incoming requests and provides the response mechanism.</span></span>
     > [!NOTE]
     > <span data-ttu-id="80988-142">В приведенном выше коде мы закомментирован `OwinStartup` атрибут и мы полагаетесь на соглашению под управлением с именем класса `Startup` .-Press ***F5*** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="80988-142">In the code above we have commented out the `OwinStartup` attribute and we're relying on the convention of running the class named `Startup` .- Press ***F5*** to run the application.</span></span> <span data-ttu-id="80988-143">Несколько раз нажмите кнопку обновления.</span><span class="sxs-lookup"><span data-stu-id="80988-143">Hit refresh a few times.</span></span>

    <span data-ttu-id="80988-144">![](owin-startup-class-detection/_static/image4.png) Примечание: Значение, показанное в образы в этом руководстве не будет соответствовать номера, который отображается.</span><span class="sxs-lookup"><span data-stu-id="80988-144">![](owin-startup-class-detection/_static/image4.png) Note: The number shown in the images in this tutorial will not match the number you see.</span></span> <span data-ttu-id="80988-145">Миллисекунды строка используется для отображения нового ответа при обновлении страницы.</span><span class="sxs-lookup"><span data-stu-id="80988-145">The millisecond string is used to show a new response when you refresh the page.</span></span>
  <span data-ttu-id="80988-146">Вы увидите сведения о трассировке в **вывода** окна.</span><span class="sxs-lookup"><span data-stu-id="80988-146">You can see the trace information in the **Output** window.</span></span>

    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a><span data-ttu-id="80988-147">Добавить дополнительные классы запуска</span><span class="sxs-lookup"><span data-stu-id="80988-147">Add More Startup Classes</span></span>

<span data-ttu-id="80988-148">В этом разделе мы добавим еще один класс запуска.</span><span class="sxs-lookup"><span data-stu-id="80988-148">In this section we'll add another Startup class.</span></span> <span data-ttu-id="80988-149">Можно добавить несколько класс запуска OWIN в приложение.</span><span class="sxs-lookup"><span data-stu-id="80988-149">You can add multiple OWIN startup class to your application.</span></span> <span data-ttu-id="80988-150">Например можно создать классы запуска для разработки, тестирования и эксплуатации.</span><span class="sxs-lookup"><span data-stu-id="80988-150">For example, you might want to create startup classes for development, testing and production.</span></span>

1. <span data-ttu-id="80988-151">Создайте новый класс запуска OWIN и назовите его `ProductionStartup`.</span><span class="sxs-lookup"><span data-stu-id="80988-151">Create a new OWIN Startup class and name it `ProductionStartup`.</span></span>
2. <span data-ttu-id="80988-152">Замените сгенерированный код следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="80988-152">Replace the generated code with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. <span data-ttu-id="80988-153">Нажмите клавишу F5 элемента управления, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="80988-153">Press Control F5 to run the app.</span></span> <span data-ttu-id="80988-154">`OwinStartup` Атрибут указывает, выполняется класс startup рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="80988-154">The `OwinStartup` attribute specifies the production startup class is run.</span></span>

    ![](owin-startup-class-detection/_static/image6.png)
4. <span data-ttu-id="80988-155">Создайте еще один класс запуска OWIN и назовите его `TestStartup`.</span><span class="sxs-lookup"><span data-stu-id="80988-155">Create another OWIN Startup class and name it `TestStartup`.</span></span>
5. <span data-ttu-id="80988-156">Замените сгенерированный код следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="80988-156">Replace the generated code with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

   <span data-ttu-id="80988-157">`OwinStartup` Перегрузка атрибут выше указывает `TestingConfiguration` как *понятное* имя класса Startup.</span><span class="sxs-lookup"><span data-stu-id="80988-157">The `OwinStartup` attribute overload above specifies `TestingConfiguration` as the *friendly* name of the Startup class.</span></span>
6. <span data-ttu-id="80988-158">Откройте *web.config* файл и добавьте ключ запуска приложения OWIN, который указывает понятное имя в классе Startup:</span><span class="sxs-lookup"><span data-stu-id="80988-158">Open the *web.config* file and add the OWIN App startup key which specifies the friendly name of the Startup class:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. <span data-ttu-id="80988-159">Нажмите клавишу F5 элемента управления, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="80988-159">Press Control F5 to run the app.</span></span> <span data-ttu-id="80988-160">Элемент параметров приложения имеет высокий приоритет и тест запускается конфигурации.</span><span class="sxs-lookup"><span data-stu-id="80988-160">The app settings element takes precedent, and the test configuration is run.</span></span>

    ![](owin-startup-class-detection/_static/image7.png)
8. <span data-ttu-id="80988-161">Удалить *понятное* имя из `OwinStartup` атрибут в `TestStartup` класса.</span><span class="sxs-lookup"><span data-stu-id="80988-161">Remove the *friendly* name from the `OwinStartup` attribute in the `TestStartup` class.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. <span data-ttu-id="80988-162">Замените ключ запуска приложения OWIN в *web.config* файл со следующими:</span><span class="sxs-lookup"><span data-stu-id="80988-162">Replace the OWIN App startup key in the *web.config* file with the following:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. <span data-ttu-id="80988-163">Отменить `OwinStartup` атрибута этих классов в код атрибута по умолчанию, созданный средой Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="80988-163">Revert the `OwinStartup` attribute in each class to the default attribute code generated by Visual Studio:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

    <span data-ttu-id="80988-164">Каждый ключ запуска приложения OWIN ниже приведет к производственного класса для запуска.</span><span class="sxs-lookup"><span data-stu-id="80988-164">Each of the OWIN App startup keys below will cause the production class to run.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

    <span data-ttu-id="80988-165">Ключ последнего запуска способ конфигурации запуска.</span><span class="sxs-lookup"><span data-stu-id="80988-165">The last startup key specifies the startup configuration method.</span></span> <span data-ttu-id="80988-166">Следующий раздел запуска приложения OWIN позволяет изменить имя класса конфигурации для `MyConfiguration` .</span><span class="sxs-lookup"><span data-stu-id="80988-166">The following OWIN App startup key allows you to change the name of the configuration class to `MyConfiguration` .</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a><span data-ttu-id="80988-167">С помощью Owinhost.exe</span><span class="sxs-lookup"><span data-stu-id="80988-167">Using Owinhost.exe</span></span>

1. <span data-ttu-id="80988-168">Замените файл Web.config следующую разметку:</span><span class="sxs-lookup"><span data-stu-id="80988-168">Replace the Web.config file with the following markup:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

   <span data-ttu-id="80988-169">Ключ последнего wins, поэтому в данном случае `TestStartup` указан.</span><span class="sxs-lookup"><span data-stu-id="80988-169">The last key wins, so in this case `TestStartup` is specified.</span></span>
2. <span data-ttu-id="80988-170">Установка Owinhost из PMC:</span><span class="sxs-lookup"><span data-stu-id="80988-170">Install Owinhost from the PMC:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. <span data-ttu-id="80988-171">Перейдите в папку приложения (в папку, содержащую *Web.config* файл) и в командную строку и введите:</span><span class="sxs-lookup"><span data-stu-id="80988-171">Navigate to the application folder (the folder containing the *Web.config* file) and in a command prompt and type:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

   <span data-ttu-id="80988-172">Командное окно будет показано следующее:</span><span class="sxs-lookup"><span data-stu-id="80988-172">The command window will show:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. <span data-ttu-id="80988-173">Окно браузера с URL-адрес `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="80988-173">Launch a browser with the URL `http://localhost:5000/`.</span></span>

    ![](owin-startup-class-detection/_static/image8.png)

   <span data-ttu-id="80988-174">OwinHost соблюдаться соглашения, запуска, перечисленные выше.</span><span class="sxs-lookup"><span data-stu-id="80988-174">OwinHost honored the startup conventions listed above.</span></span>
5. <span data-ttu-id="80988-175">В окне командной строки нажмите клавишу ВВОД для выхода OwinHost.</span><span class="sxs-lookup"><span data-stu-id="80988-175">In the command window, press Enter to exit OwinHost.</span></span>
6. <span data-ttu-id="80988-176">В `ProductionStartup` добавьте следующий атрибут OwinStartup, который указывает понятное имя *ProductionConfiguration*.</span><span class="sxs-lookup"><span data-stu-id="80988-176">In the `ProductionStartup` class, add the following OwinStartup attribute which specifies a friendly name of *ProductionConfiguration*.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. <span data-ttu-id="80988-177">В командную строку и введите:</span><span class="sxs-lookup"><span data-stu-id="80988-177">In the command prompt and type:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

   <span data-ttu-id="80988-178">Класс startup рабочей загрузки.</span><span class="sxs-lookup"><span data-stu-id="80988-178">The Production startup class is loaded.</span></span>
    ![](owin-startup-class-detection/_static/image9.png)
   <span data-ttu-id="80988-179">Наше приложение имеет несколько классов запуска, а в этом примере мы имеют отсроченные какой класс startup для загрузки только во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="80988-179">Our application has multiple startup classes, and in this example we have deferred which startup class to load until runtime.</span></span>
8. <span data-ttu-id="80988-180">Проверьте следующие параметры запуска среды выполнения:</span><span class="sxs-lookup"><span data-stu-id="80988-180">Test the following runtime startup options:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
