---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: Построение в главе загружаемые файлы для MVC EF 5 4 учебники | Документы Microsoft
author: Rick-Anderson
description: Contoso университета примера веб-приложения демонстрирует создание приложения ASP.NET MVC 4, с помощью Entity Framework 5 Code First и Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: bda7effabd715e4658d2472e1f0a66d7bba18cab
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a><span data-ttu-id="9e29f-103">Построение в главе загружаемые файлы для MVC EF 5 4 учебники</span><span class="sxs-lookup"><span data-stu-id="9e29f-103">Building the Chapter Downloads for the EF 5 MVC 4 Tutorials</span></span>
====================
<span data-ttu-id="9e29f-104">по [Рик Андерсон](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="9e29f-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[<span data-ttu-id="9e29f-105">Загрузка завершенного проекта</span><span class="sxs-lookup"><span data-stu-id="9e29f-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="9e29f-106">Contoso университета примера веб-приложения демонстрирует создание приложения ASP.NET MVC 4, с помощью Entity Framework 5 Code First и Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="9e29f-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="9e29f-107">Сведения о серии руководств см. в [первом руководстве серии](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="9e29f-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>


## <a name="building-the-chapter-downloads"></a><span data-ttu-id="9e29f-108">Построение главе загрузки</span><span class="sxs-lookup"><span data-stu-id="9e29f-108">Building the Chapter Downloads</span></span>

1. <span data-ttu-id="9e29f-109">Загрузите и распакуйте ZIP-файл образца проекта.</span><span class="sxs-lookup"><span data-stu-id="9e29f-109">Download and unzip the  project sample zip file.</span></span> <span data-ttu-id="9e29f-110">В пакет распакованную загрузки вы найдете дополнительные ZIP-файлов, для завершения каждой главы.</span><span class="sxs-lookup"><span data-stu-id="9e29f-110">In the unzipped download package, you will find additional zip files, one for the completion of each chapter.</span></span>
2. <span data-ttu-id="9e29f-111">Щелкните правой кнопкой мыши нужный ZIP-файл, нажмите кнопку **свойства**и нажмите кнопку **Unblock** кнопки.</span><span class="sxs-lookup"><span data-stu-id="9e29f-111">Right click on the desired zip file, click **Properties**, and click the **Unblock** button.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. <span data-ttu-id="9e29f-112">Распакуйте файлы.</span><span class="sxs-lookup"><span data-stu-id="9e29f-112">Unzip the file.</span></span>
4. <span data-ttu-id="9e29f-113">Дважды щелкните *CUx.sln* файл, чтобы запустить Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9e29f-113">Double-click the *CUx.sln* file to launch Visual Studio.</span></span>
5. <span data-ttu-id="9e29f-114">Из **средства** меню, нажмите кнопку **диспетчер пакетов библиотеки**, затем **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="9e29f-114">From the **Tools** menu, click **Library Package Manager**, then **Package Manager Console**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. <span data-ttu-id="9e29f-115">В пакет Диспетчер консоли (PMC), нажмите кнопку **восстановить**.</span><span class="sxs-lookup"><span data-stu-id="9e29f-115">In the Package Manager Console (PMC), click **Restore**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. <span data-ttu-id="9e29f-116">Закройте Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9e29f-116">Exit Visual Studio.</span></span>
8. <span data-ttu-id="9e29f-117">Перезапустите Visual Studio, при открытии файла решения, закрыт на предыдущем шаге.</span><span class="sxs-lookup"><span data-stu-id="9e29f-117">Restart Visual Studio, opening the solution file you closed in the step above.</span></span>
9. <span data-ttu-id="9e29f-118">В пакет Диспетчер консоли (PMC), введите `Update-Database` команды:</span><span class="sxs-lookup"><span data-stu-id="9e29f-118">In the Package Manager Console (PMC), enter the `Update-Database` command:</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > <span data-ttu-id="9e29f-119">Если возникли следующие ошибки:</span><span class="sxs-lookup"><span data-stu-id="9e29f-119">If you get the following error:</span></span>  
    >   
    >  <span data-ttu-id="9e29f-120">*Термин «Update-Database» не распознан как имя командлета, функции, файла скрипта или действующей программы. Проверьте правильность написания имени, а если включен путь, проверьте правильность пути и повторите попытку.*</span><span class="sxs-lookup"><span data-stu-id="9e29f-120">*The term 'Update-Database' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.*</span></span>  
    > <span data-ttu-id="9e29f-121">Закройте и перезапустите Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9e29f-121">Exit and restart Visual Studio.</span></span>

    <span data-ttu-id="9e29f-122">Будет выполняться каждый миграции, а затем метод начальное значение будет выполняться.</span><span class="sxs-lookup"><span data-stu-id="9e29f-122">Each migration will run, then the seed method will run.</span></span> <span data-ttu-id="9e29f-123">Теперь можно запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="9e29f-123">You can now run the app.</span></span>

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

> [!div class="step-by-step"]
> [<span data-ttu-id="9e29f-124">Назад</span><span class="sxs-lookup"><span data-stu-id="9e29f-124">Previous</span></span>](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
