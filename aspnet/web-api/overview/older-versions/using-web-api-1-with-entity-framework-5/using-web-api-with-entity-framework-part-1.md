---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: 'Часть 1: Обзор и Создание проекта | Документы Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: f9cdff0cb0cad9adad546c8f8d46ba9b010e1079
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="part-1-overview-and-creating-the-project"></a>Часть 1: Обзор и Создание проекта
====================
по [Mike Wasson](https://github.com/MikeWasson)

[Загрузка завершенного проекта](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

Entity Framework является платформу объектно реляционного сопоставления. Объекты домена в коде он преобразуется в сущности в реляционной базе данных. В большинстве случаев у вас беспокоиться о на уровне базы данных, так как он обеспечивает Entity Framework для вас. Код выполняет действия над объектами, а изменения сохраняются в базе данных.

## <a name="about-the-tutorial"></a>О учебника

В этом учебнике вы создадите приложение простой магазина. Существует две основные части в приложение. Обычные пользователи могут просматривать продукты и создания заказов:

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

Администраторы могут создавать, удалять или изменять продуктов:

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a>Навыки, которые вы узнаете

Ниже приведен изучаемого материала.

- Инструкции по использованию платформы Entity Framework с веб-API ASP.NET.
- Инструкции по использованию knockout.js для создания динамического пользовательского интерфейса клиента.
- Инструкции по использованию проверки подлинности форм с веб-API для проверки подлинности пользователей.

Несмотря на то, что этот учебник является автономной, может потребоваться сначала прочитать следующие учебники:

- [Ваш первый веб-API ASP.NET](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [Создание веб-API, который поддерживает операции CRUD](../creating-a-web-api-that-supports-crud-operations.md)

Знание [ASP.NET MVC](../../../../mvc/index.md) это очень удобно.

## <a name="overview"></a>Обзор

На высоком уровне Вот архитектуры приложения.

- ASP.NET MVC создает HTML-страниц для клиента.
- Веб-API ASP.NET предоставляет операции CRUD с данными (products и orders).
- Entity Framework преобразует моделей C#, используемых веб-API в сущности базы данных.

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

На следующей схеме показано, как объекты домена представлены на различных уровнях приложения: на уровне базы данных, объектную модель и наконец формата подключения, которая используется для передачи данных клиента по протоколу HTTP.

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a>Создание проекта Visual Studio

Можно создать проект tutorial, с помощью Visual Web Developer, экспресс-выпуск или полную версию Visual Studio.

Из **запустить** щелкните **новый проект**.

В **шаблоны** выберите **установленные шаблоны** и разверните **Visual C#** узла. В разделе **Visual C#** выберите **Web**. В списке шаблонов проектов выберите **веб-приложение ASP.NET MVC 4**. Назовите проект «ProductStore» и нажмите кнопку **ОК**.

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

В **нового проекта ASP.NET MVC 4** диалогового окна выберите **веб-приложение** и нажмите кнопку **ОК**.

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

Шаблон «Интернет-приложения» создает приложения ASP.NET MVC, который поддерживает проверку подлинности форм. Если запустить приложение, уже имеет некоторые функции:

- Новые пользователи могут зарегистрировать, щелкнув ссылку «Регистрация» в правом верхнем углу.
- Зарегистрированные пользователи выполняют вход, щелкнув ссылку «Вход».

Сведения о членстве сохраняется в базе данных, который создается автоматически. Дополнительные сведения о проверке подлинности форм в ASP.NET MVC см. в разделе [Пошаговое руководство: использование проверки подлинности форм в ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).

## <a name="update-the-css-file"></a>Обновления в CSS-файл

Этот шаг является формальной, но это сделает визуализации, как и на предыдущих снимках экрана страницы.

В обозревателе решений разверните папку содержимого и откройте файл с именем Site.css. Добавьте следующие стили CSS.

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

> [!div class="step-by-step"]
> [Вперед](using-web-api-with-entity-framework-part-2.md)
