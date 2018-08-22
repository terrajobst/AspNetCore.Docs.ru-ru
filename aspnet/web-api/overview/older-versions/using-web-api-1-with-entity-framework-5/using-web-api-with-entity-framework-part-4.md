---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: 'Часть 4: Добавление представления администрирования | Документация Майкрософт'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: ff46c7cbdf13a048e57ad88ab39077357e35c5b3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837639"
---
<a name="part-4-adding-an-admin-view"></a>Часть 4: Добавление представления администрирования
====================
по [Майк Уоссон](https://github.com/MikeWasson)

[Скачать завершенный проект](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a>Добавление представления администрирования

Теперь мы включить на стороне клиента и добавить страницу, которая может получать данные от администратора контроллера. Странице позволит пользователям для создания, редактирования и удаления продуктов, путем отправки запросов AJAX к контроллеру.

В обозревателе решений разверните папку "контроллеры" и откройте файл HomeController.cs. Этот файл содержит контроллер MVC. Добавьте метод с именем `Admin`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

**HttpRouteUrl** метод приводит к созданию URI веб-API, и мы сохраняем это пакет представления для последующего использования.

Затем поместите текстовый курсор внутри `Admin` метода действия, а затем щелкните правой кнопкой мыши и выберите **Добавление представления**. Это приведет к появлению **Добавление представления** диалоговое окно.

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

В **Добавление представления** диалоговом окне имя представления «Admin». Выберите флажок **создать строго типизированное представление**. В разделе **класс модели**, выберите «Product (ProductStore.Models)». Всех других параметров оставьте значения по умолчанию.

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

Щелкнув **добавить** добавляет в файл с именем Admin.cshtml под Views/Home. Откройте этот файл и добавьте следующий код HTML. Следующий код HTML определяет структуру страницы, но еще подключено не добавляет новых функций.

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a>Создать ссылку на страницу администрирования

В обозревателе решений разверните папку Views, а затем разверните папку Shared. Откройте файл с именем \_Layout.cshtml. Найдите **ul** элемент с идентификатором = «menu» и ссылку на действие для представления администрирования:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> В образце проекта я немного другие финальных, например замены строки «Ваша эмблема здесь». Эти элементы не влияют на функциональность приложения. Можно загрузить проект и сравните файлы.


Запустите приложение и щелкните ссылку «Admin», которая появляется в верхней части домашней страницы. На странице администрирования должна выглядеть следующим образом:

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

Справа, страницы не выполняет никаких действий. В следующем разделе мы будем использовать Knockout.js Создание динамического пользовательского интерфейса.

## <a name="add-authorization"></a>Добавление авторизации

На странице администрирования в настоящее время доступен всем узле. Изменим его, чтобы ограничить разрешения для администраторов.

Начните с добавления с ролью «Администратор» и правами администратора. В обозревателе решений разверните папку «фильтры» и откройте файл с именем InitializeSimpleMembershipAttribute.cs. Найдите `SimpleMembershipInitializer` конструктор. После вызова **WebSecurity.InitializeDatabaseConnection**, добавьте следующий код:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

Этот способ скорую руку, чтобы добавить роль «Администратор» и создайте пользователя для роли.

В обозревателе решений разверните папку "контроллеры" и откройте файл HomeController.cs. Добавить **Authorize** атрибут `Admin` метод.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

Откройте файл AdminController.cs и добавьте **Authorize** атрибута ко всей `AdminController` класса.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> MVC и веб-API, как определить **Authorize** атрибуты в разных пространствах имен. MVC использует **System.Web.Mvc.AuthorizeAttribute**, тогда как веб-API использует **System.Web.Http.AuthorizeAttribute**.


Теперь только администраторы могут просматривать на странице администрирования. Кроме того при отправке запроса HTTP к контроллеру администратора, запрос должен содержать файл cookie проверки подлинности. В противном случае сервер отправляет ответ HTTP 401 (не санкционировано). Это видно в Fiddler, отправив запрос GET к `http://localhost:*port*/api/admin`.

> [!div class="step-by-step"]
> [Назад](using-web-api-with-entity-framework-part-3.md)
> [Вперед](using-web-api-with-entity-framework-part-5.md)
