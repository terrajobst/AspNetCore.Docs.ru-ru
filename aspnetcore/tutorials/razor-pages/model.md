---
title: "Добавление модели в приложение Razor Pages в ASP.NET Core"
author: rick-anderson
description: "Добавление модели в приложение Razor Pages в ASP.NET Core"
keywords: ASP.NET Core,Razor Pages,Razor,MVC
ms.author: riande
manager: wpickett
ms.date: 7/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/modelz
ms.openlocfilehash: 1a08ecf1ee12fa0860cb6a18c1a63eaff2ddfbed
ms.sourcegitcommit: d9ec19e5452af83648074db5d96c0a0f4f9e7f9a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/29/2017
---
# <a name="adding-a-model-to-a-razor-pages-app"></a><span data-ttu-id="307b1-104">Добавление модели в приложение Razor Pages</span><span class="sxs-lookup"><span data-stu-id="307b1-104">Adding a model to a Razor Pages app</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="307b1-105">Добавление модели данных</span><span class="sxs-lookup"><span data-stu-id="307b1-105">Add a data model</span></span>

<span data-ttu-id="307b1-106">В обозревателе решений щелкните проект **RazorPagesMovie** правой кнопкой мыши и выберите пункт **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="307b1-106">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="307b1-107">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="307b1-107">Name the folder *Models*.</span></span>

<span data-ttu-id="307b1-108">Щелкните правой кнопкой мыши папку *Models* и выберите пункт **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="307b1-108">Right click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="307b1-109">Добавьте класс **Movie** и следующие свойства.</span><span class="sxs-lookup"><span data-stu-id="307b1-109">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="307b1-110">Добавление строки подключения базы данных</span><span class="sxs-lookup"><span data-stu-id="307b1-110">Add a database connection string</span></span>

<span data-ttu-id="307b1-111">Добавьте строку подключения в файл *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="307b1-111">Add a connection string to the *appsettings.json* file.</span></span>

<span data-ttu-id="307b1-112">[!code-json[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]</span><span class="sxs-lookup"><span data-stu-id="307b1-112">[!code-json[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]</span></span>

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="307b1-113">Регистрация контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="307b1-113">Register the database context</span></span>

<span data-ttu-id="307b1-114">Зарегистрируйте контекст базы данных в контейнере [внедрения зависимостей](xref:fundamentals/dependency-injection) в файле *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="307b1-114">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>

<span data-ttu-id="307b1-115">[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-6)]</span><span class="sxs-lookup"><span data-stu-id="307b1-115">[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-6)]</span></span>

<span data-ttu-id="307b1-116">Выполните сборку проекта, чтобы убедиться в отсутствии ошибок.</span><span class="sxs-lookup"><span data-stu-id="307b1-116">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="307b1-117">Добавление средств формирования шаблонов и выполнение первоначальной миграции</span><span class="sxs-lookup"><span data-stu-id="307b1-117">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="307b1-118">В этом разделе консоль диспетчера пакетов (PMC) используется для выполнения следующих действий.</span><span class="sxs-lookup"><span data-stu-id="307b1-118">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="307b1-119">Добавление веб-пакета создания кода для Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="307b1-119">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="307b1-120">Этот пакет необходим для работы ядра формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="307b1-120">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="307b1-121">Добавление первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="307b1-121">Add an initial migration.</span></span>
* <span data-ttu-id="307b1-122">Обновление базы данных с помощью первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="307b1-122">Update the database with the initial migration.</span></span>

<span data-ttu-id="307b1-123">В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="307b1-123">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

  ![Меню PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="307b1-125">В PMC введите следующие команды.</span><span class="sxs-lookup"><span data-stu-id="307b1-125">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.0
Add-Migration Initial
Update-Database
```

<span data-ttu-id="307b1-126">Команда `Install-Package` устанавливает средства, необходимые для запуска ядра формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="307b1-126">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="307b1-127">Команда `Add-Migration` формирует код для создания схемы исходной базы данных.</span><span class="sxs-lookup"><span data-stu-id="307b1-127">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="307b1-128">Схема создается на основе модели, указанной в `DbContext` (в файле *Models/MovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="307b1-128">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="307b1-129">Аргумент `Initial` используется для присвоения имен миграциям.</span><span class="sxs-lookup"><span data-stu-id="307b1-129">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="307b1-130">Можно использовать любое имя, но обычно выбирается имя, описывающее миграцию.</span><span class="sxs-lookup"><span data-stu-id="307b1-130">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="307b1-131">Дополнительные сведения см. в статье [Введение в миграции](xref:data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="307b1-131">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="307b1-132">Команда `Update-Database` выполняет `Up` метод в файле *Migrations/\<отметка-времени>_InitialCreate.cs*, который создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="307b1-132">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE[model 4windows](../../includes/RP/model4Win.md)]

[!INCLUDE[model 4](../../includes/RP/model4.md)]

<span data-ttu-id="307b1-133">В следующем учебнике рассматриваются файлы, созданные с помощью формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="307b1-133">The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="307b1-134">[Назад: Начало работы](xref:tutorials/razor-pages/razor-pages-start)
[Далее: Сформированные страницы Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="307b1-134">[Previous: Getting Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>    