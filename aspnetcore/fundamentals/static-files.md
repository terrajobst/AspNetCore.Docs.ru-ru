---
title: "Работа с статических файлов в ASP.NET Core"
author: rick-anderson
description: "Работа с статические файлы"
keywords: "ASP.NET Core, статические файлы, статические активы, HTML, CSS, JavaScript"
ms.author: riande
manager: wpickett
ms.date: 4/07/2017
ms.topic: article
ms.assetid: e32245c7-4eee-4831-bd2e-915dbf9f5f70
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/static-files
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 11457cb8684e98147447303ae4653b74414a11fb
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-working-with-static-files-in-aspnet-core"></a>Введение в работу с статических файлов в ASP.NET Core

<a name=fundamentals-static-files></a>

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

Статические файлы, такие как HTML, CSS, JavaScript и изображение, активы, которые может обслуживать приложения ASP.NET Core непосредственно на клиентах.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/sample)

## <a name="serving-static-files"></a>Обработку статических файлов

Статические файлы обычно хранятся в `web root` (*\<содержимое корневой > / wwwroot*) папки. В разделе [содержимое корневого](xref:fundamentals/index#content-root) и [корневого веб-каталога](xref:fundamentals/index#web-root) для получения дополнительной информации. Обычно задать содержимое корневого для текущего каталога, чтобы ваш проект `web root` будет найден во время разработки.

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=5&start=12&end=22)]

Статические файлы могут храниться в любой папке под `web root` и доступ по относительному пути, корневой каталог. Например, при создании проект веб-приложения по умолчанию, с помощью Visual Studio существует несколько папок, созданных в *wwwroot* папку - *css*, *изображения*, и *js*. URI для доступа к изображению в *изображения* вложенную папку:

* `http://<app>/images/<imageFileName>`
* `http://localhost:9189/images/banner3.svg`

Чтобы статические файлы к обработке, необходимо настроить [по промежуточного слоя](middleware.md) добавление статических файлов в конвейер. По промежуточного слоя статических файлов можно настроить путем добавления зависимость на *Microsoft.AspNetCore.StaticFiles* пакета в проект и затем вызова `UseStaticFiles` метод расширения из `Startup.Configure`:

[!code-csharp[Main](../fundamentals/static-files/sample/StartupStaticFiles.cs?highlight=3&name=snippet1)]

`app.UseStaticFiles();`устанавливает файлы в `web root` (*wwwroot* по умолчанию) servable. Далее будет показано как сделать servable с другими содержимое каталога `UseStaticFiles`.

Необходимо включить в пакет NuGet «Microsoft.AspNetCore.StaticFiles».

> [!NOTE]
> `web root`по умолчанию используется значение *wwwroot* каталога, но можно задать `web root` каталог с `UseWebRoot`.

Предположим, что имеется иерархии проекта, где находятся статические файлы, которые вы хотите использовать за пределами `web root`. Пример:

* wwwroot
  * css
  * images
  * ...
* MyStaticFiles
  * Test.PNG

Для запроса на доступ к *test.png*, настройте по промежуточного слоя статических файлов следующим образом:

[!code-csharp[Main](../fundamentals/static-files/sample/StartupTwoStaticFiles.cs?highlight=5,6,7,8,9,10&name=snippet1)]

Запрос на `http://<app>/StaticFiles/test.png` будет обслуживать *test.png* файла.

`StaticFileOptions()`можно задать заголовки ответа. Например, приведенный ниже код задает обработку из статических файлов *wwwroot* папок и наборов `Cache-Control` заголовок, чтобы сделать их публично кэшируемый 10 минут (600 секунд):

[!code-csharp[Main](../fundamentals/static-files/sample/StartupAddHeader.cs?name=snippet1)]

![Отображение заголовка Cache-Control заголовки ответа были добавлены](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a>Авторизация статических файлов

Предоставляет статический файл модуля **не** проверки авторизации. Все файлы, обслуживаемых, включая те, в разделе *wwwroot* являются общедоступными. Чтобы обрабатывать файлы на основе авторизации:

* Сохраните их за пределами *wwwroot* и любой каталог, доступный для по промежуточного слоя статических файлов **и**

* Обслуживать их через действия контроллера, возвращая `FileResult` применении авторизации

## <a name="enabling-directory-browsing"></a>Включение просмотра каталогов

Просмотр каталогов позволяет пользователю просмотреть список каталогов и файлов в указанном каталоге веб-приложения. Обзор каталогов отключен по умолчанию, по соображениям безопасности (см. [вопросы](#considerations)). Чтобы включить просмотр каталогов, вызовите `UseDirectoryBrowser` метод расширения из `Startup.Configure`:

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet1)]

И добавить требуемые службы путем вызова `AddDirectoryBrowser` метод расширения из `Startup.ConfigureServices`:

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet2)]

Приведенный выше код позволяет просматривать каталог *wwwroot/images* папки с помощью URL-адрес http://\<приложения > / MyImages со ссылками на всех файлов и папок:

![Просмотр каталогов](static-files/_static/dir-browse.png)

В разделе [вопросы](#considerations) на угрозы безопасности при включении просмотра.

Обратите внимание на два `app.UseStaticFiles` вызовов. Первый необходимый для обслуживания CSS, изображения и JavaScript в *wwwroot* папки, а второй вызов для просмотра каталога *wwwroot/images* папки с помощью URL-адрес http://\<приложения > / MyImages:

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?highlight=3,5&name=snippet1)]

## <a name="serving-a-default-document"></a>Обслуживает документ по умолчанию

Установка домашней страницы по умолчанию предоставляет посетители сайта можно запустить при посещении веб-узла. Для веб-приложения для обслуживания страницы по умолчанию без участия пользователя полные URI, вызывать `UseDefaultFiles` метод расширения из `Startup.Configure` следующим образом.

[!code-csharp[Main](../fundamentals/static-files/sample/StartupEmpty.cs?highlight=3&name=snippet1)]

> [!NOTE]
> `UseDefaultFiles`должен быть вызван перед `UseStaticFiles` для обслуживания файла по умолчанию. `UseDefaultFiles`является повторной записи URL-адрес, который фактически не предоставлять этот файл. Необходимо включить по промежуточного слоя статических файлов (`UseStaticFiles`) для обслуживания файла.

С `UseDefaultFiles`, будет выполнен поиск запросы в папку:

* default.htm
* default.html
* index.htm
* index.html

Первый файл найден в списке будет предоставляться как в случае запроса полный URI (несмотря на то, что URL-адрес браузера будут отображать URI, запрошенный).

Ниже показано, как изменить имя файла по умолчанию для *mydefault.html*.

[!code-csharp[Main](static-files/sample/StartupDefault.cs?name=snippet1)]

## <a name="usefileserver"></a>UseFileServer

`UseFileServer`объединяет функциональные возможности `UseStaticFiles`, `UseDefaultFiles`, и `UseDirectoryBrowser`.

Следующий пример кода позволяет статические файлы и файла для обслуживания, по умолчанию, но не допускает просмотр каталогов:

```csharp
app.UseFileServer();
   ```

Следующий пример кода позволяет статических файлов по умолчанию и просмотр каталогов:

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
   ```

В разделе [вопросы](#considerations) на угрозы безопасности при включении просмотра. Как и в `UseStaticFiles`, `UseDefaultFiles`, и `UseDirectoryBrowser`, если вы хотите предоставлять файлы, которые существуют за пределами `web root`, создать и настроить `FileServerOptions` объекта, передаваемого как параметр `UseFileServer`. Например имеется следующая иерархия каталогов в веб-приложения:

* wwwroot

  * css

  * images

  * ...

* MyStaticFiles

  * Test.PNG

  * default.html

Использовать приведенный выше пример иерархии, может потребоваться включить статические файлы, файлы по умолчанию и поиске `MyStaticFiles` каталога. В следующем фрагменте кода, осуществляются с помощью одного вызова `FileServerOptions`.

[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?highlight=5,6,7,8,9,10,11&name=snippet1)]

Если `enableDirectoryBrowsing` равно `true` требуется вызывать `AddDirectoryBrowser` метод расширения из `Startup.ConfigureServices`:

[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?name=snippet2)]

С помощью иерархии файлов и приведенный выше код:

| URI            |                             Ответ  |
| ------- | ------|
| `http://<app>/StaticFiles/test.png`    |      MyStaticFiles/test.png |
| `http://<app>/StaticFiles`              |     MyStaticFiles/default.html |

Если нет значения по умолчанию с именем файлы находятся в *MyStaticFiles* каталог, http://\<приложения > / StaticFiles возвращает каталога с интерактивными ссылками:

![Список статических файлов](static-files/_static/db2.PNG)

> [!NOTE]
> `UseDefaultFiles`и `UseDirectoryBrowser` будет иметь URL-адрес http://\<приложения > / StaticFiles без завершающей косой черты и причина стороны клиента перенаправления http://\<приложения > /StaticFiles/ (Добавление завершающей косой черты). Без конечные косая черта относительные URL-адреса в документах будет неверным.

## <a name="fileextensioncontenttypeprovider"></a>FileExtensionContentTypeProvider

`FileExtensionContentTypeProvider` Класс содержит коллекцию, которая сопоставляет расширения файлов для типов содержимого MIME. В следующем образце зарегистрированных несколько расширений файлов для известных типов MIME, заменяется «.rtf» и «.mp4» удаляется.

[!code-csharp[Main](../fundamentals/static-files/sample/StartupFileExtensionContentTypeProvider.cs?highlight=3,4,5,6,7,8,9,10,11,12,19&name=snippet1)]

В разделе [типы содержимого MIME](http://www.iana.org/assignments/media-types/media-types.xhtml).

## <a name="non-standard-content-types"></a>Нестандартные типы содержимого

По промежуточного слоя статических файлов ASP.NET понимает почти 400 типы содержимого файлов. Если пользователь запрашивает файл неизвестного типа, по промежуточного слоя статических файлов возвращает ответ HTTP 404 (не найдено). Если просмотр каталогов включен, будет отображаться ссылка на файл, но URI будет возвращать ошибку HTTP 404.

Следующий код позволяет обслуживает неизвестные типы и сделает Неизвестный файл как изображение.

[!code-csharp[Main](static-files/sample/StartupServeUnknownFileTypes.cs?name=snippet1)]

С приведенный выше код запрашивает файл с Неизвестный тип содержимого будет возвращаться в виде изображения.

>[!WARNING]
> Включение `ServeUnknownFileTypes` представляет риск для безопасности и использовать его не рекомендуется.  `FileExtensionContentTypeProvider`(описано выше) предоставляет более безопасная альтернатива обслужить файлы с нестандартные расширения.

### <a name="considerations"></a>Особенности

>[!WARNING]
> `UseDirectoryBrowser`и `UseStaticFiles` может вызвать утечку секретные данные. Рекомендуется, чтобы вы **не** directory Включение просмотра в рабочей среде. Будьте внимательны о какие каталоги включения с `UseStaticFiles` или `UseDirectoryBrowser` как весь каталог и все вложенные каталоги будут доступны. Рекомендуется оставлять открытый содержимое в своем собственном каталоге например * \<содержимое корневого > / wwwroot*, от представления приложений, файлы конфигурации и т. д.

* URL-адреса для содержимого, представлены `UseDirectoryBrowser` и `UseStaticFiles` чувствительность к регистру и ограничения на символы их базовой файловой системы. Например Windows не учитывает регистр, но Mac и Linux не.

* Приложения ASP.NET Core, размещенные в IIS использовать модуль ASP.NET Core для перенаправления всех запросов приложения, включая запросы статических файлов. Обработчик файла статистики IIS не используется, поскольку он не будет возможность обработки запросов, прежде чем они будут обработаны с модуль ASP.NET Core.

* Чтобы удалить обработчик файла статистики IIS (на уровне сервера или веб-сайт):

     * Перейдите к **модули** функции

     * Выберите **StaticFileModule** в списке

     * Коснитесь **удалить** в **действия** боковой панели

>[!WARNING]
> Если обработчик файла статистики IIS включена **и** модуля ASP.NET Core (ANCM) настроен неправильно (например если *web.config* не было развернуто), невозможно предоставить статические файлы.

* Файлы кода (включая c# и Razor) должны располагаться за пределами проекта приложения `web root` (*wwwroot* по умолчанию). При этом создается четкое разделение стороны содержимое своего приложения клиента и сервера стороны исходный код, который предотвращает утечку код со стороны сервера.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [ПО промежуточного слоя](middleware.md)

* [Введение в ASP.NET Core](../index.md)
