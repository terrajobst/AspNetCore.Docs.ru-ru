---
title: Использование соглашений веб-API
author: pranavkm
description: Сведения о соглашениях веб-API в ASP.NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: pranavkm
ms.custom: mvc
ms.date: 11/13/2018
uid: web-api/advanced/conventions
ms.openlocfilehash: ede9a46c160cf6a49aa93da710af0bf0b8f59acc
ms.sourcegitcommit: c4572be5ebb301013a5698caf9b5572b76cb2e34
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/30/2018
ms.locfileid: "52710079"
---
# <a name="use-web-api-conventions"></a>Использование соглашений веб-API

В ASP.NET Core 2.2 добавлен способ извлечения распространенной [документации по API](xref:tutorials/web-api-help-pages-using-swagger) и ее применения к нескольким действиям, контроллеру или всем контроллерам в рамках сборки. Соглашения веб-API заменяют применение атрибута [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute) к отдельным действиям. Это позволяет определять наиболее распространенные стандартные типы возвращаемых значений и коды состояния, возвращаемые из действия, а также способ выбора метода соглашения, который применяется к действию.

По умолчанию в состав ASP.NET Core MVC 2.2 входит набор стандартных соглашений `Microsoft.AspNetCore.Mvc.DefaultApiConventions`. В основу соглашений положен контроллер, который формируется в ASP.NET Core. Если ваши действия соответствуют схеме, являющейся результатом формирования шаблонов, вы сможете успешно использовать соглашения по умолчанию.

Во время выполнения <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> понимает соглашения. `ApiExplorer` является абстракцией MVC для взаимодействия с генераторами документов OpenAPI. Атрибуты из примененного соглашения связываются с действием и включаются в документацию Swagger для действия. Анализаторы API также понимают соглашения. Если ваше действие является нестандартным (например, возвращает код состояния, который не задокументирован примененным соглашением), оно выводит предупреждение, позволяя задокументировать его.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="apply-web-api-conventions"></a>Применение соглашений веб-API

Существует три способа применения соглашения. Соглашения не являются составными. Каждое действие может быть связано только с одним соглашением. Более конкретные соглашения (см. ниже) имеют приоритет над менее определенными. Если к действию применяются два или более соглашения с одинаковым приоритетом, выбор осуществляется недетерминированным образом. Существуют следующие варианты применения соглашения к действию (от наиболее конкретного до наименее конкретного):

1. `Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; Применяется к отдельным действиям и указывает тип соглашения и применимый метод соглашения. В следующем примере показано применение метода соглашения `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` к действию `Update`:

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=apiconventionmethod&highlight=2-3)]

1. `Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute`, примененный к контроллеру &mdash;, применяет тип соглашения ко всем действиям в контроллере. Методы соглашения дополняются подсказками, которые определяют, к каким действиям они будут применены (сведения в рамках соглашений о создании). Пример:

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=apiconventiontypeattribute)]

1. `Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute`, примененный к сборке &mdash;, применяет тип соглашения ко всем контроллерам в текущей сборке. Пример:

    [!code-csharp[](conventions/sample/Startup.cs?name=apiconventiontypeattribute)]

## <a name="create-web-api-conventions"></a>Создание соглашений веб-API

Соглашение — это статический тип с методами. Эти методы помечаются атрибутами `[ProducesResponseType]` или `[ProducesDefaultResponseType]`.

```csharp
public static class MyAppConventions
{
    [ProducesResponseType(200)]
    [ProducesResponseType(404)]
    public static void Find(int id)
    {
    }
}
```

Результатом применения этого соглашения к сборке является метод соглашения, применяемый к любому действию с именем `Find` и имеющий только один параметр `id` до тех пор, пока не появятся другие более конкретные атрибуты метаданных.

Помимо `[ProducesResponseType]` и `[ProducesDefaultResponseType]` к методу соглашения, определяющему методы, к которым они применяются, можно применять `[ApiConventionNameMatch]` и `[ApiConventionTypeMatch]`. Пример:

```csharp
[ProducesResponseType(200)]
[ProducesResponseType(404)]
[ApiConventionNameMatch(ApiConventionNameMatchBehavior.Prefix)]
public static void Find(
    [ApiConventionNameMatch(ApiConventionNameMatchBehavior.Suffix)]
    int id)
{ }
```

* Параметр `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix`, примененный к методу, указывает, что соглашение может соответствовать любому действию до тех пор, пока у него есть префикс "Find". Это такие методы, как `Find`, `FindPet` или `FindById`.
* `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix`, примененный к параметру, указывает, что соглашение может соответствовать методам только с одним параметром, заканчивающимся в идентификаторе суффикса. Это такие параметры, как `id` или `petId`. `ApiConventionTypeMatch` может аналогичным образом применяться к типам для ограничения типа параметра. Аргумент `params[]` можно использовать для указания остальных параметров, которым не требуется явное сопоставление.
