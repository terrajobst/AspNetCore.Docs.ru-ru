---
title: Использование служб JavaScript для создания одностраничных приложений ASP.NET Core
author: scottaddie
description: Узнайте о преимуществах использования службы JavaScript для создания одностраничных приложений (SPA) с ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 05/28/2019
uid: client-side/spa-services
ms.openlocfilehash: 19710b58bca606d21feda9069ad00edd1e4f72e9
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67813474"
---
# <a name="use-javascript-services-to-create-single-page-applications-in-aspnet-core"></a>Использование служб JavaScript для создания одностраничных приложений ASP.NET Core

По [Scott Addie](https://github.com/scottaddie) и [Fiyaz Hasan](https://fiyazhasan.me/)

Одностраничное приложение (SPA) — это популярный тип веб-приложения из-за его присущие многофункциональном пользовательском интерфейсе. Интеграция клиентские платформы одностраничных ПРИЛОЖЕНИЙ или библиотек, таких как [Angular](https://angular.io/) или [React](https://facebook.github.io/react/), серверные платформы, такие как ASP.NET Core довольно сложно. Службы JavaScript был разработан для трудностей, в процессе интеграции. Он позволяет работать между различными клиентскими и стеков технологий сервера.

## <a name="what-is-javascript-services"></a>Что такое службы JavaScript

Службы JavaScript — это совокупность технологий на стороне клиента для ASP.NET Core. Наша цель — для размещения ASP.NET Core разработчиков предпочтительной платформой на стороне сервера для построения одностраничных приложений.

Службы JavaScript состоят из двух различных пакетов NuGet:

* [Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)
* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)

Эти пакеты полезны в следующих сценариях:

* Выполните на сервере JavaScript
* Использование библиотеки или платформы одностраничных ПРИЛОЖЕНИЙ
* Создание активов на стороне клиента с помощью Webpack

В этой статье внимание следует уделять помещается в с помощью пакета SpaServices.

## <a name="what-is-spaservices"></a>Что такое SpaServices

SpaServices был создан для размещения ASP.NET Core разработчиков предпочтительной платформой на стороне сервера для построения одностраничных приложений. SpaServices не обязательно должна разработать одностраничных приложений с помощью ASP.NET Core, и он не блокирует разработчиков в платформу, конкретного клиента.

SpaServices предоставляет полезные инфраструктуры, например:

* [Предварительной визуализации на стороне сервера](#server-side-prerendering)
* [По промежуточного слоя Webpack разработки](#webpack-dev-middleware)
* [Модуль горячей замены](#hot-module-replacement)
* [Вспомогательные функции маршрутизации](#routing-helpers)

В совокупности эти компоненты инфраструктуры улучшить рабочий процесс разработки и возможности среды выполнения. Компоненты, может быть использована по отдельности.

## <a name="prerequisites-for-using-spaservices"></a>Предварительные требования для использования SpaServices

Для работы с SpaServices, установите следующее:

* [Node.js](https://nodejs.org/) (версия 6 или более поздней версии) с помощью npm

  * Чтобы проверить эти компоненты устанавливаются и может быть найден, выполните следующую команду из командной строки:

    ```console
    node -v && npm -v
    ```

  * Если развертывание веб-сайте Azure, никаких действий не требуется&mdash;Node.js установлена и доступна в серверных средах.

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * В Windows, с помощью Visual Studio 2017, установлен пакет SDK, выбрав **кросс Платформенная разработка .NET Core** рабочей нагрузки.

* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) пакета NuGet

## <a name="server-side-prerendering"></a>Предварительной визуализации на стороне сервера

Универсальное приложение (также известный как isomorphic) — это приложение JavaScript, может запускать как на сервере и на клиенте. Angular, React и других популярных платформ предоставляют универсальной платформы для этого стиля разработки приложения. Идея состоит в том, чтобы сначала отрисовки компоненты платформы на сервере с помощью Node.js, а затем делегирует дальнейшего выполнения клиенту.

ASP.NET Core [вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) предоставляемые SpaServices упрощают реализацию предварительной визуализации на сервере путем вызова функции JavaScript на сервере.

### <a name="server-side-prerendering-prerequisites"></a>Необходимые компоненты предварительной отрисовки на стороне сервера

Установка [предварительной визуализации aspnet](https://www.npmjs.com/package/aspnet-prerendering) пакета npm:

```console
npm i -S aspnet-prerendering
```

### <a name="server-side-prerendering-configuration"></a>Настройка предварительной отрисовки на стороне сервера

Вспомогательные функции тегов выполняются с помощью функции регистрации пространства имен в проекте *_ViewImports.cshtml* файла:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

Эти вспомогательные функции тегов абстрагироваться от конкретных особенностей взаимодействовать непосредственно с низкоуровневых API за счет использования HTML-синтаксис типа в представлении Razor:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="asp-prerender-module-tag-helper"></a>Вспомогательная функция тега ASP-prerender-module

`asp-prerender-module` Вспомогательной функции тега, используемый в предыдущем примере код выполняет *ClientApp/dist/main-server.js* на сервере с помощью Node.js. Для ясности *main server.js* файл — это артефакт задачи транспилирования TypeScript для JavaScript в [Webpack](https://webpack.github.io/) процесс сборки. Webpack определяется псевдоним точки входа `main-server`; и начинается обход графа зависимостей для данного псевдонима *ClientApp/boot-server.ts* файла:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

В следующем примере Angular *ClientApp/boot-server.ts* использует файл `createServerRenderer` функции и `RenderResult` тип `aspnet-prerendering` пакета npm для настройки подготовки сервера отчетов с помощью Node.js. HTML-разметка, предназначенные для отрисовки на стороне сервера, переданного в вызов функции разрешения, который обернут в JavaScript со строгой типизацией `Promise` объекта. `Promise` Точность объекта —, он асинхронно передает разметку HTML для страницы для внедрения в DOM замещающего элемента.

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="asp-prerender-data-tag-helper"></a>Вспомогательная функция тега ASP-prerender-data

При связывании с `asp-prerender-module` вспомогательной функции тега, `asp-prerender-data` вспомогательная функция тега может использоваться для передачи контекстно-зависимые сведения из представления Razor для JavaScript на стороне сервера. Например, следующая разметка передает данные на `main-server` модуля:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

Полученных `UserName` аргумент сериализуется с помощью встроенного сериализатора JSON и хранятся в `params.data` объекта. В следующем примере Angular данных используется для создания индивидуальное приветствие в `h1` элемент:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

Имена свойств, переданный вспомогательные функции тегов представляются с помощью **PascalCase** нотации. Сравните это JavaScript, где представлены те же имена свойств **camelCase**. Конфигурация по умолчанию для сериализации JSON несет ответственность за это различие.

Для обзора в предыдущем примере кода, данные могут передаваться с сервера в представление с hydrating `globals` указано свойство для `resolve` функции:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

`postList` Массива, определенные внутри `globals` объект присоединяется к браузера глобального `window` объекта. Этой переменной подъем глобальную область устраняет двойной работы, особенно в том случае, поскольку это имеет отношение к загрузке те же данные один раз на сервере и снова на стороне клиента.

![глобальная переменная postList, присоединенный к объекту окна](spa-services/_static/global_variable.png)

## <a name="webpack-dev-middleware"></a>По промежуточного слоя Webpack разработки

[По промежуточного слоя разработки Webpack](https://webpack.js.org/guides/development/#using-webpack-dev-middleware) предлагает рабочий процесс разработки, при котором Webpack создает ресурсы по требованию. По промежуточного слоя автоматически компилирует и выполняет ресурсы на стороне клиента при перезагрузке страницы в браузере. Альтернативный подход заключается в том, необходимо вручную вызвать Webpack через скрипт сборки проекта npm при изменении зависимость стороннего или пользовательского кода. Сценарий построения npm *package.json* файл отображается в следующем примере:

```json
"build": "npm run build:vendor && npm run build:custom",
```

### <a name="webpack-dev-middleware-prerequisites"></a>Предварительные требования по промежуточного слоя Webpack разработки

Установка [aspnet webpack](https://www.npmjs.com/package/aspnet-webpack) пакета npm:

```console
npm i -D aspnet-webpack
```

### <a name="webpack-dev-middleware-configuration"></a>Конфигурации по промежуточного слоя Webpack разработки

По промежуточного слоя разработки Webpack регистрируется в конвейер запросов HTTP с помощью следующего кода в *Startup.cs* файла `Configure` метод:

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_WebpackMiddlewareRegistration&highlight=4)]

`UseWebpackDevMiddleware` Метод расширения должен быть вызван перед [регистрации размещения статического файла](xref:fundamentals/static-files) через `UseStaticFiles` метода расширения. По соображениям безопасности зарегистрируйте по промежуточного слоя, только в том случае, когда приложение запускается в режиме разработки.

*Webpack.config.js* файла `output.publicPath` свойство сообщает по промежуточного слоя, чтобы просмотреть `dist` изменений в папке:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

## <a name="hot-module-replacement"></a>Модуль горячей замены

Можно рассматривать его Webpack [горячей замены модуля](https://webpack.js.org/concepts/hot-module-replacement/) компонента (HMR), как развитием [по промежуточного слоя разработки Webpack](#webpack-dev-middleware). HMR представляет те же преимущества, но дополнительно упрощает рабочий процесс разработки, автоматически обновляя содержимое страницы после компиляции изменений. Не следует путать с обновить браузер, который может повлиять на текущее состояние в памяти и сеанс отладки из SPA. Нет активную связь между службой по промежуточного слоя разработки Webpack и браузера, это означает, что изменения будут передаваться в браузер.

### <a name="hot-module-replacement-prerequisites"></a>"Горячий" необходимые условия для замены модуля

Установка [webpack-"Горячий"-по промежуточного слоя](https://www.npmjs.com/package/webpack-hot-middleware) пакета npm:

```console
npm i -D webpack-hot-middleware
```

### <a name="hot-module-replacement-configuration"></a>Горячей замены модуля конфигурации

Компонент HMR должны быть зарегистрированы в конвейер запросов HTTP MVC в `Configure` метод:

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

Как было [по промежуточного слоя разработки Webpack](#webpack-dev-middleware), `UseWebpackDevMiddleware` метод расширения должен быть вызван перед `UseStaticFiles` метода расширения. По соображениям безопасности зарегистрируйте по промежуточного слоя, только в том случае, когда приложение запускается в режиме разработки.

*Webpack.config.js* необходимо определить файл `plugins` массив, даже если оставить эти поля пустыми:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

После загрузки приложения в браузере, вкладку средства разработчика консоли предоставляет подтверждение HMR активации:

![Сообщение о подключении горячей замены модуля](spa-services/_static/hmr_connected.png)

## <a name="routing-helpers"></a>Вспомогательные функции маршрутизации

В большинстве на базе ASP.NET Core одностраничные приложения маршрутизацию на стороне клиента часто желательно Помимо маршрутизации на стороне сервера. Не мешая систем маршрутизации SPA и MVC могут работать независимо. , Однако проблемы вариантов дают один edge: определение ответы HTTP 404.

Рассмотрим сценарий, в котором без расширений маршрут `/some/page` используется. Предположим, запрос не фильтрует маршрут на стороне сервера, но его шаблон соответствует маршрут со стороны клиента. Теперь рассмотрим входящий запрос `/images/user-512.png`, который обычно ожидает найти файл изображения на сервере. Если этот путь запрошенного ресурса не соответствует любой маршрут на стороне сервера или статических файлов, маловероятно, что клиентское приложение будет обрабатывать его&mdash;требуется обычно возвращает код состояния HTTP 404.

### <a name="routing-helpers-prerequisites"></a>Маршрутизации необходимые вспомогательные функции

Установите пакет npm маршрутизации на стороне клиента. Используя Angular в качестве примера:

```console
npm i -S @angular/router
```

### <a name="routing-helpers-configuration"></a>Конфигурация маршрутизации вспомогательные функции

Метод расширения с именем `MapSpaFallbackRoute` используется в `Configure` метод:

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_MvcRoutingTable&highlight=7-9)]

Маршруты вычисляются в порядке, в котором они настроены. Следовательно `default` маршрута в приведенном выше примере кода используется сначала для сопоставления шаблонов.

## <a name="create-a-new-project"></a>Создание нового проекта

JavaScript службы предоставляют шаблонов предварительно настроенных приложений. SpaServices используется в этих шаблонах в сочетании с различных платформ и библиотек, например Angular, React и Redux.

Эти шаблоны можно установить с помощью интерфейса командной строки .NET Core, выполнив следующую команду:

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

Отображается список доступных шаблонов одностраничных ПРИЛОЖЕНИЙ:

| Шаблоны                                 | Краткое имя | Язык | Теги        |
| ------------------------------------------| :--------: | :------: | :---------: |
| MVC ASP.NET Core с Angular             | angular    | [C#]     | Web/MVC/SPA |
| MVC ASP.NET Core с React.js            | react      | [C#]     | Web/MVC/SPA |
| MVC ASP.NET Core с React.js и Redux  | reactredux | [C#]     | Web/MVC/SPA |

Чтобы создать новый проект с помощью одного из шаблонов одностраничных ПРИЛОЖЕНИЙ, включают **короткое имя** шаблона в [команды dotnet new](/dotnet/core/tools/dotnet-new) команды. Следующая команда создает Angular приложения с ASP.NET Core MVC, который настроен на стороне сервера:

```console
dotnet new angular
```

### <a name="set-the-runtime-configuration-mode"></a>Включить режим конфигурации среды выполнения

Существует два режима конфигурации основной среды выполнения:

* **Разработка**:
  * Включает в себя исходных сопоставлений в целях отладки.
  * Не предназначен для оптимизации клиентского кода для повышения производительности.
* **Производство**:
  * Исключает исходных сопоставлений.
  * Оптимизирует клиентского кода с помощью объединения и минификации.

ASP.NET Core использует переменную среды с именем `ASPNETCORE_ENVIRONMENT` для хранения режим конфигурации. Дополнительные сведения см. в разделе [среду](xref:fundamentals/environments#set-the-environment).

### <a name="run-with-net-core-cli"></a>Запустить с помощью .NET Core CLI

Восстановление NuGet требуется и пакеты npm, выполнив следующую команду в корневом каталоге проекта:

```console
dotnet restore && npm i
```

Построение и запуск приложения.

```console
dotnet run
```

Запускает приложения на localhost в соответствии с [режим конфигурации среды выполнения](#set-the-runtime-configuration-mode). Переход к `http://localhost:5000` в браузере отображает целевую страницу.

### <a name="run-with-visual-studio-2017"></a>Запустить с помощью Visual Studio 2017

Откройте *.csproj* файла, созданного [команды dotnet new](/dotnet/core/tools/dotnet-new) команды. Необходимые пакеты NuGet и npm, автоматически восстанавливаются при открытии проекта. Этот процесс восстановления может занять несколько минут, и приложение будет готово для запуска после ее завершения. Щелкните зеленую кнопку запуска или нажмите клавишу `Ctrl + F5`, и в браузере откроется целевая страница приложения. Приложение выполняется на локальном компьютере в соответствии с [режим конфигурации среды выполнения](#set-the-runtime-configuration-mode).

## <a name="test-the-app"></a>Тестирование приложения

Шаблоны SpaServices предварительно настроены для запуска тестов на стороне клиента, с помощью [Karma](https://karma-runner.github.io/1.0/index.html) и [Jasmine](https://jasmine.github.io/). Jasmine — популярный платформа модульного тестирования для JavaScript, а Karma — тестов для этих тестов. Karma настроен для работы с [по промежуточного слоя разработки Webpack](#webpack-dev-middleware) таким образом, чтобы разработчику не нужно будет остановить и запустить тест, каждый раз при внесении изменений. Будь то код, выполняемый тестовый случай или сам тестовый случай, тест выполняется автоматически.

С помощью приложения Angular, например, два Жасмин тестовых случаев уже предоставляются для `CounterComponent` в *counter.component.spec.ts* файла:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

Откройте командную строку в *ClientApp* каталога. Выполните следующую команду:

```console
npm test
```

Сценарий запускает средство запуска тестов Karma, который считывает параметры, определенные в *karma.conf.js* файла. Вместе с другими параметрами *karma.conf.js* определяет файлы теста для выполнения с помощью его `files` массива:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

## <a name="publish-the-app"></a>Публикация приложения

Объединение созданных ресурсов на стороне клиента и опубликованных артефакты ASP.NET Core в готовые к развертыванию пакет может быть громоздким. К счастью, SpaServices организует этот процесс всей публикации с пользовательский целевой объект MSBuild с именем `RunWebpack`:

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

Целевой объект MSBuild должен выполнить следующие:

1. Восстановление пакетов npm.
1. Создание средств сторонними разработчиками, клиентские сборки производственного уровня.
1. Создание пользовательских средств клиентские сборки производственного уровня.
1. Скопируйте созданный Webpack ресурсы в папку публикации.

Целевой объект MSBuild вызывается при запуске:

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Документация по angular](https://angular.io/docs)
