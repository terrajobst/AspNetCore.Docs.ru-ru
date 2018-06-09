---
uid: single-page-application/overview/templates/emberjs-template
title: Шаблон EmberJS | Документы Microsoft
author: xqiu
description: Шаблон EmberJS
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 1fb7633aee288be648d4f9681b43c8911b7dbab9
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "26506803"
---
<a name="emberjs-template"></a>Шаблон EmberJS
====================
по [Xinyang Qiu](https://github.com/xqiu)

> Шаблон MVC EmberJS записывается Натана Тоттена, Тиаго Сантос и Xinyang Qiu.
> 
> [Загрузите шаблон EmberJS MVC](https://go.microsoft.com/fwlink/?LinkId=282647)


Чтобы приступить к работе быстрого создания интерактивные клиентские веб-приложения с помощью EmberJS предназначен шаблон EmberJS SPA.

«Одностраничного приложения» (SPA) — это общий термин для веб-приложения, которое загружает одной HTML-страницы и затем обновляет страницу динамически, вместо загрузки новой страницы. После загрузки начальной страницы SPA обращается к серверу через запросы AJAX.

![](emberjs-template/_static/image1.png)

AJAX нет ничего нового, но на сегодняшний день существует платформ JavaScript, которые упрощают создание и обслуживание больших сложных приложений SPA. Кроме того HTML 5 и CSS3 упрощая процесс для создания многофункциональных пользовательских интерфейсов.

Использует шаблон SPA EmberJS [Ember](http://emberjs.com/) библиотека JavaScript для обработки обновлений страницы из запросы AJAX. Ember.js использует привязку данных для синхронизации на странице с использованием последних данных. В этом случае не нужно писать код, который описывается данным JSON и обновляет модели DOM. Вместо этого вы поместите декларативные атрибуты в формате HTML, сообщить Ember.js способ представления данных.

На стороне сервера шаблона EmberJS практически идентичен [шаблона с использованием KnockoutJS SPA](../introduction/knockoutjs-template.md). ASP.NET MVC используется для работы с HTML-документов и веб-API ASP.NET для обработки запросов AJAX от клиента. Дополнительные сведения об этих аспектов шаблона посвящены [шаблона с использованием KnockoutJS](../introduction/knockoutjs-template.md) документации. Этот раздел посвящен различия между Knockout и EmberJS шаблона.

## <a name="create-an-emberjs-spa-template-project"></a>Создайте проект шаблона EmberJS SPA

Загрузите и установите шаблон, нажав кнопку «Загрузить» выше. Может потребоваться перезапустить Visual Studio.

В **шаблоны** выберите **установленные шаблоны** и разверните **Visual C#** узла. В разделе **Visual C#** выберите **Web**. В списке шаблонов проектов выберите **веб-приложение ASP.NET MVC 4**. Имя проекта и нажмите кнопку **ОК**.

![](emberjs-template/_static/image2.png)

В **новый проект** мастера выберите **Ember.js SPA проекта**.

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a>Обзор шаблона EmberJS SPA

Шаблон EmberJS использует сочетание jQuery, Ember.js, Handlebars.js создать smooth, интерактивный пользовательский Интерфейс.

Ember.js — это библиотека JavaScript, которая использует шаблон MVC клиентские.

- Объект *шаблона*, написанный на языке шаблонов рули описывает пользовательский интерфейс приложения. В режиме выпуска [компилятора рули](https://github.com/Myslik/csharp-ember-handlebars) используется для объединения и скомпилируйте рули шаблона.
- Объект *модель* хранит данные приложений, которые он получает от сервера (ToDo списки и элементы ToDo).
- Объект *контроллера* хранит состояние приложения. Контроллеры часто представляют данные модели, соответствующие шаблоны.
- Объект *представление* преобразует простых событий приложения и передает их на контроллер.
- Объект *маршрутизатора* управляет состоянием приложения, синхронизируя URL-адреса и шаблоны.

Кроме того библиотека Ember данных может использоваться для синхронизации объектов JSON (полученный с сервера через RESTful API) и клиентские модели.

Шаблон EmberJS SPA скрипты организованы по восемь слоев:

- webapi\_adapter.js, webapi\_serializer.js: расширяет библиотеку Ember данных для работы с веб-API ASP.NET.
- Scripts/Helpers.js: Определяет новый Ember рули вспомогательных методов.
- Scripts/App.js: Приложение создает и настраивает адаптер и сериализатор.
- Скрипты, модели иприложений/\*.js: Определяет моделей.
- Сценарии иприложения/представления/\*.js: Определяет представления.
- Скрипты, приложения или контроллеров или\*.js: определяет контроллеры.
- Скрипты, приложения или маршруты, Scripts/app/router.js: Определяет маршруты.
- Шаблоны и\*.hbs: Определяет рули шаблоны.

Рассмотрим некоторые из этих сценариев более подробно.

## <a name="models"></a>Модели

Модели определяются в папке скриптов, модели или приложения. Существуют два файла модели: todoItem.js и todoList.js.

**TODO.Model.js** определяет модели (браузер) на стороне клиента для списков дел. Существует два класса модели: todoItem и todoList. В Ember моделей являются подклассами доменных служб Active Directory. Модель. Модель может иметь свойства, атрибуты которых:

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

Модели можно определить отношения с другими моделями:

[!code-css[Main](emberjs-template/samples/sample2.css)]

Моделей может вычислено свойства, которые привязаны к другим свойствам:

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

Модели может иметь наблюдателя функций, которые вызываются при изменении свойства наблюдаемых:

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a>Представления

Представления определяются в папке скриптов, приложений или представления. Представление преобразует события из пользовательского интерфейса приложения. Обработчик событий может обратный вызов функции контроллера или просто напрямую вызвать контекста данных.

Например приведенный ниже код взят из views/TodoItemEditView.js. Он определяет обработки текстовое поле ввода.

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a>Контроллер

Контроллеры определяются в папке скриптов, приложений или контроллеров. Для представления одной модели, расширить `Ember.ObjectController`:

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

Контроллер может также представлять собой коллекцию моделей, расширяя `Ember.ArrayController`. Например, TodoListController представляет собой массив `todoList` объектов. Контроллер сортирует по Идентификатору todoList, в убывающем порядке:

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

Контроллер определяет функцию с именем `addTodoList`, который создает новый todoList и добавляет его в массив. Чтобы увидеть, как вызывается эта функция, откройте файл шаблона с именем todoListTemplate.html в папке «Шаблоны». Следующий код шаблона привязывает кнопку, чтобы `addTodoList` функции:

[!code-html[Main](emberjs-template/samples/sample8.html)]

Контроллер также содержит `error` свойство, которое содержит сообщение об ошибке. Ниже приведен код шаблона, чтобы показать сообщение об ошибке (а также в todoListTemplate.html):

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a>Маршруты

Router.js определяет маршруты и шаблон по умолчанию для отображения настраивает состояния приложения и сопоставляет URL-адреса маршруты:

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

TodoListRoute.js загружает данные для TodoListRoute путем переопределения setupController функции:

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

Ember использует соглашения об именовании для соответствия URL-адреса, имена маршрутов, контроллеров и шаблоны. Дополнительные сведения см. в разделе [ http://emberjs.com/guides/routing/defining-your-routes/ ](http://emberjs.com/guides/routing/defining-your-routes/) EmberJS документацию.

## <a name="templates"></a>Шаблоны

Папка шаблонов содержит четыре шаблона:

- Application.hbs: шаблон по умолчанию, который отображается при запуске приложения.
- About.hbs: шаблона маршрута «/ о программе».
- index.hbs: шаблон для корня маршрута «/».
- todoList.hbs: шаблона для «/ todo» маршрута.
- \_NavBar.hbs: шаблон определяет меню навигации.

Шаблон приложения ведет себя как главной страницы. Он содержит заголовок, нижний колонтитул и «{{розетки}}» для вставки другие шаблоны, в зависимости от маршрута. Дополнительные сведения о шаблонах приложений в Ember см. в разделе [ http://guides.emberjs.com/v1.10.0/templates/the-application-template// ](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).

«/ TodoList» шаблон содержит два выражения цикла. Находится вне цикла `{{#each controller}}`и внутренние цикл `{{#each todos}}`. В следующем коде показано встроенная `Ember.Checkbox` просмотреть, настраиваемый `App.TodoItemEditView`и компоновку с `deleteTodo` действие.

[!code-html[Main](emberjs-template/samples/sample12.html)]

`HtmlHelperExtensions` Класс, определенный в Controllers/HtmlHelperExensions.cs, определяет вспомогательный класс функции кэширования и вставить шаблон файлов при **отладки** равно **true** в файле Web.config. Эта функция вызывается из файла представления ASP.NET MVC, определенные в Views/Home/App.cshtml:

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

Вызван без аргументов, функция отображает все файлы шаблонов в папке «Шаблоны». Можно также указать вложенную папку или файл определенного шаблона.

Когда **отладки** — **false** в файле Web.config, приложение включает элемент набора «~/bundles/templates». Этот элемент в пакет добавляется в BundleConfig.cs, с помощью компилятора рули библиотеки:

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
