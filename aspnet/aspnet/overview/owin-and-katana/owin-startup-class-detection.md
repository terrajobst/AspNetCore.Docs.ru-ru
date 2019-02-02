---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: Определение класса запуска OWIN | Документация Майкрософт
author: Praburaj
description: Этом руководстве демонстрируется настройка загружается какой класс запуска OWIN. Дополнительные сведения о OWIN см. в разделе Обзор проекта Katana. Этот учебник был...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: 0b34cca8b48383dbb028106651758dff889ed614
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667301"
---
<a name="owin-startup-class-detection"></a><span data-ttu-id="bfb60-105">Определение класса запуска OWIN</span><span class="sxs-lookup"><span data-stu-id="bfb60-105">OWIN Startup Class Detection</span></span>
====================

> <span data-ttu-id="bfb60-106">Этом руководстве демонстрируется настройка загружается какой класс запуска OWIN.</span><span class="sxs-lookup"><span data-stu-id="bfb60-106">This tutorial shows how to configure which OWIN startup class is loaded.</span></span> <span data-ttu-id="bfb60-107">Дополнительные сведения о OWIN, см. в разделе [Обзор проекта Katana](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="bfb60-107">For more information on OWIN, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="bfb60-108">Это руководство было написано с Рик Андерсон ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan и Говард Дайеркинг ( [ @howard \_Дайеркинг](https://twitter.com/howard_dierking) ).</span><span class="sxs-lookup"><span data-stu-id="bfb60-108">This tutorial was written by Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan, and Howard Dierking ( [@howard\_dierking](https://twitter.com/howard_dierking) ).</span></span>
>
> ## <a name="prerequisites"></a><span data-ttu-id="bfb60-109">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="bfb60-109">Prerequisites</span></span>
>
> [<span data-ttu-id="bfb60-110">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="bfb60-110">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/)


## <a name="owin-startup-class-detection"></a><span data-ttu-id="bfb60-111">Определение класса запуска OWIN</span><span class="sxs-lookup"><span data-stu-id="bfb60-111">OWIN Startup Class Detection</span></span>

 <span data-ttu-id="bfb60-112">Каждое приложение OWIN имеет класс запуска, где указываются компоненты конвейера приложения.</span><span class="sxs-lookup"><span data-stu-id="bfb60-112">Every OWIN Application has a startup class where you specify components for the application pipeline.</span></span> <span data-ttu-id="bfb60-113">Существует несколько способов, вы можете подключиться классе запуска в среде выполнения, в зависимости от модели размещения выберите (OwinHost, IIS и IIS Express).</span><span class="sxs-lookup"><span data-stu-id="bfb60-113">There are different ways you can connect your startup class with the runtime, depending on the hosting model you choose (OwinHost, IIS, and IIS-Express).</span></span> <span data-ttu-id="bfb60-114">Класс startup, в этом учебнике используется в каждое приложение размещения.</span><span class="sxs-lookup"><span data-stu-id="bfb60-114">The startup class shown in this tutorial can be used in every hosting application.</span></span> <span data-ttu-id="bfb60-115">Класс startup для соединения с размещения среды выполнения с помощью, один из этих приближается к:</span><span class="sxs-lookup"><span data-stu-id="bfb60-115">You connect the startup class with the hosting runtime using one of the these approaches:</span></span>

1. <span data-ttu-id="bfb60-116">**Соглашение об именовании**: Katana ищет класс с именем `Startup` в пространстве имен, имя сборки или глобальное пространство имен.</span><span class="sxs-lookup"><span data-stu-id="bfb60-116">**Naming Convention**: Katana looks for a class named `Startup` in namespace matching the assembly name or the global namespace.</span></span>
2. <span data-ttu-id="bfb60-117">**Атрибут OwinStartup**: Именно этот подход, большинство разработчиков потребуется указать класс startup.</span><span class="sxs-lookup"><span data-stu-id="bfb60-117">**OwinStartup Attribute**: This is the approach most developers will take to specify the startup class.</span></span> <span data-ttu-id="bfb60-118">Следующий атрибут будет присвоено класс startup `TestStartup` в класс `StartupDemo` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="bfb60-118">The following attribute will set the startup class to the `TestStartup` class in the `StartupDemo` namespace.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

   <span data-ttu-id="bfb60-119">`OwinStartup` Атрибут переопределяет соглашение об именовании.</span><span class="sxs-lookup"><span data-stu-id="bfb60-119">The `OwinStartup` attribute overrides the naming convention.</span></span> <span data-ttu-id="bfb60-120">Также можно указать понятное имя с этим атрибутом, тем не менее, используя понятное имя требует также использования `appSetting` элемент в файле конфигурации.</span><span class="sxs-lookup"><span data-stu-id="bfb60-120">You can also specify a friendly name with this attribute, however, using a friendly name requires you to also use the `appSetting` element in the configuration file.</span></span>
3. <span data-ttu-id="bfb60-121">**В файле конфигурации элемент appSetting**: `appSetting` Переопределяет `OwinStartup` атрибут и соглашение об именовании.</span><span class="sxs-lookup"><span data-stu-id="bfb60-121">**The appSetting element in the Configuration file**: The `appSetting` element overrides the `OwinStartup` attribute and naming convention.</span></span> <span data-ttu-id="bfb60-122">Можно иметь несколько классов запуска (каждый с помощью `OwinStartup` атрибут) и настройки, какой класс запуска будут загружены в файл конфигурации, используя разметку, аналогичную следующей:</span><span class="sxs-lookup"><span data-stu-id="bfb60-122">You can have multiple startup classes (each using an `OwinStartup` attribute) and configure which startup class will be loaded in a configuration file using markup similar to the following:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

   <span data-ttu-id="bfb60-123">Также можно использовать следующий раздел, который явно указывает класс startup и сборки:</span><span class="sxs-lookup"><span data-stu-id="bfb60-123">The following key, which explicitly specifies the startup class and assembly can also be used:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

   <span data-ttu-id="bfb60-124">Следующий код XML в файле конфигурации задает имя класса понятное запуска `ProductionConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="bfb60-124">The following XML in the configuration file specifies a friendly startup class name of `ProductionConfiguration`.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

   <span data-ttu-id="bfb60-125">Приведенная выше разметка должен использоваться со следующими `OwinStartup` атрибут, который задает понятное имя и вызывает `ProductionStartup2` класса для запуска.</span><span class="sxs-lookup"><span data-stu-id="bfb60-125">The above markup must be used with the following `OwinStartup` attribute which specifies a friendly name and causes the `ProductionStartup2` class to run.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. <span data-ttu-id="bfb60-126">Чтобы отключить обнаружение запуска OWIN добавьте `appSetting owin:AutomaticAppStartup` со значением `"false"` в файле web.config.</span><span class="sxs-lookup"><span data-stu-id="bfb60-126">To disable OWIN startup discovery add the `appSetting owin:AutomaticAppStartup` with a value of `"false"` in the web.config file.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a><span data-ttu-id="bfb60-127">Создать веб-приложение ASP.NET, с помощью запуска OWIN</span><span class="sxs-lookup"><span data-stu-id="bfb60-127">Create an ASP.NET Web App using OWIN Startup</span></span>

1. <span data-ttu-id="bfb60-128">Создайте пустое веб-приложение Asp.Net и назовите его **StartupDemo**.</span><span class="sxs-lookup"><span data-stu-id="bfb60-128">Create an empty Asp.Net web application and name it **StartupDemo**.</span></span> <span data-ttu-id="bfb60-129">-Установить `Microsoft.Owin.Host.SystemWeb` с помощью диспетчера пакетов NuGet.</span><span class="sxs-lookup"><span data-stu-id="bfb60-129">- Install `Microsoft.Owin.Host.SystemWeb` using the NuGet package manager.</span></span> <span data-ttu-id="bfb60-130">Из **средства** меню, выберите **диспетчер пакетов NuGet**, а затем **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="bfb60-130">From the **Tools** menu, select **NuGet Package Manager**, and then **Package Manager Console**.</span></span> <span data-ttu-id="bfb60-131">Введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="bfb60-131">Enter the following command:</span></span>

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
2. <span data-ttu-id="bfb60-132">Добавьте класс запуска OWIN.</span><span class="sxs-lookup"><span data-stu-id="bfb60-132">Add an OWIN startup class.</span></span> <span data-ttu-id="bfb60-133">В Visual Studio 2017, щелкните правой кнопкой мыши проект и выберите **Добавление класса**. — в **Добавление нового элемента** диалогового окна введите *OWIN* в поле поиска и измените имя на Startup.cs, а затем выберите **добавить**.</span><span class="sxs-lookup"><span data-stu-id="bfb60-133">In Visual Studio 2017 right-click the project and select **Add Class**.- In the **Add New Item** dialog box, enter *OWIN* in the search field, and change the name to Startup.cs, and then select **Add**.</span></span>

     ![](owin-startup-class-detection/_static/image1.png)

   <span data-ttu-id="bfb60-134">В следующий раз, вы хотите добавить *класс запуска Owin*, будет доступен из **добавить** меню.</span><span class="sxs-lookup"><span data-stu-id="bfb60-134">The next time you want to add an *Owin Startup class*, it will be in available from the **Add** menu.</span></span>

     ![](owin-startup-class-detection/_static/image2.png)

   <span data-ttu-id="bfb60-135">Кроме того, можно правой кнопкой мыши проект и выберите **добавить**, а затем выберите **новый элемент**, а затем выберите **класс запуска Owin**.</span><span class="sxs-lookup"><span data-stu-id="bfb60-135">Alternatively, you can right-click the project and select **Add**, then select **New Item**, and then select the **Owin Startup class**.</span></span>

     ![](owin-startup-class-detection/_static/image3.png)

- <span data-ttu-id="bfb60-136">Замените сгенерированный код в *Startup.cs* файл со следующими:</span><span class="sxs-lookup"><span data-stu-id="bfb60-136">Replace the generated code in the *Startup.cs* file with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]

  <span data-ttu-id="bfb60-137">`app.Use` Лямбда-выражение используется для регистрации компонента указанного по промежуточного слоя в конвейер OWIN.</span><span class="sxs-lookup"><span data-stu-id="bfb60-137">The `app.Use` lambda expression is used to register the specified middleware component to the OWIN pipeline.</span></span> <span data-ttu-id="bfb60-138">В этом случае мы собираемся настроить ведение журнала входящих запросов, прежде чем отвечать на входящий запрос.</span><span class="sxs-lookup"><span data-stu-id="bfb60-138">In this case we are setting up logging of incoming requests before responding to the incoming request.</span></span> <span data-ttu-id="bfb60-139">`next` Параметр является делегатом ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [задачи](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) к следующему компоненту в конвейере.</span><span class="sxs-lookup"><span data-stu-id="bfb60-139">The `next` parameter is the delegate ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [Task](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) to the next component in the pipeline.</span></span> <span data-ttu-id="bfb60-140">`app.Run` Лямбда-выражение подключает конвейер для входящих запросов и предоставляет механизм ответа.</span><span class="sxs-lookup"><span data-stu-id="bfb60-140">The `app.Run` lambda expression hooks up the pipeline to incoming requests and provides the response mechanism.</span></span>
     > [!NOTE]
     > <span data-ttu-id="bfb60-141">В приведенном выше коде мы закомментирован `OwinStartup` атрибут и мы полагаетесь на соглашению под управлением с именем класса `Startup` .-Press ***F5*** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="bfb60-141">In the code above we have commented out the `OwinStartup` attribute and we're relying on the convention of running the class named `Startup` .- Press ***F5*** to run the application.</span></span> <span data-ttu-id="bfb60-142">Несколько раз нажмите кнопку обновления.</span><span class="sxs-lookup"><span data-stu-id="bfb60-142">Hit refresh a few times.</span></span>

    <span data-ttu-id="bfb60-143">![](owin-startup-class-detection/_static/image4.png) Примечание. Значение, показанное в образы в этом руководстве не будет соответствовать номера, который отображается.</span><span class="sxs-lookup"><span data-stu-id="bfb60-143">![](owin-startup-class-detection/_static/image4.png) Note: The number shown in the images in this tutorial will not match the number you see.</span></span> <span data-ttu-id="bfb60-144">Миллисекунды строка используется для отображения нового ответа при обновлении страницы.</span><span class="sxs-lookup"><span data-stu-id="bfb60-144">The millisecond string is used to show a new response when you refresh the page.</span></span>
  <span data-ttu-id="bfb60-145">Вы увидите сведения о трассировке в **вывода** окна.</span><span class="sxs-lookup"><span data-stu-id="bfb60-145">You can see the trace information in the **Output** window.</span></span>

    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a><span data-ttu-id="bfb60-146">Добавить дополнительные классы запуска</span><span class="sxs-lookup"><span data-stu-id="bfb60-146">Add More Startup Classes</span></span>

<span data-ttu-id="bfb60-147">В этом разделе мы добавим еще один класс запуска.</span><span class="sxs-lookup"><span data-stu-id="bfb60-147">In this section we'll add another Startup class.</span></span> <span data-ttu-id="bfb60-148">Можно добавить несколько класс запуска OWIN в приложение.</span><span class="sxs-lookup"><span data-stu-id="bfb60-148">You can add multiple OWIN startup class to your application.</span></span> <span data-ttu-id="bfb60-149">Например можно создать классы запуска для разработки, тестирования и эксплуатации.</span><span class="sxs-lookup"><span data-stu-id="bfb60-149">For example, you might want to create startup classes for development, testing and production.</span></span>

1. <span data-ttu-id="bfb60-150">Создайте новый класс запуска OWIN и назовите его `ProductionStartup`.</span><span class="sxs-lookup"><span data-stu-id="bfb60-150">Create a new OWIN Startup class and name it `ProductionStartup`.</span></span>
2. <span data-ttu-id="bfb60-151">Замените сгенерированный код следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="bfb60-151">Replace the generated code with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. <span data-ttu-id="bfb60-152">Нажмите клавишу F5 элемента управления, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="bfb60-152">Press Control F5 to run the app.</span></span> <span data-ttu-id="bfb60-153">`OwinStartup` Атрибут указывает, выполняется класс startup рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="bfb60-153">The `OwinStartup` attribute specifies the production startup class is run.</span></span>

    ![](owin-startup-class-detection/_static/image6.png)
4. <span data-ttu-id="bfb60-154">Создайте еще один класс запуска OWIN и назовите его `TestStartup`.</span><span class="sxs-lookup"><span data-stu-id="bfb60-154">Create another OWIN Startup class and name it `TestStartup`.</span></span>
5. <span data-ttu-id="bfb60-155">Замените сгенерированный код следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="bfb60-155">Replace the generated code with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

   <span data-ttu-id="bfb60-156">`OwinStartup` Перегрузка атрибут выше указывает `TestingConfiguration` как *понятное* имя класса Startup.</span><span class="sxs-lookup"><span data-stu-id="bfb60-156">The `OwinStartup` attribute overload above specifies `TestingConfiguration` as the *friendly* name of the Startup class.</span></span>
6. <span data-ttu-id="bfb60-157">Откройте *web.config* файл и добавьте ключ запуска приложения OWIN, который указывает понятное имя в классе Startup:</span><span class="sxs-lookup"><span data-stu-id="bfb60-157">Open the *web.config* file and add the OWIN App startup key which specifies the friendly name of the Startup class:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. <span data-ttu-id="bfb60-158">Нажмите клавишу F5 элемента управления, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="bfb60-158">Press Control F5 to run the app.</span></span> <span data-ttu-id="bfb60-159">Элемент параметров приложения имеет высокий приоритет и тест запускается конфигурации.</span><span class="sxs-lookup"><span data-stu-id="bfb60-159">The app settings element takes precedent, and the test configuration is run.</span></span>

    ![](owin-startup-class-detection/_static/image7.png)
8. <span data-ttu-id="bfb60-160">Удалить *понятное* имя из `OwinStartup` атрибут в `TestStartup` класса.</span><span class="sxs-lookup"><span data-stu-id="bfb60-160">Remove the *friendly* name from the `OwinStartup` attribute in the `TestStartup` class.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. <span data-ttu-id="bfb60-161">Замените ключ запуска приложения OWIN в *web.config* файл со следующими:</span><span class="sxs-lookup"><span data-stu-id="bfb60-161">Replace the OWIN App startup key in the *web.config* file with the following:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. <span data-ttu-id="bfb60-162">Отменить `OwinStartup` атрибута этих классов в код атрибута по умолчанию, созданный средой Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="bfb60-162">Revert the `OwinStartup` attribute in each class to the default attribute code generated by Visual Studio:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

    <span data-ttu-id="bfb60-163">Каждый ключ запуска приложения OWIN ниже приведет к производственного класса для запуска.</span><span class="sxs-lookup"><span data-stu-id="bfb60-163">Each of the OWIN App startup keys below will cause the production class to run.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

    <span data-ttu-id="bfb60-164">Ключ последнего запуска способ конфигурации запуска.</span><span class="sxs-lookup"><span data-stu-id="bfb60-164">The last startup key specifies the startup configuration method.</span></span> <span data-ttu-id="bfb60-165">Следующий раздел запуска приложения OWIN позволяет изменить имя класса конфигурации для `MyConfiguration` .</span><span class="sxs-lookup"><span data-stu-id="bfb60-165">The following OWIN App startup key allows you to change the name of the configuration class to `MyConfiguration` .</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a><span data-ttu-id="bfb60-166">С помощью Owinhost.exe</span><span class="sxs-lookup"><span data-stu-id="bfb60-166">Using Owinhost.exe</span></span>

1. <span data-ttu-id="bfb60-167">Замените файл Web.config следующую разметку:</span><span class="sxs-lookup"><span data-stu-id="bfb60-167">Replace the Web.config file with the following markup:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

   <span data-ttu-id="bfb60-168">Ключ последнего wins, поэтому в данном случае `TestStartup` указан.</span><span class="sxs-lookup"><span data-stu-id="bfb60-168">The last key wins, so in this case `TestStartup` is specified.</span></span>
2. <span data-ttu-id="bfb60-169">Установка Owinhost из PMC:</span><span class="sxs-lookup"><span data-stu-id="bfb60-169">Install Owinhost from the PMC:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. <span data-ttu-id="bfb60-170">Перейдите в папку приложения (в папку, содержащую *Web.config* файл) и в командную строку и введите:</span><span class="sxs-lookup"><span data-stu-id="bfb60-170">Navigate to the application folder (the folder containing the *Web.config* file) and in a command prompt and type:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

   <span data-ttu-id="bfb60-171">Командное окно будет показано следующее:</span><span class="sxs-lookup"><span data-stu-id="bfb60-171">The command window will show:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. <span data-ttu-id="bfb60-172">Окно браузера с URL-адрес `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="bfb60-172">Launch a browser with the URL `http://localhost:5000/`.</span></span>

    ![](owin-startup-class-detection/_static/image8.png)

   <span data-ttu-id="bfb60-173">OwinHost соблюдаться соглашения, запуска, перечисленные выше.</span><span class="sxs-lookup"><span data-stu-id="bfb60-173">OwinHost honored the startup conventions listed above.</span></span>
5. <span data-ttu-id="bfb60-174">В окне командной строки нажмите клавишу ВВОД для выхода OwinHost.</span><span class="sxs-lookup"><span data-stu-id="bfb60-174">In the command window, press Enter to exit OwinHost.</span></span>
6. <span data-ttu-id="bfb60-175">В `ProductionStartup` добавьте следующий атрибут OwinStartup, который указывает понятное имя *ProductionConfiguration*.</span><span class="sxs-lookup"><span data-stu-id="bfb60-175">In the `ProductionStartup` class, add the following OwinStartup attribute which specifies a friendly name of *ProductionConfiguration*.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. <span data-ttu-id="bfb60-176">В командную строку и введите:</span><span class="sxs-lookup"><span data-stu-id="bfb60-176">In the command prompt and type:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

   <span data-ttu-id="bfb60-177">Класс startup рабочей загрузки.</span><span class="sxs-lookup"><span data-stu-id="bfb60-177">The Production startup class is loaded.</span></span>
    ![](owin-startup-class-detection/_static/image9.png)

   <span data-ttu-id="bfb60-178">Наше приложение имеет несколько классов запуска, а в этом примере мы имеют отсроченные какой класс startup для загрузки только во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="bfb60-178">Our application has multiple startup classes, and in this example we have deferred which startup class to load until runtime.</span></span>
8. <span data-ttu-id="bfb60-179">Проверьте следующие параметры запуска среды выполнения:</span><span class="sxs-lookup"><span data-stu-id="bfb60-179">Test the following runtime startup options:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
