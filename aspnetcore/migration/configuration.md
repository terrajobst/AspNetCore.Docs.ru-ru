---
title: Перенос конфигурации в ASP.NET Core
author: ardalis
description: Узнайте, как выполнять миграцию конфигурации из проекта ASP.NET MVC в проект ASP.NET Core MVC.
ms.author: riande
ms.date: 10/14/2016
uid: migration/configuration
ms.openlocfilehash: c379f1f64dc5ab8aeb48055124e86e4e60d93785
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64894931"
---
# <a name="migrate-configuration-to-aspnet-core"></a><span data-ttu-id="1dda0-103">Перенос конфигурации в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1dda0-103">Migrate configuration to ASP.NET Core</span></span>

<span data-ttu-id="1dda0-104">Авторы: [Стив Смит](https://ardalis.com/) (Steve Smith) и [Скотт Эдди](https://scottaddie.com) (Scott Addie)</span><span class="sxs-lookup"><span data-stu-id="1dda0-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="1dda0-105">В предыдущей статье, мы начали [миграции проекта ASP.NET MVC в ASP.NET Core MVC](xref:migration/mvc).</span><span class="sxs-lookup"><span data-stu-id="1dda0-105">In the previous article, we began [migrate an ASP.NET MVC project to ASP.NET Core MVC](xref:migration/mvc).</span></span> <span data-ttu-id="1dda0-106">В этой статье мы переносим конфигурации.</span><span class="sxs-lookup"><span data-stu-id="1dda0-106">In this article, we migrate configuration.</span></span>

<span data-ttu-id="1dda0-107">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/migration/configuration/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1dda0-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/migration/configuration/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="setup-configuration"></a><span data-ttu-id="1dda0-108">Настройка установки</span><span class="sxs-lookup"><span data-stu-id="1dda0-108">Setup configuration</span></span>

<span data-ttu-id="1dda0-109">ASP.NET Core больше не использует *Global.asax* и *web.config* файлы, которые используются в предыдущих версиях ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1dda0-109">ASP.NET Core no longer uses the *Global.asax* and *web.config* files that previous versions of ASP.NET utilized.</span></span> <span data-ttu-id="1dda0-110">В более ранних версиях ASP.NET, логика запуска приложения был помещен в `Application_StartUp` метода в *Global.asax*.</span><span class="sxs-lookup"><span data-stu-id="1dda0-110">In earlier versions of ASP.NET, application startup logic was placed in an `Application_StartUp` method within *Global.asax*.</span></span> <span data-ttu-id="1dda0-111">Далее, в ASP.NET MVC, *Startup.cs* файл был включен в корне проекта; и он был вызван при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="1dda0-111">Later, in ASP.NET MVC, a *Startup.cs* file was included in the root of the project; and, it was called when the application started.</span></span> <span data-ttu-id="1dda0-112">ASP.NET Core была внедрена такой подход полностью, поместив вся логика запуска в *Startup.cs* файла.</span><span class="sxs-lookup"><span data-stu-id="1dda0-112">ASP.NET Core has adopted this approach completely by placing all startup logic in the *Startup.cs* file.</span></span>

<span data-ttu-id="1dda0-113">*Web.config* файла также был заменен в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1dda0-113">The *web.config* file has also been replaced in ASP.NET Core.</span></span> <span data-ttu-id="1dda0-114">Самой конфигурации теперь можно настроить, как часть процедуры запуска приложения, описанные в *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="1dda0-114">Configuration itself can now be configured, as part of the application startup procedure described in *Startup.cs*.</span></span> <span data-ttu-id="1dda0-115">Конфигурации по-прежнему могут использовать XML-файлы, но обычно проекты ASP.NET Core и помещает значения конфигурации в файл в формате JSON, такие как *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="1dda0-115">Configuration can still utilize XML files, but typically ASP.NET Core projects will place configuration values in a JSON-formatted file, such as *appsettings.json*.</span></span> <span data-ttu-id="1dda0-116">Система конфигурации ASP.NET Core также упрощает доступ к переменные среды, которые может предоставить [более безопасные и надежные расположения](xref:security/app-secrets) для конкретных значений.</span><span class="sxs-lookup"><span data-stu-id="1dda0-116">ASP.NET Core's configuration system can also easily access environment variables, which can provide a [more secure and robust location](xref:security/app-secrets) for environment-specific values.</span></span> <span data-ttu-id="1dda0-117">Это особенно верно для секретные данные, например строки подключения и ключи API, которые не должны быть проверены в систему управления версиями.</span><span class="sxs-lookup"><span data-stu-id="1dda0-117">This is especially true for secrets like connection strings and API keys that shouldn't be checked into source control.</span></span> <span data-ttu-id="1dda0-118">См. в разделе [конфигурации](xref:fundamentals/configuration/index) Дополнительные сведения о конфигурации в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1dda0-118">See [Configuration](xref:fundamentals/configuration/index) to learn more about configuration in ASP.NET Core.</span></span>

<span data-ttu-id="1dda0-119">В этой статье мы начинаем с частично переноса проекта ASP.NET Core из [предыдущей статье](xref:migration/mvc).</span><span class="sxs-lookup"><span data-stu-id="1dda0-119">For this article, we are starting with the partially migrated ASP.NET Core project from [the previous article](xref:migration/mvc).</span></span> <span data-ttu-id="1dda0-120">Чтобы настроить конфигурацию, добавьте следующий конструктор и свойства *Startup.cs* файл, расположенный в корневой папке проекта:</span><span class="sxs-lookup"><span data-stu-id="1dda0-120">To setup configuration, add the following constructor and property to the *Startup.cs* file located in the root of the project:</span></span>

[!code-csharp[](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-16)]

<span data-ttu-id="1dda0-121">Обратите внимание, на этом этапе *Startup.cs* файл не будет компилироваться, так как мы еще нужно добавить следующие `using` инструкции:</span><span class="sxs-lookup"><span data-stu-id="1dda0-121">Note that at this point, the *Startup.cs* file won't compile, as we still need to add the following `using` statement:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="1dda0-122">Добавить *appsettings.json* файл в корень проекта, с помощью шаблона соответствующего элемента:</span><span class="sxs-lookup"><span data-stu-id="1dda0-122">Add an *appsettings.json* file to the root of the project using the appropriate item template:</span></span>

![Добавьте AppSettings JSON](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a><span data-ttu-id="1dda0-124">Перенос параметров конфигурации из файла web.config</span><span class="sxs-lookup"><span data-stu-id="1dda0-124">Migrate configuration settings from web.config</span></span>

<span data-ttu-id="1dda0-125">Наш проект ASP.NET MVC включены строку подключения базы данных, необходимых в *web.config*в `<connectionStrings>` элемент.</span><span class="sxs-lookup"><span data-stu-id="1dda0-125">Our ASP.NET MVC project included the required database connection string in *web.config*, in the `<connectionStrings>` element.</span></span> <span data-ttu-id="1dda0-126">В нашем проекте ASP.NET Core, мы хотим сохранить эту информацию в *appsettings.json* файла.</span><span class="sxs-lookup"><span data-stu-id="1dda0-126">In our ASP.NET Core project, we are going to store this information in the *appsettings.json* file.</span></span> <span data-ttu-id="1dda0-127">Откройте *appsettings.json*и обратите внимание, что он уже содержит следующее:</span><span class="sxs-lookup"><span data-stu-id="1dda0-127">Open *appsettings.json*, and note that it already includes the following:</span></span>

[!code-json[](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]

<span data-ttu-id="1dda0-128">В выделенной строке, описанному выше, измените имя базы данных из **_CHANGE_ME** имя базы данных.</span><span class="sxs-lookup"><span data-stu-id="1dda0-128">In the highlighted line depicted above, change the name of the database from **_CHANGE_ME** to the name of your database.</span></span>

## <a name="summary"></a><span data-ttu-id="1dda0-129">Сводка</span><span class="sxs-lookup"><span data-stu-id="1dda0-129">Summary</span></span>

<span data-ttu-id="1dda0-130">ASP.NET Core помещает всю логику запуска для приложения в одном файле, в котором необходимые службы и зависимости могут быть определены и настроен.</span><span class="sxs-lookup"><span data-stu-id="1dda0-130">ASP.NET Core places all startup logic for the application in a single file, in which the necessary services and dependencies can be defined and configured.</span></span> <span data-ttu-id="1dda0-131">Он заменяет *web.config* файл с помощью функции гибкой конфигурации, который можно использовать разные форматы файлов, таких как JSON, а также переменные среды.</span><span class="sxs-lookup"><span data-stu-id="1dda0-131">It replaces the *web.config* file with a flexible configuration feature that can leverage a variety of file formats, such as JSON, as well as environment variables.</span></span>
