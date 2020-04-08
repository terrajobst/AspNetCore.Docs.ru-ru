## <a name="grpc-not-supported-on-azure-app-service"></a>Служба приложений Azure не поддерживает gRPC

> [!WARNING]
> [gRPC для ASP.NET Core ](xref:grpc/index) сейчас не поддерживается в Службе приложений Azure и служах IIS. Реализация HTTP.sys по протоколу HTTP/2 не поддерживает заголовки в конце HTTP-ответа, которые необходимы gRPC. См. дополнительные сведения на [сайте GitHub](https://github.com/dotnet/AspNetCore/issues/9020).
