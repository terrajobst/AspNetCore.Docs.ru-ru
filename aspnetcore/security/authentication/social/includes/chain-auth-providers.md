## <a name="multiple-authentication-providers"></a><span data-ttu-id="65efd-101">Несколько поставщиков проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="65efd-101">Multiple authentication providers</span></span>

<span data-ttu-id="65efd-102">Если приложению требуется несколько поставщиков, объедините в цепочку методы расширения поставщика, реализующие [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication):</span><span class="sxs-lookup"><span data-stu-id="65efd-102">When the app requires multiple providers, chain the provider extension methods behind [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication):</span></span>

```csharp
services.AddAuthentication()
    .AddMicrosoftAccount(microsoftOptions => { ... })
    .AddGoogle(googleOptions => { ... })
    .AddTwitter(twitterOptions => { ... })
    .AddFacebook(facebookOptions => { ... });
```
