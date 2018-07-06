---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: Построение в главе загружаемые файлы для MVC EF 5 4 руководства | Документация Майкрософт
author: Rick-Anderson
description: Пример веб-приложение университета Contoso демонстрирует создание приложения ASP.NET MVC 4, с помощью Entity Framework 5 Code First и Visual Studio...
ms.author: aspnetcontent
ms.date: 07/30/2013
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: 0433c07bc42d7d5f397772704a6cb7aa2e03f8e2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810793"
---
<a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a><span data-ttu-id="fc243-103">Построение в главе загружаемые файлы для MVC EF 5 4 руководства</span><span class="sxs-lookup"><span data-stu-id="fc243-103">Building the Chapter Downloads for the EF 5 MVC 4 Tutorials</span></span>
====================
<span data-ttu-id="fc243-104">по [Рик Андерсон](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="fc243-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[<span data-ttu-id="fc243-105">Скачать завершенный проект</span><span class="sxs-lookup"><span data-stu-id="fc243-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="fc243-106">Пример веб-приложение университета Contoso демонстрирует создание приложения ASP.NET MVC 4, используя Entity Framework 5 Code First и Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="fc243-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="fc243-107">Сведения о серии руководств см. в [первом руководстве серии](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="fc243-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>


## <a name="building-the-chapter-downloads"></a><span data-ttu-id="fc243-108">Создание загрузок для глав</span><span class="sxs-lookup"><span data-stu-id="fc243-108">Building the Chapter Downloads</span></span>

1. <span data-ttu-id="fc243-109">Скачайте и распакуйте ZIP-файла образца проекта.</span><span class="sxs-lookup"><span data-stu-id="fc243-109">Download and unzip the  project sample zip file.</span></span> <span data-ttu-id="fc243-110">В пакет разархивируется загрузки вы найдете дополнительные ZIP-файлы, один для завершения каждой главы.</span><span class="sxs-lookup"><span data-stu-id="fc243-110">In the unzipped download package, you will find additional zip files, one for the completion of each chapter.</span></span>
2. <span data-ttu-id="fc243-111">Щелкните правой кнопкой нужный ZIP-файл и нажмите кнопку **свойства**и нажмите кнопку **Unblock** кнопки.</span><span class="sxs-lookup"><span data-stu-id="fc243-111">Right click on the desired zip file, click **Properties**, and click the **Unblock** button.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. <span data-ttu-id="fc243-112">Распакуйте файл.</span><span class="sxs-lookup"><span data-stu-id="fc243-112">Unzip the file.</span></span>
4. <span data-ttu-id="fc243-113">Дважды щелкните *CUx.sln* файл, чтобы запустить Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fc243-113">Double-click the *CUx.sln* file to launch Visual Studio.</span></span>
5. <span data-ttu-id="fc243-114">Из **средства** меню, щелкните **диспетчер пакетов библиотеки**, затем **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="fc243-114">From the **Tools** menu, click **Library Package Manager**, then **Package Manager Console**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. <span data-ttu-id="fc243-115">В консоли диспетчера пакетов (PMC), щелкните **восстановить**.</span><span class="sxs-lookup"><span data-stu-id="fc243-115">In the Package Manager Console (PMC), click **Restore**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. <span data-ttu-id="fc243-116">Закройте Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fc243-116">Exit Visual Studio.</span></span>
8. <span data-ttu-id="fc243-117">Перезапустите Visual Studio, откройте файл решения вы закрыли на предыдущем шаге.</span><span class="sxs-lookup"><span data-stu-id="fc243-117">Restart Visual Studio, opening the solution file you closed in the step above.</span></span>
9. <span data-ttu-id="fc243-118">В консоли диспетчера пакетов (PMC), введите `Update-Database` команды:</span><span class="sxs-lookup"><span data-stu-id="fc243-118">In the Package Manager Console (PMC), enter the `Update-Database` command:</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > <span data-ttu-id="fc243-119">Если возникнет следующая ошибка:</span><span class="sxs-lookup"><span data-stu-id="fc243-119">If you get the following error:</span></span>  
    >   
    >  <span data-ttu-id="fc243-120">*Термин «Update-Database» не распознан как имя командлета, функции, файла скрипта или действующей программы. Проверьте правильность написания имени или если был задан путь, проверьте правильность пути и повторите попытку.*</span><span class="sxs-lookup"><span data-stu-id="fc243-120">*The term 'Update-Database' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.*</span></span>  
    > <span data-ttu-id="fc243-121">Закрыть и перезапустить Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fc243-121">Exit and restart Visual Studio.</span></span>

    <span data-ttu-id="fc243-122">Будет выполняться каждую операцию миграции, а затем запуска метода заполнения.</span><span class="sxs-lookup"><span data-stu-id="fc243-122">Each migration will run, then the seed method will run.</span></span> <span data-ttu-id="fc243-123">Теперь можно запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="fc243-123">You can now run the app.</span></span>

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

> [!div class="step-by-step"]
> [<span data-ttu-id="fc243-124">Назад</span><span class="sxs-lookup"><span data-stu-id="fc243-124">Previous</span></span>](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
