---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
title: Начало работы с AJAX Control Toolkit (C#) | Документация Майкрософт
author: microsoft
description: Узнайте все что нужно знать, чтобы приступить к работе с AJAX Control Toolkit.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 16dc5c11-65be-4eae-a818-9fad7f8259c6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
msc.type: authoredcontent
ms.openlocfilehash: 6ecf716b78a789ca72e8b35e0be3e1fd0b957052
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838844"
---
<a name="get-started-with-the-ajax-control-toolkit-c"></a>Начало работы с AJAX Control Toolkit (C#)
====================
по [Microsoft](https://github.com/microsoft)

> Узнайте все что нужно знать, чтобы приступить к работе с AJAX Control Toolkit.


AJAX Control Toolkit содержит более 30 бесплатные элементы управления, которые можно использовать в приложениях ASP.NET. В этом руководстве вы узнаете, как скачать AJAX Control Toolkit и добавьте в набор средств элементы управления панели инструментов Visual Studio или Visual Web Developer Express.

## <a name="downloading-the-ajax-control-toolkit"></a>Загрузка AJAX Control Toolkit

[AJAX Control Toolkit](http://devexpress.com/act) — это проект с открытым исходным кодом, разработанная компанией членами сообщества ASP.NET и группа разработчиков ASP.NET. 


[![Загрузка AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)

**Рис 01**: загрузка AJAX Control Toolkit ([Просмотр полноразмерного изображения](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))


После загрузки файла, необходимо разблокировать файл. Щелкните правой кнопкой мыши файл, выберите свойства и нажмите кнопку **Unblock** кнопку (см. рис. 2).


[![Разблокировка файла ZIP-набор средств управления AJAX](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)

**Рис. 02**: разблокирования файла ZIP-набор средств управления AJAX ([Просмотр полноразмерного изображения](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))


После разблокирования файла можно было распаковать файл: щелкните правой кнопкой мыши файл и выберите **извлечь все** пункт меню. Теперь мы готовы для добавления в набор средств для панели элементов Visual Studio или Visual Web Developer.

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a>Добавление на панель инструментов AJAX Control Toolkit

— Это самый простой способ использования AJAX Control Toolkit, добавляемый в набор средств панели инструментов Visual Studio или Visual Web Developer (см. рис. 3). Таким образом, можно просто перетащить элемент управления набора средств на страницу, если вы хотите использовать его.


[![AJAX Control Toolkit отображается в панели элементов](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)

**Рис 03**: AJAX Control Toolkit отображается в панели элементов ([Просмотр полноразмерного изображения](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))


Во-первых необходимо добавить на панель инструментов с вкладкой AJAX Control Toolkit. Выполните следующие действия.

1. Создайте новый веб-сайт ASP.NET, выбрав пункт меню файл, создать веб-сайта. Дважды щелкните файл Default.aspx в окне обозревателя решений, чтобы открыть файл в редакторе.
2. Щелкните правой кнопкой мыши панель под вкладка "Общие" и выберите пункт меню **добавить вкладку** (см. рис. 4).
3. Введите новую вкладку, называемую AJAX Control Toolkit.


[![Добавить новую вкладку](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)

**Рис. 04**: добавить новую вкладку ([Просмотр полноразмерного изображения](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))


Далее необходимо добавить элементы управления AJAX Control Toolkit на новой вкладке. Выполните следующие действия.

- Щелкните правой кнопкой мыши под вкладке AJAX Control Toolkit и выбрать пункт меню **Выбор элементов (см. рис. 5)**.
- Перейдите в расположение, где вы распаковали AJAX Control Toolkit и выберите сборку файл AjaxControlToolkit.dll.


[![Выберите элементы для добавления на панель элементов](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)

**05 рис**: Выбор элементов для добавления на панель инструментов ([Просмотр полноразмерного изображения](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))


После выполнения этих действий все элементы набора средств управления будут отображаться в вашем инструментарии.

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a>Обновление до новой версии набора средств

Если вы использовали более ранняя версия набора средств и теперь нужно переместить в более поздней версии приведены рекомендуемые действия:

- Двоичные файлы - удалить старую версию файл AjaxControlToolkit.dll сборки из папки Bin веб-сайта.
- Элементы панели элементов - удалить вкладку AJAX Control Toolkit и выполните действия, чтобы повторно создать на вкладке с новой версией сборки, файл AjaxControlToolkit.dll.

> [!div class="step-by-step"]
> [Вперед](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
