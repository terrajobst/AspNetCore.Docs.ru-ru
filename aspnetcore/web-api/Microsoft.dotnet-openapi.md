---
title: Разработка приложений ASP.NET Core с использованием OpenAPI
author: ryanbrandenburg
description: Здесь демонстрируется, как добавить ссылки в файлы OpenAPI с использованием средства Microsoft.dotnet-openapi.
ms.author: rybrande
ms.date: 09/26/2019
monikerRange: '>= aspnetcore-3.0'
uid: web-api/Microsoft.dotnet-openapi
ms.openlocfilehash: 079e36511b63c186ffa7726bdb1e3c3bcbda9d34
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78651250"
---
# <a name="develop-aspnet-core-apps-using-openapi-tools"></a>Разработка приложений ASP.NET Core с использованием средств OpenAPI

Автор: Райан Бранденбург (Ryan Brandenburg)

[Microsoft.dotnet-openapi](https://www.nuget.org/packages/Microsoft.dotnet-openapi) — это [глобальное средство .NET Core](/dotnet/core/tools/global-tools) для управления ссылками [OpenAPI](https://github.com/OAI/OpenAPI-Specification) в рамках проекта.

## <a name="installation"></a>Установка

Чтобы установить `Microsoft.dotnet-openapi`, выполните следующую команду:

```dotnetcli
dotnet tool install -g Microsoft.dotnet-openapi
```

## <a name="add"></a>Добавить

При добавлении ссылки OpenAPI с помощью любой из команд на этой странице в файл `<OpenApiReference />`CSPROJ*добавляется элемент*, аналогичный приведенному ниже:

```xml
<OpenApiReference Include="openapi.json" />
```

Для вызова созданного клиентского кода приложению требуется предыдущая ссылка.

<!-- TODO: Restore after https://github.com/dotnet/AspNetCore/issues/12738
### Add Project

#### Options

| Short option | Long option | Description | Example |
|-------|------|-------|---------|
| -p|--project | The project to operate on. |dotnet openapi add project *--project .\Ref.csproj* ../Ref/ProjRef.csproj |

#### Arguments

|  Argument  | Description | Example |
|-------------|-------------|---------|
| source-file | The source to create a reference from. Must be a project file. |dotnet openapi add project *../Ref/ProjRef.csproj* | -->

### <a name="add-file"></a>Добавить файл

#### <a name="options"></a>Параметры

| Короткий параметр| Длинный параметр| Description | Пример |
|-------|------|-------|---------|
| -p|--updateProject | Проект для выполнения операции. |dotnet openapi add file *--updateProject .\Ref.csproj* .\OpenAPI.json |
| -c|--code-generator| Генератор кода, применяемый к ссылке. Возможные значения: `NSwagCSharp` и `NSwagTypeScript`. Если атрибут `--code-generator` не задан, по умолчанию для средств будет выбрано `NSwagCSharp`.|dotnet openapi add file .\OpenApi.json --code-generator
| -H|--help|Отображение справочных сведений.|dotnet openapi add file --help|

#### <a name="arguments"></a>Аргументы

|  Аргумент  | Description | Пример |
|-------------|-------------|---------|
| source-file | Источник, из которого создается ссылка. Должен быть файлом OpenAPI. |dotnet openapi add file *.\OpenAPI.json* |

### <a name="add-url"></a>Добавление URL-адреса

#### <a name="options"></a>Параметры

| Короткий параметр| Длинный параметр| Description | Пример |
|-------|------|-------------|---------|
| -p|--updateProject | Проект для выполнения операции. |dotnet openapi add url *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json` |
| -o|--output-file | Место размещения локальной копии файла OpenAPI. |dotnet openapi add url `https://contoso.com/openapi.json` *--output-file myclient.json* |
| -c|--code-generator| Генератор кода, применяемый к ссылке. Возможные значения: `NSwagCSharp` и `NSwagTypeScript`. |dotnet openapi add file .\OpenApi.json --code-generator
| -H|--help|Отображение справочных сведений.|dotnet openapi add url --help|

#### <a name="arguments"></a>Аргументы

|  Аргумент  | Description | Пример |
|-------------|-------------|---------|
| source-URL | Источник, из которого создается ссылка. Должен быть URL-адресом. |dotnet openapi add url `https://contoso.com/openapi.json` |

## <a name="remove"></a>Удалить

Удаляет ссылку на OpenAPI, соответствующую заданному имени файла, из файла *CSPROJ*. При удалении ссылки OpenAPI клиенты не будут создаваться. Локальные файлы *JSON* и *YAML* удаляются.

### <a name="options"></a>Параметры

| Короткий параметр| Длинный параметр| Description| Пример |
|-------|------|------------|---------|
| -p|--updateProject | Проект для выполнения операции. |dotnet openapi remove *--updateProject .\Ref.csproj* .\OpenAPI.json |
| -H|--help|Отображение справочных сведений.|dotnet openapi remove --help|

### <a name="arguments"></a>Аргументы

|  Аргумент  | Description| Пример |
| ------------|------------|---------|
| source-file | Источник, ссылку на который необходимо удалить. |dotnet openapi remove *.\OpenAPI.json* |

## <a name="refresh"></a>Обновить

Обновляет локальную версию файла, скачанного с использованием последнего содержимого из URL-адреса для скачивания.

### <a name="options"></a>Параметры

| Короткий параметр| Длинный параметр| Description | Пример |
|-------|------|-------------|---------|
| -p|--updateProject | Проект для выполнения операции. | dotnet openapi refresh *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json` |
| -H|--help|Отображение справочных сведений.|dotnet openapi refresh --help|

### <a name="arguments"></a>Аргументы

|  Аргумент  | Description | Пример |
| ------------|-------------|---------|
| source-URL | URL-адрес, ссылку из которого необходимо обновить. | dotnet openapi refresh `https://contoso.com/openapi.json` |
