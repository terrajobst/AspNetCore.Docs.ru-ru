## <a name="multiple-authentication-providers"></a>Несколько поставщиков проверки подлинности

Если приложению требуется несколько поставщиков, объедините в цепочку методы расширения поставщика, реализующие [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication):

```csharp
services.AddAuthentication()
    .AddMicrosoftAccount(microsoftOptions => { ... })
    .AddGoogle(googleOptions => { ... })
    .AddTwitter(twitterOptions => { ... })
    .AddFacebook(facebookOptions => { ... });
```
