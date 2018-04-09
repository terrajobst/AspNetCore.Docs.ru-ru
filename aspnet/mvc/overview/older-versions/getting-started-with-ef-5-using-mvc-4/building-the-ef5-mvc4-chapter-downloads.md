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
<a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a>Построение в главе загружаемые файлы для MVC EF 5 4 учебники
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

[Загрузка завершенного проекта](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso университета примера веб-приложения демонстрирует создание приложения ASP.NET MVC 4, с помощью Entity Framework 5 Code First и Visual Studio 2012. Сведения о серии руководств см. в [первом руководстве серии](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


## <a name="building-the-chapter-downloads"></a>Построение главе загрузки

1. Загрузите и распакуйте ZIP-файл образца проекта. В пакет распакованную загрузки вы найдете дополнительные ZIP-файлов, для завершения каждой главы.
2. Щелкните правой кнопкой мыши нужный ZIP-файл, нажмите кнопку **свойства**и нажмите кнопку **Unblock** кнопки.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. Распакуйте файлы.
4. Дважды щелкните *CUx.sln* файл, чтобы запустить Visual Studio.
5. Из **средства** меню, нажмите кнопку **диспетчер пакетов библиотеки**, затем **консоль диспетчера пакетов**.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. В пакет Диспетчер консоли (PMC), нажмите кнопку **восстановить**.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. Закройте Visual Studio.
8. Перезапустите Visual Studio, при открытии файла решения, закрыт на предыдущем шаге.
9. В пакет Диспетчер консоли (PMC), введите `Update-Database` команды:  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > Если возникли следующие ошибки:  
    >   
    >  *Термин «Update-Database» не распознан как имя командлета, функции, файла скрипта или действующей программы. Проверьте правильность написания имени, а если включен путь, проверьте правильность пути и повторите попытку.*  
    > Закройте и перезапустите Visual Studio.

    Будет выполняться каждый миграции, а затем метод начальное значение будет выполняться. Теперь можно запустить приложение.

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

> [!div class="step-by-step"]
> [Назад](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
