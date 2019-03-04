<span data-ttu-id="36028-101">Настройте доверие сертификату разработки HTTPS с помощью следующей команды:</span><span class="sxs-lookup"><span data-stu-id="36028-101">Trust the HTTPS development certificate by running the following command:</span></span>

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="36028-102">Приведенная выше команда отображает следующее диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="36028-102">The preceding command displays the following dialog:</span></span>

![Диалоговое окно "Предупреждение о безопасности"](~/getting-started/_static/cert.png)

<span data-ttu-id="36028-104">Выберите **Да**, если согласны доверять сертификату разработки.</span><span class="sxs-lookup"><span data-stu-id="36028-104">Select **Yes** if you agree to trust the development certificate.</span></span>

<span data-ttu-id="36028-105">Дополнительные сведения см. в разделе [Настройка доверия к сертификату разработки HTTPS ASP.NET Core](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).</span><span class="sxs-lookup"><span data-stu-id="36028-105">See [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos) for more information.</span></span>