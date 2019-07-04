---
title: Использование анализаторов веб-API
author: pranavkm
description: Сведения об анализаторах веб-API в Microsoft.AspNetCore.Mvc.Api.Analyzers.
monikerRange: '>= aspnetcore-2.2'
ms.author: prkrishn
ms.custom: mvc
ms.date: 12/14/2018
uid: web-api/advanced/analyzers
ms.openlocfilehash: 2aaef738ab2a64f85cb85708f63d2375c04cacb5
ms.sourcegitcommit: 0b9e767a09beaaaa4301915cdda9ef69daaf3ff2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2019
ms.locfileid: "67538565"
---
# <a name="use-web-api-analyzers"></a>Использование анализаторов веб-API

В ASP.NET Core 2.2 и более поздних версий представлен пакет NuGet [Microsoft.AspNetCore.Mvc.Api.Analyzers](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Api.Analyzers), содержащий анализаторы для веб-API. Анализаторы работают с контроллерами, которые аннотированы <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute>. Кроме того, к ним применяются [соглашения об использовании API](xref:web-api/advanced/conventions).

## <a name="package-installation"></a>Установка пакета

`Microsoft.AspNetCore.Mvc.Api.Analyzers` можно добавить одним из описанных ниже способов.

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* В окне **Консоль диспетчера пакетов**
  * Перейдите в раздел **Представление** > **Другие окна** > **Консоль диспетчера пакетов**.
  * Перейдите в каталог, в котором находится файл *ApiConventions.csproj*
  * Выполните следующую команду:

    ```powershell
    Install-Package Microsoft.AspNetCore.Mvc.Api.Analyzers
    ```

* В диалоговом окне **Управление пакетами NuGet**
  * Щелкните правой кнопкой мыши проект в **обозревателе решений** > **Управление пакетами NuGet**.
  * В качестве **источника пакета** выберите "nuget.org".
  * В поле поиска введите "Microsoft.AspNetCore.Mvc.Api.Analyzers".
  * Выберите пакет "Microsoft.AspNetCore.Mvc.Api.Analyzers" на вкладке **Обзор** и нажмите кнопку **Установить**.

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio для Mac](#tab/visual-studio-mac)

* Щелкните правой кнопкой мыши папку *Пакеты* на **панели решения** > **Добавление пакетов…** .
* В раскрывающемся списке **Источник** в окне **Добавление пакетов** выберите вариант "nuget.org".
* В поле поиска введите "Microsoft.AspNetCore.Mvc.Api.Analyzers".
* В области результатов выберите пакет "Microsoft.AspNetCore.Mvc.Api.Analyzers", а затем нажмите кнопку **Добавить пакет**.

### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code)

Во **встроенном терминале** выполните следующую команду.

```console
dotnet add ApiConventions.csproj package Microsoft.AspNetCore.Mvc.Api.Analyzers
```

### <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

Выполните следующую команду:

```console
dotnet add ApiConventions.csproj package Microsoft.AspNetCore.Mvc.Api.Analyzers
```

---

## <a name="analyzers-for-api-conventions"></a>Анализаторы для соглашений API

Документы OpenAPI содержат коды состояний и типы ответов, которые может возвращать действие. В ASP.NET Core MVC атрибуты, такие как <xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute> и <xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>, используются для документирования действия. <xref:tutorials/web-api-help-pages-using-swagger> позволяет более подробно документировать API.

Один из анализаторов в пакете проверяет контроллеры, которые аннотированы <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute>, и определяет действия, которые не полностью документируют их ответы. Рассмотрим следующий пример.

[!code-csharp[](conventions/sample/Controllers/ContactsController.cs?name=missing404docs&highlight=9)]

Предыдущее действие документирует тип возврата "Успех HTTP 200", но не документирует код состояния "Ошибка HTTP 404". Анализатор сообщает об отсутствующем документировании для кода состояния HTTP 404 в виде предупреждения. Эту проблему можно устранить.

![Предупреждение анализатора](conventions/_static/Analyzer.gif)

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:web-api/advanced/conventions>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:web-api/index>
