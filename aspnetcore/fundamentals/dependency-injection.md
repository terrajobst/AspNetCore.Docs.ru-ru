---
title: Внедрение зависимостей в ASP.NET Core
author: ardalis
description: Сведения о том, как ASP.NET Core реализует внедрение зависимостей и как его использовать.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/dependency-injection
ms.openlocfilehash: 8a105f835dddfcd0e9f32059e644f60dc1fdbbe1
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="dependency-injection-in-aspnet-core"></a>Внедрение зависимостей в ASP.NET Core

<a name="fundamentals-dependency-injection"></a>

Авторы: [Стив Смит](https://ardalis.com/) (Steve Smith) и [Скотт Эдди](https://scottaddie.com) (Scott Addie)

Среда ASP.NET Core разработана с нуля, чтобы обеспечить поддержку и использование внедрения зависимостей. Приложения ASP.NET Core могут использовать встроенные службы платформы, внедряя их в методы в классе Startup, кроме того, для внедрения можно настроить и службы приложений. Контейнер служб по умолчанию, предоставляемый ASP.NET Core, предлагает минимальный набор возможностей и не предназначен для замены других контейнеров.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-dependency-injection"></a>Что такое внедрение зависимостей?

Внедрение зависимостей (DI) — это методика для обеспечения слабой связи между объектами и их участниками совместной работы или зависимостями. Вместо того, чтобы напрямую создавать экземпляры участников совместной работы или использовать статические ссылки, объекты, необходимые классу для выполнения действий, предоставляются классу каким-либо образом. Чаще всего классы объявляют зависимости через свой конструктор, позволяя им следовать [принципу явных зависимостей](http://deviq.com/explicit-dependencies-principle/). Подобный подход называется "внедрением через конструктор".

Когда классы создаются с учетом внедрения зависимостей, они слабее связаны, так как не имеют прямых и жестко заданных зависимостей от своих участников совместной работы. Это соответствует [принципу инверсии зависимостей](http://deviq.com/dependency-inversion-principle/), указывающему, что *"высокоуровневые модули не должны зависеть от низкоуровневых; оба этих типа должны зависеть от абстракций"*. Вместо ссылки на определенные реализации классы запрашивают абстракции (обычно `interfaces`), которые предоставляются им при создании класса. Извлечение зависимостей в интерфейсы и предоставление реализаций этих интерфейсов в качестве параметров также является примером [шаблона разработки стратегии](http://deviq.com/strategy-design-pattern/).

Когда система предназначена для использования внедрения зависимостей, то есть множество классов запрашивают зависимости через свой конструктор (или свойства), полезно иметь выделенный класс для создания этих классов с их связанными зависимостями. Такие классы называются *контейнерами*, а если точнее, контейнерами [инверсии управления (IoC)](http://deviq.com/inversion-of-control/) или контейнерами внедрения зависимостей (DI). Контейнер по существу является фабрикой и отвечает за предоставление экземпляров типов, запрашиваемых из него. Если заданный тип объявил, что имеет зависимости, а контейнер настроен для предоставления этих типов зависимостей, он создаст зависимости в рамках создания запрошенного экземпляра. Таким образом, можно предоставить классам сложные графы зависимостей без потребности в создании жестко заданных объектов. Кроме создания объектов с зависимостями, контейнеры обычно управляют временем жизни объектов в приложении.

ASP.NET Core содержит простой встроенный контейнер (представленный интерфейсом `IServiceProvider`), поддерживающий внедрение через конструктор по умолчанию, а ASP.NET предоставляет посредством внедрения зависимостей определенные службы. Контейнер ASP.NET ссылается на управляемые им типы, как на *службы*. В остальной части этой статьи *службами* будут обозначаться типы, которыми управляет контейнер инверсии управления ASP.NET Core. Настроить службы встроенного контейнера можно в методе `ConfigureServices` в классе `Startup` вашего приложения.

> [!NOTE]
> Мартин Фаулер (Martin Fowler) написал обширную статью о [контейнерах инверсии управления и шаблоне внедрения зависимостей](https://www.martinfowler.com/articles/injection.html). Центр Microsoft Patterns and Practices также содержит хорошее описание [внедрения зависимостей](https://msdn.microsoft.com/library/hh323705.aspx).

> [!NOTE]
> Эта статья охватывает внедрение зависимостей применительно ко всем приложениям ASP.NET. Внедрение зависимостей в контроллерах MVC рассматривается в статье [Внедрение зависимостей и контроллеры](../mvc/controllers/dependency-injection.md).

### <a name="constructor-injection-behavior"></a>Поведение внедрения через конструктор

Для внедрения через конструктор требуется, чтобы соответствующий конструктор был *открытым*. В противном случае приложение выдает исключение `InvalidOperationException`:

> A suitable constructor for type "YourType" couldn't be located. Ensure the type is concrete and services are registered for all parameters of a public constructor. (Не удалось найти подходящий конструктор для типа "ваш_тип". Убедитесь, что тип является конкретным, а службы зарегистрированы для всех параметров открытого конструктора.)

Для внедрения через конструктор необходимо, чтобы существовал всего один соответствующий конструктор. Перегрузки конструктора поддерживаются, но может существовать всего одна перегрузка, все аргументы которой могут быть обработаны с помощью внедрения зависимостей. При наличии нескольких экземпляров приложение выдает исключение `InvalidOperationException`:

> Multiple constructors accepting all given argument types have been found in type "YourType". There should only be one applicable constructor. (В типе "YourType" найдено несколько конструкторов, принимающих все заданные типы аргументов. Должен присутствовать всего один соответствующий конструктор.)

Конструкторы могут принимать аргументы, предоставленные не внедрением зависимостей, но они должны поддерживать значения по умолчанию. Пример:

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

## <a name="using-framework-provided-services"></a>Использование служб, предоставленных платформой

Метод `ConfigureServices` в классе `Startup` отвечает за определение служб, которые будет использовать приложение, включая такие возможности платформы, как Entity Framework Core и ASP.NET Core MVC. Изначально `IServiceCollection`, предоставленный `ConfigureServices`, имеет следующие определенные службы (в зависимости от [настройки узла](xref:fundamentals/hosting)):

| Тип службы | Время существования |
| ----- | ------- |
| [Microsoft.AspNetCore.Hosting.IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) | Одноэлементный |
| [Microsoft.Extensions.Logging.ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory) | Одноэлементный |
| [Microsoft.Extensions.Logging.ILogger&lt;T&gt;](/dotnet/api/microsoft.extensions.logging.ilogger) | Одноэлементный |
| [Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory](/dotnet/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | Временный |
| [Microsoft.AspNetCore.Http.IHttpContextFactory](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextfactory) | Временный |
| [Microsoft.Extensions.Options.IOptions&lt;T&gt;](/dotnet/api/microsoft.extensions.options.ioptions-1) | Одноэлементный |
| [System.Diagnostics.DiagnosticSource](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticsource) | Одноэлементный |
| [System.Diagnostics.DiagnosticListener](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticlistener) | Одноэлементный |
| [Microsoft.AspNetCore.Hosting.IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) | Временный |
| [Microsoft.Extensions.ObjectPool.ObjectPoolProvider](/dotnet/api/microsoft.extensions.objectpool.objectpoolprovider) | Одноэлементный |
| [Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) | Временный |
| [Microsoft.AspNetCore.Hosting.Server.IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) | Одноэлементный |
| [Microsoft.AspNetCore.Hosting.IStartup](/dotnet/api/microsoft.aspnetcore.hosting.istartup) | Одноэлементный |
| [Microsoft.AspNetCore.Hosting.IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | Одноэлементный |

Ниже приведен пример того, как добавить дополнительные службы в контейнер с помощью нескольких методов расширения, таких как `AddDbContext`, `AddIdentity` и `AddMvc`.

[!code-csharp[](../common/samples/WebApplication1/Startup.cs?highlight=5-6,8-10,12&range=39-56)]

Предоставляемые ASP.NET функции и ПО промежуточного слоя, например MVC, соответствуют соглашению об использовании одного метода расширения Add*имя_службы* для регистрации всех служб, необходимых данному компоненту.

> [!TIP]
> Вы можете запросить определенные предоставляемые платформой службы в методах `Startup` через их списки параметров. Дополнительные сведения см. в разделе [Запуск приложения](startup.md).

## <a name="registering-services"></a>Регистрация служб

Вы можете регистрировать свои службы приложений описанным ниже образом. Первый универсальный тип представляет тип (обычно это интерфейс), который будет запрашиваться из контейнера. Второй универсальный тип представляет конкретный тип, экземпляр которого будет создаваться контейнером и использоваться для выполнения таких запросов.

[!code-csharp[](../common/samples/WebApplication1/Startup.cs?range=53-54)]

> [!NOTE]
> Каждый метод расширения `services.Add<ServiceName>` добавляет (а потенциально и настраивает) службы. Например, `services.AddMvc()` добавляет службы, нужные MVC. Рекомендуется следовать этому соглашению и поместить методы расширения в пространство имен `Microsoft.Extensions.DependencyInjection` для инкапсуляции группы регистраций служб.

Метод `AddTransient` используется для сопоставления абстрактных типов с конкретными службами, экземпляры которых создаются отдельно для каждого объекта, который в них нуждается. Это называется *временем существования* службы, а соответствующие дополнительные параметры описаны ниже. Важно выбирать подходящее время существования для каждой из регистрируемых служб. Нужно ли предоставлять новый экземпляр службы каждому классу, который его запрашивает? Нужно ли использовать один экземпляр в рамках всего заданного веб-запроса? Или нужно использовать один экземпляр в течение времени существования приложения?

Пример для этой статьи содержит простой контроллер, который отображает имена символов и называется `CharactersController`. Его метод `Index` отображает текущий список символов, хранящихся в приложении, и инициализирует коллекцию с небольшим количеством символов, если такой список отсутствует. Обратите внимание, что хотя это приложение использует Entity Framework Core и класс `ApplicationDbContext` для обеспечения сохраняемости, в контроллере об этом неизвестно. Вместо этого определенный механизм доступа к данным был абстрагирован за интерфейс `ICharacterRepository`, что соответствует [шаблону репозитория](http://deviq.com/repository-pattern/). Экземпляр `ICharacterRepository` запрашивается через конструктор и назначается частному полю, которое затем используется для доступа к символам по мере необходимости.

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Controllers/CharactersController.cs?highlight=3,5,6,7,8,14,21-27&range=8-36)]

`ICharacterRepository` определяет два метода, которые нужны контроллеру для работы с экземплярами `Character`.

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/ICharacterRepository.cs?highlight=8,9)]

Этот интерфейс, в свою очередь, реализуется конкретным типом `CharacterRepository`, используемым во время выполнения.

> [!NOTE]
> Подход к использованию внедрения зависимостей с классом `CharacterRepository` является общей моделью, которую можно применять для всех служб приложений, а не только в репозиториях или классах доступа к данным.

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Models/CharacterRepository.cs?highlight=9,11,12,13,14)]

Обратите внимание, что `CharacterRepository` запрашивает `ApplicationDbContext` в своем конструкторе. Внедрение зависимостей нередко используется по цепочке, когда каждая запрошенная зависимость запрашивает собственные зависимости. Этот контейнер отвечает за разрешение всех зависимостей на графе и возврат полностью разрешенной службы.

> [!NOTE]
> Создание запрошенного объекта, всех необходимых ему объектов и всех объектов, необходимых этим объектам, иногда называют *графом объектов*. Аналогичным образом, общий набор зависимостей, которые нужно разрешить, обычно называют *деревом зависимостей* или *графом зависимостей*.

В этом случае как `ICharacterRepository`, так и `ApplicationDbContext` нужно зарегистрировать в контейнере служб в `ConfigureServices` в `Startup`. `ApplicationDbContext` настроен для вызова метода расширения `AddDbContext<T>`. Приведенный ниже пример кода демонстрирует регистрацию типа `CharacterRepository` .

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Startup.cs?highlight=3-5,11&range=16-32)]

Контексты Entity Framework нужно добавлять в контейнер служб с использованием времени существования `Scoped`. Это выполняется автоматически, если вы используете вспомогательные методы, как показано выше. Репозитории, которые будут работать с Entity Framework, должны использовать то же время существования.

> [!WARNING]
> Главной опасностью, о которой следует помнить, является разрешение службы `Scoped` из singleton-класса. В этом случае вполне вероятно, что служба будет иметь неверное состояние при обработке последующих запросов.

Службы, имеющие зависимости, должны зарегистрировать их в контейнере. Если конструктору службы требуется примитив, например `string`, его можно внедрить с помощью [конфигурации](xref:fundamentals/configuration/index) и [шаблона параметров](xref:fundamentals/configuration/options).

## <a name="service-lifetimes-and-registration-options"></a>Время существования службы и параметры регистрации

Для служб ASP.NET можно настроить следующие параметры времени существования:

**Временный**

Временные службы времени существования создаются при каждом их запросе. Это время существования лучше всего подходит для простых служб без отслеживания состояния.

**Ограниченный областью**

Службы времени существования с заданной областью создаются один раз для каждого запроса.

> [!WARNING]
> При использовании службы с заданной областью в ПО промежуточного слоя внедрите службу в метод `Invoke` или `InvokeAsync`. Не внедряйте службу через внедрение конструктора, поскольку в таком случае служба будет вести себя как одноэлементный объект.

**Одноэлементные**

Одноэлементные службы времени существования создаются при первом их запросе (или при запуске `ConfigureServices`, если вы указываете экземпляр там), после чего каждый последующий запрос использует тот же самый экземпляр. Если приложению требуется одноэлементное поведение, рекомендуется разрешить контейнеру служб управлять временем существования службы вместо того, чтобы реализовывать одноэлементный шаблон разработки и самостоятельно управлять временем существования объекта в классе.

Службы можно регистрировать в контейнере несколькими способами. Мы уже рассмотрели, как зарегистрировать реализацию службы заданного типа, указав используемый конкретный тип. Кроме того, можно задать фабрику, которая затем будет использоваться для создания экземпляра по запросу. Третий способ заключается в том, чтобы напрямую указать используемый экземпляр типа. В этом случае контейнер никогда не будет пытаться создать экземпляр (и не будет удалять его).

Чтобы продемонстрировать различия между указанными вариантами времени существования и регистрации, рассмотрим простой интерфейс, представляющий одну или несколько задач в виде *операции* с уникальным идентификатором `OperationId`. В зависимости от того, как настроено время существования для этой службы, контейнер предоставляет запрашивающему классу одинаковые или разные экземпляры службы. Чтобы уточнить, какое время существования запрашивается, мы создадим по одному типу на каждый вариант времени существования:

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/IOperation.cs?highlight=5-8)]

Мы реализуем эти интерфейсы с помощью одного класса — `Operation`, который принимает `Guid` в своем конструкторе или использует новый `Guid`, если он не указан.

Затем в `ConfigureServices` каждый тип добавляется в контейнер согласно его времени существования:

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Startup.cs?range=26-32)]

Обратите внимание, что служба `IOperationSingletonInstance` использует определенный экземпляр с известным идентификатором `Guid.Empty`, что позволяет четко определить, когда этот тип используется (его GUID будет состоять из одних нулей). Мы также зарегистрировали `OperationService`, зависящий от каждого из других типов `Operation`, чтобы внутри запроса для каждого типа операции было очевидно, получает ли служба тот же или новый экземпляр контроллера. Эта служба лишь предоставляет свои зависимости в виде свойств, чтобы их можно было отобразить в представлении.

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Services/OperationService.cs)]

Чтобы продемонстрировать время существования объектов внутри отдельных запросов, направляемых в приложение, и между ними, пример включает `OperationsController`, который запрашивает каждый из типов `IOperation`, а также `OperationService`. Действие `Index` отображает все значения `OperationId` контроллера и службы.

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Controllers/OperationsController.cs)]

Теперь для этого действия контроллера выполняются два отдельных запроса:

![Представление операций для веб-приложения примера внедрения зависимостей, запущенного в Microsoft Edge, где отображаются значения идентификаторов (GUID) для временных, ограниченных по области, одноэлементных и относящихся к экземпляру операций контроллера и службы операций при первом запросе.](dependency-injection/_static/lifetimes_request1.png)

![Представление операций, показывающее идентификаторы операций для второго запроса.](dependency-injection/_static/lifetimes_request2.png)

Обратите внимание, какие значения `OperationId` изменяются внутри запроса, а также между запросами.

* *Временные* объекты всегда различаются — для каждого контроллера и каждой службы предоставляется новый экземпляр.

* *Ограниченные по области* объекты одинаковы в рамках запроса, но различаются для разных запросов.

* *Одноэлементные* объекты одинаковы для всех объектов и запросов (независимо от того, предоставляется ли экземпляр в `ConfigureServices`).

## <a name="resolve-a-scoped-service-within-the-application-scope"></a>Разрешение службы с заданной областью в области приложения

Создайте [IServiceScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope) с [IServiceScopeFactory.CreateScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope) для разрешения службы с заданной областью в области приложения. Этот способ позволит получить доступ к службе с заданной областью при запуске для выполнения задач по инициализации. В следующем примере показано, как получить контекст для `MyScopedService` в `Program.Main`:

```csharp
public static void Main(string[] args)
{
    var host = BuildWebHost(args);

    using (var serviceScope = host.Services.CreateScope())
    {
        var services = serviceScope.ServiceProvider;

        try
        {
            var serviceContext = services.GetRequiredService<MyScopedService>();
            // Use the context here
        }
        catch (Exception ex)
        {
            var logger = services.GetRequiredService<ILogger<Program>>();
            logger.LogError(ex, "An error occurred.");
        }
    }

    host.Run();
}
```

## <a name="scope-validation"></a>Проверка области

Когда приложение выполняется в среде разработки в ASP.NET Core 2.0 или более поздней версии, поставщик службы по умолчанию проверяет, что:

* Службы с заданной областью не разрешаются из корневого поставщика службы, прямо или косвенно.
* Службы с заданной областью не вводятся в одноэлементные объекты, прямо или косвенно.

Корневой поставщик службы создается при вызове [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider). Время существования корневого поставщика службы соответствует времени существования приложения или сервера — поставщик запускается с приложением и удаляется, когда приложение завершает работу.

Службы с заданной областью удаляются создавшим их контейнером. Если служба с заданной областью создается в корневом контейнере, время существования службы повышается до уровня одноэлементного объекта, поскольку она удаляется только корневым контейнером при завершении работы приложения или сервера. Проверка областей службы перехватывает эти ситуации при вызове `BuildServiceProvider`.

Дополнительные сведения см. в разделе ["Проверка области" в статье "Размещение"](xref:fundamentals/hosting#scope-validation).

## <a name="request-services"></a>Службы запросов

Службы, доступные в запросе ASP.NET из `HttpContext`, предоставляются посредством коллекции `RequestServices`.

![Контекстное диалоговое окно Intellisense для службы запросов HttpContext, сообщающее, что служба запросов возвращает или задает IServiceProvider, предоставляющий доступ к контейнеру службы для запроса.](dependency-injection/_static/request-services.png)

Службы запросов представляют службы, которые вы настраиваете и запрашиваете в рамках своего приложения. Когда ваши объекты задают зависимости, они удовлетворяются за счет типов из `RequestServices`, а не `ApplicationServices`.

Как правило, не следует использовать эти свойства напрямую, лучше запросить нужные вашим классам типы через конструктор класса и позволить платформе внедрить эти зависимости. Полученные таким образом классы более удобны для тестирования (см. раздел [Тестирование и отладка](../testing/index.md)) и слабее связаны.

> [!NOTE]
> Предпочтительнее запрашивать зависимости в качестве параметров конструктора, а не обращаться к коллекции `RequestServices`.

## <a name="designing-services-for-dependency-injection"></a>Разработка служб для внедрения зависимостей

Службы следует разрабатывать таким образом, чтобы они использовали внедрение зависимостей для получения своих участников совместной работы. Это означает отказ от использования вызовов статических методов с отслеживанием состояния (что приводит к проблемам с кодом, называемым [статическим слипанием](http://deviq.com/static-cling/)) и прямого создания экземпляров зависимых классов внутри служб. Принимая решение о том, следует ли создать экземпляр типа или запросить его через внедрение зависимостей, рекомендуется руководствоваться принципами, изложенными в статье о [связующем слове new](https://ardalis.com/new-is-glue). Соблюдение [принципов SOLID для объектно-ориентированного программирования](http://deviq.com/solid/) поможет сделать ваши классы небольшими, хорошо организованными и удобными в тестировании.

Что делать, если классы склонны использовать слишком много внедряемых зависимостей? Обычно это указывает на то, что класс пытается сделать слишком многое и, вероятно, нарушает [принцип единственной обязанности](http://deviq.com/single-responsibility-principle/). Определите, можно ли выполнить рефакторинг класса, переместив некоторые из его обязанностей в новый класс. Помните, что ваши классы `Controller` должны быть посвящены аспектам пользовательского интерфейса, а за реализацию бизнес-правил и доступа к данным должны отвечать [отдельные](http://deviq.com/separation-of-concerns/) классы.

Например, для доступа к данным можно внедрить `DbContext` в свои контроллеры (при условии, что вы добавили Entity Framework в контейнер служб в `ConfigureServices`). Некоторые разработчики предпочитают использовать интерфейс репозитория для базы данных, вместо того, чтобы внедрять `DbContext` напрямую. Использование интерфейса для инкапсуляции логики доступа к данным в одном месте позволяет минимизировать число изменений, необходимых при изменении базы данных.

### <a name="disposing-of-services"></a>Удаление служб

Контейнер будет вызывать `Dispose` для создаваемых им типов `IDisposable`. Но если вы самостоятельно добавите экземпляр в контейнер, он не будет удален.

Пример

```csharp
// Services implement IDisposable:
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}
public class Service3 : IDisposable {}

public interface ISomeService {}
public class SomeServiceImplementation : ISomeService, IDisposable {}


public void ConfigureServices(IServiceCollection services)
{
    // container will create the instance(s) of these types and will dispose them
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();
    services.AddSingleton<ISomeService>(sp => new SomeServiceImplementation());

    // container didn't create instance so it will NOT dispose it
    services.AddSingleton<Service3>(new Service3());
    services.AddSingleton(new Service3());
}
```

> [!NOTE]
> В версии 1.0 контейнер вызывал удаление для *всех* объектов `IDisposable`, включая те, которые он не создавал.

## <a name="replacing-the-default-services-container"></a>Замена контейнера служб по умолчанию

Встроенный контейнер служб предназначен для удовлетворения базовых потребностей платформы и большинства основанных на ней клиентских приложений. Однако разработчики могут заменить его на предпочитаемый ими контейнер. Метод `ConfigureServices` обычно возвращает `void`, но если его сигнатура изменена для возврата `IServiceProvider`, можно настроить и возвращать другой контейнер. Для платформы .NET доступно множество контейнеров инверсии управления. В этом примере используется пакет [Autofac](https://autofac.org/).

Сначала установите пакеты соответствующего контейнера:

* `Autofac`
* `Autofac.Extensions.DependencyInjection`

Затем настройте контейнер в `ConfigureServices` и возвратите `IServiceProvider`:

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
> При использовании стороннего контейнера внедрения зависимостей нужно изменить `ConfigureServices`, чтобы он возвращал `IServiceProvider` вместо `void`.

Наконец, настройте Autofac в `DefaultModule` обычным образом:

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

Одноэлементные службы должны быть потокобезопасными. Если одноэлементная служба имеет зависимость от временной службы, с учетом характера использования одноэлементной службой к этой временной службе также может предъявляться требование потокобезопасности.

## <a name="recommendations"></a>Рекомендации

При работе с внедрением зависимостей соблюдайте следующие рекомендации:

* Внедрение зависимостей предназначено для объектов, имеющих сложные зависимости. Контроллеры, службы, адаптеры и репозитории являются примерами объектов, которые можно добавить в функцию внедрения зависимостей.

* Не храните данные и конфигурации непосредственно в функции внедрения зависимостей. Например, в общем случае не следует добавлять корзину пользователя в контейнер служб. Конфигурация должна использовать [шаблон параметров](xref:fundamentals/configuration/options). Аналогичным образом, избегайте объектов "хранения данных", которые служат лишь для доступа к некоторому другому объекту. По возможности лучше запросить фактический необходимый элемент через внедрение зависимостей.

* Избегайте статического доступа к службам.

* Избегайте обнаружения службы в коде приложения.

* Избегайте статического доступа к `HttpContext`.

> [!NOTE]
> Как в случае с любыми наборами рекомендаций, могут возникнуть ситуации, когда нужно отступить от одного из правил. Мы пришли к выводу, что подобные исключения довольно редки и нужны в особых случаях, главным образом внутри самой платформы.

Помните, что внедрение зависимостей является *альтернативой* для шаблонов доступа к статическим или глобальным объектам. Вы не сможете воспользоваться преимуществами внедрения зависимостей, если будете сочетать его с доступом к статическим объектам.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Запуск приложения](xref:fundamentals/startup)
* [Тестирование и отладка](xref:testing/index)
* [Активация фабричного ПО промежуточного слоя](xref:fundamentals/middleware/extensibility)
* [Написание чистого кода в ASP.NET Core с внедрением зависимостей (MSDN)](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [Проектирование приложения на основе контейнеров. Вступление. К чему относится контейнер?](https://blogs.msdn.microsoft.com/nblumhardt/2008/12/26/container-managed-application-design-prelude-where-does-the-container-belong/)
* [Принцип явных зависимостей](http://deviq.com/explicit-dependencies-principle/)
* [Контейнеры инверсии управления и шаблон внедрения зависимостей](https://www.martinfowler.com/articles/injection.html) (Фаулер)
