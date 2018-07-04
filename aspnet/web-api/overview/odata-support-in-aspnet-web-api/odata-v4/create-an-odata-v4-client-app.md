---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: Создание клиентского приложения OData v4 (C#) | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2014
ms.topic: article
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: 1ea6121db781c2d08bc8c76cd07be3c5a4f23f62
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377396"
---
<a name="create-an-odata-v4-client-app-c"></a><span data-ttu-id="3eee4-102">Создание клиентского приложения OData v4 (C#)</span><span class="sxs-lookup"><span data-stu-id="3eee4-102">Create an OData v4 Client App (C#)</span></span>
====================
<span data-ttu-id="3eee4-103">по [Майк Уоссон](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3eee4-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="3eee4-104">В предыдущем руководстве вы создали базовую службу OData, который поддерживает операции CRUD.</span><span class="sxs-lookup"><span data-stu-id="3eee4-104">In the previous tutorial, you created a basic OData service that supports CRUD operations.</span></span> <span data-ttu-id="3eee4-105">Теперь давайте создадим клиент для службы.</span><span class="sxs-lookup"><span data-stu-id="3eee4-105">Now let's create a client for the service.</span></span>

<span data-ttu-id="3eee4-106">Запустите новый экземпляр Visual Studio и создайте новый проект консольного приложения.</span><span class="sxs-lookup"><span data-stu-id="3eee4-106">Start a new instance of Visual Studio and create a new console application project.</span></span> <span data-ttu-id="3eee4-107">В **новый проект** диалоговом окне выберите **установленные** &gt; **шаблоны** &gt; **Visual C#** &gt; **Windows Desktop**и выберите **консольное приложение** шаблона.</span><span class="sxs-lookup"><span data-stu-id="3eee4-107">In the **New Project** dialog, select **Installed** &gt; **Templates** &gt; **Visual C#** &gt; **Windows Desktop**, and select the **Console Application** template.</span></span> <span data-ttu-id="3eee4-108">Назовите проект &quot;ProductsApp&quot;.</span><span class="sxs-lookup"><span data-stu-id="3eee4-108">Name the project &quot;ProductsApp&quot;.</span></span>

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="3eee4-109">Можно также добавить консольное приложение, в то же решение Visual Studio, которая содержит службу OData.</span><span class="sxs-lookup"><span data-stu-id="3eee4-109">You can also add the console app to the same Visual Studio solution that contains the OData service.</span></span>


## <a name="install-the-odata-client-code-generator"></a><span data-ttu-id="3eee4-110">Установите генератор кода клиент OData</span><span class="sxs-lookup"><span data-stu-id="3eee4-110">Install the OData Client Code Generator</span></span>

<span data-ttu-id="3eee4-111">Из **средства** меню, выберите **расширения и обновления**.</span><span class="sxs-lookup"><span data-stu-id="3eee4-111">From the **Tools** menu, select **Extensions and Updates**.</span></span> <span data-ttu-id="3eee4-112">Выберите **Online** &gt; **коллекции Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="3eee4-112">Select **Online** &gt; **Visual Studio Gallery**.</span></span> <span data-ttu-id="3eee4-113">В поле поиска найдите &quot;генератор кода клиента OData&quot;.</span><span class="sxs-lookup"><span data-stu-id="3eee4-113">In the search box, search for &quot;OData Client Code Generator&quot;.</span></span> <span data-ttu-id="3eee4-114">Нажмите кнопку **загрузить** установить VSIX.</span><span class="sxs-lookup"><span data-stu-id="3eee4-114">Click **Download** to install the VSIX.</span></span> <span data-ttu-id="3eee4-115">Может быть предложено перезапустить Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3eee4-115">You might be prompted to restart Visual Studio.</span></span>

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a><span data-ttu-id="3eee4-116">Запуск службы OData локально</span><span class="sxs-lookup"><span data-stu-id="3eee4-116">Run the OData Service Locally</span></span>

<span data-ttu-id="3eee4-117">Запустите проект ProductService из Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3eee4-117">Run the ProductService project from Visual Studio.</span></span> <span data-ttu-id="3eee4-118">По умолчанию Visual Studio запустит браузер к корню приложения.</span><span class="sxs-lookup"><span data-stu-id="3eee4-118">By default, Visual Studio launches a browser to the application root.</span></span> <span data-ttu-id="3eee4-119">Обратите внимание, универсальный код Ресурса; он потребуется на следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="3eee4-119">Note the URI; you will need this in the next step.</span></span> <span data-ttu-id="3eee4-120">Не закрывайте приложение.</span><span class="sxs-lookup"><span data-stu-id="3eee4-120">Leave the application running.</span></span>

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> <span data-ttu-id="3eee4-121">Если поместить обоих проектов в одном решении, убедитесь, что запустить ProductService проект без отладки.</span><span class="sxs-lookup"><span data-stu-id="3eee4-121">If you put both projects in the same solution, make sure to run the ProductService project without debugging.</span></span> <span data-ttu-id="3eee4-122">На следующем шаге необходимо сохранить во время внесения изменений в проект консольного приложения службы.</span><span class="sxs-lookup"><span data-stu-id="3eee4-122">In the next step, you will need to keep the service running while you modify the console application project.</span></span>


## <a name="generate-the-service-proxy"></a><span data-ttu-id="3eee4-123">Создание прокси-службы</span><span class="sxs-lookup"><span data-stu-id="3eee4-123">Generate the Service Proxy</span></span>

<span data-ttu-id="3eee4-124">Прокси-службы представляет собой класс .NET, который определяет методы для доступа к службе OData.</span><span class="sxs-lookup"><span data-stu-id="3eee4-124">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="3eee4-125">Прокси-сервер преобразует вызовы метода в HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="3eee4-125">The proxy translates method calls into HTTP requests.</span></span> <span data-ttu-id="3eee4-126">Будет создан класс прокси-сервера, выполнив [шаблон T4](https://msdn.microsoft.com/library/bb126445.aspx).</span><span class="sxs-lookup"><span data-stu-id="3eee4-126">You will create the proxy class by running a [T4 template](https://msdn.microsoft.com/library/bb126445.aspx).</span></span>

<span data-ttu-id="3eee4-127">Щелкните правой кнопкой мыши проект.</span><span class="sxs-lookup"><span data-stu-id="3eee4-127">Right-click the project.</span></span> <span data-ttu-id="3eee4-128">Выберите **добавить** &gt; **новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="3eee4-128">Select **Add** &gt; **New Item**.</span></span>

![](create-an-odata-v4-client-app/_static/image5.png)

<span data-ttu-id="3eee4-129">В **Добавление нового элемента** диалоговом окне выберите **элементы Visual C#** &gt; **кода** &gt; **клиент OData**.</span><span class="sxs-lookup"><span data-stu-id="3eee4-129">In the **Add New Item** dialog, select **Visual C# Items** &gt; **Code** &gt; **OData Client**.</span></span> <span data-ttu-id="3eee4-130">Назовите шаблон &quot;ProductClient.tt&quot;.</span><span class="sxs-lookup"><span data-stu-id="3eee4-130">Name the template &quot;ProductClient.tt&quot;.</span></span> <span data-ttu-id="3eee4-131">Нажмите кнопку **добавить** и просмотрите предупреждение безопасности.</span><span class="sxs-lookup"><span data-stu-id="3eee4-131">Click **Add** and click through the security warning.</span></span>

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

<span data-ttu-id="3eee4-132">На этом этапе вы получите ошибку, которое можно игнорировать.</span><span class="sxs-lookup"><span data-stu-id="3eee4-132">At this point, you'll get an error, which you can ignore.</span></span> <span data-ttu-id="3eee4-133">Visual Studio автоматически выполняет шаблон, но некоторые параметры конфигурации, нужно шаблон первой.</span><span class="sxs-lookup"><span data-stu-id="3eee4-133">Visual Studio automatically runs the template, but the template needs some configuration settings first.</span></span>

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

<span data-ttu-id="3eee4-134">Откройте файл ProductClient.odata.config. В `Parameter` элемент, вставьте URI из проекта ProductService (предыдущий шаг).</span><span class="sxs-lookup"><span data-stu-id="3eee4-134">Open the file ProductClient.odata.config. In the `Parameter` element, paste in the URI from the ProductService project (previous step).</span></span> <span data-ttu-id="3eee4-135">Пример:</span><span class="sxs-lookup"><span data-stu-id="3eee4-135">For example:</span></span>

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

<span data-ttu-id="3eee4-136">Еще раз запустите шаблон.</span><span class="sxs-lookup"><span data-stu-id="3eee4-136">Run the template again.</span></span> <span data-ttu-id="3eee4-137">В обозревателе решений щелкните правой кнопкой мыши файл ProductClient.tt и выберите **пользовательское средство**.</span><span class="sxs-lookup"><span data-stu-id="3eee4-137">In Solution Explorer, right click the ProductClient.tt file and select **Run Custom Tool**.</span></span>

<span data-ttu-id="3eee4-138">Этот шаблон создает файл кода с именем ProductClient.cs, который определяет прокси-сервер.</span><span class="sxs-lookup"><span data-stu-id="3eee4-138">The template creates a code file named ProductClient.cs that defines the proxy.</span></span> <span data-ttu-id="3eee4-139">При разработке приложения, при изменении конечной точки OData, запустите шаблон еще раз, чтобы обновить прокси-сервер.</span><span class="sxs-lookup"><span data-stu-id="3eee4-139">As you develop your app, if you change the OData endpoint, run the template again to update the proxy.</span></span>

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a><span data-ttu-id="3eee4-140">Использовать прокси-службы для вызова службы OData</span><span class="sxs-lookup"><span data-stu-id="3eee4-140">Use the Service Proxy to Call the OData Service</span></span>

<span data-ttu-id="3eee4-141">Откройте файл Program.cs и замените код следующим.</span><span class="sxs-lookup"><span data-stu-id="3eee4-141">Open the file Program.cs and replace the boilerplate code with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

<span data-ttu-id="3eee4-142">Замените значение *serviceUri* с URI службы выше.</span><span class="sxs-lookup"><span data-stu-id="3eee4-142">Replace the value of *serviceUri* with the service URI from earlier.</span></span>

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

<span data-ttu-id="3eee4-143">При запуске приложения, должно отобразиться следующее:</span><span class="sxs-lookup"><span data-stu-id="3eee4-143">When you run the app, it should output the following:</span></span>

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
