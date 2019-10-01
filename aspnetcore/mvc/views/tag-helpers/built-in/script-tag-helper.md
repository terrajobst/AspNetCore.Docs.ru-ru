---
title: Вспомогательная функция тега сценария в ASP.NET Core
author: rick-anderson
ms.author: riande
description: Обнаруживайте атрибуты вспомогательной функции тега сценария ASP.NET Core и роль, которую играет каждый атрибут в расширении поведения тега сценария HTML.
ms.custom: mvc
ms.date: 12/18/2018
uid: mvc/views/tag-helpers/builtin-th/script-tag-helper
ms.openlocfilehash: 5f2fb8a45048804afa8aff2989cd53489e45a33b
ms.sourcegitcommit: fae6f0e253f9d62d8f39de5884d2ba2b4b2a6050
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/25/2019
ms.locfileid: "71256468"
---
# <a name="script-tag-helper-in-aspnet-core"></a>Вспомогательная функция тега сценария в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

[Вспомогательная функция тега сценария](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) создает ссылку на первичный или резервный файл сценария. Обычно первичный файл сценария находится в [сети доставки содержимого](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn) (CDN).

[!INCLUDE[](~/includes/cdn.md)]

Вспомогательная функция тега сценария позволяет указать CDN для файла сценария и резервную копию, если CDN недоступен. Вспомогательная функция тега сценария обеспечивает высокую производительность CDN с надежностью локального размещения.

В следующей разметке Razor показан элемент `script` файла макета, созданного с помощью шаблона веб-приложения ASP.NET Core:

[!code-html[](link-tag-helper/sample/_Layout.cshtml?name=snippet2)]

Следующий код похож на отображаемый HTML из предыдущего кода (в среде, не являющейся средой разработки):

[!code-csharp[](link-tag-helper/sample/HtmlPage2.html)]

В приведенном выше коде вспомогательная функция тега сценария создала второй элемент сценария (`<script>  (window.jQuery || document.write(`), который проверяет на наличие `window.jQuery`. Если `window.jQuery` не найден, `document.write(` выполняется и создает сценарий. 

## <a name="commonly-used-script-tag-helper-attributes"></a>Часто используемые атрибуты вспомогательной функции тега сценария

Все атрибуты, свойства и методы вспомогательной функции тега сценария см. в статье [ScriptTagHelper Class](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) (Класс ScriptTagHelper).

### <a name="href"></a>href

Предпочтительный адрес связанного ресурса. Адрес передается в созданный код HTML во всех случаях.

### <a name="asp-fallback-href"></a>asp-fallback-href

URL-адрес таблицы стилей CSS, к которой можно перейти в случае сбоя основного URL-адреса.

### <a name="asp-fallback-test-class"></a>asp-fallback-test-class

Имя класса, определенное в таблице стилей для использования в качестве теста резервного экземпляра. Дополнительные сведения можно найти по адресу: <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestClass>.

### <a name="asp-fallback-test-property"></a>asp-fallback-test-property

Имя свойства CSS, используемое для теста резервного экземпляра. Дополнительные сведения можно найти по адресу: <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestProperty>.

### <a name="asp-fallback-test-value"></a>asp-fallback-test-value

Значение свойства CSS, используемое для теста резервного экземпляра. Дополнительные сведения можно найти по адресу: <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue>.

### <a name="asp-fallback-test-value"></a>asp-fallback-test-value

Значение свойства CSS, используемое для теста резервного экземпляра. Дополнительные сведения см. в разделе <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue>.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/controllers/areas>
* <xref:razor-pages/index>
* <xref:mvc/compatibility-version>
