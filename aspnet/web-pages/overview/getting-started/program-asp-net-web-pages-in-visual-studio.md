---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: Программирование веб-страниц ASP.NET (Razor) с помощью Visual Studio | Документация Майкрософт
author: Rick-Anderson
description: В этом приложении объясняется, как можно использовать Visual Studio 2010 или Visual Web Developer 2010 Express программирование веб-страниц ASP.NET с использованием синтаксиса Razor.
ms.author: riande
ms.date: 02/13/2014
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 5b8df17ec1021d133579e23cb4f5b0d0f67d4c7c
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020459"
---
<a name="programming-aspnet-web-pages-razor-using-visual-studio"></a>Программирование веб-страниц ASP.NET (Razor) с помощью Visual Studio
====================
по [Tom FitzMacken](https://github.com/tfitzmac)

> В этой статье объясняется, как можно использовать Visual Studio или Visual Web Developer Express программу веб-сайтов ASP.NET Web Pages (Razor).
>
> Вы узнаете, как
>
> - Что необходимо для установки (при ее наличии) для работы с веб-страниц ASP.NET в вашей версии Visual Studio.
> - Как добавить поддержку для веб-страниц ASP.NET в Visual Web Developer 2010 Express.
> - Как использовать функции в Visual Studio для работы с ASP.NET Razor pages, включая IntelliSense и отладчик.
>
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемые в этом руководстве
>
>
> - Веб-страниц ASP.NET (Razor) 3
> - Visual Studio 2013
> - WebMatrix 3
>
>
> Этот учебник также работает с веб-страниц ASP.NET 2, Visual Studio 2012, Visual Studio 2010 и WebMatrix 2.


Можно программировать веб-страницы ASP.NET с синтаксисом Razor с помощью WebMatrix или других редакторов кода. Также можно использовать Microsoft Visual Studio, который является это полнофункциональная интегрированная среда разработки (IDE), предоставляет мощный набор средств для создания различных типов приложений (не только веб-сайтов). Для работы с ASP.NET Razor pages, можно использовать [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).

Ниже приведены два особенно полезных возможностей, предоставляемых Visual Studio для программирования с использованием веб-страниц ASP.NET Razor.

- *IntelliSense*. Функция IntelliSense, встроенные в Visual Studio осуществляется в большем, чем IntelliSense в WebMatrix.
- *Отладчик*. Отладчик позволяет устранять кода, остановки программы во время его выполнения, изучении состояния переменных и пошаговое выполнение кода по одной строке.

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a>Использование Visual Studio с разными версиями веб-страниц ASP.NET

Для разработки веб-приложений ASP.NET в Visual Studio 2017 следует установить **ASP.NET и веб-разработка** рабочей нагрузки.

Visual Studio 2012 и Visual Studio 2013 включают поддержку для веб-страниц ASP.NET. (При установке Visual Studio будут установлены пакеты, которые требуются для поддержки веб-страниц ASP.NET.)

Visual Studio 2010 не поддерживает по умолчанию для веб-страниц ASP.NET. Чтобы использовать веб-страниц ASP.NET с помощью Visual Studio 2010, необходимо установить пакет ASP.NET MVC. Чтобы получить ASP.NET Web Pages 2, необходимо установить ASP.NET MVC 4.

В следующей таблице перечислены поддержка для веб-страниц ASP.NET в разных версиях Visual Studio.

|  | Visual Studio 2010 | Visual Studio 2012 | Visual Studio 2013 |
| --- | --- | --- | --- |
| **ASP.NET Web Pages 2** | Установка ASP.NET MVC 4 | (Включено) | (Включено) |
| **Веб-страницы ASP.NET 3** |  | Обновление ASP.NET Web Pages 3 с помощью NuGet | (Включено) |

Для работы с Visual Studio 2010, см. в разделе [Установка поддержки для веб-страниц ASP.NET в Visual Studio 2010](#vs2010support).

## <a name="launching-visual-studio-from-webmatrix"></a>Запустите среду Visual Studio из WebMatrix

Если вы запустили проект в WebMatrix и переключитесь в Visual Studio, WebMatrix предоставляет кнопку, чтобы легко открыть проект в Visual Studio. Необходимо установить Visual Studio, установленной на компьютере для данной кнопки, включить. На следующем рисунке кнопки в WebMatrix.

![Запустите Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

При нажатии кнопки проект открыт в Visual Studio. Можно переключиться назад и вперед WebMatrix и Visual Studio без проблем. Вы получите уведомление, если файлы были изменены в другой среде, необходимо перезагрузить для получения последних изменений.

## <a name="creating-aspnet-razor-site-in-visual-studio"></a>Создание сайта ASP.NET Razor в Visual Studio

Чтобы создать на веб-сайте ASP.NET Razor в Visual Studio:

1. Запустите Visual Studio.
2. В **файл** меню, щелкните **новый веб-сайт**.

    ![Создание нового веб-сайта](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. В **новый веб-сайт** диалоговом окне выберите используемый язык (Visual C# или Visual Basic).
4. Выберите **веб-сайт ASP.NET (Razor)** шаблона.

    ![узел Razor](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. Нажмите кнопку **ОК**.

Проект существует и содержит некоторые веб-страницы по умолчанию, чтобы помочь вам приступить к работе.

### <a name="using-intellisense"></a>Using IntelliSense

Теперь, когда вы создали узел, вы увидите сообщение о том, как работает технология IntelliSense в Visual Studio.

1. Откройте в веб-сайт, вы только что создали, *Default.cshtml* страницы.
2. После `<h3>` введите теги в странице `@ServerInfo.` (включая точку). Функция IntelliSense покажет доступные методы для `ServerInfo` вспомогательная функция в раскрывающемся списке.

    ![IntelliSense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. Выберите `GetHtml` метод в списке и нажмите клавишу ВВОД. IntelliSense автоматически заполняет метод. (Как с помощью любого метода в C#, необходимо добавить `()` символов после метода.) Полный код для `GetHtml` метод выглядит как в следующем примере:

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. Нажмите клавиши Ctrl + F5, чтобы запустить эту страницу. Это выглядит страницы при отображению в браузере:

    ![страница по умолчанию в браузере](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. Закройте браузер.

### <a name="using-the-debugger"></a>С помощью отладчика

1. В верхней части *Default.cshtml* страницы после строки, которая начинается с `Page.Title`, добавьте следующую строку кода:

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. В сером поле редактора слева от кода, щелкните рядом с этой новой строке, чтобы добавить *точки останова*. Точка останова — маркер, который указывает отладчику, чтобы остановить выполнение программы на этом этапе, чтобы можно было видеть, что происходит.

    ![Задание точки останова](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. Удалите вызов `ServerInfo.GetHtml` метод и добавьте вызов `@myTime` переменной на его месте. Этот вызов отображает текущее значение времени, который возвращается новая строка кода.
4. Нажмите клавишу F5, чтобы запустить эту страницу в отладчике. Страницы останавливается в заданной точке останова. Ниже представлен результат страницы в редакторе с точкой останова (желтым цветом).

    ![точки останова для отладки](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. На панели инструментов отладки, нажмите кнопку **шаг с заходом** (или клавишу F11) для запуска следующей строке кода. Каждый раз при нажатии этой кнопки выполнения вы перейдите на следующую строку кода.

    ![Шаг с заходом в кнопку](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. Проверьте значение `myTime` переменных, удерживая указатель мыши над ним или путем проверки значения, отображаемые в **"Локальные"** и **стек вызовов** windows. Visual Studio отображает значение переменной.

    ![значение отображает время](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. После завершения проверки переменной и пошаговое выполнение кода, нажмите клавишу F5, чтобы продолжить выполнение страницы без остановки в каждой строке. После завершения всех код пошагово, браузер отображает страницу.

Дополнительные сведения об отладчике и о том, как выполнять отладку кода в Visual Studio, см. в разделе [Пошаговое руководство: отладка веб-страниц в Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a>С помощью Razor в проектах ASP.NET MVC с помощью Visual Studio

Синтаксис Razor также широко используется в проектах ASP.NET MVC. MVC — это эффективный, основанный на шаблонах способ создания динамических веб-сайтов. Если веб-узла ASP.NET Web Pages становится трудно поддерживать, может потребоваться его следует преобразовать его в приложение ASP.NET MVC. Пример создания приложения MVC, см. в разделе [Приступая к работе с ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a>Установка поддержки веб-страниц ASP.NET в Visual Studio 2010

В этом разделе показано, как установить Visual Web Developer Express 2010 и средства веб-страниц ASP.NET для Visual Studio.

1. Если у вас еще нет установщика веб-платформы, загрузите его со следующего URL:

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. Запустите установщик веб-платформы.
3. Нажмите кнопку **продуктов** вкладки.

    ![Вкладка продуктов установщика веб-платформы](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. Поиск **ASP.NET MVC 4** (для ASP.NET Web Pages 2) и нажмите кнопку **добавить**. Эти продукты включают средства Visual Studio для создания веб-сайтов ASP.NET Razor.

    ![Параметры установки установщика веб-платформы](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. Нажмите кнопку **установить** для завершения установки.
