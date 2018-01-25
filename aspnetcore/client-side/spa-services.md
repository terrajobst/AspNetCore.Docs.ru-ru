---
title: "С помощью JavaScriptServices для создания приложений на одной странице"
author: scottaddie
description: "Узнайте о преимуществах использования JavaScriptServices для создания одной страницы приложений (SPA) поддерживаемый ASP.NET Core."
ms.author: scaddie
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/spa-services
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 514efcdd78957f999e46c521d0266f092f742538
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
# <a name="using-javascriptservices-for-creating-single-page-applications-with-aspnet-core"></a>С помощью JavaScriptServices для создания приложений на одной странице с помощью ASP.NET Core

По [Scott Addie](https://github.com/scottaddie) и [Fiyaz Hasan](http://fiyazhasan.me/)

Приложение одной странице (SPA) является типом популярных веб-приложения из-за его присущие многофункциональном пользовательском интерфейсе. Интеграция клиентского SPA платформы или библиотеки, такие как [Вращательный](https://angular.io/) или [реагировать](https://facebook.github.io/react/), с серверные платформы, такие как ASP.NET Core могут возникнуть трудности. [JavaScriptServices](https://github.com/aspnet/JavaScriptServices) разработанного для уменьшения трения в процессе интеграции. Он позволяет работать между клиента и сервера технологии стеки.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

<a name="what-is-js-services"></a>

## <a name="what-is-javascriptservices"></a>Что такое JavaScriptServices?

JavaScriptServices — это совокупность технологий клиентские ASP.NET Core. Его задача — размещать в качестве разработчиков предпочтительный серверную платформу для построения SPAs ASP.NET Core.

JavaScriptServices состоит из трех различных пакетов NuGet.
* [Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)
* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)
* [Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)

Эти пакеты полезны, если вы:
* Запускать JavaScript на сервере
* Использовать SPA framework или в библиотеке
* Построение клиентского средства с Webpack

В этой статье внимание следует уделять помещается в с помощью пакета SpaServices.

<a name="what-is-spa-services"></a>

## <a name="what-is-spaservices"></a>Что такое SpaServices?

SpaServices был создан для позиционирования в качестве разработчиков предпочтительный серверную платформу для построения SPAs ASP.NET Core. Для разработки SPAs с ASP.NET Core необязательно SpaServices и он не блокирует в платформу конкретного клиента.

SpaServices предоставляет полезные инфраструктуры, такие как:
* [Серверные предварительной визуализации](#server-prerendering)
* [По промежуточного слоя Webpack разработки](#webpack-dev-middleware)
* [Модуль горячей замены](#hot-module-replacement)
* [Помощники маршрутизации](#routing-helpers)

В совокупности эти компоненты инфраструктуры расширяют рабочий процесс разработки и среду выполнения. Компоненты, может быть использована по отдельности.

<a name="spa-services-prereqs"></a>

## <a name="prerequisites-for-using-spaservices"></a>Предварительные требования для использования SpaServices

Для работы с SpaServices, установите следующее:
* [Node.js](https://nodejs.org/) (версии 6 или более поздней версии) с npm
    * Чтобы проверить эти компоненты установлены и может быть найден, выполните следующую из командной строки:

    ```console
    node -v && npm -v
    ```

Примечание: При развертывании веб-сайте Azure не нужно выполнять никаких действий, &mdash; Node.js установлена и доступна в серверных средах.

* [Пакет SDK для .NET core](https://www.microsoft.com/net/download/core) 1.0 (или более поздней версии)
    * Если вы в Windows, это можно сделать, выбрав Visual Studio 2017 **кросс платформенной разработки .NET Core** рабочей нагрузки.

* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet package

<a name="server-prerendering"></a>

## <a name="server-side-prerendering"></a>Серверные предварительной визуализации

Универсальное приложение (также известный как isomorphic) — это приложение JavaScript, может запускать как на сервере и клиенте. Угловая, реагирование на них и других популярных платформ предоставляют универсальной платформы для этого стиля разработки приложения. Основная идея — сначала отрисовки компоненты framework на сервере через Node.js, а затем делегирует дальнейшего выполнения клиенту.

ASP.NET Core [вспомогательных функций тегов](xref:mvc/views/tag-helpers/intro) предоставляемые SpaServices упрощения реализации предварительной визуализации серверные путем вызова функций JavaScript на сервере.

### <a name="prerequisites"></a>Предварительные требования

Установите следующие компоненты:
* [предварительной визуализации ASPNET](https://www.npmjs.com/package/aspnet-prerendering) пакета npm:

    ```console
    npm i -S aspnet-prerendering
    ```

### <a name="configuration"></a>Конфигурация

Вспомогательных функций тегов вносятся найти через регистрации пространства имен в проекте *_ViewImports.cshtml* файла:

[!code-cshtml[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

Эти вспомогательных функций тегов абстрагируется от особенностей взаимодействовать непосредственно с низкоуровневые интерфейсы API, используя синтаксис, подобный HTML внутри представления Razor:

[!code-cshtml[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="the-asp-prerender-module-tag-helper"></a>`asp-prerender-module` Тег вспомогательный

`asp-prerender-module` Вспомогательный тег, используемый в предыдущем примере кода выполняется *ClientApp/dist/main-server.js* на сервере через Node.js. Для ясности *main server.js* файл — это артефакт задачи transpilation TypeScript для JavaScript в [Webpack](http://webpack.github.io/) процесса построения. Webpack определяет псевдоним точки входа `main-server`; и начинается обход граф зависимостей для данного псевдонима *ClientApp, загрузки server.ts* файла:

[!code-javascript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

В следующем примере углового *ClientApp, загрузки server.ts* использует файл `createServerRenderer` функции и `RenderResult` тип `aspnet-prerendering` пакет npm для настройки подготовки сервера отчетов через Node.js. HTML-разметка, предназначенные для отрисовки на стороне сервера, переданного в вызов функции разрешения, который помещается в JavaScript строго типизированных `Promise` объекта. `Promise` Точность объекта — он асинхронно передает разметку HTML для страницы для вставки в элементе DOM заполнителя.

[!code-typescript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="the-asp-prerender-data-tag-helper"></a>`asp-prerender-data` Тег вспомогательный

При работе с `asp-prerender-module` вспомогательный тег `asp-prerender-data` вспомогательный тега может использоваться для передачи контекстных данных из представления Razor JavaScript на стороне сервера. Например, приведенный ниже код передает пользовательских данных для `main-server` модуля:

[!code-cshtml[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

Данных, полученных `UserName` аргумент сериализуется с использованием как встроенный сериализатор JSON и хранится в `params.data` объекта. В следующем примере углового данных используется для создания индивидуальное приветствие в `h1` элемента:

[!code-typescript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

Примечание: Имена свойств, переданный вспомогательных функций тегов, были представлены **PascalCase** нотации. Сравните это JavaScript, где представлены те же имена свойств с **camelCase**. Конфигурация по умолчанию для сериализации JSON отвечает за это различие.

Для расширения в предыдущем примере кода, данные могут передаваться с сервера к представлению, hydrating `globals` для свойства `resolve` функции:

[!code-typescript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

`postList` Массива, определенные внутри `globals` присоединен объект в браузере глобального `window` объекта. Этой переменной подъем в глобальной области видимости исключает двойной работы, особенно относится к загрузке те же данные один раз на сервере и повторно на клиенте.

![глобальная переменная postList, присоединенные к объекту окна](spa-services/_static/global_variable.png)

<a name="webpack-dev-middleware"></a>

## <a name="webpack-dev-middleware"></a>По промежуточного слоя Webpack разработки

[По промежуточного слоя разработки Webpack](https://webpack.github.io/docs/webpack-dev-middleware.html) предлагает рабочий процесс разработки, при котором Webpack создает ресурсы по требованию. По промежуточного слоя автоматически компилирует и выполняет ресурсами на стороне клиента при перезагрузке страницы в браузере. Альтернативный подход заключается в вручную вызывать Webpack через скрипт построения проекта npm при изменении зависимости сторонних разработчиков или пользовательский код. Npm создания скрипта *package.json* файл показан в следующем примере:

[!code-json[Main](../client-side/spa-services/sample/SpaServicesSampleApp/package.json?range=5)]

### <a name="prerequisites"></a>Предварительные требования

Установите следующие компоненты:
* [ASPNET webpack](https://www.npmjs.com/package/aspnet-webpack) пакета npm:

    ```console
    npm i -D aspnet-webpack
    ```

### <a name="configuration"></a>Конфигурация

По промежуточного слоя разработки Webpack регистрируется в конвейер запросов HTTP через следующий код в *файла Startup.cs* файла `Configure` метод:

[!code-csharp[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=webpack-middleware-registration&highlight=4)]

`UseWebpackDevMiddleware` Метод расширения должен быть вызван перед [регистрация статического файла размещения](xref:fundamentals/static-files) через `UseStaticFiles` метода расширения. По соображениям безопасности зарегистрируйте по промежуточного слоя, только в том случае, когда приложение запускается в режиме разработки.

*Webpack.config.js* файла `output.publicPath` свойство сообщает по промежуточного слоя для отслеживания `dist` папку для изменения:

[!code-javascript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

<a name="hot-module-replacement"></a>

## <a name="hot-module-replacement"></a>Модуль горячей замены

Можно представить в Webpack [горячей замены модуля](https://webpack.github.io/docs/hot-module-replacement-with-webpack.html) компоненте (HMR) с развитием [Webpack разработки по промежуточного слоя](#webpack-dev-middleware). HMR представляет те же преимущества, но дополнительно упрощает рабочего процесса разработки автоматически обновит содержимое страницы после компиляции изменений. Не следует путать с обновление браузера, который может повлиять на текущее состояние в памяти и SPA сеанс отладки. Нет активную связь между службой Webpack разработки по промежуточного слоя и браузер, это означает, что изменения передаются в браузер.

### <a name="prerequisites"></a>Предварительные требования

Установите следующие компоненты:
* [активные промежуточное webpack](https://www.npmjs.com/package/webpack-hot-middleware) пакета npm:

    ```console
    npm i -D webpack-hot-middleware
    ```

### <a name="configuration"></a>Конфигурация

Компонент HMR должен быть зарегистрирован в конвейер запросов HTTP MVC в `Configure` метод:

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

Как и в случае имел значение true, если с [Webpack разработки по промежуточного слоя](#webpack-dev-middleware), `UseWebpackDevMiddleware` метод расширения должен быть вызван перед `UseStaticFiles` метода расширения. По соображениям безопасности зарегистрируйте по промежуточного слоя, только в том случае, когда приложение запускается в режиме разработки.

*Webpack.config.js* необходимо определить файл `plugins` массив, даже если он остается пустым:

[!code-javascript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

После загрузки приложения в браузере, вкладку средства разработчика консоли предоставляет подтверждения активации HMR:

![Сообщение о подключении горячей замены модуля](spa-services/_static/hmr_connected.png)

<a name="routing-helpers"></a>

## <a name="routing-helpers"></a>Помощники маршрутизации

В большинстве ASP.NET Core-SPAs будет необходимо клиентские маршрутизации Помимо маршрутизации на стороне сервера. Системы маршрутизации SPA и MVC могут работать независимо, без помех. , Однако регистр дают один край решить проблемы: определение 404 HTTP-ответов.

Рассмотрим сценарий, в котором без расширений маршрут `/some/page` используется. Предполагается запроса не фильтрует маршрута серверных, но его шаблоном соответствует маршруту клиентские. Теперь рассмотрим входящий запрос `/images/user-512.png`, который обычно ожидает найти файл изображения на сервере. Если этот путь запрашиваемого ресурса не соответствует любой маршрут на стороне сервера или статический файл, маловероятно, что клиентское приложение будет обрабатывать — обычно требуется возвращает код состояния HTTP 404.

### <a name="prerequisites"></a>Предварительные требования

Установите следующие компоненты:
* Клиентские маршрутизации пакета npm. В примере с момента:

    ```console
    npm i -S @angular/router
    ```

### <a name="configuration"></a>Конфигурация

Метод расширения с именем `MapSpaFallbackRoute` используется в `Configure` метод:

[!code-csharp[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=mvc-routing-table&highlight=7-9)]

Совет: Маршруты, вычисляются в порядке, в котором они настроены. Следовательно `default` маршрут в предыдущем примере сначала используется сопоставление по шаблону.

<a name="new-project-creation"></a>

## <a name="creating-a-new-project"></a>Создание нового проекта

JavaScriptServices предоставляет шаблоны предварительно настроенное приложение. SpaServices используется в этих шаблонов в сочетании с помощью различных платформ и библиотек, например угловая, Aurelia, Knockout, реагирование на них и Vue.

Эти шаблоны может быть выполнена с помощью .NET Core CLI, выполнив следующую команду:

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

Отображается список доступных шаблонов SPA:

| Шаблоны                                 | Краткое имя | Язык | Теги        |
|:------------------------------------------|:-----------|:---------|:------------|
| Ядро ASP.NET MVC с углового             | angular    | [C#]     | Web/MVC/SPA |
| Ядро ASP.NET MVC с Aurelia             | aurelia    | [C#]     | Web/MVC/SPA |
| Ядро ASP.NET MVC с Knockout.js         | маскирования   | [C#]     | Web/MVC/SPA |
| Ядро ASP.NET MVC с React.js            | react      | [C#]     | Web/MVC/SPA |
| MVC ASP.NET Core с React.js и локализации  | reactredux | [C#]     | Web/MVC/SPA |
| Ядро ASP.NET MVC с Vue.js              | VUE        | [C#]     | Web/MVC/SPA | 

Чтобы создать новый проект с помощью одного из шаблонов SPA, включают **короткое имя** шаблона в `dotnet new` команды. Следующая команда создает углового приложения с ASP.NET MVC Core настроен на стороне сервера:

```console
dotnet new angular
```

<a name="runtime-config-mode"></a>

### <a name="set-the-runtime-configuration-mode"></a>Установите режим среды выполнения конфигурации

Существует два режима конфигурации основной среды выполнения:
* **Разработка**:
    * Содержит исходное сопоставление в целях отладки.
    * Не Оптимизируйте код на стороне клиента для повышения производительности.
* **Производство**:
    * Исключает исходное сопоставление.
    * Оптимизирует код на стороне клиента через объединение и Минификация.

ASP.NET Core использует переменную среды с именем `ASPNETCORE_ENVIRONMENT` для хранения режим конфигурации. В разделе  **[параметр среды](xref:fundamentals/environments#setting-the-environment)**  для получения дополнительной информации.

### <a name="running-with-net-core-cli"></a>Запуск с .NET Core CLI

Восстановление требуется NuGet и пакета npm, выполнив следующую команду в корне проекта.

```console
dotnet restore && npm i
```

Построение и запуск приложения.

```console
dotnet run
```

Приложение запускается на localhost согласно [режим среды выполнения конфигурации](#runtime-config-mode). Переход к `http://localhost:5000` в браузере отображается главная страница.

### <a name="running-with-visual-studio-2017"></a>Работает с Visual Studio 2017 г.

Откройте *.csproj* файл, созданный `dotnet new` команды. Необходимые пакеты NuGet и npm, автоматически восстанавливаются при открытии проекта. Этот процесс восстановления может занять несколько минут, и приложение готово к запуску после ее завершения. Нажмите зеленую кнопку для запуска или нажмите клавишу `Ctrl + F5`, и в браузере будет открыта на главную страницу приложения. Приложение будет выполняться на localhost согласно [режим среды выполнения конфигурации](#runtime-config-mode). 

<a name="app-testing"></a>

## <a name="testing-the-app"></a>Тестирование приложения

Выполнять тесты на стороне клиента, с помощью шаблонов SpaServices предварительно настроены [Karma](https://karma-runner.github.io/1.0/index.html) и [Jasmine](https://jasmine.github.io/). Jasmine является популярных платформа модульного тестирования для JavaScript, тогда как Karma тестов для этих тестов. Karma настроен для работы с [Webpack разработки по промежуточного слоя](#webpack-dev-middleware) таким образом, что не требуется остановка и запуск теста каждый раз при внесении изменений. Является ли код запуска для тестового случая или сам тестовый случай, тест будет выполняться автоматически.

С помощью угловых приложения в качестве примера, два тестовых случаев Jasmine уже предоставляются для `CounterComponent` в *counter.component.spec.ts* файла:

[!code-typescript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

Откройте окно командной строки, в корне проекта и выполните следующую команду:

```console
npm test
```

Скрипт запускает средство запуска тестов Karma, который считывает параметры, определенные в *karma.conf.js* файла. Вместе с другими параметрами *karma.conf.js* определяет файлы теста для выполнения через его `files` массива:

[!code-javascript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

<a name="app-publishing"></a>

## <a name="publishing-the-application"></a>Публикация приложения

Объединение созданный клиентских средств и опубликованные артефакты ASP.NET Core в готовности развернуть пакет может быть очень обременительным. К счастью, SpaServices управляет всей публикации процесса с пользовательской целевой объект MSBuild, с именем `RunWebpack`:

[!code-xml[Main](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

Целевой объект MSBuild должен выполнить следующие действия:
1. Восстановление пакета npm
1. Создание сборки уровня рабочей сторонних разработчиков, клиентских средств
1. Создание уровня рабочей сборки пользовательских клиентских средств
1. Копирование ресурсов Webpack создается в папке публикации

Целевой объект MSBuild вызывается при выполнении:

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Угловое документы](https://angular.io/docs)
