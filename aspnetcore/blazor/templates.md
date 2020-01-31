---
title: Шаблоны Blazor ASP.NET Core
author: guardrex
description: Сведения о ASP.NET Core Blazor шаблонах приложений и структуре проекта Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2020
no-loc:
- Blazor
- SignalR
uid: blazor/templates
ms.openlocfilehash: acfa4b8a42cbd310c6fc6dc973573578e94ef999
ms.sourcegitcommit: c81ef12a1b6e6ac838e5e07042717cf492e6635b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2020
ms.locfileid: "76885506"
---
# <a name="aspnet-core-opno-locblazor-templates"></a>Шаблоны Blazor ASP.NET Core

Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Платформа Blazor предоставляет шаблоны для разработки приложений для каждой из моделей размещения Blazor.

* Blazorая сборка (`blazorwasm`)
* Сервер Blazor (`blazorserver`)

Дополнительные сведения о моделях размещения Blazorсм. в разделе <xref:blazor/hosting-models>.

Пошаговые инструкции по созданию Blazor приложения из шаблона см. в разделе <xref:blazor/get-started>.

## <a name="opno-locblazor-project-structure"></a>Структура проекта Blazor

Следующие файлы и папки составляют Blazor приложение, созданное на основе шаблона Blazor:

* *Program.cs* &ndash; точку входа приложения, которая настраивает:

  * [Узел](xref:fundamentals/host/generic-host) ASP.NET Core (Blazor Server)
  * Узел веб-сборки (Blazor веб-сборка) &ndash; код в этом файле уникален для приложений, созданных из Blazor шаблона веб-сборки (`blazorwasm`).
    * Компонент `App`, который является корневым компонентом приложения, указывается в качестве `app` элемента DOM для метода `Add`.
    * Службы можно настроить с помощью метода `ConfigureServices` в построителе узлов (например, `builder.Services.AddSingleton<IMyDependency, MyDependency>();`).
    * Конфигурацию можно указать с помощью построителя узлов (`builder.Configuration`).

* *Startup.CS* (Blazor Server) &ndash; содержит логику запуска приложения. Класс `Startup` определяет два метода:

  * `ConfigureServices` &ndash; настраивает службы [внедрения зависимостей (DI)](xref:fundamentals/dependency-injection) приложения. В Blazor серверных приложений службы добавляются путем вызова <xref:Microsoft.Extensions.DependencyInjection.ComponentServiceCollectionExtensions.AddServerSideBlazor*>, а `WeatherForecastService` добавляется в контейнер службы для использования в примере `FetchData` компонента.
  * `Configure` &ndash; настраивает конвейер обработки запросов приложения:
    * <xref:Microsoft.AspNetCore.Builder.ComponentEndpointRouteBuilderExtensions.MapBlazorHub*> вызывается для настройки конечной точки для соединения в режиме реального времени с браузером. Соединение создается с помощью [SignalR](xref:signalr/introduction), которое является платформой для добавления веб-функций в режиме реального времени к приложениям.
    * [Мапфаллбакктопаже ("/_Host")](xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapFallbackToPage*) вызывается для настройки корневой страницы приложения (*pages/_Host. cshtml*) и включения навигации.

* *wwwroot/index.HTML* (Blazor веб-сборка) &ndash; корневая страница приложения, реализованная как HTML-страница:
  * При первом запросе любой страницы приложения Эта страница подготавливается к просмотру и возвращается в ответе.
  * На странице указывается, где отображается корневой компонент `App`. Компонент `App` (*app. Razor*) указан в качестве `app` элемента DOM для метода `AddComponent` в `Startup.Configure`.
  * Загружается файл `_framework/blazor.webassembly.js` JavaScript, который:
    * Скачивает среду выполнения .NET, приложение и зависимости приложения.
    * Инициализирует среду выполнения для запуска приложения.

* *Pages/_Host. cshtml* (Blazor Server) &ndash; корневую страницу приложения, реализованного в виде страницы Razor:
  * При первом запросе любой страницы приложения Эта страница подготавливается к просмотру и возвращается в ответе.
  * Загружается файл `_framework/blazor.server.js` JavaScript, который настраивает подключение SignalR в режиме реального времени между браузером и сервером.
  * На странице узла указывается, где отображается корневой компонент `App` (*app. Razor*).

* *App. razor* &ndash; корневой компонент приложения, который настраивает маршрутизацию на стороне клиента с помощью компонента <xref:Microsoft.AspNetCore.Components.Routing.Router>. Компонент `Router` перехватывает навигацию браузера и отображает страницу, соответствующую запрошенному адресу.

* Папка *страниц* &ndash; содержит маршрутизируемые компоненты и страницы ( *. Razor*), которые составляют приложение Blazor. Маршрут для каждой страницы указывается с помощью директивы [`@page`](xref:mvc/views/razor#page) . Шаблон включает следующие компоненты:
  * `Index` (*index. Razor*) &ndash; реализует домашнюю страницу.
  * `Counter` (*Counter. Razor*) &ndash; реализует страницу счетчика.
  * `Error` (*ошибка. Razor*, только серверное приложение Blazor) &ndash; отображаться при возникновении необработанного исключения в приложении.
  * `FetchData` (*FetchData. Razor*) &ndash; реализует страницу Выбор данных.

* *Общая* папка &ndash; содержит другие компоненты пользовательского интерфейса (*Razor*), используемые приложением:
  * `MainLayout` (*маинлайаут. Razor*) &ndash; компонент макета приложения.
  * `NavMenu` (*навмену. Razor*) &ndash; реализует навигацию по боковой панели. Включает [компонент навлинк](xref:blazor/routing#navlink-component) (<xref:Microsoft.AspNetCore.Components.Routing.NavLink>), который визуализирует ссылки навигации на другие компоненты Razor. Компонент `NavLink` автоматически указывает выбранное состояние при загрузке компонента, что помогает пользователю понять, какой компонент в данный момент отображается.

* *_Imports. razor* &ndash; содержит общие директивы Razor для включения в компоненты приложения ( *. Razor*), такие как директивы [`@using`](xref:mvc/views/razor#using) для пространств имен.

* Папка *данных* (Blazor Server) &ndash; содержит класс `WeatherForecast` и реализацию `WeatherForecastService`, которые предоставляют примеры данных о погоде в компонент `FetchData` приложения.

* *wwwroot* &ndash; [корневую папку веб-узла](xref:fundamentals/index#web-root) для приложения, содержащего общие статические ресурсы приложения.

* параметры конфигурации для приложения &ndash; *appSettings. JSON* (Blazor Server).
