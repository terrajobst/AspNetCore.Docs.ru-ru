---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: Использование счетчиков производительности SignalR в веб-роли Azure | Документы Microsoft
author: guardrex
description: Описывается установка и использование счетчиков производительности SignalR в веб-роли Azure.
keywords: Счетчик ASP.NET,SignalR,Performance, веб-роли azure
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/11/2017
ms.topic: article
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: 2f6c6feb030fc17f95e7862c39029569f3d8c5dc
ms.sourcegitcommit: d8aa1d314891e981460b5e5c912afb730adbb3ad
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/05/2018
ms.locfileid: "28988020"
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a><span data-ttu-id="61bb5-104">Использование счетчиков производительности SignalR в веб-роли Azure</span><span class="sxs-lookup"><span data-stu-id="61bb5-104">Using SignalR performance counters in an Azure Web Role</span></span>

<span data-ttu-id="61bb5-105">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="61bb5-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="61bb5-106">SignalR счетчики производительности используются для наблюдения за производительностью приложения в веб-роли Azure.</span><span class="sxs-lookup"><span data-stu-id="61bb5-106">SignalR performance counters are used to monitor your app's performance in an Azure Web Role.</span></span> <span data-ttu-id="61bb5-107">Счетчики отслеживаются с помощью системы диагностики Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="61bb5-107">The counters are captured by Microsoft Azure Diagnostics.</span></span> <span data-ttu-id="61bb5-108">Установка счетчиков производительности SignalR в Azure с *signalr.exe*, то же самое средство, используемое для изолированного или локальных приложений.</span><span class="sxs-lookup"><span data-stu-id="61bb5-108">You install SignalR performance counters on Azure with *signalr.exe*, the same tool used for standalone or on-premises apps.</span></span> <span data-ttu-id="61bb5-109">Поскольку роли Azure являются временными, настройке приложения для установки и регистрации счетчиков производительности SignalR при запуске.</span><span class="sxs-lookup"><span data-stu-id="61bb5-109">Since Azure roles are transient, you configure an app to install and register SignalR performance counters upon startup.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="61bb5-110">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="61bb5-110">Prerequisites</span></span>

* [<span data-ttu-id="61bb5-111">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="61bb5-111">Visual Studio 2015</span></span>](https://www.visualstudio.com/vs/visual-studio-express/)
* <span data-ttu-id="61bb5-112">[Microsoft Azure SDK для Visual Studio 2015 (VS2015)](https://azure.microsoft.com/downloads/) **Примечание: Перезагрузите компьютер после установки пакета SDK.**</span><span class="sxs-lookup"><span data-stu-id="61bb5-112">[Microsoft Azure SDK for Visual Studio 2015 (VS2015)](https://azure.microsoft.com/downloads/) **Note: Restart your machine after installing the SDK.**</span></span>
* <span data-ttu-id="61bb5-113">Подписка на Microsoft Azure: подписаться на бесплатную пробную учетную запись Azure, в разделе [бесплатная пробная версия Azure](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="61bb5-113">Microsoft Azure subscription: To sign up for a free Azure trial account, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a><span data-ttu-id="61bb5-114">Создание приложения веб-роли Azure, предоставляет счетчиков производительности SignalR</span><span class="sxs-lookup"><span data-stu-id="61bb5-114">Creating an Azure Web Role application that exposes SignalR performance counters</span></span>

1. <span data-ttu-id="61bb5-115">Откройте Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="61bb5-115">Open Visual Studio 2015.</span></span>

2. <span data-ttu-id="61bb5-116">В Visual Studio 2015 выберите **файл** > **New** > **проекта**.</span><span class="sxs-lookup"><span data-stu-id="61bb5-116">In Visual Studio 2015, select **File** > **New** > **Project**.</span></span>

3. <span data-ttu-id="61bb5-117">В **шаблоны** области **новый проект** под **Visual C#** выберите **облака** , а затем выберите **Облачной службы azure** шаблона.</span><span class="sxs-lookup"><span data-stu-id="61bb5-117">In the **Templates** pane of the **New Project** window under the **Visual C#** node, select the **Cloud** node and select the **Azure Cloud Service** template.</span></span> <span data-ttu-id="61bb5-118">Присвойте приложению имя **SignalRPerfCounters** и выберите **ОК**.</span><span class="sxs-lookup"><span data-stu-id="61bb5-118">Name the app **SignalRPerfCounters** and select **OK**.</span></span>

   ![Новое приложение в облаке](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)
    
4. <span data-ttu-id="61bb5-120">В **новой облачной службы Microsoft Azure** диалогового окна выберите **веб-роли ASP.NET** и выберите > кнопку, чтобы добавить роль в проект.</span><span class="sxs-lookup"><span data-stu-id="61bb5-120">In the **New Microsoft Azure Cloud Service** dialog, select **ASP.NET Web Role** and select the > button to add the role to the project.</span></span> <span data-ttu-id="61bb5-121">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="61bb5-121">Select **OK**.</span></span>

   ![Добавить веб-роль ASP.NET](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)
    
5. <span data-ttu-id="61bb5-123">В **новый веб-приложение ASP.NET — WebRole1** диалогового окна выберите **MVC** шаблона, а затем выберите **ОК**.</span><span class="sxs-lookup"><span data-stu-id="61bb5-123">In the **New ASP.NET Web Application - WebRole1** dialog, select the **MVC** template, and then select **OK**.</span></span>

   ![Добавить MVC и веб-API](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)
    
6. <span data-ttu-id="61bb5-125">В **обозревателе решений**откройте *diagnostics.wadcfgx* файл **WebRole1**.</span><span class="sxs-lookup"><span data-stu-id="61bb5-125">In **Solution Explorer**, open the *diagnostics.wadcfgx* file under **WebRole1**.</span></span>

   ![Diagnostics.wadcfgx обозревателя решений](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)
    
7. <span data-ttu-id="61bb5-127">Замените содержимое файла со следующей конфигурацией и сохраните файл:</span><span class="sxs-lookup"><span data-stu-id="61bb5-127">Replace the contents of the file with the following configuration and save the file:</span></span>

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]
    
8. <span data-ttu-id="61bb5-128">Откройте **консоль диспетчера пакетов** из **средства** > **диспетчера пакетов NuGet**.</span><span class="sxs-lookup"><span data-stu-id="61bb5-128">Open the **Package Manager Console** from **Tools** > **NuGet Package Manager**.</span></span> <span data-ttu-id="61bb5-129">Введите следующие команды, чтобы установить последнюю версию SignalR и служебные программы пакета SignalR.</span><span class="sxs-lookup"><span data-stu-id="61bb5-129">Enter the following commands to install the latest version of SignalR and the SignalR utilities package:</span></span>

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]
    
9. <span data-ttu-id="61bb5-130">Настройка приложения для установки счетчиков производительности SignalR в экземпляре роли при его запуске и перезапускается.</span><span class="sxs-lookup"><span data-stu-id="61bb5-130">Configure the app to install the SignalR performance counters into the role instance when it starts up or recycles.</span></span> <span data-ttu-id="61bb5-131">В **обозревателе решений**, щелкните правой кнопкой мыши **WebRole1** проект и выберите **добавить** > **новую папку**.</span><span class="sxs-lookup"><span data-stu-id="61bb5-131">In **Solution Explorer**, right-click on the **WebRole1** project and select **Add** > **New Folder**.</span></span> <span data-ttu-id="61bb5-132">Имя новой папки *запуска*.</span><span class="sxs-lookup"><span data-stu-id="61bb5-132">Name the new folder *Startup*.</span></span>

   ![Добавьте папку при запуске](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)
    
10. <span data-ttu-id="61bb5-134">Копировать *signalr.exe* файла (добавлены с классом **Microsoft.AspNet.SignalR.Utils** пакета) из \<папки проекта > / SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\< версия > / средств для *запуска* папку, созданную на предыдущем шаге.</span><span class="sxs-lookup"><span data-stu-id="61bb5-134">Copy the *signalr.exe* file (added with the **Microsoft.AspNet.SignalR.Utils** package) from \<project folder>/SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\<version>/tools to the *Startup* folder you created in the previous step.</span></span>

11. <span data-ttu-id="61bb5-135">В **обозревателе решений**, щелкните правой кнопкой мыши *запуска* папку и выберите **добавить** > **существующий элемент**.</span><span class="sxs-lookup"><span data-stu-id="61bb5-135">In **Solution Explorer**, right-click the *Startup* folder and select **Add** > **Existing Item**.</span></span> <span data-ttu-id="61bb5-136">В диалоговом окне, которое отображается, выберите *signalr.exe* и выберите **добавить**.</span><span class="sxs-lookup"><span data-stu-id="61bb5-136">In the dialog that appears, select *signalr.exe* and select **Add**.</span></span>

    ![Добавление signalr.exe проект](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)
    
12. <span data-ttu-id="61bb5-138">Щелкните правой кнопкой мыши *запуска* созданную папку.</span><span class="sxs-lookup"><span data-stu-id="61bb5-138">Right-click on the *Startup* folder you created.</span></span> <span data-ttu-id="61bb5-139">Выберите **Добавить** > **Новый объект**.</span><span class="sxs-lookup"><span data-stu-id="61bb5-139">Select **Add** > **New Item**.</span></span> <span data-ttu-id="61bb5-140">Выберите **Общие** выберите **текстовый файл**и назовите новый элемент *SignalRPerfCounterInstall.cmd*.</span><span class="sxs-lookup"><span data-stu-id="61bb5-140">Select the **General** node, select **Text File**, and name the new item *SignalRPerfCounterInstall.cmd*.</span></span> <span data-ttu-id="61bb5-141">Этот командный файл устанавливает счетчиков производительности SignalR в веб-роли.</span><span class="sxs-lookup"><span data-stu-id="61bb5-141">This command file will install the SignalR performance counters into the web role.</span></span>

    ![Создание файла пакета установки счетчиков производительности SignalR](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)
     
13. <span data-ttu-id="61bb5-143">Если Visual Studio создает *SignalRPerfCounterInstall.cmd* файл, он автоматически открывается в главном окне.</span><span class="sxs-lookup"><span data-stu-id="61bb5-143">When Visual Studio creates the *SignalRPerfCounterInstall.cmd* file, it will automatically open in the main window.</span></span> <span data-ttu-id="61bb5-144">Замените содержимое файла следующий сценарий, а затем сохраните и закройте файл.</span><span class="sxs-lookup"><span data-stu-id="61bb5-144">Replace the contents of the file with the following script, then save and close the file.</span></span> <span data-ttu-id="61bb5-145">Этот скрипт выполняется *signalr.exe*, который добавляет счетчиков производительности SignalR экземпляра роли.</span><span class="sxs-lookup"><span data-stu-id="61bb5-145">This script executes *signalr.exe*, which adds the SignalR performance counters to the role instance.</span></span>

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]
    
14. <span data-ttu-id="61bb5-146">Выберите *signalr.exe* файла в **обозревателе решений**.</span><span class="sxs-lookup"><span data-stu-id="61bb5-146">Select the *signalr.exe* file in **Solution Explorer**.</span></span> <span data-ttu-id="61bb5-147">В файле **свойства**, задайте **Копировать в выходной каталог** для **всегда Копировать**.</span><span class="sxs-lookup"><span data-stu-id="61bb5-147">In the file's **Properties**, set **Copy to Output Directory** to **Copy Always**.</span></span>

    ![Задать Копировать в выходной каталог, чтобы всегда Копировать](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)
    
15. <span data-ttu-id="61bb5-149">Повторите предыдущий шаг для *SignalRPerfCounterInstall.cmd* файла.</span><span class="sxs-lookup"><span data-stu-id="61bb5-149">Repeat the previous step for the *SignalRPerfCounterInstall.cmd* file.</span></span>

    
16. <span data-ttu-id="61bb5-150">Щелкните правой кнопкой мыши *SignalRPerfCounterInstall.cmd* файла и выберите **открыть с помощью**.</span><span class="sxs-lookup"><span data-stu-id="61bb5-150">Right-click on the *SignalRPerfCounterInstall.cmd* file and select **Open With**.</span></span> <span data-ttu-id="61bb5-151">В диалоговом окне, которое отображается, выберите **двоичный редактор** и выберите **ОК**.</span><span class="sxs-lookup"><span data-stu-id="61bb5-151">In the dialog that appears, select **Binary Editor** and select **OK**.</span></span>

    ![Открыть двоичный редактор](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)
    
17. <span data-ttu-id="61bb5-153">В двоичном редакторе выберите все начальные байты в файле и удалите их.</span><span class="sxs-lookup"><span data-stu-id="61bb5-153">In the binary editor, select any leading bytes in the file and delete them.</span></span> <span data-ttu-id="61bb5-154">Сохраните и закройте файл.</span><span class="sxs-lookup"><span data-stu-id="61bb5-154">Save and close the file.</span></span>

    ![Удалить начальные байт](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)
    
18. <span data-ttu-id="61bb5-156">Откройте *ServiceDefinition.csdef* и добавить задачу запуска, которая выполняет *SignalrPerfCounterInstall.cmd* файл при запуске службы:</span><span class="sxs-lookup"><span data-stu-id="61bb5-156">Open *ServiceDefinition.csdef* and add a startup task that executes the *SignalrPerfCounterInstall.cmd* file when the service starts up:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]
    
19. <span data-ttu-id="61bb5-157">Откройте `Views/Shared/_Layout.cshtml` и удалить сценарий пакет jQuery с конца файла.</span><span class="sxs-lookup"><span data-stu-id="61bb5-157">Open `Views/Shared/_Layout.cshtml` and remove the jQuery bundle script from the end of the file.</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]
    
20. <span data-ttu-id="61bb5-158">Добавить клиент JavaScript, который постоянно вызывает `increment` метод на сервере.</span><span class="sxs-lookup"><span data-stu-id="61bb5-158">Add a JavaScript client that continuously calls the `increment` method on the server.</span></span> <span data-ttu-id="61bb5-159">Откройте `Views/Home/Index.cshtml` и замените содержимое следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="61bb5-159">Open `Views/Home/Index.cshtml` and replace the contents with the following code:</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]
    
21. <span data-ttu-id="61bb5-160">Создайте новую папку в **WebRole1** проект с именем *концентраторов*.</span><span class="sxs-lookup"><span data-stu-id="61bb5-160">Create a new folder in the **WebRole1** project named *Hubs*.</span></span> <span data-ttu-id="61bb5-161">Щелкните правой кнопкой мыши *концентраторов* папки в **обозревателе решений**выберите **Web** > **SignalR**и выберите  **Класс концентратора SignalR (v2)**.</span><span class="sxs-lookup"><span data-stu-id="61bb5-161">Right-click the *Hubs* folder in **Solution Explorer**, select **Web** > **SignalR**, and select **SignalR Hub Class (v2)**.</span></span> <span data-ttu-id="61bb5-162">Назовите новый концентратор *MyHub.cs* и выберите **добавить**.</span><span class="sxs-lookup"><span data-stu-id="61bb5-162">Name the new hub *MyHub.cs* and select **Add**.</span></span>

    ![Добавление класса концентратора SignalR в папку концентраторы в диалоговом окне Добавление нового элемента](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. <span data-ttu-id="61bb5-164">*MyHub.cs* автоматически открывается в главном окне.</span><span class="sxs-lookup"><span data-stu-id="61bb5-164">*MyHub.cs* will automatically open in the main window.</span></span> <span data-ttu-id="61bb5-165">Замените содержимое следующим кодом, а затем сохраните и закройте файл:</span><span class="sxs-lookup"><span data-stu-id="61bb5-165">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]
    
23. <span data-ttu-id="61bb5-166">*[Crank.exe](signalr-connection-density-testing-with-crank.md)*  является его плотность подключения тестирование, предоставляемый с SignalR базу кода.</span><span class="sxs-lookup"><span data-stu-id="61bb5-166">*[Crank.exe](signalr-connection-density-testing-with-crank.md)* is a connection density testing tool provided with the SignalR codebase.</span></span> <span data-ttu-id="61bb5-167">Поскольку ручки требуется постоянное подключение, его добавить на сайт для использования при тестировании.</span><span class="sxs-lookup"><span data-stu-id="61bb5-167">Since Crank requires a persistent connection, you add one to your site for use when testing.</span></span> <span data-ttu-id="61bb5-168">Добавить новую папку для **WebRole1** проект с именем *PersistentConnections*.</span><span class="sxs-lookup"><span data-stu-id="61bb5-168">Add a new folder to the **WebRole1** project called *PersistentConnections*.</span></span> <span data-ttu-id="61bb5-169">Щелкните правой кнопкой мыши эту папку и выберите **добавить** > **класса**.</span><span class="sxs-lookup"><span data-stu-id="61bb5-169">Right-click this folder and select **Add** > **Class**.</span></span> <span data-ttu-id="61bb5-170">Назовите новый файл класса *MyPersistentConnections.cs* и выберите **добавить**.</span><span class="sxs-lookup"><span data-stu-id="61bb5-170">Name the new class file *MyPersistentConnections.cs* and select **Add**.</span></span>

24. <span data-ttu-id="61bb5-171">Visual Studio откроет *MyPersistentConnections.cs* файл в главном окне.</span><span class="sxs-lookup"><span data-stu-id="61bb5-171">Visual Studio will open the *MyPersistentConnections.cs* file in the main window.</span></span> <span data-ttu-id="61bb5-172">Замените содержимое следующим кодом, а затем сохраните и закройте файл:</span><span class="sxs-lookup"><span data-stu-id="61bb5-172">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]
    
25. <span data-ttu-id="61bb5-173">С помощью `Startup` класса, объектов SignalR запуск при запуске OWIN.</span><span class="sxs-lookup"><span data-stu-id="61bb5-173">Using the `Startup` class, the SignalR objects start when OWIN starts up.</span></span> <span data-ttu-id="61bb5-174">Откройте или создайте *файла Startup.cs* и заменить его содержимое следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="61bb5-174">Open or create *Startup.cs* and replace the contents with the following code:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]
    
    <span data-ttu-id="61bb5-175">В представленном выше коде `OwinStartup` помечает этот класс для запуска OWIN.</span><span class="sxs-lookup"><span data-stu-id="61bb5-175">In the code above, the `OwinStartup` attribute marks this class to start OWIN.</span></span> <span data-ttu-id="61bb5-176">`Configuration` Метод начинает SignalR.</span><span class="sxs-lookup"><span data-stu-id="61bb5-176">The `Configuration` method starts SignalR.</span></span>
    
26. <span data-ttu-id="61bb5-177">Протестируйте приложения в эмуляторе Microsoft Azure, нажав клавишу **F5**.</span><span class="sxs-lookup"><span data-stu-id="61bb5-177">Test your application in the Microsoft Azure Emulator by pressing **F5**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="61bb5-178">При возникновении **FileLoadException** в **MapSignalR**, измените переадресации привязок в *web.config* следующее:</span><span class="sxs-lookup"><span data-stu-id="61bb5-178">If you encounter a **FileLoadException** at **MapSignalR**, change the binding redirects in *web.config* to the following:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]
    
27. <span data-ttu-id="61bb5-179">Подождите около одной минуты.</span><span class="sxs-lookup"><span data-stu-id="61bb5-179">Wait about one minute.</span></span> <span data-ttu-id="61bb5-180">Откройте окно инструментов Cloud Explorer в Visual Studio (**представление** > **Cloud Explorer**) и разверните путь `(Local)/Storage Accounts/(Development)/Tables`.</span><span class="sxs-lookup"><span data-stu-id="61bb5-180">Open the Cloud Explorer tool window in Visual Studio (**View** > **Cloud Explorer**) and expand the path `(Local)/Storage Accounts/(Development)/Tables`.</span></span> <span data-ttu-id="61bb5-181">Дважды щелкните **WADPerformanceCountersTable**.</span><span class="sxs-lookup"><span data-stu-id="61bb5-181">Double-click **WADPerformanceCountersTable**.</span></span> <span data-ttu-id="61bb5-182">Вы увидите счетчиков SignalR в таблице данных.</span><span class="sxs-lookup"><span data-stu-id="61bb5-182">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="61bb5-183">Если таблица не отображается, может потребоваться повторно введите учетные данные хранилища Azure.</span><span class="sxs-lookup"><span data-stu-id="61bb5-183">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="61bb5-184">Необходимо выбрать **обновление** кнопку, чтобы просмотреть таблицы в **Cloud Explorer** или выберите **обновление** кнопку в окне Открыть таблицу для просмотра данных в таблице.</span><span class="sxs-lookup"><span data-stu-id="61bb5-184">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

    ![При выборе таблицы счетчиков производительности WAD в Visual Studio Cloud Explorer](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![Отображение счетчиков собираться в таблице счетчиков производительности WAD](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)
    
28. <span data-ttu-id="61bb5-187">Чтобы протестировать приложение в облаке, обновите **ServiceConfiguration.Cloud.cscfg** и задайте `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` на допустимую строку подключения учетной записи хранилища Azure.</span><span class="sxs-lookup"><span data-stu-id="61bb5-187">To test your application in the cloud, update the **ServiceConfiguration.Cloud.cscfg** file and set the `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` to a valid Azure Storage account connection string.</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. <span data-ttu-id="61bb5-188">Развертывание приложения в вашей подписке Azure.</span><span class="sxs-lookup"><span data-stu-id="61bb5-188">Deploy the application to your Azure subscription.</span></span> <span data-ttu-id="61bb5-189">Дополнительные сведения о том, как развернуть приложение в Azure см. в разделе [Создание и развертывание облачной службы](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="61bb5-189">For details on how to deploy an application to Azure, see [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span>

30. <span data-ttu-id="61bb5-190">Подождите несколько минут.</span><span class="sxs-lookup"><span data-stu-id="61bb5-190">Wait a few minutes.</span></span> <span data-ttu-id="61bb5-191">В **Cloud Explorer**, найдите учетную запись хранения, настроенных выше и найдите `WADPerformanceCountersTable` таблицы в ней.</span><span class="sxs-lookup"><span data-stu-id="61bb5-191">In **Cloud Explorer**, locate the storage account you configured above and find the `WADPerformanceCountersTable` table in it.</span></span> <span data-ttu-id="61bb5-192">Вы увидите счетчиков SignalR в таблице данных.</span><span class="sxs-lookup"><span data-stu-id="61bb5-192">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="61bb5-193">Если таблица не отображается, может потребоваться повторно введите учетные данные хранилища Azure.</span><span class="sxs-lookup"><span data-stu-id="61bb5-193">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="61bb5-194">Необходимо выбрать **обновление** кнопку, чтобы просмотреть таблицы в **Cloud Explorer** или выберите **обновление** кнопку в окне Открыть таблицу для просмотра данных в таблице.</span><span class="sxs-lookup"><span data-stu-id="61bb5-194">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

<span data-ttu-id="61bb5-195">Благодарности [Ричард Мартин](https://social.msdn.microsoft.com/profile/Martin+Richard) для исходного содержимого, используемые в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="61bb5-195">Special thanks to [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) for the original content used in this tutorial.</span></span>
