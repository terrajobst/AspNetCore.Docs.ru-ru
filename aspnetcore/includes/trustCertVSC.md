Настройте доверие сертификату разработки HTTPS с помощью следующей команды:

```console
dotnet dev-certs https --trust
```

Приведенная выше команда отображает следующее диалоговое окно.

![Диалоговое окно "Предупреждение о безопасности"](~/getting-started/_static/cert.png)

Выберите **Да**, если согласны доверять сертификату разработки.

Дополнительные сведения см. в разделе [Настройка доверия к сертификату разработки HTTPS ASP.NET Core](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).