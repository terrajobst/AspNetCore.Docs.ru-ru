---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
title: Начало работы с AJAX Control Toolkit (Visual Basic) | Документация Майкрософт
author: microsoft
description: Узнайте все что нужно знать, чтобы приступить к работе с AJAX Control Toolkit.
ms.author: aspnetcontent
ms.date: 05/12/2009
ms.assetid: 9f8fa166-49a2-402c-b236-20caef0c658f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
msc.type: authoredcontent
ms.openlocfilehash: 65743c9eb8eb1b02c6cf87da53222af1312f4445
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822083"
---
<a name="get-started-with-the-ajax-control-toolkit-vb"></a>Начало работы с AJAX Control Toolkit (VB)
====================
по [Microsoft](https://github.com/microsoft)

> Узнайте все что нужно знать, чтобы приступить к работе с AJAX Control Toolkit.


AJAX Control Toolkit содержит более 30 бесплатные элементы управления, которые можно использовать в приложениях ASP.NET. В этом руководстве вы узнаете, как скачать AJAX Control Toolkit и добавьте в набор средств элементы управления панели инструментов Visual Studio или Visual Web Developer Express.

## <a name="downloading-the-ajax-control-toolkit"></a>Загрузка AJAX Control Toolkit

[AJAX Control Toolkit](http://devexpress.com/act) — это проект с открытым исходным кодом, разработанная компанией членами сообщества ASP.NET и группа разработчиков ASP.NET.


[![Загрузка AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-vb/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image1.png)

**Рис 01**: загрузка AJAX Control Toolkit ([Просмотр полноразмерного изображения](get-started-with-the-ajax-control-toolkit-vb/_static/image2.png))


После загрузки файла, необходимо разблокировать файл. Щелкните правой кнопкой мыши файл, выберите свойства и нажмите кнопку **Unblock** кнопку (см. рис. 2).


[![Разблокировка файла ZIP-набор средств управления AJAX](get-started-with-the-ajax-control-toolkit-vb/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image3.png)

**Рис. 02**: разблокирования файла ZIP-набор средств управления AJAX ([Просмотр полноразмерного изображения](get-started-with-the-ajax-control-toolkit-vb/_static/image4.png))


После разблокирования файла можно было распаковать файл: щелкните правой кнопкой мыши файл и выберите **извлечь все** пункт меню. Теперь мы готовы для добавления в набор средств для панели элементов Visual Studio или Visual Web Developer.

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a>Добавление на панель инструментов AJAX Control Toolkit

— Это самый простой способ использования AJAX Control Toolkit, добавляемый в набор средств панели инструментов Visual Studio или Visual Web Developer (см. рис. 3). Таким образом, можно просто перетащить элемент управления набора средств на страницу, если вы хотите использовать его.


[![AJAX Control Toolkit отображается в панели элементов](get-started-with-the-ajax-control-toolkit-vb/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image5.png)

**Рис 03**: AJAX Control Toolkit отображается в панели элементов ([Просмотр полноразмерного изображения](get-started-with-the-ajax-control-toolkit-vb/_static/image6.png))


Во-первых необходимо добавить на панель инструментов с вкладкой AJAX Control Toolkit. Выполните следующие действия.

1. Создайте новый веб-сайт ASP.NET, выбрав пункт меню файл, создать веб-сайта. Дважды щелкните файл Default.aspx в окне обозревателя решений, чтобы открыть файл в редакторе.
2. Щелкните правой кнопкой мыши панель под вкладка "Общие" и выберите пункт меню **добавить вкладку** (см. рис. 4).
3. Введите новую вкладку, называемую AJAX Control Toolkit.


[![Добавить новую вкладку](get-started-with-the-ajax-control-toolkit-vb/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image7.png)

**Рис. 04**: добавить новую вкладку ([Просмотр полноразмерного изображения](get-started-with-the-ajax-control-toolkit-vb/_static/image8.png))


Далее необходимо добавить элементы управления AJAX Control Toolkit на новой вкладке. Выполните следующие действия.

- Щелкните правой кнопкой мыши под вкладке AJAX Control Toolkit и выбрать пункт меню **Выбор элементов (см. рис. 5)**.
- Перейдите в расположение, где вы распаковали AJAX Control Toolkit и выберите сборку файл AjaxControlToolkit.dll.


[![Выберите элементы для добавления на панель элементов](get-started-with-the-ajax-control-toolkit-vb/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image9.png)

**05 рис**: Выбор элементов для добавления на панель инструментов ([Просмотр полноразмерного изображения](get-started-with-the-ajax-control-toolkit-vb/_static/image10.png))


После выполнения этих действий все элементы набора средств управления будут отображаться в вашем инструментарии.

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a>Обновление до новой версии набора средств

Если вы использовали более ранняя версия набора средств и теперь нужно переместить в более поздней версии приведены рекомендуемые действия:

- Двоичные файлы - удалить старую версию файл AjaxControlToolkit.dll сборки из папки Bin веб-сайта.
- Элементы панели элементов - удалить вкладку AJAX Control Toolkit и выполните действия, чтобы повторно создать на вкладке с новой версией сборки, файл AjaxControlToolkit.dll.

> [!div class="step-by-step"]
> [Назад](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)
> [Вперед](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)
