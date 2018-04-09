---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: Модульное тестирование приложений SignalR | Документы Microsoft
author: pfletcher
description: В этой статье описывается использование возможностей модульное тестирование SignalR 2.0.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: cff866716cb1179e02b930f33cb0f8c33d4a6cf0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="unit-testing-signalr-applications"></a><span data-ttu-id="a8e51-103">Тестирование приложений SignalR единицы</span><span class="sxs-lookup"><span data-stu-id="a8e51-103">Unit Testing SignalR Applications</span></span>
====================
<span data-ttu-id="a8e51-104">по [Патрик Флетчера](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="a8e51-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="a8e51-105">Этой статье описывается использование возможностей модульное тестирование SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="a8e51-105">This article describes using the Unit Testing features of SignalR 2.</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="a8e51-106">Версии программного обеспечения, используемого в этом разделе</span><span class="sxs-lookup"><span data-stu-id="a8e51-106">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="a8e51-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="a8e51-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="a8e51-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="a8e51-108">.NET 4.5</span></span>
> - <span data-ttu-id="a8e51-109">SignalR версии 2</span><span class="sxs-lookup"><span data-stu-id="a8e51-109">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="a8e51-110">Вопросы и комментарии</span><span class="sxs-lookup"><span data-stu-id="a8e51-110">Questions and comments</span></span>
> 
> <span data-ttu-id="a8e51-111">Оставьте отзыв на том, как вам понравилось этого учебника и что можно улучшить в комментарии в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="a8e51-111">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="a8e51-112">Если у вас есть вопросы, которые не связаны непосредственно для работы с учебником, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) или [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="a8e51-112">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a><span data-ttu-id="a8e51-113">Модульное тестирование приложений SignalR</span><span class="sxs-lookup"><span data-stu-id="a8e51-113">Unit testing SignalR applications</span></span>

<span data-ttu-id="a8e51-114">Функции модульного теста в SignalR 2 можно использовать для создания модульных тестов для приложения SignalR.</span><span class="sxs-lookup"><span data-stu-id="a8e51-114">You can use the unit test features in SignalR 2 to create unit tests for your SignalR application.</span></span> <span data-ttu-id="a8e51-115">Включает SignalR 2 [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) интерфейс, который можно использовать для создания макетов объекта для имитации для тестирования методов концентратора.</span><span class="sxs-lookup"><span data-stu-id="a8e51-115">SignalR 2 includes the [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) interface, which can be used to create a mock object to simulate your hub methods for testing.</span></span>

<span data-ttu-id="a8e51-116">В этом разделе вы добавите модульных тестов для приложения, созданного в [учебник по началу работы](../getting-started/tutorial-getting-started-with-signalr.md) с помощью [XUnit.net](https://github.com/xunit/xunit) и [заказа](https://github.com/Moq/moq4).</span><span class="sxs-lookup"><span data-stu-id="a8e51-116">In this section, you'll add unit tests for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using [XUnit.net](https://github.com/xunit/xunit) and [Moq](https://github.com/Moq/moq4).</span></span>

<span data-ttu-id="a8e51-117">XUnit.net будет использоваться для управления теста; Заказа будет использоваться для создания [макета](http://en.wikipedia.org/wiki/Mock_object) объект для тестирования.</span><span class="sxs-lookup"><span data-stu-id="a8e51-117">XUnit.net will be used to control the test; Moq will be used to create a [mock](http://en.wikipedia.org/wiki/Mock_object) object for testing.</span></span> <span data-ttu-id="a8e51-118">При желании; можно использовать другие платформы имитации [NSubstitute](http://nsubstitute.github.io/) также будет хорошим выбором.</span><span class="sxs-lookup"><span data-stu-id="a8e51-118">Other mocking frameworks can be used if desired; [NSubstitute](http://nsubstitute.github.io/) is also a good choice.</span></span> <span data-ttu-id="a8e51-119">Этот учебник посвящен Настройка макетов объекта двумя способами: во-первых, с помощью `dynamic` объекта (впервые появилась в .NET Framework 4), во-вторых, с помощью интерфейса.</span><span class="sxs-lookup"><span data-stu-id="a8e51-119">This tutorial demonstrates how to set up the mock object in two ways: First, using a `dynamic` object (introduced in .NET Framework 4), and second, using an interface.</span></span>

### <a name="contents"></a><span data-ttu-id="a8e51-120">Описание</span><span class="sxs-lookup"><span data-stu-id="a8e51-120">Contents</span></span>

<span data-ttu-id="a8e51-121">Этот учебник содержит следующие разделы.</span><span class="sxs-lookup"><span data-stu-id="a8e51-121">This tutorial contains the following sections.</span></span>

- [<span data-ttu-id="a8e51-122">Модульное тестирование с помощью динамической</span><span class="sxs-lookup"><span data-stu-id="a8e51-122">Unit testing with Dynamic</span></span>](#dynamic)
- [<span data-ttu-id="a8e51-123">Модульное тестирование по типу</span><span class="sxs-lookup"><span data-stu-id="a8e51-123">Unit testing by type</span></span>](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a><span data-ttu-id="a8e51-124">Модульное тестирование с помощью динамической</span><span class="sxs-lookup"><span data-stu-id="a8e51-124">Unit testing with Dynamic</span></span>

<span data-ttu-id="a8e51-125">В этом разделе вы добавите модульный тест для приложения, созданного в [учебник по началу работы](../getting-started/tutorial-getting-started-with-signalr.md) с помощью динамического объекта.</span><span class="sxs-lookup"><span data-stu-id="a8e51-125">In this section, you'll add a unit test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using a dynamic object.</span></span>

1. <span data-ttu-id="a8e51-126">Установка [XUnit Runner расширения](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) для Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="a8e51-126">Install the [XUnit Runner extension](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) for Visual Studio 2013.</span></span>
2. <span data-ttu-id="a8e51-127">Либо выполните [учебник по началу работы](../getting-started/tutorial-getting-started-with-signalr.md), или загрузить завершенное приложение из [коллекции кода MSDN](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span><span class="sxs-lookup"><span data-stu-id="a8e51-127">Either complete the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the completed application from [MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
3. <span data-ttu-id="a8e51-128">Если вы используете версию загрузки приложения Приступая к работе, откройте **консоль диспетчера пакетов** и нажмите кнопку **восстановить** для добавления в проект пакета SignalR.</span><span class="sxs-lookup"><span data-stu-id="a8e51-128">If you are using the download version of the Getting Started application, open **Package Manager Console** and click **Restore** to add the SignalR package to the project.</span></span>

    ![Восстановление пакетов](unit-testing-signalr-applications/_static/image1.png)
4. <span data-ttu-id="a8e51-130">Добавьте проект в решение для модульного теста.</span><span class="sxs-lookup"><span data-stu-id="a8e51-130">Add a project to the solution for the unit test.</span></span> <span data-ttu-id="a8e51-131">Щелкните правой кнопкой мыши решение в **обозревателе решений** и выберите **добавить**, **новый проект...** . В разделе **C#** выберите **Windows** узла.</span><span class="sxs-lookup"><span data-stu-id="a8e51-131">Right-click your solution in **Solution Explorer** and select **Add**, **New Project...**. Under the **C#** node, select the **Windows** node.</span></span> <span data-ttu-id="a8e51-132">Выберите **библиотека классов**.</span><span class="sxs-lookup"><span data-stu-id="a8e51-132">Select **Class Library**.</span></span> <span data-ttu-id="a8e51-133">Имя для нового проекта **TestLibrary** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="a8e51-133">Name the new project **TestLibrary** and click **OK**.</span></span>

    ![Создание библиотеки тестов](unit-testing-signalr-applications/_static/image2.png)
5. <span data-ttu-id="a8e51-135">Добавьте ссылку в тестовом проекте библиотеки в проект SignalRChat.</span><span class="sxs-lookup"><span data-stu-id="a8e51-135">Add a reference in the test library project to the SignalRChat project.</span></span> <span data-ttu-id="a8e51-136">Щелкните правой кнопкой мыши **TestLibrary** проект и выберите **добавить**, **ссылку...** . Выберите **проекты** узле **решения** узла, а также проверьте **SignalRChat**.</span><span class="sxs-lookup"><span data-stu-id="a8e51-136">Right-click the **TestLibrary** project and select **Add**, **Reference...**. Select the **Projects** node under the **Solution** node, and check **SignalRChat**.</span></span> <span data-ttu-id="a8e51-137">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="a8e51-137">Click **OK**.</span></span>

    ![Добавить ссылку на проект](unit-testing-signalr-applications/_static/image3.png)
6. <span data-ttu-id="a8e51-139">Добавление пакетов, SignalR, заказа и XUnit **TestLibrary** проекта.</span><span class="sxs-lookup"><span data-stu-id="a8e51-139">Add the SignalR, Moq, and XUnit packages to the **TestLibrary** project.</span></span> <span data-ttu-id="a8e51-140">В **консоль диспетчера пакетов**, задайте **проект по умолчанию** раскрывающийся список для **TestLibrary**.</span><span class="sxs-lookup"><span data-stu-id="a8e51-140">In the **Package Manager Console**, set the **Default Project** dropdown to **TestLibrary**.</span></span> <span data-ttu-id="a8e51-141">В окне консоли, выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="a8e51-141">Run the following commands in the console window:</span></span>

   - `Install-Package Microsoft.AspNet.SignalR`
   - `Install-Package Moq`
   - `Install-Package XUnit`

     ![Установить пакеты](unit-testing-signalr-applications/_static/image4.png)
7. <span data-ttu-id="a8e51-143">Создайте файл теста.</span><span class="sxs-lookup"><span data-stu-id="a8e51-143">Create the test file.</span></span> <span data-ttu-id="a8e51-144">Щелкните правой кнопкой мыши **TestLibrary** проекта и нажмите кнопку **добавить...** , **Класса**.</span><span class="sxs-lookup"><span data-stu-id="a8e51-144">Right-click the **TestLibrary** project and click **Add...**, **Class**.</span></span> <span data-ttu-id="a8e51-145">Имя для нового класса **Tests.cs**.</span><span class="sxs-lookup"><span data-stu-id="a8e51-145">Name the new class **Tests.cs**.</span></span>
8. <span data-ttu-id="a8e51-146">Замените содержимое Tests.cs следующий код.</span><span class="sxs-lookup"><span data-stu-id="a8e51-146">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    <span data-ttu-id="a8e51-147">В приведенном выше коде тестовый клиент создается с помощью `Mock` объекта из [заказа](https://github.com/Moq/moq4) библиотеки типа [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (в версии 2.1 SignalR, назначьте `dynamic` для типа параметр). `IHubCallerConnectionContext` Интерфейс является прокси-объект, с помощью которого можно вызывать методы на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="a8e51-147">In the code above, a test client is created using the `Mock` object from the [Moq](https://github.com/Moq/moq4) library, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span> <span data-ttu-id="a8e51-148">`broadcastMessage` Функция затем определено для макетов клиента таким образом, он может быть вызван `ChatHub` класса.</span><span class="sxs-lookup"><span data-stu-id="a8e51-148">The `broadcastMessage` function is then defined for the mock client so that it can be called by the `ChatHub` class.</span></span> <span data-ttu-id="a8e51-149">Затем вызывается обработчик тестов `Send` метод `ChatHub` класс, который в свою очередь вызывает макеты `broadcastMessage` функции.</span><span class="sxs-lookup"><span data-stu-id="a8e51-149">The test engine then calls the `Send` method of the `ChatHub` class, which in turn calls the mocked `broadcastMessage` function.</span></span>
9. <span data-ttu-id="a8e51-150">Постройте решение, нажав клавишу **F6**.</span><span class="sxs-lookup"><span data-stu-id="a8e51-150">Build the solution by pressing **F6**.</span></span>
10. <span data-ttu-id="a8e51-151">Выполните модульный тест.</span><span class="sxs-lookup"><span data-stu-id="a8e51-151">Run the unit test.</span></span> <span data-ttu-id="a8e51-152">В Visual Studio, выберите **тест**, **Windows**, **Test Explorer**.</span><span class="sxs-lookup"><span data-stu-id="a8e51-152">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="a8e51-153">Щелкните правой кнопкой мыши в окне обозревателя тестов **HubsAreMockableViaDynamic** и выберите **выполнение выбранных тестов**.</span><span class="sxs-lookup"><span data-stu-id="a8e51-153">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![Обозреватель тестов](unit-testing-signalr-applications/_static/image5.png)
11. <span data-ttu-id="a8e51-155">Убедитесь, что тест был пройден, установив на нижней панели в окне обозревателя тестов.</span><span class="sxs-lookup"><span data-stu-id="a8e51-155">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="a8e51-156">Окно покажет, что тест был пройден.</span><span class="sxs-lookup"><span data-stu-id="a8e51-156">The window will show that the test passed.</span></span>

    ![Тест пройден](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a><span data-ttu-id="a8e51-158">Модульное тестирование по типу</span><span class="sxs-lookup"><span data-stu-id="a8e51-158">Unit testing by type</span></span>

<span data-ttu-id="a8e51-159">В этом разделе вы добавите теста для приложения, созданные в [учебник по началу работы](../getting-started/tutorial-getting-started-with-signalr.md) с помощью интерфейс, который содержит метод для тестирования.</span><span class="sxs-lookup"><span data-stu-id="a8e51-159">In this section, you'll add a test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using an interface that contains the method to be tested.</span></span>

1. <span data-ttu-id="a8e51-160">Выполните шаги 1 – 7 [модульного тестирования с динамической](#dynamic) учебника выше.</span><span class="sxs-lookup"><span data-stu-id="a8e51-160">Complete steps 1-7 in the [Unit testing with Dynamic](#dynamic) tutorial above.</span></span>
2. <span data-ttu-id="a8e51-161">Замените содержимое Tests.cs следующий код.</span><span class="sxs-lookup"><span data-stu-id="a8e51-161">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    <span data-ttu-id="a8e51-162">В приведенном выше коде интерфейс создается определение сигнатура `broadcastMessage` метода, для которого обработчик тестов будет создание макетов клиента.</span><span class="sxs-lookup"><span data-stu-id="a8e51-162">In the code above, an interface is created defining the signature of the `broadcastMessage` method for which the test engine will create a mock client.</span></span> <span data-ttu-id="a8e51-163">Макеты клиента создается с помощью `Mock` объекта типа [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (в версии 2.1 SignalR, назначьте `dynamic` для параметра типа.) `IHubCallerConnectionContext` Интерфейс является прокси-объект, с помощью которого можно вызывать методы на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="a8e51-163">A mock client is then created using the `Mock` object, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span>

    <span data-ttu-id="a8e51-164">Тест, затем создает экземпляр `ChatHub`, а затем создает имитационную версию объекта `broadcastMessage` метод, который в свою очередь, вызывается путем вызова метода `Send` метод на концентраторе.</span><span class="sxs-lookup"><span data-stu-id="a8e51-164">The test then creates an instance of `ChatHub`, and then creates a mock version of the `broadcastMessage` method, which in turn is invoked by calling the `Send` method on the hub.</span></span>
3. <span data-ttu-id="a8e51-165">Постройте решение, нажав клавишу **F6**.</span><span class="sxs-lookup"><span data-stu-id="a8e51-165">Build the solution by pressing **F6**.</span></span>
4. <span data-ttu-id="a8e51-166">Выполните модульный тест.</span><span class="sxs-lookup"><span data-stu-id="a8e51-166">Run the unit test.</span></span> <span data-ttu-id="a8e51-167">В Visual Studio, выберите **тест**, **Windows**, **Test Explorer**.</span><span class="sxs-lookup"><span data-stu-id="a8e51-167">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="a8e51-168">Щелкните правой кнопкой мыши в окне обозревателя тестов **HubsAreMockableViaDynamic** и выберите **выполнение выбранных тестов**.</span><span class="sxs-lookup"><span data-stu-id="a8e51-168">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![Обозреватель тестов](unit-testing-signalr-applications/_static/image7.png)
5. <span data-ttu-id="a8e51-170">Убедитесь, что тест был пройден, установив на нижней панели в окне обозревателя тестов.</span><span class="sxs-lookup"><span data-stu-id="a8e51-170">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="a8e51-171">Окно покажет, что тест был пройден.</span><span class="sxs-lookup"><span data-stu-id="a8e51-171">The window will show that the test passed.</span></span>

    ![Тест пройден](unit-testing-signalr-applications/_static/image8.png)
