---
title: Размещение ASP.NET Core в службе приложений Azure
author: guardrex
description: Узнайте, как размещать приложения ASP.NET Core в службе приложений Azure, с помощью ссылок на полезные ресурсы.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: c2675f73880a41ee75f6ec13155419945387e109
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
# <a name="host-aspnet-core-on-azure-app-service"></a><span data-ttu-id="fb859-103">Размещение ASP.NET Core в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="fb859-103">Host ASP.NET Core on Azure App Service</span></span>

<span data-ttu-id="fb859-104">[Служба приложений Azure](https://azure.microsoft.com/services/app-service/) — это [платформа облачных вычислений Microsoft](https://azure.microsoft.com/), предназначенная для размещения веб-приложений, включая ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fb859-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

## <a name="useful-resources"></a><span data-ttu-id="fb859-105">Полезные ресурсы</span><span class="sxs-lookup"><span data-stu-id="fb859-105">Useful resources</span></span>

<span data-ttu-id="fb859-106">[Документация по веб-приложениям](/azure/app-service/) Azure — это место, где хранятся документация, учебники, примеры, руководства и другие ресурсы, связанные с приложениями Azure.</span><span class="sxs-lookup"><span data-stu-id="fb859-106">The Azure [Web Apps Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="fb859-107">Размещению приложений ASP.NET Core посвящены следующие два руководства.</span><span class="sxs-lookup"><span data-stu-id="fb859-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="fb859-108">Краткое руководство. Создание веб-приложения ASP.NET Core в Azure</span><span class="sxs-lookup"><span data-stu-id="fb859-108">Quickstart: Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="fb859-109">Создайте веб-приложение ASP.NET Core и разверните его в службе приложений Azure на базе Windows с помощью Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fb859-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="fb859-110">Краткое руководство. Создание веб-приложения .NET Core в службе приложений на базе Linux</span><span class="sxs-lookup"><span data-stu-id="fb859-110">Quickstart: Create a .NET Core web app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="fb859-111">Создайте веб-приложение ASP.NET Core и разверните его в службе приложений Azure на базе Linux с помощью командной строки.</span><span class="sxs-lookup"><span data-stu-id="fb859-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="fb859-112">Следующие статьи входят в документацию по ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fb859-112">The following articles are available in ASP.NET Core documentation:</span></span>

[<span data-ttu-id="fb859-113">Опубликовать в Azure с помощью Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fb859-113">Publish to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)  
<span data-ttu-id="fb859-114">Сведения о публикации приложения ASP.NET Core в службе приложений Azure с помощью Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fb859-114">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

[<span data-ttu-id="fb859-115">Публикация в Azure с инструментами командной строки</span><span class="sxs-lookup"><span data-stu-id="fb859-115">Publish to Azure with CLI tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli)  
<span data-ttu-id="fb859-116">Сведения о публикации приложения ASP.NET Core в службе приложений Azure с помощью клиента командной строки Git.</span><span class="sxs-lookup"><span data-stu-id="fb859-116">Learn how to publish an ASP.NET Core app to Azure App Service using the Git command-line client.</span></span>

[<span data-ttu-id="fb859-117">Непрерывное развертывание в Azure с помощью Visual Studio и Git</span><span class="sxs-lookup"><span data-stu-id="fb859-117">Continuous deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
<span data-ttu-id="fb859-118">Сведения о создании веб-приложения ASP.NET Core с помощью Visual Studio и его развертывании в службе приложений Azure с использованием Git для непрерывного развертывания.</span><span class="sxs-lookup"><span data-stu-id="fb859-118">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="fb859-119">Непрерывное развертывание в Azure с помощью VSTS</span><span class="sxs-lookup"><span data-stu-id="fb859-119">Continuous deployment to Azure with VSTS</span></span>](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)  
<span data-ttu-id="fb859-120">Сведения о настройке сборки CI для приложения ASP.NET Core и последующем создании выпуска непрерывного развертывания в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="fb859-120">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

[<span data-ttu-id="fb859-121">Песочница веб-приложений Azure</span><span class="sxs-lookup"><span data-stu-id="fb859-121">Azure Web App sandbox</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
<span data-ttu-id="fb859-122">Сведения об ограничениях среды выполнения службы приложений Azure, создаваемых платформой приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="fb859-122">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="fb859-123">Настройка приложения</span><span class="sxs-lookup"><span data-stu-id="fb859-123">Application configuration</span></span>

<span data-ttu-id="fb859-124">При использовании ASP.NET Core 2.0 и более поздних версий три пакета в составе [метапакета Microsoft.AspNetCore.All](xref:fundamentals/metapackage) предоставляют функции автоматического ведения журналов для приложений, развернутых в службе приложений Azure:</span><span class="sxs-lookup"><span data-stu-id="fb859-124">With ASP.NET Core 2.0 and later, three packages in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="fb859-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) использует [IHostingStartup](xref:host-and-deploy/platform-specific-configuration) для интеграции подсветки ASP.NET Core в службу приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="fb859-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:host-and-deploy/platform-specific-configuration) to provide ASP.NET Core lightup integration with Azure App Service.</span></span> <span data-ttu-id="fb859-126">Пакет `Microsoft.AspNetCore.AzureAppServicesIntegration` включает дополнительные функции для ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="fb859-126">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="fb859-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) выполняет [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) для добавления поставщиков ведения журналов диагностики из службы приложений Azure в пакет `Microsoft.Extensions.Logging.AzureAppServices`.</span><span class="sxs-lookup"><span data-stu-id="fb859-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="fb859-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) предоставляет реализации средства ведения журналов в поддержку журналов диагностики службы приложений Azure и функций потоковой передачи журналов.</span><span class="sxs-lookup"><span data-stu-id="fb859-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="fb859-129">Сценарии использования прокси-сервера и подсистемы балансировки нагрузки</span><span class="sxs-lookup"><span data-stu-id="fb859-129">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="fb859-130">ПО промежуточного слоя для интеграции IIS, которое настраивает ПО промежуточного слоя переадресации заголовков, и модуль ASP.NET Core настраиваются на пересылку схемы (HTTP/HTTPS) и удаленного IP-адреса расположения, где был сформирован запрос.</span><span class="sxs-lookup"><span data-stu-id="fb859-130">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="fb859-131">Для приложений, размещенных за дополнительными прокси-серверами и подсистемами балансировки нагрузки, может потребоваться дополнительная настройка.</span><span class="sxs-lookup"><span data-stu-id="fb859-131">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="fb859-132">Дополнительные сведения см. в разделе [Настройка ASP.NET Core для работы с прокси-серверами и подсистемами балансировки нагрузки](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="fb859-132">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="fb859-133">Мониторинг и ведение журналов</span><span class="sxs-lookup"><span data-stu-id="fb859-133">Monitoring and logging</span></span>

<span data-ttu-id="fb859-134">Сведения о мониторинге, ведении журналов, а также поиске и устранении неполадок см. в следующих статьях.</span><span class="sxs-lookup"><span data-stu-id="fb859-134">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="fb859-135">Мониторинг приложений в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="fb859-135">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="fb859-136">Сведения о том, как толковать квоты и параметры для приложений и планы службы приложений.</span><span class="sxs-lookup"><span data-stu-id="fb859-136">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="fb859-137">Включение функции ведения журналов диагностики для веб-приложений в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="fb859-137">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="fb859-138">Сведения о том, как включать и где искать функцию ведения журнала диагностики для кодов статуса HTTP, невыполненных запросов и активности веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="fb859-138">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

[<span data-ttu-id="fb859-139">Общие сведения об обработке ошибок в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fb859-139">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)  
<span data-ttu-id="fb859-140">Сведения об основных методах обработки ошибок в приложениях ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fb859-140">Understand common appoaches to handling errors in ASP.NET Core apps.</span></span>

[<span data-ttu-id="fb859-141">Устранение неполадок ASP.NET Core в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="fb859-141">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)  
<span data-ttu-id="fb859-142">Сведения о диагностике проблем с развертыванием приложений ASP.NET Core в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="fb859-142">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

[<span data-ttu-id="fb859-143">Справочник по общим ошибкам для службы приложений Azure и служб IIS с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fb859-143">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)  
<span data-ttu-id="fb859-144">См. распространенные ошибки с конфигурацией развертывания для приложений, размещенных в службе приложений Azure/IIS, и рекомендации по их устранению.</span><span class="sxs-lookup"><span data-stu-id="fb859-144">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="fb859-145">Связка ключей для защиты данных и слоты развертывания</span><span class="sxs-lookup"><span data-stu-id="fb859-145">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="fb859-146">[Ключи для защиты данных](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) хранятся в папке *%HOME%\ASP.NET\DataProtection-Keys*.</span><span class="sxs-lookup"><span data-stu-id="fb859-146">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="fb859-147">Эта папка копируется в сетевое хранилище и синхронизируется на всех машинах, где размещается приложение.</span><span class="sxs-lookup"><span data-stu-id="fb859-147">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="fb859-148">Во время хранения ключи не защищаются.</span><span class="sxs-lookup"><span data-stu-id="fb859-148">Keys aren't protected at rest.</span></span> <span data-ttu-id="fb859-149">В этой папке хранится связка ключей для всех экземпляров приложения в одном и том же слоте развертывания.</span><span class="sxs-lookup"><span data-stu-id="fb859-149">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="fb859-150">Отдельные слоты развертывания, такие как промежуточное хранение и производство, не используют общую связку ключей.</span><span class="sxs-lookup"><span data-stu-id="fb859-150">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="fb859-151">При переключении между разными слотами развертывания ни одна система с использованием защиты данных не сможет расшифровать хранимые данные, используя связку ключей из предыдущего слота.</span><span class="sxs-lookup"><span data-stu-id="fb859-151">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="fb859-152">Для защиты файлов cookie ПО промежуточного слоя ASP.NET Cookie использует защиту данных.</span><span class="sxs-lookup"><span data-stu-id="fb859-152">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="fb859-153">В результате пользователей, применяющих ПО промежуточного слоя ASP.NET Cookie, выбрасывает из приложения.</span><span class="sxs-lookup"><span data-stu-id="fb859-153">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="fb859-154">Для того чтобы решение связки ключей не зависело от слота, используйте внешнего поставщика связки ключей, например:</span><span class="sxs-lookup"><span data-stu-id="fb859-154">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="fb859-155">Хранилище больших двоичных объектов Azure;</span><span class="sxs-lookup"><span data-stu-id="fb859-155">Azure Blob Storage</span></span>
* <span data-ttu-id="fb859-156">Хранилище ключей Azure;</span><span class="sxs-lookup"><span data-stu-id="fb859-156">Azure Key Vault</span></span>
* <span data-ttu-id="fb859-157">Хранилище SQL;</span><span class="sxs-lookup"><span data-stu-id="fb859-157">SQL store</span></span>
* <span data-ttu-id="fb859-158">Кэш Redis.</span><span class="sxs-lookup"><span data-stu-id="fb859-158">Redis cache</span></span>

<span data-ttu-id="fb859-159">Дополнительные сведения см. в разделе [Поставщики хранилища ключей](xref:security/data-protection/implementation/key-storage-providers).</span><span class="sxs-lookup"><span data-stu-id="fb859-159">For more information, see [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a><span data-ttu-id="fb859-160">Развертывание предварительной версии ASP.NET Core в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="fb859-160">Deploy ASP.NET Core preview release to Azure App Service</span></span>

<span data-ttu-id="fb859-161">Предварительную версию приложения ASP.NET Core можно развернуть в службе приложений Azure, используя следующие методы.</span><span class="sxs-lookup"><span data-stu-id="fb859-161">ASP.NET Core preview apps can be deployed to Azure App Service with the following approaches:</span></span>

* [<span data-ttu-id="fb859-162">Установка расширения сайта предварительной версии</span><span class="sxs-lookup"><span data-stu-id="fb859-162">Install the preview site extention</span></span>](#site-x)
* [<span data-ttu-id="fb859-163">Автономное развертывание приложения</span><span class="sxs-lookup"><span data-stu-id="fb859-163">Deploy the app self contained</span></span>](#self)
* [<span data-ttu-id="fb859-164">Использование Docker с веб-приложениями для контейнеров</span><span class="sxs-lookup"><span data-stu-id="fb859-164">Use Docker with Web Apps for containers</span></span>](#docker)

<span data-ttu-id="fb859-165">Если у вас возникли проблемы при использовании расширения сайта предварительной версии, сообщите о них на [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span><span class="sxs-lookup"><span data-stu-id="fb859-165">If you have a problem using the preview site extension, open an issue on [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span></span>

<a name="site-x"></a>
### <a name="install-the-preview-site-extention"></a><span data-ttu-id="fb859-166">Установка расширения сайта предварительной версии</span><span class="sxs-lookup"><span data-stu-id="fb859-166">Install the preview site extention</span></span>

* <span data-ttu-id="fb859-167">На портале Azure перейдите к колонке "Служба приложений".</span><span class="sxs-lookup"><span data-stu-id="fb859-167">From the Azure portal, navigate to the App Service blade.</span></span>
* <span data-ttu-id="fb859-168">Введите "ex" в поле поиска.</span><span class="sxs-lookup"><span data-stu-id="fb859-168">Enter "ex" in the search box.</span></span>
* <span data-ttu-id="fb859-169">Выберите **Расширения**.</span><span class="sxs-lookup"><span data-stu-id="fb859-169">Select **Extensions**.</span></span>
* <span data-ttu-id="fb859-170">Выберите "Добавить".</span><span class="sxs-lookup"><span data-stu-id="fb859-170">Select "Add".</span></span>

![Колонка "Приложение Azure" с предыдущими шагами](index/_static/x1.png)

* <span data-ttu-id="fb859-172">Выберите **Расширения среды выполнения ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="fb859-172">Select **ASP.NET Core Runtime Extensions**.</span></span>
* <span data-ttu-id="fb859-173">Нажмите **ОК** > **ОК**.</span><span class="sxs-lookup"><span data-stu-id="fb859-173">Select **OK** > **OK**.</span></span>

<span data-ttu-id="fb859-174">Когда операции добавления будут выполнены, будет установлена последняя предварительная версия .NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="fb859-174">When the add operations completes, the latest .NET Core 2.1 preview is installed.</span></span> <span data-ttu-id="fb859-175">Чтобы проверить установку, запустите `dotnet --info` в консоли.</span><span class="sxs-lookup"><span data-stu-id="fb859-175">You can verify the installation by running `dotnet --info` in the console.</span></span> <span data-ttu-id="fb859-176">В колонке "Служба приложений":</span><span class="sxs-lookup"><span data-stu-id="fb859-176">From the App Service blade:</span></span>

* <span data-ttu-id="fb859-177">Введите "con" в поле поиска.</span><span class="sxs-lookup"><span data-stu-id="fb859-177">Enter "con" in the search box.</span></span>
* <span data-ttu-id="fb859-178">Выберите **Консоль**.</span><span class="sxs-lookup"><span data-stu-id="fb859-178">Select **Console**.</span></span>
* <span data-ttu-id="fb859-179">Введите `dotnet --info` в консоли.</span><span class="sxs-lookup"><span data-stu-id="fb859-179">Enter `dotnet --info` in the console.</span></span>

![Колонка "Приложение Azure" с предыдущими шагами](index/_static/cons.png)

<span data-ttu-id="fb859-181">Это изображение было актуальным на момент написания статьи.</span><span class="sxs-lookup"><span data-stu-id="fb859-181">The preceding image was current at the time this was written.</span></span> <span data-ttu-id="fb859-182">Вы можете увидеть другую версию.</span><span class="sxs-lookup"><span data-stu-id="fb859-182">You may see a different version.</span></span>

<span data-ttu-id="fb859-183">`dotnet --info` отображает путь к расширению сайта, где установлена предварительная версия.</span><span class="sxs-lookup"><span data-stu-id="fb859-183">The `dotnet --info` displays the the path to the site extension where the Preview has been installed.</span></span> <span data-ttu-id="fb859-184">Он показывает, что приложение выполняется из расширения сайта, а не из расположения *ProgramFiles* по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="fb859-184">It shows the app is running from the site extension instead of from the default *ProgramFiles* location.</span></span> <span data-ttu-id="fb859-185">Если вы видите *ProgramFiles*, перезапустите сайт и выполните `dotnet --info`.</span><span class="sxs-lookup"><span data-stu-id="fb859-185">If you see *ProgramFiles*, restart the site and run `dotnet --info`.</span></span>

#### <a name="use-the-preview-site-extention-with-an-arm-template"></a><span data-ttu-id="fb859-186">Использование расширения сайта предварительного просмотра с шаблоном ARM</span><span class="sxs-lookup"><span data-stu-id="fb859-186">Use the preview site extention with an ARM template</span></span>

<span data-ttu-id="fb859-187">Если вы используете шаблон ARM для создания и развертывания приложений, вы можете использовать тип ресурса `siteextensions`, чтобы добавить расширение сайта в веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="fb859-187">If you are using an ARM template to create and deploy applications you can use the `siteextensions` resource type to add the site extension to a Web App.</span></span> <span data-ttu-id="fb859-188">Пример:</span><span class="sxs-lookup"><span data-stu-id="fb859-188">For example:</span></span>

[!code-json[Main](index/sample/arm.json?highlight=2)]

<a name="self"></a>
### <a name="deploy-the-app-self-contained"></a><span data-ttu-id="fb859-189">Автономное развертывание приложения</span><span class="sxs-lookup"><span data-stu-id="fb859-189">Deploy the app self contained</span></span>

<span data-ttu-id="fb859-190">Вы можете развернуть [автономное приложение](/dotnet/core/deploying/#self-contained-deployments-scd), которое при развертывании содержит в себе среду выполнения предварительной версии.</span><span class="sxs-lookup"><span data-stu-id="fb859-190">You can deploy a [self-contained app](/dotnet/core/deploying/#self-contained-deployments-scd) that carries the preview runtime with it when being deployed.</span></span> <span data-ttu-id="fb859-191">При развертывании автономного приложения.</span><span class="sxs-lookup"><span data-stu-id="fb859-191">When deploying a self contained app:</span></span>

* <span data-ttu-id="fb859-192">Не нужно подготавливать сайт.</span><span class="sxs-lookup"><span data-stu-id="fb859-192">You don’t need to prepare your site.</span></span>
* <span data-ttu-id="fb859-193">Необходимо опубликовать приложение не так, как при развертывании приложения после установки пакета SDK на сервере.</span><span class="sxs-lookup"><span data-stu-id="fb859-193">Requires you to publish your application differently than you would when deploying an app once the SDK is installed on the server.</span></span>

<span data-ttu-id="fb859-194">Автономные приложения — это вариант для всех приложений .NET Core.</span><span class="sxs-lookup"><span data-stu-id="fb859-194">Self-contained apps are an option for all .NET Core applications.</span></span>

<a name="docker"></a>
### <a name="use-docker-with-web-apps-for-containers"></a><span data-ttu-id="fb859-195">Использование Docker с веб-приложениями для контейнеров</span><span class="sxs-lookup"><span data-stu-id="fb859-195">Use Docker with Web Apps for containers</span></span>

<span data-ttu-id="fb859-196">[Центр Docker](https://hub.docker.com/r/microsoft/aspnetcore/) содержит образы Docker из последней предварительной версии 2.1.</span><span class="sxs-lookup"><span data-stu-id="fb859-196">The [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contains the latest 2.1 preview Docker images.</span></span> <span data-ttu-id="fb859-197">Можно использовать их в качестве базового образа и развертывать в веб-приложения для контейнеров, как обычно.</span><span class="sxs-lookup"><span data-stu-id="fb859-197">You can use them as your base image and deploy to Web Apps for Containers as you normally would.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fb859-198">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="fb859-198">Additional resources</span></span>

* [<span data-ttu-id="fb859-199">Общие сведения о веб-приложениях (5-минутное видео)</span><span class="sxs-lookup"><span data-stu-id="fb859-199">Web Apps overview (5-minute overview video)</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="fb859-200">Служба приложений Azure: оптимальное место для размещения приложений .NET (55-минутное видео)</span><span class="sxs-lookup"><span data-stu-id="fb859-200">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="fb859-201">Azure, пятница: практики диагностики и устранения неполадок в службе приложений Azure (12-минутное видео)</span><span class="sxs-lookup"><span data-stu-id="fb859-201">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="fb859-202">Общие сведения о диагностике в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="fb859-202">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)

<span data-ttu-id="fb859-203">Служба приложений Azure на Windows Server использует [службы IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="fb859-203">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="fb859-204">Технологии IIS посвящены следующие статьи.</span><span class="sxs-lookup"><span data-stu-id="fb859-204">The following topics pertain to the underlying IIS technology:</span></span>

* [<span data-ttu-id="fb859-205">Размещение ASP.NET Core в Windows со службами IIS</span><span class="sxs-lookup"><span data-stu-id="fb859-205">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="fb859-206">Введение в модуль ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fb859-206">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="fb859-207">Справочник по конфигурации модуля ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fb859-207">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="fb859-208">Модули IIS с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fb859-208">IIS Modules with ASP.NET Core</span></span>](xref:host-and-deploy/iis/modules)
* [<span data-ttu-id="fb859-209">Библиотека Microsoft TechNet: Windows Server</span><span class="sxs-lookup"><span data-stu-id="fb859-209">Microsoft TechNet Library: Windows Server</span></span>](/windows-server/windows-server-versions)
