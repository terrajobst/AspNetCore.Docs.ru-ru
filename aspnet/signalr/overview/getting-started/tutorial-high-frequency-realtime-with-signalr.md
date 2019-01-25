---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: Учебник. Создание приложения в режиме реального времени с высокой частотой с SignalR 2 | Документация Майкрософт
author: bradygaster
description: Этом руководстве показано, как создать веб-приложения, использующего ASP.NET SignalR для предоставления функции обмена сообщениями с высокой частотой.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 44aaa2b0c059de310e963f642fa56c2f00a7e443
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836731"
---
# <a name="tutorial-create-high-frequency-real-time-app-with-signalr-2"></a><span data-ttu-id="91129-103">Учебник. Создание приложения в режиме реального времени с высокой частотой с SignalR 2</span><span class="sxs-lookup"><span data-stu-id="91129-103">Tutorial: Create high-frequency real-time app with SignalR 2</span></span>

<span data-ttu-id="91129-104">Этом руководстве показано, как создать веб-приложения, использующего ASP.NET SignalR 2 для предоставления функции обмена сообщениями с высокой частотой.</span><span class="sxs-lookup"><span data-stu-id="91129-104">This tutorial shows how to create a web application that uses ASP.NET SignalR 2 to provide high-frequency messaging functionality.</span></span> <span data-ttu-id="91129-105">В этом случае «обмена сообщениями высоких частот» означает, что сервер отправляет обновления по фиксированной ставке.</span><span class="sxs-lookup"><span data-stu-id="91129-105">In this case, "high-frequency messaging" means the server sends updates at a fixed rate.</span></span> <span data-ttu-id="91129-106">Вы отправляете до 10 сообщений в секунду.</span><span class="sxs-lookup"><span data-stu-id="91129-106">You send up to 10 messages a second.</span></span>

<span data-ttu-id="91129-107">Созданное вами приложение отображает фигуры, пользователи смогут перетаскивать.</span><span class="sxs-lookup"><span data-stu-id="91129-107">The application you create displays a shape that users can drag.</span></span> <span data-ttu-id="91129-108">Сервер обновляет положение фигуры на всех подключенных браузеров в соответствии с положение перетаскиваемого фигуры, с помощью синхронизированного обновления.</span><span class="sxs-lookup"><span data-stu-id="91129-108">The server updates the position of the shape in all connected browsers to match the position of the dragged shape using timed updates.</span></span>

<span data-ttu-id="91129-109">Основные понятия, представленных в этом руководстве имеют приложений в режиме реального времени играх и других приложений моделирования.</span><span class="sxs-lookup"><span data-stu-id="91129-109">Concepts introduced in this tutorial have applications in real-time gaming and other simulation applications.</span></span>

<span data-ttu-id="91129-110">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="91129-110">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="91129-111">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="91129-111">Set up the project</span></span>
> * <span data-ttu-id="91129-112">Создание базового приложения</span><span class="sxs-lookup"><span data-stu-id="91129-112">Create the base application</span></span>
> * <span data-ttu-id="91129-113">Сопоставить к концентратору, при запуске приложения</span><span class="sxs-lookup"><span data-stu-id="91129-113">Map to the hub when app starts</span></span>
> * <span data-ttu-id="91129-114">Добавление клиента</span><span class="sxs-lookup"><span data-stu-id="91129-114">Add the client</span></span>
> * <span data-ttu-id="91129-115">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="91129-115">Run the app</span></span>
> * <span data-ttu-id="91129-116">Добавить цикл клиента</span><span class="sxs-lookup"><span data-stu-id="91129-116">Add the client loop</span></span>
> * <span data-ttu-id="91129-117">Добавление сервера выполните циклический</span><span class="sxs-lookup"><span data-stu-id="91129-117">Add the server loop</span></span>
> * <span data-ttu-id="91129-118">Добавление анимации</span><span class="sxs-lookup"><span data-stu-id="91129-118">Add smooth animation</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="91129-119">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="91129-119">Prerequisites</span></span>

* <span data-ttu-id="91129-120">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) с **ASP.NET и веб-разработка** рабочей нагрузки.</span><span class="sxs-lookup"><span data-stu-id="91129-120">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="91129-121">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="91129-121">Set up the project</span></span>

<span data-ttu-id="91129-122">В этом разделе вы создадите проект в Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="91129-122">In this section, you create the project in Visual Studio 2017.</span></span>

<span data-ttu-id="91129-123">В этом разделе показано, как использовать Visual Studio 2017 для создания пустой веб-приложения ASP.NET и добавление библиотек SignalR и jQuery.UI.</span><span class="sxs-lookup"><span data-stu-id="91129-123">This section shows how to use Visual Studio 2017 to create an empty ASP.NET Web Application and add the SignalR and jQuery.UI libraries.</span></span>

1. <span data-ttu-id="91129-124">В Visual Studio создайте веб-приложения ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="91129-124">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Создание веб-](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

1. <span data-ttu-id="91129-126">В **новый веб-приложение ASP.NET - MoveShapeDemo** окне оставьте **пустой** затем выберите **ОК**.</span><span class="sxs-lookup"><span data-stu-id="91129-126">In the **New ASP.NET Web Application - MoveShapeDemo** window, leave **Empty** selected and select **OK**.</span></span>

1. <span data-ttu-id="91129-127">В **обозревателе решений**, щелкните правой кнопкой мыши проект и выберите **добавить** > **новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="91129-127">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="91129-128">В **Добавление нового элемента — MoveShapeDemo**выберите **установленные** > **Visual C#**   >  **Web**  >  **SignalR** , а затем выберите **класс концентратора SignalR (v2)**.</span><span class="sxs-lookup"><span data-stu-id="91129-128">In **Add New Item - MoveShapeDemo**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="91129-129">Назовите класс *MoveShapeHub* и добавьте его в проект.</span><span class="sxs-lookup"><span data-stu-id="91129-129">Name the class *MoveShapeHub* and add it to the project.</span></span>

    <span data-ttu-id="91129-130">На этом шаге создается *MoveShapeHub.cs* файл класса.</span><span class="sxs-lookup"><span data-stu-id="91129-130">This step creates the *MoveShapeHub.cs* class file.</span></span> <span data-ttu-id="91129-131">Одновременно он добавляет набор файлов сценариев и ссылки на сборки, которые поддерживают SignalR в проект.</span><span class="sxs-lookup"><span data-stu-id="91129-131">Simultaneously, it adds  a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="91129-132">Выберите **средства** > **диспетчер пакетов NuGet** > **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="91129-132">Select **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

1. <span data-ttu-id="91129-133">В **консоль диспетчера пакетов**, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="91129-133">In **Package Manager Console**, run this command:</span></span>

    ```console
    Install-Package jQuery.UI.Combined
    ```

    <span data-ttu-id="91129-134">Команда устанавливает библиотеки jQuery UI.</span><span class="sxs-lookup"><span data-stu-id="91129-134">The command installs the jQuery UI library.</span></span> <span data-ttu-id="91129-135">Используется для анимации фигуры.</span><span class="sxs-lookup"><span data-stu-id="91129-135">You use it to animate the shape.</span></span>

1. <span data-ttu-id="91129-136">В **обозревателе решений**, разверните узел скриптов.</span><span class="sxs-lookup"><span data-stu-id="91129-136">In **Solution Explorer**, expand the Scripts node.</span></span>

    ![Ссылки на библиотеку скрипта](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

    <span data-ttu-id="91129-138">Библиотеки скрипта для jQuery, jQueryUI и SignalR, отображаются в проект.</span><span class="sxs-lookup"><span data-stu-id="91129-138">Script libraries for jQuery, jQueryUI, and SignalR are visible in the project.</span></span>

## <a name="create-the-base-application"></a><span data-ttu-id="91129-139">Создание базового приложения</span><span class="sxs-lookup"><span data-stu-id="91129-139">Create the base application</span></span>

<span data-ttu-id="91129-140">В этом разделе вы создадите приложение браузера.</span><span class="sxs-lookup"><span data-stu-id="91129-140">In this section, you create a browser application.</span></span> <span data-ttu-id="91129-141">Приложение отправляет расположение фигуры на сервер во время каждого события перемещения мыши.</span><span class="sxs-lookup"><span data-stu-id="91129-141">The app sends the location of the shape to the server during each mouse move event.</span></span> <span data-ttu-id="91129-142">Сервер осуществляет широковещательную передачу эти сведения для всех других подключенных клиентов в режиме реального времени.</span><span class="sxs-lookup"><span data-stu-id="91129-142">The server broadcasts this information to all other connected clients in real time.</span></span> <span data-ttu-id="91129-143">Вы узнаете об этом приложении в следующих разделах.</span><span class="sxs-lookup"><span data-stu-id="91129-143">You learn more about this application in later sections.</span></span>

1. <span data-ttu-id="91129-144">Откройте *MoveShapeHub.cs* файла.</span><span class="sxs-lookup"><span data-stu-id="91129-144">Open the *MoveShapeHub.cs* file.</span></span>

1. <span data-ttu-id="91129-145">Замените код в *MoveShapeHub.cs* файла следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="91129-145">Replace the code in the *MoveShapeHub.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

1. <span data-ttu-id="91129-146">Сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="91129-146">Save the file.</span></span>

<span data-ttu-id="91129-147">`MoveShapeHub` Класс является реализацией концентратор SignalR.</span><span class="sxs-lookup"><span data-stu-id="91129-147">The `MoveShapeHub` class is an implementation of a SignalR hub.</span></span> <span data-ttu-id="91129-148">Как и в [начало работы с SignalR](tutorial-getting-started-with-signalr.md) руководстве концентратора есть метод, который клиенты вызывать напрямую.</span><span class="sxs-lookup"><span data-stu-id="91129-148">As in the [Getting Started with SignalR](tutorial-getting-started-with-signalr.md) tutorial, the hub has a method that the clients call directly.</span></span> <span data-ttu-id="91129-149">В этом случае клиент посылает объект с новой X и Y координаты фигуры на сервер.</span><span class="sxs-lookup"><span data-stu-id="91129-149">In this case, the client sends an object with the new X and Y coordinates of the shape to the server.</span></span> <span data-ttu-id="91129-150">Эти координаты получить широковещательная рассылка для всех других подключенных клиентов.</span><span class="sxs-lookup"><span data-stu-id="91129-150">Those coordinates get broadcasted to all other connected clients.</span></span> <span data-ttu-id="91129-151">SignalR автоматически сериализует этого объекта с помощью JSON.</span><span class="sxs-lookup"><span data-stu-id="91129-151">SignalR automatically serializes this object using JSON.</span></span>

<span data-ttu-id="91129-152">Приложение отправляет `ShapeModel` объект клиенту.</span><span class="sxs-lookup"><span data-stu-id="91129-152">The app sends the `ShapeModel` object to the client.</span></span> <span data-ttu-id="91129-153">Содержит члены для хранения позиции фигуры.</span><span class="sxs-lookup"><span data-stu-id="91129-153">It has members to store the position of the shape.</span></span> <span data-ttu-id="91129-154">Версия объекта на сервере также содержится элемент для отслеживания хранения данных какой клиент.</span><span class="sxs-lookup"><span data-stu-id="91129-154">The version of the object on the server also has a member to track which client's data is being stored.</span></span> <span data-ttu-id="91129-155">Этот объект не позволяет отправлять данные клиента само на себя сервер.</span><span class="sxs-lookup"><span data-stu-id="91129-155">This object prevents the server from sending a client's data back to itself.</span></span> <span data-ttu-id="91129-156">Использует этот член `JsonIgnore` атрибут, чтобы приложение из сериализации данных и отправкой обратно клиенту.</span><span class="sxs-lookup"><span data-stu-id="91129-156">This member uses the `JsonIgnore` attribute to keep the application from serializing the data and sending it back to the client.</span></span>

## <a name="map-to-the-hub-when-app-starts"></a><span data-ttu-id="91129-157">Сопоставить к концентратору, при запуске приложения</span><span class="sxs-lookup"><span data-stu-id="91129-157">Map to the hub when app starts</span></span>

<span data-ttu-id="91129-158">Затем можно настроить сопоставление концентратору при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="91129-158">Next, you set up mapping to the hub when the application starts.</span></span> <span data-ttu-id="91129-159">В SignalR 2 Добавление класса запуска OWIN создает сопоставление.</span><span class="sxs-lookup"><span data-stu-id="91129-159">In SignalR 2, adding an OWIN startup class creates the mapping.</span></span>

1. <span data-ttu-id="91129-160">В **обозревателе решений**, щелкните правой кнопкой мыши проект и выберите **добавить** > **новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="91129-160">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="91129-161">В **Добавление нового элемента — MoveShapeDemo** выберите **установленные** > **Visual C#**   >  **Web** и затем Выберите **класс запуска OWIN**.</span><span class="sxs-lookup"><span data-stu-id="91129-161">In **Add New Item - MoveShapeDemo** select **Installed** > **Visual C#** > **Web** and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="91129-162">Назовите класс *запуска* и выберите **ОК**.</span><span class="sxs-lookup"><span data-stu-id="91129-162">Name the class *Startup* and select **OK**.</span></span>

1. <span data-ttu-id="91129-163">Замените код по умолчанию в *Startup.cs* файла следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="91129-163">Replace the default code in the *Startup.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

<span data-ttu-id="91129-164">Вызовы класса запуска OWIN `MapSignalR` при выполнении приложения `Configuration` метод.</span><span class="sxs-lookup"><span data-stu-id="91129-164">The OWIN startup class calls `MapSignalR` when the app executes the `Configuration` method.</span></span> <span data-ttu-id="91129-165">Приложение добавляет класс запуска OWIN в обработке в режиме `OwinStartup` атрибут сборки.</span><span class="sxs-lookup"><span data-stu-id="91129-165">The app adds the class to OWIN's startup process using the `OwinStartup` assembly attribute.</span></span>

## <a name="add-the-client"></a><span data-ttu-id="91129-166">Добавление клиента</span><span class="sxs-lookup"><span data-stu-id="91129-166">Add the client</span></span>

<span data-ttu-id="91129-167">Добавьте HTML-страницы для клиента.</span><span class="sxs-lookup"><span data-stu-id="91129-167">Add the HTML page for the client.</span></span>

1. <span data-ttu-id="91129-168">В **обозревателе решений**, щелкните правой кнопкой мыши проект и выберите **добавить** > **HTML-страницу**.</span><span class="sxs-lookup"><span data-stu-id="91129-168">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="91129-169">Присвойте странице имя **по умолчанию** и выберите **ОК**.</span><span class="sxs-lookup"><span data-stu-id="91129-169">Name the page **Default** and select **OK**.</span></span>

1. <span data-ttu-id="91129-170">В **обозревателе решений**, щелкните правой кнопкой мыши *Default.html* и выберите **задать в качестве начальной страницы**.</span><span class="sxs-lookup"><span data-stu-id="91129-170">In **Solution Explorer**, right-click *Default.html* and select **Set as Start Page**.</span></span>

1. <span data-ttu-id="91129-171">Замените код по умолчанию в *Default.html* файла следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="91129-171">Replace the default code in the *Default.html* file with this code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html?highlight=14-16)]

1. <span data-ttu-id="91129-172">В **обозревателе решений**, разверните **сценариев**.</span><span class="sxs-lookup"><span data-stu-id="91129-172">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="91129-173">Библиотеки скрипта для jQuery и SignalR, отображаются в проект.</span><span class="sxs-lookup"><span data-stu-id="91129-173">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="91129-174">Диспетчер пакетов устанавливается более поздняя версия скрипты SignalR.</span><span class="sxs-lookup"><span data-stu-id="91129-174">The package manager installs a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="91129-175">Обновите ссылки на скрипты в блоке кода в соответствии с версии файлов скриптов в проекте.</span><span class="sxs-lookup"><span data-stu-id="91129-175">Update the script references in the code block to correspond to the versions of the script files in the project.</span></span>

<span data-ttu-id="91129-176">Этот код HTML и JavaScript создает красный `div` вызывается `shape`.</span><span class="sxs-lookup"><span data-stu-id="91129-176">This HTML and JavaScript code creates a red `div` called `shape`.</span></span> <span data-ttu-id="91129-177">Он включает поведение перетаскивания фигуры, с помощью библиотеки jQuery и использует `drag` событий для отправки положение фигуры на сервер.</span><span class="sxs-lookup"><span data-stu-id="91129-177">It enables the shape's dragging behavior using the jQuery library and uses the `drag` event to send the shape's position to the server.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="91129-178">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="91129-178">Run the app</span></span>

<span data-ttu-id="91129-179">Приложение можно запустить для se'e его работы.</span><span class="sxs-lookup"><span data-stu-id="91129-179">You can run the app to se\`e it work.</span></span> <span data-ttu-id="91129-180">При перетаскивании фигуры вокруг окна браузера фигуры слишком перемещает в других браузерах.</span><span class="sxs-lookup"><span data-stu-id="91129-180">When you drag the shape around a browser window, the shape moves in the other browsers too.</span></span>

1. <span data-ttu-id="91129-181">На панели инструментов, включите **отладки скриптов** и затем нажмите кнопку воспроизведения, чтобы запустить приложение в режиме отладки.</span><span class="sxs-lookup"><span data-stu-id="91129-181">In the toolbar, turn on **Script Debugging** and then select the play button to run the application in Debug mode.</span></span>

    ![Снимок экрана: Включение режим отладки и выбрав play пользователя.](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)

    <span data-ttu-id="91129-183">Откроется окно браузера с красным фигуры в правом верхнем углу.</span><span class="sxs-lookup"><span data-stu-id="91129-183">A browser window opens with the red shape in the upper-right corner.</span></span>

1. <span data-ttu-id="91129-184">Скопируйте URL-адрес страницы.</span><span class="sxs-lookup"><span data-stu-id="91129-184">Copy the page's URL.</span></span>

1. <span data-ttu-id="91129-185">Откройте другой браузер и вставьте URL-адрес в адресной строке.</span><span class="sxs-lookup"><span data-stu-id="91129-185">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="91129-186">Перетащите фигуру в одном из окна браузера.</span><span class="sxs-lookup"><span data-stu-id="91129-186">Drag the shape in one of the browser windows.</span></span> <span data-ttu-id="91129-187">Ниже приведен фигуры в другом окне браузера.</span><span class="sxs-lookup"><span data-stu-id="91129-187">The shape in the other browser window follows.</span></span>

<span data-ttu-id="91129-188">Хотя приложение функции, с помощью этого метода, не рекомендуемые модель программирования.</span><span class="sxs-lookup"><span data-stu-id="91129-188">While the application functions using this method, it's not a recommended programming model.</span></span> <span data-ttu-id="91129-189">Не ограничено верхней количеству начало отправленных сообщений.</span><span class="sxs-lookup"><span data-stu-id="91129-189">There's no upper limit to the number of messages getting sent.</span></span> <span data-ttu-id="91129-190">Таким образом клиенты и сервер получить перегружены сообщений и производительность снижается.</span><span class="sxs-lookup"><span data-stu-id="91129-190">As a result, the clients and server get overwhelmed with messages and performance degrades.</span></span> <span data-ttu-id="91129-191">Кроме того в приложении отображается анимированный значок несвязанном на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="91129-191">Also, the app displays a disjointed animation on the client.</span></span> <span data-ttu-id="91129-192">Эта анимация рывками происходит потому, что фигуры мгновенно перемещает каждым методом.</span><span class="sxs-lookup"><span data-stu-id="91129-192">This jerky animation happens because the shape moves instantly by each method.</span></span> <span data-ttu-id="91129-193">Это лучше, если фигура плавно перемещается каждого нового расположения.</span><span class="sxs-lookup"><span data-stu-id="91129-193">It's better if the shape moves smoothly to each new location.</span></span> <span data-ttu-id="91129-194">Далее вы узнаете, как устранить эти проблемы.</span><span class="sxs-lookup"><span data-stu-id="91129-194">Next, you learn how to fix those issues.</span></span>

## <a name="add-the-client-loop"></a><span data-ttu-id="91129-195">Добавить цикл клиента</span><span class="sxs-lookup"><span data-stu-id="91129-195">Add the client loop</span></span>

<span data-ttu-id="91129-196">Отправка расположение фигуры на каждое событие перемещения мыши создает ненужные объема сетевого трафика.</span><span class="sxs-lookup"><span data-stu-id="91129-196">Sending the location of the shape on every mouse move event creates an unnecessary amount of network traffic.</span></span> <span data-ttu-id="91129-197">Приложению необходимо регулировать сообщения от клиента.</span><span class="sxs-lookup"><span data-stu-id="91129-197">The app needs to throttle the messages from the client.</span></span>

<span data-ttu-id="91129-198">Используйте javascript `setInterval` функции, чтобы настроить цикл, который отправляет на сервер по фиксированной ставке новые сведения о положении.</span><span class="sxs-lookup"><span data-stu-id="91129-198">Use the javascript `setInterval` function to set up a loop that sends new position information to the server at a fixed rate.</span></span> <span data-ttu-id="91129-199">Этот цикл представляет основные «цикла игры».</span><span class="sxs-lookup"><span data-stu-id="91129-199">This loop is a basic representation of a "game loop."</span></span> <span data-ttu-id="91129-200">Это многократно вызванной функции, которые можно применить все функциональные возможности игры.</span><span class="sxs-lookup"><span data-stu-id="91129-200">It's a repeatedly called function that drives all the functionality of a game.</span></span>

1. <span data-ttu-id="91129-201">Замените код клиента в *Default.html* файла следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="91129-201">Replace the client code in the *Default.html* file with this code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html?highlight=14-16)]

    > [!IMPORTANT]
    > <span data-ttu-id="91129-202">Необходимо заменить ссылки на скрипты еще раз.</span><span class="sxs-lookup"><span data-stu-id="91129-202">You have to replace the script references again.</span></span> <span data-ttu-id="91129-203">Они должны соответствовать версиям сценариев в проекте.</span><span class="sxs-lookup"><span data-stu-id="91129-203">They must match the versions of the scripts in the project.</span></span>

    <span data-ttu-id="91129-204">Этот новый код добавляет `updateServerModel` функции.</span><span class="sxs-lookup"><span data-stu-id="91129-204">This new code adds the `updateServerModel` function.</span></span> <span data-ttu-id="91129-205">Он вызывается с фиксированной частотой.</span><span class="sxs-lookup"><span data-stu-id="91129-205">It gets called on a fixed frequency.</span></span> <span data-ttu-id="91129-206">Функция отправляет на сервер данные положения всякий раз, когда `moved` флаг указывает, что новые данные позиции для отправки.</span><span class="sxs-lookup"><span data-stu-id="91129-206">The function sends the position data to the server whenever the `moved` flag indicates that there's new position data to send.</span></span>

1. <span data-ttu-id="91129-207">Нажмите кнопку воспроизведения, чтобы запустить приложение</span><span class="sxs-lookup"><span data-stu-id="91129-207">Select the play button to start the application</span></span>

1. <span data-ttu-id="91129-208">Скопируйте URL-адрес страницы.</span><span class="sxs-lookup"><span data-stu-id="91129-208">Copy the page's URL.</span></span>

1. <span data-ttu-id="91129-209">Откройте другой браузер и вставьте URL-адрес в адресной строке.</span><span class="sxs-lookup"><span data-stu-id="91129-209">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="91129-210">Перетащите фигуру в одном из окна браузера.</span><span class="sxs-lookup"><span data-stu-id="91129-210">Drag the shape in one of the browser windows.</span></span> <span data-ttu-id="91129-211">Ниже приведен фигуры в другом окне браузера.</span><span class="sxs-lookup"><span data-stu-id="91129-211">The shape in the other browser window follows.</span></span>

<span data-ttu-id="91129-212">Так как приложение регулирует количество сообщений, отправляемых на сервер, анимация не будет отображаться как smooth был сначала.</span><span class="sxs-lookup"><span data-stu-id="91129-212">Since the app throttles the number of messages that get sent to the server, the animation won't appear as smooth did at first.</span></span>

## <a name="add-the-server-loop"></a><span data-ttu-id="91129-213">Добавление сервера выполните циклический</span><span class="sxs-lookup"><span data-stu-id="91129-213">Add the server loop</span></span>

<span data-ttu-id="91129-214">В текущем приложении сообщения, отправленные с сервера клиенту иду так часто, как их получения.</span><span class="sxs-lookup"><span data-stu-id="91129-214">In the current application, messages sent from the server to the client go out as often as they're received.</span></span> <span data-ttu-id="91129-215">Этот сетевой трафик представляется подобную проблему, как показано на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="91129-215">This network traffic presents a similar problem as we see on the client.</span></span>

<span data-ttu-id="91129-216">Приложение может отправлять сообщения чаще, чем они требуются.</span><span class="sxs-lookup"><span data-stu-id="91129-216">The app can send messages more often than they're needed.</span></span> <span data-ttu-id="91129-217">Соединение может стать распространяются в результате.</span><span class="sxs-lookup"><span data-stu-id="91129-217">The connection can become flooded as a result.</span></span> <span data-ttu-id="91129-218">В этом разделе описывается, как для обновления сервера, чтобы добавить таймер, который регулирует скорость исходящих сообщений.</span><span class="sxs-lookup"><span data-stu-id="91129-218">This section describes how to update the server to add a timer that throttles the rate of the outgoing messages.</span></span>

1. <span data-ttu-id="91129-219">Замените содержимое файла `MoveShapeHub.cs` следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="91129-219">Replace the contents of `MoveShapeHub.cs` with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

1. <span data-ttu-id="91129-220">Нажмите кнопку воспроизведения, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="91129-220">Select the play button to start the application.</span></span>

1. <span data-ttu-id="91129-221">Скопируйте URL-адрес страницы.</span><span class="sxs-lookup"><span data-stu-id="91129-221">Copy the page's URL.</span></span>

1. <span data-ttu-id="91129-222">Откройте другой браузер и вставьте URL-адрес в адресной строке.</span><span class="sxs-lookup"><span data-stu-id="91129-222">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="91129-223">Перетащите фигуру в одном из окна браузера.</span><span class="sxs-lookup"><span data-stu-id="91129-223">Drag the shape in one of the browser windows.</span></span>

<span data-ttu-id="91129-224">Этот код при развертывании клиента, чтобы добавить `Broadcaster` класса.</span><span class="sxs-lookup"><span data-stu-id="91129-224">This code expands the client to add the `Broadcaster` class.</span></span> <span data-ttu-id="91129-225">Новый класс регулирует исходящих сообщений, с помощью `Timer` класс из .NET framework.</span><span class="sxs-lookup"><span data-stu-id="91129-225">The new class throttles the outgoing messages using the `Timer` class from the .NET framework.</span></span>

<span data-ttu-id="91129-226">Рекомендуется узнать, что сам концентратор является временным.</span><span class="sxs-lookup"><span data-stu-id="91129-226">It's good to learn that the hub itself is transitory.</span></span> <span data-ttu-id="91129-227">Оно создается каждый раз при необходимости.</span><span class="sxs-lookup"><span data-stu-id="91129-227">It's created every time it's needed.</span></span> <span data-ttu-id="91129-228">Поэтому приложение создает `Broadcaster` как единственный экземпляр.</span><span class="sxs-lookup"><span data-stu-id="91129-228">So the app creates the `Broadcaster` as a singleton.</span></span> <span data-ttu-id="91129-229">Используется отложенная инициализация отложить `Broadcaster`его создания, если она нужна.</span><span class="sxs-lookup"><span data-stu-id="91129-229">It uses lazy initialization to defer the `Broadcaster`'s creation until it's needed.</span></span> <span data-ttu-id="91129-230">Гарантирующий, что приложение создает первый экземпляр концентратора полностью до запуска таймера.</span><span class="sxs-lookup"><span data-stu-id="91129-230">That guarantees that the app creates the first hub instance completely before starting the timer.</span></span>

<span data-ttu-id="91129-231">Вызов клиентов `UpdateShape` функция затем перемещается за пределы центра `UpdateModel` метод.</span><span class="sxs-lookup"><span data-stu-id="91129-231">The call to the clients' `UpdateShape` function is then moved out of the hub's `UpdateModel` method.</span></span> <span data-ttu-id="91129-232">Больше не вызывается немедленно в том случае, когда приложение получает входящие сообщения.</span><span class="sxs-lookup"><span data-stu-id="91129-232">It's no longer called immediately whenever the app receives incoming messages.</span></span> <span data-ttu-id="91129-233">Вместо этого приложение отправляет сообщения на клиентах со скоростью 25 вызовов в секунду.</span><span class="sxs-lookup"><span data-stu-id="91129-233">Instead, the app sends the messages to the clients at a rate of 25 calls per second.</span></span> <span data-ttu-id="91129-234">Этот процесс управляется `_broadcastLoop` таймера изнутри `Broadcaster` класса.</span><span class="sxs-lookup"><span data-stu-id="91129-234">The process is  managed by the `_broadcastLoop` timer from within the `Broadcaster` class.</span></span>

<span data-ttu-id="91129-235">Наконец, вместо вызова метода клиента от концентратора напрямую, `Broadcaster` классу необходимо получить ссылку на данный момент операционной `_hubContext` концентратора.</span><span class="sxs-lookup"><span data-stu-id="91129-235">Lastly, instead of calling the client method from the hub directly, the `Broadcaster` class needs to get a reference to the currently operating `_hubContext` hub.</span></span> <span data-ttu-id="91129-236">Он возвращает ссылку на с `GlobalHost`.</span><span class="sxs-lookup"><span data-stu-id="91129-236">It gets the reference with the `GlobalHost`.</span></span>

## <a name="add-smooth-animation"></a><span data-ttu-id="91129-237">Добавление анимации</span><span class="sxs-lookup"><span data-stu-id="91129-237">Add smooth animation</span></span>

<span data-ttu-id="91129-238">Оно будет почти все готово, но можно сделать один Дополнительные улучшения.</span><span class="sxs-lookup"><span data-stu-id="91129-238">The application is almost finished, but we could make one more improvement.</span></span> <span data-ttu-id="91129-239">Приложение перемещает фигуры на клиенте в ответ на сообщения сервера.</span><span class="sxs-lookup"><span data-stu-id="91129-239">The app moves the shape on the client in response to server messages.</span></span> <span data-ttu-id="91129-240">Вместо задания позиции фигуры в новое расположение, заданным на сервере, используйте библиотеку JQuery UI `animate` функции.</span><span class="sxs-lookup"><span data-stu-id="91129-240">Instead of setting the position of the shape to the new location given by the server, use the JQuery UI library's `animate` function.</span></span> <span data-ttu-id="91129-241">Он может перемещать фигуру плавно между его текущим и новым позиции.</span><span class="sxs-lookup"><span data-stu-id="91129-241">It can move the shape smoothly between its current and new position.</span></span>

1. <span data-ttu-id="91129-242">Обновление клиента `updateShape` метод в *Default.html* файл, чтобы он выглядел выделенный код:</span><span class="sxs-lookup"><span data-stu-id="91129-242">Update the client's `updateShape` method in the *Default.html* file to look like the highlighted code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

1. <span data-ttu-id="91129-243">Нажмите кнопку воспроизведения, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="91129-243">Select the play button to start the application.</span></span>

1. <span data-ttu-id="91129-244">Скопируйте URL-адрес страницы.</span><span class="sxs-lookup"><span data-stu-id="91129-244">Copy the page's URL.</span></span>

1. <span data-ttu-id="91129-245">Откройте другой браузер и вставьте URL-адрес в адресной строке.</span><span class="sxs-lookup"><span data-stu-id="91129-245">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="91129-246">Перетащите фигуру в одном из окна браузера.</span><span class="sxs-lookup"><span data-stu-id="91129-246">Drag the shape in one of the browser windows.</span></span>

<span data-ttu-id="91129-247">Перемещение фигуры в другом окне появляется менее рывками.</span><span class="sxs-lookup"><span data-stu-id="91129-247">The movement of the shape in the other window appears less jerky.</span></span> <span data-ttu-id="91129-248">Приложение выполняет интерполяцию выкатиться за время, а не значение один раз на входящее сообщение.</span><span class="sxs-lookup"><span data-stu-id="91129-248">The app interpolates its movement over time rather than being set once per incoming message.</span></span>

<span data-ttu-id="91129-249">Этот код перемещает фигуру из старой позиции в новую.</span><span class="sxs-lookup"><span data-stu-id="91129-249">This code moves the shape from the old location to the new one.</span></span> <span data-ttu-id="91129-250">Сервер предоставляет позицию фигуры в ходе анимации интервала.</span><span class="sxs-lookup"><span data-stu-id="91129-250">The server gives the position of the shape over the course of the animation interval.</span></span> <span data-ttu-id="91129-251">В этом случае это 100 миллисекунд.</span><span class="sxs-lookup"><span data-stu-id="91129-251">In this case, that's 100 milliseconds.</span></span> <span data-ttu-id="91129-252">Приложение очищает любые предыдущие анимацию на фигуре перед началом новой анимации.</span><span class="sxs-lookup"><span data-stu-id="91129-252">The app clears any previous animation running on the shape before the new animation starts.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="91129-253">Получение кода</span><span class="sxs-lookup"><span data-stu-id="91129-253">Get the code</span></span>

[<span data-ttu-id="91129-254">Скачать завершенный проект</span><span class="sxs-lookup"><span data-stu-id="91129-254">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

## <a name="additional-resources"></a><span data-ttu-id="91129-255">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="91129-255">Additional resources</span></span>

<span data-ttu-id="91129-256">Парадигма связи, вы узнали о полезен для разработки игр и других режимов моделирования, как [ShootR игры, созданные с помощью SignalR](https://shootr.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="91129-256">The communication paradigm you just learned about is useful for developing online games and other simulations, like [the ShootR game created with SignalR](https://shootr.azurewebsites.net/).</span></span>

<span data-ttu-id="91129-257">Дополнительные сведения о SignalR см. следующие ресурсы:</span><span class="sxs-lookup"><span data-stu-id="91129-257">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="91129-258">Проект SignalR</span><span class="sxs-lookup"><span data-stu-id="91129-258">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="91129-259">SignalR GitHub и примерами</span><span class="sxs-lookup"><span data-stu-id="91129-259">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="91129-260">Вики-сайте SignalR</span><span class="sxs-lookup"><span data-stu-id="91129-260">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="91129-261">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="91129-261">Next steps</span></span>

<span data-ttu-id="91129-262">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="91129-262">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="91129-263">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="91129-263">Set up the project</span></span>
> * <span data-ttu-id="91129-264">Создания базового приложения</span><span class="sxs-lookup"><span data-stu-id="91129-264">Created the base application</span></span>
> * <span data-ttu-id="91129-265">Сопоставленные в центр, при запуске приложения</span><span class="sxs-lookup"><span data-stu-id="91129-265">Mapped to the hub when app starts</span></span>
> * <span data-ttu-id="91129-266">Добавлении клиента</span><span class="sxs-lookup"><span data-stu-id="91129-266">Added the client</span></span>
> * <span data-ttu-id="91129-267">Запускаю приложение</span><span class="sxs-lookup"><span data-stu-id="91129-267">Ran the app</span></span>
> * <span data-ttu-id="91129-268">Добавить цикл клиента</span><span class="sxs-lookup"><span data-stu-id="91129-268">Added the client loop</span></span>
> * <span data-ttu-id="91129-269">Добавить сервер</span><span class="sxs-lookup"><span data-stu-id="91129-269">Added the server loop</span></span>
> * <span data-ttu-id="91129-270">Добавлена создавать плавную анимацию</span><span class="sxs-lookup"><span data-stu-id="91129-270">Added smooth animation</span></span>

<span data-ttu-id="91129-271">Перейдите к следующей статье, чтобы научиться создавать веб-приложения, использующего ASP.NET SignalR 2 для предоставления широковещательных функциональные возможности сервера.</span><span class="sxs-lookup"><span data-stu-id="91129-271">Advance to the next article to learn how to create a web application that uses ASP.NET SignalR 2 to provide server broadcast functionality.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="91129-272">SignalR 2 и широковещательная рассылка сервера</span><span class="sxs-lookup"><span data-stu-id="91129-272">SignalR 2 and server broadcasting</span></span>](tutorial-server-broadcast-with-signalr.md)