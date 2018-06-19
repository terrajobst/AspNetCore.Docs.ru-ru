---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: 'Часть 2: Уровень доступа к данным | Документы Microsoft'
author: JoeStagner
description: Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения Tailspin Spyworks. Вторая часть посвящена Добавление доступа к данным.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: 9f734b04a0f4cec3c33bc5b42ef283ea64cdb463
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890493"
---
<a name="part-2-data-access-layer"></a>Часть 2: Уровень доступа к данным
====================
по [(Joe Stagner)](https://github.com/JoeStagner)

> Tailspin Spyworks показано, как чрезвычайно простой можно создавать мощные и масштабируемые приложения для платформы .NET. Он показывает как использовать новые возможности в ASP.NET 4 для создания Интернет-магазина, включая покупок, извлечения и администрирования.
> 
> Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения Tailspin Spyworks. Вторая часть посвящена Добавление доступа к данным.


## <a id="_Toc260221668"></a>  Добавление доступа к данным

Наше приложение электронной коммерции будет зависеть от двух баз данных.

Сведения о заказчике мы используем стандартной базы данных членства ASP.NET. Для наших корзину покупок и продукта каталога мы будет реализован базы данных SQL Express следующим образом.

![](tailspin-spyworks-part-2/_static/image1.jpg)

После создания базы данных (Commerce.mdf) в приложении приложения\_папку данных, мы можно перейти к созданию нашей уровня доступа к данным платформы Entity Framework .NET.

Мы создадим папку с именем «данных\_доступ» и их правой кнопкой мыши на эту папку и выберите «Добавить новый элемент».

В разделе «Установленные шаблоны» элемент и затем выберите пункт меню «модель EDM ADO.NET» введите EDM\_Commerce.edmx как имя и нажмите кнопку «Добавить».

![](tailspin-spyworks-part-2/_static/image2.jpg)

Выберите «Создать из базы данных».

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

Сохраните и постройте.

Теперь мы готовы к добавьте наш первый компонент — меню категории продукта.

> [!div class="step-by-step"]
> [Назад](tailspin-spyworks-part-1.md)
> [Вперед](tailspin-spyworks-part-3.md)
