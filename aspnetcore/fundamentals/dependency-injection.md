---
title: "Внедрение зависимостей в ASP.NET Core"
author: ardalis
description: "Узнайте, как ASP.NET Core реализует внедрения зависимостей и способ его использования."
keywords: "ASP.NET Core внедрения зависимостей, di"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: fccd69be-7ad1-47fb-b203-b3633b6b9a9b
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/dependency-injection
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d2e191a7395110cde7ab5b2f19b6154c96fb496e
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="introduction-to-dependency-injection-in-aspnet-core"></a>Общие сведения о внедрение зависимостей в ASP.NET Core

<a name=fundamentals-dependency-injection></a>

По [Стив Смит](http://ardalis.com) и [Скотт Addie](https://scottaddie.com)

ASP.NET Core является разработана с нуля для поддержки и воспользоваться внедрения зависимостей. ASP.NET Core приложения могут использовать службы встроенная платформа, задав их внедрены в методы в классе при запуске, и службы приложения могут быть настроены для внедрения также. Контейнер служб по умолчанию, предоставляемый ASP.NET Core предоставляет минимальный набор и не предназначен для замены другие контейнеры.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/sample)

## <a name="what-is-dependency-injection"></a>Что такое внедрения зависимости

Внедрение зависимостей (DI) — это метод по достижению слабой связи между объектами и участники совместной работы или зависимости. Вместо того чтобы напрямую при создании экземпляра участники совместной работы, или с помощью ссылки на статические объектов, класс должен выполнять свои действия предназначены для класса каким-либо образом. Чаще всего классы объявите их зависимости, через своих конструкторов, позволяя выполните [явные зависимости принцип](http://deviq.com/explicit-dependencies-principle/). Такой подход известен как «конструктор инъекция».

Когда классы создаются с помощью DI помните, они более слабо связаны, так как они не имеют прямой, жестко зависимости на их участники совместной работы. Это соответствует [принципом инверсии зависимостей](http://deviq.com/dependency-inversion-principle/), указывающее, что *«высокий уровень модули не следует полагаться на низкий уровень модули; оба должны зависеть от абстракций».* Вместо ссылки на определенных реализаций, классы запроса абстрактные классы (обычно `interfaces`) предоставляются которого к ним при создании класса. Извлечение зависимостей в интерфейсах и обеспечивая реализацию этих интерфейсов в качестве параметров также является примером [шаблон разработки стратегии](http://deviq.com/strategy-design-pattern/).

При разработке системы для использования DI, множество классов, запрашивающего их зависимости через их конструктор (или свойства), бывает полезно иметь класс выделенного при создании этих классов с их связанные зависимости. Эти классы называются *контейнеры*, или, точнее говоря, [инверсии управления (IoC)](http://deviq.com/inversion-of-control/) контейнерами или контейнерами внедрения зависимости (DI). Контейнер — по существу фабрику, которая отвечает за предоставление экземпляры типов, которые запрашиваются из него. Если данный тип объявлен этот пакет имеет зависимости, что контейнер был настроен для предоставления типы зависимостей, он создаст зависимостей в ходе создания запрошенного экземпляра. Таким образом можно предоставить классы без необходимости создания любого объекта, жестко сложные графики зависимостей. Помимо создания объектов с зависимостями, контейнеры, обычно управлять временем жизни объектов в приложении.

ASP.NET Core включает встроенные простого контейнера (представленного `IServiceProvider` интерфейса), поддерживающем внедрение конструктора по умолчанию и ASP.NET можно использовать посредством DI определенных служб. ASP. Контейнер NET ссылается на типы, он управляет как *службы*. В остальной части этой статьи *служб* будет ссылаться на типы, которые управляются контейнер IoC ASP.NET Core. Необходимо настроить службы встроенного контейнера в `ConfigureServices` метод в вашем приложении `Startup` класса.

> [!NOTE]
> Martin Fowler записал широко статьи [инверсии контейнеры элементов управления и шаблон внедрения зависимостей](http://www.martinfowler.com/articles/injection.html). Шаблоны и методики Майкрософт имеет значительные описание [внедрения зависимостей](https://msdn.microsoft.com/library/dn178469(v=pandp.30).aspx).

> [!NOTE]
> В этой статье описывается внедрение зависимостей применительно ко всем приложениям ASP.NET. Внедрение зависимостей в MVC контроллеры рассматривается в [внедрения зависимостей и контроллеров](../mvc/controllers/dependency-injection.md).

### <a name="constructor-injection-behavior"></a>Внедрение поведение конструктора

Внедрение конструктора требует рассматриваемой конструктор *открытый*. В противном случае приложение создаст исключение `InvalidOperationException`:

> Не удалось найти подходящий конструктор для типа «YourType». Убедитесь, тип конкретизирован и службы, зарегистрированные для всех параметров открытого конструктора.


Внедрение конструктора необходимо, только один соответствующий конструктор. Перегрузки конструктора поддерживаются, но может существовать только одна перегрузка, аргументы которых можно все удовлетворять внедрения зависимостей. Если существует несколько экземпляров, приложение будет вызывать `InvalidOperationException`:

> В типе «YourType» найдено несколько конструкторов принимает все типы данного аргумента. Должен быть только один соответствующий конструктор.

Конструкторы могут принимать аргументы, которые не предоставляются внедрения зависимостей, но они должны поддерживать значения по умолчанию. Пример:

```csharp
// throws InvalidOperationException: Unable to resolve service for type 'System.String'...
public CharactersController(ICharacterRepository characterRepository, string title)
{
    _characterRepository = characterRepository;
    _title = title;
}

// runs without error
public CharactersController(ICharacterRepository characterRepository, string title = "Characters")
{
    _characterRepository = characterRepository;
    _title = title;
}
```

## <a name="using-framework-provided-services"></a>С помощью службы, предоставленные платформой

`ConfigureServices` Метод в `Startup` класс отвечает за определение службы будет использоваться приложением, включая возможности платформы Entity Framework Core и ASP.NET Core MVC. Изначально `IServiceCollection` для `ConfigureServices` имеет следующие определенные службы (в зависимости от [настройку узла](xref:fundamentals/hosting)):

| Тип службы | Время существования |
| ----- | ------- |
| [Microsoft.AspNetCore.Hosting.IHostingEnvironment](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) | Одноэлементный |
| [Microsoft.Extensions.Logging.ILoggerFactory](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.iloggerfactory) | Одноэлементный |
| [Microsoft.Extensions.Logging.ILogger&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) | Одноэлементный |
| [Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | Временной |
| [Microsoft.AspNetCore.Http.IHttpContextFactory](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.ihttpcontextfactory) | Временной |
| [Microsoft.Extensions.Options.IOptions&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) | Одноэлементный |
| [System.Diagnostics.DiagnosticSource](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticsource) | Одноэлементный |
| [System.Diagnostics.DiagnosticListener](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticlistener) | Одноэлементный |
| [Microsoft.AspNetCore.Hosting.IStartupFilter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.istartupfilter) | Временной |
| [Microsoft.Extensions.ObjectPool.ObjectPoolProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.objectpool.objectpoolprovider) | Одноэлементный |
| [Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.iconfigureoptions-1) | Временной |
| [Microsoft.AspNetCore.Hosting.Server.IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) | Одноэлементный |
| [Microsoft.AspNetCore.Hosting.IStartup](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.istartup) | Одноэлементный |
| [Microsoft.AspNetCore.Hosting.IApplicationLifetime](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | Одноэлементный |

Ниже приведен пример того, как добавить дополнительные службы в контейнер с помощью нескольких методов расширения, такие как `AddDbContext`, `AddIdentity`, и `AddMvc`.

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=5-6,8-10,12&range=39-56)]

Функции и возможности ASP.NET, например MVC, по промежуточного слоя Следуйте соглашению об использовании одного добавить*ServiceName* метод расширения для регистрации всех служб, необходимых для этой функции.

>[!TIP]
> Можно запросить определенных предоставленные платформой служб в `Startup` методы через их списки параметров - в разделе [запуска приложения](startup.md) для получения дополнительных сведений.

## <a name="registering-your-own-services"></a>Регистрация собственного служб

Можно зарегистрировать служб приложения следующим образом. Первый универсального типа представляет тип (обычно интерфейс), который будет запрашиваться из контейнера. Второго универсального типа представляет конкретный тип, который будет создавать экземпляры контейнером и использоваться для выполнения таких запросов.

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?range=53-54)]

> [!NOTE]
> Каждый `services.Add<ServiceName>` метод расширения добавляет (и потенциально настраивает) службы. Например `services.AddMvc()` добавляет MVC требует службы. Рекомендуется следовать соглашению, поместив методы расширения в `Microsoft.Extensions.DependencyInjection` пространства имен для инкапсуляции группы службы регистрации.

`AddTransient` Метод используется для сопоставления с конкретной службы, экземпляры которых создаются отдельно для каждого объекта, который требуется для абстрактных типов. Этот процесс известен как службы *время существования*, и время существования Дополнительные параметры описаны ниже. Очень важно для выбора соответствующего времени жизни для каждой из служб, которые можно зарегистрировать. Новый экземпляр службы должен быть предусмотрен для каждого класса, он запрашивает? Использовать один экземпляр на протяжении данного веб-запроса? Или следует использовать один экземпляр в течение времени существования приложения?

В этом образце для данной статьи является простой контроллер, который отображает имена символов, вызывается `CharactersController`. Его `Index` метод отображает текущий список символов, хранящихся в приложении и инициализирует коллекцию с небольшим количеством символов, если нет ни одного. Обратите внимание, что, несмотря на то, что в данном приложении используется Entity Framework Core и `ApplicationDbContext` класса для сохранения, ни один из, она становится очевидной, если в контроллере. Вместо этого было абстрагированы механизм доступа конкретные данные за интерфейс, `ICharacterRepository`, что соответствует [шаблон репозитория](http://deviq.com/repository-pattern/). Экземпляр `ICharacterRepository` запрашиваемой через конструктор, который назначен закрытое поле, которое затем используется для доступа к символам, при необходимости.

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Controllers/CharactersController.cs?highlight=3,5,6,7,8,14,21-27&range=8-36)]

`ICharacterRepository` Определяет два метода, который требуется работать с контроллером `Character` экземпляров.

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/ICharacterRepository.cs?highlight=8,9)]

Этот интерфейс реализуется в свою очередь конкретный тип `CharacterRepository`, который используется во время выполнения.

> [!NOTE]
> Способ DI используется с `CharacterRepository` класс является общей модели, можно выполнить для всех служб приложения, не только в «репозитории» или классов доступа к данным.

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Models/CharacterRepository.cs?highlight=9,11,12,13,14)]

Обратите внимание, что `CharacterRepository` запросов `ApplicationDbContext` в своем конструкторе. Довольно часто для внедрения зависимости для использования в цепочке манере наподобие этого, с каждой запрошенной зависимости, в свою очередь запрашивает собственные зависимости. Контейнер отвечает за разрешает все зависимости на графе и возвращения службы полностью устранены.

> [!NOTE]
> Создание запрошенный объект и все объекты, которые требуется и все объекты, их требуется, иногда называется *граф объекта*. Аналогичным образом, общий набор зависимостей, которые должны быть устранены, обычно называют *дерево зависимостей* или *граф зависимостей*.

В этом случае оба `ICharacterRepository` и в свою очередь `ApplicationDbContext` должен быть зарегистрирован в контейнере службы `ConfigureServices` в `Startup`. `ApplicationDbContext`настраивается при вызове метода расширения `AddDbContext<T>`. В следующем коде показано регистрацию `CharacterRepository` типа.

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Startup.cs?highlight=3-5,11&range=16-32)]

Контексты Entity Framework должны быть добавлены в контейнер службы с помощью `Scoped` времени существования. Это является занимается автоматически при использовании вспомогательных методов, как показано выше. Репозитории, которые позволят использовать платформы Entity Framework следует использовать то же время существования.

>[!WARNING]
> Устранение основных опасности необходимо использовать с осторожностью `Scoped` из Singleton-классом. Вполне вероятно, в этом случае, служба будет иметь неверное состояние при обработке последующих запросов.

Службы, имеющие зависимости следует зарегистрировать их в контейнере. Если конструктор служб требуется примитив, таких как `string`, это могут быть добавлены с помощью [параметры шаблона и конфигурации](configuration.md).

## <a name="service-lifetimes-and-registration-options"></a>Время существования службы и параметры регистрации

Службы ASP.NET могут быть настроены следующие параметры времени жизни:

**Временной**

Время существования временной службы создаются каждый раз, когда они запрашиваются. Это время существования лучше всего подходит для простой и без сохранения состояния службы.

**Областью действия**

Служб времени существования с областью создаются один раз для каждого запроса.

**Одноэлементный**

Службы времени жизни Singleton создаются при первом запросе (или когда `ConfigureServices` запускается при указании экземпляра существует) и затем каждый последующий запрос будет использовать тот же экземпляр. Если приложению требуется одноэлементное поведение, что контейнер службы для управления временем существования службы рекомендуется вместо реализации шаблона разработки одноэлементный и самостоятельного управления временем жизни объекта в классе.

Службы могут быть зарегистрированы с контейнером несколькими способами. Мы уже видели, как зарегистрировать реализацию службы заданного типа, указав конкретный тип для использования. Кроме того фабрику можно указать, который будет использоваться для создания экземпляра по требованию. Третий способ является непосредственно указать экземпляр типа для использования, в котором регистр контейнера никогда не будет пытаться создать экземпляр (и не удалит его экземпляра).

Для демонстрации различия между указанными вариантами временем существования и регистрации, рассмотрим простой интерфейс, представляющий одну или несколько задач, как *операции* с уникальным идентификатором `OperationId`. В зависимости от того, как мы настроить время жизни для этой службы контейнер будет предоставляют одинаковые или разные экземпляры службы для запроса класса. Чтобы было ясно, какие время существования запрашивается, мы создадим одного типа на параметр lifetime:

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/IOperation.cs?highlight=5-8)]

Мы реализуют эти интерфейсы, с помощью одного класса, `Operation`, которая принимает `Guid` в конструктор, или использует новый `Guid` Если не указан.

Затем в `ConfigureServices`, каждый тип будет добавлен в контейнере согласно существования именованных:

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Startup.cs?range=26-32)]

Обратите внимание, что `IOperationSingletonInstance` служба использует определенный экземпляр с известным Идентификатором `Guid.Empty` , он будет ясна, при использовании этого типа (его идентификатора Guid будет состоять из одних нулей). Мы также зарегистрировать `OperationService` , зависит от каждого из других `Operation` типов, чтобы сделать его снимите внутри запроса ли эта служба получает тот же экземпляр контроллера или создать новую, для каждого типа операции. Эта служба не всего предоставляют его зависимости в виде свойств, чтобы их можно было отобразить в представлении.

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Services/OperationService.cs)]

Чтобы продемонстрировать времени жизни объектов внутри и между отдельные отдельных запросов в приложение, пример включает `OperationsController` , запрашивает каждого вида `IOperation` типа, а также `OperationService`. `Index` Действие отображает все контроллера и службы `OperationId` значения.

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Controllers/OperationsController.cs)]

Теперь два отдельных запросов, выполненных для этого действия контроллера:

![Представление операций в Microsoft Edge со значениями операции идентификатор (GUID) для временной области, одноэлементные и экземпляр контроллера операций и операции службы при первом запросе веб-приложения пример внедрения зависимостей.](dependency-injection/_static/lifetimes_request1.png)

![Операции просмотра со значениями код операции для второго запроса.](dependency-injection/_static/lifetimes_request2.png)

Обратите внимание, что из `OperationId` значения изменяются внутри запроса, а также между запросами.

* *Временные* объекты всегда различаются; для каждого контроллера и каждая служба предоставляется новый экземпляр.

* *Областью действия* объекты одинаковы в запросе, но отличается для различных запросов

* *Одноэлементный* объекты одинаковы для всех объектов и каждого запроса (независимо от того, предоставляется ли экземпляр в `ConfigureServices`)

## <a name="request-services"></a>Запрос служб

Запрашивать службы, доступные в ASP.NET `HttpContext` , предоставляются через `RequestServices` коллекции.

![HttpContext запрос службы Intellisense контекстные диалогового окна о том, что службы запрос возвращает или задает IServiceProvider, предоставляющий доступ к контейнеру запроса службы.](dependency-injection/_static/request-services.png)

Запрос службы представляют собой службы настройки и запросов как часть приложения. При объектов указать зависимости, они выполняются на типы, которые содержатся в `RequestServices`, а не `ApplicationServices`.

Как правило не следует использовать эти свойства напрямую, предпочитая для запроса типов, требуемую через ваш класс конструктора классов, а окно платформа внедрить эти зависимости. Это создает классы, которые более удобны для тестирования (см. [тестирования](../testing/index.md)) и более слабо связаны.

> [!NOTE]
> Предпочтение запросы зависимости как параметры конструктора для доступа к `RequestServices` коллекции.

## <a name="designing-your-services-for-dependency-injection"></a>Разработка для внедрения зависимости служб

Следует разрабатывать с помощью внедрения зависимости получить их участники совместной работы служб. Это означает, что не использовать вызовы статического метода, с отслеживанием состояния (что привести Запах код, известный как [статических напечатанными](http://deviq.com/static-cling/)) и прямой создание экземпляров зависимых классов внутри служб. Может помочь запомнить фразу [новые является связующего](http://ardalis.com/new-is-glue), при выборе, следует ли создать экземпляр типа или отправить запрос через внедрения зависимостей. Следуя [СПЛОШНОЙ принципы из объектно-ориентированном проектировании](http://deviq.com/solid/), ваши классы естественным образом стремятся маленькое, хорошо организованную и легко протестированных.

Что делать, если можно найти, что классы, обычно склонны к способом слишком много зависимостей, введенного? Это обычно является признаком класс пытается делать слишком много что, вероятно, нарушил SRP - [персональной ответственности](http://deviq.com/single-responsibility-principle/). См. Если класс рефакторинга, переместив некоторые из его функции в новый класс. Имейте в виду, что ваш `Controller` классы следует уделить особое внимание проблемы пользовательского интерфейса, поэтому бизнеса правил и данных access детали реализации должны храниться в классах, соответствующие этим [разделения проблем](http://deviq.com/separation-of-concerns/).

В отношении доступа к данным в частности, можно ввести `DbContext` в контроллерах (при условии, что вы добавили EF в контейнере службы `ConfigureServices`). Некоторые разработчики предпочитают использовать интерфейс репозитория в базу данных вместо вводится `DbContext` напрямую. С помощью интерфейса, предназначенный для инкапсуляции данных логики доступа в одном месте можно свести к минимуму число знаков, необходимо будет изменить при изменении базы данных.

### <a name="disposing-of-services"></a>Удаление служб

Контейнер будет вызвать `Dispose` для `IDisposable` типов, он создает. Тем не менее при добавлении экземпляра к контейнеру самостоятельно, она не будет удален.

Пример.

```csharp
// Services implement IDisposable:
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}
public class Service3 : IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    // container will create the instance(s) of these types and will dispose them
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();

    // container did not create instance so it will NOT dispose it
    services.AddSingleton<Service3>(new Service3());
    services.AddSingleton(new Service3());
}
```

> [!NOTE]
> В версии 1.0, контейнер вызван dispose для *все* `IDisposable` объекты, включая те, не была создана.

## <a name="replacing-the-default-services-container"></a>Замена контейнер служб по умолчанию

Контейнер встроенных служб предназначен для обслуживания basic framework и большинство клиентские приложения, основанные на нем. Тем не менее разработчики могут заменять встроенного контейнера их предпочитаемыми контейнерами. `ConfigureServices` Метод обычно возвращает `void`, но при изменении его подпись для возврата `IServiceProvider`, другой контейнер можно настроить и возвращается. Многие контейнеры IOC доступны для .NET. В этом примере [Autofac](http://autofac.org/) используется пакет.

Сначала установите пакеты соответствующего контейнера:

* `Autofac`
* `Autofac.Extensions.DependencyInjection`

Настройте контейнер в `ConfigureServices` и возвращают `IServiceProvider`:

```csharp
public IServiceProvider ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    // Add other framework services

    // Add Autofac
    var containerBuilder = new ContainerBuilder();
    containerBuilder.RegisterModule<DefaultModule>();
    containerBuilder.Populate(services);
    var container = containerBuilder.Build();
    return new AutofacServiceProvider(container);
}
```

> [!NOTE]
> При использовании сторонних DI контейнер, необходимо изменить `ConfigureServices` , чтобы он возвращал `IServiceProvider` вместо `void`.

И наконец, настройте Autofac как обычно в `DefaultModule`:

```csharp
public class DefaultModule : Module
{
    protected override void Load(ContainerBuilder builder)
    {
        builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
    }
}
```

Во время выполнения Autofac будет использоваться для разрешения типов и внедрения зависимостей. [Дополнительные сведения об использовании Autofac и ASP.NET Core](http://docs.autofac.org/en/latest/integration/aspnetcore.html).

### <a name="thread-safety"></a>Потокобезопасность

Одноэлементные службы должны быть потокобезопасными. Если служба singleton имеет зависимость от временных службы, временная служба может также потребоваться потокобезопасность, в зависимости от того, как он используется единственный экземпляр.

## <a name="recommendations"></a>Рекомендации

При работе с внедрения зависимостей, учитывайте следующие рекомендации:

* DI — для объектов, имеющих сложных зависимостей. Контроллеры, служб, адаптеры и репозиториев иллюстрируют все объекты, которые могут быть добавлены DI.

* Не храните данные и конфигурации непосредственно в DI. Например корзину пользователя обычно не следует добавлять в контейнер службы. Следует использовать конфигурации [параметры модели](configuration.md#options-config-objects). Аналогичным образом можно Избегайте «данных владельца» объекты, которые существуют только для доступа к какой-либо другой объект. Лучше запроса фактический элемент понадобится DI, если это возможно.

* Избегайте доступа к службам.

* Избегайте размещение службы в коде приложения.

* Избегайте доступа к `HttpContext`.

> [!NOTE]
> Как и все наборы рекомендаций могут возникнуть ситуации, в которых требуется одно игнорируется. Мы пришли к выводу исключения редко — главным образом специальные случаи, входящие в этой платформе.

Помните, что является внедрения зависимостей *альтернативные* для шаблонов доступа к статическим или глобального объекта. Вы не сможете использовать преимущества DI Если перепутать с доступом статический объект.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Запуск приложения](startup.md)

* [Тестирование](../testing/index.md)

* [Написание кода очистки в ASP.NET Core с помощью внедрения зависимости (MSDN)](https://msdn.microsoft.com/magazine/mt703433.aspx)

* [Управляемые контейнера при разработке приложения Prelude: Когда осуществляет контейнер принадлежать?](http://blogs.msdn.com/b/nblumhardt/archive/2008/12/27/container-managed-application-design-prelude-where-does-the-container-belong.aspx)

* [Принцип явные зависимости](http://deviq.com/explicit-dependencies-principle/)

* [Контейнеры элементов управления и шаблон внедрения зависимостей инверсии](http://www.martinfowler.com/articles/injection.html) (Fowler)
