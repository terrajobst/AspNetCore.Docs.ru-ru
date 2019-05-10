# <a name="aspnet-core-authorization-sample"></a>Пример авторизации ASP.NET Core

В этом примере показано использование авторизации Razor Pages путем соглашений. В этом примере демонстрируются функции, описываемые в [соглашения об авторизации Razor Pages](https://docs.microsoft.com/aspnet/core/security/authorization/razor-pages-authorization) раздела.

Авторизация пользователя в этом примере используется файл cookie проверки подлинности, которые описываются в [использовать проверку подлинности файлов cookie без ASP.NET Core Identity](https://docs.microsoft.com/aspnet/core/security/authentication/cookie) раздела. Основные понятия и примеры, приведенные в этом разделе в равной мере применимы к приложениям, использующим удостоверения ASP.NET Core. Сведения об использовании удостоверения ASP.NET Core см. в разделе [Общие сведения об Identity в ASP.NET Core](https://docs.microsoft.com/aspnet/core/security/authentication/identity).

Используйте адрес электронной почты **maria.rodriguez@contoso.com** для проверки подлинности пользователя и любой другой пароль. Пользователь проходит проверку подлинности в `AuthenticateUser` метод в *Pages/Account/Login.cshtml.cs* файла. В реальный пример пользователь может пройти проверку подлинности в базе данных.

## <a name="examples-in-this-sample"></a>Включенные примеры

| Функция | Описание |
| --- | --- |
| [AuthorizePage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) | Добавляет [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) на страницу с указанным путем. |
| [AuthorizeFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) | Добавляет [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) ко всем страницам в папке по указанному пути. |
| [AllowAnonymousToPage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) | Добавляет [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) на страницу с указанным путем. |
| [AllowAnonymousToFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) | Добавляет [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) ко всем страницам в папке по указанному пути. |
