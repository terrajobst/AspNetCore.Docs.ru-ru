# <a name="aspnet-core-authorization-sample"></a>Образец авторизации ASP.NET Core

В этом примере описывается использование страниц Razor авторизации обозначениями. В этом примере демонстрируется функции, описанные в [страниц Razor авторизации соглашения](https://docs.microsoft.com/aspnet/core/security/authorization/razor-pages-authorization) раздела.

Авторизация пользователя в этом образце используется cookie проверки подлинности, которые описываются в [cookie использовать проверку подлинности без ASP.NET Core Identity](https://docs.microsoft.com/aspnet/core/security/authentication/cookie) раздела. Сведения об использовании ASP.NET Core Identity см. в разделах в [проверки подлинности](https://docs.microsoft.com/aspnet/core/security/authentication/index) раздел документации.

При выполнении этого образца, использовать адрес электронной почты **maria.rodriguez@contoso.com** для проверки подлинности пользователя.

## <a name="examples-in-this-sample"></a>Включенные примеры

|                                                                              Функция                                                                               |                                                                                        Описание                                                                                         |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|          [AuthorizePage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage)          |                Добавляет [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) на страницу с указанным путем.                |
|        [AuthorizeFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder)        |      Добавляет [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) на все страницы в папку по указанному пути.      |
|   [AllowAnonymousToPage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage)   |            Добавляет [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) на страницу с указанным путем.            |
| [AllowAnonymousToFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) | Добавляет [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) на все страницы в папку по указанному пути. |

