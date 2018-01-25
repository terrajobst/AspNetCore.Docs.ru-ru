---
title: "Создание вспомогательных функций тегов в ASP.NET Core"
author: rick-anderson
description: "Сведения о разработке вспомогательных функций тегов в ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 01/19/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/tag-helpers/authoring
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a1f1b2c2e60a1337c15f019185c764d0a9ada1b5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
# <a name="author-tag-helpers-in-aspnet-core-a-walkthrough-with-samples"></a>Автор вспомогательных функций тегов в ASP.NET Core, пошаговое руководство с примерами

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="get-started-with-tag-helpers"></a>Приступая к работе с вспомогательных функций тегов

Этот учебник содержит вводные сведения о программирования вспомогательных функций тегов. [Общие сведения о вспомогательных функций тегов](intro.md) описываются преимущества, которые предоставляют вспомогательных функций тегов.

Вспомогательный объект тег является любой класс, реализующий `ITagHelper` интерфейса. Тем не менее, при создании вспомогательных тег вы обычно являются производными от `TagHelper`, выполнив Да предоставляет вам доступ к `Process` метод.

1. Создайте новый проект ASP.NET Core с именем **AuthoringTagHelpers**. Нет необходимости проверки подлинности для этого проекта.

2. Создайте папку для хранения вспомогательных функций тегов, которая называется *TagHelpers*. *TagHelpers* папка *не* обязательно, но это разумного соглашение. Теперь давайте приступить к написанию Некоторые помощники тега в краткой форме.

## <a name="a-minimal-tag-helper"></a>Вспомогательный объект минимальной тега

В этом разделе запись помощник тег, который обновляет тег электронной почты. Пример:

```html
<email>Support</email>
   ```

Сервер будет использовать наши вспомогательные тег электронной почты для преобразования, разметку в следующее:

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

То есть тег, делает это ссылку в электронной почте. Может потребоваться в случае, если вы пишете механизмом поддержки блогов, для отправки электронной почты для отдела маркетинга, поддержки и другие контакты все в одном домене.

1.  Добавьте следующие `EmailTagHelper` класса *TagHelpers* папки.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]
    
    **Примечания.**
    
    * Тег вспомогательные методы используют соглашение об именовании, предназначенного элементы корневого имени класса (минус *вспомогательной функции тегов* часть имени класса). В этом примере имя корневой папки **электронной почты**является вспомогательной функции тегов *электронной почты*, поэтому `<email>` целевые тег. Это соглашение об именовании подойдут для большинства вспомогательных функций тегов, впоследствии будет показано как переопределить его.
    
    * Класс `EmailTagHelper` является производным от класса `TagHelper`. `TagHelper` Класс предоставляет методы и свойства для записи вспомогательных функций тегов.
    
    * Переопределенный `Process` метод управляет назначение вспомогательный тег при выполнении. `TagHelper` Класс также предоставляет асинхронную версию (`ProcessAsync`) с теми же параметрами.
    
    * Параметр контекста для `Process` (и `ProcessAsync`) содержит сведения, связанные с выполнением текущего HTML-тега.
    
    * Выходной параметр, чтобы `Process` (и `ProcessAsync`) содержит элемент с отслеживанием состояния HTML отражает исходного источника, используемый для создания HTML-тег и содержимое.
    
    * Наш имя класса содержит суффикс **вспомогательной функции тегов**, который является *не* требуется, но он считается наилучшим соглашением рекомендаций. Можно объявить класс как:
    
    ```csharp
    public class Email : TagHelper
    ```

2.  Чтобы сделать `EmailTagHelper` класса для наших представлений Razor, добавьте `addTagHelper` директиву *Views/_ViewImports.cshtml* файла:[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]
    
    Приведенный выше код использует синтаксисе знаков подстановки для указания всех вспомогательных функций тегов в нашем сборки будет доступно. Первая строка после `@addTagHelper` указывает тег вспомогательное приложение для загрузки (используйте «*» для всех вспомогательных функций тегов), и вторая строка «AuthoringTagHelpers» указывает тег модуль сборки. Кроме того, обратите внимание, что вторая строка переносит вспомогательных функций тегов Core ASP.NET MVC с помощью синтаксиса подстановочный знак (Эти вспомогательные методы обсуждаются в [Общие сведения о вспомогательных функций тегов](intro.md).) Это `@addTagHelper` директиву, которая делает доступными для представления Razor вспомогательный тег. Кроме того можно указать полное доменное имя (FQN) тега вспомогательного метода, как показано ниже:
    
```csharp
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```
    
<!--
the following snippet uses TagHelpers3 and should use TagHelpers (not the 3)
    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]
-->
    
Чтобы добавить тег вспомогательный класс для представления с помощью FQN, сначала добавьте FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), а затем имя сборки (*AuthoringTagHelpers*). Большинство разработчиков будет предпочитают использовать синтаксисе знаков подстановки. [Общие сведения о вспомогательных функций тегов](intro.md) попадают в подробности синтаксиса тегов вспомогательный добавления, удаления, иерархии и подстановочный знак.
    
3.  Обновить разметки в *Views/Home/Contact.cshtml* файла с учетом внесенных изменений:

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

4.  Запустите приложение и использовать любимом браузере для просмотра исходного кода HTML, чтобы можно было удостовериться, что теги электронной почты заменяются разметки привязки (например, `<a>Support</a>`). *Поддержка* и *маркетинга* отображаются как ссылки, но они не имеют `href` атрибут, чтобы сделать их работы. Мы исправим это в следующем разделе.

Примечание: Как HTML-теги и атрибуты, теги, имена классов и атрибутов в Razor и C# не учитывают регистр.

## <a name="setattribute-and-setcontent"></a>SetAttribute и SetContent

В этом разделе мы обновим `EmailTagHelper` , чтобы он создает допустимый тег для электронной почты. Мы обновим принимают данные из представления Razor (в виде `mail-to` атрибут) и использовать его при создании привязки.

Обновление `EmailTagHelper` класса следующими строками:

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]

**Примечания.**

* Преобразуется в стиле Pascal имена классов и свойств для вспомогательных функций тегов их [нижний регистр kebab](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101). Таким образом Чтобы использовать `MailTo` атрибут, вы воспользуетесь `<email mail-to="value"/>` эквивалент.

* Последняя строка задает завершенного содержимое для наших минимально функциональной тег вспомогательного метода.

* Выделенная строка показан синтаксис для добавления атрибутов:

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]

Этот способ подходит для атрибута «href», пока он не существует в настоящее время в коллекции атрибутов. Можно также использовать `output.Attributes.Add` метод, чтобы добавить атрибут вспомогательной функции тегов в конец коллекции атрибутов тега.

1.  Обновить разметки в *Views/Home/Contact.cshtml* файла с учетом внесенных изменений:[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]

2.  Запустите приложение и убедитесь, что он создает правильные ссылки.
    
    > [!NOTE]
    >В случае записи тег электронной почты самостоятельно закрытие (`<email mail-to="Rick" />`), также будут самозакрывающегося конечного результата. Чтобы включить возможность записать тег с помощью открывающего тега (`<email mail-to="Rick">`) необходимо дополнить класса на следующий:
    >
    > [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]
    
    С самозакрывающегося тега вспомогательный электронной почты, результатом будет `<a href="mailto:Rick@contoso.com" />`. Требуется закрывать теги привязки не допустимый HTML-код, поэтому не нужно создавать его, но может потребоваться создать помощник тег, который не требуется закрывать. Вспомогательных функций тегов задать тип `TagMode` свойство после прочтения тег.
    
### <a name="processasync"></a>ProcessAsync

В этом разделе мы записываем вспомогательный асинхронной электронной почты.

1.  Замените `EmailTagHelper` класса следующим кодом:

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]

    **Примечания.**

    * Эта версия использует асинхронную `ProcessAsync` метод. Асинхронная `GetChildContentAsync` возвращает `Task` содержащий `TagHelperContent`.

    * Используйте `output` для получения содержимого элемента HTML.

2.  Внесите следующее изменение для *Views/Home/Contact.cshtml* файл, поэтому вспомогательные тег можно получить по электронной почте целевой.

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

3.  Запустите приложение и убедитесь, он создает ссылки на допустимый адрес электронной почты.

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a>RemoveAll, PreContent.SetHtmlContent и PostContent.SetHtmlContent

1.  Добавьте следующие `BoldTagHelper` класса *TagHelpers* папки.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]

    **Примечания.**
    
    * `[HtmlTargetElement]` Атрибут передает параметр атрибута, указывающее, что любой HTML-элемент, который содержит HTML-атрибут с именем «bold» будет соответствовать, а `Process` переопределяющий метод в классе будет выполняться. В нашем примере `Process` метод удаляет атрибут «полужирный» и окружает содержащего разметку с `<strong></strong>`.
    
    * Поскольку вы не хотите заменить существующие теги содержимого, необходимо написать открывающий `<strong>` тег с `PreContent.SetHtmlContent` метод и закрытие `</strong>` тег с `PostContent.SetHtmlContent` метод.
    
2.  Изменить *About.cshtml* представление для размещения `bold` значение атрибута. Ниже приведен полный код.

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]

3.  Запустите приложение. Любимом браузере можно использовать для проверки источника и проверьте разметку.

    `[HtmlTargetElement]` Атрибут выше предназначен только для разметки HTML, который предоставляет имя атрибута «полужирный». `<bold>` Элемент не был изменен тег вспомогательный метод.

4. Закомментируйте `[HtmlTargetElement]` строку атрибута и он будет по умолчанию для различных версий `<bold>` теги, то есть HTML-разметку формы `<bold>`. Помните, что именования по умолчанию будет совпадать с именем класса **Полужирный**вспомогательной функции тегов для `<bold>` тегов.

5. Запустите приложение и убедитесь, что `<bold>` тег обрабатывается тег вспомогательный метод.

Декорирования классов с несколькими `[HtmlTargetElement]` атрибуты результаты логической операцией или целевых объектов. Например используя приведенный ниже код, полужирным тега или атрибута bold будет соответствовать.

[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]

При добавлении нескольких атрибутов той же инструкции, среда выполнения рассматривает их как логическое и. Например, в следующем коде HTML-элемент должен иметь имя «bold» с атрибутом с именем «bold» (`<bold bold />`) для сопоставления.

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

Можно также использовать `[HtmlTargetElement]` для изменения имени целевого элемента. Например, если вы хотите, чтобы `BoldTagHelper` целевой `<MyBold>` тегов, используется следующий атрибут:

```csharp
[HtmlTargetElement("MyBold")]
   ```

## <a name="pass-a-model-to-a-tag-helper"></a>Передать модель помощникам тега

1.  Добавить *моделей* папки.

2.  Добавьте следующий класс `WebsiteContext` в папку *Models*:

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]

3.  Добавьте следующие `WebsiteInformationTagHelper` класса *TagHelpers* папки.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]
    
    **Примечания.**
    
    * Как упоминалось ранее, вспомогательных функций тегов преобразует имена классов в стиле Pascal C# и свойства для вспомогательных функций тегов в [нижний регистр kebab](http://wiki.c2.com/?KebabCase). Таким образом Чтобы использовать `WebsiteInformationTagHelper` в Razor, вы напишете `<website-information />`.
    
    * Вы явным образом не идентифицируете целевой элемент с `[HtmlTargetElement]` атрибута, поэтому по умолчанию `website-information` будут применяться. Если вы применили (Примечание не kebab так, но совпадает с именем класса) следующий атрибут:
    
    ```csharp
    [HtmlTargetElement("WebsiteInformation")]
    ```
    
    Ниже тег варианта kebab `<website-information />` не совпадают. Если вы хотите `[HtmlTargetElement]` атрибут, используйте kebab случая, как показано ниже:
    
    ```csharp
    [HtmlTargetElement("Website-Information")]
    ```
    
    * Элементы, которые являются самозакрывающегося не имеет содержимого. В этом примере разметки Razor будет использоваться самостоятельно закрывающегося тега, но будут создавать вспомогательные тег [раздел](http://www.w3.org/TR/html5/sections.html#the-section-element) элемента (не требуется закрывать и записи содержимого внутри `section` элемент). Таким образом, необходимо задать `TagMode` для `StartTagAndEndTag` для записи выходных данных. Кроме того, вы можете закомментировать строку, устанавливающую `TagMode` и записи разметки закрывающим тегом. (Пример разметки указан далее в этом руководстве.)
    
    * `$` (Знак доллара) в следующей строке использует [интерполируются строка](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):
    
    ```cshtml
    $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
    ```

4.  Добавьте следующую разметку, чтобы *About.cshtml* представления. Выделенную разметку сведения о веб-сайта.
    
    [!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,12-)]
    
    >[!NOTE]
    > В разметке Razor, показано ниже:
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=13-17)]
    > 
    >Знает Razor `info` атрибут — это класс, не является строкой, и необходимо написать код C#. Любой атрибут вспомогательной функции нестроковые тег должен быть записан без `@` символов.
    
5.  Запустите приложение и перейдите в представление о программе, чтобы просмотреть сведения веб-сайта.

    >[!NOTE]
    >Можно использовать следующую разметку с закрывающий тег и удалите строку с `TagMode.StartTagAndEndTag` во вспомогательном методе тег:
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]

## <a name="condition-tag-helper"></a>Вспомогательный тег условие

Вспомогательный тег условие отображает выходные данные при передаче значения true.

1.  Добавьте следующие `ConditionTagHelper` класса *TagHelpers* папки.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]

2.  Замените содержимое *Views/Home/Index.cshtml* файла следующей разметкой:

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
    
3.  Замените `Index` метод в `Home` контроллера с помощью следующего кода:

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]

4.  Запустите приложение и перейдите на домашнюю страницу. Разметка в условной `div` не будут отображаться. Добавить строку запроса `?approved=true` URL-адрес (например, `http://localhost:1235/Home/Index?approved=true`). `approved`имеет значение true, а также условия отображения разметки.

>[!NOTE]
>Используйте [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) оператор, чтобы указать атрибут целевой вместо указания строки, как в случае со вспомогательным методом тег полужирным шрифтом:
>
>[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]
>
>[Nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) оператор будет защищать код должен его постоянно оптимизируемого (мы может возникнуть необходимость изменить имя, `RedCondition`).

### <a name="avoid-tag-helper-conflicts"></a>Избегание конфликтов в тег вспомогательный

В этом разделе запись пару автоматическое связывание вспомогательных функций тегов. Первый заменяет разметку, содержащую URL-адрес, начинающийся с HTTP HTML привязки тег содержащую же URL-адрес (и тем самым давая ссылку на URL-адрес). Второй будет то же сделайте для URL-адреса начиная с веб-публикации.

Поскольку тесно связаны эти две вспомогательные функции и их может рефакторинг в будущем, мы будем держать их в том же файле.

1.  Добавьте следующие `AutoLinkerHttpTagHelper` класса *TagHelpers* папки.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]

    >[!NOTE]
    >`AutoLinkerHttpTagHelper` Класса цели `p` элементы и использует [Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) для создания привязки.

2.  Добавьте следующую разметку в конец *Views/Home/Contact.cshtml* файла:

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]

3.  Запустите приложение и проверьте, правильно отображает вспомогательный тег привязки.

4.  Обновление `AutoLinker` класса для включения `AutoLinkerWwwTagHelper` которого преобразует www текст в тег, который содержит исходный текст веб-публикации. Обновленный код выделяется ниже:

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]

5.  Запустите приложение. Обратите внимание, www текст отображается в виде ссылки, но не в тексте HTTP. Если установить точку останова в обоих классах, вы увидите, вспомогательный класс тег HTTP выполняется первой. Проблема заключается в кэширования вывода вспомогательный тегов, что при запуске вспомогательного тег WWW она перезаписывает кэшированные выходные данные модуля поддержки тег HTTP. Далее в этом учебнике будет показано, как можно управлять порядком выполнения вспомогательных функций тегов в. Нам нужно исправить код с помощью следующего:

    [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]

    >[!NOTE]
    >В первом выпуске автоматическое связывание вспомогательных функций тегов полученный содержимое целевого объекта с помощью следующего кода:
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]
    >
    >То есть, можно вызвать `GetChildContentAsync` с помощью `TagHelperOutput` переданные `ProcessAsync` метод. Как упоминалось ранее, так как выходные данные кэшируются, последний тег вспомогательный метод для запуска wins. Вы фиксированной этой проблеме с помощью следующего кода:
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]
    >
    >Приведенный выше код проверяет, если содержимое было изменено, и если да, он получает содержимое из выходного буфера.

6.  Запустите приложение и убедиться, что две ссылки работают должным образом. Хотя может показаться, что наши вспомогательные тег компоновщика автоматически будет полным и правильным, у него есть незначительные проблемы. Если вспомогательные тег WWW выполняться в первую очередь, веб-ссылки не будет правильно. Обновление кода, добавив `Order` перегрузку, чтобы управлять порядком, запускаемую в теге. `Order` Свойство определяет порядок выполнения относительно других вспомогательных функций тегов, предназначенные для одного элемента. Значение порядка по умолчанию равно нулю, и экземпляры с более низкими значениями выполняются первыми.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]
    
    Приведенный выше код гарантирует, вспомогательные тег HTTP выполняется перед вспомогательный тег WWW. Изменение `Order` для `MaxValue` и убедитесь, что разметки, созданной для тега WWW неверен.

## <a name="inspect-and-retrieve-child-content"></a>Проверки и извлечения содержимого дочернего элемента

Вспомогательных функций тегов предоставляют несколько свойств, чтобы извлечь содержимое.

-  Результат `GetChildContentAsync` можно добавить `output.Content`.
-  Вы можете проверить результат `GetChildContentAsync` с `GetContent`.
-  При изменении `output.Content`, тексте вспомогательной функции тегов не будет выполнен или не визуализуется, если вызывается `GetChildContentAsync` как в нашем примере компоновщик автоматическое:

[!code-csharp[Main](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]

-  Несколько вызовов `GetChildContentAsync` возвращает то же значение и повторно не выполняет `TagHelper` body передается в параметре значение false, указывающее, не для использования кэшированных результатов.
