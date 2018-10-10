---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: Построение в главе загружаемые файлы для MVC EF 5 4 руководства | Документация Майкрософт
author: Rick-Anderson
description: Пример веб-приложение университета Contoso демонстрирует создание приложения ASP.NET MVC 4, с помощью Entity Framework 5 Code First и Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: 6b5d10ba9e878908953e999bd1fd44970acf4ca5
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911972"
---
<a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a>Построение в главе загружаемые файлы для MVC EF 5 4 руководства
====================
по [Рик Андерсон]((https://twitter.com/RickAndMSFT))

[Скачать завершенный проект](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Пример веб-приложение университета Contoso демонстрирует создание приложения ASP.NET MVC 4, используя Entity Framework 5 Code First и Visual Studio 2012. Сведения о серии руководств см. в [первом руководстве серии](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


## <a name="building-the-chapter-downloads"></a>Создание загрузок для глав

1. Скачайте и распакуйте ZIP-файла образца проекта. В пакет разархивируется загрузки вы найдете дополнительные ZIP-файлы, один для завершения каждой главы.
2. Щелкните правой кнопкой нужный ZIP-файл и нажмите кнопку **свойства**и нажмите кнопку **Unblock** кнопки.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. Распакуйте файл.
4. Дважды щелкните *CUx.sln* файл, чтобы запустить Visual Studio.
5. Из **средства** меню, щелкните **диспетчер пакетов NuGet**, затем **консоль диспетчера пакетов**.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. В консоли диспетчера пакетов (PMC), щелкните **восстановить**.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. Закройте Visual Studio.
8. Перезапустите Visual Studio, откройте файл решения вы закрыли на предыдущем шаге.
9. В консоли диспетчера пакетов (PMC), введите `Update-Database` команды:  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > Если возникнет следующая ошибка:  
    >   
    >  *Термин «Update-Database» не распознан как имя командлета, функции, файла скрипта или действующей программы. Проверьте правильность написания имени или если был задан путь, проверьте правильность пути и повторите попытку.*  
    > Закрыть и перезапустить Visual Studio.

    Будет выполняться каждую операцию миграции, а затем запуска метода заполнения. Теперь можно запустить приложение.

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

> [!div class="step-by-step"]
> [Назад](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
