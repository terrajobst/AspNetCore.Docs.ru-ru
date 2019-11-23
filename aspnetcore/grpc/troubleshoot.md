---
title: Устранение неполадок gRPC в .NET Core
author: jamesnk
description: Устранение ошибок при использовании gRPC в .NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 10/16/2019
uid: grpc/troubleshoot
ms.openlocfilehash: c501cda14f3bac9297695ece59cbc4634e4b7895
ms.sourcegitcommit: e71b6a85b0e94a600af607107e298f932924c849
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/17/2019
ms.locfileid: "72519050"
---
# <a name="troubleshoot-grpc-on-net-core"></a><span data-ttu-id="f991a-103">Устранение неполадок gRPC в .NET Core</span><span class="sxs-lookup"><span data-stu-id="f991a-103">Troubleshoot gRPC on .NET Core</span></span>

<span data-ttu-id="f991a-104">[Джеймс Ньютона-короля](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="f991a-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

<span data-ttu-id="f991a-105">В этом документе обсуждаются часто встречающиеся проблемы при разработке gRPC приложений на .NET.</span><span class="sxs-lookup"><span data-stu-id="f991a-105">This document discusses commonly encountered problems when developing gRPC apps on .NET.</span></span>

## <a name="mismatch-between-client-and-service-ssltls-configuration"></a><span data-ttu-id="f991a-106">Несоответствие конфигурации SSL/TLS клиента и службы</span><span class="sxs-lookup"><span data-stu-id="f991a-106">Mismatch between client and service SSL/TLS configuration</span></span>

<span data-ttu-id="f991a-107">Шаблон gRPC и примеры используют [протокол TLS](https://tools.ietf.org/html/rfc5246) для защиты служб gRPC по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f991a-107">The gRPC template and samples use [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246) to secure gRPC services by default.</span></span> <span data-ttu-id="f991a-108">Клиенты gRPC должны использовать безопасное соединение для успешного вызова защищенных служб gRPC.</span><span class="sxs-lookup"><span data-stu-id="f991a-108">gRPC clients need to use a secure connection to call secured gRPC services successfully.</span></span>

<span data-ttu-id="f991a-109">Вы можете убедиться, что ASP.NET Core служба gRPC использует TLS в журналах, написанных при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="f991a-109">You can verify the ASP.NET Core gRPC service is using TLS in the logs written on app start.</span></span> <span data-ttu-id="f991a-110">Служба будет прослушивать конечную точку HTTPS:</span><span class="sxs-lookup"><span data-stu-id="f991a-110">The service will be listening on an HTTPS endpoint:</span></span>

```
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
```

<span data-ttu-id="f991a-111">Клиент .NET Core должен использовать `https` в адресе сервера для выполнения вызовов с защищенным соединением:</span><span class="sxs-lookup"><span data-stu-id="f991a-111">The .NET Core client must use `https` in the server address to make calls with a secured connection:</span></span>

```csharp
static async Task Main(string[] args)
{
    // The port number(5001) must match the port of the gRPC server.
    var channel = GrpcChannel.ForAddress("https://localhost:5001");
    var client = new Greet.GreeterClient(channel);
}
```

<span data-ttu-id="f991a-112">Все реализации клиента gRPC поддерживают протокол TLS.</span><span class="sxs-lookup"><span data-stu-id="f991a-112">All gRPC client implementations support TLS.</span></span> <span data-ttu-id="f991a-113">клиентам gRPC из других языков обычно требуется канал, настроенный с помощью `SslCredentials`.</span><span class="sxs-lookup"><span data-stu-id="f991a-113">gRPC clients from other languages typically require the channel configured with `SslCredentials`.</span></span> <span data-ttu-id="f991a-114">`SslCredentials` указывает сертификат, который будет использовать клиент, и его необходимо использовать вместо незащищенных учетных данных.</span><span class="sxs-lookup"><span data-stu-id="f991a-114">`SslCredentials` specifies the certificate that the client will use, and it must be used instead of insecure credentials.</span></span> <span data-ttu-id="f991a-115">Примеры настройки различных реализаций клиента gRPC для использования TLS см. в разделе [Проверка подлинности gRPC](https://www.grpc.io/docs/guides/auth/).</span><span class="sxs-lookup"><span data-stu-id="f991a-115">For examples of configuring the different gRPC client implementations to use TLS, see [gRPC Authentication](https://www.grpc.io/docs/guides/auth/).</span></span>

## <a name="call-a-grpc-service-with-an-untrustedinvalid-certificate"></a><span data-ttu-id="f991a-116">Вызов службы gRPC с недоверенным или недопустимым сертификатом</span><span class="sxs-lookup"><span data-stu-id="f991a-116">Call a gRPC service with an untrusted/invalid certificate</span></span>

<span data-ttu-id="f991a-117">Для клиента .NET gRPC служба должна иметь доверенный сертификат.</span><span class="sxs-lookup"><span data-stu-id="f991a-117">The .NET gRPC client requires the service to have a trusted certificate.</span></span> <span data-ttu-id="f991a-118">При вызове службы gRPC без доверенного сертификата возвращается следующее сообщение об ошибке:</span><span class="sxs-lookup"><span data-stu-id="f991a-118">The following error message is returned when calling a gRPC service without a trusted certificate:</span></span>

> <span data-ttu-id="f991a-119">Необработанное исключение.</span><span class="sxs-lookup"><span data-stu-id="f991a-119">Unhandled exception.</span></span> <span data-ttu-id="f991a-120">System .NET. http. HttpRequestException: не удалось установить SSL-подключение, см. внутреннее исключение.</span><span class="sxs-lookup"><span data-stu-id="f991a-120">System.Net.Http.HttpRequestException: The SSL connection could not be established, see inner exception.</span></span>
> <span data-ttu-id="f991a-121">---> System. Security. Authentication. исключение: удаленный сертификат является недопустимым согласно процедуре проверки.</span><span class="sxs-lookup"><span data-stu-id="f991a-121">---> System.Security.Authentication.AuthenticationException: The remote certificate is invalid according to the validation procedure.</span></span>

<span data-ttu-id="f991a-122">Эта ошибка может появиться, если вы тестируете приложение локально, а ASP.NET Core сертификат разработки HTTPS не является доверенным.</span><span class="sxs-lookup"><span data-stu-id="f991a-122">You may see this error if you are testing your app locally and the ASP.NET Core HTTPS development certificate is not trusted.</span></span> <span data-ttu-id="f991a-123">См. сведения об устранении этой проблемы в руководстве по [настройке доверия к сертификату разработки HTTPS ASP.NET Core в Windows и macOS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).</span><span class="sxs-lookup"><span data-stu-id="f991a-123">For instructions to fix this issue, see [Trust the ASP.NET Core HTTPS development certificate on Windows and macOS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).</span></span>

<span data-ttu-id="f991a-124">Если вы вызываете службу gRPC на другом компьютере и не можете доверять сертификату, то клиент gRPC можно настроить на игнорирование недействительного сертификата.</span><span class="sxs-lookup"><span data-stu-id="f991a-124">If you are calling a gRPC service on another machine and are unable to trust the certificate then the gRPC client can be configured to ignore the invalid certificate.</span></span> <span data-ttu-id="f991a-125">В следующем коде используется [HttpClientHandler. серверцертификатекустомвалидатионкаллбакк](/dotnet/api/system.net.http.httpclienthandler.servercertificatecustomvalidationcallback) , чтобы разрешить вызовы без доверенного сертификата:</span><span class="sxs-lookup"><span data-stu-id="f991a-125">The following code uses [HttpClientHandler.ServerCertificateCustomValidationCallback](/dotnet/api/system.net.http.httpclienthandler.servercertificatecustomvalidationcallback) to allow calls without a trusted certificate:</span></span>

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
> <span data-ttu-id="f991a-126">Недоверенные сертификаты следует использовать только во время разработки приложения.</span><span class="sxs-lookup"><span data-stu-id="f991a-126">Untrusted certificates should only be used during app development.</span></span> <span data-ttu-id="f991a-127">В рабочих приложениях всегда следует использовать действительные сертификаты.</span><span class="sxs-lookup"><span data-stu-id="f991a-127">Production apps should always use valid certificates.</span></span>

## <a name="call-insecure-grpc-services-with-net-core-client"></a><span data-ttu-id="f991a-128">Вызов незащищенных служб gRPC Services с помощью клиента .NET Core</span><span class="sxs-lookup"><span data-stu-id="f991a-128">Call insecure gRPC services with .NET Core client</span></span>

<span data-ttu-id="f991a-129">Для вызова незащищенных служб gRPC с клиентом .NET Core требуется дополнительная настройка.</span><span class="sxs-lookup"><span data-stu-id="f991a-129">Additional configuration is required to call insecure gRPC services with the .NET Core client.</span></span> <span data-ttu-id="f991a-130">Клиент gRPC должен задать для параметра `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` значение `true` и использовать `http` в адресе сервера:</span><span class="sxs-lookup"><span data-stu-id="f991a-130">The gRPC client must set the `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` switch to `true` and use `http` in the server address:</span></span>

```csharp
// This switch must be set before creating the GrpcChannel/HttpClient.
AppContext.SetSwitch(
    "System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport", true);

// The port number(5000) must match the port of the gRPC server.
var channel = GrpcChannel.ForAddress("http://localhost:5000");
var client = new Greet.GreeterClient(channel);
```

## <a name="unable-to-start-aspnet-core-grpc-app-on-macos"></a><span data-ttu-id="f991a-131">Не удается запустить ASP.NET Core приложение gRPC на macOS</span><span class="sxs-lookup"><span data-stu-id="f991a-131">Unable to start ASP.NET Core gRPC app on macOS</span></span>

<span data-ttu-id="f991a-132">Kestrel не поддерживает HTTP/2 с TLS в macOS и более ранних версиях Windows, таких как Windows 7.</span><span class="sxs-lookup"><span data-stu-id="f991a-132">Kestrel doesn't support HTTP/2 with TLS on macOS and older Windows versions such as Windows 7.</span></span> <span data-ttu-id="f991a-133">Шаблон ASP.NET Core gRPC и примеры по умолчанию используют протокол TLS.</span><span class="sxs-lookup"><span data-stu-id="f991a-133">The ASP.NET Core gRPC template and samples use TLS by default.</span></span> <span data-ttu-id="f991a-134">При попытке запустить сервер gRPC появится следующее сообщение об ошибке:</span><span class="sxs-lookup"><span data-stu-id="f991a-134">You'll see the following error message when you attempt to start the gRPC server:</span></span>

> <span data-ttu-id="f991a-135">Не удалось выполнить привязку к https://localhost:5001 в интерфейсе замыкания на себя IPv4: "HTTP/2 over TLS" не поддерживается в macOS из-за отсутствия поддержки АЛПН. ".</span><span class="sxs-lookup"><span data-stu-id="f991a-135">Unable to bind to https://localhost:5001 on the IPv4 loopback interface: 'HTTP/2 over TLS is not supported on macOS due to missing ALPN support.'.</span></span>

<span data-ttu-id="f991a-136">Чтобы обойти эту ошибку, настройте Kestrel и клиент gRPC для использования HTTP/2 *без* TLS.</span><span class="sxs-lookup"><span data-stu-id="f991a-136">To work around this issue, configure Kestrel and the gRPC client to use HTTP/2 *without* TLS.</span></span> <span data-ttu-id="f991a-137">Это следует делать только во время разработки.</span><span class="sxs-lookup"><span data-stu-id="f991a-137">You should only do this during development.</span></span> <span data-ttu-id="f991a-138">Отсутствие использования TLS приведет к тому, что сообщения gRPC будут отправляться без шифрования.</span><span class="sxs-lookup"><span data-stu-id="f991a-138">Not using TLS will result in gRPC messages being sent without encryption.</span></span>

<span data-ttu-id="f991a-139">Kestrel должен настроить конечную точку HTTP/2 без TLS в *Program.CS*:</span><span class="sxs-lookup"><span data-stu-id="f991a-139">Kestrel must configure an HTTP/2 endpoint without TLS in *Program.cs*:</span></span>

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

<span data-ttu-id="f991a-140">Если конечная точка HTTP/2 настроена без TLS, [листеноптионс. Protocols](xref:fundamentals/servers/kestrel#listenoptionsprotocols) для конечной точки должно быть установлено в значение `HttpProtocols.Http2`.</span><span class="sxs-lookup"><span data-stu-id="f991a-140">When an HTTP/2 endpoint is configured without TLS, the endpoint's [ListenOptions.Protocols](xref:fundamentals/servers/kestrel#listenoptionsprotocols) must be set to `HttpProtocols.Http2`.</span></span> <span data-ttu-id="f991a-141">`HttpProtocols.Http1AndHttp2` не может использоваться, так как протокол TLS требуется для согласования HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="f991a-141">`HttpProtocols.Http1AndHttp2` can't be used because TLS is required to negotiate HTTP/2.</span></span> <span data-ttu-id="f991a-142">Без TLS все соединения с конечной точкой по умолчанию заключаются в HTTP/1.1, а вызовы gRPC завершаются ошибкой.</span><span class="sxs-lookup"><span data-stu-id="f991a-142">Without TLS, all connections to the endpoint default to HTTP/1.1, and gRPC calls fail.</span></span>

<span data-ttu-id="f991a-143">Кроме того, клиент gRPC должен быть настроен так, чтобы не использовал TLS.</span><span class="sxs-lookup"><span data-stu-id="f991a-143">The gRPC client must also be configured to not use TLS.</span></span> <span data-ttu-id="f991a-144">Дополнительные сведения см. в статье [вызов незащищенных служб gRPC Services с помощью клиента .NET Core](#call-insecure-grpc-services-with-net-core-client).</span><span class="sxs-lookup"><span data-stu-id="f991a-144">For more information, see [Call insecure gRPC services with .NET Core client](#call-insecure-grpc-services-with-net-core-client).</span></span>

> [!WARNING]
> <span data-ttu-id="f991a-145">HTTP/2 без TLS следует использовать только во время разработки приложений.</span><span class="sxs-lookup"><span data-stu-id="f991a-145">HTTP/2 without TLS should only be used during app development.</span></span> <span data-ttu-id="f991a-146">В рабочих приложениях всегда следует использовать безопасность транспорта.</span><span class="sxs-lookup"><span data-stu-id="f991a-146">Production apps should always use transport security.</span></span> <span data-ttu-id="f991a-147">Дополнительные сведения см. [в разделе вопросы безопасности в gRPC for ASP.NET Core](xref:grpc/security#transport-security).</span><span class="sxs-lookup"><span data-stu-id="f991a-147">For more information, see [Security considerations in gRPC for ASP.NET Core](xref:grpc/security#transport-security).</span></span>

## <a name="grpc-c-assets-are-not-code-generated-from-proto-files"></a><span data-ttu-id="f991a-148">ресурсы C# gRPC не являются кодом, созданным из файлов с.</span><span class="sxs-lookup"><span data-stu-id="f991a-148">gRPC C# assets are not code generated from .proto files</span></span>

<span data-ttu-id="f991a-149">Создание кода gRPC для конкретных клиентов и базовых классов служб требует наличия в проекте ссылок на файлы и инструментарий protobuf.</span><span class="sxs-lookup"><span data-stu-id="f991a-149">gRPC code generation of concrete clients and service base classes requires protobuf files and tooling to be referenced from a project.</span></span> <span data-ttu-id="f991a-150">Необходимо включить:</span><span class="sxs-lookup"><span data-stu-id="f991a-150">You must include:</span></span>

* <span data-ttu-id="f991a-151">*.* укажите файлы, которые вы хотите использовать в группе элементов `<Protobuf>`.</span><span class="sxs-lookup"><span data-stu-id="f991a-151">*.proto* files you want to use in the `<Protobuf>` item group.</span></span> <span data-ttu-id="f991a-152">Проект должен ссылаться на [импортированные файлы ](https://developers.google.com/protocol-buffers/docs/proto3#importing-definitions) с указанными файлами.</span><span class="sxs-lookup"><span data-stu-id="f991a-152">[Imported *.proto* files](https://developers.google.com/protocol-buffers/docs/proto3#importing-definitions) must be referenced by the project.</span></span>
* <span data-ttu-id="f991a-153">Ссылка на пакет для пакета инструментов gRPC [gRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/).</span><span class="sxs-lookup"><span data-stu-id="f991a-153">Package reference to the gRPC tooling package [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/).</span></span>

<span data-ttu-id="f991a-154">Дополнительные сведения о создании gRPC C# ресурсов см. в разделе <xref:grpc/basics>.</span><span class="sxs-lookup"><span data-stu-id="f991a-154">For more information on generating gRPC C# assets, see <xref:grpc/basics>.</span></span>

<span data-ttu-id="f991a-155">По умолчанию `<Protobuf>` ссылка создает конкретный клиент и базовый класс службы.</span><span class="sxs-lookup"><span data-stu-id="f991a-155">By default, a `<Protobuf>` reference generates a concrete client and a service base class.</span></span> <span data-ttu-id="f991a-156">Атрибут `GrpcServices` элемента Reference можно использовать для ограничения C# создания ресурсов.</span><span class="sxs-lookup"><span data-stu-id="f991a-156">The reference element's `GrpcServices` attribute can be used to limit C# asset generation.</span></span> <span data-ttu-id="f991a-157">Допустимые параметры `GrpcServices`:</span><span class="sxs-lookup"><span data-stu-id="f991a-157">Valid `GrpcServices` options are:</span></span>

* <span data-ttu-id="f991a-158">`Both` (по умолчанию, если отсутствует)</span><span class="sxs-lookup"><span data-stu-id="f991a-158">`Both` (default when not present)</span></span>
* `Server`
* `Client`
* `None`

<span data-ttu-id="f991a-159">Для ASP.NET Core веб-приложения, в котором размещены службы gRPC Services, требуется только базовый класс службы:</span><span class="sxs-lookup"><span data-stu-id="f991a-159">An ASP.NET Core web app hosting gRPC services only needs the service base class generated:</span></span>

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" GrpcServices="Server" />
</ItemGroup>
```

<span data-ttu-id="f991a-160">Клиентское приложение gRPC, выполняющее вызовы gRPC, требует только создания конкретного клиента:</span><span class="sxs-lookup"><span data-stu-id="f991a-160">A gRPC client app making gRPC calls only needs the concrete client generated:</span></span>

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
</ItemGroup>
```

## <a name="wpf-projects-unable-to-generate-grpc-c-assets-from-proto-files"></a><span data-ttu-id="f991a-161">Проекты WPF не могут создавать ресурсы C# gRPC из файлов с.</span><span class="sxs-lookup"><span data-stu-id="f991a-161">WPF projects unable to generate gRPC C# assets from .proto files</span></span>

<span data-ttu-id="f991a-162">Проекты WPF имеют [известную ошибку](https://github.com/dotnet/wpf/issues/810) , препятствующую правильной работе создания кода gRPC.</span><span class="sxs-lookup"><span data-stu-id="f991a-162">WPF projects have a [known issue](https://github.com/dotnet/wpf/issues/810) that prevents gRPC code generation from working correctly.</span></span> <span data-ttu-id="f991a-163">Любые типы gRPC, созданные в проекте WPF с помощью ссылок на `Grpc.Tools` и *...* Files, будут создавать ошибки компиляции при использовании:</span><span class="sxs-lookup"><span data-stu-id="f991a-163">Any gRPC types generated in a WPF project by referencing `Grpc.Tools` and *.proto* files will create compilation errors when used:</span></span>

> <span data-ttu-id="f991a-164">ошибка CS0246: не удалось найти тип или имя пространства имен "Мигрпксервицес" (возможно, отсутствует директива using или ссылка на сборку?)</span><span class="sxs-lookup"><span data-stu-id="f991a-164">error CS0246: The type or namespace name 'MyGrpcServices' could not be found (are you missing a using directive or an assembly reference?)</span></span>

<span data-ttu-id="f991a-165">Эту проблему можно решить, выполнив следующие действия.</span><span class="sxs-lookup"><span data-stu-id="f991a-165">You can workaround this issue by:</span></span>

1. <span data-ttu-id="f991a-166">Создайте новый проект библиотеки классов .NET Core.</span><span class="sxs-lookup"><span data-stu-id="f991a-166">Create a new .NET Core class library project.</span></span>
2. <span data-ttu-id="f991a-167">В новом проекте добавьте ссылки, чтобы включить [ C# создание кода из файлов *\*.* Files](xref:grpc/basics#generated-c-assets):</span><span class="sxs-lookup"><span data-stu-id="f991a-167">In the new project, add references to enable [C# code generation from *\*.proto* files](xref:grpc/basics#generated-c-assets):</span></span>
    * <span data-ttu-id="f991a-168">Добавьте ссылку на пакет [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/).</span><span class="sxs-lookup"><span data-stu-id="f991a-168">Add a package reference to [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/) package.</span></span>
    * <span data-ttu-id="f991a-169">Добавьте файлы *\*.proto* в группу элементов `<Protobuf>`.</span><span class="sxs-lookup"><span data-stu-id="f991a-169">Add *\*.proto* files to the `<Protobuf>` item group.</span></span>
3. <span data-ttu-id="f991a-170">В приложении WPF добавьте ссылку на новый проект.</span><span class="sxs-lookup"><span data-stu-id="f991a-170">In the WPF application, add a reference to the new project.</span></span>

<span data-ttu-id="f991a-171">Приложение WPF может использовать созданные типы gRPC из нового проекта библиотеки классов.</span><span class="sxs-lookup"><span data-stu-id="f991a-171">The WPF application can use the gRPC generated types from the new class library project.</span></span>

[!INCLUDE[](~/includes/gRPCazure.md)]
