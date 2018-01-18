---
title: "Работа с файлами статического в ASP.NET Core"
author: rick-anderson
description: "Узнайте, как для обслуживания и защитить статические файлы и настройки размещения веб-приложение ASP.NET Core поведением по промежуточного слоя статических файлов."
keywords: "ASP.NET Core, статические файлы, статические активы, HTML, CSS, JavaScript"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/18/2018
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/static-files
ms.openlocfilehash: 912923860939a1d1dd91ccc79862e23f9095d161
ms.sourcegitcommit: a3e88639a6bcf8fb4d634036dac93130c464a097
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/18/2018
---
# <a name="work-with-static-files-in-aspnet-core"></a>Работа с файлами статического в ASP.NET Core

По [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Скотт Addie](https://twitter.com/Scott_Addie)

Статические файлы, например HTML, CSS, изображения и JavaScript, являются активы приложения ASP.NET Core предоставляет клиентам напрямую. Некоторые Настройка не требуется, чтобы включить для обслуживания этих файлов.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="serve-static-files"></a>Обрабатывать статические файлы

Статические файлы хранятся в корневом каталоге проекта web. Каталог по умолчанию —  *\<content_root > / wwwroot*, но его можно изменить через [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) метод. В разделе [содержимое корневого](xref:fundamentals/index#content-root) и [корневого веб-каталога](xref:fundamentals/index#web-root) для получения дополнительной информации.

Приложения веб-узел должен быть в курсе содержимого корневого каталога.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

`WebHost.CreateDefaultBuilder` Метод задает содержимое корневого в текущем каталоге:

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Задайте корневой содержимого в текущем каталоге путем вызова [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) внутри `Program.Main`:

[!code-csharp[](static-files/samples/1x/Program.cs?name=snippet_ProgramClass&highlight=7)]

---

Статические файлы будут доступны с помощью путь относительно корневого веб-каталога. Например **веб-приложение** шаблон проекта содержит несколько папок в *wwwroot* папки:

* **wwwroot**
  * **css**
  * **images**
  * **js**

Для доступа к файлу в формате URI *изображения* вложенной *http://\<server_address > /images/\<image_file_name >*. Например *http://localhost:9189/images/banner3.svg*.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Если для различных версий платформы .NET Framework, добавьте [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) пакета в проект. Если код предназначен для .NET Core [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) включает этот пакет.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Добавить [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) пакета в проект.

---

Настройка [по промежуточного слоя](xref:fundamentals/middleware) позволяющее обслуживанием статических файлов.

### <a name="serve-files-inside-of-web-root"></a>Обрабатывать файлы внутри корневого веб-каталога

Вызвать [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) метода в `Startup.Configure`:

[!code-csharp[](static-files/samples/1x/StartupStaticFiles.cs?name=snippet_ConfigureMethod&highlight=3)]

Без параметров `UseStaticFiles` перегруженный метод помечает файлы в корневой веб как servable. Следующие ссылки разметки *wwwroot/images/banner1.svg*:

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_wwwroot)]

### <a name="serve-files-outside-of-web-root"></a>Обрабатывать файлы за пределами корневого веб-каталога

Рассмотрим Иерархия каталогов, в которой статические файлы обслуживать находятся за пределами корневого веб.

* **wwwroot**
  * **css**
  * **images**
  * **js**
* **MyStaticFiles**
  * **images**
      * *banner1.svg*

Запрос можно получить доступ к *banner1.svg* файла путем настройки по промежуточного слоя статических файлов следующим образом:

[!code-csharp[](static-files/samples/1x/StartupTwoStaticFiles.cs?name=snippet_ConfigureMethod&highlight=5-10)]

В приведенном выше коде *MyStaticFiles* Иерархия каталогов выполняется через публично *StaticFiles* сегмент URI. Запрос на *http://\<server_address > /StaticFiles/images/banner1.svg* служит *banner1.svg* файла.

Следующие ссылки разметки *MyStaticFiles/images/banner1.svg*:

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_outside)]

### <a name="set-http-response-headers"></a>Задать заголовки ответов HTTP

Объект [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) объект может использоваться для задания заголовков HTTP-ответов. Кроме настройки обслуживание статических файлов от корня веб-, следующий код задает `Cache-Control` заголовка:

[!code-csharp[](static-files/samples/1x/StartupAddHeader.cs?name=snippet_ConfigureMethod)]

[HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) существует метод [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) пакета.

Файлы были сделаны публично кэшируемый 10 минут (600 секунд):

![Отображение заголовка Cache-Control заголовки ответа были добавлены](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a>Авторизация статических файлов

По промежуточного слоя статических файлов не предоставляет возможность проверки авторизации. Все файлы, обслуживаемых, включая те, в разделе *wwwroot*, являются открытыми. Чтобы обрабатывать файлы на основе авторизации:

* Сохраните их за пределами *wwwroot* и любой каталог, доступный для по промежуточного слоя статических файлов **и**
* Обслуживает их через метод действия, к которому применяется авторизации. Вернуть [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) объекта:

[!code-csharp[](static-files/samples/1x/Controllers/HomeController.cs?name=snippet_BannerImageAction)]

## <a name="enable-directory-browsing"></a>Включение просмотра каталогов

Просмотр каталогов позволяет пользователям веб-приложения см. список каталогов и файлов в указанном каталоге. Обзор каталогов отключен по умолчанию, по соображениям безопасности (см. [вопросы](#considerations)). Просмотр с помощью вызова каталогов включения [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) метод в `Startup.Configure`:

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=12-17)]

Добавить требуемые службы путем вызова [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) метод `Startup.ConfigureServices`:

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureServicesMethod&highlight=3)]

Предыдущий код позволяет просматривать каталог *wwwroot/images* папки с помощью URL-адрес *http://\<server_address > / MyImages*, со ссылками на всех файлов и папок:

![Просмотр каталогов](static-files/_static/dir-browse.png)

В разделе [вопросы](#considerations) на угрозы безопасности при включении просмотра.

Обратите внимание на два `UseStaticFiles` вызывает в следующем примере. Первый вызов включает обслуживанием статических файлов в *wwwroot* папки. Второй вызов включает просмотр каталогов из *wwwroot/images* папки с помощью URL-адрес *http://\<server_address > / MyImages*:

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=3,5)]

## <a name="serve-a-default-document"></a>Обслуживать документ по умолчанию

Настройка домашней страницы по умолчанию служит посетители логических отправной точкой при посещении веб-узла. Чтобы обслуживать страницы по умолчанию без полного указания имен URI пользователь, вызовите [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) метод `Startup.Configure`:

[!code-csharp[](static-files/samples/1x/StartupEmpty.cs?name=snippet_ConfigureMethod&highlight=3)]

> [!IMPORTANT]
> `UseDefaultFiles`должен быть вызван перед `UseStaticFiles` для обслуживания файла по умолчанию. `UseDefaultFiles`является перезаписи URL-адрес, который фактически не использовать файл. Включение по промежуточного слоя статических файлов через `UseStaticFiles` для обслуживания файла.

С `UseDefaultFiles`, запросы для поиска папки:

* *default.htm*
* *default.html*
* *index.htm*
* *index.html*

Первый файл найден в списке обслуживается, как будто полный URI запроса. URL-адрес браузера продолжает отражают запрошенного URI.

Следующий код позволяет изменить имя файла по умолчанию для *mydefault.html*:

[!code-csharp[](static-files/samples/1x/StartupDefault.cs?name=snippet_ConfigureMethod)]

## <a name="usefileserver"></a>UseFileServer

[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) объединяет функциональные возможности `UseStaticFiles`, `UseDefaultFiles`, и `UseDirectoryBrowser`.

Следующий пример кода позволяет обслуживанием статические файлы и файл по умолчанию. Просмотр каталогов не включена.

```csharp
app.UseFileServer();
```

Следующий код основан перегрузки без параметров, позволяя просмотр каталогов:

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
```

Примите во внимание следующие иерархии каталога.

* **wwwroot**
  * **css**
  * **images**
  * **js**
* **MyStaticFiles**
  * **images**
      * *banner1.svg*
  * *default.html*

Следующий пример кода позволяет статические файлы, файлы по умолчанию и просмотр каталога из `MyStaticFiles`:

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureMethod&highlight=5-11)]

`AddDirectoryBrowser`должно вызываться при `EnableDirectoryBrowsing` значение свойства `true`:

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureServicesMethod)]

С помощью иерархии файлов и предшествующий код, URL-адреса устранить следующим образом:

| URI            |                             Ответ  |
| ------- | ------|
| *http://\<server_address>/StaticFiles/images/banner1.svg*    |      MyStaticFiles/images/banner1.svg |
| *http://\<server_address>/StaticFiles*             |     MyStaticFiles/default.html |

Если нет файла с именем по умолчанию в *MyStaticFiles* каталога, *http://\<server_address > / StaticFiles* возвращает каталога с интерактивными ссылками:

![Список статических файлов](static-files/_static/db2.png)

> [!NOTE]
> `UseDefaultFiles`и `UseDirectoryBrowser` URL-адрес *http://\<server_address > / StaticFiles* без завершающей косой черты для запуска на стороне клиента перенаправления *http://\<server_address > / StaticFiles /*. Обратите внимание на добавленную конечную косую черту. Относительные URL-адреса в документах, считаются недопустимый без косую черту в конце.

## <a name="fileextensioncontenttypeprovider"></a>FileExtensionContentTypeProvider

[FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) класс содержит `Mappings` свойство, служащее сопоставление расширений файлов для типов содержимого MIME. В следующем примере несколько расширений файлов, регистрируются в известные типы MIME. *.Rtf* заменяется расширения, и *.mp4* удаляется.

[!code-csharp[](static-files/samples/1x/StartupFileExtensionContentTypeProvider.cs?name=snippet_ConfigureMethod&highlight=3-12,19)]

В разделе [типы содержимого MIME](http://www.iana.org/assignments/media-types/media-types.xhtml).

## <a name="non-standard-content-types"></a>Нестандартные типы содержимого

По промежуточного слоя статических файлов, которые понимает почти 400 типы содержимого файлов. Если пользователь запрашивает файл неизвестного типа, по промежуточного слоя статических файлов возвращает ответ HTTP 404 (не найдено). Если просмотр каталогов включен, отображается ссылка на файл. URI возвращает ошибку HTTP 404.

Следующий код позволяет обслуживает неизвестные типы, а также преобразование Неизвестный файл как изображение:

[!code-csharp[](static-files/samples/1x/StartupServeUnknownFileTypes.cs?name=snippet_ConfigureMethod)]

В вышеописанном примере кода запрос файл неизвестного типа содержимого возвращается в виде изображения.

> [!WARNING]
> Включение [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) представляет угрозу безопасности. По умолчанию оно отключено, и его использование не рекомендуется. [FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) предоставляет более безопасная альтернатива обслужить файлы с нестандартные расширения.

### <a name="considerations"></a>Особенности

> [!WARNING]
> `UseDirectoryBrowser`и `UseStaticFiles` может вызвать утечку секретные данные. Настоятельно рекомендуется отключить просмотр в рабочей среде каталогов. Внимательно просмотрите, какие каталоги включаются посредством `UseStaticFiles` или `UseDirectoryBrowser`. Весь каталог и его подкаталогах становятся доступен из Интернета. Хранилище файлов подходит для выступают в роли Public в выделенный каталог, например  *\<content_root > / wwwroot*. Эти файлы отдельные представления MVC, страниц Razor (только 2.x), файлы конфигурации и т. д.

* URL-адреса для содержимого, представлены `UseDirectoryBrowser` и `UseStaticFiles` чувствительность к регистру и ограничения на символы базовой файловой системы. Например, без учета регистра Windows&mdash;не Mac и Linux.

* Приложения ASP.NET Core, размещенные в IIS, воспользуйтесь [модуля ASP.NET Core (ANCM)](xref:fundamentals/servers/aspnet-core-module) для перенаправления всех запросов приложений, включая запросы статических файлов. Обработчик файла статистики IIS не используется. Он имеет не может обработать запросы, прежде чем вы обрабатываются ANCM.

* Выполните следующие действия в диспетчере служб IIS для удаления обработчику файла статистики на уровне сервера или веб-сайт IIS.
    1. Перейдите к **модули** компонентов.
    1. Выберите **StaticFileModule** в списке.
    1. Нажмите кнопку **удалить** в **действия** боковой панели.

> [!WARNING]
> Если обработчик файла статистики IIS включена **и** ANCM настроен неправильно, обслуживаются статических файлов. Это происходит, например, если *web.config* файл не развернут.

* Поместите файлы кода (включая *.cs* и *.cshtml*) за пределами корневого веб-каталога проекта приложения. Логическое разделение таким образом создается между содержимым клиентского и серверного кода приложения. Это предотвращает утечку серверный код.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [ПО промежуточного слоя](xref:fundamentals/middleware)

* [Введение в ASP.NET Core](xref:index)