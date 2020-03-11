---
title: Вспомогательная функция тега сценария в ASP.NET Core
author: rick-anderson
ms.author: riande
description: Обнаруживайте атрибуты вспомогательной функции тега сценария ASP.NET Core и роль, которую играет каждый атрибут в расширении поведения тега сценария HTML.
ms.custom: mvc
ms.date: 12/02/2019
uid: mvc/views/tag-helpers/builtin-th/script-tag-helper
ms.openlocfilehash: a037abb6a454e6d06305e7d7f6ecad0c2a0ca717
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78652096"
---
# <a name="script-tag-helper-in-aspnet-core"></a>Вспомогательная функция тега сценария в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT)

[Вспомогательная функция тега сценария](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) создает ссылку на первичный или резервный файл сценария. Обычно первичный файл сценария находится в [сети доставки содержимого](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn) (CDN).

[!INCLUDE[](~/includes/cdn.md)]

Вспомогательная функция тега сценария позволяет указать CDN для файла сценария и резервную копию, если CDN недоступен. Вспомогательная функция тега сценария обеспечивает высокую производительность CDN с надежностью локального размещения.

В следующей разметке Razor показан элемент `script` с резервной копией:

```html
<script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-3.3.1.min.js"
        asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
        asp-fallback-test="window.jQuery"
        crossorigin="anonymous"
        integrity="sha384-tsQFqpEReu7ZLhBV2VZlAu7zcOV+rXbYlF2cqB8txI/8aZajjp4Bqd+V6D5IgvKT">
</script>
```

Не используйте атрибут `<script>`defer[ элемента ](https://developer.mozilla.org/docs/Web/HTML/Element/script), чтобы отложить загрузку скрипта CDN. Вспомогательная функция тега скрипта обрабатывает код JavaScript, который сразу же выполняет выражение [asp-fallback-test](#asp-fallback-test). Это выражение дает сбой, если загрузка скрипта CDN отложена.

## <a name="commonly-used-script-tag-helper-attributes"></a>Часто используемые атрибуты вспомогательной функции тега сценария

Все атрибуты, свойства и методы вспомогательной функции тега сценария см. в статье [ScriptTagHelper Class](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) (Класс ScriptTagHelper).

### <a name="asp-fallback-test"></a>asp-fallback-test

Метод скрипта, определенный в основном скрипте, для использования в тесте резервного экземпляра. Дополнительные сведения см. в разделе <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper.FallbackTestExpression>.

### <a name="asp-fallback-src"></a>asp-fallback-src

URL-адрес тега Script, на который можно перейти в случае сбоя основного URL-адреса. Дополнительные сведения см. в разделе <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper.FallbackSrc>.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/controllers/areas>
* <xref:razor-pages/index>
* <xref:mvc/compatibility-version>
