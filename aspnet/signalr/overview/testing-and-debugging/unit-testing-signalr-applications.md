---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: Модульное тестирование приложений SignalR | Документация Майкрософт
author: pfletcher
description: В этой статье описывается использование функций Unit Testing SignalR 2.0.
ms.author: riande
ms.date: 06/10/2014
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: ba8f5d4577403fe9765641d7ee5d88bde045680a
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910828"
---
<a name="unit-testing-signalr-applications"></a><span data-ttu-id="3b1ad-103">Модульное тестирование приложений SignalR</span><span class="sxs-lookup"><span data-stu-id="3b1ad-103">Unit Testing SignalR Applications</span></span>
====================
<span data-ttu-id="3b1ad-104">по [Патрик Флетчера](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="3b1ad-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="3b1ad-105">В этой статье описывается использование функции Unit Testing SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-105">This article describes using the Unit Testing features of SignalR 2.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="3b1ad-106">Версии программного обеспечения, используемого в этом разделе</span><span class="sxs-lookup"><span data-stu-id="3b1ad-106">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="3b1ad-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="3b1ad-107">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="3b1ad-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="3b1ad-108">.NET 4.5</span></span>
> - <span data-ttu-id="3b1ad-109">SignalR версии 2</span><span class="sxs-lookup"><span data-stu-id="3b1ad-109">SignalR version 2</span></span>
>
>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="3b1ad-110">Вопросы и комментарии</span><span class="sxs-lookup"><span data-stu-id="3b1ad-110">Questions and comments</span></span>
>
> <span data-ttu-id="3b1ad-111">Оставьте свои отзывы на том, как вам понравилось, и этот учебник и что можно улучшить в комментариях в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-111">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="3b1ad-112">Если у вас есть вопросы, которые не имеют отношения к руководству, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) или [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="3b1ad-112">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a><span data-ttu-id="3b1ad-113">Модульное тестирование приложений SignalR</span><span class="sxs-lookup"><span data-stu-id="3b1ad-113">Unit testing SignalR applications</span></span>

<span data-ttu-id="3b1ad-114">Функции модульных тестов в SignalR 2 можно использовать для создания модульных тестов для приложения SignalR.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-114">You can use the unit test features in SignalR 2 to create unit tests for your SignalR application.</span></span> <span data-ttu-id="3b1ad-115">SignalR 2 включает [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) интерфейс, который может использоваться для создания объекта макеты для имитации свои методы концентратора для тестирования.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-115">SignalR 2 includes the [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) interface, which can be used to create a mock object to simulate your hub methods for testing.</span></span>

<span data-ttu-id="3b1ad-116">В этом разделе вы добавите модульных тестов для приложения, созданного в [учебнике Приступая к работе](../getting-started/tutorial-getting-started-with-signalr.md) с помощью [XUnit.net](https://github.com/xunit/xunit) и [Moq](https://github.com/Moq/moq4).</span><span class="sxs-lookup"><span data-stu-id="3b1ad-116">In this section, you'll add unit tests for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using [XUnit.net](https://github.com/xunit/xunit) and [Moq](https://github.com/Moq/moq4).</span></span>

<span data-ttu-id="3b1ad-117">XUnit.net будет использоваться для контроля теста; MOQ будет использоваться для создания [макетирование](http://en.wikipedia.org/wiki/Mock_object) объект для тестирования.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-117">XUnit.net will be used to control the test; Moq will be used to create a [mock](http://en.wikipedia.org/wiki/Mock_object) object for testing.</span></span> <span data-ttu-id="3b1ad-118">Можно использовать другие платформы макетирования, при необходимости; [NSubstitute](http://nsubstitute.github.io/) также хорошо подходит.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-118">Other mocking frameworks can be used if desired; [NSubstitute](http://nsubstitute.github.io/) is also a good choice.</span></span> <span data-ttu-id="3b1ad-119">Этом руководстве показано, как настроить макета объекта двумя способами: во-первых, с помощью `dynamic` объекта (появился в .NET Framework 4), во-вторых, с помощью интерфейса.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-119">This tutorial demonstrates how to set up the mock object in two ways: First, using a `dynamic` object (introduced in .NET Framework 4), and second, using an interface.</span></span>

### <a name="contents"></a><span data-ttu-id="3b1ad-120">Описание</span><span class="sxs-lookup"><span data-stu-id="3b1ad-120">Contents</span></span>

<span data-ttu-id="3b1ad-121">Этот учебник содержит следующие разделы.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-121">This tutorial contains the following sections.</span></span>

- [<span data-ttu-id="3b1ad-122">Модульное тестирование с помощью динамической</span><span class="sxs-lookup"><span data-stu-id="3b1ad-122">Unit testing with Dynamic</span></span>](#dynamic)
- [<span data-ttu-id="3b1ad-123">Модульное тестирование по типу</span><span class="sxs-lookup"><span data-stu-id="3b1ad-123">Unit testing by type</span></span>](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a><span data-ttu-id="3b1ad-124">Модульное тестирование с помощью динамической</span><span class="sxs-lookup"><span data-stu-id="3b1ad-124">Unit testing with Dynamic</span></span>

<span data-ttu-id="3b1ad-125">В этом разделе вы добавите модульный тест для приложения, созданного в [учебнике Приступая к работе](../getting-started/tutorial-getting-started-with-signalr.md) с помощью динамического объекта.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-125">In this section, you'll add a unit test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using a dynamic object.</span></span>

1. <span data-ttu-id="3b1ad-126">Установка [расширения средство запуска XUnit](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) для Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-126">Install the [XUnit Runner extension](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) for Visual Studio 2013.</span></span>
2. <span data-ttu-id="3b1ad-127">Либо выполните [учебнике Приступая к работе](../getting-started/tutorial-getting-started-with-signalr.md), или загрузить готовое приложение из [коллекции кода MSDN](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span><span class="sxs-lookup"><span data-stu-id="3b1ad-127">Either complete the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the completed application from [MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
3. <span data-ttu-id="3b1ad-128">Если вы используете версию загрузки, Приступая к работе приложения, откройте **консоль диспетчера пакетов** и нажмите кнопку **восстановить** Добавление SignalR пакета в проект.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-128">If you are using the download version of the Getting Started application, open **Package Manager Console** and click **Restore** to add the SignalR package to the project.</span></span>

    ![Восстановление пакетов](unit-testing-signalr-applications/_static/image1.png)
4. <span data-ttu-id="3b1ad-130">Добавление проекта к решению для модульного теста.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-130">Add a project to the solution for the unit test.</span></span> <span data-ttu-id="3b1ad-131">Щелкните правой кнопкой мыши решение в **обозревателе решений** и выберите **добавить**, **новый проект...** . В разделе **C#** выберите **Windows** узла.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-131">Right-click your solution in **Solution Explorer** and select **Add**, **New Project...**. Under the **C#** node, select the **Windows** node.</span></span> <span data-ttu-id="3b1ad-132">Выберите **библиотека классов**.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-132">Select **Class Library**.</span></span> <span data-ttu-id="3b1ad-133">Назовите новый проект **TestLibrary** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-133">Name the new project **TestLibrary** and click **OK**.</span></span>

    ![Создание тестов библиотеки](unit-testing-signalr-applications/_static/image2.png)
5. <span data-ttu-id="3b1ad-135">Добавьте ссылку в тестовом проекте библиотеки в проект SignalRChat.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-135">Add a reference in the test library project to the SignalRChat project.</span></span> <span data-ttu-id="3b1ad-136">Щелкните правой кнопкой мыши **TestLibrary** проекта и выберите **добавить**, **ссылку...** . Выберите **проекты** узле **решение** узел и проверьте **SignalRChat**.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-136">Right-click the **TestLibrary** project and select **Add**, **Reference...**. Select the **Projects** node under the **Solution** node, and check **SignalRChat**.</span></span> <span data-ttu-id="3b1ad-137">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-137">Click **OK**.</span></span>

    ![Добавить ссылку на проект](unit-testing-signalr-applications/_static/image3.png)
6. <span data-ttu-id="3b1ad-139">Добавьте пакеты SignalR, Moq и XUnit для **TestLibrary** проекта.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-139">Add the SignalR, Moq, and XUnit packages to the **TestLibrary** project.</span></span> <span data-ttu-id="3b1ad-140">В **консоль диспетчера пакетов**, задайте **проект по умолчанию** раскрывающийся список, чтобы **TestLibrary**.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-140">In the **Package Manager Console**, set the **Default Project** dropdown to **TestLibrary**.</span></span> <span data-ttu-id="3b1ad-141">Выполните следующие команды в окне консоли:</span><span class="sxs-lookup"><span data-stu-id="3b1ad-141">Run the following commands in the console window:</span></span>

   - `Install-Package Microsoft.AspNet.SignalR`
   - `Install-Package Moq`
   - `Install-Package XUnit`

     ![Установка пакетов](unit-testing-signalr-applications/_static/image4.png)
7. <span data-ttu-id="3b1ad-143">Создайте тестовый файл.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-143">Create the test file.</span></span> <span data-ttu-id="3b1ad-144">Щелкните правой кнопкой мыши **TestLibrary** проекта и нажмите кнопку **добавить...** , **Класс**.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-144">Right-click the **TestLibrary** project and click **Add...**, **Class**.</span></span> <span data-ttu-id="3b1ad-145">Назовите новый класс **Tests.cs**.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-145">Name the new class **Tests.cs**.</span></span>
8. <span data-ttu-id="3b1ad-146">Замените содержимое Tests.cs следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-146">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    <span data-ttu-id="3b1ad-147">В приведенном выше коде тестовый клиент создается с помощью `Mock` объекта из [Moq](https://github.com/Moq/moq4) библиотеки типа [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (в SignalR 2.1, назначьте `dynamic` для типа параметр.) `IHubCallerConnectionContext` Интерфейс является прокси-объект, с помощью которого можно вызывать методы на клиент.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-147">In the code above, a test client is created using the `Mock` object from the [Moq](https://github.com/Moq/moq4) library, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span> <span data-ttu-id="3b1ad-148">`broadcastMessage` Функция затем определена для макетов клиента, чтобы к нему можно обращаться `ChatHub` класса.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-148">The `broadcastMessage` function is then defined for the mock client so that it can be called by the `ChatHub` class.</span></span> <span data-ttu-id="3b1ad-149">Затем вызывается обработчик тестов `Send` метод `ChatHub` класс, который в свою очередь вызывает макеты `broadcastMessage` функции.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-149">The test engine then calls the `Send` method of the `ChatHub` class, which in turn calls the mocked `broadcastMessage` function.</span></span>
9. <span data-ttu-id="3b1ad-150">Постройте решение, нажав клавишу **F6**.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-150">Build the solution by pressing **F6**.</span></span>
10. <span data-ttu-id="3b1ad-151">Выполните модульный тест.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-151">Run the unit test.</span></span> <span data-ttu-id="3b1ad-152">В Visual Studio выберите **теста**, **Windows**, **обозреватель тестов**.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-152">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="3b1ad-153">Щелкните правой кнопкой мыши в окне обозревателя тестов **HubsAreMockableViaDynamic** и выберите **запуск выбранных тестов**.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-153">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![Обозреватель тестов](unit-testing-signalr-applications/_static/image5.png)
11. <span data-ttu-id="3b1ad-155">Убедитесь, что тест был пройден, проверив нижней области в окне обозревателя тестов.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-155">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="3b1ad-156">В окне отображаются, что тест был пройден.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-156">The window will show that the test passed.</span></span>

    ![Тест пройден](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a><span data-ttu-id="3b1ad-158">Модульное тестирование по типу</span><span class="sxs-lookup"><span data-stu-id="3b1ad-158">Unit testing by type</span></span>

<span data-ttu-id="3b1ad-159">В этом разделе вы добавите теста для приложения, созданного в [учебнике Приступая к работе](../getting-started/tutorial-getting-started-with-signalr.md) с помощью интерфейса, который содержит метод, чтобы протестировать.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-159">In this section, you'll add a test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using an interface that contains the method to be tested.</span></span>

1. <span data-ttu-id="3b1ad-160">Выполните шаги 1 – 7 в [модульное тестирование с помощью динамической](#dynamic) выше руководства.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-160">Complete steps 1-7 in the [Unit testing with Dynamic](#dynamic) tutorial above.</span></span>
2. <span data-ttu-id="3b1ad-161">Замените содержимое Tests.cs следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-161">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    <span data-ttu-id="3b1ad-162">В приведенном выше коде интерфейс создается определение подпись `broadcastMessage` метод, для которого обработчик тестов будет создан клиент макетов.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-162">In the code above, an interface is created defining the signature of the `broadcastMessage` method for which the test engine will create a mock client.</span></span> <span data-ttu-id="3b1ad-163">Макеты клиент создается с помощью `Mock` объект типа [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (в SignalR 2.1, назначьте `dynamic` для параметра типа.) `IHubCallerConnectionContext` Интерфейс является прокси-объект, с помощью которого можно вызывать методы на клиент.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-163">A mock client is then created using the `Mock` object, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span>

    <span data-ttu-id="3b1ad-164">Тест, затем создает экземпляр класса `ChatHub`, а затем создает имитационную версию объекта `broadcastMessage` метод, который в свою очередь, вызывается путем вызова метода `Send` метод на концентраторе.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-164">The test then creates an instance of `ChatHub`, and then creates a mock version of the `broadcastMessage` method, which in turn is invoked by calling the `Send` method on the hub.</span></span>
3. <span data-ttu-id="3b1ad-165">Постройте решение, нажав клавишу **F6**.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-165">Build the solution by pressing **F6**.</span></span>
4. <span data-ttu-id="3b1ad-166">Выполните модульный тест.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-166">Run the unit test.</span></span> <span data-ttu-id="3b1ad-167">В Visual Studio выберите **теста**, **Windows**, **обозреватель тестов**.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-167">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="3b1ad-168">Щелкните правой кнопкой мыши в окне обозревателя тестов **HubsAreMockableViaDynamic** и выберите **запуск выбранных тестов**.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-168">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![Обозреватель тестов](unit-testing-signalr-applications/_static/image7.png)
5. <span data-ttu-id="3b1ad-170">Убедитесь, что тест был пройден, проверив нижней области в окне обозревателя тестов.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-170">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="3b1ad-171">В окне отображаются, что тест был пройден.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-171">The window will show that the test passed.</span></span>

    ![Тест пройден](unit-testing-signalr-applications/_static/image8.png)
