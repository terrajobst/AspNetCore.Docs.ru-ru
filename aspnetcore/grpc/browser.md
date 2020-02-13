---
title: Использование gRPC в приложениях на основе браузера
author: jamesnk
description: Узнайте, как настроить gRPC Services на ASP.NET Core для вызова из приложений браузера с помощью gRPC-Web.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 02/10/2020
uid: grpc/browser
ms.openlocfilehash: 333fc8c4277bbac47042d4904c276e963186914a
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172276"
---
# <a name="use-grpc-in-browser-apps"></a>Использование gRPC в приложениях на основе браузера

[Джеймс Ньютона-короля](https://twitter.com/jamesnk)

> [!IMPORTANT]
> **gRPC — веб-поддержка в .NET — экспериментальная**
>
> gRPC-Web для .NET — это экспериментальный проект, а не фиксированный продукт. Мы хотим:
>
> * Проверьте, что наш подход к реализации gRPC-Web работает.
> * Получите отзыв о том, если этот подход полезен разработчикам .NET по сравнению с традиционным способом настройки gRPC-Web через прокси-сервер.
>
> Оставьте отзыв на [https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet) , чтобы убедиться в том, что мы создаем что-то, что и для разработчиков.

Невозможно вызвать службу HTTP/2 gRPC из приложения на основе браузера. [gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) — это протокол, позволяющий браузеру JavaScript и приложениям блазор вызывать службы gRPC. В этой статье объясняется, как использовать gRPC-Web в .NET Core.

## <a name="configure-grpc-web-in-aspnet-core"></a>Настройка gRPC-Web в ASP.NET Core

службы gRPC, размещенные в ASP.NET Core, можно настроить для поддержки gRPC-Web вместе с HTTP/2 gRPC. gRPC — веб-сайт не требует изменений в службах. Единственным изменением является конфигурация запуска.

Чтобы включить gRPC-Web со службой ASP.NET Core gRPC, выполните следующие действия.

* Добавьте ссылку на пакет [GRPC. AspNetCore. Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web) .
* Настройте приложение для использования gRPC-Web, добавив `AddGrpcWeb` и `UseGrpcWeb` в *Startup.CS*:

[!code-csharp[](~/grpc/browser/sample/Startup.cs?name=snippet_1&highlight=10,14)]

Предыдущий код:

* Добавляет по промежуточного слоя gRPC-Web, `UseGrpcWeb`, после маршрутизации и перед конечными точками.
* Указывает метод `endpoints.MapGrpcService<GreeterService>()` поддерживает gRPC-Web с `EnableGrpcWeb`. 

Кроме того, можно настроить все службы для поддержки gRPC-Web, добавив `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` в ConfigureServices.

[!code-csharp[](~/grpc/browser/sample/AllServicesSupportExample_Startup.cs?name=snippet_1&highlight=6,13)]

Для вызова gRPC-Web из браузера может потребоваться дополнительная настройка, например настройка ASP.NET Core для поддержки CORS. Дополнительные сведения см. в разделе [Поддержка CORS](xref:security/cors).

## <a name="call-grpc-web-from-the-browser"></a>Вызов gRPC-Web из браузера

Приложения браузера могут использовать gRPC-Web для вызова служб gRPC. При вызове gRPC Services с помощью gRPC-Web из браузера существуют некоторые требования и ограничения.

* Сервер должен быть настроен для поддержки gRPC-Web.
* Потоковая передача клиента и вызовы двунаправленной потоковой передачи не поддерживаются. Поддерживается потоковая передача сервера.
* Для вызова служб gRPC в другом домене необходимо настроить [CORS](xref:security/cors) на сервере.

### <a name="javascript-grpc-web-client"></a>JavaScript gRPC — веб-клиент

Существует клиент JavaScript gRPC-Web. Инструкции по использованию gRPC-Web из JavaScript см. в статье [написание кода клиента JavaScript с помощью gRPC-Web](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code).

### <a name="configure-grpc-web-with-the-net-grpc-client"></a>Настройка gRPC-Web с помощью клиента .NET gRPC

Клиент .NET gRPC можно настроить для выполнения вызовов gRPC-Web. Это полезно для Блазор приложений веб- [сборки](xref:blazor/index#blazor-webassembly) , которые размещаются в браузере и имеют те же ограничения HTTP, что и код JavaScript. Вызов gRPC-Web с клиентом .NET аналогичен [http/2 gRPC](xref:grpc/client). Единственным изменением является то, как создается канал.

Чтобы использовать gRPC-Web:

* Добавьте ссылку на пакет [GRPC .NET. Client. Web](https://www.nuget.org/packages/Grpc.Net.Client.Web) .
* Убедитесь, что ссылка на [GRPC .NET. Client](https://www.nuget.org/packages/Grpc.Net.Client) Package находится в 2.27.0 или более поздней версии.
* Настройте канал для использования `GrpcWebHandler`:

[!code-csharp[](~/grpc/browser/sample/Handler.cs?name=snippet_1)]

Предыдущий код:

* Настраивает канал для использования gRPC-Web.
* Создает клиент и выполняет вызов с помощью канала.

При создании `GrpcWebHandler` имеет следующие параметры конфигурации:

* **Иннерхандлер**: базовый <xref:System.Net.Http.HttpMessageHandler>, который делает HTTP-запрос gRPC, например `HttpClientHandler`.
* **Mode**: тип перечисления, указывающий, `Content-Type` ли запрос HTTP-запроса gRPC `application/grpc-web` или `application/grpc-web-text`.
    * `GrpcWebMode.GrpcWeb` настраивает содержимое для отправки без кодирования. Значение по умолчанию.
    * `GrpcWebMode.GrpcWebText` настраивает содержимое в кодировке Base64. Требуется для вызовов потоковой передачи сервера в браузерах.
* **Хттпверсион**: протокол HTTP `Version` используется для задания [HttpRequestMessage. Version](xref:System.Net.Http.HttpRequestMessage.Version) в базовом HTTP-запросе gRPC. gRPC-Web не требует определенной версии и не переопределяет значение по умолчанию, если не указано иное.

> [!IMPORTANT]
> Созданные клиенты gRPC имеют синхронные и асинхронные методы для вызова унарных методов. Например, `SayHello` синхронизируется и `SayHelloAsync` является асинхронным. Вызов метода Sync в приложении Блазор сборки приведет к тому, что приложение перестанет отвечать на запросы. Асинхронные методы всегда должны использоваться в Блазор.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [проект GitHub gRPC для веб-клиентов](https://github.com/grpc/grpc-web)
* <xref:security/cors>
