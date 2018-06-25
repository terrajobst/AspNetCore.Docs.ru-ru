---
title: Начало работы с NSwag и ASP.NET Core
author: zuckerthoben
description: Узнайте, как использовать NSwag для создания документации и страниц справки для веб-API в ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 05/08/2018
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: f4cc9ec1f32ef2bd0056ba8d0cbbbe9228834d85
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279208"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a>Начало работы с NSwag и ASP.NET Core

Авторы: [Кристоф Ниенабер (Christoph Nienaber)](https://twitter.com/zuckerthoben) и [Рико Сутер (Rico Suter)](https://rsuter.com)

::: moniker range="<= aspnetcore-2.0"
[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))
::: moniker-end

Для использования [NSwag](https://github.com/RSuter/NSwag) с ПО промежуточного слоя ASP.NET Core требуется пакет NuGet [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/). Пакет состоит из генератора Swagger, пользовательского интерфейса Swagger (v2 и v3) и [пользовательского интерфейса ReDoc](https://github.com/Rebilly/ReDoc).

Настоятельно рекомендуется использовать возможности создания кода в NSwag. Выберите один из следующих вариантов создания кода:

* Используйте [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), классическое приложение Windows для создания клиентского кода в C# и TypeScript для вашего API.
* Используйте пакеты NuGet [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) или [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/), чтобы создавать код внутри проекта.
* Используйте NSwag из [командной строки](https://github.com/NSwag/NSwag/wiki/CommandLine).
* Используйте пакет NuGet [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild).

## <a name="features"></a>Функции

Основная причина применения NSwag — возможность не только ввести пользовательский интерфейс Swagger и генератор Swagger, но и использовать гибкие возможности создания кода. Вам не требуется существующий API &mdash; вы можете использовать API сторонних разработчиков, которые содержат Swagger и позволяют NSwag создавать реализацию клиента. В любом случае цикл разработки ускоряется, и вам проще адаптироваться к изменениям API.

## <a name="package-installation"></a>Установка пакета

Пакет NuGet NSwag можно добавить одним из описанных ниже способов.

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* В окне **Консоль диспетчера пакетов**
  * Перейдите в раздел **Представление** > **Другие окна** > **Консоль диспетчера пакетов**
  * Перейдите в каталог, в котором существует файл *TodoApi.csproj*
  * Выполните следующую команду:

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* В диалоговом окне **Управление пакетами NuGet**
  * Щелкните правой кнопкой мыши проект в **обозревателе решений** > **Управление пакетами NuGet**
  * В качестве **источника пакета** выберите "nuget.org".
  * В поле поиска введите "NSwag.AspNetCore"
  * Выберите пакет "NSwag.AspNetCore" на вкладке **Обзор** и нажмите **Установить**

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio для Mac](#tab/visual-studio-mac)

* Щелкните правой кнопкой мыши папку *Пакеты* на **панели решения** > **Добавление пакетов...**.
* В раскрывающемся списке **Источник** в окне **Добавление пакетов** выберите вариант "nuget.org".
* В поле поиска введите "NSwag.AspNetCore"
* В области результатов выберите пакет "NSwag.AspNetCore" и нажмите **Добавить пакет**

### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code)

Во **встроенном терминале** выполните следующую команду.

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

### <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

Выполните следующую команду:

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a>Добавление и настройка ПО промежуточного слоя Swagger

Импортируйте следующие пространства имен в класс `Startup`:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_StartupConfigureImports)]

В методе `Startup.Configure` включите ПО промежуточного слоя для обслуживания созданной спецификации Swagger и пользовательского интерфейса Swagger:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-10)]

Запустите приложение. Перейдите к `http://localhost:<port>/swagger` для просмотра пользовательского интерфейса Swagger. Перейдите к `http://localhost:<port>/swagger/v1/swagger.json` для просмотра спецификации Swagger.

## <a name="code-generation"></a>Создание кода

### <a name="via-nswagstudio"></a>Через NSwagStudio

* Установите NSwagStudio из официального [репозитория GitHub](https://github.com/RSuter/NSwag/wiki/NSwagStudio).
* Запустите NSwagStudio. Введите URL-адрес файла *swagger.json* в текстовое поле **URL-адрес спецификации Swagger** и нажмите кнопку **Создать локальную копию**.
* Выберите тип выходных данных **Клиент CSharp**. Другие доступные варианты: **клиент TypeScript** и **контроллер веб-API CSharp**. Использование контроллера веб-API по сути является обратным созданием. Служба воссоздается по спецификации службы.
* Нажмите кнопку **Создать выходные данные**. Будет создана полная клиентская реализация проекта *TodoApi.NSwag* на языке C#. Откройте вкладку **Клиент CSharp** в разделе **Выходные данные**, чтобы просмотреть созданный клиентский код:

```csharp
//----------------------
// <auto-generated>
//     Generated using the NSwag toolchain v11.17.3.0 (NJsonSchema v9.10.46.0 (Newtonsoft.Json v9.0.0.0)) (http://NSwag.org)
// </auto-generated>
//----------------------

namespace MyNamespace
{
    #pragma warning disable // Disable all warnings

    [System.CodeDom.Compiler.GeneratedCode("NSwag",
        "11.17.3.0 (NJsonSchema v9.10.46.0 (Newtonsoft.Json v9.0.0.0))")]
    public partial class TodoClient
    {
        private string _baseUrl = "http://localhost:50499";
        private System.Lazy<Newtonsoft.Json.JsonSerializerSettings> _settings;

        public TodoClient()
        {
            _settings = new System.Lazy
                <Newtonsoft.Json.JsonSerializerSettings>(() =>
            {
                var settings = new Newtonsoft.Json.JsonSerializerSettings();
                UpdateJsonSerializerSettings(settings);
                return settings;
            });
        }

        public string BaseUrl
        {
            get { return _baseUrl; }
            set { _baseUrl = value; }
        }

        // code omitted for brevity
```

> [!TIP]
> Клиентский код C# создается на основе параметров, определенных на вкладке **Параметры** вкладки **Клиент CSharp**. Измените параметры для выполнения таких задач, как переименование пространства имен по умолчанию и создание синхронного метода.

* Скопируйте созданный код C# в файл в клиентском проекте (например, приложение [Xamarin.Forms](/xamarin/xamarin-forms/)).
* Начните использовать веб-API:

```csharp
var todoClient = new TodoClient();

// Gets all to-dos from the API
var allTodos = await todoClient.GetAllAsync();

// Create a new TodoItem, and save it in the API
var createdTodo = await todoClient.CreateAsync(new TodoItem());

// Get a single to-do by ID
var foundTodo = await todoClient.GetByIdAsync(1);
```

> [!NOTE]
> Вы можете вставить базовый URL-адрес и (или) HTTP-клиент в API-клиент. Рекомендуется всегда [повторно использовать HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).

### <a name="other-ways-to-generate-client-code"></a>Другие способы создания кода клиента

Можно создать клиентский код другими способами, более подходящими для вашего рабочего процесса:

* [MSBuild](https://www.nuget.org/packages/NSwag.MSBuild/)

* [В коде](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [Шаблоны T4](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customize"></a>Настройка

Swagger предоставляет параметры для документирования объектной модели и упрощения использования веб-API.

### <a name="api-info-and-description"></a>Данные и описание API

В методе `Startup.Configure` действие по настройке, передаваемое в метод `UseSwagger`, можно использовать для добавления таких сведений, как автор, лицензия и описание:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_UseSwagger)]

Пользовательский интерфейс Swagger отображает сведения о версии:

![Пользовательский интерфейс Swagger с информацией о версии](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a>XML-комментарии

XML-комментарии можно включить следующим образом.

# <a name="visual-studiotabvisual-studio-xml"></a>[Visual Studio](#tab/visual-studio-xml/)

* В **обозревателе решений** щелкните проект правой кнопкой мыши и выберите пункт **Свойства**.
* Установите флажок **Файл XML-документации** в разделе **Вывод** на вкладке **Сборка**

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[Visual Studio для Mac](#tab/visual-studio-mac-xml/)

* Откройте диалоговое окно **Параметры проекта** > **Сборка** > **Компилятор**.
* Установите флажок **Сформировать XML-документацию** в разделе **Общие параметры**

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[Visual Studio Code.](#tab/visual-studio-code-xml/)

Вручную добавьте в файл *.csproj* следующий фрагмент кода:

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement)]

---

### <a name="data-annotations"></a>Заметки к данным

::: moniker range="<= aspnetcore-2.0"
NSwag использует [Отражение](/dotnet/csharp/programming-guide/concepts/reflection), и для действий веб-API рекомендуется возвращать значение типа [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult). Следовательно, NSwag не удается определить, что делает ваше действие и что оно возвращает. Рассмотрим следующий пример.

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

Предыдущее действие возвращает `IActionResult`, но внутри действия возвращается [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) или [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest). Заметки к данным используются, чтобы сообщить клиентам, какие коды состояния HTTP возвращает это действие. Добавьте к действию следующие атрибуты:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
NSwag использует [Отражение](/dotnet/csharp/programming-guide/concepts/reflection), и для действий веб-API рекомендуется возвращать значение типа [IActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1). Следовательно, NSwag может вывести только тип возвращаемого значения, определенный `T`. Другие типы возвращаемых значений в действии вывести невозможно. Рассмотрим следующий пример.

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

Предыдущее действие возвращает `ActionResult<T>`, но внутри действия возвращается [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute). Поскольку контроллер дополнен атрибутом [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute), ответ [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) также возможен. Дополнительные сведения см. в разделе [Автоматические отклики HTTP 400](xref:web-api/index#automatic-http-400-responses). Заметки к данным используются, чтобы сообщить клиентам, какие коды состояния HTTP возвращает это действие. Добавьте к действию следующие атрибуты:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]
::: moniker-end

Генератор Swagger теперь может точно описать это действие, и созданные клиенты знают, что они получают при вызове конечной точки. Настоятельно рекомендуется добавлять эти атрибуты ко всем действиям. Рекомендации о том, какие ответы HTTP должны возвращать ваши действия API, см. в [спецификации RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).
