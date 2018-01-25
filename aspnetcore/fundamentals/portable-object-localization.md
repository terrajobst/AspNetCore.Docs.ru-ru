---
title: "Настройка объекта переносимой локализации"
author: sebastienros
description: "В этой статье приведены сведения о файлах переносимый объект и описаны действия по их использованию в приложении ASP.NET Core с помощью платформы Orchard Core."
ms.author: scaddie
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/portable-object-localization
ms.openlocfilehash: ad68c8a7df5a8ea0f7ef42137c29cd3b37657052
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
# <a name="configure-portable-object-localization-with-orchard-core"></a>Настройка объекта переносимой локализации с основными Orchard

По [Sébastien Ros](https://github.com/sebastienros) и [Скотт Addie](https://twitter.com/Scott_Addie)

В этой статье рассматриваются действия по использованию файлов переносимой объекта (PO) в приложении ASP.NET Core с [Orchard Core](https://github.com/OrchardCMS/OrchardCore) framework.

**Примечание:** Orchard ядра не продуктов корпорации Майкрософт. Следовательно Корпорация Майкрософт не поддерживает эту функцию.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-a-po-file"></a>Что такое файл PO?

Файлы PO распространяются как текстовые файлы, содержащие переведенных строк для данного языка. Некоторые преимущества использования файлов PO *.resx* файлы включают в себя:
- Файлы PO поддерживают преобразование во множественную форму; *.resx* файлы не поддерживают преобразование во множественную форму.
- Файлы PO не компилируются подобно *.resx* файлов. Таким образом специальные действия сборки и оснащения, не являются обязательными.
- Файлы PO работает со средствами совместной работы сети редактирования.

### <a name="example"></a>Пример

Ниже приведен образец файла PO содержащая перевод для двух строк на французском языке, включая его формы во множественном числе:

*fr.po*

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

В этом примере используется следующий синтаксис:

- `#:`: Комментарий, указывающее контекст строки, которые будут преобразованы. Та же строка может быть переведены различным образом в зависимости от того, где он используется.
- `msgid`: Непреобразованный строка.
- `msgstr`: Переведенных строк.

В случае поддержки преобразование во множественную форму можно определить дополнительные записи.

- `msgid_plural`: Непреобразованный во множественном числе строка.
- `msgstr[0]`: Переведенных строк для варианта 0.
- `msgstr[N]`: Переведенных строк для вариантов N.

Спецификация файла PO можно найти [здесь](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html).

## <a name="configuring-po-file-support-in-aspnet-core"></a>Настройка поддержки файлов заказа на Покупку в ASP.NET Core

Этот пример основан на основных компонентов MVC-приложениях ASP.NET создан из шаблона проекта Visual Studio 2017 г.

### <a name="referencing-the-package"></a>Ссылка на пакет

Добавьте ссылку на `OrchardCore.Localization.Core` пакет NuGet. Его можно найти на [MyGet](https://www.myget.org/) источника следующих пакетов: https://www.myget.org/F/orchardcore-preview/api/v3/index.json

*.Csproj* файл теперь содержит строку следующего вида (номер версии могут различаться):

[!code-xml[Main](localization/sample/POLocalization/POLocalization.csproj?range=9)]

### <a name="registering-the-service"></a>Регистрация службы

Добавьте необходимые службы, чтобы `ConfigureServices` метод *файла Startup.cs*:

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_ConfigureServices&highlight=4-21)]

Добавьте необходимые по промежуточного слоя для `Configure` метод *файла Startup.cs*:

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_Configure&highlight=15)]

Добавьте следующий код для представления Razor по выбору. *About.cshtml* используется в этом примере.

[!code-cshtml[Main](localization/sample/POLocalization/Views/Home/About.cshtml)]

`IViewLocalizer` Экземпляра внедренного и используемый для преобразования текста «Hello world!».

### <a name="creating-a-po-file"></a>Создание файла заказа на Покупку

Создайте файл с именем  *<culture code>расширениями* в корневой папке приложения. В этом примере имя файла — *fr.po* так, как используется французский язык:

[!code-text[Main](localization/sample/POLocalization/fr.po)]

Файл содержит строку для преобразования и французский перевод строки. Переводы вернуться к их родительский язык и региональные параметры, при необходимости. В этом примере *fr.po* файл используется в том случае, если запрошенный язык и региональные параметры `fr-FR` или `fr-CA`.

### <a name="testing-the-application"></a>Тестирование приложения

Запустите приложение и перейдите по URL-адресу `/Home/About`. Текст **Здравствуй, мир!** отображается.

Перейдите на URL-адрес `/Home/About?culture=fr-FR`. Текст **Bonjour le monde!** отображается.

## <a name="pluralization"></a>Преобразование во множественную форму

Файлы PO поддерживает преобразование во множественную форму формы, это полезно, когда та же строка требует перевода различаться в зависимости от количества элементов. Эта задача выполняется сложной из-за того, что каждый язык определяет настраиваемые правила, чтобы выбрать строку, используемую в зависимости от количества элементов.

Пакет Orchard локализации предоставляет API для вызова этих различных форм множественного числа автоматически.

### <a name="creating-pluralization-po-files"></a>Создание файлов PO преобразование во множественную форму

Добавьте следующее содержимое в ранее упомянутых *fr.po* файла:

```text
msgid "There is one item."
msgid_plural "There are {0} items."
msgstr[0] "Il y a un élément."
msgstr[1] "Il y a {0} éléments."
```

В разделе [что такое файл PO?](#what-is-a-po-file) каждая запись в этом примере представляет описание.

### <a name="adding-a-language-using-different-pluralization-forms"></a>Добавление языка, с помощью форм разных преобразование во множественную форму

В предыдущем примере были использованы строки английского и французского языков. Английского и французского имеет только две формы преобразование во множественную форму и совместно использовать те же правила формы это, что количество элементов один сопоставляется первой формы во множественном числе. Другие количества элементов сопоставляются с вторая форма множественного числа.

Не все языки используют те же самые правила. Это показано с чешском языке, который имеет три формы во множественном числе.

Создание `cs.po` следующим образом и обратите внимание на то, каким образом преобразование во множественную форму должно три различным переводам:

[!code-text[Main](localization/sample/POLocalization/cs.po)]

Чтобы принять чешский локализации, добавьте `"cs"` в список поддерживаемых языков и региональных параметров в `ConfigureServices` метод:

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

Изменить *Views/Home/About.cshtml* файл для отображения локализованных строк для нескольких мощности во множественном числе:

```cshtml
<p>@Localizer.Plural(1, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(2, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(5, "There is one item.", "There are {0} items.")</p>
```

**Примечание:** в реальной ситуации переменной будет использоваться для представления числа. Здесь мы повторяем один и тот же код с тремя различными значениями для предоставления очень определенный регистр.

После переключения языков и региональных параметров, см.

Для `/Home/About`:

```html
There is one item.
There are 2 items.
There are 5 items.
```

Для `/Home/About?culture=fr`:

```html
Il y a un élément.
Il y a 2 éléments.
Il y a 5 éléments.
```

Для `/Home/About?culture=cs`:

```html
Existuje jedna položka.
Existují 2 položky.
Existuje 5 položek.
```

Обратите внимание, что для чешского языка и региональных параметров, три переводы отличаются. Языки и региональные параметры французский "и" Английский совместное использование одной конструкции для двух последних переведенных строк.

## <a name="advanced-tasks"></a>Расширенные задачи

### <a name="contextualizing-strings"></a>Изучение в контексте строк

Приложения часто содержат строки, которые будут преобразованы в нескольких местах. Та же строка может иметь различные преобразования в определенных местах внутри приложения (представлений Razor или файлы класса). Файл PO поддерживает понятие контекста файла, который может использоваться для классификации, представленному в строку. С помощью контекста файла, строки могут быть переведены по-разному, в зависимости от контекста файла (или отсутствия контекста файла).

Службы локализации PO используют имя полный класс или представление, которое используется при преобразовании строки. Это достигается путем установки значения на `msgctxt` запись.

Рассмотрим незначительным дополнением к предыдущему *fr.po* примере. Представления Razor, расположенный *Views/Home/About.cshtml* может определяться как контекст файла, задав зарезервировано `msgctxt` значение этого параметра:

```text
msgctxt "Views.Home.About"
msgid "Hello world!"
msgstr "Bonjour le monde!"
```

С `msgctxt` задано таким образом, перевод текста происходит при переходе к `/Home/About?culture=fr-FR`. Перевод не произойдет при переходе к `/Home/Contact?culture=fr-FR`.

При совпадении с контекстом данный файл не определенной записи резервный механизм Orchard Core ищет в соответствующий файл PO без контекста. При отсутствии является контекст не определенный файл, определенный для *Views/Home/Contact.cshtml*, перехода по страницам `/Home/Contact?culture=fr-FR` загружает файл заказа на Покупку, такие как:

[!code-text[Main](localization/sample/POLocalization/fr.po)]

### <a name="changing-the-location-of-po-files"></a>Изменение расположения файлов заказа на Покупку

Можно изменить расположение по умолчанию файлы заказа на Покупку в `ConfigureServices`:

```csharp
services.AddPortableObjectLocalization(options => options.ResourcesPath = "Localization");
```

В этом примере PO файлы загружаются из *локализации* папки.

### <a name="implementing-a-custom-logic-for-finding-localization-files"></a>Реализация пользовательской логики для поиска файлов локализации

Когда требуется более сложная логика для поиска файлов PO `OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider` интерфейс можно реализовать и зарегистрирован как служба. Это полезно в том случае, когда PO файлы могут храниться в различных местах или когда файлы должны размещаться в иерархии папок.

### <a name="using-a-different-default-pluralized-language"></a>С помощью языка по умолчанию имена во множественном числе

В пакете `Plural` метод расширения, относящиеся к две формы во множественном числе. Для языков, требующие другие формы во множественном числе создание метода расширения. С помощью метода расширения не нужно будет вводить любой файл локализации для языка по умолчанию &mdash; исходных строк уже доступны непосредственно в коде.

Можно использовать более универсальный командлет `Plural(int count, string[] pluralForms, params object[] arguments)` перегрузку, которая принимает строковый массив переводы.
