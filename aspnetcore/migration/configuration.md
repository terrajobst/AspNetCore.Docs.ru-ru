---
title: "Миграция конфигурации"
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/configuration
ms.openlocfilehash: 23b96ad11201f9b82cbd9fb832757d905407d228
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="migrating-configuration"></a><span data-ttu-id="d88cb-102">Миграция конфигурации</span><span class="sxs-lookup"><span data-stu-id="d88cb-102">Migrating Configuration</span></span>

<span data-ttu-id="d88cb-103">По [Стив Смит](https://ardalis.com/) и [Скотт Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="d88cb-103">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="d88cb-104">В предыдущей статье мы начали [миграции проекта ASP.NET MVC в ASP.NET Core MVC](mvc.md).</span><span class="sxs-lookup"><span data-stu-id="d88cb-104">In the previous article, we began [migrating an ASP.NET MVC project to ASP.NET Core MVC](mvc.md).</span></span> <span data-ttu-id="d88cb-105">В этой статье мы миграцию конфигурации.</span><span class="sxs-lookup"><span data-stu-id="d88cb-105">In this article, we migrate configuration.</span></span>

<span data-ttu-id="d88cb-106">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d88cb-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="setup-configuration"></a><span data-ttu-id="d88cb-107">Настройка установки</span><span class="sxs-lookup"><span data-stu-id="d88cb-107">Setup Configuration</span></span>

<span data-ttu-id="d88cb-108">Больше не используется ASP.NET Core *Global.asax* и *web.config* файлы, загруженные в предыдущих версиях ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d88cb-108">ASP.NET Core no longer uses the *Global.asax* and *web.config* files that previous versions of ASP.NET utilized.</span></span> <span data-ttu-id="d88cb-109">В более ранних версиях ASP.NET логику запуска приложения был помещен в `Application_StartUp` метода в *Global.asax*.</span><span class="sxs-lookup"><span data-stu-id="d88cb-109">In earlier versions of ASP.NET, application startup logic was placed in an `Application_StartUp` method within *Global.asax*.</span></span> <span data-ttu-id="d88cb-110">Далее, в ASP.NET MVC *файла Startup.cs* файл был включен в корне проекта; и он был вызван при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="d88cb-110">Later, in ASP.NET MVC, a *Startup.cs* file was included in the root of the project; and, it was called when the application started.</span></span> <span data-ttu-id="d88cb-111">ASP.NET Core полностью приняла этот подход, поместив вся логика запуска в *файла Startup.cs* файл.</span><span class="sxs-lookup"><span data-stu-id="d88cb-111">ASP.NET Core has adopted this approach completely by placing all startup logic in the *Startup.cs* file.</span></span>

<span data-ttu-id="d88cb-112">*Web.config* также завершает в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d88cb-112">The *web.config* file has also been replaced in ASP.NET Core.</span></span> <span data-ttu-id="d88cb-113">Сама конфигурация теперь можно, как часть процедуры при запуске приложения, описанной в *файла Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="d88cb-113">Configuration itself can now be configured, as part of the application startup procedure described in *Startup.cs*.</span></span> <span data-ttu-id="d88cb-114">Конфигурация по-прежнему можно использовать XML-файлов, но обычно проекты ASP.NET Core будет помещать значения конфигурации в файл в формате JSON, такие как *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d88cb-114">Configuration can still utilize XML files, but typically ASP.NET Core projects will place configuration values in a JSON-formatted file, such as *appsettings.json*.</span></span> <span data-ttu-id="d88cb-115">Система конфигурации ASP.NET Core можно также легко получить переменные среды, в которых может предоставлять более безопасным и надежным для конкретных значений.</span><span class="sxs-lookup"><span data-stu-id="d88cb-115">ASP.NET Core's configuration system can also easily access environment variables, which can provide a more secure and robust location for environment-specific values.</span></span> <span data-ttu-id="d88cb-116">Это особенно верно для секретов, такие как строки подключения и ключи API, которые не должны проверяться в системе управления версиями.</span><span class="sxs-lookup"><span data-stu-id="d88cb-116">This is especially true for secrets like connection strings and API keys that shouldn't be checked into source control.</span></span> <span data-ttu-id="d88cb-117">В разделе [конфигурации](xref:fundamentals/configuration/index) для получения дополнительных сведений о конфигурации в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d88cb-117">See [Configuration](xref:fundamentals/configuration/index) to learn more about configuration in ASP.NET Core.</span></span>

<span data-ttu-id="d88cb-118">Для данной статьи мы приступаете к работе с частично миграции проекта ASP.NET Core из [предыдущей статьи](mvc.md).</span><span class="sxs-lookup"><span data-stu-id="d88cb-118">For this article, we are starting with the partially-migrated ASP.NET Core project from [the previous article](mvc.md).</span></span> <span data-ttu-id="d88cb-119">Для настройки конфигурации, добавьте следующий конструктор и свойства *файла Startup.cs* файл, расположенный в корневой папке проекта:</span><span class="sxs-lookup"><span data-stu-id="d88cb-119">To setup configuration, add the following constructor and property to the *Startup.cs* file located in the root of the project:</span></span>

[!code-csharp[Main](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-21)]

<span data-ttu-id="d88cb-120">Обратите внимание, на этом этапе *файла Startup.cs* файла не может быть скомпилирован, как все равно нужно добавить следующие `using` инструкции:</span><span class="sxs-lookup"><span data-stu-id="d88cb-120">Note that at this point, the *Startup.cs* file won't compile, as we still need to add the following `using` statement:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="d88cb-121">Добавить *appsettings.json* файла в корневую папку проекта, с помощью шаблона соответствующий элемент:</span><span class="sxs-lookup"><span data-stu-id="d88cb-121">Add an *appsettings.json* file to the root of the project using the appropriate item template:</span></span>

![Добавить AppSettings JSON](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a><span data-ttu-id="d88cb-123">Перенос параметров конфигурации из файла web.config</span><span class="sxs-lookup"><span data-stu-id="d88cb-123">Migrate Configuration Settings from web.config</span></span>

<span data-ttu-id="d88cb-124">Наш проект ASP.NET MVC включены в строку подключения базы данных, необходимых в *web.config*в `<connectionStrings>` элемент.</span><span class="sxs-lookup"><span data-stu-id="d88cb-124">Our ASP.NET MVC project included the required database connection string in *web.config*, in the `<connectionStrings>` element.</span></span> <span data-ttu-id="d88cb-125">В данном проекте ASP.NET Core мы будем хранить эти сведения в *appsettings.json* файла.</span><span class="sxs-lookup"><span data-stu-id="d88cb-125">In our ASP.NET Core project, we are going to store this information in the *appsettings.json* file.</span></span> <span data-ttu-id="d88cb-126">Откройте *appsettings.json*и обратите внимание, что он уже включает в себя следующее:</span><span class="sxs-lookup"><span data-stu-id="d88cb-126">Open *appsettings.json*, and note that it already includes the following:</span></span>

[!code-json[Main](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]


<span data-ttu-id="d88cb-127">В выделенной строке, описанные выше, измените имя базы данных из **_CHANGE_ME** к имени базы данных.</span><span class="sxs-lookup"><span data-stu-id="d88cb-127">In the highlighted line depicted above, change the name of the database from **_CHANGE_ME** to the name of your database.</span></span>

## <a name="summary"></a><span data-ttu-id="d88cb-128">Сводка</span><span class="sxs-lookup"><span data-stu-id="d88cb-128">Summary</span></span>

<span data-ttu-id="d88cb-129">ASP.NET Core заключает всю логику запуска приложения в одном файле, в котором необходимые службы и зависимости может быть определения и настройка.</span><span class="sxs-lookup"><span data-stu-id="d88cb-129">ASP.NET Core places all startup logic for the application in a single file, in which the necessary services and dependencies can be defined and configured.</span></span> <span data-ttu-id="d88cb-130">Он заменяет *web.config* файла с помощью функции гибкая настройка, который можно использовать в разнообразных форматах, например JSON, а также переменные среды.</span><span class="sxs-lookup"><span data-stu-id="d88cb-130">It replaces the *web.config* file with a flexible configuration feature that can leverage a variety of file formats, such as JSON, as well as environment variables.</span></span>
