---
title: Устранение неполадок gRPC в .NET Core
author: jamesnk
description: Устранение ошибок при использовании gRPC в .NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 08/12/2019
uid: grpc/troubleshoot
ms.openlocfilehash: ad74bfa57d2dde316734d55d86075f463e78ee56
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/14/2019
ms.locfileid: "69029037"
---
# <a name="troubleshoot-grpc-on-net-core"></a><span data-ttu-id="6a7ea-103">Устранение неполадок gRPC в .NET Core</span><span class="sxs-lookup"><span data-stu-id="6a7ea-103">Troubleshoot gRPC on .NET Core</span></span>

<span data-ttu-id="6a7ea-104">[Джеймс Ньютона-короля](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="6a7ea-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

## <a name="mismatch-between-client-and-service-ssltls-configuration"></a><span data-ttu-id="6a7ea-105">Несоответствие конфигурации SSL/TLS клиента и службы</span><span class="sxs-lookup"><span data-stu-id="6a7ea-105">Mismatch between client and service SSL/TLS configuration</span></span>

<span data-ttu-id="6a7ea-106">Шаблон gRPC и примеры используют [протокол TLS](https://tools.ietf.org/html/rfc5246) для защиты служб gRPC по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="6a7ea-106">The gRPC template and samples use [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246) to secure gRPC services by default.</span></span> <span data-ttu-id="6a7ea-107">Клиенты gRPC должны использовать безопасное соединение для успешного вызова защищенных служб gRPC.</span><span class="sxs-lookup"><span data-stu-id="6a7ea-107">gRPC clients need to use a secure connection to call secured gRPC services successfully.</span></span>

<span data-ttu-id="6a7ea-108">Вы можете убедиться, что ASP.NET Core служба gRPC использует TLS в журналах, написанных при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="6a7ea-108">You can verify the ASP.NET Core gRPC service is using TLS in the logs written on app start.</span></span> <span data-ttu-id="6a7ea-109">Служба будет прослушивать конечную точку HTTPS:</span><span class="sxs-lookup"><span data-stu-id="6a7ea-109">The service will be listening on an HTTPS endpoint:</span></span>

```
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
```

<span data-ttu-id="6a7ea-110">Клиент .NET Core должен использовать `https` в адресе сервера для выполнения вызовов с защищенным соединением:</span><span class="sxs-lookup"><span data-stu-id="6a7ea-110">The .NET Core client must use `https` in the server address to make calls with a secured connection:</span></span>

```csharp
static async Task Main(string[] args)
{
    var httpClient = new HttpClient();
    // The port number(5001) must match the port of the gRPC server.
    httpClient.BaseAddress = new Uri("https://localhost:5001");
    var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
}
```

<span data-ttu-id="6a7ea-111">Все реализации клиента gRPC поддерживают протокол TLS.</span><span class="sxs-lookup"><span data-stu-id="6a7ea-111">All gRPC client implementations support TLS.</span></span> <span data-ttu-id="6a7ea-112">для клиентов gRPC из других языков обычно требуется канал, настроенный с помощью `SslCredentials`.</span><span class="sxs-lookup"><span data-stu-id="6a7ea-112">gRPC clients from other languages typically require the channel configured with `SslCredentials`.</span></span> <span data-ttu-id="6a7ea-113">`SslCredentials`Указывает сертификат, который будет использоваться клиентом, и его необходимо использовать вместо незащищенных учетных данных.</span><span class="sxs-lookup"><span data-stu-id="6a7ea-113">`SslCredentials` specifies the certificate that the client will use, and it must be used instead of insecure credentials.</span></span> <span data-ttu-id="6a7ea-114">Примеры настройки различных реализаций клиента gRPC для использования TLS см. в разделе [Проверка подлинности gRPC](https://www.grpc.io/docs/guides/auth/).</span><span class="sxs-lookup"><span data-stu-id="6a7ea-114">For examples of configuring the different gRPC client implementations to use TLS, see [gRPC Authentication](https://www.grpc.io/docs/guides/auth/).</span></span>

## <a name="call-insecure-grpc-services-with-net-core-client"></a><span data-ttu-id="6a7ea-115">Вызов незащищенных служб gRPC Services с помощью клиента .NET Core</span><span class="sxs-lookup"><span data-stu-id="6a7ea-115">Call insecure gRPC services with .NET Core client</span></span>

<span data-ttu-id="6a7ea-116">Для вызова незащищенных служб gRPC с клиентом .NET Core требуется дополнительная настройка.</span><span class="sxs-lookup"><span data-stu-id="6a7ea-116">Additional configuration is required to call insecure gRPC services with the .NET Core client.</span></span> <span data-ttu-id="6a7ea-117">Клиент gRPC должен задать `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` для `true` параметра значение и использовать `http` в адресе сервера:</span><span class="sxs-lookup"><span data-stu-id="6a7ea-117">The gRPC client must set the `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` switch to `true` and use `http` in the server address:</span></span>

```csharp
// This switch must be set before creating the HttpClient.
AppContext.SetSwitch("System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport", true);

var httpClient = new HttpClient();
// The port number(5000) must match the port of the gRPC server.
httpClient.BaseAddress = new Uri("http://localhost:5000");
var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
```

## <a name="unable-to-start-aspnet-core-grpc-app-on-macos"></a><span data-ttu-id="6a7ea-118">Не удается запустить ASP.NET Core приложение gRPC на macOS</span><span class="sxs-lookup"><span data-stu-id="6a7ea-118">Unable to start ASP.NET Core gRPC app on macOS</span></span>

<span data-ttu-id="6a7ea-119">Kestrel не поддерживает HTTP/2 с TLS в macOS и более ранних версиях Windows, таких как Windows 7.</span><span class="sxs-lookup"><span data-stu-id="6a7ea-119">Kestrel doesn't support HTTP/2 with TLS on macOS and older Windows versions such as Windows 7.</span></span> <span data-ttu-id="6a7ea-120">Шаблон ASP.NET Core gRPC и примеры по умолчанию используют протокол TLS.</span><span class="sxs-lookup"><span data-stu-id="6a7ea-120">The ASP.NET Core gRPC template and samples use TLS by default.</span></span> <span data-ttu-id="6a7ea-121">При попытке запустить сервер gRPC появится следующее сообщение об ошибке:</span><span class="sxs-lookup"><span data-stu-id="6a7ea-121">You'll see the following error message when you attempt to start the gRPC server:</span></span>

> <span data-ttu-id="6a7ea-122">Не удалось выполнить привязку к https://localhost:5001 интерфейсу замыкания на себя IPv4: "HTTP/2 через TLS не поддерживается в macOS из-за отсутствия поддержки АЛПН".</span><span class="sxs-lookup"><span data-stu-id="6a7ea-122">Unable to bind to https://localhost:5001 on the IPv4 loopback interface: 'HTTP/2 over TLS is not supported on macOS due to missing ALPN support.'.</span></span>

<span data-ttu-id="6a7ea-123">Чтобы обойти эту ошибку, настройте Kestrel и клиент gRPC для использования HTTP/2 *без* TLS.</span><span class="sxs-lookup"><span data-stu-id="6a7ea-123">To work around this issue, configure Kestrel and the gRPC client to use HTTP/2 *without* TLS.</span></span> <span data-ttu-id="6a7ea-124">Это следует делать только во время разработки.</span><span class="sxs-lookup"><span data-stu-id="6a7ea-124">You should only do this during development.</span></span> <span data-ttu-id="6a7ea-125">Отсутствие использования TLS приведет к тому, что сообщения gRPC будут отправляться без шифрования.</span><span class="sxs-lookup"><span data-stu-id="6a7ea-125">Not using TLS will result in gRPC messages being sent without encryption.</span></span>

<span data-ttu-id="6a7ea-126">Kestrel должен настроить конечную точку HTTP/2 без TLS в *Program.CS*:</span><span class="sxs-lookup"><span data-stu-id="6a7ea-126">Kestrel must configure an HTTP/2 endpoint without TLS in *Program.cs*:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(options =>
            {
                // Setup a HTTP/2 endpoint without TLS.
                options.ListenLocalhost(5000, o => o.Protocols = HttpProtocols.Http2);
            });
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="6a7ea-127">Кроме того, клиент gRPC должен быть настроен так, чтобы не использовал TLS.</span><span class="sxs-lookup"><span data-stu-id="6a7ea-127">The gRPC client must also be configured to not use TLS.</span></span> <span data-ttu-id="6a7ea-128">Дополнительные сведения см. в статье [вызов незащищенных служб gRPC Services с помощью клиента .NET Core](#call-insecure-grpc-services-with-net-core-client).</span><span class="sxs-lookup"><span data-stu-id="6a7ea-128">For more information, see [Call insecure gRPC services with .NET Core client](#call-insecure-grpc-services-with-net-core-client).</span></span>

> [!WARNING]
> <span data-ttu-id="6a7ea-129">HTTP/2 без TLS следует использовать только во время разработки приложений.</span><span class="sxs-lookup"><span data-stu-id="6a7ea-129">HTTP/2 without TLS should only be used during app development.</span></span> <span data-ttu-id="6a7ea-130">В рабочих приложениях всегда следует использовать безопасность транспорта.</span><span class="sxs-lookup"><span data-stu-id="6a7ea-130">Production apps should always use transport security.</span></span> <span data-ttu-id="6a7ea-131">Дополнительные сведения см. [в разделе вопросы безопасности в gRPC for ASP.NET Core](xref:grpc/security#transport-security).</span><span class="sxs-lookup"><span data-stu-id="6a7ea-131">For more information, see [Security considerations in gRPC for ASP.NET Core](xref:grpc/security#transport-security).</span></span>

## <a name="grpc-c-assets-are-not-code-generated-from-proto-files"></a><span data-ttu-id="6a7ea-132">ресурсы C# gRPC не являются кодом, созданным из файлов с  *\*.*</span><span class="sxs-lookup"><span data-stu-id="6a7ea-132">gRPC C# assets are not code generated from *\*.proto* files</span></span>

<span data-ttu-id="6a7ea-133">Создание кода gRPC для конкретных клиентов и базовых классов служб требует наличия в проекте ссылок на файлы и инструментарий protobuf.</span><span class="sxs-lookup"><span data-stu-id="6a7ea-133">gRPC code generation of concrete clients and service base classes requires protobuf files and tooling to be referenced from a project.</span></span> <span data-ttu-id="6a7ea-134">Необходимо включить:</span><span class="sxs-lookup"><span data-stu-id="6a7ea-134">You must include:</span></span>

* <span data-ttu-id="6a7ea-135">*.* укажите файлы, которые необходимо использовать в `<Protobuf>` группе элементов.</span><span class="sxs-lookup"><span data-stu-id="6a7ea-135">*.proto* files you want to use in the `<Protobuf>` item group.</span></span> <span data-ttu-id="6a7ea-136">Проект должен ссылаться на [импортированные файлы ](https://developers.google.com/protocol-buffers/docs/proto3#importing-definitions) с указанными файлами.</span><span class="sxs-lookup"><span data-stu-id="6a7ea-136">[Imported *.proto* files](https://developers.google.com/protocol-buffers/docs/proto3#importing-definitions) must be referenced by the project.</span></span>
* <span data-ttu-id="6a7ea-137">Ссылка на пакет для пакета инструментов gRPC [gRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/).</span><span class="sxs-lookup"><span data-stu-id="6a7ea-137">Package reference to the gRPC tooling package [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/).</span></span>

<span data-ttu-id="6a7ea-138">Дополнительные сведения о создании ресурсов gRPC C# см. в <xref:grpc/basics>разделе.</span><span class="sxs-lookup"><span data-stu-id="6a7ea-138">For more information on generating gRPC C# assets, see <xref:grpc/basics>.</span></span>

<span data-ttu-id="6a7ea-139">По умолчанию `<Protobuf>` ссылка создает конкретный клиент и базовый класс службы.</span><span class="sxs-lookup"><span data-stu-id="6a7ea-139">By default, a `<Protobuf>` reference generates a concrete client and a service base class.</span></span> <span data-ttu-id="6a7ea-140">`GrpcServices` Атрибут элемента Reference можно использовать для ограничения C# создания ресурсов.</span><span class="sxs-lookup"><span data-stu-id="6a7ea-140">The reference element's `GrpcServices` attribute can be used to limit C# asset generation.</span></span> <span data-ttu-id="6a7ea-141">Допустимые `GrpcServices` параметры:</span><span class="sxs-lookup"><span data-stu-id="6a7ea-141">Valid `GrpcServices` options are:</span></span>

* <span data-ttu-id="6a7ea-142">`Both`(по умолчанию, если отсутствует)</span><span class="sxs-lookup"><span data-stu-id="6a7ea-142">`Both` (default when not present)</span></span>
* `Server`
* `Client`
* `None`

<span data-ttu-id="6a7ea-143">Для ASP.NET Core веб-приложения, в котором размещены службы gRPC Services, требуется только базовый класс службы:</span><span class="sxs-lookup"><span data-stu-id="6a7ea-143">An ASP.NET Core web app hosting gRPC services only needs the service base class generated:</span></span>

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" GrpcServices="Server" />
</ItemGroup>
```

<span data-ttu-id="6a7ea-144">Клиентское приложение gRPC, выполняющее вызовы gRPC, требует только создания конкретного клиента:</span><span class="sxs-lookup"><span data-stu-id="6a7ea-144">A gRPC client app making gRPC calls only needs the concrete client generated:</span></span>

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
</ItemGroup>
```
