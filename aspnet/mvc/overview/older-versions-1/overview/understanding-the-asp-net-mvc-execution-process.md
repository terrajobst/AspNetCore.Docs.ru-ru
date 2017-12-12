---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: "Основные сведения о процессе выполнения ASP.NET MVC | Документы Microsoft"
author: microsoft
description: "Узнайте, как платформа ASP.NET MVC обрабатывает пошаговые запрос браузера."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 5837c6e49709d6b86ee52cd88ffd4759c1850544
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="understanding-the-aspnet-mvc-execution-process"></a>Основные сведения о процессе выполнения ASP.NET MVC
====================
по [Microsoft](https://github.com/microsoft)

> Узнайте, как платформа ASP.NET MVC обрабатывает пошаговые запрос браузера.


Запросы на основе ASP.NET MVC веб-приложения сначала пройти через **UrlRoutingModule** объект, используемый модулем HTTP. Этот модуль анализирует запрос и выбирает маршрут. **UrlRoutingModule** выбирает первый объект маршрута, соответствующий текущему запросу. (Объект маршрута — это класс, реализующий **RouteBase**, и обычно является экземпляром класса **маршрута** класса.) Если подходящих маршрутов нет, **UrlRoutingModule** объекта ничего не делает и передает запрос обратно на обычный запрос ASP.NET или IIS обработку.

Из выбранного **маршрута** объекта, **UrlRoutingModule** объектом **IRouteHandler** объекта, с которым связан **маршрута**объекта. Как правило, в MVC-приложении, это будет экземпляр **MvcRouteHandler**. **IRouteHandler** создает экземпляр **IHttpHandler** и передает его **IHttpContext** объекта. По умолчанию **IHttpHandler** экземпляра для MVC — **MvcHandler** объекта. **MvcHandler** объект затем выбирает контроллер, который будет обрабатывать запрос.

> [!NOTE]
> При выполнении MVC веб-приложения ASP.NET в IIS 7.0 без расширения имени файла является обязательным для проектов MVC. В службах IIS 6.0 обработчик требуется сопоставить расширение имени файла MVC было связано, к библиотеке DLL ISAPI ASP.NET.


Модуль и обработчик являются точками входа с платформой ASP.NET MVC. Они выполните следующие действия.

- Выбор нужного контроллера в веб-приложение MVC.
- Получение отдельного экземпляра контроллера.
- Вызов контроллера **Execute** метод.

Ниже перечислены этапы выполнения веб-проекта MVC.

- Получение первого запроса для приложения 

    - В файле Global.asax **маршрута** объекты добавляются в **RouteTable** объекта.
- Выполнение маршрутизации 

    - **UrlRoutingModule** модуль использует первый подходящий **маршрута** объекта в **RouteTable** коллекции для создания **RouteData** Объект, который затем используется для создания **RequestContext** (**IHttpContext**) объекта.
- Создание обработчика запроса MVC 

    - **MvcRouteHandler** объект создает экземпляр **MvcHandler** класса и передает его **RequestContext** экземпляра.
- Создание контроллера 

    - **MvcHandler** объектов используют **RequestContext** экземпляра для идентификации **IControllerFactory** объекта (обычно это экземпляр  **DefaultControllerFactory** класса) для создания экземпляра контроллера с.
- Контроллер EXECUTE — **MvcHandler** экземпляр вызывает контроллер s **Execute** метод. |
- Вызов действия 

    - Наследуют большинство контроллеров **контроллера** базового класса. Для контроллеров, сделать это **ControllerActionInvoker** объекта, связанного с контроллером определяет, какой метод действия контроллера класса для вызова, а затем вызывает этот метод.
- Выполнение результата 

    - Метод типичные действия может реагировать на действия пользователя, подготовьте соответствующие данные ответа и выполните результат, возвращая тип результата. Следующие типы встроенных результатов, которые могут быть выполнены: **ViewResult** (который выполняет визуализацию представления и является типом результата чаще всего), **RedirectToRouteResult**,  **RedirectResult**, **ContentResult**, **JsonResult**, и **EmptyResult**.
