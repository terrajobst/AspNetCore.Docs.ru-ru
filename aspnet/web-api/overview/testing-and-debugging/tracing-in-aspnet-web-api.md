---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: Трассировка в ASP.NET Web API 2 | Документация Майкрософт
author: MikeWasson
description: Показано, как включить трассировку в веб-API ASP.NET.
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 66a837e9-600b-4b72-97a9-19804231c64a
msc.legacyurl: /web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 697e7e91ae2d9d5712d9306a291635793063117b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838507"
---
<a name="tracing-in-aspnet-web-api-2"></a>Трассировка в ASP.NET Web API 2
====================
по [Майк Уоссон](https://github.com/MikeWasson)

> Когда вы пытаетесь отладить веб-приложение, нет альтернативы для хороший набор журналов трассировки. Этом руководстве показано, как включить трассировку в веб-API ASP.NET. Эту функцию можно использовать для трассировки, что платформа веб-API делает до и после он вызывает вашего контроллера. Также можно отслеживать собственный код.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемые в этом руководстве
> 
> 
> - [Visual Studio 2017](https://www.visualstudio.com/downloads/) (также работает с Visual Studio 2015)
> - Веб-API 2
> - [Microsoft.AspNet.WebApi.Tracing](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)


## <a name="enable-systemdiagnostics-tracing-in-web-api"></a>Включение трассировки в веб-API System.Diagnostics

Во-первых мы создадим новый проект веб-приложения ASP.NET. В Visual Studio из **файл** меню, выберите **New**, затем **проекта**. В разделе **шаблоны**, **Web**выберите **веб-приложение ASP.NET**.

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

Выберите шаблон проекта веб-API.

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

Из **средства** меню, выберите **диспетчер пакетов библиотеки**, затем **консоли**.

В окне консоли диспетчера пакетов введите следующие команды.

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

Первая команда устанавливает последнюю версию пакета трассировки веб-API. Он также обновляет основные пакеты веб-API. Вторая команда обновляет пакет WebApi.WebHost до последней версии.

> [!NOTE]
> Если вы хотите охватить определенную версию веб-API, используйте флаг версии, при установке пакета трассировки.


Откройте файл WebApiConfig.cs в приложении\_Начальная папка. Добавьте следующий код, чтобы **зарегистрировать** метод.

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

Этот код добавляет [пространство имен SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) класс в конвейер веб-API. **Пространство имен SystemDiagnosticsTraceWriter** класс записывает трассировки, чтобы [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace).

Чтобы просмотреть трассировки, запустите приложение в отладчике. В браузере перейдите к `/api/values`.

![](tracing-in-aspnet-web-api/_static/image5.png)

Инструкции трассировки записываются в окно вывода в Visual Studio. (Из **представление** меню, выберите **вывода**).

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

Так как **пространство имен SystemDiagnosticsTraceWriter** записывает трассировки, чтобы **System.Diagnostics.Trace**, вы можете зарегистрировать дополнительные прослушиватели трассировки; например, чтобы записать трассировки в файл журнала. Дополнительные сведения о записи трассировки, см. в разделе [прослушиватели трассировки](https://msdn.microsoft.com/library/4y5y10s7.aspx) в MSDN.

### <a name="configuring-systemdiagnosticstracewriter"></a>Настройка пространство имен SystemDiagnosticsTraceWriter

Ниже показано, как настроить модуль записи трассировки.

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

Существует два параметра, которыми можно управлять:

- IsVerbose: Если значение равно false, каждой трассировки содержит минимум информации. Если значение равно true, трассировки включают дополнительные сведения.
- MinimumLevel: Задает минимальный уровень трассировки. Уровни трассировки, в порядке,:, отладки, Info, Warn, ошибка и Неустранимая ошибка.

## <a name="adding-traces-to-your-web-api-application"></a>Добавление трассировки в приложение веб-API

Добавление записи трассировки предоставляющим немедленный доступ к трассировок, созданных по конвейеру веб-API. Также можно использовать модуль записи трассировки для трассировки кода:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

Чтобы получить модуль записи трассировки, вызовите **HttpConfiguration.Services.GetTraceWriter**. От контроллера, этот метод доступен через **ApiController.Configuration** свойство.

Для записи трассировки, можно вызвать **ITraceWriter.Trace** метод напрямую, но [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) класс определяет некоторые методы расширений, которые являются более понятным. Например **Info** в приведенном выше примере создает трассировку с уровнем трассировки **Info**.

## <a name="web-api-tracing-infrastructure"></a>Инфраструктура трассировки веб-API

В этом разделе описывается, как написать средство записи отслеживания для веб-API.

Пакет Microsoft.AspNet.WebApi.Tracing надстраивается над более общими инфраструктура трассировки в веб-API. Вместо использования Microsoft.AspNet.WebApi.Tracing, можно также подключить другой библиотеки трассировки/входа, такие как [NLog](http://nlog-project.org/) или [log4net](http://logging.apache.org/log4net/).

Для сбора данных трассировки, реализовать **ITraceWriter** интерфейс. Ниже приведен простой пример:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

**ITraceWriter.Trace** метод создает объект trace. Вызывающий объект задает уровень категории и трассировки. Категория может быть любая строка, определяемые пользователем. Реализация **трассировки** делать следующее:

1. Создайте новый **TraceRecord**. Инициализируйте его с запросом, категорию и уровень трассировки, как показано. Эти значения предоставляемые вызывающим объектом.
2. Вызвать *traceAction* делегировать. Внутри этот делегат, вызывающий объект должен заполнить в остальной части **TraceRecord**.
3. Запись **TraceRecord**, с помощью любой подходящей методики ведения журнала, что вам нравится. В следующем примере просто вызывает **System.Diagnostics.Trace**.

## <a name="setting-the-trace-writer"></a>Настройка записи трассировки

Чтобы включить трассировку, необходимо настроить веб-API для использования вашей **ITraceWriter** реализации. Это сделать через **HttpConfiguration** объекта, как показано в следующем коде:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

Модуль записи трассировки только один может быть активна. По умолчанию устанавливает веб-API &quot;холостой&quot; слежения, который не выполняет никаких действий. ( &quot;Холостой&quot; трассировочного существует, поэтому код трассировки не нужно проверить, является ли модуль записи трассировки **null** перед записью трассировки.)

## <a name="how-web-api-tracing-works"></a>Как веб-API, трассировка Works

Трассировка в веб-API используется в веб-API использует *фасадной* шаблон: Если трассировка включена, веб-API создает оболочку различные части конвейера запросов с классами, которые выполняют вызовы трассировки.

Например, при выборе контроллера, этот конвейер использует **IHttpControllerSelector** интерфейс. С включенной трассировкой, pipleline вставляет класс, реализующий **IHttpControllerSelector** , но вызовы через настоящей реализации:

![Трассировка веб-API использует шаблон фасадной.](tracing-in-aspnet-web-api/_static/image8.png)

Такая схема следующие преимущества:

- Если вы не добавите модуль записи трассировки, компоненты трассировки не создаются и не влияют на производительность.
- Если заменить службы по умолчанию, такие как **IHttpControllerSelector** с собственную реализацию, трассировка не влияет, так как трассировка выполняется путем объект-оболочку.

Также можно заменить всю платформу .NET framework трассировки веб-API с помощью собственных пользовательских framework, заменив значение по умолчанию **ITraceManager** службы:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

Реализуйте **ITraceManager.Initialize** для инициализации ваша система трассировки. Имейте в виду, что этот параметр заменяет *всей* framework трассировки, включая весь код трассировки, встроенный в веб-API.
