---
uid: web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
title: ASP.NET Web Pages 2 Developer Preview ReadMe | Документация Майкрософт
author: microsoft
description: ''
ms.author: riande
ms.date: 09/14/2011
ms.assetid: 159a92e2-e011-4da7-b61d-2edde2a967da
msc.legacyurl: /web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
msc.type: authoredcontent
ms.openlocfilehash: 93e3f9c9d7c90f1ebfd9f482166aeb833cae73e9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837289"
---
<a name="aspnet-web-pages-2-developer-preview-readme"></a>ASP.NET Web Pages 2 Developer Preview ReadMe
====================
по [Microsoft](https://github.com/microsoft)

## <a name="aspnet-web-pages-2-developer-preview-readme"></a>ASP.NET Web Pages 2 Developer Preview ReadMe

14 сентября 2011 г.

### <a name="contents"></a>Описание

#### <a id="_Toc303701284"></a>  Замечания по установке

Чтобы установить веб-страниц 2 Developer Preview, доступны следующие варианты:

- Установка с помощью бета-версии 2 WebMatrix [установщика веб-платформы](https://go.microsoft.com/fwlink/?LinkId=226883). WebMatrix — это набор бесплатного средства разработки, которая включает веб-страниц ASP.NET. Дополнительные сведения см. в разделе установки [лучшие возможности в ASP.NET Web Pages 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824).

- Установка веб-страниц 2 Developer Preview напрямую с помощью [ссылка для скачивания](https://go.microsoft.com/fwlink/?LinkID=226335). Используйте этот подход, если вы хотите создавать приложения веб-страниц, с помощью текстового редактора, например в блокноте. Чтобы запустить приложения Web Pages 2, необходимо иметь IIS Express 7.5. (Это входит в состав автоматически WebMatrix.) Советы по тестированию на странице веб-страниц, с помощью IIS Express, см. боковую панель «Создание и тестирование ASP.NET страницы с помощью вашей собственной текстовый редактор» в [начало работы с WebMatrix и ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202889).

ASP.NET Web Pages 2 Developer Preview можно установить и можно запустить side-by-side с ASP.NET Web Pages 1. <a id="a"></a>Дополнительные сведения см. в разделе «Запуск веб-страниц приложений Side-by-Side» в [лучшие возможности веб-страниц 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824).

#### <a id="_Toc303701285"></a>  Документация по

На странице веб-страниц, веб-сайта ASP.NET доступны руководства и другие сведения о веб-страниц ASP.NET ([https://www.asp.net/web-pages/](../../index.md)). Сведения о новых возможностях и улучшениях в Web Pages 2 см. в разделе [лучшие возможности веб-страниц 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824).

#### <a id="_Toc303701286"></a>  Поддержка

<a id="_Toc209852135"></a><a id="_Toc255833657"></a> Это является предварительной и официально не поддерживается. Если у вас есть вопросы о работе с этим выпуском, размещайте их на форум по ASP.NET Web Pages ([ https://forums.asp.net/1224.aspx/1?WebMatrix ](https://forums.asp.net/1224.aspx/1?WebMatrix) ), где участники сообщества ASP.NET помогают друг другу, предоставляя полезные сведения.

#### <a id="_Toc303701287"></a>  Требования к программному обеспечению

ASP.NET Web Pages 2 требуется .NET Framework 4. Она также работает с выпуском .NET Framework 4.5 Developer Preview.

<a id="_Toc303701288"></a><a id="_Breaking_Changes"></a>

#### <a name="fixes-known-issues-and-breaking-changes"></a>Исправления, известные проблемы и критические изменения

<a id="_Toc224729061"></a><a id="_Toc238051347"></a>

- **Является\* методов (например, IsDateTime) теперь возвращают правильные значения для всех языков и региональных параметров.** Некоторые методы, например *IsDateTime* ранее возвращенный *false* когда они должны будут возвращены *true* так, как они ранее выполнение проверки для конкретного языка и региональных параметров. Эти методы были исправлены теперь учитывать язык и региональные параметры. Это критическое изменение; Если приложение использует старое поведение, оно будет прервано.
- **Поведение метода Href изменилось.** Ранее вызов Href("~/SomeFile") возвращает URL-адрес, являющийся относительным для текущего файла. Теперь Href("~/SomeFile") всегда возвращает абсолютный путь от корня приложения. В большинстве случаев такое поведение не будет зависеть от возвращаемого значения. Это изменение было внесено для устранения определенных сценариях Ajax. Например рассмотрим следующий пример кода: 

    [!code-cshtml[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample1.cshtml)]

    Этот код ранее разрешится Images/Logo.jpg, который будет неверным для Ajax-запросом к этой странице. Он будет разрешаться в корень (/ MySite/Images/Logo.jpg).
- **Метод HttpContext.RedirectLocal изменился**. Теперь этот метод принимает только URL-адреса, которые задаются относительно текущего приложения. Полный URL-адреса будут отклонены.
- **Метода ModelState.IsValid теперь необходимо сначала вызовите Validate**. Если данные преобразуются в приложения для использования новых методов проверки входных данных и при вызове *ModelState.IsValid* , должен теперь вызываемый метод *Validation.Validate* заранее. Например теперь необходимо выполнить этот шаблон. 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample2.cs)]

  Тем не менее, рекомендуется, если использовать новые методы проверки входных данных, не используйте *ModelState.IsValid*. Вместо этого можно структурировать код следующим образом: 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample3.cs)]
- **В Internet Explorer 7 и Internet Explorer 8, проверка на стороне клиента не работает**. Проверка на стороне клиента не работает из-за несовместимости с jQuery 1.6.2, входящий в состав стандартного шаблона проекта. (Работает проверки на стороне сервера.).

#### <a id="_Toc303701289"></a>  Заявление об отказе

© 2011 Корпорация Майкрософт. Все права защищены. Данный документ предоставляется «как-является.» Сведения и мнения, выраженные в данном документе, включая URL-адреса и ссылки на другие веб-сайта, могут быть изменены без предварительного уведомления. Вы принимаете на себя весь риск, связанный с его использованием.
