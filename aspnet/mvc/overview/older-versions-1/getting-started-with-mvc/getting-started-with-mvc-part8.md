---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Добавление столбца в модель | Документы Microsoft
author: shanselman
description: Это руководство для новичков, в котором представлены основные сведения по ASP.NET MVC. Создание простого веб-приложения, чтение и запись из базы данных.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 7a0b64ce00fc5ee6d49990f1d4d93a154c467bf5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/10/2018
---
<a name="adding-a-column-to-the-model"></a>Добавление столбца модели
====================
по [Скотт Хансельман](https://github.com/shanselman)

> Это руководство для новичков, в котором представлены основные сведения по ASP.NET MVC. Вы создадите простое веб-приложение выполняет чтение и запись из базы данных. Посетите [центр обучения ASP.NET MVC](../../../index.md) для поиска других ASP.NET MVC, учебники и примеры.


В этом разделе мы собираемся показывает, как можно внести изменения в схему базе данных и обработки изменений в приложении.

Давайте добавим столбца «Оценка» таблицы фильма. Вернитесь в интегрированную среду разработки и выберите в обозревателе базы данных. Щелкните правой кнопкой мыши таблицу фильма и выберите Открыть определение таблицы.

Добавьте столбец «Оценка», как показано ниже. Поскольку мы не имеют любые оценки, столбец можно разрешить значения NULL. Нажмите кнопку Сохранить.

[![Изменение таблицы фильмов](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)

Затем вернуться в обозревателе решений и откройте файл Movies.edmx (который находится в папке \Models). Щелкните правой кнопкой мыши в области конструктора (белая область) и выберите модель обновление из базы данных.

[![Фильмы - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)

Это приведет к запуску «Мастера обновления». Перейдите на вкладку обновления внутри нее и нажмите "Готово". Класс модели нашей фильма обновляются с помощью нового столбца.

![Мастер обновления (2)](getting-started-with-mvc-part8/_static/image5.png)

После нажатия кнопки Готово, вы увидите, что в нашей модели сущности фильма добавлен новый столбец оценки.

[![Сущности фильма](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)

Мы добавили столбец в модели базы данных, но представления не знаете о нем.

## <a name="update-views-with-model-changes"></a>Обновление представлений с изменениями в модели

Корпорация Майкрософт может обновлять наши шаблоны представления для отражения нового столбца оценки несколькими способами. Поскольку мы создали эти представления путем создания их через диалоговое окно добавления представления, мы их удалить и повторно создать их повторно. Однако обычно людей будет уже были внесены изменения в свои шаблоны представления из первоначального формирования формирования шаблонов и требуется добавить или удалить поля вручную, так же, как это делалось с поля «идентификатор» для создания.

Откройте шаблон \Views\Movies\Index.aspx и добавьте &lt;й&gt;Оценка&lt;/th&gt; на заголовок таблицы фильма. Я добавил мое после жанра. В той же позиции столбца, но ниже, добавьте строку для вывода нашей новой оценки.

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

Наш окончательного Index.aspx шаблона будет выглядеть следующим образом:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

Давайте затем откройте шаблон \Views\Movies\Create.aspx и добавить метку и текстовое поле для наших новое свойство оценки:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

Нашей окончательного шаблон Create.aspx будет выглядеть следующим образом и изменим наш обозреватель заголовок и вторичный &lt;h2&gt; заголовка примерно в «Создать фильма «пока мы здесь!

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

Запуск приложения, и теперь у вас есть новое поле в базе данных, который добавляется на страницу создания. Добавление нового фильма - это время с оценку - и нажмите кнопку Создать.

[![Создание фильма - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)

Нажмите кнопку "Создать", вы являетесь отправлена страницы индекса там, где вы новые фильма указана с нового столбца оценки в базе данных

[![Список фильмов - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)

Это основам получен работы, делая контроллеров, связывая их с представлениями и передачи данных жестко. Затем мы создан и предназначенных базы данных и поместить некоторые данные в. Мы извлекает данные из базы данных и он отображается в HTML-таблицу. Затем мы добавили создать форму, которая позволяют добавлять данные в базу данных сами из веб-приложения. Мы добавлены проверки, а затем сделать проверки используется JavaScript на стороне клиента. Наконец мы изменил базу данных, чтобы включить новый столбец данных, а затем обновлены наши две страницы для создания и отображения новых данных.

Читателю теперь можно переходить к обучении промежуточного уровня «[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)», а также многие видеоролики и ресурсы на [ https://asp.net/mvc ](https://asp.net/mvc) Чтобы получить дополнительные сведения о ASP.NET MVC!

Желаем удачи!

- Скотт Хансельман - [ http://hanselman.com ](http://hanselman.com) и [ @shanselman ](http://twitter.com/shanselman) в Twitter.

> [!div class="step-by-step"]
> [Назад](getting-started-with-mvc-part7.md)
