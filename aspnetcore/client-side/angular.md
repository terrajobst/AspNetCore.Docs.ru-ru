---
title: "С помощью AngularJS для приложений на одной странице (SPAs)"
author: rick-anderson
description: "Сведения о создании приложения ASP.NET SPA стиля, с помощью AngularJS"
keywords: ASP.NET Core, AngularJS, SPA
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 4b30576b-2718-4c39-9253-a59966747893
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/angular
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d271560476ee4efdffbd457e37eb769a7ae6ca25
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/22/2017
---
# <a name="using-angularjs-for-single-page-applications-spas-with-aspnet-core"></a>С помощью AngularJS для приложений на одной странице (SPAs) с ASP.NET Core


По [Venkata Koppaka](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/) и [Скотт Addie](https://scottaddie.com)

В этой статье вы узнаете, как создать приложение ASP.NET SPA стиля, с помощью AngularJS.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample)

## <a name="what-is-angularjs"></a>Что такое AngularJS?

[AngularJS](https://angularjs.org/) — это современная платформа JavaScript из Google, обычно используется для работы с приложениями одной страницы (SPAs). AngularJS открытой источником в рамках лицензии MIT и может располагаться после разработки ход AngularJS [своего репозитория GitHub](https://github.com/angular/angular.js). Библиотеке называется угловая, поскольку HTML использует углового были сформированы квадратные скобки.

AngularJS не является библиотекой манипуляции DOM, например jQuery, но она использует подмножество называется jQLite jQuery. AngularJS в основе декларативных атрибутов HTML, которые можно добавить теги HTML. Можно попробовать AngularJS в браузере с помощью [School код веб-сайта](https://www.codeschool.com/courses/shaping-up-with-angularjs) или [W3Schools веб-сайт](https://www.w3schools.com/angular/).

Эта статья посвящена AngularJS некоторые особенности, на котором заголовок угловая.

## <a name="getting-started"></a>Начало работы

Чтобы начать использовать AngularJS в приложении ASP.NET, необходимо установить его как часть проекта или сослаться на нее из сети доставки содержимого (CDN).

### <a name="installation"></a>Установка

Добавление AngularJS в приложении несколькими способами. Если вы начинаете новое веб-приложение ASP.NET Core в Visual Studio, можно добавить с помощью встроенной AngularJS [Bower](bower.md) поддержки. Откройте *bower.json*и добавить запись `dependencies` свойство:

<a name=angular-bower-json></a>

[!code-json[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/bower.json?highlight=9)]

При сохранении *bower.json* файл, угловая устанавливается в своем проекте *wwwroot/lib* папки. Кроме того, оно будет отображаться в `Dependencies/Bower` папки. См. на следующем снимке экрана.

![Обозреватель решений с проектом AngularJS](angular/_static/angular-solution-explorer.png)

Добавьте `<script>` ссылку внизу `<body>` части HTML-страницы или *_Layout.cshtml* файла, как показано ниже:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=4&range=48-52)]

Рекомендуется, что производственных приложений использовать CDN для стандартных библиотек, как AngularJS. AngularJS можно ссылаться из одного из нескольких CDN, подобные следующему:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=10&range=53-67)]

Получив ссылку на *angular.js* файл скрипта, вы будете готовы приступить к использованию AngularJS на веб-страницах.

## <a name="key-components"></a>Основные компоненты

AngularJS включает ряд основных компонентов, таких как *директивы*, *шаблоны*, *знаки повторения*, *модули*, * контроллеры*, *компоненты*, *маршрутизатора компонент* и многое другое. Давайте рассмотрим, как эти компоненты взаимодействуют для добавления поведения на веб-страницы.

### <a name="directives"></a>Директивы

Использует AngularJS [директивы](https://docs.angularjs.org/guide/directive) расширить HTML с пользовательскими атрибутами и элементами. Директивы AngularJS определяются через `data-ng-*` или `ng-*` префиксы (`ng` — сокращение угловое). Существует два вида директивы AngularJS.

   1. **Примитив директивы**: эти стандартные углового командой и являются частью платформы AngularJS.

   2. **Пользовательские директивы**: это пользовательских директив, которые можно определить.

Одним из простых директив, используемых во всех приложениях AngularJS является `ng-app` директивы, который загружает приложения AngularJS. Эта директива может применяться к `<body>` тег или дочерний элемент текста. Рассмотрим пример, в действии. Если вы находитесь в проекте ASP.NET, можно либо добавить HTML-файл для `wwwroot` папки, или добавьте новое действие контроллера и связанного представления. В этом случае я добавил новый `Directives` методом действия `HomeController.cs`. Здесь показан связанного представления:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Directives.cshtml?highlight=5,7)]

Чтобы эти образцы независимы друг от друга, не используется файл общего макета. Вы увидите, что мы внутренние тега body с `ng-app` директиву, чтобы указать на этой странице — это приложение AngularJS. `{{2+2}}` Выражение привязки углового данных, которое будет рассмотрено более чуть позже. Ниже приведен результат, если вы запустите это приложение:

![Простой угловой директивы](angular/_static/simple-directive.png)

Другие директивы примитивы в AngularJS включают:

`ng-controller`Определяет, какой контроллер JavaScript привязан к какой вид.

`ng-model`Определяет модель, с которыми связаны значения свойств элемента HTML.

`ng-init`Используется для инициализации данных приложения в виде выражения для текущей области.

`ng-if`Удаляет и повторно создает HTML-элемента, заданного в модели DOM, в зависимости от truthiness введенное выражение.

`ng-repeat`Повторяет данного блока HTML для набора данных.

`ng-show`Показывает или скрывает данного элемента HTML на основе выражения, предоставленные.

Полный список всех примитивов директив, поддерживаемые в AngularJS, можно найти [разделе директив документации на веб-сайте AngularJS документации](https://docs.angularjs.org/api/ng/directive).

### <a name="data-binding"></a>привязка данных,

Предоставляет AngularJS [привязки данных](https://docs.angularjs.org/guide/databinding) поддерживает out-of--box с помощью `ng-bind` директива или данные, такие как синтаксис выражений привязки `{{expression}}`. AngularJS поддерживает двустороннюю привязку данных хранения данных из модели в синхронизации с помощью шаблона представления в любое время. Любые изменения в представлении автоматически отражаются в модели. Аналогичным образом любые изменения в модели, отражаются в представлении.

Создать HTML-файл или действия контроллера с сопутствующей представлением с именем `Databinding`. Следующие представления:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Databinding.cshtml?highlight=8,9,10)]

Обратите внимание, что можно отобразить значения модели, с помощью директивы или данные привязки (`ng-bind`). Получившаяся в результате страница должна выглядеть следующим образом:

![Простая привязка данных](angular/_static/simple-databinding.png)

### <a name="templates"></a>Шаблоны

[Шаблоны](https://docs.angularjs.org/guide/templates) в AngularJS просто HTML-страниц оформляются с директивы AngularJS и артефактов. Шаблон AngularJS находится несколько директив, выражений, фильтров и элементы управления, объединяющие с кодом HTML, представление формы.

Добавление другого представления для демонстрации шаблонов и добавьте в него следующий:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Templates.cshtml?highlight=8,9,10)]

Шаблон содержит директивы AngularJS как `ng-app`, `ng-init`, `ng-model` и синтаксис выражений привязки данных для привязки `personName` свойство. Выполняется в браузере, представление выглядит следующим образом на снимке экрана ниже.

![Простой пример шаблоны 1](angular/_static/simple-templates-1.png)

Если изменить имя, введя в поле ввода, вы увидите текст рядом с полем ввода динамическое обновление, показывающая углового двухстороннюю привязку данных в действие.

![Простой пример шаблоны 2](angular/_static/simple-templates-2.png)

### <a name="expressions"></a>Выражения

[Выражения](https://docs.angularjs.org/guide/expression) в AngularJS — это фрагменты кода JavaScript, которые записываются в `{{ expression }}` синтаксиса. Данные из этих выражений привязан к HTML так же, как `ng-bind` директивы. Основное различие между AngularJS выражений и регулярных выражений JavaScript является этой AngularJS выражений производится по `$scope` объекта в AngularJS.

AngularJS выражения в примере ниже привязки `personName` и простой JavaScript вычисляется выражение:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Expressions.cshtml?highlight=8,9,10)]

Пример, в браузере отображается `personName` данных и результаты вычисления:

![Простые выражения](angular/_static/simple-expressions.png)

### <a name="repeaters"></a>Знаки повторения

Повторять в AngularJS выполняется через простые директива вызывается `ng-repeat`. `ng-repeat` Директива повторяет данного элемента HTML в представлении по длине массива повторяющихся данных. Знаки повторения в AngularJS можно повторить к массиву строк или объектов. Ниже приведен пример использования повторяющихся к массиву строк.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters.cshtml?highlight=8,10,11)]

[Repeat-директива](https://docs.angularjs.org/api/ng/directive/ngRepeat) выводит ряд элементов списка в неупорядоченный список, как видно в средствах разработчика на этом снимке экрана показано:

![Пример повторителя](angular/_static/repeater.png)

Ниже приведен пример, в котором повторяется к массиву объектов. `ng-init` Устанавливает директива `names` массива, где каждый элемент является объектом, содержащим сначала имени и фамилии. `ng-repeat` Назначения, `name in names`, выводит элемент списка для каждого элемента массива.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters2.cshtml?highlight=8,9,10,11,13,14)]

Выходные данные в этом случае является таким же, как в предыдущем примере.

Угловая предоставляет некоторые дополнительные директивы, которые могут помочь в составлении поведение в зависимости от того, где цикла в ходе выполнения.

`$index`

Используйте `$index` в `ng-repeat` цикла, чтобы определить, какой индекс позиции в цикле в настоящее время включена.

`$even` и `$odd`.

Используйте `$even` в `ng-repeat` цикла, чтобы определить, является ли текущий индекс в цикле даже индексированных строк. Аналогичным образом, использовать `$odd` чтобы определить, является ли текущий индекс нечетных индексированных строк.

`$first` и `$last`.

Используйте `$first` в `ng-repeat` цикла, чтобы определить, является ли текущий индекс в цикле первой строки. Аналогичным образом, использовать `$last` , чтобы определить, является ли текущий индекс последней строки.

Ниже приведен пример, показывающий `$index`, `$even`, `$odd`, `$first`, и `$last` в действии:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters3.cshtml?highlight=14,15,16,17,18)]

Ниже приведен результат:

![Пример повторителя 2](angular/_static/repeaters2.png)

### <a name="scope"></a>$scope

`$scope`представляет собой объект JavaScript, который выступает в качестве связующего между представлением (шаблон) и контроллер (описано ниже). Шаблон представления в AngularJS только известны значения, присоединенных к `$scope` объекта в контроллере.

> [!NOTE]
> В мире MVVM `$scope` объект в AngularJS часто определяется как ViewModel. Команда AngularJS ссылается на `$scope` объект в качестве модели данных. [Дополнительные сведения об областях в AngularJS](https://docs.angularjs.org/guide/scope).

Ниже приведен простой пример, показывающий, как задать свойства для `$scope` в отдельном файле JavaScript, *scope.js*:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/scope.js?highlight=2,3)]

Наблюдать за `$scope` передать параметр в строке 2 контроллера. Этот объект является знает о представлении. В строке 3 рекомендуется задать свойство с именем «name» в «Mary Jane».

Что происходит, когда определенное свойство, не найден в представлении? Свойства «имя» и «возраст» ссылается представление, определенное ниже:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Scope.cshtml?highlight=9,10,14)]

Обратите внимание, в строке 9, что мы просим угловая отображается свойство «имя», с использованием синтаксиса выражений. Строка 10 затем ссылается на «age», свойство, которое не существует. Выполнение примере именем, равным «Мэри Jane» и ничего для возраста. Отсутствующие свойства игнорируются.

![Пример области](angular/_static/scope.png)

### <a name="modules"></a>Модули

Объект [модуль](https://docs.angularjs.org/guide/module) в AngularJS — это совокупность контроллеров, службы, директивы, и т. д. `angular.module()` Вызов функции используется для создания, регистрации и получения модулей AngularJS. Все модули, включая те, поставляемый корпорацией AngularJS команды и библиотек сторонних производителей, должен быть зарегистрирован с помощью `angular.module()` функции.

Ниже приведен фрагмент кода, показывающий, как создать новый модуль в AngularJS. Первый параметр является имя модуля. Второй параметр определяет зависимости на другие модули. Далее в этой статье мы будет отображаться как передавать эти зависимости `angular.module()` вызова метода.

```javascript
var personApp = angular.module('personApp', []);
```

Используйте `ng-app` директивы для представления модуль AngularJS на странице. Чтобы использовать модуль, назначить имя модуля, `personApp` в этом примере для `ng-app` директив в нашем шаблона.

```html
<body ng-app="personApp">
```

### <a name="controllers"></a>Контроллеры

[Контроллеры](https://docs.angularjs.org/guide/controller) в AngularJS, первой точкой входа для кода. `<module name>.controller()` Вызов функции используется для создания и регистрации контроллеров в AngularJS. `ng-controller` Директива используется для представления контроллер AngularJS на HTML-странице. Роли контроллера в угловой является установка состояние и поведение модели данных (`$scope`). Контроллеры не должен использоваться для управления DOM напрямую.

Ниже приведен фрагмент кода, который регистрирует новый контроллер. `personApp` Переменная в фрагменте ссылается углового модуль, который определен в строке 2.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/controllers.js?highlight=2,5)]

Представление с помощью `ng-controller` директива назначает имя контроллера:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Controllers.cshtml?highlight=8,14)]

На странице отображается «Мария» и «Jane», которые соответствуют `firstName` и `lastName` свойства присоединяется к `$scope` объекта:

![Пример контроллера](angular/_static/controllers.png)

### <a name="components"></a>Компоненты

[Компоненты](https://docs.angularjs.org/guide/component) в угловой 1.5.x разрешить для инкапсуляции и возможность создания отдельного HTML-элементов. В угловой 1.4.x удалось добиться одной функции, с помощью метода .directive().

С помощью метода .component(), разработки упрощено получение функциональные возможности директивы и контроллера. Другие преимущества —; изоляция на уровне области, принадлежат рекомендации и миграции углового 2 становится проще задачу. `<module name>.component()` Вызов функции используется для создания и регистрации компонентов в AngularJS.

Ниже приведен фрагмент кода, который регистрирует новый компонент. `personApp` Переменная в фрагменте ссылается углового модуль, который определен в строке 2.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/components.js?highlight=2,5,13)]

Представление, где отображается пользовательский элемент HTML.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Components.cshtml?highlight=8)]

Связанный шаблон, который используется компонентом:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personcomponent.html?highlight=2,3)]

На странице отображается «Aftab» и «Ansari», которые соответствуют `firstName` и `lastName` свойства присоединяется к `vm` объекта:

![Пример компонентов](angular/_static/components.png)

### <a name="services"></a>Службы

[Службы](https://docs.angularjs.org/guide/services) в AngularJS обычно используются для общего кода, который абстрагироваться в файл, который может использоваться в течение времени существования углового приложения. Неактивно создаются службы, это означает, что не будет экземпляр службы, пока не будет использоваться компонентом, который зависит от службы. Фабрики являются примером службы, используемые в приложениях AngularJS. Фабрики создаются с помощью `myApp.factory()` функции вызова, где `myApp` — модуль.

Ниже приведен пример, демонстрирующий использование фабрик в AngularJS.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/simpleFactory.js?highlight=1)]

Для вызова этой фабрики от контроллера, передачи `personFactory` как параметр `controller` функции:

```javascript
personApp.controller('personController', function($scope,personFactory) {
  $scope.name = personFactory.getName();
});
```

### <a name="using-services-to-talk-to-a-rest-endpoint"></a>Можно также обратиться к конечной точке REST с помощью служб

Ниже — законченный пример использует службы в AngularJS для взаимодействия с конечной точкой веб-API ASP.NET Core. В примере получает данные из веб-API и данные отображаются в Просмотр шаблона. Начнем с создания представления сначала:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Index.cshtml?highlight=5,8,10,17,18,19)]

В этом представлении у нас есть углового модуль, называемый `PersonsApp` и вызывается контроллер `personController`. Мы используем `ng-repeat` для прохода по списку лиц. Мы ссылаются три пользовательских файлов JavaScript в строках 17-19.

*PersonApp.js* файл используется для регистрации `PersonsApp` модуль; и синтаксис аналогично предыдущих примерах. Мы используем `angular.module` функцию, чтобы создать новый экземпляр модуля, мы будем работать с.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personApp.js?highlight=3)]

Давайте рассмотрим *personFactory.js*ниже. Поступает вызов модуля `factory` метод для создания фабрики. Строка 12 показывает встроенных угловая `$http` службы, получение сведений о людей из веб-службы.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personFactory.js?highlight=6,7,12)]

В *personController.js*, поступает вызов модуля `controller` метод для создания контроллера. `$scope` Объекта `people` присваивается данные, возвращенные из personFactory (строка 13).

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personController.js?highlight=6,7,13)]

Давайте кратко рассмотрим веб-API и модель, стоящий за ней. `Person` Модель является POCO (Plain объект среды CLR) с `Id`, `FirstName`, и `LastName` свойства:

[!code-csharp[Main](angular/sample/AngularJSSample/src/AngularJSSample/Models/Person.cs)]

`Person` Контроллера возвращает список формата JSON `Person` объектов:

[!code-csharp[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Controllers/Api/PersonController.cs?highlight=9,10,19)]

Давайте посмотрим, приложение в действии:

![Отображение результатов REST контроллера](angular/_static/rest-bound.png)

Вы можете [просмотреть структуру приложения на GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).

> [!NOTE]
> Дополнительные сведения о структурировании приложения AngularJS см [Джон Папа углового по стилю](https://github.com/johnpapa/angular-styleguide)

&nbsp;

> [!NOTE]
> Чтобы создать модуль AngularJS, контроллер, фабрики, директивы и просмотр файлов легко, нужно убедиться, что извлечь Sayed Hashimi [SideWaffle шаблон пакета для Visual Studio](http://sidewaffle.com/). Sayed Hashimi — старший руководитель программы в Visual Studio Team Web корпорации Майкрософт и шаблоны SideWaffle считаются gold standard. На момент написания этой статьи SideWaffle доступна для Visual Studio 2012, 2013 и 2015.

### <a name="routing-and-multiple-views"></a>Служба маршрутизации и несколько представлений

AngularJS имеет встроенные маршрут поставщика для обработки SPA (одностраничного приложения) на основе переходов. Для работы с маршрутизацией в AngularJS, необходимо добавить `angular-route` библиотеки с помощью Bower. Вы увидите на [bower.json](#angular-bower-json) файл, указанный в начале этой статьи, что мы уже на нее имеются ссылки в проект.

После установки пакета добавьте ссылку на скрипт (*angular route.js*) для представления.

Теперь давайте лица приложения мы было создание и добавление навигации. Во-первых, будет выполнено копирование приложения путем создания нового `PeopleController` действие, вызванное `Spa` и соответствующим `Spa.cshtml` представления путем копирования в представлении Index.cshtml в `People` папки. Добавьте ссылку на скрипт `angular-route` (см. строку 11). Также добавьте `div` , отмеченные `ng-view` директивы (см. строку 6) как заполнитель для размещения представлений в. Мы будем использовать некоторые дополнительные *.js* файлы, на которые ссылаются на строках 13-16.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Spa.cshtml?highlight=6,11,12,13,14,15,16)]

Давайте рассмотрим *personModule.js* файл, чтобы увидеть, как мы Создание модуля с маршрутизацией. Мы передаем `ngRoute` как библиотеку в модуль. Этот модуль обработку маршрутизации в приложении.

[!code-javascript[Main](angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personModule.js)]

*PersonRoutes.js* файла, ниже, определяет маршруты, в зависимости от поставщика маршрута. Строки 4-7 определение навигации произнеся эффективно, если URL-адрес с `/persons` — запрошено, используйте шаблон называется `partials/personlist` при одновременной ликвидации `personListController`. Строки с 8 по 11 указывают страницу сведений с параметром маршрута `personId`. Если URL-адрес не соответствует одному из шаблонов, угловая по умолчанию `/persons` представления.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personRoutes.js?highlight=4,5,6,7,8,9,10,11,13)]

`personlist.html` Файл является частичное представление, содержащее только HTML, чтобы отобразить список людей.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personlist.html?highlight=3)]

Контроллер определяется с помощью модуля `controller` функционировать в *personListController.js*.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personListController.js?highlight=1)]

Если запустить это приложение и перейдите к `people/spa#/persons` URL-адрес, мы увидим:

![Представление списка лиц](angular/_static/spa-persons.png)

При переходе на страницу сведений, например `people/spa#/persons/2`, мы увидим частичного представления сведений:

![Подробное представление Person](angular/_static/spa-persons-2.png)

Можно просмотреть полный исходный и все файлы, не отображаются в этой статье на [GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).

### <a name="event-handlers"></a>Обработчики событий

Существует несколько директив в AngularJS, добавить возможности обработки событий в входных элементов в вашей модели HTML DOM. Ниже приведен список событий, которые встроены в AngularJS.

   * `ng-click`

   * `ng-dbl-click`

   * `ng-mousedown`

   * `ng-mouseup`

   * `ng-mouseenter`

   * `ng-mouseleave`

   * `ng-mousemove`

   * `ng-keydown`

   * `ng-keyup`

   * `ng-keypress`

   * `ng-change`

> [!NOTE]
> Можно добавить собственные обработчики событий с помощью [пользовательских директив компонентов в AngularJS](https://docs.angularjs.org/guide/directive).

Давайте взглянем на как `ng-click` построенная событий. Создайте новый файл JavaScript с именем *eventHandlerController.js*и добавьте в него следующее:

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/eventHandlerController.js?highlight=5,6,7)]

Обратите внимание на новое `sayName` функционировать в `eventHandlerController` в строке 5 выше. Выполняет метод для теперь отображается предупреждение JavaScript для пользователя без приветственное сообщение.

Представления привязывает функции контроллера AngularJS события. Строки 9 есть кнопка, на котором `ng-click` применен угловой директивы. Он вызывает нашей `sayName` функции, которая присоединяется к `$scope` объект, переданный в этом представлении.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Events.cshtml?highlight=9)]

Пример выполнения показывает, что контроллер `sayName` функция вызывается автоматически при нажатии кнопки.

![Click-событие](angular/_static/events.png)

Дополнительные сведения о AngularJS Встроенные события обработчика директив нужно убедиться, что заголовок, чтобы [веб-сайт документации](https://docs.angularjs.org/api/ng/directive/ngClick) из AngularJS.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Угловое документы](https://docs.angularjs.org)

* [Угловое Info 2](https://angular.io/)
