---
title: "Работа с данными в ASP.NET Core"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: a8fb7eb7-e0e5-4394-84f3-1f1dbe0ba2ef
ms.technology: aspnet
ms.prod: asp.net-core
ms.openlocfilehash: 3566127476289ae085a9161132b103638bc9b068
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="working-with-data-in-aspnet-core"></a><span data-ttu-id="64071-103">Работа с данными в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="64071-103">Working with Data in ASP.NET Core</span></span> 

*   [<span data-ttu-id="64071-104">Начало работы с ASP.NET Core и Entity Framework Core с использованием Visual Studio</span><span class="sxs-lookup"><span data-stu-id="64071-104">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](ef-mvc/index.md)
    *   [<span data-ttu-id="64071-105">Начало работы</span><span class="sxs-lookup"><span data-stu-id="64071-105">Getting started</span></span>](ef-mvc/intro.md)
    *   [<span data-ttu-id="64071-106">Операции создания, чтения, обновления и удаления</span><span class="sxs-lookup"><span data-stu-id="64071-106">Create, Read, Update, and Delete operations</span></span>](ef-mvc/crud.md)
    *   [<span data-ttu-id="64071-107">Сортировка, фильтрация, разбиение на страницы и группирование данных</span><span class="sxs-lookup"><span data-stu-id="64071-107">Sorting, filtering, paging, and grouping</span></span>](ef-mvc/sort-filter-page.md)
    *   [<span data-ttu-id="64071-108">Миграции</span><span class="sxs-lookup"><span data-stu-id="64071-108">Migrations</span></span>](ef-mvc/migrations.md)
    *   [<span data-ttu-id="64071-109">Создание сложной модели данных</span><span class="sxs-lookup"><span data-stu-id="64071-109">Creating a complex data model</span></span>](ef-mvc/complex-data-model.md)
    *   [<span data-ttu-id="64071-110">Чтение связанных данных</span><span class="sxs-lookup"><span data-stu-id="64071-110">Reading related data</span></span>](ef-mvc/read-related-data.md)
    *   [<span data-ttu-id="64071-111">Обновление связанных данных</span><span class="sxs-lookup"><span data-stu-id="64071-111">Updating related data</span></span>](ef-mvc/update-related-data.md)
    *   [<span data-ttu-id="64071-112">Обработка конфликтов параллелизма</span><span class="sxs-lookup"><span data-stu-id="64071-112">Handling concurrency conflicts</span></span>](ef-mvc/concurrency.md)
    *   [<span data-ttu-id="64071-113">Наследование</span><span class="sxs-lookup"><span data-stu-id="64071-113">Inheritance</span></span>](ef-mvc/inheritance.md)
    *   [<span data-ttu-id="64071-114">Дополнительные разделы</span><span class="sxs-lookup"><span data-stu-id="64071-114">Advanced topics</span></span>](ef-mvc/advanced.md)
* <span data-ttu-id="64071-115">[ASP.NET Core с EF Core — новая база данных](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) (сайт документации по Entity Framework Core)</span><span class="sxs-lookup"><span data-stu-id="64071-115">[ASP.NET Core with EF Core - new database](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) (Entity Framework Core documentation site)</span></span>
* <span data-ttu-id="64071-116">[ASP.NET Core с EF Core — существующая база данных](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db) (сайт документации по Entity Framework Core)</span><span class="sxs-lookup"><span data-stu-id="64071-116">[ASP.NET Core with EF Core - existing database](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db) (Entity Framework Core documentation site)</span></span>
*   [<span data-ttu-id="64071-117">Начало работы с ASP.NET Core и Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="64071-117">Getting Started with ASP.NET Core and Entity Framework 6</span></span>](entity-framework-6.md)
*   [<span data-ttu-id="64071-118">Служба хранилища Azure</span><span class="sxs-lookup"><span data-stu-id="64071-118">Azure Storage</span></span>](azure-storage/index.md)
    *   [<span data-ttu-id="64071-119">Добавление службы хранилища Azure с помощью подключенных служб Visual Studio</span><span class="sxs-lookup"><span data-stu-id="64071-119">Adding Azure Storage by Using Visual Studio Connected Services</span></span>](https://azure.microsoft.com/documentation/articles/vs-azure-tools-connected-services-storage/)
    *   [<span data-ttu-id="64071-120">Начало работы с хранилищем BLOB-объектов Azure и подключенными службами Visual Studio</span><span class="sxs-lookup"><span data-stu-id="64071-120">Get Started with Azure Blob storage and Visual Studio Connected Services</span></span>](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/)
    *   [<span data-ttu-id="64071-121">Начало работы с хранилищем очередей и подключенными службами Visual Studio</span><span class="sxs-lookup"><span data-stu-id="64071-121">Get Started with Queue Storage and Visual Studio Connected Services</span></span>](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-queues/)
    *   [<span data-ttu-id="64071-122">Начало работы с хранилищем таблиц Azure и подключенными службами Visual Studio</span><span class="sxs-lookup"><span data-stu-id="64071-122">How to Get Started with Azure Table Storage and Visual Studio Connected Services</span></span>](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-tables/)
