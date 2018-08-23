---
title: Размещение ASP.NET Core в службе приложений Azure
author: guardrex
description: Узнайте, как размещать приложения ASP.NET Core в службе приложений Azure, с помощью ссылок на полезные ресурсы.
ms.author: riande
ms.custom: mvc
ms.date: 07/24/2018
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: 9a7d20378cac597b748d8a60eb0f0bf17c9ba082
ms.sourcegitcommit: d27317c16f113e7c111583042ec7e4c5a26adf6f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/20/2018
ms.locfileid: "41746065"
---
# <a name="host-aspnet-core-on-azure-app-service"></a><span data-ttu-id="118bc-103">Размещение ASP.NET Core в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="118bc-103">Host ASP.NET Core on Azure App Service</span></span>

<span data-ttu-id="118bc-104">[Служба приложений Azure](https://azure.microsoft.com/services/app-service/) — это [платформа облачных вычислений Microsoft](https://azure.microsoft.com/), предназначенная для размещения веб-приложений, включая ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="118bc-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

## <a name="useful-resources"></a><span data-ttu-id="118bc-105">Полезные ресурсы</span><span class="sxs-lookup"><span data-stu-id="118bc-105">Useful resources</span></span>

<span data-ttu-id="118bc-106">[Документация по веб-приложениям](/azure/app-service/) Azure — это место, где хранятся документация, учебники, примеры, руководства и другие ресурсы, связанные с приложениями Azure.</span><span class="sxs-lookup"><span data-stu-id="118bc-106">The Azure [Web Apps Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="118bc-107">Размещению приложений ASP.NET Core посвящены следующие два руководства.</span><span class="sxs-lookup"><span data-stu-id="118bc-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="118bc-108">Краткое руководство. Создание веб-приложения ASP.NET Core в Azure</span><span class="sxs-lookup"><span data-stu-id="118bc-108">Quickstart: Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="118bc-109">Создайте веб-приложение ASP.NET Core и разверните его в службе приложений Azure на базе Windows с помощью Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="118bc-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="118bc-110">Краткое руководство. Создание веб-приложения .NET Core в службе приложений на базе Linux</span><span class="sxs-lookup"><span data-stu-id="118bc-110">Quickstart: Create a .NET Core web app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="118bc-111">Создайте веб-приложение ASP.NET Core и разверните его в службе приложений Azure на базе Linux с помощью командной строки.</span><span class="sxs-lookup"><span data-stu-id="118bc-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="118bc-112">Следующие статьи входят в документацию по ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="118bc-112">The following articles are available in ASP.NET Core documentation:</span></span>

[<span data-ttu-id="118bc-113">Опубликовать в Azure с помощью Visual Studio</span><span class="sxs-lookup"><span data-stu-id="118bc-113">Publish to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)  
<span data-ttu-id="118bc-114">Сведения о публикации приложения ASP.NET Core в службе приложений Azure с помощью Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="118bc-114">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

[<span data-ttu-id="118bc-115">Публикация в Azure с инструментами командной строки</span><span class="sxs-lookup"><span data-stu-id="118bc-115">Publish to Azure with CLI tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli)  
<span data-ttu-id="118bc-116">Сведения о публикации приложения ASP.NET Core в службе приложений Azure с помощью клиента командной строки Git.</span><span class="sxs-lookup"><span data-stu-id="118bc-116">Learn how to publish an ASP.NET Core app to Azure App Service using the Git command-line client.</span></span>

[<span data-ttu-id="118bc-117">Непрерывное развертывание в Azure с помощью Visual Studio и Git</span><span class="sxs-lookup"><span data-stu-id="118bc-117">Continuous deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
<span data-ttu-id="118bc-118">Сведения о создании веб-приложения ASP.NET Core с помощью Visual Studio и его развертывании в службе приложений Azure с использованием Git для непрерывного развертывания.</span><span class="sxs-lookup"><span data-stu-id="118bc-118">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="118bc-119">Непрерывное развертывание в Azure с помощью VSTS</span><span class="sxs-lookup"><span data-stu-id="118bc-119">Continuous deployment to Azure with VSTS</span></span>](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)  
<span data-ttu-id="118bc-120">Сведения о настройке сборки CI для приложения ASP.NET Core и последующем создании выпуска непрерывного развертывания в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="118bc-120">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

[<span data-ttu-id="118bc-121">Песочница веб-приложений Azure</span><span class="sxs-lookup"><span data-stu-id="118bc-121">Azure Web App sandbox</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
<span data-ttu-id="118bc-122">Сведения об ограничениях среды выполнения службы приложений Azure, создаваемых платформой приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="118bc-122">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="application-configuration"></a><span data-ttu-id="118bc-123">Настройка приложения</span><span class="sxs-lookup"><span data-stu-id="118bc-123">Application configuration</span></span>

<span data-ttu-id="118bc-124">В ASP.NET Core 2.0 или более поздней версии следующие пакеты NuGet предоставляют возможности автоматического ведения журнала для приложений, развернутых в службе приложений Azure:</span><span class="sxs-lookup"><span data-stu-id="118bc-124">In ASP.NET Core 2.0 or later, the following NuGet packages provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="118bc-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) использует [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) для интеграции подсветки ASP.NET Core в службу приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="118bc-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) to provide ASP.NET Core light-up integration with Azure App Service.</span></span> <span data-ttu-id="118bc-126">Пакет `Microsoft.AspNetCore.AzureAppServicesIntegration` включает дополнительные функции для ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="118bc-126">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="118bc-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) выполняет [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) для добавления поставщиков ведения журналов диагностики из службы приложений Azure в пакет `Microsoft.Extensions.Logging.AzureAppServices`.</span><span class="sxs-lookup"><span data-stu-id="118bc-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="118bc-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) предоставляет реализации средства ведения журналов в поддержку журналов диагностики службы приложений Azure и функций потоковой передачи журналов.</span><span class="sxs-lookup"><span data-stu-id="118bc-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

<span data-ttu-id="118bc-129">Если планируется использовать .NET Core и указана ссылка на [метапакет Microsoft.AspNetCore.All](xref:fundamentals/metapackage), пакеты уже включены.</span><span class="sxs-lookup"><span data-stu-id="118bc-129">If targeting .NET Core and referencing the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), the packages are already included.</span></span> <span data-ttu-id="118bc-130">Пакеты отсутствуют в более новом [метапакете Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="118bc-130">The packages are absent from the newer [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="118bc-131">Если планируется использовать .NET Framework или указана ссылка на метапакет `Microsoft.AspNetCore.App`, ссылайтесь на отдельные пакеты ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="118bc-131">If targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, reference the individual logging packages.</span></span>

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="118bc-132">Сценарии использования прокси-сервера и подсистемы балансировки нагрузки</span><span class="sxs-lookup"><span data-stu-id="118bc-132">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="118bc-133">ПО промежуточного слоя для интеграции IIS, которое настраивает ПО промежуточного слоя переадресации заголовков, и модуль ASP.NET Core настраиваются на пересылку схемы (HTTP/HTTPS) и удаленного IP-адреса расположения, где был сформирован запрос.</span><span class="sxs-lookup"><span data-stu-id="118bc-133">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="118bc-134">Для приложений, размещенных за дополнительными прокси-серверами и подсистемами балансировки нагрузки, может потребоваться дополнительная настройка.</span><span class="sxs-lookup"><span data-stu-id="118bc-134">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="118bc-135">Дополнительные сведения см. в разделе [Настройка ASP.NET Core для работы с прокси-серверами и подсистемами балансировки нагрузки](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="118bc-135">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="118bc-136">Мониторинг и ведение журналов</span><span class="sxs-lookup"><span data-stu-id="118bc-136">Monitoring and logging</span></span>

<span data-ttu-id="118bc-137">Сведения о мониторинге, ведении журналов, а также поиске и устранении неполадок см. в следующих статьях.</span><span class="sxs-lookup"><span data-stu-id="118bc-137">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="118bc-138">Мониторинг приложений в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="118bc-138">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="118bc-139">Сведения о том, как толковать квоты и параметры для приложений и планы службы приложений.</span><span class="sxs-lookup"><span data-stu-id="118bc-139">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="118bc-140">Включение функции ведения журналов диагностики для веб-приложений в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="118bc-140">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="118bc-141">Сведения о том, как включать и где искать функцию ведения журнала диагностики для кодов статуса HTTP, невыполненных запросов и активности веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="118bc-141">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

[<span data-ttu-id="118bc-142">Общие сведения об обработке ошибок в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="118bc-142">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)  
<span data-ttu-id="118bc-143">Сведения об основных методах обработки ошибок в приложениях ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="118bc-143">Understand common approaches to handling errors in ASP.NET Core apps.</span></span>

[<span data-ttu-id="118bc-144">Устранение неполадок ASP.NET Core в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="118bc-144">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)  
<span data-ttu-id="118bc-145">Сведения о диагностике проблем с развертыванием приложений ASP.NET Core в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="118bc-145">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

[<span data-ttu-id="118bc-146">Справочник по общим ошибкам для службы приложений Azure и служб IIS с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="118bc-146">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)  
<span data-ttu-id="118bc-147">См. распространенные ошибки с конфигурацией развертывания для приложений, размещенных в службе приложений Azure/IIS, и рекомендации по их устранению.</span><span class="sxs-lookup"><span data-stu-id="118bc-147">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="118bc-148">Связка ключей для защиты данных и слоты развертывания</span><span class="sxs-lookup"><span data-stu-id="118bc-148">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="118bc-149">[Ключи для защиты данных](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) хранятся в папке *%HOME%\ASP.NET\DataProtection-Keys*.</span><span class="sxs-lookup"><span data-stu-id="118bc-149">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="118bc-150">Эта папка копируется в сетевое хранилище и синхронизируется на всех машинах, где размещается приложение.</span><span class="sxs-lookup"><span data-stu-id="118bc-150">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="118bc-151">Во время хранения ключи не защищаются.</span><span class="sxs-lookup"><span data-stu-id="118bc-151">Keys aren't protected at rest.</span></span> <span data-ttu-id="118bc-152">В этой папке хранится связка ключей для всех экземпляров приложения в одном и том же слоте развертывания.</span><span class="sxs-lookup"><span data-stu-id="118bc-152">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="118bc-153">Отдельные слоты развертывания, такие как промежуточное хранение и производство, не используют общую связку ключей.</span><span class="sxs-lookup"><span data-stu-id="118bc-153">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="118bc-154">При переключении между разными слотами развертывания ни одна система с использованием защиты данных не сможет расшифровать хранимые данные, используя связку ключей из предыдущего слота.</span><span class="sxs-lookup"><span data-stu-id="118bc-154">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="118bc-155">Для защиты файлов cookie ПО промежуточного слоя ASP.NET Cookie использует защиту данных.</span><span class="sxs-lookup"><span data-stu-id="118bc-155">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="118bc-156">В результате пользователей, применяющих ПО промежуточного слоя ASP.NET Cookie, выбрасывает из приложения.</span><span class="sxs-lookup"><span data-stu-id="118bc-156">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="118bc-157">Для того чтобы решение связки ключей не зависело от слота, используйте внешнего поставщика связки ключей, например:</span><span class="sxs-lookup"><span data-stu-id="118bc-157">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="118bc-158">Хранилище больших двоичных объектов Azure;</span><span class="sxs-lookup"><span data-stu-id="118bc-158">Azure Blob Storage</span></span>
* <span data-ttu-id="118bc-159">Хранилище ключей Azure;</span><span class="sxs-lookup"><span data-stu-id="118bc-159">Azure Key Vault</span></span>
* <span data-ttu-id="118bc-160">Хранилище SQL;</span><span class="sxs-lookup"><span data-stu-id="118bc-160">SQL store</span></span>
* <span data-ttu-id="118bc-161">Кэш Redis.</span><span class="sxs-lookup"><span data-stu-id="118bc-161">Redis cache</span></span>

<span data-ttu-id="118bc-162">Дополнительные сведения см. в разделе [Поставщики хранилища ключей](xref:security/data-protection/implementation/key-storage-providers).</span><span class="sxs-lookup"><span data-stu-id="118bc-162">For more information, see [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a><span data-ttu-id="118bc-163">Развертывание предварительной версии ASP.NET Core в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="118bc-163">Deploy ASP.NET Core preview release to Azure App Service</span></span>

<span data-ttu-id="118bc-164">Предварительную версию приложения ASP.NET Core можно развернуть в службе приложений Azure, используя следующие методы.</span><span class="sxs-lookup"><span data-stu-id="118bc-164">ASP.NET Core preview apps can be deployed to Azure App Service with the following approaches:</span></span>

* <span data-ttu-id="118bc-165">[Установка расширения сайта предварительной версии](#install-the-preview-site-extension)
<!-- * [Deploy the app self-contained](#deploy-the-app-self-contained) --></span><span class="sxs-lookup"><span data-stu-id="118bc-165">[Install the preview site extension](#install-the-preview-site-extension)
<!-- * [Deploy the app self-contained](#deploy-the-app-self-contained) --></span></span>
* [<span data-ttu-id="118bc-166">Использование Docker с веб-приложениями для контейнеров</span><span class="sxs-lookup"><span data-stu-id="118bc-166">Use Docker with Web Apps for containers</span></span>](#use-docker-with-web-apps-for-containers)

<span data-ttu-id="118bc-167">Если у вас возникли проблемы при использовании расширения сайта предварительной версии, сообщите о них на [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span><span class="sxs-lookup"><span data-stu-id="118bc-167">If a problem occurs using the preview site extension, open an issue on [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span></span>

### <a name="install-the-preview-site-extension"></a><span data-ttu-id="118bc-168">Установка расширения сайта предварительной версии</span><span class="sxs-lookup"><span data-stu-id="118bc-168">Install the preview site extension</span></span>

1. <span data-ttu-id="118bc-169">На портале Azure перейдите к колонке "Служба приложений".</span><span class="sxs-lookup"><span data-stu-id="118bc-169">From the Azure portal, navigate to the App Service blade.</span></span>
1. <span data-ttu-id="118bc-170">Выберите веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="118bc-170">Select the web app.</span></span>
1. <span data-ttu-id="118bc-171">В поле поиска введите "ex" или прокрутите список панелей управления до пункта **СРЕДСТВА РАЗРАБОТКИ**.</span><span class="sxs-lookup"><span data-stu-id="118bc-171">Enter "ex" in the search box or scroll down the list of management panes to **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="118bc-172">Выберите **СРЕДСТВА РАЗРАБОТКИ** > **Расширения**.</span><span class="sxs-lookup"><span data-stu-id="118bc-172">Select **DEVELOPMENT TOOLS** > **Extensions**.</span></span>
1. <span data-ttu-id="118bc-173">Нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="118bc-173">Select **Add**.</span></span>

   ![Колонка "Приложение Azure" с предыдущими шагами](index/_static/x1.png)

1. <span data-ttu-id="118bc-175">Выберите **Расширения ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="118bc-175">Select **ASP.NET Core Extensions**.</span></span>
1. <span data-ttu-id="118bc-176">Для принятия условий нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="118bc-176">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="118bc-177">Нажмите **OK**, чтобы установить расширение.</span><span class="sxs-lookup"><span data-stu-id="118bc-177">Select **OK** to install the extension.</span></span>

<span data-ttu-id="118bc-178">По завершении операций добавления устанавливается последняя предварительная версия .NET Core.</span><span class="sxs-lookup"><span data-stu-id="118bc-178">When the add operations complete, the latest .NET Core preview is installed.</span></span> <span data-ttu-id="118bc-179">Проверьте установку, запустив `dotnet --info` в консоли.</span><span class="sxs-lookup"><span data-stu-id="118bc-179">Verify the installation by running `dotnet --info` in the console.</span></span> <span data-ttu-id="118bc-180">В колонке **Служба приложений**.</span><span class="sxs-lookup"><span data-stu-id="118bc-180">From the **App Service** blade:</span></span>

1. <span data-ttu-id="118bc-181">В поле поиска введите "con" или прокрутите список панелей управления до пункта **СРЕДСТВА РАЗРАБОТКИ**.</span><span class="sxs-lookup"><span data-stu-id="118bc-181">Enter "con" in the search box or scroll down the list of management panes to **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="118bc-182">Выберите **СРЕДСТВА РАЗРАБОТКИ** > **Консоль**.</span><span class="sxs-lookup"><span data-stu-id="118bc-182">Select **DEVELOPMENT TOOLS** > **Console**.</span></span>
1. <span data-ttu-id="118bc-183">Введите `dotnet --info` в консоли.</span><span class="sxs-lookup"><span data-stu-id="118bc-183">Enter `dotnet --info` in the console.</span></span>

<span data-ttu-id="118bc-184">Если версия `2.1.300-preview1-008174` является последним предварительным выпуском, вы получите следующие выходные данные, выполнив `dotnet --info` в командной строке:</span><span class="sxs-lookup"><span data-stu-id="118bc-184">If version `2.1.300-preview1-008174` is the latest preview release, the following output is obtained by running `dotnet --info` at the command prompt:</span></span>

![Колонка "Приложение Azure" с предыдущими шагами](index/_static/cons.png)

<span data-ttu-id="118bc-186">В примере на предыдущей иллюстрации показана версия ASP.NET Core `2.1.300-preview1-008174`.</span><span class="sxs-lookup"><span data-stu-id="118bc-186">The version of ASP.NET Core shown in the preceding image, `2.1.300-preview1-008174`, is an example.</span></span> <span data-ttu-id="118bc-187">Последняя предварительная версия ASP.NET Core во время настройки расширения сайта отображается при выполнении `dotnet --info`.</span><span class="sxs-lookup"><span data-stu-id="118bc-187">The latest preview version of ASP.NET Core at the time the site extension is configured appears when you execute `dotnet --info`.</span></span>

<span data-ttu-id="118bc-188">`dotnet --info` отображает путь к расширению сайта, где установлена предварительная версия.</span><span class="sxs-lookup"><span data-stu-id="118bc-188">The `dotnet --info` displays the path to the site extension where the Preview has been installed.</span></span> <span data-ttu-id="118bc-189">Он показывает, что приложение выполняется из расширения сайта, а не из расположения *ProgramFiles* по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="118bc-189">It shows the app is running from the site extension instead of from the default *ProgramFiles* location.</span></span> <span data-ttu-id="118bc-190">Если вы видите *ProgramFiles*, перезапустите сайт и выполните `dotnet --info`.</span><span class="sxs-lookup"><span data-stu-id="118bc-190">If you see *ProgramFiles*, restart the site and run `dotnet --info`.</span></span>

<span data-ttu-id="118bc-191">**Использование расширения сайта предварительной версии с шаблоном ARM**</span><span class="sxs-lookup"><span data-stu-id="118bc-191">**Use the preview site extension with an ARM template**</span></span>

<span data-ttu-id="118bc-192">Если вы используете шаблон ARM для создания и развертывания приложений, можно использовать тип ресурса `siteextensions`, чтобы добавить расширение сайта в веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="118bc-192">If an ARM template is used to create and deploy apps, the `siteextensions` resource type can be used to add the site extension to a web app.</span></span> <span data-ttu-id="118bc-193">Пример:</span><span class="sxs-lookup"><span data-stu-id="118bc-193">For example:</span></span>

[!code-json[Main](index/sample/arm.json?highlight=2)]

<!--
### Deploy the app self-contained

A [self-contained app](/dotnet/core/deploying/#self-contained-deployments-scd) can be deployed that carries the preview runtime in the deployment. When deploying a self-contained app:

* The site doesn't need to be prepared.
* The app must be published differently than when publishing for a framework-dependent deployment with the shared runtime and host on the server.

Self-contained apps are an option for all ASP.NET Core apps.
-->

### <a name="use-docker-with-web-apps-for-containers"></a><span data-ttu-id="118bc-194">Использование Docker с веб-приложениями для контейнеров</span><span class="sxs-lookup"><span data-stu-id="118bc-194">Use Docker with Web Apps for containers</span></span>

<span data-ttu-id="118bc-195">[Центр Docker](https://hub.docker.com/r/microsoft/aspnetcore/) содержит образы Docker из последней предварительной версии.</span><span class="sxs-lookup"><span data-stu-id="118bc-195">The [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contains the latest preview Docker images.</span></span> <span data-ttu-id="118bc-196">Их можно использовать в качестве базового образа.</span><span class="sxs-lookup"><span data-stu-id="118bc-196">The images can be used as a base image.</span></span> <span data-ttu-id="118bc-197">Использование образа и развертывание в веб-приложениях для контейнеров выполняется как обычно.</span><span class="sxs-lookup"><span data-stu-id="118bc-197">Use the image and deploy to Web Apps for Containers normally.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="118bc-198">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="118bc-198">Additional resources</span></span>

* [<span data-ttu-id="118bc-199">Общие сведения о веб-приложениях (5-минутное видео)</span><span class="sxs-lookup"><span data-stu-id="118bc-199">Web Apps overview (5-minute overview video)</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="118bc-200">Служба приложений Azure: оптимальное место для размещения приложений .NET (55-минутное видео)</span><span class="sxs-lookup"><span data-stu-id="118bc-200">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="118bc-201">Azure, пятница: практики диагностики и устранения неполадок в службе приложений Azure (12-минутное видео)</span><span class="sxs-lookup"><span data-stu-id="118bc-201">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="118bc-202">Общие сведения о диагностике в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="118bc-202">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

<span data-ttu-id="118bc-203">Служба приложений Azure на Windows Server использует [службы IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="118bc-203">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="118bc-204">Технологии IIS посвящены следующие статьи.</span><span class="sxs-lookup"><span data-stu-id="118bc-204">The following topics pertain to the underlying IIS technology:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [<span data-ttu-id="118bc-205">Библиотека Microsoft TechNet: Windows Server</span><span class="sxs-lookup"><span data-stu-id="118bc-205">Microsoft TechNet Library: Windows Server</span></span>](/windows-server/windows-server-versions)
