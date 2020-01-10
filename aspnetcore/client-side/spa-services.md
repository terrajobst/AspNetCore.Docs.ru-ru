---
title: Использование служб JavaScript для создания одностраничных приложений в ASP.NET Core
author: scottaddie
description: Узнайте о преимуществах использования служб JavaScript для создания одностраничного приложения (SPA), поддерживаемого ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 09/06/2019
uid: client-side/spa-services
ms.openlocfilehash: caff1a735de3274b371f67e6e485dc42e579452c
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/09/2020
ms.locfileid: "75828221"
---
# <a name="use-javascript-services-to-create-single-page-applications-in-aspnet-core"></a>Использование служб JavaScript для создания одностраничных приложений в ASP.NET Core

По [Scott Addie](https://github.com/scottaddie) и [Fiyaz Hasan](https://fiyazhasan.me/)

Одностраничное приложение (SPA) — это популярный тип веб-приложения из-за его присущие многофункциональном пользовательском интерфейсе. Интеграция клиентских платформ и библиотек SPA, таких как [угловой](https://angular.io/) или [реагирование](https://facebook.github.io/react/), с серверными платформами, такими как ASP.NET Core, может быть трудной задачей. Службы JavaScript были разработаны для уменьшения трения в процессе интеграции. Он позволяет работать между различными клиентскими и стеков технологий сервера.

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> Функции, описанные в этой статье, устарели по ASP.NET Core 3,0. В пакете NuGet [Microsoft. AspNetCore. спасервицес. Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices.Extensions) доступен более простой механизм интеграции с алгоритмом Spa. Дополнительные сведения см. в разделе [[объявление] Обсолетинг Microsoft. AspNetCore. спасервицес и Microsoft. AspNetCore. нодесервицес](https://github.com/dotnet/AspNetCore/issues/12890).

::: moniker-end

## <a name="what-is-javascript-services"></a>Что такое службы JavaScript

Службы JavaScript — это набор технологий на стороне клиента для ASP.NET Core. Наша цель — для размещения ASP.NET Core разработчиков предпочтительной платформой на стороне сервера для построения одностраничных приложений.

Службы JavaScript состоят из двух разных пакетов NuGet:

* [Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)
* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)

Эти пакеты полезны в следующих сценариях:

* Выполните на сервере JavaScript
* Использование библиотеки или платформы одностраничных ПРИЛОЖЕНИЙ
* Создание активов на стороне клиента с помощью Webpack

В этой статье внимание следует уделять помещается в с помощью пакета SpaServices.

## <a name="what-is-spaservices"></a>Что такое SpaServices

SpaServices был создан для размещения ASP.NET Core разработчиков предпочтительной платформой на стороне сервера для построения одностраничных приложений. Спасервицес не требуется для разработки одностраничные приложения с ASP.NET Core и не блокирует разработчиков в определенной клиентской платформе.

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

  * При развертывании на веб-сайт Azure никаких действий не требуется,&mdash;узел Node. js установлен и доступен в серверных средах.

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * В Windows с помощью Visual Studio 2017 пакет SDK устанавливается путем выбора рабочей нагрузки **кросс-платформенная разработка .NET Core** .

* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) пакета NuGet

## <a name="server-side-prerendering"></a>Предварительной визуализации на стороне сервера

Универсальное приложение (также известный как isomorphic) — это приложение JavaScript, может запускать как на сервере и на клиенте. Angular, React и других популярных платформ предоставляют универсальной платформы для этого стиля разработки приложения. Идея состоит в том, чтобы сначала отрисовки компоненты платформы на сервере с помощью Node.js, а затем делегирует дальнейшего выполнения клиенту.

ASP.NET Core [вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) предоставляемые SpaServices упрощают реализацию предварительной визуализации на сервере путем вызова функции JavaScript на сервере.

### <a name="server-side-prerendering-prerequisites"></a>Предварительная подготовка необходимых компонентов на стороне сервера

Установите пакет NPM для [визуализации ASPNET](https://www.npmjs.com/package/aspnet-prerendering) :

```console
npm i -S aspnet-prerendering
```

### <a name="server-side-prerendering-configuration"></a>Конфигурация предварительной подготовки к просмотру на стороне сервера

Вспомогательные функции тегов выполняются с помощью функции регистрации пространства имен в проекте *_ViewImports.cshtml* файла:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

Эти вспомогательные функции тегов абстрагироваться от конкретных особенностей взаимодействовать непосредственно с низкоуровневых API за счет использования HTML-синтаксис типа в представлении Razor:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="asp-prerender-module-tag-helper"></a>Вспомогательная функция тега ASP-PreRender-Module

`asp-prerender-module` Вспомогательной функции тега, используемый в предыдущем примере код выполняет *ClientApp/dist/main-server.js* на сервере с помощью Node.js. Для ясности *main server.js* файл — это артефакт задачи транспилирования TypeScript для JavaScript в [Webpack](https://webpack.github.io/) процесс сборки. Webpack определяется псевдоним точки входа `main-server`; и начинается обход графа зависимостей для данного псевдонима *ClientApp/boot-server.ts* файла:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

В следующем примере Angular *ClientApp/boot-server.ts* использует файл `createServerRenderer` функции и `RenderResult` тип `aspnet-prerendering` пакета npm для настройки подготовки сервера отчетов с помощью Node.js. HTML-разметка, предназначенные для отрисовки на стороне сервера, переданного в вызов функции разрешения, который обернут в JavaScript со строгой типизацией `Promise` объекта. `Promise` Точность объекта —, он асинхронно передает разметку HTML для страницы для внедрения в DOM замещающего элемента.

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="asp-prerender-data-tag-helper"></a>Вспомогательная функция тега ASP-PreRender-Data

При связывании с `asp-prerender-module` вспомогательной функции тега, `asp-prerender-data` вспомогательная функция тега может использоваться для передачи контекстно-зависимые сведения из представления Razor для JavaScript на стороне сервера. Например, следующая разметка передает данные на `main-server` модуля:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

Полученных `UserName` аргумент сериализуется с помощью встроенного сериализатора JSON и хранятся в `params.data` объекта. В следующем примере Angular данных используется для создания индивидуальное приветствие в `h1` элемент:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

Имена свойств, переданные в вспомогательные функции тегов, представлены с помощью нотации **PascalCase** . Сравните это JavaScript, где представлены те же имена свойств **camelCase**. Конфигурация по умолчанию для сериализации JSON несет ответственность за это различие.

Для обзора в предыдущем примере кода, данные могут передаваться с сервера в представление с hydrating `globals` указано свойство для `resolve` функции:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

`postList` Массива, определенные внутри `globals` объект присоединяется к браузера глобального `window` объекта. Этой переменной подъем глобальную область устраняет двойной работы, особенно в том случае, поскольку это имеет отношение к загрузке те же данные один раз на сервере и снова на стороне клиента.

![глобальная переменная postList, присоединенный к объекту окна](spa-services/_static/global_variable.png)

## <a name="webpack-dev-middleware"></a>По промежуточного слоя Webpack разработки

[По промежуточного слоя разработки Webpack](https://webpack.js.org/guides/development/#using-webpack-dev-middleware) предлагает рабочий процесс разработки, при котором Webpack создает ресурсы по требованию. По промежуточного слоя автоматически компилирует и выполняет ресурсы на стороне клиента при перезагрузке страницы в браузере. Альтернативный подход заключается в том, необходимо вручную вызвать Webpack через скрипт сборки проекта npm при изменении зависимость стороннего или пользовательского кода. Сценарий построения npm *package.json* файл отображается в следующем примере:

```json
"build": "npm run build:vendor && npm run build:custom",
```

### <a name="webpack-dev-middleware-prerequisites"></a>Предварительные требования для по промежуточного слоя разработки пакета

Установите пакет [ASPNET-](https://www.npmjs.com/package/aspnet-webpack) NPM:

```console
npm i -D aspnet-webpack
```

### <a name="webpack-dev-middleware-configuration"></a>Конфигурация по промежуточного слоя разработки пакета

По промежуточного слоя разработки Webpack регистрируется в конвейер запросов HTTP с помощью следующего кода в *Startup.cs* файла `Configure` метод:

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_WebpackMiddlewareRegistration&highlight=4)]

`UseWebpackDevMiddleware` Метод расширения должен быть вызван перед [регистрации размещения статического файла](xref:fundamentals/static-files) через `UseStaticFiles` метода расширения. По соображениям безопасности зарегистрируйте по промежуточного слоя, только в том случае, когда приложение запускается в режиме разработки.

*Webpack.config.js* файла `output.publicPath` свойство сообщает по промежуточного слоя, чтобы просмотреть `dist` изменений в папке:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

## <a name="hot-module-replacement"></a>Модуль горячей замены

Можно рассматривать его Webpack [горячей замены модуля](https://webpack.js.org/concepts/hot-module-replacement/) компонента (HMR), как развитием [по промежуточного слоя разработки Webpack](#webpack-dev-middleware). HMR представляет те же преимущества, но дополнительно упрощает рабочий процесс разработки, автоматически обновляя содержимое страницы после компиляции изменений. Не следует путать с обновить браузер, который может повлиять на текущее состояние в памяти и сеанс отладки из SPA. Нет активную связь между службой по промежуточного слоя разработки Webpack и браузера, это означает, что изменения будут передаваться в браузер.

### <a name="hot-module-replacement-prerequisites"></a>Предварительные требования для замены активных модулей

Установите пакет NPM для " [горячего по промежуточного слоя](https://www.npmjs.com/package/webpack-hot-middleware) ".

```console
npm i -D webpack-hot-middleware
```

### <a name="hot-module-replacement-configuration"></a>Горячая замена конфигурации модуля

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

В большинстве ASP.NET Core случаев одностраничные приложения на стороне клиента часто требуется маршрутизация на клиентские службы в дополнение к маршрутизации на стороне сервера. Не мешая систем маршрутизации SPA и MVC могут работать независимо. , Однако проблемы вариантов дают один edge: определение ответы HTTP 404.

Рассмотрим сценарий, в котором без расширений маршрут `/some/page` используется. Предположим, запрос не фильтрует маршрут на стороне сервера, но его шаблон соответствует маршрут со стороны клиента. Теперь рассмотрим входящий запрос `/images/user-512.png`, который обычно ожидает найти файл изображения на сервере. Если запрошенный путь к ресурсу не соответствует ни одному маршруту на стороне сервера или статическому файлу, то маловероятно, что клиентское приложение будет его обработано&mdash;обычно возвращая код состояния HTTP 404.

### <a name="routing-helpers-prerequisites"></a>Предварительные требования для вспомогательных функций маршрутизации

Установите пакет NPM маршрутизации на стороне клиента. Используя Angular в качестве примера:

```console
npm i -S @angular/router
```

### <a name="routing-helpers-configuration"></a>Настройка вспомогательных функций маршрутизации

Метод расширения с именем `MapSpaFallbackRoute` используется в `Configure` метод:

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_MvcRoutingTable&highlight=7-9)]

Маршруты оцениваются в том порядке, в котором они настроены. Следовательно `default` маршрута в приведенном выше примере кода используется сначала для сопоставления шаблонов.

## <a name="create-a-new-project"></a>Создание нового проекта

Службы JavaScript предоставляют предварительно настроенные шаблоны приложений. Спасервицес используется в этих шаблонах в сочетании с различными платформами и библиотеками, такими как угловые, реагирующие и Redux.

Эти шаблоны можно установить с помощью интерфейса командной строки .NET Core, выполнив следующую команду:

```dotnetcli
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

Отображается список доступных шаблонов одностраничных ПРИЛОЖЕНИЙ:

| Шаблоны                                 | Сокращенное имя | Language | Tags        |
| ------------------------------------------| :--------: | :------: | :---------: |
| MVC ASP.NET Core с Angular             | angular    | [C#]     | MVC/Веб/SPA |
| MVC ASP.NET Core с React.js            | react      | [C#]     | MVC/Веб/SPA |
| MVC ASP.NET Core с React.js и Redux  | reactredux | [C#]     | MVC/Веб/SPA |

Чтобы создать новый проект с помощью одного из шаблонов одностраничных ПРИЛОЖЕНИЙ, включают **короткое имя** шаблона в [команды dotnet new](/dotnet/core/tools/dotnet-new) команды. Следующая команда создает Angular приложения с ASP.NET Core MVC, который настроен на стороне сервера:

```dotnetcli
dotnet new angular
```

### <a name="set-the-runtime-configuration-mode"></a>Включить режим конфигурации среды выполнения

Существует два режима конфигурации основной среды выполнения:

* **Разработка**:
  * Включает в себя исходных сопоставлений в целях отладки.
  * Не предназначен для оптимизации клиентского кода для повышения производительности.
* **Рабочая среда**:
  * Исключает исходных сопоставлений.
  * Оптимизирует код на стороне клиента с помощью объединения и минификации.

ASP.NET Core использует переменную среды с именем `ASPNETCORE_ENVIRONMENT` для хранения режим конфигурации. Дополнительные сведения см. [в разделе Задание среды](xref:fundamentals/environments#set-the-environment).

### <a name="run-with-net-core-cli"></a>Запуск с .NET Core CLI

Восстановление NuGet требуется и пакеты npm, выполнив следующую команду в корневом каталоге проекта:

```dotnetcli
dotnet restore && npm i
```

Построение и запуск приложения.

```dotnetcli
dotnet run
```

Запускает приложения на localhost в соответствии с [режим конфигурации среды выполнения](#set-the-runtime-configuration-mode). Переход к `http://localhost:5000` в браузере отображает целевую страницу.

### <a name="run-with-visual-studio-2017"></a>Запуск с Visual Studio 2017

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

Дополнительные сведения о публикации в Azure см. в [этой ошибке GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/12474) .

Объединение созданных ресурсов на стороне клиента и опубликованных артефакты ASP.NET Core в готовые к развертыванию пакет может быть громоздким. К счастью, SpaServices организует этот процесс всей публикации с пользовательский целевой объект MSBuild с именем `RunWebpack`:

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

Целевой объект MSBuild должен выполнить следующие:

1. Восстановите пакеты NPM.
1. Создайте сборку производственного уровня сторонних клиентских ресурсов.
1. Создание сборки настраиваемых клиентских ресурсов на рабочем уровне.
1. Скопируйте созданные в составе пакета ресурсы в папку публикации.

Целевой объект MSBuild вызывается при запуске:

```dotnetcli
dotnet publish -c Release
```

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Документация по angular](https://angular.io/docs)
