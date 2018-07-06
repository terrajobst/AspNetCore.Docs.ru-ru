---
uid: web-api/overview/error-handling/exception-handling
title: Обработка исключений в веб-API ASP.NET | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 03/12/2012
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: 2db00c1ab241e0dad051c2a3bcd3b0e4bbf4d5c4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807665"
---
<a name="exception-handling-in-aspnet-web-api"></a>Обработка исключений в веб-API ASP.NET
====================
по [Майк Уоссон](https://github.com/MikeWasson)

В этой статье описывается ошибка и обработка исключений в веб-API ASP.NET.

- [HttpResponseException](#httpresponserexception)
- [Фильтры исключений](#exception_filters)
- [Регистрация фильтры исключений](#registering_exception_filters)
- [Ошибка HTTP](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a>HttpResponseException

Что произойдет, если контроллер Web API создает неперехваченное исключение? По умолчанию большинство исключений, преобразуются в ответ HTTP с кодом состояния 500 Внутренняя ошибка сервера.

**HttpResponseException** типа является особым случаем. Это исключение возвращает любой код состояния HTTP, указанный в конструкторе исключения. Например, следующий метод возвращает ошибку 404, не найден, если *идентификатор* недопустимый параметр.

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

Для большего контроля над ответом, можно также создать всего сообщения ответа и включить его с **HttpResponseException:** 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a>Фильтры исключений

Можно настроить как веб-API обрабатывает исключения, написав *фильтра исключений*. Фильтр исключений выполняется в том случае, когда метод контроллера выдает любое необработанное исключение, которое является *не* **HttpResponseException** исключение. **HttpResponseException** типа является особым случаем, поскольку он разработан специально для возврата HTTP-ответа.

Реализовывают **System.Web.Http.Filters.IExceptionFilter** интерфейс. Самый простой способ создать фильтр исключений является наследование от **System.Web.Http.Filters.ExceptionFilterAttribute** класса и переопределить **OnException** метод.

> [!NOTE]
> Фильтры исключений в веб-API ASP.NET, аналогичны в ASP.NET MVC. Тем не менее они объявлены в отдельном пространстве имен и функция отдельно. В частности **HandleErrorAttribute** класс, используемый в MVC не обрабатывает исключения, создаваемые контроллеров веб-API.


Ниже приведен фильтр, который преобразует **NotImplementedException** исключения в HTTP-состояния кода 501, не реализованы:

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

**Ответа** свойство **HttpActionExecutedContext** объект содержит сообщение HTTP-ответа, отправляемого клиенту.

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a>Регистрация фильтры исключений

Существует несколько способов регистрации фильтра исключений веб-API:

- Действием
- Контроллером
- Глобально

Чтобы применить фильтр для определенных действий, добавьте фильтр как атрибут действия:

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

Чтобы применить фильтр, чтобы все действия на контроллере, добавьте фильтр как атрибут в класс контроллера:

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

Чтобы применить фильтр глобально ко всем контроллерам веб-API, добавьте экземпляр фильтра, который **GlobalConfiguration.Configuration.Filters** коллекции. В этой коллекции фильтры об исключении применяются к любому действию контроллера веб-API.

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

Если вы используете шаблон проекта «ASP.NET MVC 4 веб-приложение» для создания проекта, поместите код конфигурации веб-API внутри `WebApiConfig` класс, который находится в приложении\_Начальная папка:

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a>Ошибка HTTP

**HttpError** объекта обеспечивают согласованное выполнение для возврата сведений об ошибке в тексте ответа. В следующем примере показано, как вернуть код состояния HTTP 404 (не найдено) с **HttpError** в тексте ответа.

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

**CreateErrorResponse** является методом расширения, определенные в **System.Net.Http.HttpRequestMessageExtensions** класса. На внутреннем уровне **CreateErrorResponse** создает **HttpError** экземпляра, а затем создает **HttpResponseMessage** , содержащий **HttpError**.

В этом примере если метод выполнен успешно, возвращает продукт в HTTP-ответа. Но если запрашиваемого продукта не найден, ответ HTTP содержит **HttpError** в тексте запроса. Ответ может выглядеть следующим образом:

[!code-console[Main](exception-handling/samples/sample9.cmd)]

Обратите внимание, что **HttpError** был сериализован в формат JSON, в этом примере. Одно из преимуществ использования **HttpError** — что он проходит через же [согласования содержимого](../formats-and-model-binding/content-negotiation.md) и все другие строго типизированную модель процесса сериализации.

### <a name="httperror-and-model-validation"></a>Ошибка HTTP и проверке модели

Для проверки модели, можно передать в состояние модели, чтобы **CreateErrorResponse**, чтобы включить ошибки проверки в ответе:

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

В этом примере может вернуть следующий ответ:

[!code-console[Main](exception-handling/samples/sample11.cmd)]

Дополнительные сведения о проверке модели, см. в разделе [проверка модели в веб-API ASP.NET](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).

### <a name="using-httperror-with-httpresponseexception"></a>С помощью Ошибка HTTP с HttpResponseException

В предыдущих примерах возвращаются **HttpResponseMessage** сообщение от действия контроллера, но можно также использовать **HttpResponseException** для возврата **HttpError**. Это позволит вернуть строго типизированную модель в случае обычной успеха при возврате по-прежнему **HttpError** Если возникает ошибка:

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
