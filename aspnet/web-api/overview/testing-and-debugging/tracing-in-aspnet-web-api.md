---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: Трассировка в ASP.NET Web API 2 | Документация Майкрософт
author: MikeWasson
description: Показано, как включить трассировку в веб-API ASP.NET.
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 66a837e9-600b-4b72-97a9-19804231c64a
msc.legacyurl: /web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 426e3500c504a651a325eec021e2d9e8b9cc1d32
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795284"
---
<a name="tracing-in-aspnet-web-api-2"></a><span data-ttu-id="99f8b-103">Трассировка в ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="99f8b-103">Tracing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="99f8b-104">по [Майк Уоссон](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="99f8b-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="99f8b-105">Когда вы пытаетесь отладить веб-приложение, нет альтернативы для хороший набор журналов трассировки.</span><span class="sxs-lookup"><span data-stu-id="99f8b-105">When you are trying to debug a web-based application, there is no substitute for a good set of trace logs.</span></span> <span data-ttu-id="99f8b-106">Этом руководстве показано, как включить трассировку в веб-API ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="99f8b-106">This tutorial shows how to enable tracing in ASP.NET Web API.</span></span> <span data-ttu-id="99f8b-107">Эту функцию можно использовать для трассировки, что платформа веб-API делает до и после он вызывает вашего контроллера.</span><span class="sxs-lookup"><span data-stu-id="99f8b-107">You can use this feature to trace what the Web API framework does before and after it invokes your controller.</span></span> <span data-ttu-id="99f8b-108">Также можно отслеживать собственный код.</span><span class="sxs-lookup"><span data-stu-id="99f8b-108">You can also use it to trace your own code.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="99f8b-109">Версии программного обеспечения, используемые в этом руководстве</span><span class="sxs-lookup"><span data-stu-id="99f8b-109">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="99f8b-110">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) (также работает с Visual Studio 2015)</span><span class="sxs-lookup"><span data-stu-id="99f8b-110">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) (also works with Visual Studio 2015)</span></span>
> - <span data-ttu-id="99f8b-111">Веб-API 2</span><span class="sxs-lookup"><span data-stu-id="99f8b-111">Web API 2</span></span>
> - [<span data-ttu-id="99f8b-112">Microsoft.AspNet.WebApi.Tracing</span><span class="sxs-lookup"><span data-stu-id="99f8b-112">Microsoft.AspNet.WebApi.Tracing</span></span>](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)

## <a name="enable-systemdiagnostics-tracing-in-web-api"></a><span data-ttu-id="99f8b-113">Включение трассировки в веб-API System.Diagnostics</span><span class="sxs-lookup"><span data-stu-id="99f8b-113">Enable System.Diagnostics Tracing in Web API</span></span>

<span data-ttu-id="99f8b-114">Во-первых мы создадим новый проект веб-приложения ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="99f8b-114">First, we'll create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="99f8b-115">В Visual Studio из **файл** меню, выберите **New** > **проекта**.</span><span class="sxs-lookup"><span data-stu-id="99f8b-115">In Visual Studio, from the **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="99f8b-116">В разделе **шаблоны**, **Web**выберите **веб-приложение ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="99f8b-116">Under **Templates**, **Web**, select **ASP.NET Web Application**.</span></span>

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="99f8b-117">Выберите шаблон проекта веб-API.</span><span class="sxs-lookup"><span data-stu-id="99f8b-117">Choose the Web API project template.</span></span>

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

<span data-ttu-id="99f8b-118">Из **средства** меню, выберите **диспетчер пакетов библиотеки**, затем **консоли**.</span><span class="sxs-lookup"><span data-stu-id="99f8b-118">From the **Tools** menu, select **Library Package Manager**, then **Package Manage Console**.</span></span>

<span data-ttu-id="99f8b-119">В окне консоли диспетчера пакетов введите следующие команды.</span><span class="sxs-lookup"><span data-stu-id="99f8b-119">In the Package Manager Console window, type the following commands.</span></span>

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

<span data-ttu-id="99f8b-120">Первая команда устанавливает последнюю версию пакета трассировки веб-API.</span><span class="sxs-lookup"><span data-stu-id="99f8b-120">The first command installs the latest Web API tracing package.</span></span> <span data-ttu-id="99f8b-121">Он также обновляет основные пакеты веб-API.</span><span class="sxs-lookup"><span data-stu-id="99f8b-121">It also updates the core Web API packages.</span></span> <span data-ttu-id="99f8b-122">Вторая команда обновляет пакет WebApi.WebHost до последней версии.</span><span class="sxs-lookup"><span data-stu-id="99f8b-122">The second command updates the WebApi.WebHost package to the latest version.</span></span>

> [!NOTE]
> <span data-ttu-id="99f8b-123">Если вы хотите охватить определенную версию веб-API, используйте флаг версии, при установке пакета трассировки.</span><span class="sxs-lookup"><span data-stu-id="99f8b-123">If you want to target a specific version of Web API, use the -Version flag when you install the tracing package.</span></span>


<span data-ttu-id="99f8b-124">Откройте файл WebApiConfig.cs в приложении\_Начальная папка.</span><span class="sxs-lookup"><span data-stu-id="99f8b-124">Open the file WebApiConfig.cs in the App\_Start folder.</span></span> <span data-ttu-id="99f8b-125">Добавьте следующий код, чтобы **зарегистрировать** метод.</span><span class="sxs-lookup"><span data-stu-id="99f8b-125">Add the following code to the **Register** method.</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

<span data-ttu-id="99f8b-126">Этот код добавляет [пространство имен SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) класс в конвейер веб-API.</span><span class="sxs-lookup"><span data-stu-id="99f8b-126">This code adds the [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) class to the Web API pipeline.</span></span> <span data-ttu-id="99f8b-127">**Пространство имен SystemDiagnosticsTraceWriter** класс записывает трассировки, чтобы [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace).</span><span class="sxs-lookup"><span data-stu-id="99f8b-127">The **SystemDiagnosticsTraceWriter** class writes traces to [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace).</span></span>

<span data-ttu-id="99f8b-128">Чтобы просмотреть трассировки, запустите приложение в отладчике.</span><span class="sxs-lookup"><span data-stu-id="99f8b-128">To see the traces, run the application in the debugger.</span></span> <span data-ttu-id="99f8b-129">В браузере перейдите к `/api/values`.</span><span class="sxs-lookup"><span data-stu-id="99f8b-129">In the browser, navigate to `/api/values`.</span></span>

![](tracing-in-aspnet-web-api/_static/image5.png)

<span data-ttu-id="99f8b-130">Инструкции трассировки записываются в окно вывода в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="99f8b-130">The trace statements are written to the Output window in Visual Studio.</span></span> <span data-ttu-id="99f8b-131">(Из **представление** меню, выберите **вывода**).</span><span class="sxs-lookup"><span data-stu-id="99f8b-131">(From the **View** menu, select **Output**).</span></span>

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

<span data-ttu-id="99f8b-132">Так как **пространство имен SystemDiagnosticsTraceWriter** записывает трассировки, чтобы **System.Diagnostics.Trace**, вы можете зарегистрировать дополнительные прослушиватели трассировки; например, чтобы записать трассировки в файл журнала.</span><span class="sxs-lookup"><span data-stu-id="99f8b-132">Because **SystemDiagnosticsTraceWriter** writes traces to **System.Diagnostics.Trace**, you can register additional trace listeners; for example, to write traces to a log file.</span></span> <span data-ttu-id="99f8b-133">Дополнительные сведения о записи трассировки, см. в разделе [прослушиватели трассировки](https://msdn.microsoft.com/library/4y5y10s7.aspx) в MSDN.</span><span class="sxs-lookup"><span data-stu-id="99f8b-133">For more information about trace writers, see the [Trace Listeners](https://msdn.microsoft.com/library/4y5y10s7.aspx) topic on MSDN.</span></span>

### <a name="configuring-systemdiagnosticstracewriter"></a><span data-ttu-id="99f8b-134">Настройка пространство имен SystemDiagnosticsTraceWriter</span><span class="sxs-lookup"><span data-stu-id="99f8b-134">Configuring SystemDiagnosticsTraceWriter</span></span>

<span data-ttu-id="99f8b-135">Ниже показано, как настроить модуль записи трассировки.</span><span class="sxs-lookup"><span data-stu-id="99f8b-135">The following code shows how to configure the trace writer.</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="99f8b-136">Существует два параметра, которыми можно управлять:</span><span class="sxs-lookup"><span data-stu-id="99f8b-136">There are two settings that you can control:</span></span>

- <span data-ttu-id="99f8b-137">IsVerbose: Если значение равно false, каждой трассировки содержит минимум информации.</span><span class="sxs-lookup"><span data-stu-id="99f8b-137">IsVerbose: If false, each trace contains minimal information.</span></span> <span data-ttu-id="99f8b-138">Если значение равно true, трассировки включают дополнительные сведения.</span><span class="sxs-lookup"><span data-stu-id="99f8b-138">If true, traces include more information.</span></span>
- <span data-ttu-id="99f8b-139">MinimumLevel: Задает минимальный уровень трассировки.</span><span class="sxs-lookup"><span data-stu-id="99f8b-139">MinimumLevel: Sets the minimum trace level.</span></span> <span data-ttu-id="99f8b-140">Уровни трассировки, в порядке,:, отладки, Info, Warn, ошибка и Неустранимая ошибка.</span><span class="sxs-lookup"><span data-stu-id="99f8b-140">Trace levels, in order, are Debug, Info, Warn, Error, and Fatal.</span></span>

## <a name="adding-traces-to-your-web-api-application"></a><span data-ttu-id="99f8b-141">Добавление трассировки в приложение веб-API</span><span class="sxs-lookup"><span data-stu-id="99f8b-141">Adding Traces to Your Web API Application</span></span>

<span data-ttu-id="99f8b-142">Добавление записи трассировки предоставляющим немедленный доступ к трассировок, созданных по конвейеру веб-API.</span><span class="sxs-lookup"><span data-stu-id="99f8b-142">Adding a trace writer gives you immediate access to the traces created by the Web API pipeline.</span></span> <span data-ttu-id="99f8b-143">Также можно использовать модуль записи трассировки для трассировки кода:</span><span class="sxs-lookup"><span data-stu-id="99f8b-143">You can also use the trace writer to trace your own code:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

<span data-ttu-id="99f8b-144">Чтобы получить модуль записи трассировки, вызовите **HttpConfiguration.Services.GetTraceWriter**.</span><span class="sxs-lookup"><span data-stu-id="99f8b-144">To get the trace writer, call **HttpConfiguration.Services.GetTraceWriter**.</span></span> <span data-ttu-id="99f8b-145">От контроллера, этот метод доступен через **ApiController.Configuration** свойство.</span><span class="sxs-lookup"><span data-stu-id="99f8b-145">From a controller, this method is accessible through the **ApiController.Configuration** property.</span></span>

<span data-ttu-id="99f8b-146">Для записи трассировки, можно вызвать **ITraceWriter.Trace** метод напрямую, но [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) класс определяет некоторые методы расширений, которые являются более понятным.</span><span class="sxs-lookup"><span data-stu-id="99f8b-146">To write a trace, you can call the **ITraceWriter.Trace** method directly, but the [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) class defines some extension methods that are more friendly.</span></span> <span data-ttu-id="99f8b-147">Например **Info** в приведенном выше примере создает трассировку с уровнем трассировки **Info**.</span><span class="sxs-lookup"><span data-stu-id="99f8b-147">For example, the **Info** method shown above creates a trace with trace level **Info**.</span></span>

## <a name="web-api-tracing-infrastructure"></a><span data-ttu-id="99f8b-148">Инфраструктура трассировки веб-API</span><span class="sxs-lookup"><span data-stu-id="99f8b-148">Web API Tracing Infrastructure</span></span>

<span data-ttu-id="99f8b-149">В этом разделе описывается, как написать средство записи отслеживания для веб-API.</span><span class="sxs-lookup"><span data-stu-id="99f8b-149">This section describes how to write a custom trace writer for Web API.</span></span>

<span data-ttu-id="99f8b-150">Пакет Microsoft.AspNet.WebApi.Tracing надстраивается над более общими инфраструктура трассировки в веб-API.</span><span class="sxs-lookup"><span data-stu-id="99f8b-150">The Microsoft.AspNet.WebApi.Tracing package is built on top of a more general tracing infrastructure in Web API.</span></span> <span data-ttu-id="99f8b-151">Вместо использования Microsoft.AspNet.WebApi.Tracing, можно также подключить другой библиотеки трассировки/входа, такие как [NLog](http://nlog-project.org/) или [log4net](http://logging.apache.org/log4net/).</span><span class="sxs-lookup"><span data-stu-id="99f8b-151">Instead of using Microsoft.AspNet.WebApi.Tracing, you can also plug in some other tracing/loggin library, such as [NLog](http://nlog-project.org/) or [log4net](http://logging.apache.org/log4net/).</span></span>

<span data-ttu-id="99f8b-152">Для сбора данных трассировки, реализовать **ITraceWriter** интерфейс.</span><span class="sxs-lookup"><span data-stu-id="99f8b-152">To collect traces, implement the **ITraceWriter** interface.</span></span> <span data-ttu-id="99f8b-153">Ниже приведен простой пример:</span><span class="sxs-lookup"><span data-stu-id="99f8b-153">Here is a simple example:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="99f8b-154">**ITraceWriter.Trace** метод создает объект trace.</span><span class="sxs-lookup"><span data-stu-id="99f8b-154">The **ITraceWriter.Trace** method creates a trace.</span></span> <span data-ttu-id="99f8b-155">Вызывающий объект задает уровень категории и трассировки.</span><span class="sxs-lookup"><span data-stu-id="99f8b-155">The caller specifies a category and trace level.</span></span> <span data-ttu-id="99f8b-156">Категория может быть любая строка, определяемые пользователем.</span><span class="sxs-lookup"><span data-stu-id="99f8b-156">The category can be any user-defined string.</span></span> <span data-ttu-id="99f8b-157">Реализация **трассировки** делать следующее:</span><span class="sxs-lookup"><span data-stu-id="99f8b-157">Your implementation of **Trace** should do the following:</span></span>

1. <span data-ttu-id="99f8b-158">Создайте новый **TraceRecord**.</span><span class="sxs-lookup"><span data-stu-id="99f8b-158">Create a new **TraceRecord**.</span></span> <span data-ttu-id="99f8b-159">Инициализируйте его с запросом, категорию и уровень трассировки, как показано.</span><span class="sxs-lookup"><span data-stu-id="99f8b-159">Initialize it with the request, category, and trace level, as shown.</span></span> <span data-ttu-id="99f8b-160">Эти значения предоставляемые вызывающим объектом.</span><span class="sxs-lookup"><span data-stu-id="99f8b-160">These values are provided by the caller.</span></span>
2. <span data-ttu-id="99f8b-161">Вызвать *traceAction* делегировать.</span><span class="sxs-lookup"><span data-stu-id="99f8b-161">Invoke the *traceAction* delegate.</span></span> <span data-ttu-id="99f8b-162">Внутри этот делегат, вызывающий объект должен заполнить в остальной части **TraceRecord**.</span><span class="sxs-lookup"><span data-stu-id="99f8b-162">Inside this delegate, the caller is expected to fill in the rest of the **TraceRecord**.</span></span>
3. <span data-ttu-id="99f8b-163">Запись **TraceRecord**, с помощью любой подходящей методики ведения журнала, что вам нравится.</span><span class="sxs-lookup"><span data-stu-id="99f8b-163">Write the **TraceRecord**, using any logging technique that you like.</span></span> <span data-ttu-id="99f8b-164">В следующем примере просто вызывает **System.Diagnostics.Trace**.</span><span class="sxs-lookup"><span data-stu-id="99f8b-164">The example shown here simply calls into **System.Diagnostics.Trace**.</span></span>

## <a name="setting-the-trace-writer"></a><span data-ttu-id="99f8b-165">Настройка записи трассировки</span><span class="sxs-lookup"><span data-stu-id="99f8b-165">Setting the Trace Writer</span></span>

<span data-ttu-id="99f8b-166">Чтобы включить трассировку, необходимо настроить веб-API для использования вашей **ITraceWriter** реализации.</span><span class="sxs-lookup"><span data-stu-id="99f8b-166">To enable tracing, you must configure Web API to use your **ITraceWriter** implementation.</span></span> <span data-ttu-id="99f8b-167">Это сделать через **HttpConfiguration** объекта, как показано в следующем коде:</span><span class="sxs-lookup"><span data-stu-id="99f8b-167">You do this through the **HttpConfiguration** object, as shown in the following code:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="99f8b-168">Модуль записи трассировки только один может быть активна.</span><span class="sxs-lookup"><span data-stu-id="99f8b-168">Only one trace writer can be active.</span></span> <span data-ttu-id="99f8b-169">По умолчанию устанавливает веб-API &quot;холостой&quot; слежения, который не выполняет никаких действий.</span><span class="sxs-lookup"><span data-stu-id="99f8b-169">By default, Web API sets a &quot;no-op&quot; tracer that does nothing.</span></span> <span data-ttu-id="99f8b-170">( &quot;Холостой&quot; трассировочного существует, поэтому код трассировки не нужно проверить, является ли модуль записи трассировки **null** перед записью трассировки.)</span><span class="sxs-lookup"><span data-stu-id="99f8b-170">(The &quot;no-op&quot; tracer exists so that tracing code does not have to check whether the trace writer is **null** before writing a trace.)</span></span>

## <a name="how-web-api-tracing-works"></a><span data-ttu-id="99f8b-171">Как веб-API, трассировка Works</span><span class="sxs-lookup"><span data-stu-id="99f8b-171">How Web API Tracing Works</span></span>

<span data-ttu-id="99f8b-172">Трассировка в веб-API использует *фасадной* шаблон: Если трассировка включена, веб-API создает оболочку различные части конвейера запросов с классами, которые выполняют вызовы трассировки.</span><span class="sxs-lookup"><span data-stu-id="99f8b-172">Tracing in Web API uses a *facade* pattern: When tracing is enabled, Web API wraps various parts of the request pipeline with classes that perform trace calls.</span></span>

<span data-ttu-id="99f8b-173">Например, при выборе контроллера, этот конвейер использует **IHttpControllerSelector** интерфейс.</span><span class="sxs-lookup"><span data-stu-id="99f8b-173">For example, when selecting a controller, the pipeline uses the **IHttpControllerSelector** interface.</span></span> <span data-ttu-id="99f8b-174">С включенной трассировкой, pipleline вставляет класс, реализующий **IHttpControllerSelector** , но вызовы через настоящей реализации:</span><span class="sxs-lookup"><span data-stu-id="99f8b-174">With tracing enabled, the pipleline inserts a class that implements **IHttpControllerSelector** but calls through to the real implementation:</span></span>

![Трассировка веб-API использует шаблон фасадной.](tracing-in-aspnet-web-api/_static/image8.png)

<span data-ttu-id="99f8b-176">Такая схема следующие преимущества:</span><span class="sxs-lookup"><span data-stu-id="99f8b-176">The benefits of this design include:</span></span>

- <span data-ttu-id="99f8b-177">Если вы не добавите модуль записи трассировки, компоненты трассировки не создаются и не влияют на производительность.</span><span class="sxs-lookup"><span data-stu-id="99f8b-177">If you do not add a trace writer, the tracing components are not instantiated and have no performance impact.</span></span>
- <span data-ttu-id="99f8b-178">Если заменить службы по умолчанию, такие как **IHttpControllerSelector** с собственную реализацию, трассировка не влияет, так как трассировка выполняется путем объект-оболочку.</span><span class="sxs-lookup"><span data-stu-id="99f8b-178">If you replace default services such as **IHttpControllerSelector** with your own custom implementation, tracing is not affected, because tracing is done by the wrapper object.</span></span>

<span data-ttu-id="99f8b-179">Также можно заменить всю платформу .NET framework трассировки веб-API с помощью собственных пользовательских framework, заменив значение по умолчанию **ITraceManager** службы:</span><span class="sxs-lookup"><span data-stu-id="99f8b-179">You can also replace the entire Web API trace framework with your own custom framework, by replacing the default **ITraceManager** service:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="99f8b-180">Реализуйте **ITraceManager.Initialize** для инициализации ваша система трассировки.</span><span class="sxs-lookup"><span data-stu-id="99f8b-180">Implement **ITraceManager.Initialize** to initialize your tracing system.</span></span> <span data-ttu-id="99f8b-181">Имейте в виду, что этот параметр заменяет *всей* framework трассировки, включая весь код трассировки, встроенный в веб-API.</span><span class="sxs-lookup"><span data-stu-id="99f8b-181">Be aware that this replaces the *entire* trace framework, including all of the tracing code that is built into Web API.</span></span>
