# <a name="aspnet-core-authorization-sample"></a>Пример авторизации ASP.NET Core

В этом примере показано использование Razor Pages авторизации по соглашениям. В этом примере демонстрируются функции, описанные в разделе [соглашения об авторизации Razor Pages](https://docs.microsoft.com/aspnet/core/security/authorization/razor-pages-authorization) .

Авторизация пользователей в этом примере использует функции проверки подлинности файлов cookie, описанные в разделе [использование проверки подлинности файлов cookie без ASP.NET Core Identity](https://docs.microsoft.com/aspnet/core/security/authentication/cookie) . Понятия и примеры, приведенные в этом разделе, применяются одинаково к приложениям, использующим удостоверение ASP.NET Core. Сведения об использовании удостоверения ASP.NET Core см. в статье [Введение в удостоверение в ASP.NET Core](https://docs.microsoft.com/aspnet/core/security/authentication/identity).

Используйте адрес электронной почты **maria.rodriguez@contoso.com** для проверки подлинности пользователя с любым паролем. Пользователь прошел проверку подлинности в методе `AuthenticateUser` в файле *pages/Account/Login. cshtml. CS* . В реальном примере пользователь будет проходить проверку подлинности в базе данных.

## <a name="examples-in-this-sample"></a>Включенные примеры

| Компонент | Description |
| --- | --- |
| [аусоризепаже](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) | Добавляет [аусоризефилтер](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) на страницу с указанным путем. |
| [аусоризефолдер](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) | Добавляет [аусоризефилтер](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) ко всем страницам в папке с указанным путем. |
| [аллованонимаустопаже](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) | Добавляет [аллованонимаусфилтер](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) в страницу с указанным путем. |
| [аллованонимаустофолдер](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) | Добавляет [аллованонимаусфилтер](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) ко всем страницам в папке с указанным путем. |
