---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Опубликуйте приложение в Azure службы приложений Azure | Документы Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: cc8a9199144e9fac041435938ea8899374ea199f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30867818"
---
<a name="publish-the-app-to-azure-azure-app-service"></a><span data-ttu-id="ec381-102">Опубликуйте приложение в Azure службы приложений Azure</span><span class="sxs-lookup"><span data-stu-id="ec381-102">Publish the App to Azure Azure App Service</span></span>
====================
<span data-ttu-id="ec381-103">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ec381-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="ec381-104">Загрузка завершенного проекта</span><span class="sxs-lookup"><span data-stu-id="ec381-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="ec381-105">На последнем этапе будет публиковать приложение в Azure.</span><span class="sxs-lookup"><span data-stu-id="ec381-105">As the last step, you will publish the application to Azure.</span></span> <span data-ttu-id="ec381-106">В обозревателе решений щелкните правой кнопкой мыши проект и выберите **публикации**.</span><span class="sxs-lookup"><span data-stu-id="ec381-106">In Solution Explorer, right-click the project and select **Publish**.</span></span>

![](part-10/_static/image1.png)

<span data-ttu-id="ec381-107">Щелкнув **публикации** вызывает **веб-публикация** диалогового окна.</span><span class="sxs-lookup"><span data-stu-id="ec381-107">Clicking **Publish** invokes the **Publish Web** dialog.</span></span> <span data-ttu-id="ec381-108">Если вы установили флажок **узлов в облаке** при первом создании проекта, а затем соединение и параметры уже настроены.</span><span class="sxs-lookup"><span data-stu-id="ec381-108">If you checked **Host in Cloud** when you first created the project, then the connection and settings are already configured.</span></span> <span data-ttu-id="ec381-109">В этом случае просто щелкните **параметры** вкладку и проверьте &quot;выполнять миграции Code First&quot;.</span><span class="sxs-lookup"><span data-stu-id="ec381-109">In that case, just click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="ec381-110">(Если вы не выполняется проверка **узлов в облаке** в начале, выполните действия в [разделу](#new-website).)</span><span class="sxs-lookup"><span data-stu-id="ec381-110">(If you didn't check **Host in Cloud** at the beginning, then follow the steps in the [next section](#new-website).)</span></span>

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

<span data-ttu-id="ec381-111">Чтобы развернуть приложение, нажмите кнопку **публикации**.</span><span class="sxs-lookup"><span data-stu-id="ec381-111">To deploy the app, click **Publish**.</span></span> <span data-ttu-id="ec381-112">Можно просмотреть ход выполнения публикации в **действия публикации Web** окна.</span><span class="sxs-lookup"><span data-stu-id="ec381-112">You can view the publishing progress in the **Web Publish Activity** window.</span></span> <span data-ttu-id="ec381-113">(От **представление** последовательно выберите пункты **другие окна**, а затем выберите **действия публикации Web**.)</span><span class="sxs-lookup"><span data-stu-id="ec381-113">(From the **View** menu, select **Other Windows**, then select **Web Publish Activity**.)</span></span>

![](part-10/_static/image4.png)

<span data-ttu-id="ec381-114">По завершении развертывание приложения Visual Studio автоматически откроется браузер по умолчанию URL-адрес веб-сайта, развернутые и созданное приложение теперь работает в облаке.</span><span class="sxs-lookup"><span data-stu-id="ec381-114">When Visual Studio finishes deploying the app, the default browser automatically opens to the URL of the deployed website, and the application that you created is now running in the cloud.</span></span> <span data-ttu-id="ec381-115">URL-адрес в адресной строке браузера показывает, что сайт загружается из Интернета.</span><span class="sxs-lookup"><span data-stu-id="ec381-115">The URL in the browser address bar shows that the site is being loaded from the Internet.</span></span>

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a><span data-ttu-id="ec381-116">Развертывание для нового веб-сайта</span><span class="sxs-lookup"><span data-stu-id="ec381-116">Deploying to a New Website</span></span>

<span data-ttu-id="ec381-117">Если не были проверены **узлов в облаке** при создании проекта новое веб-приложение можно настроить сейчас.</span><span class="sxs-lookup"><span data-stu-id="ec381-117">If you did not check **Host in Cloud** when you first created the project, you can configure a new web app now.</span></span> <span data-ttu-id="ec381-118">В обозревателе решений щелкните правой кнопкой мыши проект и выберите **публикации**.</span><span class="sxs-lookup"><span data-stu-id="ec381-118">In Solution Explorer, right-click the project and select **Publish**.</span></span> <span data-ttu-id="ec381-119">Выберите **профиль** и нажмите кнопку **веб-сайтов Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="ec381-119">Select the **Profile** tab and click **Microsoft Azure Websites**.</span></span> <span data-ttu-id="ec381-120">Если вы не вошли в настоящее время на Azure, будет предложено войти.</span><span class="sxs-lookup"><span data-stu-id="ec381-120">If you aren't currently signed in to Azure, you will be prompted to sign in.</span></span>

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

<span data-ttu-id="ec381-121">В **существующих веб-сайтов** диалоговое окно, нажмите кнопку **New**.</span><span class="sxs-lookup"><span data-stu-id="ec381-121">In the **Existing Websites** dialog, click **New**.</span></span>

![](part-10/_static/image9.png)

<span data-ttu-id="ec381-122">Введите имя сайта.</span><span class="sxs-lookup"><span data-stu-id="ec381-122">Enter a site name.</span></span> <span data-ttu-id="ec381-123">Выберите подписки Azure и регион.</span><span class="sxs-lookup"><span data-stu-id="ec381-123">Select your Azure subscription and the region.</span></span> <span data-ttu-id="ec381-124">В разделе **сервера базы данных**выберите **создать новый сервер**, или выберите существующий сервер.</span><span class="sxs-lookup"><span data-stu-id="ec381-124">Under **Database server**, select **Create New Server**, or select an existing server.</span></span> <span data-ttu-id="ec381-125">Нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="ec381-125">Click **Create**.</span></span>

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

<span data-ttu-id="ec381-126">Нажмите кнопку **параметры** вкладку и проверьте &quot;выполнять миграции Code First&quot;.</span><span class="sxs-lookup"><span data-stu-id="ec381-126">Click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="ec381-127">Нажмите кнопку **публикации**.</span><span class="sxs-lookup"><span data-stu-id="ec381-127">Then click **Publish**.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ec381-128">Назад</span><span class="sxs-lookup"><span data-stu-id="ec381-128">Previous</span></span>](part-9.md)
