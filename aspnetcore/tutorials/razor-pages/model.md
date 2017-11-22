---
title: "Добавление модели в приложение Razor Pages в ASP.NET Core"
author: rick-anderson
description: "Добавление модели в приложение Razor Pages в ASP.NET Core"
keywords: ASP.NET Core,Razor Pages,Razor,MVC
ms.author: riande
manager: wpickett
ms.date: 07/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/model
ms.openlocfilehash: 38f27a1d5ca80cec4b7bc43c3d5473fc829f1b05
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="adding-a-model-to-a-razor-pages-app"></a><span data-ttu-id="d55cc-104">Добавление модели в приложение Razor Pages</span><span class="sxs-lookup"><span data-stu-id="d55cc-104">Adding a model to a Razor Pages app</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="d55cc-105">Добавление модели данных</span><span class="sxs-lookup"><span data-stu-id="d55cc-105">Add a data model</span></span>

<span data-ttu-id="d55cc-106">В обозревателе решений щелкните проект **RazorPagesMovie** правой кнопкой мыши и выберите пункт **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="d55cc-106">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="d55cc-107">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="d55cc-107">Name the folder *Models*.</span></span>

<span data-ttu-id="d55cc-108">Щелкните правой кнопкой мыши папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="d55cc-108">Right click the *Models* folder.</span></span> <span data-ttu-id="d55cc-109">Выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="d55cc-109">Select **Add** > **Class**.</span></span> <span data-ttu-id="d55cc-110">Добавьте класс **Movie** и следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="d55cc-110">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="d55cc-111">Добавление строки подключения базы данных</span><span class="sxs-lookup"><span data-stu-id="d55cc-111">Add a database connection string</span></span>

<span data-ttu-id="d55cc-112">Добавьте строку подключения в файл *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d55cc-112">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="d55cc-113">Регистрация контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="d55cc-113">Register the database context</span></span>

<span data-ttu-id="d55cc-114">Зарегистрируйте контекст базы данных в контейнере [внедрения зависимостей](xref:fundamentals/dependency-injection) в файле *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="d55cc-114">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-6)]

<span data-ttu-id="d55cc-115">Выполните сборку проекта, чтобы убедиться в отсутствии ошибок.</span><span class="sxs-lookup"><span data-stu-id="d55cc-115">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="d55cc-116">Добавление средств формирования шаблонов и выполнение первоначальной миграции</span><span class="sxs-lookup"><span data-stu-id="d55cc-116">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="d55cc-117">В этом разделе консоль диспетчера пакетов (PMC) используется для выполнения следующих действий.</span><span class="sxs-lookup"><span data-stu-id="d55cc-117">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="d55cc-118">Добавление веб-пакета создания кода для Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d55cc-118">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="d55cc-119">Этот пакет необходим для работы ядра формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="d55cc-119">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="d55cc-120">Добавление первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="d55cc-120">Add an initial migration.</span></span>
* <span data-ttu-id="d55cc-121">Обновления базы данных с помощью первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="d55cc-121">Update the database with the initial migration.</span></span>

<span data-ttu-id="d55cc-122">В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="d55cc-122">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Меню PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="d55cc-124">В PMC введите следующие команды.</span><span class="sxs-lookup"><span data-stu-id="d55cc-124">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.0
Add-Migration Initial
Update-Database
```

<span data-ttu-id="d55cc-125">Команда `Install-Package` устанавливает средства, необходимые для запуска ядра формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="d55cc-125">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="d55cc-126">Команда `Add-Migration` формирует код для создания схемы исходной базы данных.</span><span class="sxs-lookup"><span data-stu-id="d55cc-126">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="d55cc-127">Схема создается на основе модели, указанной в `DbContext` (в файле *Models/MovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="d55cc-127">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="d55cc-128">Аргумент `Initial` используется для присвоения имен миграциям.</span><span class="sxs-lookup"><span data-stu-id="d55cc-128">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="d55cc-129">Можно использовать любое имя, но обычно выбирается имя, описывающее миграцию.</span><span class="sxs-lookup"><span data-stu-id="d55cc-129">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="d55cc-130">Дополнительные сведения см. в статье [Введение в миграции](xref:data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="d55cc-130">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="d55cc-131">Команда `Update-Database` выполняет метод `Up` в файле *Migrations/\<метка-времени>_InitialCreate.cs*, который создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="d55cc-131">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE[model 4windows](../../includes/RP/model4Win.md)]

[!INCLUDE[model 4](../../includes/RP/model4tbl.md)]

<a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="d55cc-132">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="d55cc-132">Test the app</span></span>

* <span data-ttu-id="d55cc-133">Запустите приложение и добавьте `/Movies` к URL-адресу в браузере (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="d55cc-133">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="d55cc-134">Протестируйте ссылку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="d55cc-134">Test the **Create** link.</span></span>

 ![Страница "Создать"](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="d55cc-136">Протестируйте ссылки **Изменить**, **Сведения** и **Удалить**.</span><span class="sxs-lookup"><span data-stu-id="d55cc-136">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="d55cc-137">Если появляется исключение SQL, выполните миграции и обновите базу данных.</span><span class="sxs-lookup"><span data-stu-id="d55cc-137">If you get a SQL exception, verify you have run migrations and updated the database:</span></span>

<span data-ttu-id="d55cc-138">В следующем учебнике рассматриваются файлы, созданные с помощью формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="d55cc-138">The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="d55cc-139">[Назад: Начало работы](xref:tutorials/razor-pages/razor-pages-start)
[Далее: Сформированные страницы Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="d55cc-139">[Previous: Getting Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>    
