---
title: Устранение неполадок gRPC в .NET Core
author: jamesnk
description: Устранение ошибок при использовании gRPC в .NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 09/21/2019
uid: grpc/troubleshoot
ms.openlocfilehash: 15377ba4b31ce9319df300b23e5a95c67bca7db4
ms.sourcegitcommit: 04ce94b3c1b01d167f30eed60c1c95446dfe759d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/21/2019
ms.locfileid: "71176506"
---
# <a name="troubleshoot-grpc-on-net-core"></a><span data-ttu-id="a5995-103">Устранение неполадок gRPC в .NET Core</span><span class="sxs-lookup"><span data-stu-id="a5995-103">Troubleshoot gRPC on .NET Core</span></span>

<span data-ttu-id="a5995-104">[Джеймс Ньютона-короля](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="a5995-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

<span data-ttu-id="a5995-105">В этом документе обсуждаются часто встречающиеся проблемы при разработке gRPC приложений на .NET.</span><span class="sxs-lookup"><span data-stu-id="a5995-105">This document discusses commonly encountered problems when developing gRPC apps on .NET.</span></span>

## <a name="mismatch-between-client-and-service-ssltls-configuration"></a><span data-ttu-id="a5995-106">Несоответствие конфигурации SSL/TLS клиента и службы</span><span class="sxs-lookup"><span data-stu-id="a5995-106">Mismatch between client and service SSL/TLS configuration</span></span>

<span data-ttu-id="a5995-107">Шаблон gRPC и примеры используют [протокол TLS](https://tools.ietf.org/html/rfc5246) для защиты служб gRPC по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="a5995-107">The gRPC template and samples use [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246) to secure gRPC services by default.</span></span> <span data-ttu-id="a5995-108">Клиенты gRPC должны использовать безопасное соединение для успешного вызова защищенных служб gRPC.</span><span class="sxs-lookup"><span data-stu-id="a5995-108">gRPC clients need to use a secure connection to call secured gRPC services successfully.</span></span>

<span data-ttu-id="a5995-109">Вы можете убедиться, что ASP.NET Core служба gRPC использует TLS в журналах, написанных при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="a5995-109">You can verify the ASP.NET Core gRPC service is using TLS in the logs written on app start.</span></span> <span data-ttu-id="a5995-110">Служба будет прослушивать конечную точку HTTPS:</span><span class="sxs-lookup"><span data-stu-id="a5995-110">The service will be listening on an HTTPS endpoint:</span></span>

```
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
```

<span data-ttu-id="a5995-111">Клиент .NET Core должен использовать `https` в адресе сервера для выполнения вызовов с защищенным соединением:</span><span class="sxs-lookup"><span data-stu-id="a5995-111">The .NET Core client must use `https` in the server address to make calls with a secured connection:</span></span>

```csharp
static async Task Main(string[] args)
{
    // The port number(5001) must match the port of the gRPC server.
    var channel = GrpcChannel.ForAddress("https://localhost:5001");
    var client = new Greet.GreeterClient(channel);
}
```

<span data-ttu-id="a5995-112">Все реализации клиента gRPC поддерживают протокол TLS.</span><span class="sxs-lookup"><span data-stu-id="a5995-112">All gRPC client implementations support TLS.</span></span> <span data-ttu-id="a5995-113">для клиентов gRPC из других языков обычно требуется канал, настроенный с помощью `SslCredentials`.</span><span class="sxs-lookup"><span data-stu-id="a5995-113">gRPC clients from other languages typically require the channel configured with `SslCredentials`.</span></span> <span data-ttu-id="a5995-114">`SslCredentials`Указывает сертификат, который будет использоваться клиентом, и его необходимо использовать вместо незащищенных учетных данных.</span><span class="sxs-lookup"><span data-stu-id="a5995-114">`SslCredentials` specifies the certificate that the client will use, and it must be used instead of insecure credentials.</span></span> <span data-ttu-id="a5995-115">Примеры настройки различных реализаций клиента gRPC для использования TLS см. в разделе [Проверка подлинности gRPC](https://www.grpc.io/docs/guides/auth/).</span><span class="sxs-lookup"><span data-stu-id="a5995-115">For examples of configuring the different gRPC client implementations to use TLS, see [gRPC Authentication](https://www.grpc.io/docs/guides/auth/).</span></span>

## <a name="call-a-grpc-service-with-an-untrustedinvalid-certificate"></a><span data-ttu-id="a5995-116">Вызов службы gRPC с недоверенным или недопустимым сертификатом</span><span class="sxs-lookup"><span data-stu-id="a5995-116">Call a gRPC service with an untrusted/invalid certificate</span></span>

<span data-ttu-id="a5995-117">Для клиента .NET gRPC служба должна иметь доверенный сертификат.</span><span class="sxs-lookup"><span data-stu-id="a5995-117">The .NET gRPC client requires the service to have a trusted certificate.</span></span> <span data-ttu-id="a5995-118">При вызове службы gRPC без доверенного сертификата возвращается следующее сообщение об ошибке:</span><span class="sxs-lookup"><span data-stu-id="a5995-118">The following error message is returned when calling a gRPC service without a trusted certificate:</span></span>

> <span data-ttu-id="a5995-119">Необработанное исключение.</span><span class="sxs-lookup"><span data-stu-id="a5995-119">Unhandled exception.</span></span> <span data-ttu-id="a5995-120">System .NET. http. HttpRequestException: Не удалось установить SSL-соединение, см. внутреннее исключение.</span><span class="sxs-lookup"><span data-stu-id="a5995-120">System.Net.Http.HttpRequestException: The SSL connection could not be established, see inner exception.</span></span>
> <span data-ttu-id="a5995-121">---> System. Security. Authentication. исключение: В соответствии с процедурой проверки удаленный сертификат является недопустимым.</span><span class="sxs-lookup"><span data-stu-id="a5995-121">---> System.Security.Authentication.AuthenticationException: The remote certificate is invalid according to the validation procedure.</span></span>

<span data-ttu-id="a5995-122">Эта ошибка может появиться, если вы тестируете приложение локально, а ASP.NET Core сертификат разработки HTTPS не является доверенным.</span><span class="sxs-lookup"><span data-stu-id="a5995-122">You may see this error if you are testing your app locally and the ASP.NET Core HTTPS development certificate is not trusted.</span></span> <span data-ttu-id="a5995-123">См. сведения об устранении этой проблемы в руководстве по [настройке доверия к сертификату разработки HTTPS ASP.NET Core в Windows и macOS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).</span><span class="sxs-lookup"><span data-stu-id="a5995-123">For instructions to fix this issue, see [Trust the ASP.NET Core HTTPS development certificate on Windows and macOS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).</span></span>

<span data-ttu-id="a5995-124">Если вы вызываете службу gRPC на другом компьютере и не можете доверять сертификату, то клиент gRPC можно настроить на игнорирование недействительного сертификата.</span><span class="sxs-lookup"><span data-stu-id="a5995-124">If you are calling a gRPC service on another machine and are unable to trust the certificate then the gRPC client can be configured to ignore the invalid certificate.</span></span> <span data-ttu-id="a5995-125">В следующем коде используется [HttpClientHandler. серверцертификатекустомвалидатионкаллбакк](/dotnet/api/system.net.http.httpclienthandler.servercertificatecustomvalidationcallback) , чтобы разрешить вызовы без доверенного сертификата:</span><span class="sxs-lookup"><span data-stu-id="a5995-125">The following code uses [HttpClientHandler.ServerCertificateCustomValidationCallback](/dotnet/api/system.net.http.httpclienthandler.servercertificatecustomvalidationcallback) to allow calls without a trusted certificate:</span></span>

```csharp
var httpClientHandler = new HttpClientHandler();
// Return `true` to allow certificates that are untrusted/invalid
httpClientHandler.ServerCertificateCustomValidationCallback = 
    HttpClientHandler.DangerousAcceptAnyServerCertificateValidator;
var httpClient = new HttpClient(httpClientHandler);

var channel = GrpcChannel.ForAddress("https://localhost:5001",
    new GrpcChannelOptions { HttpClient = httpClient });
var client = new Greet.GreeterClient(channel);
```

> [!WARNING]
> <span data-ttu-id="a5995-126">Недоверенные сертификаты следует использовать только во время разработки приложения.</span><span class="sxs-lookup"><span data-stu-id="a5995-126">Untrusted certificates should only be used during app development.</span></span> <span data-ttu-id="a5995-127">В рабочих приложениях всегда следует использовать действительные сертификаты.</span><span class="sxs-lookup"><span data-stu-id="a5995-127">Production apps should always use valid certificates.</span></span>

## <a name="call-insecure-grpc-services-with-net-core-client"></a><span data-ttu-id="a5995-128">Вызов незащищенных служб gRPC Services с помощью клиента .NET Core</span><span class="sxs-lookup"><span data-stu-id="a5995-128">Call insecure gRPC services with .NET Core client</span></span>

<span data-ttu-id="a5995-129">Для вызова незащищенных служб gRPC с клиентом .NET Core требуется дополнительная настройка.</span><span class="sxs-lookup"><span data-stu-id="a5995-129">Additional configuration is required to call insecure gRPC services with the .NET Core client.</span></span> <span data-ttu-id="a5995-130">Клиент gRPC должен задать `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` для `true` параметра значение и использовать `http` в адресе сервера:</span><span class="sxs-lookup"><span data-stu-id="a5995-130">The gRPC client must set the `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` switch to `true` and use `http` in the server address:</span></span>

```csharp
// This switch must be set before creating the GrpcChannel/HttpClient.
AppContext.SetSwitch(
    "System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport", true);

// The port number(5000) must match the port of the gRPC server.
var channel = GrpcChannel.ForAddress("http://localhost:5000");
var client = new Greet.GreeterClient(channel);
```

## <a name="unable-to-start-aspnet-core-grpc-app-on-macos"></a><span data-ttu-id="a5995-131">Не удается запустить ASP.NET Core приложение gRPC на macOS</span><span class="sxs-lookup"><span data-stu-id="a5995-131">Unable to start ASP.NET Core gRPC app on macOS</span></span>

<span data-ttu-id="a5995-132">Kestrel не поддерживает HTTP/2 с TLS в macOS и более ранних версиях Windows, таких как Windows 7.</span><span class="sxs-lookup"><span data-stu-id="a5995-132">Kestrel doesn't support HTTP/2 with TLS on macOS and older Windows versions such as Windows 7.</span></span> <span data-ttu-id="a5995-133">Шаблон ASP.NET Core gRPC и примеры по умолчанию используют протокол TLS.</span><span class="sxs-lookup"><span data-stu-id="a5995-133">The ASP.NET Core gRPC template and samples use TLS by default.</span></span> <span data-ttu-id="a5995-134">При попытке запустить сервер gRPC появится следующее сообщение об ошибке:</span><span class="sxs-lookup"><span data-stu-id="a5995-134">You'll see the following error message when you attempt to start the gRPC server:</span></span>

> <span data-ttu-id="a5995-135">Не удалось выполнить привязку к https://localhost:5001 интерфейсу замыкания на себя IPv4: "HTTP/2 через TLS не поддерживается в macOS из-за отсутствия поддержки АЛПН".</span><span class="sxs-lookup"><span data-stu-id="a5995-135">Unable to bind to https://localhost:5001 on the IPv4 loopback interface: 'HTTP/2 over TLS is not supported on macOS due to missing ALPN support.'.</span></span>

<span data-ttu-id="a5995-136">Чтобы обойти эту ошибку, настройте Kestrel и клиент gRPC для использования HTTP/2 *без* TLS.</span><span class="sxs-lookup"><span data-stu-id="a5995-136">To work around this issue, configure Kestrel and the gRPC client to use HTTP/2 *without* TLS.</span></span> <span data-ttu-id="a5995-137">Это следует делать только во время разработки.</span><span class="sxs-lookup"><span data-stu-id="a5995-137">You should only do this during development.</span></span> <span data-ttu-id="a5995-138">Отсутствие использования TLS приведет к тому, что сообщения gRPC будут отправляться без шифрования.</span><span class="sxs-lookup"><span data-stu-id="a5995-138">Not using TLS will result in gRPC messages being sent without encryption.</span></span>

<span data-ttu-id="a5995-139">Kestrel должен настроить конечную точку HTTP/2 без TLS в *Program.CS*:</span><span class="sxs-lookup"><span data-stu-id="a5995-139">Kestrel must configure an HTTP/2 endpoint without TLS in *Program.cs*:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(options =>
            {
                // Setup a HTTP/2 endpoint without TLS.
                options.ListenLocalhost(5000, o => o.Protocols = 
                    HttpProtocols.Http2);
            });
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="a5995-140">Если конечная точка HTTP/2 настроена без TLS, [листеноптионс. Protocols](xref:fundamentals/servers/kestrel#listenoptionsprotocols) должна иметь значение `HttpProtocols.Http2`.</span><span class="sxs-lookup"><span data-stu-id="a5995-140">When an HTTP/2 endpoint is configured without TLS, the endpoint's [ListenOptions.Protocols](xref:fundamentals/servers/kestrel#listenoptionsprotocols) must be set to `HttpProtocols.Http2`.</span></span> <span data-ttu-id="a5995-141">`HttpProtocols.Http1AndHttp2`не может использоваться, так как протокол TLS требуется для согласования HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="a5995-141">`HttpProtocols.Http1AndHttp2` can't be used because TLS is required to negotiate HTTP/2.</span></span> <span data-ttu-id="a5995-142">Без TLS все соединения с конечной точкой по умолчанию заключаются в HTTP/1.1, а вызовы gRPC завершаются ошибкой.</span><span class="sxs-lookup"><span data-stu-id="a5995-142">Without TLS, all connections to the endpoint default to HTTP/1.1, and gRPC calls fail.</span></span>

<span data-ttu-id="a5995-143">Кроме того, клиент gRPC должен быть настроен так, чтобы не использовал TLS.</span><span class="sxs-lookup"><span data-stu-id="a5995-143">The gRPC client must also be configured to not use TLS.</span></span> <span data-ttu-id="a5995-144">Дополнительные сведения см. в статье [вызов незащищенных служб gRPC Services с помощью клиента .NET Core](#call-insecure-grpc-services-with-net-core-client).</span><span class="sxs-lookup"><span data-stu-id="a5995-144">For more information, see [Call insecure gRPC services with .NET Core client](#call-insecure-grpc-services-with-net-core-client).</span></span>

> [!WARNING]
> <span data-ttu-id="a5995-145">HTTP/2 без TLS следует использовать только во время разработки приложений.</span><span class="sxs-lookup"><span data-stu-id="a5995-145">HTTP/2 without TLS should only be used during app development.</span></span> <span data-ttu-id="a5995-146">В рабочих приложениях всегда следует использовать безопасность транспорта.</span><span class="sxs-lookup"><span data-stu-id="a5995-146">Production apps should always use transport security.</span></span> <span data-ttu-id="a5995-147">Дополнительные сведения см. [в разделе вопросы безопасности в gRPC for ASP.NET Core](xref:grpc/security#transport-security).</span><span class="sxs-lookup"><span data-stu-id="a5995-147">For more information, see [Security considerations in gRPC for ASP.NET Core](xref:grpc/security#transport-security).</span></span>

## <a name="grpc-c-assets-are-not-code-generated-from-proto-files"></a><span data-ttu-id="a5995-148">ресурсы C# gRPC не являются кодом, созданным из файлов с  *\*.*</span><span class="sxs-lookup"><span data-stu-id="a5995-148">gRPC C# assets are not code generated from *\*.proto* files</span></span>

<span data-ttu-id="a5995-149">Создание кода gRPC для конкретных клиентов и базовых классов служб требует наличия в проекте ссылок на файлы и инструментарий protobuf.</span><span class="sxs-lookup"><span data-stu-id="a5995-149">gRPC code generation of concrete clients and service base classes requires protobuf files and tooling to be referenced from a project.</span></span> <span data-ttu-id="a5995-150">Необходимо включить:</span><span class="sxs-lookup"><span data-stu-id="a5995-150">You must include:</span></span>

* <span data-ttu-id="a5995-151">*.* укажите файлы, которые необходимо использовать в `<Protobuf>` группе элементов.</span><span class="sxs-lookup"><span data-stu-id="a5995-151">*.proto* files you want to use in the `<Protobuf>` item group.</span></span> <span data-ttu-id="a5995-152">Проект должен ссылаться на [импортированные файлы ](https://developers.google.com/protocol-buffers/docs/proto3#importing-definitions) с указанными файлами.</span><span class="sxs-lookup"><span data-stu-id="a5995-152">[Imported *.proto* files](https://developers.google.com/protocol-buffers/docs/proto3#importing-definitions) must be referenced by the project.</span></span>
* <span data-ttu-id="a5995-153">Ссылка на пакет для пакета инструментов gRPC [gRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/).</span><span class="sxs-lookup"><span data-stu-id="a5995-153">Package reference to the gRPC tooling package [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/).</span></span>

<span data-ttu-id="a5995-154">Дополнительные сведения о создании ресурсов gRPC C# см. в <xref:grpc/basics>разделе.</span><span class="sxs-lookup"><span data-stu-id="a5995-154">For more information on generating gRPC C# assets, see <xref:grpc/basics>.</span></span>

<span data-ttu-id="a5995-155">По умолчанию `<Protobuf>` ссылка создает конкретный клиент и базовый класс службы.</span><span class="sxs-lookup"><span data-stu-id="a5995-155">By default, a `<Protobuf>` reference generates a concrete client and a service base class.</span></span> <span data-ttu-id="a5995-156">`GrpcServices` Атрибут элемента Reference можно использовать для ограничения C# создания ресурсов.</span><span class="sxs-lookup"><span data-stu-id="a5995-156">The reference element's `GrpcServices` attribute can be used to limit C# asset generation.</span></span> <span data-ttu-id="a5995-157">Допустимые `GrpcServices` параметры:</span><span class="sxs-lookup"><span data-stu-id="a5995-157">Valid `GrpcServices` options are:</span></span>

* <span data-ttu-id="a5995-158">`Both`(по умолчанию, если отсутствует)</span><span class="sxs-lookup"><span data-stu-id="a5995-158">`Both` (default when not present)</span></span>
* `Server`
* `Client`
* `None`

<span data-ttu-id="a5995-159">Для ASP.NET Core веб-приложения, в котором размещены службы gRPC Services, требуется только базовый класс службы:</span><span class="sxs-lookup"><span data-stu-id="a5995-159">An ASP.NET Core web app hosting gRPC services only needs the service base class generated:</span></span>

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" GrpcServices="Server" />
</ItemGroup>
```

<span data-ttu-id="a5995-160">Клиентское приложение gRPC, выполняющее вызовы gRPC, требует только создания конкретного клиента:</span><span class="sxs-lookup"><span data-stu-id="a5995-160">A gRPC client app making gRPC calls only needs the concrete client generated:</span></span>

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
</ItemGroup>
```
