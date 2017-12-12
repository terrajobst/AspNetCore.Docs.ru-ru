---
uid: web-pages/overview/api-reference/asp-net-web-pages-api-reference
title: "Веб-страницы ASP.NET (Razor) краткий справочник по API | Документы Microsoft"
author: tfitzmac
description: "Эта страница содержит список, содержащий краткие примеры наиболее часто используемые объекты, свойства и методы для программирования веб-страниц ASP.NET с синтаксисом Razor."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: 4001cb9b-3bfd-4ace-8a89-1561d8421e2c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/api-reference/asp-net-web-pages-api-reference
msc.type: authoredcontent
ms.openlocfilehash: 35f91f4dbea4881d9dabc4ab7c6b96dbb6a01ea2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-pages-razor-api-quick-reference"></a>Веб-страницы ASP.NET (Razor) краткий справочник по API
====================
по [Tom FitzMacken](https://github.com/tfitzmac)

> Эта страница содержит список, содержащий краткие примеры наиболее часто используемые объекты, свойства и методы для программирования веб-страниц ASP.NET с синтаксисом Razor.
> 
> Описания «(v2)», отмеченные появились в веб-страниц ASP.NET версии 2.
> 
> Справочная документация по API, в разделе [справочная документация веб-страниц ASP.NET](https://go.microsoft.com/fwlink/?LinkId=208659) на сайте MSDN.
> 
> ## <a name="software-versions"></a>Версии программного обеспечения
> 
> 
> - Веб-страниц ASP.NET (Razor) 3
>   
> 
> Этот учебник также работает с 2 веб-страниц ASP.NET и веб-страниц ASP.NET 1.0 (за исключением функций, отмеченных как v2).


Эта страница содержит сведения о следующих:

- [Классы](#Classes)
- [Данные](#Data)
- [Вспомогательные функции](#Helpers)
- [Проверка](#Validation)

<a id="Classes"></a>
## <a name="classes"></a>Классы

### `AppState[key], AppState[index],App`

Содержит данные, которые могут совместно использоваться все страницы в приложении. Можно использовать динамическую `App` свойство для доступа к данным, как показано в следующем примере:

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample1.html)]

### `AsBool(), AsBool(true|false)`

Преобразует строковое значение в логическое значение (ИСТИНА или ЛОЖЬ). Возвращает значение false или указанное значение, если строка не представляет значение true или false.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample2.cs)]

### `AsDateTime(), AsDateTime(value)`

Преобразует строковое значение для даты и времени. Возвращает `DateTime.MinValue` или указанное значение, если строка не представляет даты и времени.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample3.cs)]

### `AsDecimal(), AsDecimal(value)`

Преобразует строковое значение в десятичное значение. Возвращает 0,0 или указанное значение, если строка не представляет значение десятичного числа.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample4.cs)]

### `AsFloat(), AsFloat(value)`

Преобразует строковое значение в значение с плавающей запятой. Возвращает 0,0 или указанное значение, если строка не представляет значение десятичного числа.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample5.cs)]

### `AsInt(), AsInt(value)`

Преобразует строковое значение в целое число. Возвращает значение 0 или указанное значение, если строка не представляет целое число.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample6.cs)]

### `Href(path [, param1 [, param2]])`

Создает URL-адрес веб обозревателем из путь к локальному файлу с частями необязательный дополнительный путь.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample7.cshtml)]

### `Html.Raw(value)`

Подготавливает к просмотру *значение* как HTML-разметка, вместо отображения его выходные данные в кодировке HTML.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample8.cshtml)]

### `IsBool(), IsDateTime(), IsDecimal(), IsFloat(), IsInt()`

Возвращает значение true, если значение может быть преобразовано из строки в указанный тип.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample9.cs)]

### `IsEmpty()`

Возвращает значение true, если объект или переменная не имеет значения.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample10.cs)]

### `IsPost`

Возвращает значение true, если запрос POST. (Начальных запросов обычно являются GET).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample11.cs)]

### `Layout`

Задает путь страницы макета для применения к этой странице.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample12.html)]

### `PageData[key], PageData[index],Page`

Содержит данные, общим для страниц, страниц макетов и частичных страниц в текущий запрос. Можно использовать динамическую `Page` свойство для доступа к данным, как показано в следующем примере:

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample13.html)]

### `RenderBody()`

(Макет страницы) Отображает содержимое страницы содержимого, не все именованные разделы.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample14.cs)]

### `RenderPage(path, values)`  
`RenderPage(path[,param1 [, param2]])`

Отображает страницу содержимого, используя заданный путь и необязательные дополнительные данные. Можно получить значения дополнительных параметров из `PageData` по положению (например, 1) или ключ (например, 2).

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample15.js)]

### `RenderSection(sectionName [, required = true|false])`

(Макет страницы) Отображает раздел содержимого с именем. Задать *необходимые* значение false, чтобы сделать необязательный раздел.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample16.js)]

### `Request.Cookies[key]`

Возвращает или задает значение файла cookie HTTP.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample17.cs)]

### `Request.Files[key]`

Получает файлы, которые были загружены в текущий запрос.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample18.js)]

### `Request.Form[key]`

Возвращает данные, была отправлена в форме (в виде строки). `Request[key]`проверяет оба `Request.Form` и `Request.QueryString` коллекции.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample19.cs)]

### `Request.QueryString[key]`

Возвращает данные, который указан в строке запроса URL-адреса. `Request[key]`проверяет оба `Request.Form` и `Request.QueryString` коллекции.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample20.cs)]

### `Request.Unvalidated(key)`  
`Request.Unvalidated().QueryString|Form|Cookies|Headers[key]`

Выборочно отключает запрос проверки для элементов формы, значения строки запроса, файл cookie или значение заголовка. Проверка запросов включена по умолчанию и пользователи не смогут учета разметки или других потенциально опасное содержимое.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample21.cs)]

### `Response.AddHeader(name, value)`

Добавляет заголовок НТТР сервер в ответ.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample22.cs)]

### `Response.OutputCache(seconds [, sliding] [, varyByParams])`

Кэширование вывода страниц в указанное время. При необходимости установить *скользящий* Сброс времени ожидания для доступа к каждой странице и *varyByParams* кэширование разные версии страницы для каждой строки запроса, различающимся в запросе страницы.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample23.js)]

### `Response.Redirect(path)`

Перенаправляет запрос браузера в новое расположение.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample24.js)]

### `Response.SetStatus(httpStatusCode)`

Задает код состояния HTTP, отправляемого в браузер.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample25.cs)]

### `Response.WriteBinary(data [, mimetype])`

Записывает содержимое *данные* ответ с типом MIME, необязательно.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample26.js)]

### `Response.WriteFile(file)`

Записывает содержимое файла в ответ.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample27.cs)]

### `@section(sectionName) {content }`

(Макет страницы) Определяет, имя раздела содержимого.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample28.cshtml)]

### `Server.HtmlDecode(htmlText)`

Декодирует строку, — в кодировке HTML.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample29.cs)]

### `Server.HtmlEncode(text)`

Кодирует строку для подготовки к просмотру в HTML-разметку.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample30.cs)]

### `Server.MapPath(virtualPath)`

Возвращает физический путь к серверу для заданного виртуального пути.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample31.cs)]

### `Server.UrlDecode(urlText)`

Расшифровывает текст с URL-адреса.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample32.cs)]

### `Server.UrlEncode(text)`

Кодирует текст для размещения в URL-АДРЕСЕ.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample33.cs)]

### `Session[key]`

Возвращает или задает значение, которое существует, пока пользователь не закроет браузер.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample34.css)]

### `ToString()`

Отображает строковое представление значения объекта.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample35.html)]

### `UrlData[index]`

Возвращает дополнительные сведения по URL-адресу (например, */MyPage/ExtraData*).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample36.cs)]

### `WebSecurity.ChangePassword(userName,currentPassword,newPassword)`

Изменяет пароль для указанного пользователя.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample37.cs)]

### `WebSecurity.ConfirmAccount(accountConfirmationToken)`

Подтверждение учетной записи с помощью токена подтверждения учетной записи.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample38.cs)]

### `WebSecurity.CreateAccount(userName, password`  
 `[, requireConfirmationToken = true|false])`

Создает новую учетную запись пользователя с указанным именем пользователя и паролем. Если требуется маркер подтверждения, передайте значение true, если для *requireConfirmationToken.*

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample39.cs)]

### `WebSecurity.CurrentUserId`

Возвращает целочисленный идентификатор для пользователя, вошедшего в систему.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample40.cs)]

### `WebSecurity.CurrentUserName`

Возвращает имя пользователя, вошедшего в систему.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample41.cs)]

### `WebSecurity.GeneratePasswordResetToken(username`  
 `[, tokenExpirationInMinutesFromNow])`

Создает маркер сброса пароля, которое может быть отправлено в сообщении электронной почты для пользователя, чтобы пользователь может сбросить пароль.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample42.cs)]

### `WebSecurity.GetUserId(userName)`

Возвращает идентификатор пользователя, от имени пользователя.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample43.cs)]

### `WebSecurity.IsAuthenticated`

Возвращает значение true, если текущий пользователь вошел в систему.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample44.cs)]

### `WebSecurity.IsConfirmed(userName)`

Возвращает значение true, если пользователь подтвержден (например, с помощью подтверждения адреса электронной почты).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample45.cs)]

### `WebSecurity.IsCurrentUser(userName)`

Возвращает значение true, если имя текущего пользователя совпадает с именем указанного пользователя.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample46.cs)]

### `WebSecurity.Login(userName,password[, persistCookie])`

Осуществляет вход пользователя в, задав маркер проверки подлинности в файле cookie.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample47.cs)]

### `WebSecurity.Logout()`

Осуществляет вход пользователя помещает путем удаления файла cookie проверки подлинности для маркеров.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample48.css)]

### `WebSecurity.RequireAuthenticatedUser()`

Если пользователь не прошел проверку подлинности, задает состояние HTTP 401 (не санкционировано).

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample49.css)]

### `WebSecurity.RequireRoles(roles)`

Если текущий пользователь не является членом одной из указанных ролей, задает состояние HTTP 401 (не санкционировано).

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample50.html)]

### `WebSecurity.RequireUser(userId)`  
`WebSecurity.RequireUser(userName)`

Если текущий пользователь не является пользователь, заданный параметром *username*, задает состояние HTTP 401 (не санкционировано).

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample51.css)]

### `WebSecurity.ResetPassword(passwordResetToken,newPassword)`

Если маркер сброса пароля является допустимой, изменяет пароль пользователя на новый пароль.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample52.css)]

<a id="Data"></a>
## <a name="data"></a>Данные

### `Database.Execute(SQLstatement [,parameters]`

Выполняет *SQLstatement* (с необязательными параметрами), такие как INSERT, DELETE или UPDATE и возвращает количество затронутых записей.

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample53.sql)]

### `Database.GetLastInsertId()`

Возвращает столбец идентификаторов из последней вставленной строки.

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample54.sql)]

### `Database.Open(filename)`  
`Database.Open(connectionStringName)`

Открывает указанный файл базы данных или базы данных, указанной с помощью именованную строку соединения из *Web.config* файла.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample55.cs)]

### `Database.OpenConnectionString(connectionString)`

Открывает базу данных с помощью строки подключения. (Это отличается от `Database.Open`, которая использует имя строки подключения.)

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample56.cs)]

### `Database.Query(SQLstatement[,parameters])`

Запрашивает базу данных при помощи *SQLstatement* (иногда с передаваемыми параметрами) и возвращает результаты в виде коллекции.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample57.html)]

### `Database.QuerySingle(SQLstatement [, parameters])`

Выполняет *SQLstatement* (с необязательными параметрами) и возвращает одну запись.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample58.cs)]

### `Database.QueryValue(SQLstatement [, parameters])`

Выполняет *SQLstatement* (с необязательными параметрами) и возвращает одиночное значение.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample59.cs)]

<a id="Helpers"></a>
## <a name="helpers"></a>Вспомогательные функции

### `Analytics.GetGoogleHtml(webPropertyId)`

Отображает код Google Analytics JavaScript для указанного идентификатора.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample60.js)]

### `Analytics.GetStatCounterHtml(project,security)`

Отображает код JavaScript StatCounter аналитики для указанного проекта.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample61.css)]

### `Analytics.GetYahooHtml(account)`

Отображает код Yahoo Analytics JavaScript для указанной учетной записи.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample62.js)]

### `Bing.SearchBox([boxWidth])`

Передает поиск Bing. Укажите сайт для поиска и заголовком поля поиска, можно задать `Bing.SiteUrl` и `Bing.SiteTitle` свойства. Обычно эти свойства задаются  *\_AppStart* страницы.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample63.html)]

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample64.cshtml)]

### `Chart(width,height [, template] [, templatePath])`

Инициализирует диаграммы.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample65.cshtml)]

### `Chart.AddLegend([title] [, name])`

Добавляет условные обозначения диаграммы.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample66.cshtml)]

### `Chart.AddSeries([name] [, chartType] [, chartArea]`  
 `[, axisLabel] [, legend] [, markerStep] [, xValue]`  
 `[, xField] [, yValues] [, yFields] [, options])`

Добавляет последовательность значений диаграммы.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample67.cshtml)]

### `Crypto.Hash(string [, algorithm])`  
`Crypto.Hash(bytes [, algorithm])`

Возвращает хэш-значение для указанных данных. Алгоритм по умолчанию — `sha256`.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample68.html)]

### `Facebook.LikeButton(href [, buttonLayout] [, showFaces] [, width] [, height]`   
 `[, action] [, font] [, colorScheme] [, refLabel])`

Позволяет устанавливать подключение к страницам пользователей Facebook.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample69.js)]

### `FileUpload.GetHtml([initialNumberOfFiles] [, allowMoreFilesToBeAdded]`  
 `[, includeFormTag] [, addText] [, uploadText])`

Отрисовывает пользовательский Интерфейс для передачи файлов.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample70.html)]

### `GamerCard.GetHtml(gamerTag)`

Выполняет визуализацию указанного тега игровой Xbox.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample71.js)]

### `Gravatar.GetHtml(email [, imageSize] [, defaultImage] [, rating]`  
 `[, imageExtension] [, attributes])`

Отображает изображение глобально распознаваемые, чтобы указанный адрес электронной почты.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample72.css)]

### `Json.Encode(object)`

Преобразует объект данных в строку в формате JavaScript Object Notation (JSON).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample73.cs)]

### `Json.Decode(string)`

Преобразует входной строки с кодированием JSON в объект данных, который можно итерацию или вставить в базу данных.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample74.cs)]

### `LinkShare.GetHtml(pageTitle[, pageLinkBack] [, twitterUserName]`  
 `[, additionalTweetText] [, linkSites])`

Подготавливает к просмотру социальной сети ссылок, с помощью указанного заголовка и необязательный URL-адрес.

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample75.xml)]

### `ModelStateDictionary.AddError(key, errorMessage)`

Связывает сообщение об ошибке с полем формы. Используйте `ModelState` вспомогательный метод для обращения к этому члену.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample76.cs)]

### `ModelStateDictionary.AddFormError(errorMessage)`

Связывает сообщение об ошибке с формой. Используйте `ModelState` вспомогательный метод для обращения к этому члену.

[!code-powershell[Main](asp-net-web-pages-api-reference/samples/sample77.ps1)]

### `ModelStateDictionary.IsValid`

Возвращает значение true, если нет ошибок проверки. Используйте `ModelState` вспомогательный метод для обращения к этому члену.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample78.cs)]

### `ObjectInfo.Print(value [, depth] [, enumerationLength])`

Отображает свойства и значения объекта и всех дочерних объектов.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample79.css)]

### `Recaptcha.GetHtml([, publicKey] [, theme] [, language] [, tabIndex])`

Подготавливает к просмотру проверку reCAPTCHA.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample80.css)]

### `ReCaptcha.PublicKey`  
 `ReCaptcha.PrivateKey`

Задает открытый и закрытый ключи reCAPTCHA службы. Обычно эти свойства задаются  *\_AppStart* страницы.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample81.css)]

### `ReCaptcha.Validate([, privateKey])`

Возвращает результат теста reCAPTCHA.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample82.cs)]

### `ServerInfo.GetHtml()`

Отображает сведения о состоянии о веб-страниц ASP.NET.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample83.cshtml)]

### `Twitter.Profile(twitterUserName)`

Подготавливает к просмотру поток Twitter для указанного пользователя.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample84.js)]

### `Twitter.Search(searchQuery)`

Подготавливает к просмотру поток Twitter для поиска указанного текста.

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample85.xml)]

### `Video.Flash(filename [, width, height])`

Подготавливает к просмотру флэш-памяти видеопроигрывателя для указанного файла с необязательным шириной и высотой.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample86.cshtml)]

### `Video.MediaPlayer(filename [, width, height])`

Подготавливает к просмотру проигрывателя Windows Media для указанного файла с необязательным шириной и высотой.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample87.cshtml)]

### `Video.Silverlight(filename, width, height)`

Подготавливает к просмотру проигрывателя Silverlight для указанного *.xap* файл с требуемой ширины и высоты.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample88.cshtml)]

### `WebCache.Get(key)`

Возвращает объект, заданный *ключ*, или значение null, если объект не найден.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample89.cs)]

### `WebCache.Remove(key)`

Удаляет заданный объект *ключ* из кэша.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample90.cs)]

### `WebCache.Set(key, value [, minutesToCache] [, slidingExpiration])`

Помещает *значение* в кэш под именем, заданным *ключ*.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample91.html)]

### `WebGrid(data)`

Создает новый `WebGrid` с помощью данных из запроса.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample92.cs)]

### `WebGrid.GetHtml()`

Выводит разметку для отображения данных в HTML-таблицу.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample93.html)]

### `WebGrid.Pager()`

Отображает страничный навигатор для `WebGrid` объекта.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample94.html)]

### `WebImage(path)`

Загружает изображение из указанного пути.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample95.cs)]

### `WebImage.AddImagesWatermark(image)`

Добавляет указанное изображение в качестве водяного знака.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample96.cs)]

### `WebImage.AddTextWatermark(text)`

Добавляет указанный текст для изображения.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample97.cs)]

### `WebImage.FlipHorizontal()`  
`WebImage.FlipVertical()`

Зеркальное отражение изображения по горизонтали или вертикали.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample98.css)]

### `WebImage.GetImageFromRequest()`

Загружает изображение при отправке изображения страницы при передаче файла.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample99.cs)]

### `WebImage.Resize(width,height)`

Изменяет размер изображения.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample100.css)]

### `WebImage.RotateLeft()`  
`WebImage.RotateRight()`

Поворот изображения влево или вправо.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample101.css)]

### `WebImage.Save(path [, imageFormat])`

Сохраняет изображение в указанный каталог.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample102.js)]

### `WebMail.Password`

Задает пароль для SMTP-сервера. Обычно это свойство задается в  *\_AppStart* страницы.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample103.cs)]

### `WebMail.Send(to, subject, body [, from] [, cc] [, filesToAttach] [, isBodyHtml]`  
 `[, additionalHeaders])`

Отправляет сообщение электронной почты.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample104.css)]

### `WebMail.SmtpServer`

Задает имя SMTP-сервера. Обычно это свойство задается в*\_AppStart* страницы.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample105.html)]

### `WebMail.UserName`

Задает имя пользователя для SMTP-сервера. Обычно это свойство следует задать в  *\_AppStart* страницы.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample106.html)]

<a id="Validation"></a>
## <a name="validation"></a>Проверка

### `Html.ValidationMessage(field)`

(v2) Отображает сообщение об ошибке проверки для указанного поля.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample107.cshtml)]

### `Html.ValidationSummary([message])`

(v2) Отображает список всех ошибок проверки.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample108.cshtml)]

### `Validation.Add(field, validationType)`

(v2) Регистрирует элемент ввода для указанного типа проверки.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample109.js)]

### `Validation.ClassFor(field)`

(v2) Динамически подготавливает к просмотру атрибутов класса CSS для проверки на стороне клиента, что можно форматировать сообщения об ошибках проверки. (Требуется ссылаться на библиотеки соответствующие клиентских скриптов и определении классов CSS).

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample110.html)]

### `Validation.For(field)`

(v2) Включает проверку на стороне клиента для поля ввода пользователя. (Требуется ссылаться на библиотеки соответствующего клиентского скрипта).

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample111.html)]

### `Validation.IsValid()`

(v2) Возвращает значение true, если все элементы пользовательского ввода, зарегистрированного для проверки содержать допустимые значения.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample112.cs)]

### `Validation.RequireField(field[, errorMessage])`

(v2) Указывает, что пользователям необходимо предоставить значение для элемента ввода.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample113.cs)]

### `Validation.RequireFields(field1[, field12, field3, ...])`

(v2) Указывает, что пользователям необходимо предоставить значения для каждого элемента ввода пользователя. Этот метод не позволяет задать пользовательское сообщение об ошибке.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample114.html)]

### `Validator.DateTime ([error message])`  
`Validator.Decimal([error message])`  
`Validator.EqualsTo(otherField,[error message])`  
`Validator.Float([error message])`  
`Validator.Integer([error message])`  
`Validator.Range(min,max [, error message])`  
`Validator.RegEx(pattern[, error message])`  
`Validator.Required([error message])`  
`Validator.StringLength(length)`  
`Validator.Url([error message])`

(v2) Задает проверку при использовании `Validation.Add` метода.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample115.js)]
