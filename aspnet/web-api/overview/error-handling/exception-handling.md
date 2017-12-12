---
uid: web-api/overview/error-handling/exception-handling
title: "Обработка исключений в ASP.NET Web API | Документы Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2012
ms.topic: article
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: c65ddcca012840d70ab5a33af92edb30041be971
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="exception-handling-in-aspnet-web-api"></a>Обработка исключений в веб-API ASP.NET
====================
по [Mike Wasson](https://github.com/MikeWasson)

Эта статья описывает ошибки и обработка исключений в веб-API ASP.NET.

- [HttpResponseException](#httpresponserexception)
- [Фильтры исключений](#exception_filters)
- [Регистрация фильтры исключений](#registering_exception_filters)
- [Ошибка HTTP](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a>HttpResponseException

Что происходит, если контроллер веб-API вызывает неперехваченное исключение? По умолчанию большинство исключений преобразуются в HTTP-ответа с кодом состояния 500 Внутренняя ошибка сервера.

**HttpResponseException** типа является особым случаем. Это исключение возвращает любой код состояния HTTP, задать в конструкторе исключение. Например, следующий метод возвращает 404, не найден, если *идентификатор* указан недопустимый параметр.

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

Для усиления контроля над ответ также можно создать сообщение ответа и включить его с **HttpResponseException:** 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a>Фильтры исключений

Можно настроить как веб-API обрабатывает исключения, написав *фильтра исключений*. Фильтр исключений выполняется в том случае, когда метод контроллера вызывает любой необработанное исключение, которое является *не* **HttpResponseException** исключение. **HttpResponseException** типа является особым случаем, поскольку он разработан специально для возврата ответа HTTP.

Фильтры исключений реализовать **System.Web.Http.Filters.IExceptionFilter** интерфейса. Самый простой способ записи фильтра исключений является являются производными от **System.Web.Http.Filters.ExceptionFilterAttribute** класса и переопределить **OnException** метод.

> [!NOTE]
> Фильтры исключений в веб-API ASP.NET, аналогичны в ASP.NET MVC. Тем не менее они объявлены в отдельном пространстве имен и функция отдельно. В частности **HandleErrorAttribute** класс, используемый в MVC не обрабатывает исключения, вызванные контроллеров веб-API.


Ниже приведен фильтр, который преобразует **NotImplementedException** 501, не реализован код исключения в код состояния HTTP:

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

**Ответ** свойство **HttpActionExecutedContext** объект содержит сообщения HTTP-ответа, отправляемое клиенту.

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a>Регистрация фильтры исключений

Существует несколько способов для регистрации в фильтре исключений веб-API:

- Действия
- Контроллер
- Глобально

Чтобы применить фильтр для определенных действий, добавьте фильтр как атрибут действие:

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

Чтобы применить фильтр к все действия на контроллере, добавьте фильтр как атрибут в класс контроллера:

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

Чтобы применить фильтр глобально для всех контроллеров веб-API, нужно добавить экземпляр фильтра, который будет **GlobalConfiguration.Configuration.Filters** коллекции. Фильтры сведения в этой коллекции применяются к любому действию контроллера веб-API.

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

Если используется шаблон проекта «ASP.NET MVC 4 веб-приложения» для создания проекта, поместите код конфигурации веб-API в `WebApiConfig` класс, который находится в приложении\_Начальная папка:

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a>Ошибка HTTP

**HttpError** объект предоставляет согласованный способ возвращает сведения об ошибке в тексте ответа. Приведенный ниже показано, как вернуть код состояния HTTP 404 (не найдено) с **HttpError** в тексте ответа.

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

**CreateErrorResponse** метод расширения определен в **System.Net.Http.HttpRequestMessageExtensions** класса. На внутреннем уровне **CreateErrorResponse** создает **HttpError** экземпляра, а затем создает **HttpResponseMessage** , содержащий **HttpError**.

В этом примере если метод выполнен успешно, возвращает произведение в HTTP-ответа. Но если запрошенный продукт не найден, HTTP-ответа содержит **HttpError** в тексте запроса. Ответ может выглядеть следующим образом:

[!code-console[Main](exception-handling/samples/sample9.cmd)]

Обратите внимание, что **HttpError** в этом примере был сериализован в JSON. Одно из преимуществ использования **HttpError** — что они проходят через же [согласования содержимого](../formats-and-model-binding/content-negotiation.md) и процесса сериализации, как и любые другие модели, строго типизированными.

### <a name="httperror-and-model-validation"></a>Ошибка HTTP и проверка модели

Для проверки модели, можно передать состояние модели для **CreateErrorResponse**, чтобы включить ошибки проверки в ответе:

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

В этом примере может возвратить следующий ответ:

[!code-console[Main](exception-handling/samples/sample11.cmd)]

Дополнительные сведения о проверке модели см. в разделе [проверки модели в ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).

### <a name="using-httperror-with-httpresponseexception"></a>Ошибка HTTP при помощи HttpResponseException

В предыдущих примерах возвращаются **HttpResponseMessage** сообщение от действия контроллера, но можно также использовать **HttpResponseException** для возврата **HttpError**. Это позволит вернуть строго типизированных модели в случае обычной успех при возврате по-прежнему **HttpError** Если возникает ошибка:

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
