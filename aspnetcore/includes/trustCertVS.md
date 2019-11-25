::: moniker range=">= aspnetcore-3.0"
Visual Studio отображает следующее диалоговое окно.

![Этот проект настроен для использования SSL. Чтобы избежать предупреждений об SSL в браузере, вы можете сделать самозаверяющий сертификат, созданный ASP.NET Core, доверенным. Вы хотите сделать SSL-сертификат ASP.NET Core доверенным?](~/getting-started/_static/trustCert-3x.png)

Выберите **Да**, если вы делаете SSL-сертификат доверенным.

Отобразится следующее диалоговое окно.

![Диалоговое окно "Предупреждение о безопасности"](~/getting-started/_static/cert.png)

Выберите **Да**, если согласны доверять сертификату разработки.

Дополнительные сведения см. в разделе [Настройка доверия к сертификату разработки HTTPS ASP.NET Core](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).
::: moniker-end

::: moniker range="< aspnetcore-3.0"
Visual Studio отображает следующее диалоговое окно.

![Этот проект настроен для использования SSL. Чтобы избежать предупреждений о SSL в браузере, вы можете сделать самозаверяющий сертификат, созданный IIS Express, доверенным. Вы хотите сделать SSL-сертификат IIS Express доверенным?](~/getting-started/_static/trustCert.png)

Выберите **Да**, чтобы сделать SSL-сертификат IIS Express доверенным.

Отобразится следующее диалоговое окно.

![Диалоговое окно "Предупреждение о безопасности"](~/getting-started/_static/cert.png)

Выберите **Да**, если согласны доверять сертификату разработки.

Дополнительные сведения см. в разделе [Настройка доверия к сертификату разработки HTTPS ASP.NET Core](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).
::: moniker-end