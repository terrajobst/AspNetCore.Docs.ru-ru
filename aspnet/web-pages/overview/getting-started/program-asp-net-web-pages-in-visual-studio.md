---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: Программирование веб-страниц ASP.NET (Razor) с помощью Visual Studio | Документы Microsoft
author: tfitzmac
description: В этом приложении объясняется, как можно использовать Visual Studio 2010 или Visual Web Developer 2010 Express программу веб-страниц ASP.NET с синтаксисом Razor.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2014
ms.topic: article
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: eb17c8cc1fab5b552c8495e74bb86ae9dbc5b972
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30896590"
---
<a name="programming-aspnet-web-pages-razor-using-visual-studio"></a>Программирование веб-страниц ASP.NET (Razor) с помощью Visual Studio
====================
по [Tom FitzMacken](https://github.com/tfitzmac)

> В этой статье объясняется, как можно использовать Visual Studio или Visual Web Developer Express программу веб-сайтов ASP.NET Web Pages (Razor).
> 
> Вы узнаете
> 
> - Что необходимо для установки (при ее наличии) для работы с веб-страниц ASP.NET в вашей версии Visual Studio.
> - Как добавить поддержку для веб-страниц ASP.NET в Visual Web Developer 2010 Express.
> - Способы использования возможностей в Visual Studio для работы с страниц ASP.NET Razor, включая IntelliSense и отладчик.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемая в этом учебнике
> 
> 
> - Веб-страниц ASP.NET (Razor) 3
> - Visual Studio 2013
> - WebMatrix 3
>   
> 
> Этот учебник также работает с 2 веб-страниц ASP.NET, Visual Studio 2012, Visual Studio 2010 и WebMatrix 2.


Можно создавать веб-страницы ASP.NET с синтаксисом Razor с помощью WebMatrix или другие редакторы кода. Также можно использовать Microsoft Visual Studio, которая является полнофункциональным интегрированная среда разработки (IDE), предоставляет мощный набор средств для создания многих типов приложений (не только веб-сайтов). Для работы с ASP.NET Razor страниц, можно установить несколько полных выпусках Visual Studio либо бесплатный [Visual Studio Express для Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express) выпуска.

Две функции особенно полезна, предоставляемых Visual Studio для программирования веб-страниц ASP.NET Razor

- *IntelliSense*. Функции IntelliSense, встроенные в Visual Studio является мощным, чем технология IntelliSense в WebMatrix.
- *Debugger*. Отладчик позволяет устранить путем остановки программы, пока он работает, проверки переменных и пошаговое выполнение кода по одной строке кода.

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a>С помощью Visual Studio с разными версиями веб-страниц ASP.NET

Visual Studio 2012 и Visual Studio 2013, включают поддержку для веб-страниц ASP.NET. (Пакеты, которые требуются для поддержки веб-страниц ASP.NET устанавливаются при установке Visual Studio.)

Visual Studio 2010 не поддерживает по умолчанию для веб-страниц ASP.NET. Чтобы использовать веб-страниц ASP.NET с помощью Visual Studio 2010, необходимо установить пакет ASP.NET MVC. Для доступа к веб-страницы ASP.NET 2 установите ASP.NET MVC 4.

В следующей таблице перечислены поддержка для веб-страниц ASP.NET в различных версиях Visual Studio.

|  | Visual Studio 2010 | Visual Studio 2012 | Visual Studio 2013 |
| --- | --- | --- | --- |
| **Веб-страницы ASP.NET 2** | Установка ASP.NET MVC 4 | (Включено) | (Включено) |
| **Веб-страницы ASP.NET 3** |  | Обновление для ASP.NET Web Pages 3 с помощью NuGet | (Включено) |

Для работы с Visual Studio 2010, в разделе [Установка поддержки для веб-страниц ASP.NET в Visual Studio 2010](#vs2010support).

## <a name="launching-visual-studio-from-webmatrix"></a>При запуске Visual Studio из WebMatrix

Если вы запустили проект в WebMatrix и переключитесь в Visual Studio, WebMatrix содержит кнопку, чтобы легко открыть проект в Visual Studio. Необходимо иметь Visual Studio, установленной на компьютере для этой кнопки должно быть включено. На следующем изображении показана кнопки в WebMatrix.

![Запустите Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

При нажатии кнопки проект открыт в Visual Studio. Вы можно переключаться между WebMatrix и Visual Studio без проблем. Вы получите уведомление, если все файлы были изменены в другой среде и необходимо перезагрузить для получения последних изменений.

## <a name="creating-aspnet-razor-site-in-visual-studio"></a>Создание сайта ASP.NET Razor в Visual Studio

Чтобы создать на веб-сайт ASP.NET Razor в Visual Studio:

1. Запустите Visual Studio или Visual Web Developer.
2. В **файл** меню, нажмите кнопку **новый веб-сайт**.

    ![Создание нового веб-сайта](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. В **новый веб-сайт** диалогового окна выберите используемый язык (Visual C# или Visual Basic).
4. Выберите **веб-сайт ASP.NET (Razor)** шаблона.

    ![узел Razor](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. Нажмите кнопку **ОК**.

Новый проект существует и не будет заполнен некоторые веб-страницы по умолчанию, чтобы помочь вам приступить к работе.

### <a name="using-intellisense"></a>Using IntelliSense

После создания сайта, вы увидите, как работает IntelliSense в Visual Studio.

1. Откройте в веб-сайт, вы только что создали, *Default.cshtml* страницы.
2. После `<h3>` теги на странице введите `@ServerInfo.` (включая точку). Обратите внимание на то, как IntelliSense отображает доступные методы `ServerInfo` вспомогательный в раскрывающемся списке. 

    ![intellisense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. Выберите `GetHtml` метод из списка и нажмите клавишу ВВОД. IntelliSense автоматически заполняет метод. (Как с любым методом в C# необходимо добавить `()` символов после метода.)  
   Полный код `GetHtml` метод выглядит как в следующем примере:  

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. Нажмите клавиши Ctrl + F5, чтобы запустить страницу. Это выглядит страницы при отображению в браузере: 

    ![страница по умолчанию в браузере](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. Закройте браузер.

### <a name="using-the-debugger"></a>С помощью отладчика

1. В верхней части *Default.cshtml* страницы после строки, которая начинается с `Page.Title`, добавьте следующую строку кода: 

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. В сером поле редактирования слева от кода, нажмите кнопку рядом с этой новой строке для добавления *останова*. Точка останова — маркер, который указывает отладчику, что для остановки на этом этапе работы программы, чтобы можно было видеть, что происходит.

    ![Набор точек останова](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. Удалите вызов `ServerInfo.GetHtml` метода и добавьте вызов `@myTime` переменной на его месте. Этот вызов отображает текущее значение времени, возвращенный новую строку кода.
4. Нажмите клавишу F5 для запуска страницы в отладчике. Страница останавливается на заданной точке останова. На следующем рисунке, как страница выглядит в редакторе с точкой останова (желтым цветом). 

    ![точки останова для отладки](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. В панели инструментов отладки нажмите **шаг с заходом** (или клавишу F11) для запуска следующей строке кода. Каждый раз при нажатии этой кнопки переходе выполнение на следующей строке кода.

    ![Шаг с заходом в кнопку](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. Проверьте значение `myTime` переменных, удерживая указатель мыши над ним или путем проверки значения, отображаемые в **локальные** и **стек вызовов** windows. Visual Studio отображает значение переменной.

    ![значение отображает время](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. После завершения проверки переменной и пошаговое выполнение кода, нажмите клавишу F5, чтобы продолжить выполнение страницы без остановки в каждой строке. После завершения всех код пошагово, браузер отображает страницу.

Дополнительные сведения об отладчике и о том, как выполнить отладку кода в Visual Studio см. в разделе [Пошаговое руководство: отладка веб-страницы в Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a>С помощью Razor в проектах ASP.NET MVC с помощью Visual Studio

Синтаксис Razor также широко используется в проектах ASP.NET MVC. MVC — эффективный, основанный на шаблонах способ создания динамических веб-сайтов. Если веб-узла ASP.NET Web Pages становится трудно поддерживать, может потребоваться следует преобразовать его в приложение ASP.NET MVC. Пример создания приложения MVC см. в разделе [Приступая к работе с ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a>Установка поддержки веб-страниц ASP.NET в Visual Studio 2010

В этом разделе показано, как установить Visual Web Developer Express 2010 и средства веб-страниц ASP.NET для Visual Studio.

1. Если у вас еще нет установщика веб-платформы, загрузите его со следующего URL:

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. Запустите установщик веб-платформы.
3. Нажмите кнопку **продуктов** вкладки.

    ![Вкладка продуктов установщика веб-платформы](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. Поиск **ASP.NET MVC 4** (для ASP.NET Web Pages 2) и нажмите кнопку **добавить**. Эти продукты включают средства Visual Studio для построения веб-сайтов ASP.NET Razor.

    ![Параметры установки установщика веб-платформы](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. Нажмите кнопку **установить** для завершения установки.
