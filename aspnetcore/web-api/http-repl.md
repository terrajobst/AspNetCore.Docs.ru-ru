---
title: Тестирование веб-API с помощью HTTP REPL
author: scottaddie
description: Узнайте, как использовать глобальное средство .NET Core HTTP REPL для просмотра и тестирования веб-API ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 07/12/2019
uid: web-api/http-repl
ms.openlocfilehash: 1774382305cc3d479291700390807d277a24bfa7
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/18/2019
ms.locfileid: "68308347"
---
# <a name="test-web-apis-with-the-http-repl"></a><span data-ttu-id="2da92-103">Тестирование веб-API с помощью HTTP REPL</span><span class="sxs-lookup"><span data-stu-id="2da92-103">Test web APIs with the HTTP REPL</span></span>

<span data-ttu-id="2da92-104">Автор: [Скотт Адди](https://twitter.com/Scott_Addie) (Scott Addie)</span><span class="sxs-lookup"><span data-stu-id="2da92-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="2da92-105">HTTP read-eval-print loop (REPL):</span><span class="sxs-lookup"><span data-stu-id="2da92-105">The HTTP Read-Eval-Print Loop (REPL) is:</span></span>

* <span data-ttu-id="2da92-106">это кроссплатформенная программа командной строки, которая поддерживается везде, где поддерживается .NET Core;</span><span class="sxs-lookup"><span data-stu-id="2da92-106">A lightweight, cross-platform command-line tool that's supported everywhere .NET Core is supported.</span></span>
* <span data-ttu-id="2da92-107">служит для создания HTTP-запросов с целью тестирования веб-API ASP.NET Core и просмотра их результатов.</span><span class="sxs-lookup"><span data-stu-id="2da92-107">Used for making HTTP requests to test ASP.NET Core web APIs and view their results.</span></span>

<span data-ttu-id="2da92-108">Поддерживаются следующие [HTTP-команды](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods):</span><span class="sxs-lookup"><span data-stu-id="2da92-108">The following [HTTP verbs](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods) are supported:</span></span>

* [<span data-ttu-id="2da92-109">DELETE</span><span class="sxs-lookup"><span data-stu-id="2da92-109">DELETE</span></span>](#test-http-delete-requests)
* [<span data-ttu-id="2da92-110">GET</span><span class="sxs-lookup"><span data-stu-id="2da92-110">GET</span></span>](#test-http-get-requests)
* [<span data-ttu-id="2da92-111">HEAD</span><span class="sxs-lookup"><span data-stu-id="2da92-111">HEAD</span></span>](#test-http-head-requests)
* [<span data-ttu-id="2da92-112">OPTIONS</span><span class="sxs-lookup"><span data-stu-id="2da92-112">OPTIONS</span></span>](#test-http-options-requests)
* [<span data-ttu-id="2da92-113">PATCH</span><span class="sxs-lookup"><span data-stu-id="2da92-113">PATCH</span></span>](#test-http-patch-requests)
* [<span data-ttu-id="2da92-114">POST</span><span class="sxs-lookup"><span data-stu-id="2da92-114">POST</span></span>](#test-http-post-requests)
* [<span data-ttu-id="2da92-115">PUT</span><span class="sxs-lookup"><span data-stu-id="2da92-115">PUT</span></span>](#test-http-put-requests)

<span data-ttu-id="2da92-116">Для выполнения дальнейших инструкций [просмотрите или скачайте пример веб-API ASP.NET Core](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([как скачивать](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="2da92-116">To follow along, [view or download the sample ASP.NET Core web API](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2da92-117">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="2da92-117">Prerequisites</span></span>

* [!INCLUDE [2.1-SDK](~/includes/2.1-SDK.md)]

## <a name="installation"></a><span data-ttu-id="2da92-118">Установка</span><span class="sxs-lookup"><span data-stu-id="2da92-118">Installation</span></span>

<span data-ttu-id="2da92-119">Чтобы установить HTTP REPL, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="2da92-119">To install the HTTP REPL, run the following command:</span></span>

```console
dotnet tool install -g dotnet-httprepl
    --version 2.2.0-*
    --add-source https://dotnet.myget.org/F/dotnet-core/api/v3/index.json
```

<span data-ttu-id="2da92-120">[Глобальное средство .NET Core](/dotnet/core/tools/global-tools#install-a-global-tool) устанавливается из пакета NuGet [dotnet-httprepl](https://dotnet.myget.org/feed/dotnet-core/package/nuget/dotnet-httprepl
), размещенного в MyGet.</span><span class="sxs-lookup"><span data-stu-id="2da92-120">A [.NET Core Global Tool](/dotnet/core/tools/global-tools#install-a-global-tool) is installed from the [dotnet-httprepl](https://dotnet.myget.org/feed/dotnet-core/package/nuget/dotnet-httprepl
) NuGet package hosted on MyGet.</span></span>

## <a name="usage"></a><span data-ttu-id="2da92-121">Использование</span><span class="sxs-lookup"><span data-stu-id="2da92-121">Usage</span></span>

<span data-ttu-id="2da92-122">После успешной установки HTTP REPL выполните следующую команду, чтобы запустить средство:</span><span class="sxs-lookup"><span data-stu-id="2da92-122">After successful installation of the tool, run the following command to start the HTTP REPL:</span></span>

```console
dotnet httprepl
```

<span data-ttu-id="2da92-123">Чтобы просмотреть доступные команды HTTP REPL, выполните одну из следующих команд:</span><span class="sxs-lookup"><span data-stu-id="2da92-123">To view the available HTTP REPL commands, run one of the following commands:</span></span>

```console
dotnet httprepl -h
```

```console
dotnet httprepl --help
```

<span data-ttu-id="2da92-124">Выводится следующий результат.</span><span class="sxs-lookup"><span data-stu-id="2da92-124">The following output is displayed:</span></span>

```console
Usage: dotnet httprepl [<BASE_ADDRESS>] [options]

Arguments:
  <BASE_ADDRESS> - The initial base address for the REPL.

Options:
  --help - Show help information.

Once the REPL starts, these commands are valid:

HTTP Commands:
Use these commands to execute requests against your application.

GET            Issues a GET request.
POST           Issues a POST request.
PUT            Issues a PUT request.
DELETE         Issues a DELETE request.
PATCH          Issues a PATCH request.
HEAD           Issues a HEAD request.
OPTIONS        Issues an OPTIONS request.

set header     Sets or clears a header for all requests. e.g. `set header content-type application/json`


Navigation Commands:
The REPL allows you to navigate your URL space and focus on specific APIs that you are working on.

set base       Set the base URI. e.g. `set base http://locahost:5000`
set swagger    Set the URI, relative to your base if set, of the Swagger document for this API. e.g. `set swagger /swagger/v1/swagger.json`
ls             Show all endpoints for the current path.
cd             Append the given directory to the currently selected path, or move up a path when using `cd ..`.

Shell Commands:
Use these commands to interact with the REPL shell.

clear          Removes all text from the shell.
echo [on/off]  Turns request echoing on or off, show the request that was made when using request commands.
exit           Exit the shell.

REPL Customization Commands:
Use these commands to customize the REPL behavior.

pref [get/set] Allows viewing or changing preferences, e.g. 'pref set editor.command.default 'C:\Program Files\Microsoft VS Code\Code.exe'`
run            Runs the script at the given path. A script is a set of commands that can be typed with one command per line.
ui             Displays the Swagger UI page, if available, in the default browser.

Use help <COMMAND> to learn more details about individual commands. e.g. `help get`
```

<span data-ttu-id="2da92-125">В HTTP REPL поддерживается завершение команд.</span><span class="sxs-lookup"><span data-stu-id="2da92-125">The HTTP REPL offers command completion.</span></span> <span data-ttu-id="2da92-126">Нажимая клавишу <kbd>TAB</kbd>, можно переходить по списку команд, которые начинаются с введенных символов или конечной точки API.</span><span class="sxs-lookup"><span data-stu-id="2da92-126">Pressing the <kbd>Tab</kbd> key iterates through the list of commands that complete the characters or API endpoint that you typed.</span></span> <span data-ttu-id="2da92-127">Доступные команды CLI описываются в следующих разделах.</span><span class="sxs-lookup"><span data-stu-id="2da92-127">The following sections outline the available CLI commands.</span></span>

## <a name="connect-to-the-web-api"></a><span data-ttu-id="2da92-128">Подключение к веб-API</span><span class="sxs-lookup"><span data-stu-id="2da92-128">Connect to the web API</span></span>

<span data-ttu-id="2da92-129">Чтобы подключиться к веб-API, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="2da92-129">Connect to a web API by running the following command:</span></span>

```console
dotnet httprepl <BASE URI>
```

<span data-ttu-id="2da92-130">`<BASE URI>` — это базовый универсальный код ресурса (URI) для веб-API.</span><span class="sxs-lookup"><span data-stu-id="2da92-130">`<BASE URI>` is the base URI for the web API.</span></span> <span data-ttu-id="2da92-131">Например:</span><span class="sxs-lookup"><span data-stu-id="2da92-131">For example:</span></span>

```console
dotnet httprepl https://localhost:5001
```

<span data-ttu-id="2da92-132">Кроме того, когда программа HTTP REPL запущена, можно в любой момент выполнить следующую команду:</span><span class="sxs-lookup"><span data-stu-id="2da92-132">Alternatively, run the following command at any time while the HTTP REPL is running:</span></span>

```console
set base <BASE URI>
```

<span data-ttu-id="2da92-133">Например:</span><span class="sxs-lookup"><span data-stu-id="2da92-133">For example:</span></span>

```console
(Disconnected)~ set base https://localhost:5001
```

## <a name="point-to-the-swagger-document-for-the-web-api"></a><span data-ttu-id="2da92-134">Ссылка на документ Swagger для веб-API</span><span class="sxs-lookup"><span data-stu-id="2da92-134">Point to the Swagger document for the web API</span></span>

<span data-ttu-id="2da92-135">Чтобы изучить веб-API, укажите относительный универсальный код ресурса (URI) документа Swagger для веб-API.</span><span class="sxs-lookup"><span data-stu-id="2da92-135">To properly inspect the web API, set the relative URI to the Swagger document for the web API.</span></span> <span data-ttu-id="2da92-136">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="2da92-136">Run the following command:</span></span>

```console
set swagger <RELATIVE URI>
```

<span data-ttu-id="2da92-137">Например:</span><span class="sxs-lookup"><span data-stu-id="2da92-137">For example:</span></span>

```console
https://localhost:5001/~ set swagger /swagger/v1/swagger.json
```

## <a name="navigate-the-web-api"></a><span data-ttu-id="2da92-138">Просмотр веб-API</span><span class="sxs-lookup"><span data-stu-id="2da92-138">Navigate the web API</span></span>

### <a name="view-available-endpoints"></a><span data-ttu-id="2da92-139">Просмотр доступных конечных точек</span><span class="sxs-lookup"><span data-stu-id="2da92-139">View available endpoints</span></span>

<span data-ttu-id="2da92-140">Чтобы получить список конечных точек (контроллеров) по текущему пути веб-API, выполните команду `ls` или `dir`:</span><span class="sxs-lookup"><span data-stu-id="2da92-140">To list the different endpoints (controllers) at the current path of the web API address, run the `ls` or `dir` command:</span></span>

```console
https://localhot:5001/~ ls
```

<span data-ttu-id="2da92-141">Будут выведены данные в следующем формате:</span><span class="sxs-lookup"><span data-stu-id="2da92-141">The following output format is displayed:</span></span>

```console
.        []
Fruits   [get|post]
People   [get|post]

https://localhost:5001/~
```

<span data-ttu-id="2da92-142">Из приведенных выше результатов видно, что доступно два контроллера: `Fruits` и `People`.</span><span class="sxs-lookup"><span data-stu-id="2da92-142">The preceding output indicates that there are two controllers available: `Fruits` and `People`.</span></span> <span data-ttu-id="2da92-143">Они оба поддерживают операции HTTP GET и POST без параметров.</span><span class="sxs-lookup"><span data-stu-id="2da92-143">Both controllers support parameterless HTTP GET and POST operations.</span></span>

<span data-ttu-id="2da92-144">Более подробную информацию можно получить, перейдя к определенному контроллеру.</span><span class="sxs-lookup"><span data-stu-id="2da92-144">Navigating into a specific controller reveals more detail.</span></span> <span data-ttu-id="2da92-145">Например, из выходных данных приведенной ниже команды видно, что контроллер `Fruits` также поддерживает операции HTTP GET, PUT и DELETE.</span><span class="sxs-lookup"><span data-stu-id="2da92-145">For example, the following command's output shows the `Fruits` controller also supports HTTP GET, PUT, and DELETE operations.</span></span> <span data-ttu-id="2da92-146">Каждая из этих операций принимает параметр `id` в маршруте:</span><span class="sxs-lookup"><span data-stu-id="2da92-146">Each of these operations expects an `id` parameter in the route:</span></span>

```console
https://localhost:5001/fruits~ ls
.      [get|post]
..     []
{id}   [get|put|delete]

https://localhost:5001/fruits~
```

<span data-ttu-id="2da92-147">Кроме того, можно выполнить команду `ui`, чтобы открыть страницу с пользовательским интерфейсом Swagger веб-API в браузере.</span><span class="sxs-lookup"><span data-stu-id="2da92-147">Alternatively, run the `ui` command to open the web API's Swagger UI page in a browser.</span></span> <span data-ttu-id="2da92-148">Например:</span><span class="sxs-lookup"><span data-stu-id="2da92-148">For example:</span></span>

```console
https://localhost:5001/~ ui
```

### <a name="navigate-to-an-endpoint"></a><span data-ttu-id="2da92-149">Переход к конечной точке</span><span class="sxs-lookup"><span data-stu-id="2da92-149">Navigate to an endpoint</span></span>

<span data-ttu-id="2da92-150">Чтобы перейти к другой конечной точке веб-API, выполните команду `cd`:</span><span class="sxs-lookup"><span data-stu-id="2da92-150">To navigate to a different endpoint on the web API, run the `cd` command:</span></span>

```console
https://localhost:5001/~ cd people
```

<span data-ttu-id="2da92-151">В пути, указываемом после команды `cd`, регистр символов не учитывается.</span><span class="sxs-lookup"><span data-stu-id="2da92-151">The path following the `cd` command is case insensitive.</span></span> <span data-ttu-id="2da92-152">Будут выведены данные в следующем формате:</span><span class="sxs-lookup"><span data-stu-id="2da92-152">The following output format is displayed:</span></span>

```console
/people    [get|post]

https://localhost:5001/people~
```

## <a name="customize-the-http-repl"></a><span data-ttu-id="2da92-153">Настройка HTTP REPL</span><span class="sxs-lookup"><span data-stu-id="2da92-153">Customize the HTTP REPL</span></span>

<span data-ttu-id="2da92-154">[Цвета](#set-color-preferences), используемые в HTTP REPL по умолчанию, можно настроить.</span><span class="sxs-lookup"><span data-stu-id="2da92-154">The HTTP REPL's default [colors](#set-color-preferences) can be customized.</span></span> <span data-ttu-id="2da92-155">Кроме того, можно определить [текстовый редактор по умолчанию](#set-the-default-text-editor).</span><span class="sxs-lookup"><span data-stu-id="2da92-155">Additionally, a [default text editor](#set-the-default-text-editor) can be defined.</span></span> <span data-ttu-id="2da92-156">Настройки HTTP REPL сохраняются на протяжении текущего сеанса и учитываются при открытии следующих сеансов.</span><span class="sxs-lookup"><span data-stu-id="2da92-156">The HTTP REPL preferences are persisted across the current session and are honored in future sessions.</span></span> <span data-ttu-id="2da92-157">Измененные настройки хранятся в следующем файле:</span><span class="sxs-lookup"><span data-stu-id="2da92-157">Once modified, the preferences are stored in the following file:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="2da92-158">Linux</span><span class="sxs-lookup"><span data-stu-id="2da92-158">Linux</span></span>](#tab/linux)

<span data-ttu-id="2da92-159">*%HOME%/.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="2da92-159">*%HOME%/.httpreplprefs*</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="2da92-160">macOS</span><span class="sxs-lookup"><span data-stu-id="2da92-160">macOS</span></span>](#tab/macos)

<span data-ttu-id="2da92-161">*%HOME%/.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="2da92-161">*%HOME%/.httpreplprefs*</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="2da92-162">Windows</span><span class="sxs-lookup"><span data-stu-id="2da92-162">Windows</span></span>](#tab/windows)

<span data-ttu-id="2da92-163">*%USERPROFILE%\\.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="2da92-163">*%USERPROFILE%\\.httpreplprefs*</span></span>

---

<span data-ttu-id="2da92-164">Файл *HTTPREPLPREFS* загружается при запуске. Во время выполнения изменения в нем не отслеживаются.</span><span class="sxs-lookup"><span data-stu-id="2da92-164">The *.httpreplprefs* file is loaded on startup and not monitored for changes at runtime.</span></span> <span data-ttu-id="2da92-165">Изменения, внесенные в него вручную, вступают в силу только после перезапуска средства.</span><span class="sxs-lookup"><span data-stu-id="2da92-165">Manual modifications to the file take effect only after restarting the tool.</span></span>

### <a name="view-the-settings"></a><span data-ttu-id="2da92-166">Просмотр параметров</span><span class="sxs-lookup"><span data-stu-id="2da92-166">View the settings</span></span>

<span data-ttu-id="2da92-167">Чтобы просмотреть доступные параметры, выполните команду `pref get`.</span><span class="sxs-lookup"><span data-stu-id="2da92-167">To view the available settings, run the `pref get` command.</span></span> <span data-ttu-id="2da92-168">Например:</span><span class="sxs-lookup"><span data-stu-id="2da92-168">For example:</span></span>

```console
https://localhost:5001/~ pref get
```

<span data-ttu-id="2da92-169">В результате выполнения приведенной выше команды выводятся доступные пары "ключ-значение":</span><span class="sxs-lookup"><span data-stu-id="2da92-169">The preceding command displays the available key-value pairs:</span></span>

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

### <a name="set-color-preferences"></a><span data-ttu-id="2da92-170">Настройки цветов</span><span class="sxs-lookup"><span data-stu-id="2da92-170">Set color preferences</span></span>

<span data-ttu-id="2da92-171">Раскрашивание ответов в настоящее время поддерживается только для JSON.</span><span class="sxs-lookup"><span data-stu-id="2da92-171">Response colorization is currently supported for JSON only.</span></span> <span data-ttu-id="2da92-172">Чтобы настроить цвета по умолчанию в программе HTTP REPL, найдите ключ, соответствующий изменяемому цвету.</span><span class="sxs-lookup"><span data-stu-id="2da92-172">To customize the default HTTP REPL tool coloring, locate the key corresponding to the color to be changed.</span></span> <span data-ttu-id="2da92-173">Инструкции по поиску ключей см. в разделе [Просмотр параметров](#view-the-settings).</span><span class="sxs-lookup"><span data-stu-id="2da92-173">For instructions on how to find the keys, see the [View the settings](#view-the-settings) section.</span></span> <span data-ttu-id="2da92-174">Например, измените значение ключа `colors.json` с `Green` на `White` следующим образом:</span><span class="sxs-lookup"><span data-stu-id="2da92-174">For example, change the `colors.json` key value from `Green` to `White` as follows:</span></span>

```console
https://localhost:5001/people~ pref set colors.json White
```

<span data-ttu-id="2da92-175">Можно использовать только [допустимые цвета](https://github.com/aspnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs).</span><span class="sxs-lookup"><span data-stu-id="2da92-175">Only the [allowed colors](https://github.com/aspnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs) may be used.</span></span> <span data-ttu-id="2da92-176">При выполнении последующих HTTP-запросов к выходным данным будут применяться новые цвета.</span><span class="sxs-lookup"><span data-stu-id="2da92-176">Subsequent HTTP requests display output with the new coloring.</span></span>

<span data-ttu-id="2da92-177">Если определенные цветовые ключи не заданы, используются более общие ключи.</span><span class="sxs-lookup"><span data-stu-id="2da92-177">When specific color keys aren't set, more generic keys are considered.</span></span> <span data-ttu-id="2da92-178">Рассмотрим это поведение на следующем примере:</span><span class="sxs-lookup"><span data-stu-id="2da92-178">To demonstrate this fallback behavior, consider the following example:</span></span>

* <span data-ttu-id="2da92-179">Если у `colors.json.name` нет значения, используется `colors.json.string`.</span><span class="sxs-lookup"><span data-stu-id="2da92-179">If `colors.json.name` doesn't have a value, `colors.json.string` is used.</span></span>
* <span data-ttu-id="2da92-180">Если у `colors.json.string` нет значения, используется `colors.json.literal`.</span><span class="sxs-lookup"><span data-stu-id="2da92-180">If `colors.json.string` doesn't have a value, `colors.json.literal` is used.</span></span>
* <span data-ttu-id="2da92-181">Если у `colors.json.literal` нет значения, используется `colors.json`.</span><span class="sxs-lookup"><span data-stu-id="2da92-181">If `colors.json.literal` doesn't have a value, `colors.json` is used.</span></span> 
* <span data-ttu-id="2da92-182">Если у `colors.json` нет значения, используется текст цвета по умолчанию для командной оболочки (`AllowedColors.None`).</span><span class="sxs-lookup"><span data-stu-id="2da92-182">If `colors.json` doesn't have a value, the command shell's default text color (`AllowedColors.None`) is used.</span></span>

### <a name="set-indentation-size"></a><span data-ttu-id="2da92-183">Установка размера отступа</span><span class="sxs-lookup"><span data-stu-id="2da92-183">Set indentation size</span></span>

<span data-ttu-id="2da92-184">Настройка размера отступа в ответах в настоящее время поддерживается только для JSON.</span><span class="sxs-lookup"><span data-stu-id="2da92-184">Response indentation size customization is currently supported for JSON only.</span></span> <span data-ttu-id="2da92-185">Размер по умолчанию — два пробела.</span><span class="sxs-lookup"><span data-stu-id="2da92-185">The default size is two spaces.</span></span> <span data-ttu-id="2da92-186">Например:</span><span class="sxs-lookup"><span data-stu-id="2da92-186">For example:</span></span>

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

<span data-ttu-id="2da92-187">Чтобы изменить размер по умолчанию, задайте ключ `formatting.json.indentSize`.</span><span class="sxs-lookup"><span data-stu-id="2da92-187">To change the default size, set the `formatting.json.indentSize` key.</span></span> <span data-ttu-id="2da92-188">Например, чтобы использовались четыре пробела, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="2da92-188">For example, to always use four spaces:</span></span>

```console
pref set formatting.json.indentSize 4
```

<span data-ttu-id="2da92-189">В последующих ответах будет применяться отступ в четыре пробела:</span><span class="sxs-lookup"><span data-stu-id="2da92-189">Subsequent responses honor the setting of four spaces:</span></span>

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

### <a name="set-indentation-size"></a><span data-ttu-id="2da92-190">Установка размера отступа</span><span class="sxs-lookup"><span data-stu-id="2da92-190">Set indentation size</span></span>

<span data-ttu-id="2da92-191">Настройка размера отступа в ответах в настоящее время поддерживается только для JSON.</span><span class="sxs-lookup"><span data-stu-id="2da92-191">Response indentation size customization is currently supported for JSON only.</span></span> <span data-ttu-id="2da92-192">Размер по умолчанию — два пробела.</span><span class="sxs-lookup"><span data-stu-id="2da92-192">The default size is two spaces.</span></span> <span data-ttu-id="2da92-193">Например:</span><span class="sxs-lookup"><span data-stu-id="2da92-193">For example:</span></span>

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

<span data-ttu-id="2da92-194">Чтобы изменить размер по умолчанию, задайте ключ `formatting.json.indentSize`.</span><span class="sxs-lookup"><span data-stu-id="2da92-194">To change the default size, set the `formatting.json.indentSize` key.</span></span> <span data-ttu-id="2da92-195">Например, чтобы использовались четыре пробела, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="2da92-195">For example, to always use four spaces:</span></span>

```console
pref set formatting.json.indentSize 4
```

<span data-ttu-id="2da92-196">В последующих ответах будет применяться отступ в четыре пробела:</span><span class="sxs-lookup"><span data-stu-id="2da92-196">Subsequent responses honor the setting of four spaces:</span></span>

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

### <a name="set-the-default-text-editor"></a><span data-ttu-id="2da92-197">Установка текстового редактора по умолчанию</span><span class="sxs-lookup"><span data-stu-id="2da92-197">Set the default text editor</span></span>

<span data-ttu-id="2da92-198">По умолчанию текстовый редактор для HTTP REPL не настроен.</span><span class="sxs-lookup"><span data-stu-id="2da92-198">By default, the HTTP REPL has no text editor configured for use.</span></span> <span data-ttu-id="2da92-199">Для тестирования методов веб-API, требующих текста HTTP-запроса, необходимо задать текстовый редактор по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="2da92-199">To test web API methods requiring an HTTP request body, a default text editor must be set.</span></span> <span data-ttu-id="2da92-200">Средство HTTP REPL запускает настроенный текстовый редактор исключительно в целях составления текста запроса.</span><span class="sxs-lookup"><span data-stu-id="2da92-200">The HTTP REPL tool launches the configured text editor for the sole purpose of composing the request body.</span></span> <span data-ttu-id="2da92-201">Чтобы указать текстовый редактор по умолчанию, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="2da92-201">Run the following command to set your preferred text editor as the default:</span></span>

```console
pref set editor.command.default "<EXECUTABLE>"
```

<span data-ttu-id="2da92-202">В приведенной выше команде `<EXECUTABLE>` — это полный путь к исполняемому файлу текстового редактора.</span><span class="sxs-lookup"><span data-stu-id="2da92-202">In the preceding command, `<EXECUTABLE>` is the full path to the text editor's executable file.</span></span> <span data-ttu-id="2da92-203">Например, чтобы задать Visual Studio Code как текстовый редактор по умолчанию, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="2da92-203">For example, run the following command to set Visual Studio Code as the default text editor:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="2da92-204">Linux</span><span class="sxs-lookup"><span data-stu-id="2da92-204">Linux</span></span>](#tab/linux)

```console
pref set editor.command.default "/usr/bin/code"
```

# <a name="macostabmacos"></a>[<span data-ttu-id="2da92-205">macOS</span><span class="sxs-lookup"><span data-stu-id="2da92-205">macOS</span></span>](#tab/macos)

```console
pref set editor.command.default "/Applications/Visual Studio Code.app/Contents/Resources/app/bin/code"
```

# <a name="windowstabwindows"></a>[<span data-ttu-id="2da92-206">Windows</span><span class="sxs-lookup"><span data-stu-id="2da92-206">Windows</span></span>](#tab/windows)

```console
pref set editor.command.default "C:\Program Files\Microsoft VS Code\Code.exe"
```

---

<span data-ttu-id="2da92-207">Чтобы текстовый редактор по умолчанию запускался с определенными аргументами CLI, задайте ключ `editor.command.default.arguments`.</span><span class="sxs-lookup"><span data-stu-id="2da92-207">To launch the default text editor with specific CLI arguments, set the `editor.command.default.arguments` key.</span></span> <span data-ttu-id="2da92-208">Например, предположим, что Visual Studio Code — это текстовый редактор по умолчанию и необходимо, чтобы средство HTTP REPL всегда запускало сеанс Visual Studio Code с отключенными расширениями.</span><span class="sxs-lookup"><span data-stu-id="2da92-208">For example, assume Visual Studio Code is the default text editor and that you always want the HTTP REPL to open Visual Studio Code in a new session with extensions disabled.</span></span> <span data-ttu-id="2da92-209">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="2da92-209">Run the following command:</span></span>

```console
pref set editor.command.default.arguments "--disable-extensions --new-window"
```

## <a name="test-http-get-requests"></a><span data-ttu-id="2da92-210">Тестирование HTTP-запросов GET</span><span class="sxs-lookup"><span data-stu-id="2da92-210">Test HTTP GET requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="2da92-211">Краткий обзор</span><span class="sxs-lookup"><span data-stu-id="2da92-211">Synopsis</span></span>

```console
get <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="2da92-212">Аргументы</span><span class="sxs-lookup"><span data-stu-id="2da92-212">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="2da92-213">Параметр маршрута, который требуется методу действия соответствующего контроллера.</span><span class="sxs-lookup"><span data-stu-id="2da92-213">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="2da92-214">Параметры</span><span class="sxs-lookup"><span data-stu-id="2da92-214">Options</span></span>

<span data-ttu-id="2da92-215">Для команды `get` доступны следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="2da92-215">The following options are available for the `get` command:</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="2da92-216">Пример</span><span class="sxs-lookup"><span data-stu-id="2da92-216">Example</span></span>

<span data-ttu-id="2da92-217">Чтобы отправить HTTP-запрос GET, выполните указанные ниже действия.</span><span class="sxs-lookup"><span data-stu-id="2da92-217">To issue an HTTP GET request:</span></span>

1. <span data-ttu-id="2da92-218">Выполните команду `get` в конечной точке, которая поддерживает ее:</span><span class="sxs-lookup"><span data-stu-id="2da92-218">Run the `get` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ get
    ```

    <span data-ttu-id="2da92-219">Выходные данные приведенной выше команды имеют следующий формат:</span><span class="sxs-lookup"><span data-stu-id="2da92-219">The preceding command displays the following output format:</span></span>

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

1. <span data-ttu-id="2da92-220">Получите определенную запись, передав параметр в команду `get`:</span><span class="sxs-lookup"><span data-stu-id="2da92-220">Retrieve a specific record by passing a parameter to the `get` command:</span></span>

    ```console
    https://localhost:5001/people~ get 2
    ```

    <span data-ttu-id="2da92-221">Выходные данные приведенной выше команды имеют следующий формат:</span><span class="sxs-lookup"><span data-stu-id="2da92-221">The preceding command displays the following output format:</span></span>

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

## <a name="test-http-post-requests"></a><span data-ttu-id="2da92-222">Тестирование HTTP-запросов POST</span><span class="sxs-lookup"><span data-stu-id="2da92-222">Test HTTP POST requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="2da92-223">Краткий обзор</span><span class="sxs-lookup"><span data-stu-id="2da92-223">Synopsis</span></span>

```console
post <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="2da92-224">Аргументы</span><span class="sxs-lookup"><span data-stu-id="2da92-224">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="2da92-225">Параметр маршрута, который требуется методу действия соответствующего контроллера.</span><span class="sxs-lookup"><span data-stu-id="2da92-225">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="2da92-226">Параметры</span><span class="sxs-lookup"><span data-stu-id="2da92-226">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="2da92-227">Пример</span><span class="sxs-lookup"><span data-stu-id="2da92-227">Example</span></span>

<span data-ttu-id="2da92-228">Чтобы отправить HTTP-запрос POST, выполните указанные ниже действия.</span><span class="sxs-lookup"><span data-stu-id="2da92-228">To issue an HTTP POST request:</span></span>

1. <span data-ttu-id="2da92-229">Выполните команду `post` в конечной точке, которая поддерживает ее:</span><span class="sxs-lookup"><span data-stu-id="2da92-229">Run the `post` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ post -h Content-Type=application/json
    ```

    <span data-ttu-id="2da92-230">В приведенной выше команде в заголовке HTTP-запроса `Content-Type` указан формат текста запроса JSON.</span><span class="sxs-lookup"><span data-stu-id="2da92-230">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="2da92-231">В текстовом редакторе по умолчанию открывается файл *TMP* с шаблоном JSON, представляющим текст HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="2da92-231">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="2da92-232">Например:</span><span class="sxs-lookup"><span data-stu-id="2da92-232">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="2da92-233">Инструкции по определению текстового редактора по умолчанию см. в разделе [Установка текстового редактора по умолчанию](#set-the-default-text-editor).</span><span class="sxs-lookup"><span data-stu-id="2da92-233">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="2da92-234">Измените шаблон JSON в соответствии с требованиями к проверке модели:</span><span class="sxs-lookup"><span data-stu-id="2da92-234">Modify the JSON template to satisfy model validation requirements:</span></span>

  ```json
  {
    "id": 0,
    "name": "Scott Addie"
  }
  ```

1. <span data-ttu-id="2da92-235">Сохраните файл *TMP* и закройте текстовый редактор.</span><span class="sxs-lookup"><span data-stu-id="2da92-235">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="2da92-236">В командной оболочке появятся следующие выходные данные:</span><span class="sxs-lookup"><span data-stu-id="2da92-236">The following output appears in the command shell:</span></span>

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

## <a name="test-http-put-requests"></a><span data-ttu-id="2da92-237">Тестирование HTTP-запросов PUT</span><span class="sxs-lookup"><span data-stu-id="2da92-237">Test HTTP PUT requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="2da92-238">Краткий обзор</span><span class="sxs-lookup"><span data-stu-id="2da92-238">Synopsis</span></span>

```console
put <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="2da92-239">Аргументы</span><span class="sxs-lookup"><span data-stu-id="2da92-239">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="2da92-240">Параметр маршрута, который требуется методу действия соответствующего контроллера.</span><span class="sxs-lookup"><span data-stu-id="2da92-240">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="2da92-241">Параметры</span><span class="sxs-lookup"><span data-stu-id="2da92-241">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="2da92-242">Пример</span><span class="sxs-lookup"><span data-stu-id="2da92-242">Example</span></span>

<span data-ttu-id="2da92-243">Чтобы отправить HTTP-запрос PUT, выполните указанные ниже действия.</span><span class="sxs-lookup"><span data-stu-id="2da92-243">To issue an HTTP PUT request:</span></span>

1. <span data-ttu-id="2da92-244">*Необязательно:* Чтобы просмотреть данные перед их изменением, выполните команду `get`:</span><span class="sxs-lookup"><span data-stu-id="2da92-244">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

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

    <span data-ttu-id="2da92-245">В приведенной выше команде в заголовке HTTP-запроса `Content-Type` указан формат текста запроса JSON.</span><span class="sxs-lookup"><span data-stu-id="2da92-245">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="2da92-246">В текстовом редакторе по умолчанию открывается файл *TMP* с шаблоном JSON, представляющим текст HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="2da92-246">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="2da92-247">Например:</span><span class="sxs-lookup"><span data-stu-id="2da92-247">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="2da92-248">Инструкции по определению текстового редактора по умолчанию см. в разделе [Установка текстового редактора по умолчанию](#set-the-default-text-editor).</span><span class="sxs-lookup"><span data-stu-id="2da92-248">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="2da92-249">Измените шаблон JSON в соответствии с требованиями к проверке модели:</span><span class="sxs-lookup"><span data-stu-id="2da92-249">Modify the JSON template to satisfy model validation requirements:</span></span>

    ```json
    {
      "id": 2,
      "name": "Cherry"
    }
    ```

1. <span data-ttu-id="2da92-250">Сохраните файл *TMP* и закройте текстовый редактор.</span><span class="sxs-lookup"><span data-stu-id="2da92-250">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="2da92-251">В командной оболочке появятся следующие выходные данные:</span><span class="sxs-lookup"><span data-stu-id="2da92-251">The following output appears in the command shell:</span></span>

    ```console
    [main 2019-06-28T17:27:01.805Z] update#setState idle
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:28:21 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="2da92-252">*Необязательно:* Чтобы просмотреть изменения, выполните команду `get`.</span><span class="sxs-lookup"><span data-stu-id="2da92-252">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="2da92-253">Например, если вы ввели "Cherry" в текстовом редакторе, команда `get` вернет следующие выходные данные:</span><span class="sxs-lookup"><span data-stu-id="2da92-253">For example, if you typed "Cherry" in the text editor, a `get` returns the following:</span></span>

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

## <a name="test-http-delete-requests"></a><span data-ttu-id="2da92-254">Тестирование HTTP-запросов DELETE</span><span class="sxs-lookup"><span data-stu-id="2da92-254">Test HTTP DELETE requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="2da92-255">Краткий обзор</span><span class="sxs-lookup"><span data-stu-id="2da92-255">Synopsis</span></span>

```console
delete <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="2da92-256">Аргументы</span><span class="sxs-lookup"><span data-stu-id="2da92-256">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="2da92-257">Параметр маршрута, который требуется методу действия соответствующего контроллера.</span><span class="sxs-lookup"><span data-stu-id="2da92-257">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="2da92-258">Параметры</span><span class="sxs-lookup"><span data-stu-id="2da92-258">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="2da92-259">Пример</span><span class="sxs-lookup"><span data-stu-id="2da92-259">Example</span></span>

<span data-ttu-id="2da92-260">Чтобы отправить HTTP-запрос DELETE, выполните указанные ниже действия.</span><span class="sxs-lookup"><span data-stu-id="2da92-260">To issue an HTTP DELETE request:</span></span>

1. <span data-ttu-id="2da92-261">*Необязательно:* Чтобы просмотреть данные перед их изменением, выполните команду `get`:</span><span class="sxs-lookup"><span data-stu-id="2da92-261">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

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

    <span data-ttu-id="2da92-262">Выходные данные приведенной выше команды имеют следующий формат:</span><span class="sxs-lookup"><span data-stu-id="2da92-262">The preceding command displays the following output format:</span></span>

    ```console
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:36:42 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="2da92-263">*Необязательно:* Чтобы просмотреть изменения, выполните команду `get`.</span><span class="sxs-lookup"><span data-stu-id="2da92-263">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="2da92-264">В этом примере команда `get` возвращает следующие данные:</span><span class="sxs-lookup"><span data-stu-id="2da92-264">In this example, a `get` returns the following:</span></span>

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

## <a name="test-http-patch-requests"></a><span data-ttu-id="2da92-265">Тестирование HTTP-запросов PATCH</span><span class="sxs-lookup"><span data-stu-id="2da92-265">Test HTTP PATCH requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="2da92-266">Краткий обзор</span><span class="sxs-lookup"><span data-stu-id="2da92-266">Synopsis</span></span>

```console
patch <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="2da92-267">Аргументы</span><span class="sxs-lookup"><span data-stu-id="2da92-267">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="2da92-268">Параметр маршрута, который требуется методу действия соответствующего контроллера.</span><span class="sxs-lookup"><span data-stu-id="2da92-268">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="2da92-269">Параметры</span><span class="sxs-lookup"><span data-stu-id="2da92-269">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

## <a name="test-http-head-requests"></a><span data-ttu-id="2da92-270">Тестирование HTTP-запросов HEAD</span><span class="sxs-lookup"><span data-stu-id="2da92-270">Test HTTP HEAD requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="2da92-271">Краткий обзор</span><span class="sxs-lookup"><span data-stu-id="2da92-271">Synopsis</span></span>

```console
head <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="2da92-272">Аргументы</span><span class="sxs-lookup"><span data-stu-id="2da92-272">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="2da92-273">Параметр маршрута, который требуется методу действия соответствующего контроллера.</span><span class="sxs-lookup"><span data-stu-id="2da92-273">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="2da92-274">Параметры</span><span class="sxs-lookup"><span data-stu-id="2da92-274">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="test-http-options-requests"></a><span data-ttu-id="2da92-275">Тестирование HTTP-запросов OPTIONS</span><span class="sxs-lookup"><span data-stu-id="2da92-275">Test HTTP OPTIONS requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="2da92-276">Краткий обзор</span><span class="sxs-lookup"><span data-stu-id="2da92-276">Synopsis</span></span>

```console
options <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="2da92-277">Аргументы</span><span class="sxs-lookup"><span data-stu-id="2da92-277">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="2da92-278">Параметр маршрута, который требуется методу действия соответствующего контроллера.</span><span class="sxs-lookup"><span data-stu-id="2da92-278">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="2da92-279">Параметры</span><span class="sxs-lookup"><span data-stu-id="2da92-279">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="set-http-request-headers"></a><span data-ttu-id="2da92-280">Установка заголовков HTTP-запросов</span><span class="sxs-lookup"><span data-stu-id="2da92-280">Set HTTP request headers</span></span>

<span data-ttu-id="2da92-281">Задать заголовок HTTP-запроса можно одним из следующих способов.</span><span class="sxs-lookup"><span data-stu-id="2da92-281">To set an HTTP request header, use one of the following approaches:</span></span>

1. <span data-ttu-id="2da92-282">Внутри HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="2da92-282">Set inline with the HTTP request.</span></span> <span data-ttu-id="2da92-283">Например:</span><span class="sxs-lookup"><span data-stu-id="2da92-283">For example:</span></span>

  ```console
  https://localhost:5001/people~ post -h Content-Type=application/json
  ```

  <span data-ttu-id="2da92-284">При таком подходе для каждого заголовка HTTP-запроса требуется собственный параметр `-h`.</span><span class="sxs-lookup"><span data-stu-id="2da92-284">With the preceding approach, each distinct HTTP request header requires its own `-h` option.</span></span>

1. <span data-ttu-id="2da92-285">Перед отправкой HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="2da92-285">Set before sending the HTTP request.</span></span> <span data-ttu-id="2da92-286">Например:</span><span class="sxs-lookup"><span data-stu-id="2da92-286">For example:</span></span>

  ```console
  https://localhost:5001/people~ set header Content-Type application/json
  ```

  <span data-ttu-id="2da92-287">Если заголовок задается перед отправкой запроса, он продолжает действовать на протяжении всего сеанса командной оболочки.</span><span class="sxs-lookup"><span data-stu-id="2da92-287">When setting the header before sending a request, the header remains set for the duration of the command shell session.</span></span> <span data-ttu-id="2da92-288">Чтобы очистить заголовок, укажите пустое значение.</span><span class="sxs-lookup"><span data-stu-id="2da92-288">To clear the header, provide an empty value.</span></span> <span data-ttu-id="2da92-289">Например:</span><span class="sxs-lookup"><span data-stu-id="2da92-289">For example:</span></span>

  ```console
  https://localhost:5001/people~ set header Content-Type
  ```

## <a name="toggle-http-request-display"></a><span data-ttu-id="2da92-290">Включение и отключение отображения HTTP-запроса</span><span class="sxs-lookup"><span data-stu-id="2da92-290">Toggle HTTP request display</span></span>

<span data-ttu-id="2da92-291">По умолчанию отправляемый HTTP-запрос не отображается.</span><span class="sxs-lookup"><span data-stu-id="2da92-291">By default, display of the HTTP request being sent is suppressed.</span></span> <span data-ttu-id="2da92-292">Соответствующую настройку можно изменить на всю длительность сеанса командной оболочки.</span><span class="sxs-lookup"><span data-stu-id="2da92-292">It's possible to change the corresponding setting for the duration of the command shell session.</span></span>

### <a name="enable-request-display"></a><span data-ttu-id="2da92-293">Включение отображения запроса</span><span class="sxs-lookup"><span data-stu-id="2da92-293">Enable request display</span></span>

<span data-ttu-id="2da92-294">Чтобы отправляемый HTTP-запрос отображался, выполните команду `echo on`.</span><span class="sxs-lookup"><span data-stu-id="2da92-294">View the HTTP request being sent by running the `echo on` command.</span></span> <span data-ttu-id="2da92-295">Например:</span><span class="sxs-lookup"><span data-stu-id="2da92-295">For example:</span></span>

```console
https://localhost:5001/people~ echo on
Request echoing is on
```

<span data-ttu-id="2da92-296">При отправке последующих HTTP-запросов в рамках текущего сеанса заголовки запросов будут отображаться.</span><span class="sxs-lookup"><span data-stu-id="2da92-296">Subsequent HTTP requests in the current session display the request headers.</span></span> <span data-ttu-id="2da92-297">Например:</span><span class="sxs-lookup"><span data-stu-id="2da92-297">For example:</span></span>

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

### <a name="disable-request-display"></a><span data-ttu-id="2da92-298">Отключение отображения запроса</span><span class="sxs-lookup"><span data-stu-id="2da92-298">Disable request display</span></span>

<span data-ttu-id="2da92-299">Чтобы отправляемый HTTP-запрос не отображался, выполните команду `echo off`.</span><span class="sxs-lookup"><span data-stu-id="2da92-299">Suppress display of the HTTP request being sent by running the `echo off` command.</span></span> <span data-ttu-id="2da92-300">Например:</span><span class="sxs-lookup"><span data-stu-id="2da92-300">For example:</span></span>

```console
https://localhost:5001/people~ echo off
Request echoing is off
```

## <a name="run-a-script"></a><span data-ttu-id="2da92-301">Выполнение скрипта</span><span class="sxs-lookup"><span data-stu-id="2da92-301">Run a script</span></span>

<span data-ttu-id="2da92-302">Если вы часто выполняете один и тот же набор команд HTTP REPL, их можно сохранить в текстовом файле.</span><span class="sxs-lookup"><span data-stu-id="2da92-302">If you frequently execute the same set of HTTP REPL commands, consider storing them in a text file.</span></span> <span data-ttu-id="2da92-303">Команды в файле имеют тот же формат, что и выполняемые вручную в командной строке.</span><span class="sxs-lookup"><span data-stu-id="2da92-303">Commands in the file take the same form as those executed manually on the command line.</span></span> <span data-ttu-id="2da92-304">Их можно выполнять в пакетном режиме с помощью команды `run`.</span><span class="sxs-lookup"><span data-stu-id="2da92-304">The commands can be executed in a batched fashion using the `run` command.</span></span> <span data-ttu-id="2da92-305">Например:</span><span class="sxs-lookup"><span data-stu-id="2da92-305">For example:</span></span>

1. <span data-ttu-id="2da92-306">Создайте текстовый файл с набором команд, разделенных символами новой строки.</span><span class="sxs-lookup"><span data-stu-id="2da92-306">Create a text file containing a set of newline-delimited commands.</span></span> <span data-ttu-id="2da92-307">Например, это может быть файл *people-script.txt* со следующими командами:</span><span class="sxs-lookup"><span data-stu-id="2da92-307">To illustrate, consider a *people-script.txt* file containing the following commands:</span></span>

    ```text
    set base https://localhost:5001
    ls
    cd People
    ls
    get 1
    ```

1. <span data-ttu-id="2da92-308">Выполните команду `run`, передав в нее путь к текстовому файлу.</span><span class="sxs-lookup"><span data-stu-id="2da92-308">Execute the `run` command, passing in the text file's path.</span></span> <span data-ttu-id="2da92-309">Например:</span><span class="sxs-lookup"><span data-stu-id="2da92-309">For example:</span></span>

    ```console
    https://localhost:5001/~ run C:\http-repl-scripts\people-script.txt
    ```

    <span data-ttu-id="2da92-310">Появится следующий результат:</span><span class="sxs-lookup"><span data-stu-id="2da92-310">The following output appears:</span></span>

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

## <a name="clear-the-output"></a><span data-ttu-id="2da92-311">Очистка выходных данных</span><span class="sxs-lookup"><span data-stu-id="2da92-311">Clear the output</span></span>

<span data-ttu-id="2da92-312">Чтобы удалить все выходные данные средства HTTP REPL из командной оболочки, выполните команду `clear` или `cls`.</span><span class="sxs-lookup"><span data-stu-id="2da92-312">To remove all output written to the command shell by the HTTP REPL tool, run the `clear` or `cls` command.</span></span> <span data-ttu-id="2da92-313">Например, предположим, что в командной оболочке есть следующие выходные данные:</span><span class="sxs-lookup"><span data-stu-id="2da92-313">To illustrate, imagine the command shell contains the following output:</span></span>

```console
dotnet httprepl https://localhost:5001
(Disconnected)~ set base "https://localhost:5001"
Using swagger metadata from https://localhost:5001/swagger/v1/swagger.json

https://localhost:5001/~ ls
.        []
Fruits   [get|post]
People   [get|post]

https://localhost:5001/~
```

<span data-ttu-id="2da92-314">Чтобы очистить их, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="2da92-314">Run the following command to clear the output:</span></span>

```console
https://localhost:5001/~ clear
```

<span data-ttu-id="2da92-315">После выполнения приведенной выше команды командная оболочка содержит только следующие выходные данные:</span><span class="sxs-lookup"><span data-stu-id="2da92-315">After running the preceding command, the command shell contains only the following output:</span></span>

```console
https://localhost:5001/~
```

## <a name="additional-resources"></a><span data-ttu-id="2da92-316">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="2da92-316">Additional resources</span></span>

* [<span data-ttu-id="2da92-317">Запросы к REST API</span><span class="sxs-lookup"><span data-stu-id="2da92-317">REST API requests</span></span>](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods)
* [<span data-ttu-id="2da92-318">Репозиторий HTTP REPL в GitHub</span><span class="sxs-lookup"><span data-stu-id="2da92-318">HTTP REPL GitHub repository</span></span>](https://github.com/aspnet/HttpRepl)
