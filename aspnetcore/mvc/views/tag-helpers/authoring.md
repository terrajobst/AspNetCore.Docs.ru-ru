---
title: Создание вспомогательных функций тегов в ASP.NET Core
author: rick-anderson
description: Сведения о разработке вспомогательных функций тегов в ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 01/19/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/authoring
ms.openlocfilehash: 85809a0f7bfe0adcf535c2be7f5b77e8abd947fd
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
ms.locfileid: "32741002"
---
# <a name="author-tag-helpers-in-aspnet-core"></a>Создание вспомогательных функций тегов в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="get-started-with-tag-helpers"></a>Начало работы с вспомогательными функциями тегов

В этом учебнике приводятся основные сведения о программировании вспомогательных функций тегов. В статье [Общие сведения о вспомогательных функциях тегов](intro.md) описываются преимущества, предоставляемые вспомогательными функциями тегов.

Вспомогательная функция тега — это любой класс, реализующий интерфейс `ITagHelper`. Однако при разработке вспомогательной функции тега обычно производится наследование `TagHelper`, так как это дает доступ к методу `Process`.

1. Создайте проект ASP.NET Core с именем **AuthoringTagHelpers**. Проверка подлинности для этого проекта не потребуется.

2. Создайте папку, в которой будут помещаться вспомогательные функции тегов, с именем *TagHelpers*. Папка *TagHelpers* *не* является обязательной; она создается согласно принятому соглашению. Теперь приступим к написанию простейших вспомогательных функций тегов.

## <a name="a-minimal-tag-helper"></a>Простейшая вспомогательная функция тега

В этом разделе вы напишете вспомогательную функцию тега, которая обновляет тег электронной почты. Пример:

```html
<email>Support</email>
   ```

Сервер будет использовать эту вспомогательную функцию тега для преобразования разметки следующим образом:

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

Это тег привязки, который преобразует элемент в ссылку с адресом электронной почты. Он может потребоваться, если вы создаете подсистему ведения блогов и вам нужно реализовать отправку электронной почты в отдел маркетинга, службу технической поддержки и на другие адреса в одном домене.

1. Добавьте приведенный ниже класс `EmailTagHelper` в папку *TagHelpers*.

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]
    
   **Примечания.**
    
   * Для вспомогательных функций тегов используется соглашение об именовании в отношении элементов имени корневого класса (за исключением части *TagHelper* этого имени). В этом примере корневым именем класса **Email**TagHelper является *email*, поэтому целевым является тег `<email>`. Это соглашение об именовании подходит для большинства вспомогательных функций тегов, позднее мы покажем, как переопределить его.
    
   * Класс `EmailTagHelper` является производным от класса `TagHelper`. Класс `TagHelper` предоставляет методы и свойства для написания вспомогательных функций тегов.
    
   * Переопределенный `Process` метод управляет действиями, выполняемыми вспомогательной функцией тега. Класс `TagHelper` также предоставляет асинхронную версию (`ProcessAsync`) с теми же параметрами.
    
   * Параметр контекста метода `Process` (и `ProcessAsync`) содержит сведения, связанные с выполнением текущего HTML-тега.
    
   * Выходной параметр метода `Process` (и `ProcessAsync`) содержит элемент HTML с отслеживанием состояния, который представляет источник, используемый для формирования HTML-тега и содержимого.
    
   * В нашем случае имя класса имеет суффикс **TagHelper**, который *не* является обязательным, но считается рекомендуемым соглашением. Класс можно объявить следующим образом:
    
   ```csharp
   public class Email : TagHelper
   ```

2. Чтобы сделать класс `EmailTagHelper` доступным для всех представлений Razor, добавьте директиву `addTagHelper` в файл *Views/_ViewImports.cshtml*: [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]
    
   В приведенном выше коде используется синтаксис с подстановочным знаком, который указывает, что будут доступны все вспомогательные функции тегов в сборке. Первая строка после `@addTagHelper` указывает на загружаемую вспомогательную функцию тега (символ "*" соответствует всем вспомогательным функциям тегов), а вторая строка ("AuthoringTagHelpers") указывает на сборку, в которой находится вспомогательная функция тега. Кроме того, обратите внимание на то, что во второй строке с помощью синтаксиса с подстановочным знаком добавляются вспомогательные функции тегов ASP.NET Core MVC (эти вспомогательные функции рассматриваются в статье [Общие сведения о вспомогательных функциях тегов](intro.md)). Вспомогательная функция тега становится доступной для представления Razor посредством директивы `@addTagHelper`. Вы также можете указать полное имя вспомогательной функции тега, как показано ниже.
    
```csharp
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```
    
<!--
the following snippet uses TagHelpers3 and should use TagHelpers (not the 3)
    [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]
-->
    
Чтобы добавить вспомогательную функцию тега в представление с помощью полного имени, сначала добавьте полное имя (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), а затем — имя сборки (*AuthoringTagHelpers*). Большинство разработчиков предпочитают использовать синтаксис с подстановочным знаком. В статье [Общие сведения о вспомогательных функциях тегов](intro.md) подробно рассматривается добавление и удаление вспомогательных функций тегов, их иерархия и синтаксис с подстановочным знаком.
    
3. Внесите следующие изменения в разметку в файле *Views/Home/Contact.cshtml*:

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

4. Запустите приложение и просмотрите исходный текст HTML в любимом браузере, чтобы проверить, были ли теги электронной почты заменены разметкой привязки (например, `<a>Support</a>`). Строки *Support* и *Marketing* отображаются как ссылки, но у них нет атрибута `href`, поэтому ссылки будут нерабочими. Мы исправим это в следующем разделе.

## <a name="setattribute-and-setcontent"></a>SetAttribute и SetContent

В этом разделе мы изменим функцию `EmailTagHelper` так, чтобы она создавала правильный тег привязки для адреса электронной почты. Она будет извлекать сведения из представления Razor (в виде атрибута `mail-to`) и использовать их для создания привязки.

Измените класс `EmailTagHelper` следующим образом:

[!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]

**Примечания.**

* Имена классов и свойств, указанные в стиле Pascal, для вспомогательных функций тегов преобразуются в [кебаб-стиль нижнего регистра](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101). Поэтому для использования атрибута `MailTo` следует использовать эквивалент `<email mail-to="value"/>`.

* В последней строке задается готовое содержимое для нашей простейшей функциональной вспомогательной функции тега.

* В выделенной строке показан синтаксис добавления атрибутов:

[!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]

Такой подход работает для атрибута href, если его в настоящее время нет в коллекции атрибутов. Вы также можете добавить атрибут вспомогательной функции тега в конец коллекции атрибутов тегов с помощью метода `output.Attributes.Add`.

1. Внесите следующие изменения в разметку в файле *Views/Home/Contact.cshtml*: [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]

2. Запустите приложение и убедитесь в том, что ссылки создаются правильно.
    
   > [!NOTE]
   > Если бы вам нужно было написать самозакрывающийся тег электронной почты (`<email mail-to="Rick" />`), выходной тег также был бы самозакрывающимся. Чтобы получить возможность создавать теги с помощью только открывающего тега (`<email mail-to="Rick">`), класс необходимо декорировать следующим образом:
   > 
   > [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]
    
   При использовании самозакрывающейся вспомогательной функции тега электронной почты результат будет следующим: `<a href="mailto:Rick@contoso.com" />`. Самозакрывающиеся теги привязки не являются допустимым кодом HTML, поэтому создавать их, как правило, нежелательно, но вам может потребоваться создать самозакрывающуюся вспомогательную функцию тега. Вспомогательные функции тегов задают тип свойства `TagMode` после считывания тега.
    
### <a name="processasync"></a>ProcessAsync

В этом разделе мы напишем асинхронную вспомогательную функцию тега электронной почты.

1. Замените класс `EmailTagHelper` на следующий код:

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]

   **Примечания.**

   * В этой версии используется асинхронный метод `ProcessAsync`. Асинхронный метод `GetChildContentAsync` возвращает объект `Task`, содержащий `TagHelperContent`.

   * Для получения содержимого элемента HTML используйте параметр `output`.

2. Чтобы вспомогательная функция тега могла получить целевой адрес электронной почты, внесите следующее изменение в файл *Views/Home/Contact.cshtml*:

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

3. Запустите приложение и убедитесь в том, что ссылки электронной почты создаются правильно.

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a>RemoveAll, PreContent.SetHtmlContent и PostContent.SetHtmlContent

1. Добавьте приведенный ниже класс `BoldTagHelper` в папку *TagHelpers*.

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]

   **Примечания.**
    
   * Атрибут `[HtmlTargetElement]` передает параметр атрибута, который указывает, что любой элемент HTML, содержащий атрибут HTML с именем bold, будет соответствовать условиям и метод переопределения `Process` в классе будет выполняться. В этом примере метод `Process` удаляет атрибут bold и заключает содержащую разметку в теги `<strong></strong>`.
    
   * Так как заменять существующее содержимое тега не нужно, открывающий тег `<strong>` должен записываться с помощью метода `PreContent.SetHtmlContent`, а закрывающий тег `</strong>` — с помощью метода `PostContent.SetHtmlContent`.
    
2. Измените представление *About.cshtml* так, чтобы оно содержало значение атрибута `bold`. Ниже приведен готовый код.

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]

3. Запустите приложение. Для проверки исходного кода и разметки можно использовать любой браузер на ваш выбор.

   Представленный выше атрибут `[HtmlTargetElement]` предназначен только для разметки HTML, которая предоставляет имя атрибута bold. Элемент `<bold>` не был изменен вспомогательной функцией тега.

4. Если закомментировать строку с атрибутом `[HtmlTargetElement]`, функция будет нацелена по умолчанию на теги `<bold>`, то есть на разметку HTML в форме `<bold>`. Помните, что в соответствии с соглашением об именовании по умолчанию имя класса **Bold**TagHelper соответствует тегам `<bold>`.

5. Запустите приложение и убедитесь в том, что тег `<bold>` обрабатывается вспомогательной функцией тега.

Если декорировать класс несколькими атрибутами `[HtmlTargetElement]`, к целевым тегам применяется логическая операция ИЛИ. Например, в приведенном ниже коде условиям будет соответствовать тег bold или атрибут bold.

[!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]

Если в один оператор добавляются несколько атрибутов, среда выполнения применяет к ним логическую операцию И. Например, в приведенном ниже коде элемент HTML будет соответствовать условиям, если он имеет имя bold и имеет атрибут с именем bold (`<bold bold />`).

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

С помощью `[HtmlTargetElement]` можно также изменить имя целевого элемента. Например, чтобы вспомогательная функция `BoldTagHelper` предназначалась для тегов `<MyBold>`, используйте следующий атрибут:

```csharp
[HtmlTargetElement("MyBold")]
   ```

## <a name="pass-a-model-to-a-tag-helper"></a>Передача модели во вспомогательную функцию тега

1. Добавьте папку *Models*.

2. Добавьте следующий класс `WebsiteContext` в папку *Models*:

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]

3. Добавьте приведенный ниже класс `WebsiteInformationTagHelper` в папку *TagHelpers*.

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]
    
   **Примечания.**
    
   * Как было сказано ранее, имена классов и свойств C# в стиле Pascal для вспомогательных функций тегов преобразуются в [кебаб-стиль нижнего регистра](http://wiki.c2.com/?KebabCase). Поэтому для использования функции `WebsiteInformationTagHelper` в Razor необходимо написать `<website-information />`.
    
   * Целевой элемент не определяется напрямую с помощью атрибута `[HtmlTargetElement]`, поэтому по умолчанию целевым будет элемент `website-information`. Например, вы применяете следующий атрибут (обратите внимание, что он не в стиле кебаб, но соответствует имени класса):
    
   ```csharp
   [HtmlTargetElement("WebsiteInformation")]
   ```
    
   Ему не будет соответствовать тег в стиле кебаб нижнего регистра `<website-information />`. Чтобы использовать атрибут `[HtmlTargetElement]`, следует применить стиль кебаб, как показано ниже.
    
   ```csharp
   [HtmlTargetElement("Website-Information")]
   ```
    
   * Самозакрывающиеся элементы не имеют содержимого. В этом примере в разметке Razor используется самозакрывающийся тег, но вспомогательная функция тега будет создавать элемент [section](http://www.w3.org/TR/html5/sections.html#the-section-element) (который не является самозакрывающимся, и вы добавляете содержимое внутри элемента `section`). Поэтому для записи выходных данных следует присвоить свойству `TagMode` значение `StartTagAndEndTag`. Кроме того, можно закомментировать строку, в которой задается `TagMode`, и написать разметку с закрывающим тегом. (Пример разметки приводится далее в этом руководстве.)
    
   * Символ `$` (знак доллара) в приведенной ниже строке определяет использование [интерполированной строки](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings).
    
   ```cshtml
   $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
   ```

4. Добавьте приведенную ниже разметку в представление *About.cshtml*. Выделенная разметка служит для вывода сведений о веб-сайте.
    
   [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,12-999)]
    
   > [!NOTE]
   > В Razor используйте следующую разметку:
   > 
   > [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=13-17)]
   > 
   > Razor известно, что атрибут `info` — это класс, а не строка, и что вы хотите написать код C#. Любой нестроковый атрибут вспомогательной функции тега должен быть записан без символа `@`.
    
5. Запустите приложение и перейдите к представлению About, чтобы просмотреть сведения о веб-сайте.

   > [!NOTE]
   > Вы можете использовать следующую разметку с закрывающим тегом и удалить строку со свойством `TagMode.StartTagAndEndTag` во вспомогательной функции тега:
   > 
   > [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]

## <a name="condition-tag-helper"></a>Вспомогательная функция тега условия

Вспомогательная функция тега условия преобразовывает выходные данные для просмотра, если в нее передано значение true.

1. Добавьте приведенный ниже класс `ConditionTagHelper` в папку *TagHelpers*.

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]

2. Замените содержимое файла *Views/Home/Index.cshtml* следующей разметкой:

   ```cshtml
   @using AuthoringTagHelpers.Models
   @model WebsiteContext
    
   @{
       ViewData["Title"] = "Home Page";
   }
    
   <div>
       <h3>Information about our website (outdated):</h3>
       <website-information info=@Model />
       <div condition="@Model.Approved">
           <p>
               This website has <strong surround="em"> @Model.Approved </strong> been approved yet.
               Visit www.contoso.com for more information.
           </p>
       </div>
   </div>
   ```
    
3. Замените метод `Index` в контроллере `Home` следующим кодом:

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]

4. Запустите приложение и перейдите на домашнюю страницу. Разметка в условном теге `div` не будет отображаться. Добавьте строку запроса `?approved=true` к URL-адресу (например, `http://localhost:1235/Home/Index?approved=true`). `approved` принимает значение true, и условная разметка будет отображаться.

> [!NOTE]
> Для определения целевого атрибута используйте оператор [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof), а не указывайте строку, как в случае со вспомогательной функцией тега bold:
> 
> [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]
> 
> Оператор [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) защищает код в случае его рефакторинга (имя может потребоваться изменить на `RedCondition`).

### <a name="avoid-tag-helper-conflicts"></a>Как избежать конфликтов между вспомогательными функциями тегов

В этом разделе вы напишете пару вспомогательных функций тегов для автоматического связывания. Первая функция будет заменять разметку, которая содержит URL-адрес, начинающийся с префикса HTTP, на HTML-тег привязки, содержащий тот же URL-адрес (то есть предоставляющий ссылку на URL-адрес). Вторая функция будет делать то же самое для URL-адреса, начинающегося с WWW.

Так как эти две вспомогательные функции тесно связаны друг с другом и в будущем может потребоваться их рефакторинг, мы поместим их в один файл.

1. Добавьте приведенный ниже класс `AutoLinkerHttpTagHelper` в папку *TagHelpers*.

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]

   >[!NOTE]
   >Для класса `AutoLinkerHttpTagHelper` целевыми являются элементы `p`, и в нем используется [регулярное выражение](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) для создания привязки.

2. Добавьте следующую разметку в конец файла *Views/Home/Contact.cshtml*:

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]

3. Запустите приложение и проверьте, правильно ли вспомогательная функция тега отображает привязку.

4. Обновите класс `AutoLinker`, включив в него функцию `AutoLinkerWwwTagHelper`, которая будет преобразовывать текст с префиксом WWW в тег привязки, который также содержит исходный текст с префиксом WWW. Обновленный код выделен ниже.

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]

5. Запустите приложение. Обратите внимание на то, что текст с префиксом WWW отображается как ссылка, а текст с префиксом HTTP — нет. Если установить точку останова в обоих классах, то можно увидеть, что класс вспомогательной функции тега HTTP выполняется первым. Проблема в том, что выходные данные вспомогательной функции тега кэшируются и, когда выполняется вспомогательная функция тега WWW, она перезаписывает кэшированные выходные данные вспомогательной функции тега HTTP. Далее в этом руководстве мы узнаем, как управлять очередностью выполнения вспомогательных функций тегов. Исправим код следующим образом:

   [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]

   > [!NOTE]
   > В первом выпуске вспомогательных функций тегов с автоматическим связыванием для получения содержимого целевых элементов использовался следующий код:
   > 
   > [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]
   > 
   > То есть вызывался метод `GetChildContentAsync` с передачей объекта `TagHelperOutput` в метод `ProcessAsync`. Как было сказано ранее, из-за кэширования выходных данных приоритет имеет последняя выполняемая вспомогательная функция тега. Эта проблема исправлялась с помощью следующего кода:
   > 
   > [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]
   > 
   > Приведенный выше код проверяет, было ли содержимое изменено. Если да, он получает содержимое из выходного буфера.

6. Запустите приложение и проверьте, правильно ли работают обе ссылки. Может показаться, что теперь вспомогательная функция тега с автоматическим связыванием полностью готова и работает правильно, однако есть одна деталь. Если вспомогательная функция тега WWW выполняется первой, ссылки с префиксом WWW будут неправильными. Измените код, добавив перегрузку `Order` для управления очередностью выполнения вспомогательных функций. Свойство `Order` определяет очередность выполнения относительно других вспомогательных функций тегов с тем же целевым элементом. Значение очередности по умолчанию — ноль. Экземпляры с более низкими значениями выполняются в первую очередь.

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]
    
   Приведенный выше код гарантирует, что вспомогательная функция тега HTTP будет выполняться перед вспомогательной функцией тега WWW. Измените значение `Order` на `MaxValue` и убедитесь в том, что для тега WWW разметка создается неправильно.

## <a name="inspect-and-retrieve-child-content"></a>Проверка и извлечение содержимого дочернего элемента

Вспомогательные функции тегов предоставляют несколько свойств для извлечения содержимого.

-  Результат выполнения метода `GetChildContentAsync` можно добавить к `output.Content`.
-  Результат выполнения метода `GetChildContentAsync` можно проверить с помощью `GetContent`.
-  При изменении `output.Content` тело функции TagHelper будет выполняться или обрабатываться, только если вызвать метод `GetChildContentAsync`, как в нашем примере автоматического связывания.

[!code-csharp[](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]

-  При нескольких вызовах `GetChildContentAsync` возвращается одно и то же значение и тело функции `TagHelper` не выполняется повторно, если только не передать параметр false, который указывает на то, что не следует использовать кэшированный результат.
