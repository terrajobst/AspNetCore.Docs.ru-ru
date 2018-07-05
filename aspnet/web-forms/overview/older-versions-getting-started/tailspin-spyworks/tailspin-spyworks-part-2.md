---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: 'Часть 2: Уровень доступа к данным | Документация Майкрософт'
author: JoeStagner
description: В этой серии руководств описаны все действия, предпринимаемые для создайте пример приложения Tailspin Spyworks. В части 2 рассматривается добавление уровня доступа к данным.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: 13abb02e76b3af80aa11d09e75dc223403917804
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382573"
---
<a name="part-2-data-access-layer"></a>Часть 2: Уровень доступа к данным
====================
по [(Joe Stagner)](https://github.com/JoeStagner)

> Tailspin Spyworks демонстрирует, как чрезвычайно прост процесс создания мощных и масштабируемых приложений для платформы .NET. Он демонстрирует способы использования новые возможности в ASP.NET 4 для создание Интернет-магазина, включая покупок, извлечения и администрирования.
> 
> В этой серии руководств описаны все действия, предпринимаемые для создайте пример приложения Tailspin Spyworks. В части 2 рассматривается добавление уровня доступа к данным.


## <a id="_Toc260221668"></a>  Добавление уровня доступа к данным

Наше приложение электронной коммерции будет зависеть от двух баз данных.

Сведения о клиенте мы будем использовать стандартной базы данных членства ASP.NET. Для наших корзину для покупок и продукта каталог реализуется базу данных SQL Express следующим образом.

![](tailspin-spyworks-part-2/_static/image1.jpg)

После создания базы данных (Commerce.mdf) в приложении приложения\_можно переходить к слою доступа данных, использующий .NET Entity Framework создать папку данных.

Мы создадим папку с именем «данные\_доступ» и их правой кнопкой мыши на эту папку и выберите «Добавить новый элемент».

В «Установленные шаблоны» элемент и затем выберите «ADO.NET Entity Data Model» введите EDM\_Commerce.edmx как имя и нажмите кнопку «Добавить».

![](tailspin-spyworks-part-2/_static/image2.jpg)

Выберите «Создать из базы данных».

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

Сохраните и выполните сборку.

Теперь мы готовы добавить наш первый компонент — меню категории продукта.

> [!div class="step-by-step"]
> [Назад](tailspin-spyworks-part-1.md)
> [Вперед](tailspin-spyworks-part-3.md)
