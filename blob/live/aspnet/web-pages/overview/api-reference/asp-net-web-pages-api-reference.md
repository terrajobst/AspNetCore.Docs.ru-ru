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
<a name="aspnet-web-pages-razor-api-quick-reference"></a><span data-ttu-id="71324-103">Веб-страницы ASP.NET (Razor) краткий справочник по API</span><span class="sxs-lookup"><span data-stu-id="71324-103">ASP.NET Web Pages (Razor) API Quick Reference</span></span>
====================
<span data-ttu-id="71324-104">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="71324-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="71324-105">Эта страница содержит список, содержащий краткие примеры наиболее часто используемые объекты, свойства и методы для программирования веб-страниц ASP.NET с синтаксисом Razor.</span><span class="sxs-lookup"><span data-stu-id="71324-105">This page contains a list with brief examples of the most commonly used objects, properties, and methods for programming ASP.NET Web Pages with Razor syntax.</span></span>
> 
> <span data-ttu-id="71324-106">Описания «(v2)», отмеченные появились в веб-страниц ASP.NET версии 2.</span><span class="sxs-lookup"><span data-stu-id="71324-106">Descriptions marked with "(v2)" were introduced in ASP.NET Web Pages version 2.</span></span>
> 
> <span data-ttu-id="71324-107">Справочная документация по API, в разделе [справочная документация веб-страниц ASP.NET](https://go.microsoft.com/fwlink/?LinkId=208659) на сайте MSDN.</span><span class="sxs-lookup"><span data-stu-id="71324-107">For API reference documentation, see the [ASP.NET Web Pages Reference Documentation](https://go.microsoft.com/fwlink/?LinkId=208659) on MSDN.</span></span>
> 
> ## <a name="software-versions"></a><span data-ttu-id="71324-108">Версии программного обеспечения</span><span class="sxs-lookup"><span data-stu-id="71324-108">Software versions</span></span>
> 
> 
> - <span data-ttu-id="71324-109">Веб-страниц ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="71324-109">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="71324-110">Этот учебник также работает с 2 веб-страниц ASP.NET и веб-страниц ASP.NET 1.0 (за исключением функций, отмеченных как v2).</span><span class="sxs-lookup"><span data-stu-id="71324-110">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0 (except for features marked v2).</span></span>


<span data-ttu-id="71324-111">Эта страница содержит сведения о следующих:</span><span class="sxs-lookup"><span data-stu-id="71324-111">This page contains reference information for the following:</span></span>

- [<span data-ttu-id="71324-112">Классы</span><span class="sxs-lookup"><span data-stu-id="71324-112">Classes</span></span>](#Classes)
- [<span data-ttu-id="71324-113">Данные</span><span class="sxs-lookup"><span data-stu-id="71324-113">Data</span></span>](#Data)
- [<span data-ttu-id="71324-114">Вспомогательные функции</span><span class="sxs-lookup"><span data-stu-id="71324-114">Helpers</span></span>](#Helpers)
- [<span data-ttu-id="71324-115">Проверка</span><span class="sxs-lookup"><span data-stu-id="71324-115">Validation</span></span>](#Validation)

<a id="Classes"></a>
## <a name="classes"></a><span data-ttu-id="71324-116">Классы</span><span class="sxs-lookup"><span data-stu-id="71324-116">Classes</span></span>

### `AppState[key], AppState[index],App`

<span data-ttu-id="71324-117">Содержит данные, которые могут совместно использоваться все страницы в приложении.</span><span class="sxs-lookup"><span data-stu-id="71324-117">Contains data that can be shared by any pages in the application.</span></span> <span data-ttu-id="71324-118">Можно использовать динамическую `App` свойство для доступа к данным, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="71324-118">You can use the dynamic `App` property to access the same data, as in the following example:</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample1.html)]

### `AsBool(), AsBool(true|false)`

<span data-ttu-id="71324-119">Преобразует строковое значение в логическое значение (ИСТИНА или ЛОЖЬ).</span><span class="sxs-lookup"><span data-stu-id="71324-119">Converts a string value to a Boolean value (true/false).</span></span> <span data-ttu-id="71324-120">Возвращает значение false или указанное значение, если строка не представляет значение true или false.</span><span class="sxs-lookup"><span data-stu-id="71324-120">Returns false or the specified value if the string does not represent true/false.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample2.cs)]

### `AsDateTime(), AsDateTime(value)`

<span data-ttu-id="71324-121">Преобразует строковое значение для даты и времени.</span><span class="sxs-lookup"><span data-stu-id="71324-121">Converts a string value to date/time.</span></span> <span data-ttu-id="71324-122">Возвращает `DateTime.MinValue` или указанное значение, если строка не представляет даты и времени.</span><span class="sxs-lookup"><span data-stu-id="71324-122">Returns `DateTime.MinValue` or the specified value if the string does not represent a date/time.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample3.cs)]

### `AsDecimal(), AsDecimal(value)`

<span data-ttu-id="71324-123">Преобразует строковое значение в десятичное значение.</span><span class="sxs-lookup"><span data-stu-id="71324-123">Converts a string value to a decimal value.</span></span> <span data-ttu-id="71324-124">Возвращает 0,0 или указанное значение, если строка не представляет значение десятичного числа.</span><span class="sxs-lookup"><span data-stu-id="71324-124">Returns 0.0 or the specified value if the string does not represent a decimal value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample4.cs)]

### `AsFloat(), AsFloat(value)`

<span data-ttu-id="71324-125">Преобразует строковое значение в значение с плавающей запятой.</span><span class="sxs-lookup"><span data-stu-id="71324-125">Converts a string value to a float.</span></span> <span data-ttu-id="71324-126">Возвращает 0,0 или указанное значение, если строка не представляет значение десятичного числа.</span><span class="sxs-lookup"><span data-stu-id="71324-126">Returns 0.0 or the specified value if the string does not represent a decimal value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample5.cs)]

### `AsInt(), AsInt(value)`

<span data-ttu-id="71324-127">Преобразует строковое значение в целое число.</span><span class="sxs-lookup"><span data-stu-id="71324-127">Converts a string value to an integer.</span></span> <span data-ttu-id="71324-128">Возвращает значение 0 или указанное значение, если строка не представляет целое число.</span><span class="sxs-lookup"><span data-stu-id="71324-128">Returns 0 or the specified value if the string does not represent an integer.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample6.cs)]

### `Href(path [, param1 [, param2]])`

<span data-ttu-id="71324-129">Создает URL-адрес веб обозревателем из путь к локальному файлу с частями необязательный дополнительный путь.</span><span class="sxs-lookup"><span data-stu-id="71324-129">Creates a browser-compatible URL from a local file path, with optional additional path parts.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample7.cshtml)]

### `Html.Raw(value)`

<span data-ttu-id="71324-130">Подготавливает к просмотру *значение* как HTML-разметка, вместо отображения его выходные данные в кодировке HTML.</span><span class="sxs-lookup"><span data-stu-id="71324-130">Renders *value* as HTML markup instead of rendering it as HTML-encoded output.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample8.cshtml)]

### `IsBool(), IsDateTime(), IsDecimal(), IsFloat(), IsInt()`

<span data-ttu-id="71324-131">Возвращает значение true, если значение может быть преобразовано из строки в указанный тип.</span><span class="sxs-lookup"><span data-stu-id="71324-131">Returns true if the value can be converted from a string to the specified type.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample9.cs)]

### `IsEmpty()`

<span data-ttu-id="71324-132">Возвращает значение true, если объект или переменная не имеет значения.</span><span class="sxs-lookup"><span data-stu-id="71324-132">Returns true if the object or variable has no value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample10.cs)]

### `IsPost`

<span data-ttu-id="71324-133">Возвращает значение true, если запрос POST.</span><span class="sxs-lookup"><span data-stu-id="71324-133">Returns true if the request is a POST.</span></span> <span data-ttu-id="71324-134">(Начальных запросов обычно являются GET).</span><span class="sxs-lookup"><span data-stu-id="71324-134">(Initial requests are usually a GET.)</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample11.cs)]

### `Layout`

<span data-ttu-id="71324-135">Задает путь страницы макета для применения к этой странице.</span><span class="sxs-lookup"><span data-stu-id="71324-135">Specifies the path of a layout page to apply to this page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample12.html)]

### `PageData[key], PageData[index],Page`

<span data-ttu-id="71324-136">Содержит данные, общим для страниц, страниц макетов и частичных страниц в текущий запрос.</span><span class="sxs-lookup"><span data-stu-id="71324-136">Contains data shared between the page, layout pages, and partial pages in the current request.</span></span> <span data-ttu-id="71324-137">Можно использовать динамическую `Page` свойство для доступа к данным, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="71324-137">You can use the dynamic `Page` property to access the same data, as in the following example:</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample13.html)]

### `RenderBody()`

<span data-ttu-id="71324-138">(Макет страницы) Отображает содержимое страницы содержимого, не все именованные разделы.</span><span class="sxs-lookup"><span data-stu-id="71324-138">(Layout pages) Renders the content of a content page that is not in any named sections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample14.cs)]

### `RenderPage(path, values)`  
`RenderPage(path[,param1 [, param2]])`

<span data-ttu-id="71324-139">Отображает страницу содержимого, используя заданный путь и необязательные дополнительные данные.</span><span class="sxs-lookup"><span data-stu-id="71324-139">Renders a content page using the specified path and optional extra data.</span></span> <span data-ttu-id="71324-140">Можно получить значения дополнительных параметров из `PageData` по положению (например, 1) или ключ (например, 2).</span><span class="sxs-lookup"><span data-stu-id="71324-140">You can get the values of the extra parameters from `PageData` by position (example 1) or key (example 2).</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample15.js)]

### `RenderSection(sectionName [, required = true|false])`

<span data-ttu-id="71324-141">(Макет страницы) Отображает раздел содержимого с именем.</span><span class="sxs-lookup"><span data-stu-id="71324-141">(Layout pages) Renders a content section that has a name.</span></span> <span data-ttu-id="71324-142">Задать *необходимые* значение false, чтобы сделать необязательный раздел.</span><span class="sxs-lookup"><span data-stu-id="71324-142">Set *required* to false to make a section optional.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample16.js)]

### `Request.Cookies[key]`

<span data-ttu-id="71324-143">Возвращает или задает значение файла cookie HTTP.</span><span class="sxs-lookup"><span data-stu-id="71324-143">Gets or sets the value of an HTTP cookie.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample17.cs)]

### `Request.Files[key]`

<span data-ttu-id="71324-144">Получает файлы, которые были загружены в текущий запрос.</span><span class="sxs-lookup"><span data-stu-id="71324-144">Gets the files that were uploaded in the current request.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample18.js)]

### `Request.Form[key]`

<span data-ttu-id="71324-145">Возвращает данные, была отправлена в форме (в виде строки).</span><span class="sxs-lookup"><span data-stu-id="71324-145">Gets data that was posted in a form (as strings).</span></span> <span data-ttu-id="71324-146">`Request[key]`проверяет оба `Request.Form` и `Request.QueryString` коллекции.</span><span class="sxs-lookup"><span data-stu-id="71324-146">`Request[key]` checks both the `Request.Form` and the `Request.QueryString` collections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample19.cs)]

### `Request.QueryString[key]`

<span data-ttu-id="71324-147">Возвращает данные, который указан в строке запроса URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="71324-147">Gets data that was specified in the URL query string.</span></span> <span data-ttu-id="71324-148">`Request[key]`проверяет оба `Request.Form` и `Request.QueryString` коллекции.</span><span class="sxs-lookup"><span data-stu-id="71324-148">`Request[key]` checks both the `Request.Form` and the `Request.QueryString` collections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample20.cs)]

### `Request.Unvalidated(key)`  
`Request.Unvalidated().QueryString|Form|Cookies|Headers[key]`

<span data-ttu-id="71324-149">Выборочно отключает запрос проверки для элементов формы, значения строки запроса, файл cookie или значение заголовка.</span><span class="sxs-lookup"><span data-stu-id="71324-149">Selectively disables request validation for a form element, query-string value, cookie, or header value.</span></span> <span data-ttu-id="71324-150">Проверка запросов включена по умолчанию и пользователи не смогут учета разметки или других потенциально опасное содержимое.</span><span class="sxs-lookup"><span data-stu-id="71324-150">Request validation is enabled by default and prevents users from posting markup or other potentially dangerous content.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample21.cs)]

### `Response.AddHeader(name, value)`

<span data-ttu-id="71324-151">Добавляет заголовок НТТР сервер в ответ.</span><span class="sxs-lookup"><span data-stu-id="71324-151">Adds an HTTP server header to the response.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample22.cs)]

### `Response.OutputCache(seconds [, sliding] [, varyByParams])`

<span data-ttu-id="71324-152">Кэширование вывода страниц в указанное время.</span><span class="sxs-lookup"><span data-stu-id="71324-152">Caches the page output for a specified time.</span></span> <span data-ttu-id="71324-153">При необходимости установить *скользящий* Сброс времени ожидания для доступа к каждой странице и *varyByParams* кэширование разные версии страницы для каждой строки запроса, различающимся в запросе страницы.</span><span class="sxs-lookup"><span data-stu-id="71324-153">Optionally set *sliding* to reset the timeout on each page access and *varyByParams* to cache different versions of the page for each different query string in the page request.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample23.js)]

### `Response.Redirect(path)`

<span data-ttu-id="71324-154">Перенаправляет запрос браузера в новое расположение.</span><span class="sxs-lookup"><span data-stu-id="71324-154">Redirects the browser request to a new location.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample24.js)]

### `Response.SetStatus(httpStatusCode)`

<span data-ttu-id="71324-155">Задает код состояния HTTP, отправляемого в браузер.</span><span class="sxs-lookup"><span data-stu-id="71324-155">Sets the HTTP status code sent to the browser.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample25.cs)]

### `Response.WriteBinary(data [, mimetype])`

<span data-ttu-id="71324-156">Записывает содержимое *данные* ответ с типом MIME, необязательно.</span><span class="sxs-lookup"><span data-stu-id="71324-156">Writes the contents of *data* to the response with an optional MIME type.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample26.js)]

### `Response.WriteFile(file)`

<span data-ttu-id="71324-157">Записывает содержимое файла в ответ.</span><span class="sxs-lookup"><span data-stu-id="71324-157">Writes the contents of a file to the response.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample27.cs)]

### `@section(sectionName) {content }`

<span data-ttu-id="71324-158">(Макет страницы) Определяет, имя раздела содержимого.</span><span class="sxs-lookup"><span data-stu-id="71324-158">(Layout pages) Defines a content section that has a name.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample28.cshtml)]

### `Server.HtmlDecode(htmlText)`

<span data-ttu-id="71324-159">Декодирует строку, — в кодировке HTML.</span><span class="sxs-lookup"><span data-stu-id="71324-159">Decodes a string that is HTML encoded.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample29.cs)]

### `Server.HtmlEncode(text)`

<span data-ttu-id="71324-160">Кодирует строку для подготовки к просмотру в HTML-разметку.</span><span class="sxs-lookup"><span data-stu-id="71324-160">Encodes a string for rendering in HTML markup.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample30.cs)]

### `Server.MapPath(virtualPath)`

<span data-ttu-id="71324-161">Возвращает физический путь к серверу для заданного виртуального пути.</span><span class="sxs-lookup"><span data-stu-id="71324-161">Returns the server physical path for the specified virtual path.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample31.cs)]

### `Server.UrlDecode(urlText)`

<span data-ttu-id="71324-162">Расшифровывает текст с URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="71324-162">Decodes text from a URL.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample32.cs)]

### `Server.UrlEncode(text)`

<span data-ttu-id="71324-163">Кодирует текст для размещения в URL-АДРЕСЕ.</span><span class="sxs-lookup"><span data-stu-id="71324-163">Encodes text to put in a URL.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample33.cs)]

### `Session[key]`

<span data-ttu-id="71324-164">Возвращает или задает значение, которое существует, пока пользователь не закроет браузер.</span><span class="sxs-lookup"><span data-stu-id="71324-164">Gets or sets a value that exists until the user closes the browser.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample34.css)]

### `ToString()`

<span data-ttu-id="71324-165">Отображает строковое представление значения объекта.</span><span class="sxs-lookup"><span data-stu-id="71324-165">Displays a string representation of the object's value.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample35.html)]

### `UrlData[index]`

<span data-ttu-id="71324-166">Возвращает дополнительные сведения по URL-адресу (например, */MyPage/ExtraData*).</span><span class="sxs-lookup"><span data-stu-id="71324-166">Gets additional data from the URL (for example, */MyPage/ExtraData*).</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample36.cs)]

### `WebSecurity.ChangePassword(userName,currentPassword,newPassword)`

<span data-ttu-id="71324-167">Изменяет пароль для указанного пользователя.</span><span class="sxs-lookup"><span data-stu-id="71324-167">Changes the password for the specified user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample37.cs)]

### `WebSecurity.ConfirmAccount(accountConfirmationToken)`

<span data-ttu-id="71324-168">Подтверждение учетной записи с помощью токена подтверждения учетной записи.</span><span class="sxs-lookup"><span data-stu-id="71324-168">Confirms an account using the account confirmation token.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample38.cs)]

### `WebSecurity.CreateAccount(userName, password`  
 `[, requireConfirmationToken = true|false])`

<span data-ttu-id="71324-169">Создает новую учетную запись пользователя с указанным именем пользователя и паролем.</span><span class="sxs-lookup"><span data-stu-id="71324-169">Creates a new user account with the specified user name and password.</span></span> <span data-ttu-id="71324-170">Если требуется маркер подтверждения, передайте значение true, если для *requireConfirmationToken.*</span><span class="sxs-lookup"><span data-stu-id="71324-170">To require a confirmation token, pass true for *requireConfirmationToken.*</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample39.cs)]

### `WebSecurity.CurrentUserId`

<span data-ttu-id="71324-171">Возвращает целочисленный идентификатор для пользователя, вошедшего в систему.</span><span class="sxs-lookup"><span data-stu-id="71324-171">Gets the integer identifier for the currently logged-in user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample40.cs)]

### `WebSecurity.CurrentUserName`

<span data-ttu-id="71324-172">Возвращает имя пользователя, вошедшего в систему.</span><span class="sxs-lookup"><span data-stu-id="71324-172">Gets the name for the currently logged-in user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample41.cs)]

### `WebSecurity.GeneratePasswordResetToken(username`  
 `[, tokenExpirationInMinutesFromNow])`

<span data-ttu-id="71324-173">Создает маркер сброса пароля, которое может быть отправлено в сообщении электронной почты для пользователя, чтобы пользователь может сбросить пароль.</span><span class="sxs-lookup"><span data-stu-id="71324-173">Generates a password-reset token that can be sent in email to a user so that the user can reset the password.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample42.cs)]

### `WebSecurity.GetUserId(userName)`

<span data-ttu-id="71324-174">Возвращает идентификатор пользователя, от имени пользователя.</span><span class="sxs-lookup"><span data-stu-id="71324-174">Returns the user ID from the user name.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample43.cs)]

### `WebSecurity.IsAuthenticated`

<span data-ttu-id="71324-175">Возвращает значение true, если текущий пользователь вошел в систему.</span><span class="sxs-lookup"><span data-stu-id="71324-175">Returns true if the current user is logged in.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample44.cs)]

### `WebSecurity.IsConfirmed(userName)`

<span data-ttu-id="71324-176">Возвращает значение true, если пользователь подтвержден (например, с помощью подтверждения адреса электронной почты).</span><span class="sxs-lookup"><span data-stu-id="71324-176">Returns true if the user has been confirmed (for example, through a confirmation email).</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample45.cs)]

### `WebSecurity.IsCurrentUser(userName)`

<span data-ttu-id="71324-177">Возвращает значение true, если имя текущего пользователя совпадает с именем указанного пользователя.</span><span class="sxs-lookup"><span data-stu-id="71324-177">Returns true if the current user's name matches the specified user name.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample46.cs)]

### `WebSecurity.Login(userName,password[, persistCookie])`

<span data-ttu-id="71324-178">Осуществляет вход пользователя в, задав маркер проверки подлинности в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="71324-178">Logs the user in by setting an authentication token in the cookie.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample47.cs)]

### `WebSecurity.Logout()`

<span data-ttu-id="71324-179">Осуществляет вход пользователя помещает путем удаления файла cookie проверки подлинности для маркеров.</span><span class="sxs-lookup"><span data-stu-id="71324-179">Logs the user out by removing the authentication token cookie.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample48.css)]

### `WebSecurity.RequireAuthenticatedUser()`

<span data-ttu-id="71324-180">Если пользователь не прошел проверку подлинности, задает состояние HTTP 401 (не санкционировано).</span><span class="sxs-lookup"><span data-stu-id="71324-180">If the user is not authenticated, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample49.css)]

### `WebSecurity.RequireRoles(roles)`

<span data-ttu-id="71324-181">Если текущий пользователь не является членом одной из указанных ролей, задает состояние HTTP 401 (не санкционировано).</span><span class="sxs-lookup"><span data-stu-id="71324-181">If the current user is not a member of one of the specified roles, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample50.html)]

### `WebSecurity.RequireUser(userId)`  
`WebSecurity.RequireUser(userName)`

<span data-ttu-id="71324-182">Если текущий пользователь не является пользователь, заданный параметром *username*, задает состояние HTTP 401 (не санкционировано).</span><span class="sxs-lookup"><span data-stu-id="71324-182">If the current user is not the user specified by *username*, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample51.css)]

### `WebSecurity.ResetPassword(passwordResetToken,newPassword)`

<span data-ttu-id="71324-183">Если маркер сброса пароля является допустимой, изменяет пароль пользователя на новый пароль.</span><span class="sxs-lookup"><span data-stu-id="71324-183">If the password reset token is valid, changes the user's password to the new password.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample52.css)]

<a id="Data"></a>
## <a name="data"></a><span data-ttu-id="71324-184">Данные</span><span class="sxs-lookup"><span data-stu-id="71324-184">Data</span></span>

### `Database.Execute(SQLstatement [,parameters]`

<span data-ttu-id="71324-185">Выполняет *SQLstatement* (с необязательными параметрами), такие как INSERT, DELETE или UPDATE и возвращает количество затронутых записей.</span><span class="sxs-lookup"><span data-stu-id="71324-185">Executes *SQLstatement* (with optional parameters) such as INSERT, DELETE, or UPDATE and returns a count of affected records.</span></span>

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample53.sql)]

### `Database.GetLastInsertId()`

<span data-ttu-id="71324-186">Возвращает столбец идентификаторов из последней вставленной строки.</span><span class="sxs-lookup"><span data-stu-id="71324-186">Returns the identity column from the most recently inserted row.</span></span>

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample54.sql)]

### `Database.Open(filename)`  
`Database.Open(connectionStringName)`

<span data-ttu-id="71324-187">Открывает указанный файл базы данных или базы данных, указанной с помощью именованную строку соединения из *Web.config* файла.</span><span class="sxs-lookup"><span data-stu-id="71324-187">Opens either the specified database file or the database specified using a named connection string from the *Web.config* file.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample55.cs)]

### `Database.OpenConnectionString(connectionString)`

<span data-ttu-id="71324-188">Открывает базу данных с помощью строки подключения.</span><span class="sxs-lookup"><span data-stu-id="71324-188">Opens a database using the connection string.</span></span> <span data-ttu-id="71324-189">(Это отличается от `Database.Open`, которая использует имя строки подключения.)</span><span class="sxs-lookup"><span data-stu-id="71324-189">(This contrasts with `Database.Open`, which uses a connection string name.)</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample56.cs)]

### `Database.Query(SQLstatement[,parameters])`

<span data-ttu-id="71324-190">Запрашивает базу данных при помощи *SQLstatement* (иногда с передаваемыми параметрами) и возвращает результаты в виде коллекции.</span><span class="sxs-lookup"><span data-stu-id="71324-190">Queries the database using *SQLstatement* (optionally passing parameters) and returns the results as a collection.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample57.html)]

### `Database.QuerySingle(SQLstatement [, parameters])`

<span data-ttu-id="71324-191">Выполняет *SQLstatement* (с необязательными параметрами) и возвращает одну запись.</span><span class="sxs-lookup"><span data-stu-id="71324-191">Executes *SQLstatement* (with optional parameters) and returns a single record.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample58.cs)]

### `Database.QueryValue(SQLstatement [, parameters])`

<span data-ttu-id="71324-192">Выполняет *SQLstatement* (с необязательными параметрами) и возвращает одиночное значение.</span><span class="sxs-lookup"><span data-stu-id="71324-192">Executes *SQLstatement* (with optional parameters) and returns a single value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample59.cs)]

<a id="Helpers"></a>
## <a name="helpers"></a><span data-ttu-id="71324-193">Вспомогательные функции</span><span class="sxs-lookup"><span data-stu-id="71324-193">Helpers</span></span>

### `Analytics.GetGoogleHtml(webPropertyId)`

<span data-ttu-id="71324-194">Отображает код Google Analytics JavaScript для указанного идентификатора.</span><span class="sxs-lookup"><span data-stu-id="71324-194">Renders the Google Analytics JavaScript code for the specified ID.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample60.js)]

### `Analytics.GetStatCounterHtml(project,security)`

<span data-ttu-id="71324-195">Отображает код JavaScript StatCounter аналитики для указанного проекта.</span><span class="sxs-lookup"><span data-stu-id="71324-195">Renders the StatCounter Analytics JavaScript code for the specified project.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample61.css)]

### `Analytics.GetYahooHtml(account)`

<span data-ttu-id="71324-196">Отображает код Yahoo Analytics JavaScript для указанной учетной записи.</span><span class="sxs-lookup"><span data-stu-id="71324-196">Renders the Yahoo Analytics JavaScript code for the specified account.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample62.js)]

### `Bing.SearchBox([boxWidth])`

<span data-ttu-id="71324-197">Передает поиск Bing.</span><span class="sxs-lookup"><span data-stu-id="71324-197">Passes a search to Bing.</span></span> <span data-ttu-id="71324-198">Укажите сайт для поиска и заголовком поля поиска, можно задать `Bing.SiteUrl` и `Bing.SiteTitle` свойства.</span><span class="sxs-lookup"><span data-stu-id="71324-198">To specify the site to search and a title for the search box, you can set the `Bing.SiteUrl` and `Bing.SiteTitle` properties.</span></span> <span data-ttu-id="71324-199">Обычно эти свойства задаются  *\_AppStart* страницы.</span><span class="sxs-lookup"><span data-stu-id="71324-199">Normally you set these properties in the *\_AppStart* page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample63.html)]

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample64.cshtml)]

### `Chart(width,height [, template] [, templatePath])`

<span data-ttu-id="71324-200">Инициализирует диаграммы.</span><span class="sxs-lookup"><span data-stu-id="71324-200">Initializes a chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample65.cshtml)]

### `Chart.AddLegend([title] [, name])`

<span data-ttu-id="71324-201">Добавляет условные обозначения диаграммы.</span><span class="sxs-lookup"><span data-stu-id="71324-201">Adds a legend to a chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample66.cshtml)]

### `Chart.AddSeries([name] [, chartType] [, chartArea]`  
 `[, axisLabel] [, legend] [, markerStep] [, xValue]`  
 `[, xField] [, yValues] [, yFields] [, options])`

<span data-ttu-id="71324-202">Добавляет последовательность значений диаграммы.</span><span class="sxs-lookup"><span data-stu-id="71324-202">Adds a series of values to the chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample67.cshtml)]

### `Crypto.Hash(string [, algorithm])`  
`Crypto.Hash(bytes [, algorithm])`

<span data-ttu-id="71324-203">Возвращает хэш-значение для указанных данных.</span><span class="sxs-lookup"><span data-stu-id="71324-203">Returns a hash for the specified data.</span></span> <span data-ttu-id="71324-204">Алгоритм по умолчанию — `sha256`.</span><span class="sxs-lookup"><span data-stu-id="71324-204">The default algorithm is `sha256`.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample68.html)]

### `Facebook.LikeButton(href [, buttonLayout] [, showFaces] [, width] [, height]`   
 `[, action] [, font] [, colorScheme] [, refLabel])`

<span data-ttu-id="71324-205">Позволяет устанавливать подключение к страницам пользователей Facebook.</span><span class="sxs-lookup"><span data-stu-id="71324-205">Lets Facebook users make a connection to pages.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample69.js)]

### `FileUpload.GetHtml([initialNumberOfFiles] [, allowMoreFilesToBeAdded]`  
 `[, includeFormTag] [, addText] [, uploadText])`

<span data-ttu-id="71324-206">Отрисовывает пользовательский Интерфейс для передачи файлов.</span><span class="sxs-lookup"><span data-stu-id="71324-206">Renders UI for uploading files.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample70.html)]

### `GamerCard.GetHtml(gamerTag)`

<span data-ttu-id="71324-207">Выполняет визуализацию указанного тега игровой Xbox.</span><span class="sxs-lookup"><span data-stu-id="71324-207">Renders the specified Xbox gamer tag.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample71.js)]

### `Gravatar.GetHtml(email [, imageSize] [, defaultImage] [, rating]`  
 `[, imageExtension] [, attributes])`

<span data-ttu-id="71324-208">Отображает изображение глобально распознаваемые, чтобы указанный адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="71324-208">Renders the Gravatar image for the specified email address.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample72.css)]

### `Json.Encode(object)`

<span data-ttu-id="71324-209">Преобразует объект данных в строку в формате JavaScript Object Notation (JSON).</span><span class="sxs-lookup"><span data-stu-id="71324-209">Converts a data object to a string in the JavaScript Object Notation (JSON) format.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample73.cs)]

### `Json.Decode(string)`

<span data-ttu-id="71324-210">Преобразует входной строки с кодированием JSON в объект данных, который можно итерацию или вставить в базу данных.</span><span class="sxs-lookup"><span data-stu-id="71324-210">Converts a JSON-encoded input string to a data object that you can iterate over or insert into a database.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample74.cs)]

### `LinkShare.GetHtml(pageTitle[, pageLinkBack] [, twitterUserName]`  
 `[, additionalTweetText] [, linkSites])`

<span data-ttu-id="71324-211">Подготавливает к просмотру социальной сети ссылок, с помощью указанного заголовка и необязательный URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="71324-211">Renders social networking links using the specified title and optional URL.</span></span>

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample75.xml)]

### `ModelStateDictionary.AddError(key, errorMessage)`

<span data-ttu-id="71324-212">Связывает сообщение об ошибке с полем формы.</span><span class="sxs-lookup"><span data-stu-id="71324-212">Associates an error message with a form field.</span></span> <span data-ttu-id="71324-213">Используйте `ModelState` вспомогательный метод для обращения к этому члену.</span><span class="sxs-lookup"><span data-stu-id="71324-213">Use the `ModelState` helper to access this member.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample76.cs)]

### `ModelStateDictionary.AddFormError(errorMessage)`

<span data-ttu-id="71324-214">Связывает сообщение об ошибке с формой.</span><span class="sxs-lookup"><span data-stu-id="71324-214">Associates an error message with a form.</span></span> <span data-ttu-id="71324-215">Используйте `ModelState` вспомогательный метод для обращения к этому члену.</span><span class="sxs-lookup"><span data-stu-id="71324-215">Use the `ModelState` helper to access this member.</span></span>

[!code-powershell[Main](asp-net-web-pages-api-reference/samples/sample77.ps1)]

### `ModelStateDictionary.IsValid`

<span data-ttu-id="71324-216">Возвращает значение true, если нет ошибок проверки.</span><span class="sxs-lookup"><span data-stu-id="71324-216">Returns true if there are no validation errors.</span></span> <span data-ttu-id="71324-217">Используйте `ModelState` вспомогательный метод для обращения к этому члену.</span><span class="sxs-lookup"><span data-stu-id="71324-217">Use the `ModelState` helper to access this member.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample78.cs)]

### `ObjectInfo.Print(value [, depth] [, enumerationLength])`

<span data-ttu-id="71324-218">Отображает свойства и значения объекта и всех дочерних объектов.</span><span class="sxs-lookup"><span data-stu-id="71324-218">Renders the properties and values of an object and any child objects.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample79.css)]

### `Recaptcha.GetHtml([, publicKey] [, theme] [, language] [, tabIndex])`

<span data-ttu-id="71324-219">Подготавливает к просмотру проверку reCAPTCHA.</span><span class="sxs-lookup"><span data-stu-id="71324-219">Renders the reCAPTCHA verification test.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample80.css)]

### `ReCaptcha.PublicKey`  
 `ReCaptcha.PrivateKey`

<span data-ttu-id="71324-220">Задает открытый и закрытый ключи reCAPTCHA службы.</span><span class="sxs-lookup"><span data-stu-id="71324-220">Sets public and private keys for the reCAPTCHA service.</span></span> <span data-ttu-id="71324-221">Обычно эти свойства задаются  *\_AppStart* страницы.</span><span class="sxs-lookup"><span data-stu-id="71324-221">Normally you set these properties in the *\_AppStart* page.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample81.css)]

### `ReCaptcha.Validate([, privateKey])`

<span data-ttu-id="71324-222">Возвращает результат теста reCAPTCHA.</span><span class="sxs-lookup"><span data-stu-id="71324-222">Returns the result of the reCAPTCHA test.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample82.cs)]

### `ServerInfo.GetHtml()`

<span data-ttu-id="71324-223">Отображает сведения о состоянии о веб-страниц ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="71324-223">Renders status information about ASP.NET Web Pages.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample83.cshtml)]

### `Twitter.Profile(twitterUserName)`

<span data-ttu-id="71324-224">Подготавливает к просмотру поток Twitter для указанного пользователя.</span><span class="sxs-lookup"><span data-stu-id="71324-224">Renders a Twitter stream for the specified user.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample84.js)]

### `Twitter.Search(searchQuery)`

<span data-ttu-id="71324-225">Подготавливает к просмотру поток Twitter для поиска указанного текста.</span><span class="sxs-lookup"><span data-stu-id="71324-225">Renders a Twitter stream for the specified search text.</span></span>

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample85.xml)]

### `Video.Flash(filename [, width, height])`

<span data-ttu-id="71324-226">Подготавливает к просмотру флэш-памяти видеопроигрывателя для указанного файла с необязательным шириной и высотой.</span><span class="sxs-lookup"><span data-stu-id="71324-226">Renders a Flash video player for the specified file with optional width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample86.cshtml)]

### `Video.MediaPlayer(filename [, width, height])`

<span data-ttu-id="71324-227">Подготавливает к просмотру проигрывателя Windows Media для указанного файла с необязательным шириной и высотой.</span><span class="sxs-lookup"><span data-stu-id="71324-227">Renders a Windows Media player for the specified file with optional width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample87.cshtml)]

### `Video.Silverlight(filename, width, height)`

<span data-ttu-id="71324-228">Подготавливает к просмотру проигрывателя Silverlight для указанного *.xap* файл с требуемой ширины и высоты.</span><span class="sxs-lookup"><span data-stu-id="71324-228">Renders a Silverlight player for the specified *.xap* file with required width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample88.cshtml)]

### `WebCache.Get(key)`

<span data-ttu-id="71324-229">Возвращает объект, заданный *ключ*, или значение null, если объект не найден.</span><span class="sxs-lookup"><span data-stu-id="71324-229">Returns the object specified by *key*, or null if the object is not found.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample89.cs)]

### `WebCache.Remove(key)`

<span data-ttu-id="71324-230">Удаляет заданный объект *ключ* из кэша.</span><span class="sxs-lookup"><span data-stu-id="71324-230">Removes the object specified by *key* from the cache.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample90.cs)]

### `WebCache.Set(key, value [, minutesToCache] [, slidingExpiration])`

<span data-ttu-id="71324-231">Помещает *значение* в кэш под именем, заданным *ключ*.</span><span class="sxs-lookup"><span data-stu-id="71324-231">Puts *value* into the cache under the name specified by *key*.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample91.html)]

### `WebGrid(data)`

<span data-ttu-id="71324-232">Создает новый `WebGrid` с помощью данных из запроса.</span><span class="sxs-lookup"><span data-stu-id="71324-232">Creates a new `WebGrid` object using data from a query.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample92.cs)]

### `WebGrid.GetHtml()`

<span data-ttu-id="71324-233">Выводит разметку для отображения данных в HTML-таблицу.</span><span class="sxs-lookup"><span data-stu-id="71324-233">Renders markup to display data in an HTML table.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample93.html)]

### `WebGrid.Pager()`

<span data-ttu-id="71324-234">Отображает страничный навигатор для `WebGrid` объекта.</span><span class="sxs-lookup"><span data-stu-id="71324-234">Renders a pager for the `WebGrid` object.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample94.html)]

### `WebImage(path)`

<span data-ttu-id="71324-235">Загружает изображение из указанного пути.</span><span class="sxs-lookup"><span data-stu-id="71324-235">Loads an image from the specified path.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample95.cs)]

### `WebImage.AddImagesWatermark(image)`

<span data-ttu-id="71324-236">Добавляет указанное изображение в качестве водяного знака.</span><span class="sxs-lookup"><span data-stu-id="71324-236">Adds the specified image as a watermark.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample96.cs)]

### `WebImage.AddTextWatermark(text)`

<span data-ttu-id="71324-237">Добавляет указанный текст для изображения.</span><span class="sxs-lookup"><span data-stu-id="71324-237">Adds the specified text to the image.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample97.cs)]

### `WebImage.FlipHorizontal()`  
`WebImage.FlipVertical()`

<span data-ttu-id="71324-238">Зеркальное отражение изображения по горизонтали или вертикали.</span><span class="sxs-lookup"><span data-stu-id="71324-238">Flips the image horizontally or vertically.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample98.css)]

### `WebImage.GetImageFromRequest()`

<span data-ttu-id="71324-239">Загружает изображение при отправке изображения страницы при передаче файла.</span><span class="sxs-lookup"><span data-stu-id="71324-239">Loads an image when an image is posted to a page during a file upload.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample99.cs)]

### `WebImage.Resize(width,height)`

<span data-ttu-id="71324-240">Изменяет размер изображения.</span><span class="sxs-lookup"><span data-stu-id="71324-240">Resizes an the image.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample100.css)]

### `WebImage.RotateLeft()`  
`WebImage.RotateRight()`

<span data-ttu-id="71324-241">Поворот изображения влево или вправо.</span><span class="sxs-lookup"><span data-stu-id="71324-241">Rotates the image to the left or the right.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample101.css)]

### `WebImage.Save(path [, imageFormat])`

<span data-ttu-id="71324-242">Сохраняет изображение в указанный каталог.</span><span class="sxs-lookup"><span data-stu-id="71324-242">Saves the image to the specified path.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample102.js)]

### `WebMail.Password`

<span data-ttu-id="71324-243">Задает пароль для SMTP-сервера.</span><span class="sxs-lookup"><span data-stu-id="71324-243">Sets the password for the SMTP server.</span></span> <span data-ttu-id="71324-244">Обычно это свойство задается в  *\_AppStart* страницы.</span><span class="sxs-lookup"><span data-stu-id="71324-244">Normally you set this property in the *\_AppStart* page.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample103.cs)]

### `WebMail.Send(to, subject, body [, from] [, cc] [, filesToAttach] [, isBodyHtml]`  
 `[, additionalHeaders])`

<span data-ttu-id="71324-245">Отправляет сообщение электронной почты.</span><span class="sxs-lookup"><span data-stu-id="71324-245">Sends an email message.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample104.css)]

### `WebMail.SmtpServer`

<span data-ttu-id="71324-246">Задает имя SMTP-сервера.</span><span class="sxs-lookup"><span data-stu-id="71324-246">Sets the SMTP server name.</span></span> <span data-ttu-id="71324-247">Обычно это свойство задается в*\_AppStart* страницы.</span><span class="sxs-lookup"><span data-stu-id="71324-247">Normally you set this property in the*\_AppStart* page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample105.html)]

### `WebMail.UserName`

<span data-ttu-id="71324-248">Задает имя пользователя для SMTP-сервера.</span><span class="sxs-lookup"><span data-stu-id="71324-248">Sets the user name for the SMTP server.</span></span> <span data-ttu-id="71324-249">Обычно это свойство следует задать в  *\_AppStart* страницы.</span><span class="sxs-lookup"><span data-stu-id="71324-249">Normally you should set this property in the *\_AppStart* page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample106.html)]

<a id="Validation"></a>
## <a name="validation"></a><span data-ttu-id="71324-250">Проверка</span><span class="sxs-lookup"><span data-stu-id="71324-250">Validation</span></span>

### `Html.ValidationMessage(field)`

<span data-ttu-id="71324-251">(v2) Отображает сообщение об ошибке проверки для указанного поля.</span><span class="sxs-lookup"><span data-stu-id="71324-251">(v2) Renders a validation error message for the specified field.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample107.cshtml)]

### `Html.ValidationSummary([message])`

<span data-ttu-id="71324-252">(v2) Отображает список всех ошибок проверки.</span><span class="sxs-lookup"><span data-stu-id="71324-252">(v2) Displays a list of all validation errors.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample108.cshtml)]

### `Validation.Add(field, validationType)`

<span data-ttu-id="71324-253">(v2) Регистрирует элемент ввода для указанного типа проверки.</span><span class="sxs-lookup"><span data-stu-id="71324-253">(v2) Registers a user input element for the specified type of validation.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample109.js)]

### `Validation.ClassFor(field)`

<span data-ttu-id="71324-254">(v2) Динамически подготавливает к просмотру атрибутов класса CSS для проверки на стороне клиента, что можно форматировать сообщения об ошибках проверки.</span><span class="sxs-lookup"><span data-stu-id="71324-254">(v2) Dynamically renders CSS class attributes for client-side validation so that you can format validation error messages.</span></span> <span data-ttu-id="71324-255">(Требуется ссылаться на библиотеки соответствующие клиентских скриптов и определении классов CSS).</span><span class="sxs-lookup"><span data-stu-id="71324-255">(Requires that you reference the appropriate client-script libraries and that you define CSS classes.)</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample110.html)]

### `Validation.For(field)`

<span data-ttu-id="71324-256">(v2) Включает проверку на стороне клиента для поля ввода пользователя.</span><span class="sxs-lookup"><span data-stu-id="71324-256">(v2) Enables client-side validation for the user input field.</span></span> <span data-ttu-id="71324-257">(Требуется ссылаться на библиотеки соответствующего клиентского скрипта).</span><span class="sxs-lookup"><span data-stu-id="71324-257">(Requires that you reference the appropriate client-script libraries.)</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample111.html)]

### `Validation.IsValid()`

<span data-ttu-id="71324-258">(v2) Возвращает значение true, если все элементы пользовательского ввода, зарегистрированного для проверки содержать допустимые значения.</span><span class="sxs-lookup"><span data-stu-id="71324-258">(v2) Returns true if all user input elements that are registred for validation contain valid values.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample112.cs)]

### `Validation.RequireField(field[, errorMessage])`

<span data-ttu-id="71324-259">(v2) Указывает, что пользователям необходимо предоставить значение для элемента ввода.</span><span class="sxs-lookup"><span data-stu-id="71324-259">(v2) Specifies that users must provide a value for the user input element.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample113.cs)]

### `Validation.RequireFields(field1[, field12, field3, ...])`

<span data-ttu-id="71324-260">(v2) Указывает, что пользователям необходимо предоставить значения для каждого элемента ввода пользователя.</span><span class="sxs-lookup"><span data-stu-id="71324-260">(v2) Specifies that users must provide values for each of the user input elements.</span></span> <span data-ttu-id="71324-261">Этот метод не позволяет задать пользовательское сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="71324-261">This method does not let you specify a custom error message.</span></span>

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

<span data-ttu-id="71324-262">(v2) Задает проверку при использовании `Validation.Add` метода.</span><span class="sxs-lookup"><span data-stu-id="71324-262">(v2) Specifies a validation test when you use the `Validation.Add` method.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample115.js)]
