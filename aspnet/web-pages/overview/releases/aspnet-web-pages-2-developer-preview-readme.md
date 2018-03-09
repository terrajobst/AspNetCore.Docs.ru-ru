---
uid: web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
title: "ASP.NET Web Pages 2 разработчика Предварительный просмотр сведений | Документы Microsoft"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/14/2011
ms.topic: article
ms.assetid: 159a92e2-e011-4da7-b61d-2edde2a967da
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
msc.type: authoredcontent
ms.openlocfilehash: 119265c62abb3f3110cdc7f0b94a7c9b16b4251c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-pages-2-developer-preview-readme"></a>Веб-страницы ASP.NET 2 Developer Preview ReadMe
====================
по [Microsoft](https://github.com/microsoft)

## <a name="aspnet-web-pages-2-developer-preview-readme"></a>Веб-страницы ASP.NET 2 Developer Preview ReadMe

14 сентября 2011 г.

### <a name="contents"></a>Описание

#### <a id="_Toc303701284"></a>Замечания по установке

Чтобы установить веб-страницы 2 Developer Preview, доступны следующие параметры.

- Установка с помощью бета-версии 2 WebMatrix [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=226883). WebMatrix — это набор инструментов бесплатные веб-разработки, включающий веб-страниц ASP.NET. Дополнительные сведения см. в разделе установки в [верхней функции в ASP.NET Web Pages 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824).

- Установить веб-страницы 2 Developer Preview непосредственно с помощью [ссылка для загрузки](https://go.microsoft.com/fwlink/?LinkID=226335). Используйте этот подход, если вы хотите создавать приложения веб-страницы, используя текстовый редактор, например Блокнот. Для запуска приложения 2 веб-страницы, необходимо иметь IIS Express 7.5. (Это включается автоматически с помощью WebMatrix.) Советы по тестированию веб-страница с помощью IIS Express, см. в разделе боковой панели «Создание и тестирование ASP.NET страниц с помощью ваш собственный текстовый редактор» в [начало работы с WebMatrix и ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202889).

Веб-страниц ASP.NET 2 Developer Preview можно установить и выполнять side-by-side с 1 веб-страниц ASP.NET. <a id="a"></a>Дополнительные сведения см. подраздел «Запущена веб-страниц приложений-параллельных» в [верхней функции в состоянии предварительной версии Web Pages 2](https://go.microsoft.com/fwlink/?LinkID=227824).

#### <a id="_Toc303701285"></a>Документация

Учебники и другие сведения о веб-страницы ASP.NET доступны на веб-страницы ASP.NET веб-сайта ([https://www.asp.net/web-pages/](../../index.md)). Сведения о новых возможностях и улучшениях в 2 веб-страниц см. в разделе [верхней функции в состоянии предварительной версии Web Pages 2](https://go.microsoft.com/fwlink/?LinkID=227824).

#### <a id="_Toc303701286"></a>Поддержка

<a id="_Toc209852135"></a><a id="_Toc255833657"></a>Это является предварительной и официально не поддерживается. При наличии вопросов о работе в этом выпуске, задайте их на форуме ASP.NET Web Pages ([https://forums.asp.net/1224.aspx/1?WebMatrix](https://forums.asp.net/1224.aspx/1?WebMatrix) ), где члены сообщества ASP.NET друг другу, предоставляя полезные сведения.

#### <a id="_Toc303701287"></a>Требования к программному обеспечению

Веб-страницы ASP.NET 2 требуется .NET Framework 4. Она также работает с версии .NET Framework 4.5 Developer Preview.

<a id="_Toc303701288"></a><a id="_Breaking_Changes"></a>

#### <a name="fixes-known-issues-and-breaking-changes"></a>Исправления, известных проблем и критические изменения

<a id="_Toc224729061"></a><a id="_Toc238051347"></a>

- **—\* Методов (например, IsDateTime) теперь возвращают правильные значения для всех языков и региональных параметров.** Некоторые методы, например *IsDateTime* вернул *false* при они должны возвращены *true* так, как они ранее Проверка конкретного языка и региональных параметров. Эти методы будут исправлены для теперь учитывают язык и региональные параметры. Это критическое изменение; Если приложение использует старое поведение, будет нарушено.
- **Изменилось поведение метода Href.** Ранее вызов Href("~/SomeFile") возвращает URL-адрес относительно текущего файла. Теперь Href("~/SomeFile") всегда возвращает абсолютный путь от корня приложения. В большинстве случаев такое поведение не провести различие в возвращаемом значении. Это изменение было внесено для устранения некоторых сценариев Ajax. Например рассмотрим следующий пример кода: 

    [!code-cshtml[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample1.cshtml)]

    Этот код ранее разрешится Images/Logo.jpg, который будет неверным для Ajax-запросом к этой странице. Теперь он будет преобразован корневой (/ MySite/Images/Logo.jpg).
- **Метод HttpContext.RedirectLocal был изменен**. Теперь этот метод принимает только URL-адреса, относительно текущего приложения. Полный URL-адреса, будут отклонены.
- **Метод ModelState.IsValid теперь необходимо сначала вызвать Validate**. Если данные преобразуются в приложения для использования новых методов проверки входных данных и вызова *ModelState.IsValid* метод, необходимо вызвать *Validation.Validate* заранее. Например теперь должны иметь следующий вид. 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample2.cs)]

 Однако рекомендуется, если использовать новые методы проверки входных данных, не используйте *ModelState.IsValid*. Вместо этого структурировать код следующим образом: 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample3.cs)]
- **Проверка на стороне клиента не работают в Internet Explorer 7 и Internet Explorer 8,**. Проверка на стороне клиента не работает из-за несовместимости с jQuery 1.6.2, входящий в состав шаблона проекта по умолчанию. (Работает проверки на стороне сервера.).

#### <a id="_Toc303701289"></a>Отказ от ответственности

© Корпорация Майкрософт, 2011. Все права защищены. Данный документ предоставляется «как-—.» Сведения и мнения, содержащиеся в этом документе, включая URL-адреса и ссылки на другие веб-сайта, могут изменяться без предварительного уведомления. Вы принимаете на себя весь риск, связанный с его использованием.
