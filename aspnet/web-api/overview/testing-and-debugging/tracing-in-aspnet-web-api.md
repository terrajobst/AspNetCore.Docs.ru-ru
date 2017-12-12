---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: "Трассировка в ASP.NET Web API 2 | Документы Microsoft"
author: MikeWasson
description: "Показано, как включить трассировку в веб-API ASP.NET."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 66a837e9-600b-4b72-97a9-19804231c64a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: f35c8a10018ce796e2d905d6ee839ff09bb380a1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="tracing-in-aspnet-web-api-2"></a>Трассировка в ASP.NET Web API 2
====================
по [Mike Wasson](https://github.com/MikeWasson)

> Когда выполняется попытка отладки веб-приложений, нет альтернативы для хороший набор журналов трассировки. Этого учебника показано, как включить трассировку в веб-API ASP.NET. Эту функцию можно использовать для трассировки, платформа веб-API, делает до и после он вызывает контроллер. Его также можно использовать для трассировки кода.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемая в этом учебнике
> 
> 
> - [Visual Studio 2017 г](https://www.visualstudio.com/downloads/) (также работает с Visual Studio 2015)
> - Веб-API 2
> - [Microsoft.AspNet.WebApi.Tracing](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)


## <a name="enable-systemdiagnostics-tracing-in-web-api"></a>Включить трассировку в веб-API System.Diagnostics

Во-первых мы создадим новый проект веб-приложения ASP.NET. В Visual Studio из **файл** последовательно выберите пункты **New**, затем **проекта**. В разделе **шаблоны**, **Web**выберите **веб-приложение ASP.NET**.

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

Выберите шаблон проекта веб-API.

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

Из **средства** последовательно выберите пункты **диспетчер пакетов библиотеки**, затем **консоли управления пакета**.

В окне консоли диспетчера пакетов введите следующие команды.

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

Первая команда устанавливает последнюю версию пакета трассировки веб-API. Он также обновляет основные пакеты веб-API. Вторая команда обновляет пакет WebApi.WebHost до последней версии.

> [!NOTE]
> Если требуется для конкретной версии веб-API, используйте флаг версии при установке пакета трассировки.


Откройте файл WebApiConfig.cs в приложении\_Начальная папка. Добавьте следующий код в **зарегистрировать** метод.

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

Этот код добавляет [пространство имен SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/en-us/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) класса конвейера веб-API. **Пространство имен SystemDiagnosticsTraceWriter** класс записывает трассировки, чтобы [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace).

Для просмотра трассировки, запустите приложение в отладчике. В браузере перейдите к `/api/values`.

![](tracing-in-aspnet-web-api/_static/image5.png)

Операторы трассировки записываются в окно вывода в Visual Studio. (От **представление** последовательно выберите пункты **вывода**).

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

Поскольку **пространство имен SystemDiagnosticsTraceWriter** записывает трассировки, чтобы **System.Diagnostics.Trace**, вы можете зарегистрировать дополнительные прослушиватели трассировки, например, для записи трассировки в файл журнала. Дополнительные сведения о записи трассировки см. в разделе [прослушиватели трассировки](https://msdn.microsoft.com/en-us/library/4y5y10s7.aspx) в MSDN.

### <a name="configuring-systemdiagnosticstracewriter"></a>Настройка пространство имен SystemDiagnosticsTraceWriter

Ниже показано, как настроить модуль записи трассировки.

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

Существует два параметра, которыми можно управлять:

- IsVerbose: Если значение равно false, каждой трассировки содержит минимум информации. Значение true, если трассировки содержат больше информации.
- MinimumLevel: Задает минимальный уровень трассировки. Уровни трассировки, в порядке, являются отладки, сведения, предупреждение, ошибка и Неустранимая ошибка.

## <a name="adding-traces-to-your-web-api-application"></a>Добавление в приложение веб-API трассировки

Добавление записи трассировки предоставляющим немедленный доступ для трассировок, созданных по конвейеру веб-API. Можно также использовать модуль записи трассировки для трассировки кода:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

Чтобы получить модуль записи трассировки, вызовите **HttpConfiguration.Services.GetTraceWriter**. От контроллера, этот метод доступен через **ApiController.Configuration** свойство.

Для записи трассировки, можно вызвать метод **ITraceWriter.Trace** непосредственно, но [ITraceWriterExtensions](https://msdn.microsoft.com/en-us/library/system.web.http.tracing.itracewriterextensions.aspx) класс определяет некоторые методы расширений, которые являются более понятным. Например **сведения** в приведенном выше примере создает трассировку с уровнем трассировки **сведения**.

## <a name="web-api-tracing-infrastructure"></a>Инфраструктура веб-API трассировки

В этом разделе описывается создание модуль записи трассировки для веб-API.

Пакета Microsoft.AspNet.WebApi.Tracing построено на основе более общих инфраструктура трассировки в веб-API. Вместо Microsoft.AspNet.WebApi.Tracing, можно также подключить другой трассировка и ведение библиотеки, такие как [NLog](http://nlog-project.org/) или [log4net](http://logging.apache.org/log4net/).

Для сбора данных трассировки, реализовать **ITraceWriter** интерфейса. Ниже приведен простой пример.

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

**ITraceWriter.Trace** метод создает трассировку. Вызывающий объект задает уровень категории и трассировки. Категория может быть любой строкой, определяемой пользователем. Реализация **трассировки** должен выполнить следующие:

1. Создайте новый **TraceRecord**. Инициализируйте его с запросом, категорию и уровень трассировки, как показано. Эти значения предоставляемые вызывающим участником.
2. Вызвать *traceAction* делегата. Внутри этого делегата вызывающий объект должен заполнить остальную часть **TraceRecord**.
3. Запись **TraceRecord**, способом ведения журнала, для которых. В следующем примере просто вызывает **System.Diagnostics.Trace**.

## <a name="setting-the-trace-writer"></a>Настройка записи трассировки

Чтобы включить трассировку, необходимо настроить веб-API для использования вашей **ITraceWriter** реализации. Для этого через **HttpConfiguration** объекта, как показано в следующем коде:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

Модуль записи трассировки только один может быть активна. По умолчанию устанавливает веб-API &quot;холостой&quot; слежения, который не выполняет никаких действий. ( &quot;Холостой&quot; трассировочные существует, поэтому код трассировки не нужно проверять, является ли модуль записи трассировки **null** перед записью трассировки.)

## <a name="how-web-api-tracing-works"></a>Как веб-API, отслеживания работы

Трассировка на веб-API используется в веб-API использует *фасадной* шаблон: когда трассировка включена, веб-API переносится различные части конвейера запросов с классами, которые выполняют вызовы трассировки.

Например, при выборе контроллера, конвейере использует **IHttpControllerSelector** интерфейса. С включенной трассировкой pipleline вставляет класс, реализующий **IHttpControllerSelector** , но вызовы через фактической реализации:

![Трассировка веб-API использует шаблон оболочкой.](tracing-in-aspnet-web-api/_static/image8.png)

Преимущества этой схемы:

- Если не добавить модуль записи трассировки, компоненты трассировки не создаются и не влияет на производительность.
- Если заменить службы по умолчанию, таких как **IHttpControllerSelector** с собственную реализацию, трассировка, не влияет, поскольку объект-оболочка делает трассировки.

Можно также заменить все возможности платформы веб-API трассировки собственные пользовательские framework, заменив значение по умолчанию **ITraceManager** службы:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

Реализуйте **ITraceManager.Initialize** для инициализации системы трассировки. Имейте в виду, что этот параметр заменяет *всей* framework трассировки, включая весь код трассировки, который встроен в веб-API.
