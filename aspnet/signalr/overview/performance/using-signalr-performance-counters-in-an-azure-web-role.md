---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: Использование счетчиков производительности SignalR в веб-роли Azure | Документация Майкрософт
author: guardrex
description: Описывается установка и использование счетчиков производительности SignalR в веб-роли Azure.
keywords: Счетчик ASP.NET,SignalR,Performance, веб-роли azure
ms.author: riande
ms.date: 02/11/2017
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: acc9836535466a801f1f46ec18d05937e2e42af2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828689"
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a><span data-ttu-id="ce355-104">Использование счетчиков производительности SignalR в веб-роли Azure</span><span class="sxs-lookup"><span data-stu-id="ce355-104">Using SignalR performance counters in an Azure Web Role</span></span>

<span data-ttu-id="ce355-105">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="ce355-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ce355-106">Счетчики производительности SignalR используются для отслеживания производительности приложения в веб-роли Azure.</span><span class="sxs-lookup"><span data-stu-id="ce355-106">SignalR performance counters are used to monitor your app's performance in an Azure Web Role.</span></span> <span data-ttu-id="ce355-107">Счетчики регистрируются с помощью системы диагностики Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="ce355-107">The counters are captured by Microsoft Azure Diagnostics.</span></span> <span data-ttu-id="ce355-108">Установка счетчиков производительности SignalR в Azure с помощью *signalr.exe*, тот же инструмент, используемое для изолированными, так и локальных приложений.</span><span class="sxs-lookup"><span data-stu-id="ce355-108">You install SignalR performance counters on Azure with *signalr.exe*, the same tool used for standalone or on-premises apps.</span></span> <span data-ttu-id="ce355-109">Так как роли Azure являются временными, Настройка приложения для установки и регистрации счетчиков производительности SignalR при запуске.</span><span class="sxs-lookup"><span data-stu-id="ce355-109">Since Azure roles are transient, you configure an app to install and register SignalR performance counters upon startup.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ce355-110">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="ce355-110">Prerequisites</span></span>

* [<span data-ttu-id="ce355-111">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="ce355-111">Visual Studio 2015</span></span>](https://www.visualstudio.com/vs/visual-studio-express/)
* <span data-ttu-id="ce355-112">[Microsoft Azure SDK для Visual Studio 2015 (VS2015)](https://azure.microsoft.com/downloads/) **Примечание: Перезагрузите компьютер после установки пакета SDK.**</span><span class="sxs-lookup"><span data-stu-id="ce355-112">[Microsoft Azure SDK for Visual Studio 2015 (VS2015)](https://azure.microsoft.com/downloads/) **Note: Restart your machine after installing the SDK.**</span></span>
* <span data-ttu-id="ce355-113">Подписки Microsoft Azure: чтобы зарегистрироваться для получения бесплатной пробной учетной записи Azure, см. в разделе [бесплатной пробной версии Azure](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="ce355-113">Microsoft Azure subscription: To sign up for a free Azure trial account, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a><span data-ttu-id="ce355-114">Создание приложения веб-роли Azure, предоставляет счетчики производительности SignalR</span><span class="sxs-lookup"><span data-stu-id="ce355-114">Creating an Azure Web Role application that exposes SignalR performance counters</span></span>

1. <span data-ttu-id="ce355-115">Откройте Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="ce355-115">Open Visual Studio 2015.</span></span>

2. <span data-ttu-id="ce355-116">В Visual Studio 2015 выберите **файл** > **New** > **проекта**.</span><span class="sxs-lookup"><span data-stu-id="ce355-116">In Visual Studio 2015, select **File** > **New** > **Project**.</span></span>

3. <span data-ttu-id="ce355-117">В **шаблоны** области **новый проект** под **Visual C#** выберите **Cloud** узел и выберите **Облачной службы azure** шаблона.</span><span class="sxs-lookup"><span data-stu-id="ce355-117">In the **Templates** pane of the **New Project** window under the **Visual C#** node, select the **Cloud** node and select the **Azure Cloud Service** template.</span></span> <span data-ttu-id="ce355-118">Присвойте приложению имя **SignalRPerfCounters** и выберите **ОК**.</span><span class="sxs-lookup"><span data-stu-id="ce355-118">Name the app **SignalRPerfCounters** and select **OK**.</span></span>

   ![Новое Облачное приложение](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)
    
4. <span data-ttu-id="ce355-120">В **новая облачная служба Microsoft Azure** диалоговом окне выберите **веб-роль ASP.NET** и выберите >, чтобы добавить роль к проекту.</span><span class="sxs-lookup"><span data-stu-id="ce355-120">In the **New Microsoft Azure Cloud Service** dialog, select **ASP.NET Web Role** and select the > button to add the role to the project.</span></span> <span data-ttu-id="ce355-121">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="ce355-121">Select **OK**.</span></span>

   ![Добавление веб-роль ASP.NET](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)
    
5. <span data-ttu-id="ce355-123">В **новый веб-приложение ASP.NET — WebRole1** диалоговом окне выберите **MVC** шаблона, а затем выберите **ОК**.</span><span class="sxs-lookup"><span data-stu-id="ce355-123">In the **New ASP.NET Web Application - WebRole1** dialog, select the **MVC** template, and then select **OK**.</span></span>

   ![Добавление MVC и веб-API](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)
    
6. <span data-ttu-id="ce355-125">В **обозревателе решений**откройте *diagnostics.wadcfgx* файл **WebRole1**.</span><span class="sxs-lookup"><span data-stu-id="ce355-125">In **Solution Explorer**, open the *diagnostics.wadcfgx* file under **WebRole1**.</span></span>

   ![Diagnostics.wadcfgx обозревателя решений](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)
    
7. <span data-ttu-id="ce355-127">Замените содержимое файла со следующей конфигурацией и сохраните файл:</span><span class="sxs-lookup"><span data-stu-id="ce355-127">Replace the contents of the file with the following configuration and save the file:</span></span>

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]
    
8. <span data-ttu-id="ce355-128">Откройте **консоль диспетчера пакетов** из **средства** > **диспетчер пакетов NuGet**.</span><span class="sxs-lookup"><span data-stu-id="ce355-128">Open the **Package Manager Console** from **Tools** > **NuGet Package Manager**.</span></span> <span data-ttu-id="ce355-129">Введите следующие команды, чтобы установить последнюю версию SignalR и служебные программы пакета SignalR.</span><span class="sxs-lookup"><span data-stu-id="ce355-129">Enter the following commands to install the latest version of SignalR and the SignalR utilities package:</span></span>

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]
    
9. <span data-ttu-id="ce355-130">Настройте приложение для установки счетчиков производительности SignalR в экземпляр роли при его запуске и перезапускается.</span><span class="sxs-lookup"><span data-stu-id="ce355-130">Configure the app to install the SignalR performance counters into the role instance when it starts up or recycles.</span></span> <span data-ttu-id="ce355-131">В **обозревателе решений**, щелкните правой кнопкой мыши **WebRole1** проекта и выберите **добавить** > **новую папку**.</span><span class="sxs-lookup"><span data-stu-id="ce355-131">In **Solution Explorer**, right-click on the **WebRole1** project and select **Add** > **New Folder**.</span></span> <span data-ttu-id="ce355-132">Назовите новую папку *запуска*.</span><span class="sxs-lookup"><span data-stu-id="ce355-132">Name the new folder *Startup*.</span></span>

   ![Добавление папка "Автозагрузка"](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)
    
10. <span data-ttu-id="ce355-134">Копировать *signalr.exe* файл (добавлены с классом **Microsoft.AspNet.SignalR.Utils** пакета) из \<папки проекта > / SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\< версии > / средств *запуска* папку, созданную на предыдущем шаге.</span><span class="sxs-lookup"><span data-stu-id="ce355-134">Copy the *signalr.exe* file (added with the **Microsoft.AspNet.SignalR.Utils** package) from \<project folder>/SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\<version>/tools to the *Startup* folder you created in the previous step.</span></span>

11. <span data-ttu-id="ce355-135">В **обозревателе решений**, щелкните правой кнопкой мыши *запуска* папку и выберите **добавить** > **существующий элемент**.</span><span class="sxs-lookup"><span data-stu-id="ce355-135">In **Solution Explorer**, right-click the *Startup* folder and select **Add** > **Existing Item**.</span></span> <span data-ttu-id="ce355-136">В появившемся диалоговом окне выберите *signalr.exe* и выберите **добавить**.</span><span class="sxs-lookup"><span data-stu-id="ce355-136">In the dialog that appears, select *signalr.exe* and select **Add**.</span></span>

    ![Добавление signalr.exe проект](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)
    
12. <span data-ttu-id="ce355-138">Щелкните правой кнопкой мыши *запуска* созданной папке.</span><span class="sxs-lookup"><span data-stu-id="ce355-138">Right-click on the *Startup* folder you created.</span></span> <span data-ttu-id="ce355-139">Выберите **Добавить** > **Новый объект**.</span><span class="sxs-lookup"><span data-stu-id="ce355-139">Select **Add** > **New Item**.</span></span> <span data-ttu-id="ce355-140">Выберите **Общие** выберите **текстовый файл**и назовите новый элемент *SignalRPerfCounterInstall.cmd*.</span><span class="sxs-lookup"><span data-stu-id="ce355-140">Select the **General** node, select **Text File**, and name the new item *SignalRPerfCounterInstall.cmd*.</span></span> <span data-ttu-id="ce355-141">Этот командный файл устанавливает счетчики производительности SignalR в веб-роли.</span><span class="sxs-lookup"><span data-stu-id="ce355-141">This command file will install the SignalR performance counters into the web role.</span></span>

    ![Создание файла пакета установки счетчиков производительности SignalR](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)
     
13. <span data-ttu-id="ce355-143">Когда Visual Studio создает *SignalRPerfCounterInstall.cmd* файл, он автоматически открывается в главном окне.</span><span class="sxs-lookup"><span data-stu-id="ce355-143">When Visual Studio creates the *SignalRPerfCounterInstall.cmd* file, it will automatically open in the main window.</span></span> <span data-ttu-id="ce355-144">Замените содержимое файла следующий сценарий, а затем сохраните и закройте файл.</span><span class="sxs-lookup"><span data-stu-id="ce355-144">Replace the contents of the file with the following script, then save and close the file.</span></span> <span data-ttu-id="ce355-145">Этот сценарий выполняет *signalr.exe*, который добавляет счетчики производительности SignalR в экземпляр роли.</span><span class="sxs-lookup"><span data-stu-id="ce355-145">This script executes *signalr.exe*, which adds the SignalR performance counters to the role instance.</span></span>

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]
    
14. <span data-ttu-id="ce355-146">Выберите *signalr.exe* файл **обозревателе решений**.</span><span class="sxs-lookup"><span data-stu-id="ce355-146">Select the *signalr.exe* file in **Solution Explorer**.</span></span> <span data-ttu-id="ce355-147">В файле **свойства**, задайте **Копировать в выходной каталог** для **всегда Копировать**.</span><span class="sxs-lookup"><span data-stu-id="ce355-147">In the file's **Properties**, set **Copy to Output Directory** to **Copy Always**.</span></span>

    ![Переключите копию в выходной каталог, чтобы всегда Копировать](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)
    
15. <span data-ttu-id="ce355-149">Повторите предыдущий шаг для *SignalRPerfCounterInstall.cmd* файла.</span><span class="sxs-lookup"><span data-stu-id="ce355-149">Repeat the previous step for the *SignalRPerfCounterInstall.cmd* file.</span></span>

    
16. <span data-ttu-id="ce355-150">Щелкните правой кнопкой мыши *SignalRPerfCounterInstall.cmd* файл и выберите **открыть с помощью**.</span><span class="sxs-lookup"><span data-stu-id="ce355-150">Right-click on the *SignalRPerfCounterInstall.cmd* file and select **Open With**.</span></span> <span data-ttu-id="ce355-151">В появившемся диалоговом окне выберите **двоичный редактор** и выберите **ОК**.</span><span class="sxs-lookup"><span data-stu-id="ce355-151">In the dialog that appears, select **Binary Editor** and select **OK**.</span></span>

    ![Открыть двоичный редактор](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)
    
17. <span data-ttu-id="ce355-153">В двоичном редакторе выберите все начальные байты в файле и удалите их.</span><span class="sxs-lookup"><span data-stu-id="ce355-153">In the binary editor, select any leading bytes in the file and delete them.</span></span> <span data-ttu-id="ce355-154">Сохраните и закройте файл.</span><span class="sxs-lookup"><span data-stu-id="ce355-154">Save and close the file.</span></span>

    ![Удаление начальных байтов](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)
    
18. <span data-ttu-id="ce355-156">Откройте *ServiceDefinition.csdef* и добавить задачу запуска, который выполняет *SignalrPerfCounterInstall.cmd* файл при запуске службы:</span><span class="sxs-lookup"><span data-stu-id="ce355-156">Open *ServiceDefinition.csdef* and add a startup task that executes the *SignalrPerfCounterInstall.cmd* file when the service starts up:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]
    
19. <span data-ttu-id="ce355-157">Откройте `Views/Shared/_Layout.cshtml` и удалить сценарий jQuery пакета из конца файла.</span><span class="sxs-lookup"><span data-stu-id="ce355-157">Open `Views/Shared/_Layout.cshtml` and remove the jQuery bundle script from the end of the file.</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]
    
20. <span data-ttu-id="ce355-158">Добавьте клиент JavaScript, который постоянно вызывает `increment` метод на сервере.</span><span class="sxs-lookup"><span data-stu-id="ce355-158">Add a JavaScript client that continuously calls the `increment` method on the server.</span></span> <span data-ttu-id="ce355-159">Откройте `Views/Home/Index.cshtml` и замените его содержимое следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="ce355-159">Open `Views/Home/Index.cshtml` and replace the contents with the following code:</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]
    
21. <span data-ttu-id="ce355-160">Создайте новую папку в **WebRole1** проект с именем *концентраторов*.</span><span class="sxs-lookup"><span data-stu-id="ce355-160">Create a new folder in the **WebRole1** project named *Hubs*.</span></span> <span data-ttu-id="ce355-161">Щелкните правой кнопкой мыши *концентраторов* папку в **обозревателе решений**выберите **Web** > **SignalR**и выберите  **Класс концентратора SignalR (v2)**.</span><span class="sxs-lookup"><span data-stu-id="ce355-161">Right-click the *Hubs* folder in **Solution Explorer**, select **Web** > **SignalR**, and select **SignalR Hub Class (v2)**.</span></span> <span data-ttu-id="ce355-162">Назовите новый концентратор *MyHub.cs* и выберите **добавить**.</span><span class="sxs-lookup"><span data-stu-id="ce355-162">Name the new hub *MyHub.cs* and select **Add**.</span></span>

    ![Добавляемый класс концентратора SignalR в папку концентраторов в диалоговом окне Добавление нового элемента](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. <span data-ttu-id="ce355-164">*MyHub.cs* автоматически открывается в главном окне.</span><span class="sxs-lookup"><span data-stu-id="ce355-164">*MyHub.cs* will automatically open in the main window.</span></span> <span data-ttu-id="ce355-165">Замените содержимое следующим кодом, а затем сохраните и закройте файл:</span><span class="sxs-lookup"><span data-stu-id="ce355-165">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]
    
23. <span data-ttu-id="ce355-166">*[Crank.exe](signalr-connection-density-testing-with-crank.md)*  является тестирование, предоставляемый с SignalR codebase плотности подключений.</span><span class="sxs-lookup"><span data-stu-id="ce355-166">*[Crank.exe](signalr-connection-density-testing-with-crank.md)* is a connection density testing tool provided with the SignalR codebase.</span></span> <span data-ttu-id="ce355-167">Так как Crank требуется постоянное подключение, его добавить на сайт для использования при тестировании.</span><span class="sxs-lookup"><span data-stu-id="ce355-167">Since Crank requires a persistent connection, you add one to your site for use when testing.</span></span> <span data-ttu-id="ce355-168">Добавьте новую папку для **WebRole1** проект с именем *PersistentConnections*.</span><span class="sxs-lookup"><span data-stu-id="ce355-168">Add a new folder to the **WebRole1** project called *PersistentConnections*.</span></span> <span data-ttu-id="ce355-169">Щелкните правой кнопкой мыши эту папку и выберите **добавить** > **класс**.</span><span class="sxs-lookup"><span data-stu-id="ce355-169">Right-click this folder and select **Add** > **Class**.</span></span> <span data-ttu-id="ce355-170">Назовите новый файл класса *MyPersistentConnections.cs* и выберите **добавить**.</span><span class="sxs-lookup"><span data-stu-id="ce355-170">Name the new class file *MyPersistentConnections.cs* and select **Add**.</span></span>

24. <span data-ttu-id="ce355-171">Visual Studio открывает *MyPersistentConnections.cs* файл в главном окне.</span><span class="sxs-lookup"><span data-stu-id="ce355-171">Visual Studio will open the *MyPersistentConnections.cs* file in the main window.</span></span> <span data-ttu-id="ce355-172">Замените содержимое следующим кодом, а затем сохраните и закройте файл:</span><span class="sxs-lookup"><span data-stu-id="ce355-172">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]
    
25. <span data-ttu-id="ce355-173">С помощью `Startup` класс, объекты SignalR запуск при запуске OWIN.</span><span class="sxs-lookup"><span data-stu-id="ce355-173">Using the `Startup` class, the SignalR objects start when OWIN starts up.</span></span> <span data-ttu-id="ce355-174">Откройте или создайте *Startup.cs* и замените его содержимое следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="ce355-174">Open or create *Startup.cs* and replace the contents with the following code:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]
    
    <span data-ttu-id="ce355-175">В приведенном выше коде `OwinStartup` атрибут помечает этот класс для запуска OWIN.</span><span class="sxs-lookup"><span data-stu-id="ce355-175">In the code above, the `OwinStartup` attribute marks this class to start OWIN.</span></span> <span data-ttu-id="ce355-176">`Configuration` Метод начинает SignalR.</span><span class="sxs-lookup"><span data-stu-id="ce355-176">The `Configuration` method starts SignalR.</span></span>
    
26. <span data-ttu-id="ce355-177">Протестируйте приложение в эмуляторе Microsoft Azure, нажав клавишу **F5**.</span><span class="sxs-lookup"><span data-stu-id="ce355-177">Test your application in the Microsoft Azure Emulator by pressing **F5**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ce355-178">При возникновении **FileLoadException** в **MapSignalR**, изменить переадресации привязок в *web.config* следующим:</span><span class="sxs-lookup"><span data-stu-id="ce355-178">If you encounter a **FileLoadException** at **MapSignalR**, change the binding redirects in *web.config* to the following:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]
    
27. <span data-ttu-id="ce355-179">Подождите около одной минуты.</span><span class="sxs-lookup"><span data-stu-id="ce355-179">Wait about one minute.</span></span> <span data-ttu-id="ce355-180">Откройте окно инструментов Cloud Explorer в Visual Studio (**представление** > **Cloud Explorer**) и разверните путь `(Local)/Storage Accounts/(Development)/Tables`.</span><span class="sxs-lookup"><span data-stu-id="ce355-180">Open the Cloud Explorer tool window in Visual Studio (**View** > **Cloud Explorer**) and expand the path `(Local)/Storage Accounts/(Development)/Tables`.</span></span> <span data-ttu-id="ce355-181">Дважды щелкните **WADPerformanceCountersTable**.</span><span class="sxs-lookup"><span data-stu-id="ce355-181">Double-click **WADPerformanceCountersTable**.</span></span> <span data-ttu-id="ce355-182">Вы должны увидеть счетчиков SignalR в таблице данных.</span><span class="sxs-lookup"><span data-stu-id="ce355-182">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="ce355-183">Если вы не видите таблицу, может потребоваться повторно ввести учетные данные хранилища Azure.</span><span class="sxs-lookup"><span data-stu-id="ce355-183">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="ce355-184">Необходимо выбрать **обновить** кнопку, чтобы просмотреть таблицу в **Cloud Explorer** или выберите **обновить** кнопку в окне Открыть таблицу для просмотра данных в таблице.</span><span class="sxs-lookup"><span data-stu-id="ce355-184">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

    ![Выбор таблицы счетчиков производительности WAD в Visual Studio Cloud Explorer](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![Отображение счетчиков собираются в таблице счетчиков производительности WAD](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)
    
28. <span data-ttu-id="ce355-187">Чтобы протестировать приложение в облаке, обновите **ServiceConfiguration.Cloud.cscfg** файл и задайте для `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` допустимую строку подключения учетной записи хранилища Azure.</span><span class="sxs-lookup"><span data-stu-id="ce355-187">To test your application in the cloud, update the **ServiceConfiguration.Cloud.cscfg** file and set the `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` to a valid Azure Storage account connection string.</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. <span data-ttu-id="ce355-188">Развертывание приложения в вашей подписке Azure.</span><span class="sxs-lookup"><span data-stu-id="ce355-188">Deploy the application to your Azure subscription.</span></span> <span data-ttu-id="ce355-189">Дополнительные сведения о том, как развернуть приложение в Azure, см. в разделе [как создать и развернуть облачную службу](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="ce355-189">For details on how to deploy an application to Azure, see [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span>

30. <span data-ttu-id="ce355-190">Подождите несколько минут.</span><span class="sxs-lookup"><span data-stu-id="ce355-190">Wait a few minutes.</span></span> <span data-ttu-id="ce355-191">В **Cloud Explorer**, найдите учетную запись хранения, настроенных выше и найти `WADPerformanceCountersTable` таблицы в ней.</span><span class="sxs-lookup"><span data-stu-id="ce355-191">In **Cloud Explorer**, locate the storage account you configured above and find the `WADPerformanceCountersTable` table in it.</span></span> <span data-ttu-id="ce355-192">Вы должны увидеть счетчиков SignalR в таблице данных.</span><span class="sxs-lookup"><span data-stu-id="ce355-192">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="ce355-193">Если вы не видите таблицу, может потребоваться повторно ввести учетные данные хранилища Azure.</span><span class="sxs-lookup"><span data-stu-id="ce355-193">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="ce355-194">Необходимо выбрать **обновить** кнопку, чтобы просмотреть таблицу в **Cloud Explorer** или выберите **обновить** кнопку в окне Открыть таблицу для просмотра данных в таблице.</span><span class="sxs-lookup"><span data-stu-id="ce355-194">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

<span data-ttu-id="ce355-195">Особая благодарность [Ричард Мартин](https://social.msdn.microsoft.com/profile/Martin+Richard) для исходного содержимого, используемые в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="ce355-195">Special thanks to [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) for the original content used in this tutorial.</span></span>
