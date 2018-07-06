---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Публикация приложения в Azure службы приложений Azure | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: 66dde7b54ce084eed873afae56fd686d0dc8795f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808231"
---
<a name="publish-the-app-to-azure-azure-app-service"></a><span data-ttu-id="e181d-102">Публикация приложения в Azure службы приложений Azure</span><span class="sxs-lookup"><span data-stu-id="e181d-102">Publish the App to Azure Azure App Service</span></span>
====================
<span data-ttu-id="e181d-103">по [Майк Уоссон](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e181d-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="e181d-104">Скачать завершенный проект</span><span class="sxs-lookup"><span data-stu-id="e181d-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="e181d-105">На последнем этапе вы опубликуете приложение в Azure.</span><span class="sxs-lookup"><span data-stu-id="e181d-105">As the last step, you will publish the application to Azure.</span></span> <span data-ttu-id="e181d-106">В обозревателе решений щелкните правой кнопкой мыши проект и выберите **публикации**.</span><span class="sxs-lookup"><span data-stu-id="e181d-106">In Solution Explorer, right-click the project and select **Publish**.</span></span>

![](part-10/_static/image1.png)

<span data-ttu-id="e181d-107">Щелкнув **публикации** вызывает **веб-публикации** диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="e181d-107">Clicking **Publish** invokes the **Publish Web** dialog.</span></span> <span data-ttu-id="e181d-108">Если вы установили флажок **узел в облаке** при первом создании проекта, а затем соединение и параметры уже настроены.</span><span class="sxs-lookup"><span data-stu-id="e181d-108">If you checked **Host in Cloud** when you first created the project, then the connection and settings are already configured.</span></span> <span data-ttu-id="e181d-109">В этом случае просто щелкните **параметры** вкладку и проверьте &quot;выполнить Code First Migrations&quot;.</span><span class="sxs-lookup"><span data-stu-id="e181d-109">In that case, just click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="e181d-110">(Если вы не проверили **узел в облаке** в начале, выполните действия в [разделу](#new-website).)</span><span class="sxs-lookup"><span data-stu-id="e181d-110">(If you didn't check **Host in Cloud** at the beginning, then follow the steps in the [next section](#new-website).)</span></span>

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

<span data-ttu-id="e181d-111">Чтобы развернуть приложение, нажмите кнопку **публикации**.</span><span class="sxs-lookup"><span data-stu-id="e181d-111">To deploy the app, click **Publish**.</span></span> <span data-ttu-id="e181d-112">Можно просмотреть ход выполнения публикации в **действий веб-публикации** окна.</span><span class="sxs-lookup"><span data-stu-id="e181d-112">You can view the publishing progress in the **Web Publish Activity** window.</span></span> <span data-ttu-id="e181d-113">(Из **представление** меню, выберите **Other Windows**, а затем выберите **действий веб-публикации**.)</span><span class="sxs-lookup"><span data-stu-id="e181d-113">(From the **View** menu, select **Other Windows**, then select **Web Publish Activity**.)</span></span>

![](part-10/_static/image4.png)

<span data-ttu-id="e181d-114">Когда Visual Studio завершит развертывание приложения, автоматически откроется браузер по умолчанию URL-адрес развернутого веб-сайта и созданного приложения теперь работает в облаке.</span><span class="sxs-lookup"><span data-stu-id="e181d-114">When Visual Studio finishes deploying the app, the default browser automatically opens to the URL of the deployed website, and the application that you created is now running in the cloud.</span></span> <span data-ttu-id="e181d-115">URL-адрес в адресной строке браузера показывает, что сайт загружается из Интернета.</span><span class="sxs-lookup"><span data-stu-id="e181d-115">The URL in the browser address bar shows that the site is being loaded from the Internet.</span></span>

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a><span data-ttu-id="e181d-116">Развертывание новой веб-сайте</span><span class="sxs-lookup"><span data-stu-id="e181d-116">Deploying to a New Website</span></span>

<span data-ttu-id="e181d-117">Если вы не проверили **узел в облаке** при первоначальном создании проекта, можно настроить новое веб-приложение теперь.</span><span class="sxs-lookup"><span data-stu-id="e181d-117">If you did not check **Host in Cloud** when you first created the project, you can configure a new web app now.</span></span> <span data-ttu-id="e181d-118">В обозревателе решений щелкните правой кнопкой мыши проект и выберите **публикации**.</span><span class="sxs-lookup"><span data-stu-id="e181d-118">In Solution Explorer, right-click the project and select **Publish**.</span></span> <span data-ttu-id="e181d-119">Выберите **профиль** вкладку и нажмите кнопку **веб-сайтов Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="e181d-119">Select the **Profile** tab and click **Microsoft Azure Websites**.</span></span> <span data-ttu-id="e181d-120">Если вы не выполнили вход в настоящее время Azure, вам будет предложено войти.</span><span class="sxs-lookup"><span data-stu-id="e181d-120">If you aren't currently signed in to Azure, you will be prompted to sign in.</span></span>

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

<span data-ttu-id="e181d-121">В **существующие веб-сайты** диалоговое окно, нажмите кнопку **New**.</span><span class="sxs-lookup"><span data-stu-id="e181d-121">In the **Existing Websites** dialog, click **New**.</span></span>

![](part-10/_static/image9.png)

<span data-ttu-id="e181d-122">Введите имя сайта.</span><span class="sxs-lookup"><span data-stu-id="e181d-122">Enter a site name.</span></span> <span data-ttu-id="e181d-123">Выберите свою подписку Azure и регион.</span><span class="sxs-lookup"><span data-stu-id="e181d-123">Select your Azure subscription and the region.</span></span> <span data-ttu-id="e181d-124">В разделе **сервер базы данных**выберите **создать новый сервер**, или выберите существующий сервер.</span><span class="sxs-lookup"><span data-stu-id="e181d-124">Under **Database server**, select **Create New Server**, or select an existing server.</span></span> <span data-ttu-id="e181d-125">Нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="e181d-125">Click **Create**.</span></span>

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

<span data-ttu-id="e181d-126">Нажмите кнопку **параметры** вкладку и проверьте &quot;выполнить Code First Migrations&quot;.</span><span class="sxs-lookup"><span data-stu-id="e181d-126">Click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="e181d-127">Нажмите кнопку **публикации**.</span><span class="sxs-lookup"><span data-stu-id="e181d-127">Then click **Publish**.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e181d-128">Назад</span><span class="sxs-lookup"><span data-stu-id="e181d-128">Previous</span></span>](part-9.md)
