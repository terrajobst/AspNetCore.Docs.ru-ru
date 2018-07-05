---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: 'Часть 1: Обзор и Создание проекта | Документация Майкрософт'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: f0616383fce2e92f7d1a0b63bf840208f7327bf7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394065"
---
<a name="part-1-overview-and-creating-the-project"></a>Часть 1: Обзор и Создание проекта
====================
по [Майк Уоссон](https://github.com/MikeWasson)

[Скачать завершенный проект](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

Платформа Entity Framework — это с объектно реляционного сопоставления. Оно сопоставляет объекты домена в коде с сущностями в реляционной базе данных. По большей части у вас нет беспокоиться о на уровне базы данных, так как Entity Framework берет на себя его для вас. Код обрабатывает объекты, а изменения сохраняются в базе данных.

## <a name="about-the-tutorial"></a>О руководстве

В этом руководстве вы создадите приложение простого хранилища. Существует две основные части к приложению. Обычные пользователи могут просматривать продукты и размещают заказы:

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

Администраторы можно создать, удалить или изменить продуктов:

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a>Навыки, которые вы узнаете

Вот, вы узнаете, как:

- Сведения об использовании Entity Framework с помощью веб-API ASP.NET.
- Как создать с помощью knockout.js динамического пользовательского интерфейса клиента.
- Как использовать проверку подлинности форм с веб-API для проверки подлинности пользователей.

Несмотря на то, что это руководство представляет собой автономное, может потребоваться сначала прочитайте следующие руководства:

- [Ваш первый веб-API ASP.NET](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [Создание веб-API, который поддерживает операции CRUD](../creating-a-web-api-that-supports-crud-operations.md)

Знание [ASP.NET MVC](../../../../mvc/index.md) это очень удобно.

## <a name="overview"></a>Обзор

На высоком уровне здесь — это архитектура приложения:

- ASP.NET MVC создает HTML-страницы для клиента.
- Веб-API ASP.NET предоставляет операции CRUD с данными (products и orders).
- Платформа Entity Framework преобразует моделей C#, используемых веб-API в сущности базы данных.

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

На следующей схеме показано, как объекты домена представлены на различных уровнях приложения: на уровне базы данных, объектной модели и, наконец, формат подключения, которая используется для передачи данных клиента по протоколу HTTP.

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a>Создание проекта Visual Studio

Можно создать проект tutorial, с помощью Visual Web Developer Express или полную версию Visual Studio.

Из **запустить** щелкните **новый проект**.

В **шаблоны** области выберите **установленные шаблоны** и разверните **Visual C#** узла. В разделе **Visual C#** выберите **Web**. В списке шаблонов проектов выберите **веб-приложение ASP.NET MVC 4**. Присвойте проекту имя «ProductStore» и нажмите кнопку **ОК**.

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

В **создания проекта ASP.NET MVC 4** диалоговом окне выберите **веб-приложение** и нажмите кнопку **ОК**.

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

Шаблон «Веб-приложение» создает приложение ASP.NET MVC, который поддерживает проверку подлинности форм. Если запустить приложение сейчас, он уже имеет некоторые функции:

- Новые пользователи могут зарегистрировать, щелкнув ссылку «Зарегистрировать» в правом верхнем углу.
- Зарегистрированные пользователи могут входить в, щелкнув ссылку «Вход».

Сведения о членстве сохраняется в базе данных, который создается автоматически. Дополнительные сведения о проверке подлинности форм в ASP.NET MVC см. в разделе [Пошаговое руководство: использование проверки подлинности форм в ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).

## <a name="update-the-css-file"></a>Обновите файл CSS

Этот шаг является декоративным, но это сделает визуализации как на предыдущих снимках экрана страницы.

В обозревателе решений разверните папку и откройте файл с именем Site.css. Добавьте следующие стили CSS:

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

> [!div class="step-by-step"]
> [Вперед](using-web-api-with-entity-framework-part-2.md)
