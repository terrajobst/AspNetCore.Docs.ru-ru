---
title: "Настройка объекта переносимой локализации"
author: sebastienros
description: "В этой статье приведены сведения о файлах переносимый объект и описаны действия по их использованию в приложении ASP.NET Core с помощью платформы Orchard Core."
keywords: "ASP.NET Core, локализации, языка и региональных параметров, языка, переносимых объектов"
ms.author: scaddie
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/portable-object-localization
ms.openlocfilehash: 4fa73ae08b10217de657681a27f6097fc3443737
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/01/2017
---
# <a name="configure-portable-object-localization-with-orchard-core"></a><span data-ttu-id="dfd79-104">Настройка объекта переносимой локализации с основными Orchard</span><span class="sxs-lookup"><span data-stu-id="dfd79-104">Configure portable object localization with Orchard Core</span></span>

<span data-ttu-id="dfd79-105">По [Sébastien Ros](https://github.com/sebastienros) и [Скотт Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="dfd79-105">By [Sébastien Ros](https://github.com/sebastienros) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="dfd79-106">В этой статье рассматриваются действия по использованию файлов переносимой объекта (PO) в приложении ASP.NET Core с [Orchard Core](https://github.com/OrchardCMS/OrchardCore) framework.</span><span class="sxs-lookup"><span data-stu-id="dfd79-106">This article walks through the steps for using Portable Object (PO) files in an ASP.NET Core application with the [Orchard Core](https://github.com/OrchardCMS/OrchardCore) framework.</span></span>

<span data-ttu-id="dfd79-107">**Примечание:** Orchard Core не является продуктом корпорации Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="dfd79-107">**Note:** Orchard Core is not a Microsoft product.</span></span> <span data-ttu-id="dfd79-108">Следовательно Корпорация Майкрософт не поддерживает эту функцию.</span><span class="sxs-lookup"><span data-stu-id="dfd79-108">Consequently, Microsoft provides no support for this feature.</span></span>

<span data-ttu-id="dfd79-109">[Просмотреть или загрузить образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization) ([загрузке](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="dfd79-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-a-po-file"></a><span data-ttu-id="dfd79-110">Что такое файл PO?</span><span class="sxs-lookup"><span data-stu-id="dfd79-110">What is a PO file?</span></span>

<span data-ttu-id="dfd79-111">Файлы PO распространяются как текстовые файлы, содержащие переведенных строк для данного языка.</span><span class="sxs-lookup"><span data-stu-id="dfd79-111">PO files are distributed as text files containing the translated strings for a given language.</span></span> <span data-ttu-id="dfd79-112">Некоторые преимущества использования файлов PO *.resx* файлы включают в себя:</span><span class="sxs-lookup"><span data-stu-id="dfd79-112">Some advantages of using PO files instead *.resx* files include:</span></span>
- <span data-ttu-id="dfd79-113">Файлы PO поддерживают преобразование во множественную форму; *.resx* файлы не поддерживают преобразование во множественную форму.</span><span class="sxs-lookup"><span data-stu-id="dfd79-113">PO files support pluralization; *.resx* files don't support pluralization.</span></span>
- <span data-ttu-id="dfd79-114">Файлы PO не компилируются подобно *.resx* файлов.</span><span class="sxs-lookup"><span data-stu-id="dfd79-114">PO files aren't compiled like *.resx* files.</span></span> <span data-ttu-id="dfd79-115">Таким образом специальные действия сборки и оснащения, не являются обязательными.</span><span class="sxs-lookup"><span data-stu-id="dfd79-115">As such, specialized tooling and build steps aren't required.</span></span>
- <span data-ttu-id="dfd79-116">Файлы PO работает со средствами совместной работы сети редактирования.</span><span class="sxs-lookup"><span data-stu-id="dfd79-116">PO files work well with collaborative online editing tools.</span></span>

### <a name="example"></a><span data-ttu-id="dfd79-117">Пример</span><span class="sxs-lookup"><span data-stu-id="dfd79-117">Example</span></span>

<span data-ttu-id="dfd79-118">Ниже приведен образец файла PO содержащая перевод для двух строк на французском языке, включая его формы во множественном числе:</span><span class="sxs-lookup"><span data-stu-id="dfd79-118">Here is a sample PO file containing the translation for two strings in French, including one with its plural form:</span></span>

<span data-ttu-id="dfd79-119">*fr.PO*</span><span class="sxs-lookup"><span data-stu-id="dfd79-119">*fr.po*</span></span>

```text
#: Services/EmailService.cs:29
msgid "Enter a comma separated list of email addresses."
msgstr "Entrez une liste d'emails séparés par une virgule."

#: Views/Email.cshtml:112
msgid "The email address is \"{0}\"."
msgid_plural "The email addresses are \"{0}\"."
msgstr[0] "L'adresse email est \"{0}\"."
msgstr[1] "Les adresses email sont \"{0}\""
```

<span data-ttu-id="dfd79-120">В этом примере используется следующий синтаксис:</span><span class="sxs-lookup"><span data-stu-id="dfd79-120">This example uses the following syntax:</span></span>

- <span data-ttu-id="dfd79-121">`#:`: Комментарий, указывающее контекст строки, которые будут преобразованы.</span><span class="sxs-lookup"><span data-stu-id="dfd79-121">`#:`: A comment indicating the context of the string to be translated.</span></span> <span data-ttu-id="dfd79-122">Та же строка может быть переведены различным образом в зависимости от того, где он используется.</span><span class="sxs-lookup"><span data-stu-id="dfd79-122">The same string might be translated differently depending on where it is being used.</span></span>
- <span data-ttu-id="dfd79-123">`msgid`: Непреобразованный строка.</span><span class="sxs-lookup"><span data-stu-id="dfd79-123">`msgid`: The untranslated string.</span></span>
- <span data-ttu-id="dfd79-124">`msgstr`: Переведенных строк.</span><span class="sxs-lookup"><span data-stu-id="dfd79-124">`msgstr`: The translated string.</span></span>

<span data-ttu-id="dfd79-125">В случае поддержки преобразование во множественную форму можно определить дополнительные записи.</span><span class="sxs-lookup"><span data-stu-id="dfd79-125">In the case of pluralization support, more entries can be defined.</span></span>

- <span data-ttu-id="dfd79-126">`msgid_plural`: Непреобразованный во множественном числе строка.</span><span class="sxs-lookup"><span data-stu-id="dfd79-126">`msgid_plural`: The untranslated plural string.</span></span>
- <span data-ttu-id="dfd79-127">`msgstr[0]`: Переведенных строк для варианта 0.</span><span class="sxs-lookup"><span data-stu-id="dfd79-127">`msgstr[0]`: The translated string for the case 0.</span></span>
- <span data-ttu-id="dfd79-128">`msgstr[N]`: Переведенных строк для вариантов N.</span><span class="sxs-lookup"><span data-stu-id="dfd79-128">`msgstr[N]`: The translated string for the case N.</span></span>

<span data-ttu-id="dfd79-129">Спецификация файла PO можно найти [здесь](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html).</span><span class="sxs-lookup"><span data-stu-id="dfd79-129">The PO file specification can be found [here](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html).</span></span>

## <a name="configuring-po-file-support-in-aspnet-core"></a><span data-ttu-id="dfd79-130">Настройка поддержки файлов заказа на Покупку в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dfd79-130">Configuring PO file support in ASP.NET Core</span></span>

<span data-ttu-id="dfd79-131">Этот пример основан на основных компонентов MVC-приложениях ASP.NET создан из шаблона проекта Visual Studio 2017 г.</span><span class="sxs-lookup"><span data-stu-id="dfd79-131">This example is based on an ASP.NET Core MVC application generated from a Visual Studio 2017 project template.</span></span>

### <a name="referencing-the-package"></a><span data-ttu-id="dfd79-132">Ссылка на пакет</span><span class="sxs-lookup"><span data-stu-id="dfd79-132">Referencing the package</span></span>

<span data-ttu-id="dfd79-133">Добавьте ссылку на `OrchardCore.Localization.Core` пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="dfd79-133">Add a reference to the `OrchardCore.Localization.Core` NuGet package.</span></span> <span data-ttu-id="dfd79-134">Его можно найти на [MyGet](https://www.myget.org/) источника следующих пакетов: https://www.myget.org/F/orchardcore-preview/api/v3/index.json</span><span class="sxs-lookup"><span data-stu-id="dfd79-134">It is available on [MyGet](https://www.myget.org/) at the following package source: https://www.myget.org/F/orchardcore-preview/api/v3/index.json</span></span>

<span data-ttu-id="dfd79-135">*.Csproj* файл теперь содержит строку следующего вида (номер версии могут различаться):</span><span class="sxs-lookup"><span data-stu-id="dfd79-135">The *.csproj* file now contains a line similar to the following (version number may vary):</span></span>

[!code-xml[Main](localization/sample/POLocalization/POLocalization.csproj?range=9)]

### <a name="registering-the-service"></a><span data-ttu-id="dfd79-136">Регистрация службы</span><span class="sxs-lookup"><span data-stu-id="dfd79-136">Registering the service</span></span>

<span data-ttu-id="dfd79-137">Добавьте необходимые службы, чтобы `ConfigureServices` метод *файла Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="dfd79-137">Add the required services to the `ConfigureServices` method of *Startup.cs*:</span></span>

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_ConfigureServices&highlight=4-21)]

<span data-ttu-id="dfd79-138">Добавьте необходимые по промежуточного слоя для `Configure` метод *файла Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="dfd79-138">Add the required middleware to the `Configure` method of *Startup.cs*:</span></span>

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_Configure&highlight=15)]

<span data-ttu-id="dfd79-139">Добавьте следующий код для представления Razor по выбору.</span><span class="sxs-lookup"><span data-stu-id="dfd79-139">Add the following code to your Razor view of choice.</span></span> <span data-ttu-id="dfd79-140">*About.cshtml* используется в этом примере.</span><span class="sxs-lookup"><span data-stu-id="dfd79-140">*About.cshtml* is used in this example.</span></span>

[!code-cshtml[Main](localization/sample/POLocalization/Views/Home/About.cshtml)]

<span data-ttu-id="dfd79-141">`IViewLocalizer` Экземпляра внедренного и используемый для преобразования текста «Hello world!».</span><span class="sxs-lookup"><span data-stu-id="dfd79-141">An `IViewLocalizer` instance is injected and used to translate the text "Hello world!".</span></span>

### <a name="creating-a-po-file"></a><span data-ttu-id="dfd79-142">Создание файла заказа на Покупку</span><span class="sxs-lookup"><span data-stu-id="dfd79-142">Creating a PO file</span></span>

<span data-ttu-id="dfd79-143">Создайте файл с именем  *<culture code>расширениями* в корневой папке приложения.</span><span class="sxs-lookup"><span data-stu-id="dfd79-143">Create a file named *<culture code>.po* in your application root folder.</span></span> <span data-ttu-id="dfd79-144">В этом примере имя файла — *fr.po* так, как используется французский язык:</span><span class="sxs-lookup"><span data-stu-id="dfd79-144">In this example, the file name is *fr.po* because the French language is used:</span></span>

[!code-text[Main](localization/sample/POLocalization/fr.po)]

<span data-ttu-id="dfd79-145">Файл содержит строку для преобразования и французский перевод строки.</span><span class="sxs-lookup"><span data-stu-id="dfd79-145">This file stores both the string to translate and the French-translated string.</span></span> <span data-ttu-id="dfd79-146">Переводы вернуться к их родительский язык и региональные параметры, при необходимости.</span><span class="sxs-lookup"><span data-stu-id="dfd79-146">Translations revert to their parent culture, if necessary.</span></span> <span data-ttu-id="dfd79-147">В этом примере *fr.po* файл используется в том случае, если запрошенный язык и региональные параметры `fr-FR` или `fr-CA`.</span><span class="sxs-lookup"><span data-stu-id="dfd79-147">In this example, the *fr.po* file is used if the requested culture is `fr-FR` or `fr-CA`.</span></span>

### <a name="testing-the-application"></a><span data-ttu-id="dfd79-148">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="dfd79-148">Testing the application</span></span>

<span data-ttu-id="dfd79-149">Запустите приложение и перейдите по URL-адресу `/Home/About`.</span><span class="sxs-lookup"><span data-stu-id="dfd79-149">Run your application, and navigate to the URL `/Home/About`.</span></span> <span data-ttu-id="dfd79-150">Текст **Здравствуй, мир!**</span><span class="sxs-lookup"><span data-stu-id="dfd79-150">The text **Hello world!**</span></span> <span data-ttu-id="dfd79-151">отображается.</span><span class="sxs-lookup"><span data-stu-id="dfd79-151">is displayed.</span></span>

<span data-ttu-id="dfd79-152">Перейдите на URL-адрес `/Home/About?culture=fr-FR`.</span><span class="sxs-lookup"><span data-stu-id="dfd79-152">Navigate to the URL `/Home/About?culture=fr-FR`.</span></span> <span data-ttu-id="dfd79-153">Текст **Bonjour le monde!**</span><span class="sxs-lookup"><span data-stu-id="dfd79-153">The text **Bonjour le monde!**</span></span> <span data-ttu-id="dfd79-154">отображается.</span><span class="sxs-lookup"><span data-stu-id="dfd79-154">is displayed.</span></span>

## <a name="pluralization"></a><span data-ttu-id="dfd79-155">Преобразование во множественную форму</span><span class="sxs-lookup"><span data-stu-id="dfd79-155">Pluralization</span></span>

<span data-ttu-id="dfd79-156">Файлы PO поддерживает преобразование во множественную форму формы, это полезно, когда та же строка требует перевода различаться в зависимости от количества элементов.</span><span class="sxs-lookup"><span data-stu-id="dfd79-156">PO files support pluralization forms, which is useful when the same string needs to be translated differently based on a cardinality.</span></span> <span data-ttu-id="dfd79-157">Эта задача выполняется сложной из-за того, что каждый язык определяет настраиваемые правила, чтобы выбрать строку, используемую в зависимости от количества элементов.</span><span class="sxs-lookup"><span data-stu-id="dfd79-157">This task is made complicated by the fact that each language defines custom rules to select which string to use based on the cardinality.</span></span>

<span data-ttu-id="dfd79-158">Пакет Orchard локализации предоставляет API для вызова этих различных форм множественного числа автоматически.</span><span class="sxs-lookup"><span data-stu-id="dfd79-158">The Orchard Localization package provides an API to invoke these different plural forms automatically.</span></span>

### <a name="creating-pluralization-po-files"></a><span data-ttu-id="dfd79-159">Создание файлов PO преобразование во множественную форму</span><span class="sxs-lookup"><span data-stu-id="dfd79-159">Creating pluralization PO files</span></span>

<span data-ttu-id="dfd79-160">Добавьте следующее содержимое в ранее упомянутых *fr.po* файла:</span><span class="sxs-lookup"><span data-stu-id="dfd79-160">Add the following content to the previously mentioned *fr.po* file:</span></span>

```text
msgid "There is one item."
msgid_plural "There are {0} items."
msgstr[0] "Il y a un élément."
msgstr[1] "Il y a {0} éléments."
```

<span data-ttu-id="dfd79-161">В разделе [что такое файл PO?](#what-is-a-po-file) каждая запись в этом примере представляет описание.</span><span class="sxs-lookup"><span data-stu-id="dfd79-161">See [What is a PO file?](#what-is-a-po-file) for an explanation of what each entry in this example represents.</span></span>

### <a name="adding-a-language-using-different-pluralization-forms"></a><span data-ttu-id="dfd79-162">Добавление языка, с помощью форм разных преобразование во множественную форму</span><span class="sxs-lookup"><span data-stu-id="dfd79-162">Adding a language using different pluralization forms</span></span>

<span data-ttu-id="dfd79-163">В предыдущем примере были использованы строки английского и французского языков.</span><span class="sxs-lookup"><span data-stu-id="dfd79-163">English and French strings were used in the previous example.</span></span> <span data-ttu-id="dfd79-164">Английского и французского имеет только две формы преобразование во множественную форму и совместно использовать те же правила формы это, что количество элементов один сопоставляется первой формы во множественном числе.</span><span class="sxs-lookup"><span data-stu-id="dfd79-164">English and French have only two pluralization forms and share the same form rules, which is that a cardinality of one is mapped to the first plural form.</span></span> <span data-ttu-id="dfd79-165">Другие количества элементов сопоставляются с вторая форма множественного числа.</span><span class="sxs-lookup"><span data-stu-id="dfd79-165">Any other cardinality is mapped to the second plural form.</span></span>

<span data-ttu-id="dfd79-166">Не все языки используют те же самые правила.</span><span class="sxs-lookup"><span data-stu-id="dfd79-166">Not all languages share the same rules.</span></span> <span data-ttu-id="dfd79-167">Это показано с чешском языке, который имеет три формы во множественном числе.</span><span class="sxs-lookup"><span data-stu-id="dfd79-167">This is illustrated with the Czech language, which has three plural forms.</span></span>

<span data-ttu-id="dfd79-168">Создание `cs.po` следующим образом и обратите внимание на то, каким образом преобразование во множественную форму должно три различным переводам:</span><span class="sxs-lookup"><span data-stu-id="dfd79-168">Create the `cs.po` file as follows, and note how the pluralization needs three different translations:</span></span>

[!code-text[Main](localization/sample/POLocalization/cs.po)]

<span data-ttu-id="dfd79-169">Чтобы принять чешский локализации, добавьте `"cs"` в список поддерживаемых языков и региональных параметров в `ConfigureServices` метод:</span><span class="sxs-lookup"><span data-stu-id="dfd79-169">To accept Czech localizations, add `"cs"` to the list of supported cultures in the `ConfigureServices` method:</span></span>

```csharp
var supportedCultures = new List<CultureInfo>
{
    new CultureInfo("en-US"),
    new CultureInfo("en"),
    new CultureInfo("fr-FR"),
    new CultureInfo("fr"),
    new CultureInfo("cs")
};
```

<span data-ttu-id="dfd79-170">Изменить *Views/Home/About.cshtml* файл для отображения локализованных строк для нескольких мощности во множественном числе:</span><span class="sxs-lookup"><span data-stu-id="dfd79-170">Edit the *Views/Home/About.cshtml* file to render localized, plural strings for several cardinalities:</span></span>

```cshtml
<p>@Localizer.Plural(1, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(2, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(5, "There is one item.", "There are {0} items.")</p>
```

<span data-ttu-id="dfd79-171">**Примечание:** в реальной ситуации переменной будет использоваться для представления числа.</span><span class="sxs-lookup"><span data-stu-id="dfd79-171">**Note:** In a real world scenario, a variable would be used to represent the count.</span></span> <span data-ttu-id="dfd79-172">Здесь мы повторяем один и тот же код с тремя различными значениями для предоставления очень определенный регистр.</span><span class="sxs-lookup"><span data-stu-id="dfd79-172">Here, we repeat the same code with three different values to expose a very specific case.</span></span>

<span data-ttu-id="dfd79-173">После переключения языков и региональных параметров, см.</span><span class="sxs-lookup"><span data-stu-id="dfd79-173">Upon switching cultures, you see the following:</span></span>

<span data-ttu-id="dfd79-174">Для `/Home/About`:</span><span class="sxs-lookup"><span data-stu-id="dfd79-174">For `/Home/About`:</span></span>

```html
There is one item.
There are 2 items.
There are 5 items.
```

<span data-ttu-id="dfd79-175">Для `/Home/About?culture=fr`:</span><span class="sxs-lookup"><span data-stu-id="dfd79-175">For `/Home/About?culture=fr`:</span></span>

```html
Il y a un élément.
Il y a 2 éléments.
Il y a 5 éléments.
```

<span data-ttu-id="dfd79-176">Для `/Home/About?culture=cs`:</span><span class="sxs-lookup"><span data-stu-id="dfd79-176">For `/Home/About?culture=cs`:</span></span>

```html
Existuje jedna položka.
Existují 2 položky.
Existuje 5 položek.
```

<span data-ttu-id="dfd79-177">Обратите внимание, что для чешского языка и региональных параметров, три переводы отличаются.</span><span class="sxs-lookup"><span data-stu-id="dfd79-177">Note that for the Czech culture, the three translations are different.</span></span> <span data-ttu-id="dfd79-178">Языки и региональные параметры французский "и" Английский совместное использование одной конструкции для двух последних переведенных строк.</span><span class="sxs-lookup"><span data-stu-id="dfd79-178">The French and English cultures share the same construction for the two last translated strings.</span></span>

## <a name="advanced-tasks"></a><span data-ttu-id="dfd79-179">Расширенные задачи</span><span class="sxs-lookup"><span data-stu-id="dfd79-179">Advanced tasks</span></span>

### <a name="contextualizing-strings"></a><span data-ttu-id="dfd79-180">Изучение в контексте строк</span><span class="sxs-lookup"><span data-stu-id="dfd79-180">Contextualizing strings</span></span>

<span data-ttu-id="dfd79-181">Приложения часто содержат строки, которые будут преобразованы в нескольких местах.</span><span class="sxs-lookup"><span data-stu-id="dfd79-181">Applications often contain the strings to be translated in several places.</span></span> <span data-ttu-id="dfd79-182">Та же строка может иметь различные преобразования в определенных местах внутри приложения (представлений Razor или файлы класса).</span><span class="sxs-lookup"><span data-stu-id="dfd79-182">The same string may have a different translation in certain locations within an app (Razor views or class files).</span></span> <span data-ttu-id="dfd79-183">Файл PO поддерживает понятие контекста файла, который может использоваться для классификации, представленному в строку.</span><span class="sxs-lookup"><span data-stu-id="dfd79-183">A PO file supports the notion of a file context, which can be used to categorize the string being represented.</span></span> <span data-ttu-id="dfd79-184">С помощью контекста файла, строки могут быть переведены по-разному, в зависимости от контекста файла (или отсутствия контекста файла).</span><span class="sxs-lookup"><span data-stu-id="dfd79-184">Using a file context, a string can be translated differently, depending on the file context (or lack of a file context).</span></span>

<span data-ttu-id="dfd79-185">Службы локализации PO используют имя полный класс или представление, которое используется при преобразовании строки.</span><span class="sxs-lookup"><span data-stu-id="dfd79-185">The PO localization services use the name of the full class or the view that is used when translating a string.</span></span> <span data-ttu-id="dfd79-186">Это достигается путем установки значения на `msgctxt` запись.</span><span class="sxs-lookup"><span data-stu-id="dfd79-186">This is accomplished by setting the value on the `msgctxt` entry.</span></span>

<span data-ttu-id="dfd79-187">Рассмотрим незначительным дополнением к предыдущему *fr.po* примере.</span><span class="sxs-lookup"><span data-stu-id="dfd79-187">Consider a minor addition to the previous *fr.po* example.</span></span> <span data-ttu-id="dfd79-188">Представления Razor, расположенный *Views/Home/About.cshtml* может определяться как контекст файла, задав зарезервировано `msgctxt` значение этого параметра:</span><span class="sxs-lookup"><span data-stu-id="dfd79-188">A Razor view located at *Views/Home/About.cshtml* can be defined as the file context by setting the reserved `msgctxt` entry's value:</span></span>

```text
msgctxt "Views.Home.About"
msgid "Hello world!"
msgstr "Bonjour le monde!"
```

<span data-ttu-id="dfd79-189">С `msgctxt` задано таким образом, перевод текста происходит при переходе к `/Home/About?culture=fr-FR`.</span><span class="sxs-lookup"><span data-stu-id="dfd79-189">With the `msgctxt` set as such, text translation occurs when navigating to `/Home/About?culture=fr-FR`.</span></span> <span data-ttu-id="dfd79-190">Перевод не произойдет при переходе к `/Home/Contact?culture=fr-FR`.</span><span class="sxs-lookup"><span data-stu-id="dfd79-190">The translation won't occur when navigating to `/Home/Contact?culture=fr-FR`.</span></span>

<span data-ttu-id="dfd79-191">При совпадении с контекстом данный файл не определенной записи резервный механизм Orchard Core ищет в соответствующий файл PO без контекста.</span><span class="sxs-lookup"><span data-stu-id="dfd79-191">When no specific entry is matched with a given file context, Orchard Core's fallback mechanism looks for an appropriate PO file without a context.</span></span> <span data-ttu-id="dfd79-192">При отсутствии является контекст не определенный файл, определенный для *Views/Home/Contact.cshtml*, перехода по страницам `/Home/Contact?culture=fr-FR` загружает файл заказа на Покупку, такие как:</span><span class="sxs-lookup"><span data-stu-id="dfd79-192">Assuming there is no specific file context defined for *Views/Home/Contact.cshtml*, navigating to `/Home/Contact?culture=fr-FR` loads a PO file such as:</span></span>

[!code-text[Main](localization/sample/POLocalization/fr.po)]

### <a name="changing-the-location-of-po-files"></a><span data-ttu-id="dfd79-193">Изменение расположения файлов заказа на Покупку</span><span class="sxs-lookup"><span data-stu-id="dfd79-193">Changing the location of PO files</span></span>

<span data-ttu-id="dfd79-194">Можно изменить расположение по умолчанию файлы заказа на Покупку в `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="dfd79-194">The default location of PO files can be changed in `ConfigureServices`:</span></span>

```csharp
services.AddPortableObjectLocalization(options => options.ResourcesPath = "Localization");
```

<span data-ttu-id="dfd79-195">В этом примере PO файлы загружаются из *локализации* папки.</span><span class="sxs-lookup"><span data-stu-id="dfd79-195">In this example, the PO files are loaded from the *Localization* folder.</span></span>

### <a name="implementing-a-custom-logic-for-finding-localization-files"></a><span data-ttu-id="dfd79-196">Реализация пользовательской логики для поиска файлов локализации</span><span class="sxs-lookup"><span data-stu-id="dfd79-196">Implementing a custom logic for finding localization files</span></span>

<span data-ttu-id="dfd79-197">Когда требуется более сложная логика для поиска файлов PO `OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider` интерфейс можно реализовать и зарегистрирован как служба.</span><span class="sxs-lookup"><span data-stu-id="dfd79-197">When more complex logic is needed to locate PO files, the `OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider` interface can be implemented and registered as a service.</span></span> <span data-ttu-id="dfd79-198">Это полезно в том случае, когда PO файлы могут храниться в различных местах или когда файлы должны размещаться в иерархии папок.</span><span class="sxs-lookup"><span data-stu-id="dfd79-198">This is useful when PO files can be stored in varying locations or when the files have to be found within a hierarchy of folders.</span></span>

### <a name="using-a-different-default-pluralized-language"></a><span data-ttu-id="dfd79-199">С помощью языка по умолчанию имена во множественном числе</span><span class="sxs-lookup"><span data-stu-id="dfd79-199">Using a different default pluralized language</span></span>

<span data-ttu-id="dfd79-200">В пакете `Plural` метод расширения, относящиеся к две формы во множественном числе.</span><span class="sxs-lookup"><span data-stu-id="dfd79-200">The package includes a `Plural` extension method that is specific to two plural forms.</span></span> <span data-ttu-id="dfd79-201">Для языков, требующие другие формы во множественном числе создание метода расширения.</span><span class="sxs-lookup"><span data-stu-id="dfd79-201">For languages requiring more plural forms, create an extension method.</span></span> <span data-ttu-id="dfd79-202">С помощью метода расширения не нужно будет вводить любой файл локализации для языка по умолчанию &mdash; исходных строк уже доступны непосредственно в коде.</span><span class="sxs-lookup"><span data-stu-id="dfd79-202">With an extension method, you won't need to provide any localization file for the default language &mdash; the original strings are already available directly in the code.</span></span>

<span data-ttu-id="dfd79-203">Можно использовать более универсальный командлет `Plural(int count, string[] pluralForms, params object[] arguments)` перегрузку, которая принимает строковый массив переводы.</span><span class="sxs-lookup"><span data-stu-id="dfd79-203">You can use the more generic `Plural(int count, string[] pluralForms, params object[] arguments)` overload which accepts a string array of translations.</span></span>