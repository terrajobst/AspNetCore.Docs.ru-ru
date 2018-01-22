---
title: "Новые возможности ASP.NET Core 1.1"
author: rick-anderson
description: "Новые возможности ASP.NET Core 1.1"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: aspnetcore-1.1
ms.openlocfilehash: 56260241cd42eeb98b47277cd3f8a139a2a2fe50
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="whats-new-in-aspnet-core-11"></a><span data-ttu-id="3a59f-103">Новые возможности ASP.NET Core 1.1</span><span class="sxs-lookup"><span data-stu-id="3a59f-103">What's new in ASP.NET Core 1.1</span></span>

<span data-ttu-id="3a59f-104">В состав ASP.NET Core 1.1 входят следующие новые функции и компоненты:</span><span class="sxs-lookup"><span data-stu-id="3a59f-104">ASP.NET Core 1.1 includes the following new features:</span></span>

- [<span data-ttu-id="3a59f-105">ПО промежуточного слоя для переопределения URL-адресов</span><span class="sxs-lookup"><span data-stu-id="3a59f-105">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting)
- [<span data-ttu-id="3a59f-106">ПО промежуточного слоя для кэширования ответов</span><span class="sxs-lookup"><span data-stu-id="3a59f-106">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
- [<span data-ttu-id="3a59f-107">Просмотр компонентов как вспомогательных функций тегов</span><span class="sxs-lookup"><span data-stu-id="3a59f-107">View Components as Tag Helpers</span></span>](xref:mvc/views/view-components#invoking-a-view-component-as-a-tag-helper)
- [<span data-ttu-id="3a59f-108">ПО промежуточного слоя в качестве фильтров MVC</span><span class="sxs-lookup"><span data-stu-id="3a59f-108">Middleware as MVC filters</span></span>](xref:mvc/controllers/filters#using-middleware-in-the-filter-pipeline)
- [<span data-ttu-id="3a59f-109">Поставщик TempData на основе файлов cookie</span><span class="sxs-lookup"><span data-stu-id="3a59f-109">Cookie-based TempData provider</span></span>](xref:fundamentals/app-state#tempdata)
- [<span data-ttu-id="3a59f-110">Поставщик ведения журнала службы приложений Azure</span><span class="sxs-lookup"><span data-stu-id="3a59f-110">Azure App Service logging provider</span></span>](xref:fundamentals/logging/index#appservice)
- [<span data-ttu-id="3a59f-111">Поставщик конфигурации Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="3a59f-111">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
- [<span data-ttu-id="3a59f-112">Репозитории ключей защиты данных для хранилищ Azure и Redis</span><span class="sxs-lookup"><span data-stu-id="3a59f-112">Azure and Redis Storage Data Protection Key Repositories</span></span>](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis)
- [<span data-ttu-id="3a59f-113">Сервер WebListener для Windows</span><span class="sxs-lookup"><span data-stu-id="3a59f-113">WebListener Server for Windows</span></span>](xref:fundamentals/servers/weblistener)
- [<span data-ttu-id="3a59f-114">Поддержка WebSocket</span><span class="sxs-lookup"><span data-stu-id="3a59f-114">WebSockets support</span></span>](xref:fundamentals/websockets)

## <a name="choosing-between-versions-10-and-11-of-aspnet-core"></a><span data-ttu-id="3a59f-115">Выбор между версиями ASP.NET Core 1.0 и 1.1</span><span class="sxs-lookup"><span data-stu-id="3a59f-115">Choosing between versions 1.0 and 1.1 of ASP.NET Core</span></span>

<span data-ttu-id="3a59f-116">ASP.NET Core 1.1 имеет более широкий набор возможностей, чем 1.0.</span><span class="sxs-lookup"><span data-stu-id="3a59f-116">ASP.NET Core 1.1 has more features than 1.0.</span></span> <span data-ttu-id="3a59f-117">Как правило, мы рекомендуем использовать последнюю версию.</span><span class="sxs-lookup"><span data-stu-id="3a59f-117">In general, we recommend you use the latest version.</span></span>

## <a name="additional-information"></a><span data-ttu-id="3a59f-118">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="3a59f-118">Additional Information</span></span>

- [<span data-ttu-id="3a59f-119">Заметки о выпуске ASP.NET Core 1.1.0</span><span class="sxs-lookup"><span data-stu-id="3a59f-119">ASP.NET Core 1.1.0 Release Notes</span></span>](https://github.com/aspnet/Home/releases/tag/1.1.0)
- <span data-ttu-id="3a59f-120">Если вы хотите отслеживать ход работы и планы команды разработчиков ASP.NET Core, смотрите еженедельные выпуски [ASP.NET Community Standup](https://live.asp.net/).</span><span class="sxs-lookup"><span data-stu-id="3a59f-120">If you’d like to connect with the ASP.NET Core development team’s progress and plans, tune in to the weekly [ASP.NET Community Standup](https://live.asp.net/).</span></span>
