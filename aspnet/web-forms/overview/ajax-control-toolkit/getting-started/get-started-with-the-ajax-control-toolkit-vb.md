---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
title: Приступая к работе с элементов управления AJAX (VB) | Документы Microsoft
author: microsoft
description: Дополнительные сведения, вам нужно знать, чтобы приступить к использованию элементов управления AJAX.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 9f8fa166-49a2-402c-b236-20caef0c658f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
msc.type: authoredcontent
ms.openlocfilehash: 30653a147bd3bf581af27220e11cdecc2f89fc4a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="get-started-with-the-ajax-control-toolkit-vb"></a>Приступая к работе с элементов управления AJAX (Visual Basic)
====================
по [Microsoft](https://github.com/microsoft)

> Дополнительные сведения, вам нужно знать, чтобы приступить к использованию элементов управления AJAX.


Набор элементов управления AJAX содержит более 30 бесплатные элементы управления, которые можно использовать в приложениях ASP.NET. В этом учебнике вы узнаете, как для загрузки элементов управления AJAX и набора элементов добавлять к панели инструментов Visual Studio или Visual Web Developer Express.

## <a name="downloading-the-ajax-control-toolkit"></a>Загрузка элементов управления AJAX

[Элементов управления AJAX](http://devexpress.com/act) членами сообщества ASP.NET и командой разработчиков ASP.NET разработки проекта с открытым кодом.


[![Загрузка элементов управления AJAX](get-started-with-the-ajax-control-toolkit-vb/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image1.png)

**На рисунке 01**: Загрузка элементов управления AJAX ([Просмотр полноразмерное изображение](get-started-with-the-ajax-control-toolkit-vb/_static/image2.png))


После загрузки этого файла требуется разблокировать файл. Щелкните правой кнопкой мыши файл, выберите свойства и нажмите кнопку **Unblock** кнопку (см. рис. 2).


[![Разблокировать файл ZIP набор средств управления AJAX](get-started-with-the-ajax-control-toolkit-vb/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image3.png)

**На рисунке 02**: разблокировать набор средств управления AJAX ZIP-файл ([Просмотр полноразмерное изображение](get-started-with-the-ajax-control-toolkit-vb/_static/image4.png))


После разблокирования файла можно было распаковать файл: щелкните правой кнопкой мыши файл и выберите **извлечь все** пункт меню. Теперь мы все готово для добавления в набор инструментов в область элементов Visual Studio или Visual Web Developer.

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a>Добавление элементов управления AJAX в панель элементов

Для использования элементов управления AJAX проще всего добавить набор средств панели инструментов Visual Studio или Visual Web Developer (см. рис. 3). Таким образом, можно просто перетащить элемент набора средств управления на страницу, если вы хотите использовать его.


[![Набор элементов управления AJAX отображается в панели элементов](get-started-with-the-ajax-control-toolkit-vb/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image5.png)

**На рисунке 03**: набор элементов управления AJAX отображается в панели элементов ([Просмотр полноразмерное изображение](get-started-with-the-ajax-control-toolkit-vb/_static/image6.png))


Во-первых необходимо добавить набор элементов управления AJAX вкладку области элементов. Выполните следующие действия.

1. Создайте новый веб-сайт ASP.NET, выбрав параметр меню файл, создать веб-сайта. Дважды щелкните файл Default.aspx в окне обозревателя решений, чтобы открыть файл в редакторе.
2. Щелкните правой кнопкой мыши область элементов под вкладка «Общие» и выберите пункт меню в **добавить вкладку** (см. рис. 4).
3. Введите новую вкладку с именем набора элементов управления AJAX.


[![Добавление новой вкладки](get-started-with-the-ajax-control-toolkit-vb/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image7.png)

**На рисунке 04**: Добавление новой вкладки ([Просмотр полноразмерное изображение](get-started-with-the-ajax-control-toolkit-vb/_static/image8.png))


Далее необходимо добавить элементы управления, элементов управления AJAX на новой вкладке. Выполните следующие действия.

- Щелкните правой кнопкой мыши под вкладки элементов управления AJAX и выбрать пункт меню **Выбор элементов (см. рис. 5)**.
- Перейдите к расположению, где распаковал элементов управления AJAX и выберите файл AjaxControlToolkit.dll сборку.


[![Выберите элементы для добавления в область элементов](get-started-with-the-ajax-control-toolkit-vb/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image9.png)

**На рисунке 05**: выберите элементы для добавления в область элементов ([Просмотр полноразмерное изображение](get-started-with-the-ajax-control-toolkit-vb/_static/image10.png))


После выполнения этих шагов для всех элементов набора средств управления будут отображаться на панели элементов.

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a>Обновление до новой версии набора средств

Если вы использовали более ранней версии набора средств и теперь необходимо переместить в более поздней версии приведены рекомендуемые действия.

- Двоичные файлы - удалить старую версию файл AjaxControlToolkit.dll сборки из папки Bin веб-сайта.
- Элементы панели элементов - удаление вкладки элементов управления AJAX и выполните действия, перечисленные выше, чтобы повторно создать вкладку с новой версией сборки, файл AjaxControlToolkit.dll.

> [!div class="step-by-step"]
> [Назад](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)
> [Вперед](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)
