---
uid: single-page-application/overview/templates/emberjs-template
title: Шаблон EmberJS | Документация Майкрософт
author: xqiu
description: Шаблон EmberJS
ms.author: riande
ms.date: 01/30/2013
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: fbc3b1d299ace27d38d895e42b8e3bb3b51b36f8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827781"
---
<a name="emberjs-template"></a>Шаблон EmberJS
====================
по [Цю Xinyang](https://github.com/xqiu)

> Шаблон EmberJS записывается, Натана Тоттена, Thiago Santos и Цю Xinyang.
> 
> [Загрузить шаблон EmberJS](https://go.microsoft.com/fwlink/?LinkId=282647)


Чтобы приступить к работе, быстро создания интерактивные клиентские веб-приложений, с использованием EmberJS предназначен данный шаблон EmberJS SPA.

«Одностраничного приложения» (SPA) — это общий термин для веб-приложения, который загружает единственную HTML-страницу и затем обновляет страницу динамически, вместо загрузки новых страниц. После загрузки начальной страницы SPA взаимодействует с сервером с помощью запросов AJAX.

![](emberjs-template/_static/image1.png)

AJAX нет ничего нового, но на сегодняшний день существует платформ JavaScript, которые упрощают создание и обслуживание большого сложные приложения SPA. Кроме того HTML 5 и CSS3 что облегчает создание многофункциональных пользовательских интерфейсов.

Шаблон EmberJS SPA использует [Ember](http://emberjs.com/) библиотека JavaScript для обработки обновлений страницы из запросов AJAX. Ember.js использует привязку данных для синхронизации на странице с последними данными. Таким образом, не нужно писать код, который поможет выполнить данные JSON и обновляет DOM. Вместо этого вы поместите декларативных атрибутов в код HTML, сообщите Ember.js, как для представления данных.

На стороне сервера, шаблон EmberJS почти идентичен [шаблон KnockoutJS SPA](../introduction/knockoutjs-template.md). ASP.NET MVC используется для предоставления HTML-документы и веб-API ASP.NET для обработки запросов AJAX от клиента. Дополнительные сведения о аспекты шаблона см. [шаблон KnockoutJS](../introduction/knockoutjs-template.md) документации. В этом разделе рассматриваются различия между шаблоном Knockout и шаблон EmberJS.

## <a name="create-an-emberjs-spa-template-project"></a>Создание проекта SPA шаблон EmberJS

Скачайте и установите шаблон, нажав кнопку "скачать" выше. Может потребоваться перезапустить Visual Studio.

В **шаблоны** области выберите **установленные шаблоны** и разверните **Visual C#** узла. В разделе **Visual C#** выберите **Web**. В списке шаблонов проектов выберите **веб-приложение ASP.NET MVC 4**. Имя проекта и нажмите кнопку **ОК**.

![](emberjs-template/_static/image2.png)

В **новый проект** мастера выберите **проекта SPA Ember.js**.

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a>Общие сведения о шаблоне SPA EmberJS

Шаблон EmberJS использует сочетание jQuery, Ember.js, Handlebars.js создать smooth, интерактивный пользовательский Интерфейс.

Ember.js — это библиотека JavaScript, которая использует шаблон MVC со стороны клиента.

- Объект *шаблона*, написанные на языке шаблонов Handlebars, описывает пользовательский интерфейс приложения. В режиме выпуска [Handlebars компилятора](https://github.com/Myslik/csharp-ember-handlebars) используется для объединения и компиляции handlebars шаблона.
- Объект *модели* хранятся данные приложения, оно получает с сервера (списки дел и элементы списка дел).
- Объект *контроллера* хранит состояние приложения. Контроллеры часто представляют данные модели, соответствующие шаблоны.
- Объект *представление* преобразует простых событий приложения и передает их к контроллеру.
- Объект *маршрутизатора* управляет состоянием приложения, синхронизация URL-адреса и шаблоны.

Кроме того библиотека Ember данных можно использовать для синхронизации объектов JSON (полученный с сервера через API-интерфейсов RESTful) и клиентские модели.

Шаблон EmberJS SPA организует скрипты в восемь уровней:

- веб-API\_adapter.js, веб-API\_serializer.js: расширяет библиотеку Ember данных для работы с веб-API ASP.NET.
- Scripts/Helpers.js: Определяет новых Ember Handlebars вспомогательных функций.
- Scripts/App.js: Приложение создает и настраивает адаптер и сериализатор.
- Сценарии/приложения/модели/\*.js: Определяет моделей.
- Сценарии иприложения/представления/\*.js: Определяет представления.
- Сценарии/приложения/контроллеров/\*.js: определяет контроллеры.
- Сценарии/приложения/routes, Scripts/app/router.js: Определяет маршруты.
- Шаблоны /\*.hbs: определяет шаблоны handlebars.

Давайте рассмотрим некоторые из этих сценариев более подробно.

## <a name="models"></a>Модели

Модели определяются в папке скриптов/приложения/models. Существует два файла модели: todoItem.js и todoList.js.

**TODO.Model.js** определяет модели (браузер) на стороне клиента для списков задач. Существует два класса модели: todoItem и todoList. В Ember модели являются подклассами класса доменных служб Active Directory. Модель. Модель может иметь свойства с атрибутами:

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

Модели можно определить отношения с другими моделями:

[!code-css[Main](emberjs-template/samples/sample2.css)]

Моделей может вычислено как свойства, которые привязаны к другим свойствам:

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

Моделей может иметь наблюдателя функций, которые вызываются при изменении наблюдаемых свойств:

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a>Представления

Представления определяются в папке скриптов/приложения/представления. Представление преобразует события из пользовательского интерфейса приложения. Обработчик событий можно обратный вызов функции контроллера или просто напрямую вызвать контекста данных.

Например следующий код — от views/TodoItemEditView.js. Он определяет обработки текстовое поле ввода.

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a>Контроллер

Контроллеры, определяются в папку скриптов/приложения/контроллеры. Для представления одной модели, расширить `Ember.ObjectController`:

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

Контроллер может также представлять собой коллекцию моделей путем расширения `Ember.ArrayController`. Например, TodoListController представляет собой массив `todoList` объектов. Контроллер сортирует по Идентификатору todoList, в убывающем порядке:

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

Контроллер определяет функцию с именем `addTodoList`, который создает новый todoList и добавляет его в массив. Чтобы увидеть, как вызывается эта функция, откройте файл шаблона с именем todoListTemplate.html в папке Templates. Приведенный ниже шаблон связывает кнопку, чтобы `addTodoList` функции:

[!code-html[Main](emberjs-template/samples/sample8.html)]

Контроллер также содержит `error` свойство, которое содержит сообщение об ошибке. Ниже приведен код шаблона для отображения сообщения об ошибке (также todoListTemplate.html).

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a>Маршруты

Router.js определяет маршруты и шаблон по умолчанию для отображения, позволяет задать состояние приложения и сопоставляет URL-адреса маршруты:

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

TodoListRoute.js загружает данные для TodoListRoute путем переопределения setupController функции:

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

Ember использует соглашения об именовании в соответствии с URL-адреса, имена маршрутов, контроллеров и шаблоны. Дополнительные сведения см. в разделе [ http://emberjs.com/guides/routing/defining-your-routes/ ](http://emberjs.com/guides/routing/defining-your-routes/) EmberJS документацию.

## <a name="templates"></a>Шаблоны

Папка шаблонов содержит четыре шаблона:

- Application.hbs: шаблон по умолчанию, который отображается при запуске приложения.
- About.hbs: шаблон для маршрута «/ о программе».
- index.hbs: шаблон для корневого маршрута «/».
- todoList.hbs: шаблон для «/ todo» маршрута.
- \_NavBar.hbs: в меню навигации, которые определяет шаблон.

Данный шаблон приложения выступает в роли главной страницы. Он содержит верхний колонтитул, нижний колонтитул и «{{розетки}}» для вставки другие шаблоны, в зависимости от маршрута. Дополнительные сведения о шаблонах приложений в Ember см. в разделе [ http://guides.emberjs.com/v1.10.0/templates/the-application-template// ](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).

«/ TodoList» шаблон содержит два выражения цикла. Вне цикла является `{{#each controller}}`и внутренние цикл `{{#each todos}}`. В следующем коде показано встроенный `Ember.Checkbox` просмотреть, настраиваемый `App.TodoItemEditView`и приводится ссылка на `deleteTodo` действие.

[!code-html[Main](emberjs-template/samples/sample12.html)]

`HtmlHelperExtensions` Класс, определенный в Controllers/HtmlHelperExensions.cs, определяет вспомогательный объект функции кэширования и вставить шаблон файлов при **Отладка** присваивается **true** в файле Web.config. Эта функция вызывается из файла представления ASP.NET MVC, определенные в Views/Home/App.cshtml:

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

Вызван без аргументов, функция отображает все файлы в папке Templates шаблона. Можно также указать вложенную папку или файл определенного шаблона.

Когда **Отладка** — **false** в файле Web.config, приложение включает в себя элемент пакета «~/bundles/templates». Этот элемент пакет добавляется в BundleConfig.cs, с помощью библиотеки компилятора Handlebars:

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
