---
title: Тестирование веб-API с помощью HTTP REPL
author: scottaddie
description: Узнайте, как использовать глобальное средство .NET Core HTTP REPL для просмотра и тестирования веб-API ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 08/29/2019
uid: web-api/http-repl
ms.openlocfilehash: 086ac141a04ab4a560f2c26fb049ef8a5493dc97
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187242"
---
# <a name="test-web-apis-with-the-http-repl"></a><span data-ttu-id="29ad0-103">Тестирование веб-API с помощью HTTP REPL</span><span class="sxs-lookup"><span data-stu-id="29ad0-103">Test web APIs with the HTTP REPL</span></span>

<span data-ttu-id="29ad0-104">Автор: [Скотт Адди](https://twitter.com/Scott_Addie) (Scott Addie)</span><span class="sxs-lookup"><span data-stu-id="29ad0-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="29ad0-105">HTTP read-eval-print loop (REPL):</span><span class="sxs-lookup"><span data-stu-id="29ad0-105">The HTTP Read-Eval-Print Loop (REPL) is:</span></span>

* <span data-ttu-id="29ad0-106">это кроссплатформенная программа командной строки, которая поддерживается везде, где поддерживается .NET Core;</span><span class="sxs-lookup"><span data-stu-id="29ad0-106">A lightweight, cross-platform command-line tool that's supported everywhere .NET Core is supported.</span></span>
* <span data-ttu-id="29ad0-107">служит для создания HTTP-запросов с целью тестирования веб-API ASP.NET Core (а также веб-API, не связанных с ASP.NET Core) и просмотра их результатов;</span><span class="sxs-lookup"><span data-stu-id="29ad0-107">Used for making HTTP requests to test ASP.NET Core web APIs (and non-ASP.NET Core web APIs) and view their results.</span></span>
* <span data-ttu-id="29ad0-108">может использоваться для тестирования веб-API, размещенных в любой среде, включая localhost и Службу приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="29ad0-108">Capable of testing web APIs hosted in any environment, including localhost and Azure App Service.</span></span>

<span data-ttu-id="29ad0-109">Поддерживаются следующие [HTTP-команды](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods):</span><span class="sxs-lookup"><span data-stu-id="29ad0-109">The following [HTTP verbs](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods) are supported:</span></span>

* [<span data-ttu-id="29ad0-110">DELETE</span><span class="sxs-lookup"><span data-stu-id="29ad0-110">DELETE</span></span>](#test-http-delete-requests)
* [<span data-ttu-id="29ad0-111">GET</span><span class="sxs-lookup"><span data-stu-id="29ad0-111">GET</span></span>](#test-http-get-requests)
* [<span data-ttu-id="29ad0-112">HEAD</span><span class="sxs-lookup"><span data-stu-id="29ad0-112">HEAD</span></span>](#test-http-head-requests)
* [<span data-ttu-id="29ad0-113">OPTIONS</span><span class="sxs-lookup"><span data-stu-id="29ad0-113">OPTIONS</span></span>](#test-http-options-requests)
* [<span data-ttu-id="29ad0-114">PATCH</span><span class="sxs-lookup"><span data-stu-id="29ad0-114">PATCH</span></span>](#test-http-patch-requests)
* [<span data-ttu-id="29ad0-115">POST</span><span class="sxs-lookup"><span data-stu-id="29ad0-115">POST</span></span>](#test-http-post-requests)
* [<span data-ttu-id="29ad0-116">PUT</span><span class="sxs-lookup"><span data-stu-id="29ad0-116">PUT</span></span>](#test-http-put-requests)

<span data-ttu-id="29ad0-117">Для выполнения дальнейших инструкций [просмотрите или скачайте пример веб-API ASP.NET Core](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([как скачивать](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="29ad0-117">To follow along, [view or download the sample ASP.NET Core web API](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="29ad0-118">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="29ad0-118">Prerequisites</span></span>

* [!INCLUDE [2.1-SDK](~/includes/2.1-SDK.md)]

## <a name="installation"></a><span data-ttu-id="29ad0-119">Установка</span><span class="sxs-lookup"><span data-stu-id="29ad0-119">Installation</span></span>

<span data-ttu-id="29ad0-120">Чтобы установить HTTP REPL, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="29ad0-120">To install the HTTP REPL, run the following command:</span></span>

```dotnetcli
dotnet tool install -g Microsoft.dotnet-httprepl
```

<span data-ttu-id="29ad0-121">[Глобальное средство .NET Core](/dotnet/core/tools/global-tools#install-a-global-tool) устанавливается из пакета NuGet [Microsoft.dotnet-httprepl](https://www.nuget.org/packages/Microsoft.dotnet-httprepl).</span><span class="sxs-lookup"><span data-stu-id="29ad0-121">A [.NET Core Global Tool](/dotnet/core/tools/global-tools#install-a-global-tool) is installed from the [Microsoft.dotnet-httprepl](https://www.nuget.org/packages/Microsoft.dotnet-httprepl) NuGet package.</span></span>

## <a name="usage"></a><span data-ttu-id="29ad0-122">Использование</span><span class="sxs-lookup"><span data-stu-id="29ad0-122">Usage</span></span>

<span data-ttu-id="29ad0-123">После успешной установки HTTP REPL выполните следующую команду, чтобы запустить средство:</span><span class="sxs-lookup"><span data-stu-id="29ad0-123">After successful installation of the tool, run the following command to start the HTTP REPL:</span></span>

```console
httprepl
```

<span data-ttu-id="29ad0-124">Чтобы просмотреть доступные команды HTTP REPL, выполните одну из следующих команд:</span><span class="sxs-lookup"><span data-stu-id="29ad0-124">To view the available HTTP REPL commands, run one of the following commands:</span></span>

```console
httprepl -h
```

```console
httprepl --help
```

<span data-ttu-id="29ad0-125">Выводится следующий результат.</span><span class="sxs-lookup"><span data-stu-id="29ad0-125">The following output is displayed:</span></span>

```console
Usage:
  httprepl [<BASE_ADDRESS>] [options]

Arguments:
  <BASE_ADDRESS> - The initial base address for the REPL.

Options:
  -h|--help - Show help information.

Once the REPL starts, these commands are valid:

Setup Commands:
Use these commands to configure the tool for your API server

connect        Configures the directory structure and base address of the api server
set header     Sets or clears a header for all requests. e.g. `set header content-type application/json`

HTTP Commands:
Use these commands to execute requests against your application.

GET            get - Issues a GET request
POST           post - Issues a POST request
PUT            put - Issues a PUT request
DELETE         delete - Issues a DELETE request
PATCH          patch - Issues a PATCH request
HEAD           head - Issues a HEAD request
OPTIONS        options - Issues a OPTIONS request

Navigation Commands:
The REPL allows you to navigate your URL space and focus on specific APIs that you are working on.

set base       Set the base URI. e.g. `set base http://locahost:5000`
ls             Show all endpoints for the current path
cd             Append the given directory to the currently selected path, or move up a path when using `cd ..`

Shell Commands:
Use these commands to interact with the REPL shell.

clear          Removes all text from the shell
echo [on/off]  Turns request echoing on or off, show the request that was made when using request commands
exit           Exit the shell

REPL Customization Commands:
Use these commands to customize the REPL behavior.

pref [get/set] Allows viewing or changing preferences, e.g. 'pref set editor.command.default 'C:\\Program Files\\Microsoft VS Code\\Code.exe'`
run            Runs the script at the given path. A script is a set of commands that can be typed with one command per line
ui             Displays the Swagger UI page, if available, in the default browser

Use `help <COMMAND>` for more detail on an individual command. e.g. `help get`.
For detailed tool info, see https://aka.ms/http-repl-doc.
```

<span data-ttu-id="29ad0-126">В HTTP REPL поддерживается завершение команд.</span><span class="sxs-lookup"><span data-stu-id="29ad0-126">The HTTP REPL offers command completion.</span></span> <span data-ttu-id="29ad0-127">Нажимая клавишу <kbd>TAB</kbd>, можно переходить по списку команд, которые начинаются с введенных символов или конечной точки API.</span><span class="sxs-lookup"><span data-stu-id="29ad0-127">Pressing the <kbd>Tab</kbd> key iterates through the list of commands that complete the characters or API endpoint that you typed.</span></span> <span data-ttu-id="29ad0-128">Доступные команды CLI описываются в следующих разделах.</span><span class="sxs-lookup"><span data-stu-id="29ad0-128">The following sections outline the available CLI commands.</span></span>

## <a name="connect-to-the-web-api"></a><span data-ttu-id="29ad0-129">Подключение к веб-API</span><span class="sxs-lookup"><span data-stu-id="29ad0-129">Connect to the web API</span></span>

<span data-ttu-id="29ad0-130">Чтобы подключиться к веб-API, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="29ad0-130">Connect to a web API by running the following command:</span></span>

```console
httprepl <ROOT URI>
```

<span data-ttu-id="29ad0-131">`<ROOT URI>` — это базовый универсальный код ресурса (URI) для веб-API.</span><span class="sxs-lookup"><span data-stu-id="29ad0-131">`<ROOT URI>` is the base URI for the web API.</span></span> <span data-ttu-id="29ad0-132">Например:</span><span class="sxs-lookup"><span data-stu-id="29ad0-132">For example:</span></span>

```console
httprepl https://localhost:5001
```

<span data-ttu-id="29ad0-133">Кроме того, когда программа HTTP REPL запущена, можно в любой момент выполнить следующую команду:</span><span class="sxs-lookup"><span data-stu-id="29ad0-133">Alternatively, run the following command at any time while the HTTP REPL is running:</span></span>

```console
connect <ROOT URI>
```

<span data-ttu-id="29ad0-134">Например:</span><span class="sxs-lookup"><span data-stu-id="29ad0-134">For example:</span></span>

```console
(Disconnected)~ connect https://localhost:5001
```

## <a name="manually-point-to-the-swagger-document-for-the-web-api"></a><span data-ttu-id="29ad0-135">Создание ссылки на документ Swagger для веб-API вручную</span><span class="sxs-lookup"><span data-stu-id="29ad0-135">Manually point to the Swagger document for the web API</span></span>

<span data-ttu-id="29ad0-136">Приведенная выше команда для подключения попытается автоматически найти документ Swagger.</span><span class="sxs-lookup"><span data-stu-id="29ad0-136">The connect command above will attempt to find the Swagger document automatically.</span></span> <span data-ttu-id="29ad0-137">Если по какой-то причине ей не удается это сделать, укажите для веб-API универсальный код ресурса (URI) для документа Swagger, используя параметр `--swagger`:</span><span class="sxs-lookup"><span data-stu-id="29ad0-137">If for some reason it is unable to do so, you can specify the URI of the Swagger document for the web API by using the `--swagger` option:</span></span>

```console
connect <ROOT URI> --swagger <SWAGGER URI>
```

<span data-ttu-id="29ad0-138">Например:</span><span class="sxs-lookup"><span data-stu-id="29ad0-138">For example:</span></span>

```console
(Disconnected)~ connect https://localhost:5001 --swagger /swagger/v1/swagger.json
```

## <a name="navigate-the-web-api"></a><span data-ttu-id="29ad0-139">Просмотр веб-API</span><span class="sxs-lookup"><span data-stu-id="29ad0-139">Navigate the web API</span></span>

### <a name="view-available-endpoints"></a><span data-ttu-id="29ad0-140">Просмотр доступных конечных точек</span><span class="sxs-lookup"><span data-stu-id="29ad0-140">View available endpoints</span></span>

<span data-ttu-id="29ad0-141">Чтобы получить список конечных точек (контроллеров) по текущему пути веб-API, выполните команду `ls` или `dir`:</span><span class="sxs-lookup"><span data-stu-id="29ad0-141">To list the different endpoints (controllers) at the current path of the web API address, run the `ls` or `dir` command:</span></span>

```console
https://localhot:5001/~ ls
```

<span data-ttu-id="29ad0-142">Будут выведены данные в следующем формате:</span><span class="sxs-lookup"><span data-stu-id="29ad0-142">The following output format is displayed:</span></span>

```console
.        []
Fruits   [get|post]
People   [get|post]

https://localhost:5001/~
```

<span data-ttu-id="29ad0-143">Из приведенных выше результатов видно, что доступно два контроллера: `Fruits` и `People`.</span><span class="sxs-lookup"><span data-stu-id="29ad0-143">The preceding output indicates that there are two controllers available: `Fruits` and `People`.</span></span> <span data-ttu-id="29ad0-144">Они оба поддерживают операции HTTP GET и POST без параметров.</span><span class="sxs-lookup"><span data-stu-id="29ad0-144">Both controllers support parameterless HTTP GET and POST operations.</span></span>

<span data-ttu-id="29ad0-145">Более подробную информацию можно получить, перейдя к определенному контроллеру.</span><span class="sxs-lookup"><span data-stu-id="29ad0-145">Navigating into a specific controller reveals more detail.</span></span> <span data-ttu-id="29ad0-146">Например, из выходных данных приведенной ниже команды видно, что контроллер `Fruits` также поддерживает операции HTTP GET, PUT и DELETE.</span><span class="sxs-lookup"><span data-stu-id="29ad0-146">For example, the following command's output shows the `Fruits` controller also supports HTTP GET, PUT, and DELETE operations.</span></span> <span data-ttu-id="29ad0-147">Каждая из этих операций принимает параметр `id` в маршруте:</span><span class="sxs-lookup"><span data-stu-id="29ad0-147">Each of these operations expects an `id` parameter in the route:</span></span>

```console
https://localhost:5001/fruits~ ls
.      [get|post]
..     []
{id}   [get|put|delete]

https://localhost:5001/fruits~
```

<span data-ttu-id="29ad0-148">Кроме того, можно выполнить команду `ui`, чтобы открыть страницу с пользовательским интерфейсом Swagger веб-API в браузере.</span><span class="sxs-lookup"><span data-stu-id="29ad0-148">Alternatively, run the `ui` command to open the web API's Swagger UI page in a browser.</span></span> <span data-ttu-id="29ad0-149">Например:</span><span class="sxs-lookup"><span data-stu-id="29ad0-149">For example:</span></span>

```console
https://localhost:5001/~ ui
```

### <a name="navigate-to-an-endpoint"></a><span data-ttu-id="29ad0-150">Переход к конечной точке</span><span class="sxs-lookup"><span data-stu-id="29ad0-150">Navigate to an endpoint</span></span>

<span data-ttu-id="29ad0-151">Чтобы перейти к другой конечной точке веб-API, выполните команду `cd`:</span><span class="sxs-lookup"><span data-stu-id="29ad0-151">To navigate to a different endpoint on the web API, run the `cd` command:</span></span>

```console
https://localhost:5001/~ cd people
```

<span data-ttu-id="29ad0-152">В пути, указываемом после команды `cd`, регистр символов не учитывается.</span><span class="sxs-lookup"><span data-stu-id="29ad0-152">The path following the `cd` command is case insensitive.</span></span> <span data-ttu-id="29ad0-153">Будут выведены данные в следующем формате:</span><span class="sxs-lookup"><span data-stu-id="29ad0-153">The following output format is displayed:</span></span>

```console
/people    [get|post]

https://localhost:5001/people~
```

## <a name="customize-the-http-repl"></a><span data-ttu-id="29ad0-154">Настройка HTTP REPL</span><span class="sxs-lookup"><span data-stu-id="29ad0-154">Customize the HTTP REPL</span></span>

<span data-ttu-id="29ad0-155">[Цвета](#set-color-preferences), используемые в HTTP REPL по умолчанию, можно настроить.</span><span class="sxs-lookup"><span data-stu-id="29ad0-155">The HTTP REPL's default [colors](#set-color-preferences) can be customized.</span></span> <span data-ttu-id="29ad0-156">Кроме того, можно определить [текстовый редактор по умолчанию](#set-the-default-text-editor).</span><span class="sxs-lookup"><span data-stu-id="29ad0-156">Additionally, a [default text editor](#set-the-default-text-editor) can be defined.</span></span> <span data-ttu-id="29ad0-157">Настройки HTTP REPL сохраняются на протяжении текущего сеанса и учитываются при открытии следующих сеансов.</span><span class="sxs-lookup"><span data-stu-id="29ad0-157">The HTTP REPL preferences are persisted across the current session and are honored in future sessions.</span></span> <span data-ttu-id="29ad0-158">Измененные настройки хранятся в следующем файле:</span><span class="sxs-lookup"><span data-stu-id="29ad0-158">Once modified, the preferences are stored in the following file:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="29ad0-159">Linux</span><span class="sxs-lookup"><span data-stu-id="29ad0-159">Linux</span></span>](#tab/linux)

<span data-ttu-id="29ad0-160">*%HOME%/.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="29ad0-160">*%HOME%/.httpreplprefs*</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="29ad0-161">macOS</span><span class="sxs-lookup"><span data-stu-id="29ad0-161">macOS</span></span>](#tab/macos)

<span data-ttu-id="29ad0-162">*%HOME%/.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="29ad0-162">*%HOME%/.httpreplprefs*</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="29ad0-163">Windows</span><span class="sxs-lookup"><span data-stu-id="29ad0-163">Windows</span></span>](#tab/windows)

<span data-ttu-id="29ad0-164">*%USERPROFILE%\\.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="29ad0-164">*%USERPROFILE%\\.httpreplprefs*</span></span>

---

<span data-ttu-id="29ad0-165">Файл *HTTPREPLPREFS* загружается при запуске. Во время выполнения изменения в нем не отслеживаются.</span><span class="sxs-lookup"><span data-stu-id="29ad0-165">The *.httpreplprefs* file is loaded on startup and not monitored for changes at runtime.</span></span> <span data-ttu-id="29ad0-166">Изменения, внесенные в него вручную, вступают в силу только после перезапуска средства.</span><span class="sxs-lookup"><span data-stu-id="29ad0-166">Manual modifications to the file take effect only after restarting the tool.</span></span>

### <a name="view-the-settings"></a><span data-ttu-id="29ad0-167">Просмотр параметров</span><span class="sxs-lookup"><span data-stu-id="29ad0-167">View the settings</span></span>

<span data-ttu-id="29ad0-168">Чтобы просмотреть доступные параметры, выполните команду `pref get`.</span><span class="sxs-lookup"><span data-stu-id="29ad0-168">To view the available settings, run the `pref get` command.</span></span> <span data-ttu-id="29ad0-169">Например:</span><span class="sxs-lookup"><span data-stu-id="29ad0-169">For example:</span></span>

```console
https://localhost:5001/~ pref get
```

<span data-ttu-id="29ad0-170">В результате выполнения приведенной выше команды выводятся доступные пары "ключ-значение":</span><span class="sxs-lookup"><span data-stu-id="29ad0-170">The preceding command displays the available key-value pairs:</span></span>

```console
colors.json=Green
colors.json.arrayBrace=BoldCyan
colors.json.comma=BoldYellow
colors.json.name=BoldMagenta
colors.json.nameSeparator=BoldWhite
colors.json.objectBrace=Cyan
colors.protocol=BoldGreen
colors.status=BoldYellow
```

### <a name="set-color-preferences"></a><span data-ttu-id="29ad0-171">Настройки цветов</span><span class="sxs-lookup"><span data-stu-id="29ad0-171">Set color preferences</span></span>

<span data-ttu-id="29ad0-172">Раскрашивание ответов в настоящее время поддерживается только для JSON.</span><span class="sxs-lookup"><span data-stu-id="29ad0-172">Response colorization is currently supported for JSON only.</span></span> <span data-ttu-id="29ad0-173">Чтобы настроить цвета по умолчанию в программе HTTP REPL, найдите ключ, соответствующий изменяемому цвету.</span><span class="sxs-lookup"><span data-stu-id="29ad0-173">To customize the default HTTP REPL tool coloring, locate the key corresponding to the color to be changed.</span></span> <span data-ttu-id="29ad0-174">Инструкции по поиску ключей см. в разделе [Просмотр параметров](#view-the-settings).</span><span class="sxs-lookup"><span data-stu-id="29ad0-174">For instructions on how to find the keys, see the [View the settings](#view-the-settings) section.</span></span> <span data-ttu-id="29ad0-175">Например, измените значение ключа `colors.json` с `Green` на `White` следующим образом:</span><span class="sxs-lookup"><span data-stu-id="29ad0-175">For example, change the `colors.json` key value from `Green` to `White` as follows:</span></span>

```console
https://localhost:5001/people~ pref set colors.json White
```

<span data-ttu-id="29ad0-176">Можно использовать только [допустимые цвета](https://github.com/aspnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs).</span><span class="sxs-lookup"><span data-stu-id="29ad0-176">Only the [allowed colors](https://github.com/aspnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs) may be used.</span></span> <span data-ttu-id="29ad0-177">При выполнении последующих HTTP-запросов к выходным данным будут применяться новые цвета.</span><span class="sxs-lookup"><span data-stu-id="29ad0-177">Subsequent HTTP requests display output with the new coloring.</span></span>

<span data-ttu-id="29ad0-178">Если определенные цветовые ключи не заданы, используются более общие ключи.</span><span class="sxs-lookup"><span data-stu-id="29ad0-178">When specific color keys aren't set, more generic keys are considered.</span></span> <span data-ttu-id="29ad0-179">Рассмотрим это поведение на следующем примере:</span><span class="sxs-lookup"><span data-stu-id="29ad0-179">To demonstrate this fallback behavior, consider the following example:</span></span>

* <span data-ttu-id="29ad0-180">Если у `colors.json.name` нет значения, используется `colors.json.string`.</span><span class="sxs-lookup"><span data-stu-id="29ad0-180">If `colors.json.name` doesn't have a value, `colors.json.string` is used.</span></span>
* <span data-ttu-id="29ad0-181">Если у `colors.json.string` нет значения, используется `colors.json.literal`.</span><span class="sxs-lookup"><span data-stu-id="29ad0-181">If `colors.json.string` doesn't have a value, `colors.json.literal` is used.</span></span>
* <span data-ttu-id="29ad0-182">Если у `colors.json.literal` нет значения, используется `colors.json`.</span><span class="sxs-lookup"><span data-stu-id="29ad0-182">If `colors.json.literal` doesn't have a value, `colors.json` is used.</span></span> 
* <span data-ttu-id="29ad0-183">Если у `colors.json` нет значения, используется текст цвета по умолчанию для командной оболочки (`AllowedColors.None`).</span><span class="sxs-lookup"><span data-stu-id="29ad0-183">If `colors.json` doesn't have a value, the command shell's default text color (`AllowedColors.None`) is used.</span></span>

### <a name="set-indentation-size"></a><span data-ttu-id="29ad0-184">Установка размера отступа</span><span class="sxs-lookup"><span data-stu-id="29ad0-184">Set indentation size</span></span>

<span data-ttu-id="29ad0-185">Настройка размера отступа в ответах в настоящее время поддерживается только для JSON.</span><span class="sxs-lookup"><span data-stu-id="29ad0-185">Response indentation size customization is currently supported for JSON only.</span></span> <span data-ttu-id="29ad0-186">Размер по умолчанию — два пробела.</span><span class="sxs-lookup"><span data-stu-id="29ad0-186">The default size is two spaces.</span></span> <span data-ttu-id="29ad0-187">Например:</span><span class="sxs-lookup"><span data-stu-id="29ad0-187">For example:</span></span>

```json
[
  {
    "id": 1,
    "name": "Apple"
  },
  {
    "id": 2,
    "name": "Orange"
  },
  {
    "id": 3,
    "name": "Strawberry"
  }
]
```

<span data-ttu-id="29ad0-188">Чтобы изменить размер по умолчанию, задайте ключ `formatting.json.indentSize`.</span><span class="sxs-lookup"><span data-stu-id="29ad0-188">To change the default size, set the `formatting.json.indentSize` key.</span></span> <span data-ttu-id="29ad0-189">Например, чтобы использовались четыре пробела, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="29ad0-189">For example, to always use four spaces:</span></span>

```console
pref set formatting.json.indentSize 4
```

<span data-ttu-id="29ad0-190">В последующих ответах будет применяться отступ в четыре пробела:</span><span class="sxs-lookup"><span data-stu-id="29ad0-190">Subsequent responses honor the setting of four spaces:</span></span>

```json
[
    {
        "id": 1,
        "name": "Apple"
    },
    {
        "id": 2,
        "name": "Orange"
    },
    {
        "id": 3,
        "name": "Strawberry"
    }
]
```

### <a name="set-the-default-text-editor"></a><span data-ttu-id="29ad0-191">Установка текстового редактора по умолчанию</span><span class="sxs-lookup"><span data-stu-id="29ad0-191">Set the default text editor</span></span>

<span data-ttu-id="29ad0-192">По умолчанию текстовый редактор для HTTP REPL не настроен.</span><span class="sxs-lookup"><span data-stu-id="29ad0-192">By default, the HTTP REPL has no text editor configured for use.</span></span> <span data-ttu-id="29ad0-193">Для тестирования методов веб-API, требующих текста HTTP-запроса, необходимо задать текстовый редактор по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="29ad0-193">To test web API methods requiring an HTTP request body, a default text editor must be set.</span></span> <span data-ttu-id="29ad0-194">Средство HTTP REPL запускает настроенный текстовый редактор исключительно в целях составления текста запроса.</span><span class="sxs-lookup"><span data-stu-id="29ad0-194">The HTTP REPL tool launches the configured text editor for the sole purpose of composing the request body.</span></span> <span data-ttu-id="29ad0-195">Чтобы указать текстовый редактор по умолчанию, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="29ad0-195">Run the following command to set your preferred text editor as the default:</span></span>

```console
pref set editor.command.default "<EXECUTABLE>"
```

<span data-ttu-id="29ad0-196">В приведенной выше команде `<EXECUTABLE>` — это полный путь к исполняемому файлу текстового редактора.</span><span class="sxs-lookup"><span data-stu-id="29ad0-196">In the preceding command, `<EXECUTABLE>` is the full path to the text editor's executable file.</span></span> <span data-ttu-id="29ad0-197">Например, чтобы задать Visual Studio Code как текстовый редактор по умолчанию, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="29ad0-197">For example, run the following command to set Visual Studio Code as the default text editor:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="29ad0-198">Linux</span><span class="sxs-lookup"><span data-stu-id="29ad0-198">Linux</span></span>](#tab/linux)

```console
pref set editor.command.default "/usr/bin/code"
```

# <a name="macostabmacos"></a>[<span data-ttu-id="29ad0-199">macOS</span><span class="sxs-lookup"><span data-stu-id="29ad0-199">macOS</span></span>](#tab/macos)

```console
pref set editor.command.default "/Applications/Visual Studio Code.app/Contents/Resources/app/bin/code"
```

# <a name="windowstabwindows"></a>[<span data-ttu-id="29ad0-200">Windows</span><span class="sxs-lookup"><span data-stu-id="29ad0-200">Windows</span></span>](#tab/windows)

```console
pref set editor.command.default "C:\Program Files\Microsoft VS Code\Code.exe"
```

---

<span data-ttu-id="29ad0-201">Чтобы текстовый редактор по умолчанию запускался с определенными аргументами CLI, задайте ключ `editor.command.default.arguments`.</span><span class="sxs-lookup"><span data-stu-id="29ad0-201">To launch the default text editor with specific CLI arguments, set the `editor.command.default.arguments` key.</span></span> <span data-ttu-id="29ad0-202">Например, предположим, что Visual Studio Code — это текстовый редактор по умолчанию и необходимо, чтобы средство HTTP REPL всегда запускало сеанс Visual Studio Code с отключенными расширениями.</span><span class="sxs-lookup"><span data-stu-id="29ad0-202">For example, assume Visual Studio Code is the default text editor and that you always want the HTTP REPL to open Visual Studio Code in a new session with extensions disabled.</span></span> <span data-ttu-id="29ad0-203">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="29ad0-203">Run the following command:</span></span>

```console
pref set editor.command.default.arguments "--disable-extensions --new-window"
```

### <a name="set-the-swagger-search-paths"></a><span data-ttu-id="29ad0-204">Настройка путей поиска Swagger</span><span class="sxs-lookup"><span data-stu-id="29ad0-204">Set the Swagger search paths</span></span>

<span data-ttu-id="29ad0-205">По умолчанию REPL HTTP имеет набор относительных путей для поиска документа Swagger при выполнении команды `connect` без параметра `--swagger`.</span><span class="sxs-lookup"><span data-stu-id="29ad0-205">By default, the HTTP REPL has a set of relative paths that it uses to find the Swagger document when executing the `connect` command without the `--swagger` option.</span></span> <span data-ttu-id="29ad0-206">Эти относительные пути объединяются с корневыми и базовыми путями, указанными в команде `connect`.</span><span class="sxs-lookup"><span data-stu-id="29ad0-206">These relative paths are combined with the root and base paths specified in the `connect` command.</span></span> <span data-ttu-id="29ad0-207">Относительные пути по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="29ad0-207">The default relative paths are:</span></span>

- <span data-ttu-id="29ad0-208">*swagger.json*</span><span class="sxs-lookup"><span data-stu-id="29ad0-208">*swagger.json*</span></span>
- <span data-ttu-id="29ad0-209">*swagger/v1/swagger.json*</span><span class="sxs-lookup"><span data-stu-id="29ad0-209">*swagger/v1/swagger.json*</span></span>
- <span data-ttu-id="29ad0-210">*/swagger.json*</span><span class="sxs-lookup"><span data-stu-id="29ad0-210">*/swagger.json*</span></span>
- <span data-ttu-id="29ad0-211">*/swagger/v1/swagger.json*</span><span class="sxs-lookup"><span data-stu-id="29ad0-211">*/swagger/v1/swagger.json*</span></span>

<span data-ttu-id="29ad0-212">Чтобы использовать другой набор путей поиска в среде, используйте параметр `swagger.searchPaths`.</span><span class="sxs-lookup"><span data-stu-id="29ad0-212">To use a different set of search paths in your environment, set the `swagger.searchPaths` preference.</span></span> <span data-ttu-id="29ad0-213">Значение должно представлять собой список относительных путей, разделенных символом вертикальной черты.</span><span class="sxs-lookup"><span data-stu-id="29ad0-213">The value must be a pipe-delimited list of relative paths.</span></span> <span data-ttu-id="29ad0-214">Например:</span><span class="sxs-lookup"><span data-stu-id="29ad0-214">For example:</span></span>

```console
pref set swagger.searchPaths "swagger/v2/swagger.json|swagger/v3/swagger.json"
```

## <a name="test-http-get-requests"></a><span data-ttu-id="29ad0-215">Тестирование HTTP-запросов GET</span><span class="sxs-lookup"><span data-stu-id="29ad0-215">Test HTTP GET requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="29ad0-216">Краткий обзор</span><span class="sxs-lookup"><span data-stu-id="29ad0-216">Synopsis</span></span>

```console
get <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="29ad0-217">Аргументы</span><span class="sxs-lookup"><span data-stu-id="29ad0-217">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="29ad0-218">Параметр маршрута, который требуется методу действия соответствующего контроллера.</span><span class="sxs-lookup"><span data-stu-id="29ad0-218">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="29ad0-219">Параметры</span><span class="sxs-lookup"><span data-stu-id="29ad0-219">Options</span></span>

<span data-ttu-id="29ad0-220">Для команды `get` доступны следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="29ad0-220">The following options are available for the `get` command:</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="29ad0-221">Пример</span><span class="sxs-lookup"><span data-stu-id="29ad0-221">Example</span></span>

<span data-ttu-id="29ad0-222">Чтобы отправить HTTP-запрос GET, выполните указанные ниже действия.</span><span class="sxs-lookup"><span data-stu-id="29ad0-222">To issue an HTTP GET request:</span></span>

1. <span data-ttu-id="29ad0-223">Выполните команду `get` в конечной точке, которая поддерживает ее:</span><span class="sxs-lookup"><span data-stu-id="29ad0-223">Run the `get` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ get
    ```

    <span data-ttu-id="29ad0-224">Выходные данные приведенной выше команды имеют следующий формат:</span><span class="sxs-lookup"><span data-stu-id="29ad0-224">The preceding command displays the following output format:</span></span>

    ```console
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Fri, 21 Jun 2019 03:38:45 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 1,
        "name": "Scott Hunter"
      },
      {
        "id": 2,
        "name": "Scott Hanselman"
      },
      {
        "id": 3,
        "name": "Scott Guthrie"
      }
    ]


    https://localhost:5001/people~
    ```

1. <span data-ttu-id="29ad0-225">Получите определенную запись, передав параметр в команду `get`:</span><span class="sxs-lookup"><span data-stu-id="29ad0-225">Retrieve a specific record by passing a parameter to the `get` command:</span></span>

    ```console
    https://localhost:5001/people~ get 2
    ```

    <span data-ttu-id="29ad0-226">Выходные данные приведенной выше команды имеют следующий формат:</span><span class="sxs-lookup"><span data-stu-id="29ad0-226">The preceding command displays the following output format:</span></span>

    ```console
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Fri, 21 Jun 2019 06:17:57 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 2,
        "name": "Scott Hanselman"
      }
    ]


    https://localhost:5001/people~
    ```

## <a name="test-http-post-requests"></a><span data-ttu-id="29ad0-227">Тестирование HTTP-запросов POST</span><span class="sxs-lookup"><span data-stu-id="29ad0-227">Test HTTP POST requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="29ad0-228">Краткий обзор</span><span class="sxs-lookup"><span data-stu-id="29ad0-228">Synopsis</span></span>

```console
post <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="29ad0-229">Аргументы</span><span class="sxs-lookup"><span data-stu-id="29ad0-229">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="29ad0-230">Параметр маршрута, который требуется методу действия соответствующего контроллера.</span><span class="sxs-lookup"><span data-stu-id="29ad0-230">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="29ad0-231">Параметры</span><span class="sxs-lookup"><span data-stu-id="29ad0-231">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="29ad0-232">Пример</span><span class="sxs-lookup"><span data-stu-id="29ad0-232">Example</span></span>

<span data-ttu-id="29ad0-233">Чтобы отправить HTTP-запрос POST, выполните указанные ниже действия.</span><span class="sxs-lookup"><span data-stu-id="29ad0-233">To issue an HTTP POST request:</span></span>

1. <span data-ttu-id="29ad0-234">Выполните команду `post` в конечной точке, которая поддерживает ее:</span><span class="sxs-lookup"><span data-stu-id="29ad0-234">Run the `post` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ post -h Content-Type=application/json
    ```

    <span data-ttu-id="29ad0-235">В приведенной выше команде в заголовке HTTP-запроса `Content-Type` указан формат текста запроса JSON.</span><span class="sxs-lookup"><span data-stu-id="29ad0-235">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="29ad0-236">В текстовом редакторе по умолчанию открывается файл *TMP* с шаблоном JSON, представляющим текст HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="29ad0-236">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="29ad0-237">Например:</span><span class="sxs-lookup"><span data-stu-id="29ad0-237">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="29ad0-238">Инструкции по определению текстового редактора по умолчанию см. в разделе [Установка текстового редактора по умолчанию](#set-the-default-text-editor).</span><span class="sxs-lookup"><span data-stu-id="29ad0-238">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="29ad0-239">Измените шаблон JSON в соответствии с требованиями к проверке модели:</span><span class="sxs-lookup"><span data-stu-id="29ad0-239">Modify the JSON template to satisfy model validation requirements:</span></span>

    ```json
    {
      "id": 0,
      "name": "Scott Addie"
    }
    ```

1. <span data-ttu-id="29ad0-240">Сохраните файл *TMP* и закройте текстовый редактор.</span><span class="sxs-lookup"><span data-stu-id="29ad0-240">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="29ad0-241">В командной оболочке появятся следующие выходные данные:</span><span class="sxs-lookup"><span data-stu-id="29ad0-241">The following output appears in the command shell:</span></span>

    ```console
    HTTP/1.1 201 Created
    Content-Type: application/json; charset=utf-8
    Date: Thu, 27 Jun 2019 21:24:18 GMT
    Location: https://localhost:5001/people/4
    Server: Kestrel
    Transfer-Encoding: chunked

    {
      "id": 4,
      "name": "Scott Addie"
    }


    https://localhost:5001/people~
    ```

## <a name="test-http-put-requests"></a><span data-ttu-id="29ad0-242">Тестирование HTTP-запросов PUT</span><span class="sxs-lookup"><span data-stu-id="29ad0-242">Test HTTP PUT requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="29ad0-243">Краткий обзор</span><span class="sxs-lookup"><span data-stu-id="29ad0-243">Synopsis</span></span>

```console
put <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="29ad0-244">Аргументы</span><span class="sxs-lookup"><span data-stu-id="29ad0-244">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="29ad0-245">Параметр маршрута, который требуется методу действия соответствующего контроллера.</span><span class="sxs-lookup"><span data-stu-id="29ad0-245">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="29ad0-246">Параметры</span><span class="sxs-lookup"><span data-stu-id="29ad0-246">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="29ad0-247">Пример</span><span class="sxs-lookup"><span data-stu-id="29ad0-247">Example</span></span>

<span data-ttu-id="29ad0-248">Чтобы отправить HTTP-запрос PUT, выполните указанные ниже действия.</span><span class="sxs-lookup"><span data-stu-id="29ad0-248">To issue an HTTP PUT request:</span></span>

1. <span data-ttu-id="29ad0-249">*Необязательно:* Чтобы просмотреть данные перед их изменением, выполните команду `get`:</span><span class="sxs-lookup"><span data-stu-id="29ad0-249">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

    ```console
    https://localhost:5001/fruits~ get
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Sat, 22 Jun 2019 00:07:32 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 1,
        "data": "Apple"
      },
      {
        "id": 2,
        "data": "Orange"
      },
      {
        "id": 3,
        "data": "Strawberry"
      }
    ]

1. Run the `put` command on an endpoint that supports it:

    ```console
    https://localhost:5001/fruits~ put 2 -h Content-Type=application/json
    ```

    <span data-ttu-id="29ad0-250">В приведенной выше команде в заголовке HTTP-запроса `Content-Type` указан формат текста запроса JSON.</span><span class="sxs-lookup"><span data-stu-id="29ad0-250">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="29ad0-251">В текстовом редакторе по умолчанию открывается файл *TMP* с шаблоном JSON, представляющим текст HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="29ad0-251">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="29ad0-252">Например:</span><span class="sxs-lookup"><span data-stu-id="29ad0-252">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="29ad0-253">Инструкции по определению текстового редактора по умолчанию см. в разделе [Установка текстового редактора по умолчанию](#set-the-default-text-editor).</span><span class="sxs-lookup"><span data-stu-id="29ad0-253">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="29ad0-254">Измените шаблон JSON в соответствии с требованиями к проверке модели:</span><span class="sxs-lookup"><span data-stu-id="29ad0-254">Modify the JSON template to satisfy model validation requirements:</span></span>

    ```json
    {
      "id": 2,
      "name": "Cherry"
    }
    ```

1. <span data-ttu-id="29ad0-255">Сохраните файл *TMP* и закройте текстовый редактор.</span><span class="sxs-lookup"><span data-stu-id="29ad0-255">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="29ad0-256">В командной оболочке появятся следующие выходные данные:</span><span class="sxs-lookup"><span data-stu-id="29ad0-256">The following output appears in the command shell:</span></span>

    ```console
    [main 2019-06-28T17:27:01.805Z] update#setState idle
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:28:21 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="29ad0-257">*Необязательно:* Чтобы просмотреть изменения, выполните команду `get`.</span><span class="sxs-lookup"><span data-stu-id="29ad0-257">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="29ad0-258">Например, если вы ввели "Cherry" в текстовом редакторе, команда `get` вернет следующие выходные данные:</span><span class="sxs-lookup"><span data-stu-id="29ad0-258">For example, if you typed "Cherry" in the text editor, a `get` returns the following:</span></span>

    ```console
    https://localhost:5001/fruits~ get
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Sat, 22 Jun 2019 00:08:20 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 1,
        "data": "Apple"
      },
      {
        "id": 2,
        "data": "Cherry"
      },
      {
        "id": 3,
        "data": "Strawberry"
      }
    ]


    https://localhost:5001/fruits~
    ```

## <a name="test-http-delete-requests"></a><span data-ttu-id="29ad0-259">Тестирование HTTP-запросов DELETE</span><span class="sxs-lookup"><span data-stu-id="29ad0-259">Test HTTP DELETE requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="29ad0-260">Краткий обзор</span><span class="sxs-lookup"><span data-stu-id="29ad0-260">Synopsis</span></span>

```console
delete <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="29ad0-261">Аргументы</span><span class="sxs-lookup"><span data-stu-id="29ad0-261">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="29ad0-262">Параметр маршрута, который требуется методу действия соответствующего контроллера.</span><span class="sxs-lookup"><span data-stu-id="29ad0-262">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="29ad0-263">Параметры</span><span class="sxs-lookup"><span data-stu-id="29ad0-263">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="29ad0-264">Пример</span><span class="sxs-lookup"><span data-stu-id="29ad0-264">Example</span></span>

<span data-ttu-id="29ad0-265">Чтобы отправить HTTP-запрос DELETE, выполните указанные ниже действия.</span><span class="sxs-lookup"><span data-stu-id="29ad0-265">To issue an HTTP DELETE request:</span></span>

1. <span data-ttu-id="29ad0-266">*Необязательно:* Чтобы просмотреть данные перед их изменением, выполните команду `get`:</span><span class="sxs-lookup"><span data-stu-id="29ad0-266">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

    ```console
    https://localhost:5001/fruits~ get
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Sat, 22 Jun 2019 00:07:32 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 1,
        "data": "Apple"
      },
      {
        "id": 2,
        "data": "Orange"
      },
      {
        "id": 3,
        "data": "Strawberry"
      }
    ]

1. Run the `delete` command on an endpoint that supports it:

    ```console
    https://localhost:5001/fruits~ delete 2
    ```

    <span data-ttu-id="29ad0-267">Выходные данные приведенной выше команды имеют следующий формат:</span><span class="sxs-lookup"><span data-stu-id="29ad0-267">The preceding command displays the following output format:</span></span>

    ```console
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:36:42 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="29ad0-268">*Необязательно:* Чтобы просмотреть изменения, выполните команду `get`.</span><span class="sxs-lookup"><span data-stu-id="29ad0-268">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="29ad0-269">В этом примере команда `get` возвращает следующие данные:</span><span class="sxs-lookup"><span data-stu-id="29ad0-269">In this example, a `get` returns the following:</span></span>

    ```console
    https://localhost:5001/fruits~ get
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Sat, 22 Jun 2019 00:16:30 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 1,
        "data": "Apple"
      },
      {
        "id": 3,
        "data": "Strawberry"
      }
    ]


    https://localhost:5001/fruits~
    ```

## <a name="test-http-patch-requests"></a><span data-ttu-id="29ad0-270">Тестирование HTTP-запросов PATCH</span><span class="sxs-lookup"><span data-stu-id="29ad0-270">Test HTTP PATCH requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="29ad0-271">Краткий обзор</span><span class="sxs-lookup"><span data-stu-id="29ad0-271">Synopsis</span></span>

```console
patch <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="29ad0-272">Аргументы</span><span class="sxs-lookup"><span data-stu-id="29ad0-272">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="29ad0-273">Параметр маршрута, который требуется методу действия соответствующего контроллера.</span><span class="sxs-lookup"><span data-stu-id="29ad0-273">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="29ad0-274">Параметры</span><span class="sxs-lookup"><span data-stu-id="29ad0-274">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

## <a name="test-http-head-requests"></a><span data-ttu-id="29ad0-275">Тестирование HTTP-запросов HEAD</span><span class="sxs-lookup"><span data-stu-id="29ad0-275">Test HTTP HEAD requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="29ad0-276">Краткий обзор</span><span class="sxs-lookup"><span data-stu-id="29ad0-276">Synopsis</span></span>

```console
head <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="29ad0-277">Аргументы</span><span class="sxs-lookup"><span data-stu-id="29ad0-277">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="29ad0-278">Параметр маршрута, который требуется методу действия соответствующего контроллера.</span><span class="sxs-lookup"><span data-stu-id="29ad0-278">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="29ad0-279">Параметры</span><span class="sxs-lookup"><span data-stu-id="29ad0-279">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="test-http-options-requests"></a><span data-ttu-id="29ad0-280">Тестирование HTTP-запросов OPTIONS</span><span class="sxs-lookup"><span data-stu-id="29ad0-280">Test HTTP OPTIONS requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="29ad0-281">Краткий обзор</span><span class="sxs-lookup"><span data-stu-id="29ad0-281">Synopsis</span></span>

```console
options <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="29ad0-282">Аргументы</span><span class="sxs-lookup"><span data-stu-id="29ad0-282">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="29ad0-283">Параметр маршрута, который требуется методу действия соответствующего контроллера.</span><span class="sxs-lookup"><span data-stu-id="29ad0-283">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="29ad0-284">Параметры</span><span class="sxs-lookup"><span data-stu-id="29ad0-284">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="set-http-request-headers"></a><span data-ttu-id="29ad0-285">Установка заголовков HTTP-запросов</span><span class="sxs-lookup"><span data-stu-id="29ad0-285">Set HTTP request headers</span></span>

<span data-ttu-id="29ad0-286">Задать заголовок HTTP-запроса можно одним из следующих способов.</span><span class="sxs-lookup"><span data-stu-id="29ad0-286">To set an HTTP request header, use one of the following approaches:</span></span>

1. <span data-ttu-id="29ad0-287">Внутри HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="29ad0-287">Set inline with the HTTP request.</span></span> <span data-ttu-id="29ad0-288">Например:</span><span class="sxs-lookup"><span data-stu-id="29ad0-288">For example:</span></span>

  ```console
  https://localhost:5001/people~ post -h Content-Type=application/json
  ```

  <span data-ttu-id="29ad0-289">При таком подходе для каждого заголовка HTTP-запроса требуется собственный параметр `-h`.</span><span class="sxs-lookup"><span data-stu-id="29ad0-289">With the preceding approach, each distinct HTTP request header requires its own `-h` option.</span></span>

1. <span data-ttu-id="29ad0-290">Перед отправкой HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="29ad0-290">Set before sending the HTTP request.</span></span> <span data-ttu-id="29ad0-291">Например:</span><span class="sxs-lookup"><span data-stu-id="29ad0-291">For example:</span></span>

  ```console
  https://localhost:5001/people~ set header Content-Type application/json
  ```

  <span data-ttu-id="29ad0-292">Если заголовок задается перед отправкой запроса, он продолжает действовать на протяжении всего сеанса командной оболочки.</span><span class="sxs-lookup"><span data-stu-id="29ad0-292">When setting the header before sending a request, the header remains set for the duration of the command shell session.</span></span> <span data-ttu-id="29ad0-293">Чтобы очистить заголовок, укажите пустое значение.</span><span class="sxs-lookup"><span data-stu-id="29ad0-293">To clear the header, provide an empty value.</span></span> <span data-ttu-id="29ad0-294">Например:</span><span class="sxs-lookup"><span data-stu-id="29ad0-294">For example:</span></span>

  ```console
  https://localhost:5001/people~ set header Content-Type
  ```

## <a name="toggle-http-request-display"></a><span data-ttu-id="29ad0-295">Включение и отключение отображения HTTP-запроса</span><span class="sxs-lookup"><span data-stu-id="29ad0-295">Toggle HTTP request display</span></span>

<span data-ttu-id="29ad0-296">По умолчанию отправляемый HTTP-запрос не отображается.</span><span class="sxs-lookup"><span data-stu-id="29ad0-296">By default, display of the HTTP request being sent is suppressed.</span></span> <span data-ttu-id="29ad0-297">Соответствующую настройку можно изменить на всю длительность сеанса командной оболочки.</span><span class="sxs-lookup"><span data-stu-id="29ad0-297">It's possible to change the corresponding setting for the duration of the command shell session.</span></span>

### <a name="enable-request-display"></a><span data-ttu-id="29ad0-298">Включение отображения запроса</span><span class="sxs-lookup"><span data-stu-id="29ad0-298">Enable request display</span></span>

<span data-ttu-id="29ad0-299">Чтобы отправляемый HTTP-запрос отображался, выполните команду `echo on`.</span><span class="sxs-lookup"><span data-stu-id="29ad0-299">View the HTTP request being sent by running the `echo on` command.</span></span> <span data-ttu-id="29ad0-300">Например:</span><span class="sxs-lookup"><span data-stu-id="29ad0-300">For example:</span></span>

```console
https://localhost:5001/people~ echo on
Request echoing is on
```

<span data-ttu-id="29ad0-301">При отправке последующих HTTP-запросов в рамках текущего сеанса заголовки запросов будут отображаться.</span><span class="sxs-lookup"><span data-stu-id="29ad0-301">Subsequent HTTP requests in the current session display the request headers.</span></span> <span data-ttu-id="29ad0-302">Например:</span><span class="sxs-lookup"><span data-stu-id="29ad0-302">For example:</span></span>

```console
https://localhost:5001/people~ post

[main 2019-06-28T18:50:11.930Z] update#setState idle
Request to https://localhost:5001...

POST /people HTTP/1.1
Content-Length: 41
Content-Type: application/json
User-Agent: HTTP-REPL

{
  "id": 0,
  "name": "Scott Addie"
}

Response from https://localhost:5001...

HTTP/1.1 201 Created
Content-Type: application/json; charset=utf-8
Date: Fri, 28 Jun 2019 18:50:21 GMT
Location: https://localhost:5001/people/4
Server: Kestrel
Transfer-Encoding: chunked

{
  "id": 4,
  "name": "Scott Addie"
}


https://localhost:5001/people~
```

### <a name="disable-request-display"></a><span data-ttu-id="29ad0-303">Отключение отображения запроса</span><span class="sxs-lookup"><span data-stu-id="29ad0-303">Disable request display</span></span>

<span data-ttu-id="29ad0-304">Чтобы отправляемый HTTP-запрос не отображался, выполните команду `echo off`.</span><span class="sxs-lookup"><span data-stu-id="29ad0-304">Suppress display of the HTTP request being sent by running the `echo off` command.</span></span> <span data-ttu-id="29ad0-305">Например:</span><span class="sxs-lookup"><span data-stu-id="29ad0-305">For example:</span></span>

```console
https://localhost:5001/people~ echo off
Request echoing is off
```

## <a name="run-a-script"></a><span data-ttu-id="29ad0-306">Выполнение скрипта</span><span class="sxs-lookup"><span data-stu-id="29ad0-306">Run a script</span></span>

<span data-ttu-id="29ad0-307">Если вы часто выполняете один и тот же набор команд HTTP REPL, их можно сохранить в текстовом файле.</span><span class="sxs-lookup"><span data-stu-id="29ad0-307">If you frequently execute the same set of HTTP REPL commands, consider storing them in a text file.</span></span> <span data-ttu-id="29ad0-308">Команды в файле имеют тот же формат, что и выполняемые вручную в командной строке.</span><span class="sxs-lookup"><span data-stu-id="29ad0-308">Commands in the file take the same form as those executed manually on the command line.</span></span> <span data-ttu-id="29ad0-309">Их можно выполнять в пакетном режиме с помощью команды `run`.</span><span class="sxs-lookup"><span data-stu-id="29ad0-309">The commands can be executed in a batched fashion using the `run` command.</span></span> <span data-ttu-id="29ad0-310">Например:</span><span class="sxs-lookup"><span data-stu-id="29ad0-310">For example:</span></span>

1. <span data-ttu-id="29ad0-311">Создайте текстовый файл с набором команд, разделенных символами новой строки.</span><span class="sxs-lookup"><span data-stu-id="29ad0-311">Create a text file containing a set of newline-delimited commands.</span></span> <span data-ttu-id="29ad0-312">Например, это может быть файл *people-script.txt* со следующими командами:</span><span class="sxs-lookup"><span data-stu-id="29ad0-312">To illustrate, consider a *people-script.txt* file containing the following commands:</span></span>

    ```text
    set base https://localhost:5001
    ls
    cd People
    ls
    get 1
    ```

1. <span data-ttu-id="29ad0-313">Выполните команду `run`, передав в нее путь к текстовому файлу.</span><span class="sxs-lookup"><span data-stu-id="29ad0-313">Execute the `run` command, passing in the text file's path.</span></span> <span data-ttu-id="29ad0-314">Например:</span><span class="sxs-lookup"><span data-stu-id="29ad0-314">For example:</span></span>

    ```console
    https://localhost:5001/~ run C:\http-repl-scripts\people-script.txt
    ```

    <span data-ttu-id="29ad0-315">Появится следующий результат:</span><span class="sxs-lookup"><span data-stu-id="29ad0-315">The following output appears:</span></span>

    ```console
    https://localhost:5001/~ set base https://localhost:5001
    Using swagger metadata from https://localhost:5001/swagger/v1/swagger.json
    
    https://localhost:5001/~ ls
    .        []
    Fruits   [get|post]
    People   [get|post]
    
    https://localhost:5001/~ cd People
    /People    [get|post]
    
    https://localhost:5001/People~ ls
    .      [get|post]
    ..     []
    {id}   [get|put|delete]
    
    https://localhost:5001/People~ get 1
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Fri, 12 Jul 2019 19:20:10 GMT
    Server: Kestrel
    Transfer-Encoding: chunked
    
    {
      "id": 1,
      "name": "Scott Hunter"
    }
    
    
    https://localhost:5001/People~
    ```

## <a name="clear-the-output"></a><span data-ttu-id="29ad0-316">Очистка выходных данных</span><span class="sxs-lookup"><span data-stu-id="29ad0-316">Clear the output</span></span>

<span data-ttu-id="29ad0-317">Чтобы удалить все выходные данные средства HTTP REPL из командной оболочки, выполните команду `clear` или `cls`.</span><span class="sxs-lookup"><span data-stu-id="29ad0-317">To remove all output written to the command shell by the HTTP REPL tool, run the `clear` or `cls` command.</span></span> <span data-ttu-id="29ad0-318">Например, предположим, что в командной оболочке есть следующие выходные данные:</span><span class="sxs-lookup"><span data-stu-id="29ad0-318">To illustrate, imagine the command shell contains the following output:</span></span>

```console
httprepl https://localhost:5001
(Disconnected)~ set base "https://localhost:5001"
Using swagger metadata from https://localhost:5001/swagger/v1/swagger.json

https://localhost:5001/~ ls
.        []
Fruits   [get|post]
People   [get|post]

https://localhost:5001/~
```

<span data-ttu-id="29ad0-319">Чтобы очистить их, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="29ad0-319">Run the following command to clear the output:</span></span>

```console
https://localhost:5001/~ clear
```

<span data-ttu-id="29ad0-320">После выполнения приведенной выше команды командная оболочка содержит только следующие выходные данные:</span><span class="sxs-lookup"><span data-stu-id="29ad0-320">After running the preceding command, the command shell contains only the following output:</span></span>

```console
https://localhost:5001/~
```

## <a name="additional-resources"></a><span data-ttu-id="29ad0-321">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="29ad0-321">Additional resources</span></span>

* [<span data-ttu-id="29ad0-322">Запросы к REST API</span><span class="sxs-lookup"><span data-stu-id="29ad0-322">REST API requests</span></span>](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods)
* [<span data-ttu-id="29ad0-323">Репозиторий HTTP REPL в GitHub</span><span class="sxs-lookup"><span data-stu-id="29ad0-323">HTTP REPL GitHub repository</span></span>](https://github.com/aspnet/HttpRepl)
