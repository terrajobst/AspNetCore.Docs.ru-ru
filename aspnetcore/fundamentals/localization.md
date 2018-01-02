---
title: "Глобализация и локализация в ASP.NET Core"
author: rick-anderson
description: "Узнайте, как ASP.NET Core предоставляет службы и по промежуточного слоя для локализация содержимого на разных языках и региональных параметров."
keywords: "ASP.NET Core, локализации, языка и региональных параметров, язык, файл ресурсов, глобализации, локализации, языковой стандарт"
ms.author: riande
manager: wpickett
ms.date: 01/14/2017
ms.topic: article
ms.assetid: 7f275a09-f118-41c9-88d1-8de52d6a5aa1
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/localization
ms.openlocfilehash: a3fdbf8a1ab4ca397824a46da445fa34ddd35204
ms.sourcegitcommit: 4be61844141d3cfb6f263636a36aebd26e90fb28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/22/2017
---
# <a name="globalization-and-localization-in-aspnet-core"></a>Глобализация и локализация в ASP.NET Core

По [Рик Андерсон](https://twitter.com/RickAndMSFT), [Бауден Дэмьен](https://twitter.com/damien_bod), [Bart Calixto](https://twitter.com/bartmax), [Надим Афана](https://twitter.com/NadeemAfana), и [Ateya Hisham Bin](https://twitter.com/hishambinateya)

Создание многоязычных веб-сайта с помощью ASP.NET Core позволит узла для достижения более широкую аудиторию. ASP.NET Core предоставляет службы и ПО промежуточного слоя для локализации на разные языки и для разных региональных параметров.

Включает в себя интернационализации [глобализации](https://docs.microsoft.com/dotnet/api/system.globalization) и [локализации](https://docs.microsoft.com/dotnet/standard/globalization-localization/localization). Глобализация — это процесс разработки приложений, которые поддерживают разные культуры. Глобализация добавляет поддержку для ввода, отображения и вывода определенного набора языковых сценариев, относящихся к конкретным регионам.

Локализация — это процесс адаптации глобализованное приложение, которое уже обработана возможности локализации для определенного языка и региональных параметров или языкового стандарта.  Дополнительные сведения см. **глобализации и локализации условия** в конце этого документа.

Локализация приложений включает следующие этапы:

1. Сделать локализуемого содержимого приложения

2. Укажите локализованные ресурсы для языков и региональных параметров, поддерживаемых

3. Реализовать стратегию, чтобы выбрать язык и региональные параметры для каждого запроса

## <a name="make-the-apps-content-localizable"></a>Сделать локализуемого содержимого приложения

Представленные в ASP.NET Core `IStringLocalizer` и `IStringLocalizer<T>` были разработана для повышения производительности при разработке локализованных приложений. `IStringLocalizer`использует [ResourceManager](https://docs.microsoft.com/dotnet/api/system.resources.resourcemanager) и [ResourceReader](https://docs.microsoft.com/dotnet/api/system.resources.resourcereader) для предоставления ресурсов, связанных с языком и региональными параметрами, во время выполнения. Простой интерфейс имеет индекс и `IEnumerable` для возвращения локализованные строки. `IStringLocalizer`не требуется хранить строки языка по умолчанию в файле ресурсов. Можно разрабатывать приложения, предназначенные для локализации и не требуется создавать файлы ресурсов на раннем этапе разработки. В следующем примере кода показано, как программы-оболочки для строки «О Title» для локализации.

[!code-csharp[Main](localization/sample/Localization/Controllers/AboutController.cs)]

В представленном выше коде `IStringLocalizer<T>` реализацию поступают из [внедрения зависимостей](dependency-injection.md). Если локализованное значение атрибута «О Title» не найден, то ключу индексатора возвращается, то есть строка «О Title». Можно оставить значение по умолчанию язык строковых литералов в приложении и помещать их в локализатора, так что можно сосредоточиться на разработке приложений. Разработка приложения с языком по умолчанию и подготовить его к этапу локализации без предварительного создания файла ресурсов по умолчанию. Кроме того можно использовать традиционный подход и укажите ключ для извлечения языковую строку по умолчанию. Для многих разработчиков нового рабочего процесса не было язык по умолчанию *.resx* файл и просто обернуть строковые литералы можно снизить издержки локализация приложения. Другим разработчикам использовать традиционные рабочий процесс как его можно упростить его для работы с более строковые литералы и упростить обновление локализованные строки.

Используйте `IHtmlLocalizer<T>` реализации для ресурсов, содержащие HTML. `IHtmlLocalizer`Кодирует строку ресурса форматируемые аргументы HTML, но кодировка HTML не собственно строкой ресурса. В примере выделен под только значение `name` параметр — в кодировке HTML.

[!code-csharp[Main](../fundamentals/localization/sample/Localization/Controllers/BookController.cs?highlight=3,5,20&start=1&end=24)]

**Примечание:** обычно требуется только локализовать текст и не HTML.

На самом низком уровне, вы можете получить `IStringLocalizerFactory` из [внедрения зависимости](dependency-injection.md):

[!code-csharp[Main](localization/sample/Localization/Controllers/TestController.cs?start=9&end=26&highlight=7-13)]

Приведенный выше код демонстрирует каждого из двух фабрику создания методов.

Можно секционировать локализованные строки контроллером области, или только один контейнер. В примере приложения пустой класс с именем `SharedResource` используется для общих ресурсов.

[!code-csharp[Main](localization/sample/Localization/Resources/SharedResource.cs)]

Некоторые разработчики используют `Startup` класс, содержащий глобальные или общих строк. В приведенном ниже примере `InfoController` и `SharedResource` локализаторам используются:

[!code-csharp[Main](localization/sample/Localization/Controllers/InfoController.cs?range=9-26)]

## <a name="view-localization"></a>Представление локализации

`IViewLocalizer` Служба предоставляет локализованные строки для [представление](https://docs.microsoft.com/aspnet/core). `ViewLocalizer` Класс реализует этот интерфейс и находит расположение ресурса из пути файла представления. Ниже показано, как использовать реализацию по умолчанию `IViewLocalizer`:

[!code-cshtml[Main](localization/sample/Localization/Views/Home/About.cshtml)]

Реализация по умолчанию `IViewLocalizer` находит файл ресурсов на основе имени файла этого представления. Нет возможности использовать файл общих ресурсов. `ViewLocalizer`реализует с помощью локализатора `IHtmlLocalizer`, поэтому Razor не HTML кодирования локализованные строки. Можно параметризировать строки ресурсов и `IViewLocalizer` будет HTML кодирования параметров, но не строки ресурса. Рассмотрим следующую разметку Razor.

```cshtml
@Localizer["<i>Hello</i> <b>{0}!</b>", UserManager.GetUserName(User)]
```

Файл ресурсов может содержать следующие элементы.

| Ключ | Значение |
| ----- | ------ |
| `<i>Hello</i> <b>{0}!</b>` | `<i>Bonjour</i> <b>{0} !</b> ` |

Отображенное представление будет содержать HTML-разметку из файла ресурсов.

**Примечание:** обычно требуется только локализовать текст и не HTML.

Использование файла общего ресурса в виде, вставки `IHtmlLocalizer<T>`:

[!code-cshtml[Main](../fundamentals/localization/sample/Localization/Views/Test/About.cshtml?highlight=5,12)]

## <a name="dataannotations-localization"></a>Локализация DataAnnotations

Сообщения об ошибках DataAnnotations локализованы с `IStringLocalizer<T>`. С помощью параметра `ResourcesPath = "Resources"`, сообщения об ошибках в `RegisterViewModel` могут храниться в одном из следующих путей:

* Resources/ViewModels.Account.RegisterViewModel.fr.resx
* Resources/ViewModels/Account/RegisterViewModel.fr.resx

[!code-csharp[Main](localization/sample/Localization/ViewModels/Account/RegisterViewModel.cs?start=9&end=26)]

В ASP.NET Core MVC 1.1.0 и более поздней версии, не относящееся к проверке атрибутов локализованы. ASP.NET Core MVC 1.0 **не** поиск локализованных строк для атрибутов без проверки.

<a name="one-resource-string-multiple-classes"></a>
### <a name="using-one-resource-string-for-multiple-classes"></a>Используя одну строку ресурса для нескольких классов

Ниже показано, как использовать одну строку ресурса для проверки атрибутов с несколькими классами:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddDataAnnotationsLocalization(options => {
            options.DataAnnotationLocalizerProvider = (type, factory) =>
                factory.Create(typeof(SharedResource));
        });
}
```

В предыдущем коде `SharedResource` является класс, соответствующий resx, в которых хранятся сообщения проверки. При таком подходе DataAnnotations будет использовать только `SharedResource`, вместо того чтобы ресурсов для каждого класса. 

## <a name="provide-localized-resources-for-the-languages-and-cultures-you-support"></a>Укажите локализованные ресурсы для языков и региональных параметров, поддерживаемых  

### <a name="supportedcultures-and-supporteduicultures"></a>SupportedCultures и SupportedUICultures

ASP.NET Core позволяет задать два значения языка и региональных параметров, `SupportedCultures` и `SupportedUICultures`. [CultureInfo](https://docs.microsoft.com/dotnet/api/system.globalization.cultureinfo) для объекта `SupportedCultures` определяет результаты функции, зависящие от языка и региональных параметров, такие как дата, время, числа и денежных единиц. `SupportedCultures`также определяет порядок сортировки текста, правила регистра и сравнения строк. В разделе [CultureInfo.CurrentCulture](https://docs.microsoft.com/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture) для получения дополнительных сведений о как сервер получает язык и региональные параметры. `SupportedUICultures` Определяет, что означает строк (из *.resx* файлы) поиск которого выполняется [ResourceManager](https://docs.microsoft.com/dotnet/api/system.resources.resourcemanager). `ResourceManager` Просто выполняет поиск строк, связанных с языком и региональными параметрами, определяется `CurrentUICulture`. Каждый поток в .NET имеет `CurrentCulture` и `CurrentUICulture` объектов. ASP.NET Core проверяет эти значения при подготовке к просмотру функции, зависящие от языка и региональных параметров. Например, если язык и региональные параметры текущего потока имеет значение «en US» (английский, США) `DateTime.Now.ToLongDateString()` отображает «Четверг, февраль 18 2016", но если `CurrentCulture` имеет значение для «es-ES» (испанский, Испания) выходные данные будут «jueves de febrero 18 de 2016".

## <a name="resource-files"></a>Файлы ресурсов

Файл ресурсов — это механизм полезен для разделения локализуемые строки из кода. Переведенные строки для языка не по умолчанию изолированы *.resx* файлы ресурсов. Например, может потребоваться создать файл ресурсов для испанского языка с именем *Welcome.es.resx* содержащий перевода строки. «es» — это код языка для испанского языка. Чтобы создать этот файл ресурсов в Visual Studio:

1. В **обозревателе решений**, щелкните правой кнопкой мыши папку, которая будет содержать файл ресурсов > **добавить** > **новый элемент**.

    ![Вложенные контекстного меню: В обозревателе решений контекстное меню открыт для ресурсов. Второе контекстные меню открыт для отображения выделен новый элемент-команда Add.](localization/_static/newi.png)

2. В **поиск установленных шаблонов** введите «resource» и имя файла.

    ![Диалоговое окно ''Добавление нового элемента''](localization/_static/res.png)

3. Введите значение ключа (собственную строку) в **имя** столбец и переведенных строк в **значение** столбца.

    ![Welcome.es.resx (файл приветствия ресурсов для испанского языка) с слово Hello в столбце «имя» и слово Hola (Hello на испанском языке) в столбце значений](localization/_static/hola.png)

    Visual Studio отображает *Welcome.es.resx* файла.

    ![В обозревателе решений файл ресурсов приветствия испанский (es)](localization/_static/se.png)

<a name="error"></a>

При использовании предварительной версии Visual Studio 2017 г. версия 15,3 появится индикатор ошибки в редакторе ресурсов. Удалить *ResXFileCodeGenerator* значение из *пользовательский инструмент* сетку свойств, чтобы предотвратить это сообщение об ошибке:

![RESX-редактор](localization/_static/err.png)

Кроме того эту ошибку можно игнорировать. Мы надеемся устранить эту проблему в следующем выпуске.

## <a name="resource-file-naming"></a>Имя файла ресурсов

Ресурсы были указаны полное имя типа класса минус имя сборки. Например, французским ресурсов в проекте, в основной сборке `LocalizationWebsite.Web.dll` для класса `LocalizationWebsite.Web.Startup` будет называться *Startup.fr.resx*. Ресурс для класса `LocalizationWebsite.Web.Controllers.HomeController` будет называться *Controllers.HomeController.fr.resx*. Если пространство имен для целевого класса не соответствует имени сборки необходимо будет полное имя типа. Например, в образце проекта для типа ресурса `ExtraNamespace.Tools` будет называться *ExtraNamespace.Tools.fr.resx*.

В образце проекта `ConfigureServices` метода задает `ResourcesPath` на «Ресурсы», поэтому относительный путь проекта для контроллера home файл ресурсов — *Resources/Controllers.HomeController.fr.resx*. Кроме того можно использовать папки для организации файлов ресурсов. Для контроллера home, путь будет иметь *Resources/Controllers/HomeController.fr.resx*. Если вы не используете `ResourcesPath` параметр *.resx* файл перейдет в базовом каталоге проекта. Файл ресурсов для `HomeController` будет называться *Controllers.HomeController.fr.resx*. Вариант с использованием точки или путь соглашение об именовании зависит способ упорядочения файлов ресурсов.

| Имя ресурса | Точка, имен пути |
| ------------   | ------------- |
| Resources/Controllers.HomeController.fr.resx | Точка  |
| Resources/Controllers/HomeController.fr.resx  | Путь |
|    |     |

Файлы ресурсов с помощью `@inject IViewLocalizer` в представлений Razor выполнить той же модели. Файл ресурсов для представления может называться именования точку или путь именования. Файлы ресурсов представления Razor имитируют путь к файлу их связанного представления. При условии, что мы устанавливаем `ResourcesPath` «Ресурсы», файл ресурсов, связанных с *Views/Home/About.cshtml* представление может быть одно из следующих:

* Resources/Views/Home/About.fr.resx

* Resources/Views.Home.About.fr.resx

Если вы не используете `ResourcesPath` параметр *.resx* -файле для представления будет находиться в той же папке, что и представление.

## <a name="culture-fallback-behavior"></a>Поведение языка и региональных параметров

Например если удалить буквенный код языка и региональных параметров «.fr» и языка и региональных параметров, французском, чтение файла ресурсов по умолчанию и локализованных строк. Диспетчер ресурсов назначает значение по умолчанию или на резервный ресурс для Если ничего не удовлетворяет требованиям вашего запрошенного языка и региональных параметров. Если вы хотите просто возвращается ключ, если отсутствует ресурс для запрошенной культуры не должен иметь файл ресурсов по умолчанию.

### <a name="generate-resource-files-with-visual-studio"></a>Создавать файлы ресурсов с помощью Visual Studio

При создании файла ресурсов в Visual Studio без языка и региональных параметров в имени файла (например, *Welcome.resx*), Visual Studio создаст класс C# с помощью свойства для каждой строки. Это обычно не нужны с ASP.NET Core; значение по умолчанию обычно не имеют *.resx* файла ресурсов (A *.resx* файл без имени языка и региональных параметров). Мы рекомендуем вам создать *.resx* файл с именем языка и региональных параметров (например *Welcome.fr.resx*). При создании *.resx* файл с именем языка и региональных параметров, Visual Studio не создаст файл класса. Мы полагаем, что многие разработчики будут **не** создать файл ресурсов языка по умолчанию.

### <a name="add-other-cultures"></a>Добавление других языков и региональных параметров

Каждой комбинации языка и региональных параметров (отличных от языка по умолчанию) требуется отдельный файл ресурсов. Создание файлов ресурсов для разных языков и региональных параметров путем создания новых файлов ресурсов, в которых языковые коды ISO являются частью имени файла (например, **en-us**, **fr-ca**, и  **en-gb**). Эти коды ISO помещаются между именем файла и *.resx* файл с расширением, как и в *Welcome.es MX.resx* (Испанский/Мексика). Чтобы указать язык, удалите код страны (`MX` в предыдущем примере). Имя файла ресурсов нейтральным испанский — *Welcome.es.resx*.

## <a name="implement-a-strategy-to-select-the-languageculture-for-each-request"></a>Реализовать стратегию, чтобы выбрать язык и региональные параметры для каждого запроса  

### <a name="configure-localization"></a>Настройка локализации

Локализация настраивается в `ConfigureServices` метод:

[!code-csharp[Main](localization/sample/Localization/Program.cs?name=snippet1)]

* `AddLocalization`Добавляет службы локализации в контейнере службы. Приведенный выше также задает путь к ресурсам «Ресурсы».

* `AddViewLocalization`Добавлена поддержка локализованных Просмотр файлов. В этом представлении Образец локализации основан на суффикса файла представления. Например «fr» в *Index.fr.cshtml* файла.

* `AddDataAnnotationsLocalization`Добавляет поддержку для локализации `DataAnnotations` проверки сообщений через `IStringLocalizer` абстрактные классы.

### <a name="localization-middleware"></a>Локализация по промежуточного слоя

Текущий язык и региональные параметры в запросе задается в локализации [по промежуточного слоя](middleware.md). По промежуточного слоя локализации включено в `Configure` метод *Program.cs* файла. Обратите внимание, что по промежуточного слоя локализации должен быть настроен перед любое по промежуточного слоя, который может проверить запрос языка и региональных параметров (например, `app.UseMvcWithDefaultRoute()`).

[!code-csharp[Main](localization/sample/Localization/Program.cs?name=snippet2)]

`UseRequestLocalization`Инициализирует `RequestLocalizationOptions` объекта. При каждом запросе списка из `RequestCultureProvider` в `RequestLocalizationOptions` перечисляется и используется первый поставщик, успешно можно определить запрос языка и региональных параметров. Поставщики по умолчанию берутся из `RequestLocalizationOptions` класса:

1. `QueryStringRequestCultureProvider`
2. `CookieRequestCultureProvider`
3. `AcceptLanguageHeaderRequestCultureProvider`

Список по умолчанию передается от более свойственных наименее конкретным. Далее в этой статье мы рассмотрим, как можно изменить порядок и даже добавить поставщик пользовательского языка и региональных параметров. Если ни один из поставщиков можно определить запрос языка и региональных параметров, `DefaultRequestCulture` используется.

### <a name="querystringrequestcultureprovider"></a>QueryStringRequestCultureProvider

Некоторые приложения будет использовать строку запроса для задания [culture и Uiculture](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx). Для приложений, использующих файлы cookie или подход заголовок Accept-Language добавив строку запроса URL-адрес полезен для отладки и тестирования кода. По умолчанию `QueryStringRequestCultureProvider` регистрируется как первый Поставщик локализации в `RequestCultureProvider` списка. Запрос передан в строковых параметров `culture` и `ui-culture`. Следующий пример присваивает Испанский/Мексика определенного языка и региональных параметров (язык и регион):

   `http://localhost:5000/?culture=es-MX&ui-culture=es-MX`

Если передается только в одном из двух (`culture` или `ui-culture`), поставщика строка запроса будет оба значения с помощью тот, который передается в. Например, задание только культуры установит оба `Culture` и `UICulture`:

   `http://localhost:5000/?culture=es-MX`

### <a name="cookierequestcultureprovider"></a>CookieRequestCultureProvider

Производственного приложения часто предоставляют механизм для задания языка и региональных параметров с cookie ASP.NET Core языка и региональных параметров. Используйте `MakeCookieValue` метод для создания файла cookie.

`CookieRequestCultureProvider` `DefaultCookieName` Возвращает имя файла cookie по умолчанию, используемое для отслеживания пользователя предпочитаемый язык и региональные параметры сведения. Имя файла cookie по умолчанию — «. AspNetCore.Culture».

Недопустимый формат файла cookie `c=%LANGCODE%|uic=%LANGCODE%`, где `c` — `Culture` и `uic` — `UICulture`, например:

    c=en-UK|uic=en-US

Если задано только один язык и региональные параметры и язык и региональные параметры пользовательского интерфейса, язык и региональные параметры и язык и региональные параметры пользовательского интерфейса будет использоваться указанного языка и региональных параметров.

### <a name="the-accept-language-http-header"></a>Заголовок HTTP Accept-Language

[Заголовка Accept-Language](https://www.w3.org/International/questions/qa-accept-lang-locales) можно задать в большинстве браузеров и изначально не предназначен для указания языка пользователя. Этот параметр указывает, что браузер задал для отправки или унаследованы от операционной системы. Заголовок HTTP Accept-Language из запрос браузера не непогрешимыми способ определить язык пользователя (в разделе [настройках языка в браузере](https://www.w3.org/International/questions/qa-lang-priorities.en.php)). Рабочее приложение должно включать способом для пользователя для настройки собственного языка и региональных параметров.

### <a name="set-the-accept-language-http-header-in-ie"></a>Задайте для заголовка HTTP Accept-Language в IE

1. Щелкните значок шестеренки, коснитесь **обозревателя**.

2. Коснитесь **языков**.

    ![Параметры Интернета](localization/_static/lang.png)

3. Коснитесь **задавать настройки языка**.

4. Коснитесь **добавить язык**.

5. Добавьте язык.

6. Коснитесь язык, а затем **вверх**.

### <a name="use-a-custom-provider"></a>Использовать пользовательский поставщик

Предположим, что нужно позволить клиентам хранить их языка и региональных параметров в базах данных. Можно написать поставщика для поиска этих значений для пользователя. Следующий код демонстрирует добавление настраиваемого поставщика:

```csharp
private const string enUSCulture = "en-US";

services.Configure<RequestLocalizationOptions>(options =>
{
    var supportedCultures = new[]
    {
        new CultureInfo(enUSCulture),
        new CultureInfo("fr")
    };

    options.DefaultRequestCulture = new RequestCulture(culture: enUSCulture, uiCulture: enUSCulture);
    options.SupportedCultures = supportedCultures;
    options.SupportedUICultures = supportedCultures;

    options.RequestCultureProviders.Insert(0, new CustomRequestCultureProvider(async context =>
    {
        // My custom request culture logic
        return new ProviderCultureResult("en");
    }));
});
```

Используйте `RequestLocalizationOptions` для добавления или удаления поставщиков локализации.

### <a name="set-the-culture-programmatically"></a>Установка языка и региональных параметров программным способом

В этом примере **Localization.StarterWeb** проект на [GitHub](https://github.com/aspnet/entropy) содержится пользовательский Интерфейс, чтобы задать `Culture`. *Views/Shared/_SelectLanguagePartial.cshtml* файла можно выбрать из списка поддерживаемых языков и региональных параметров языка и региональных параметров:

[!code-cshtml[Main](localization/sample/Localization/Views/Shared/_SelectLanguagePartial.cshtml)]

*Views/Shared/_SelectLanguagePartial.cshtml* добавляется файл `footer` раздел файла разметки, она будет доступна во всех представлениях:

[!code-cshtml[Main](localization/sample/Localization/Views/Shared/_Layout.cshtml?range=43-56&highlight=10)]

`SetLanguage` Метод задает язык и региональные параметры cookie.

[!code-csharp[Main](localization/sample/Localization/Controllers/HomeController.cs?range=57-67)]

Не удается подключить *_SelectLanguagePartial.cshtml* на примеры кода для данного проекта. **Localization.StarterWeb** проект на [GitHub](https://github.com/aspnet/entropy) содержится код для потока `RequestLocalizationOptions` для Razor, частичная через [внедрения зависимостей](dependency-injection.md) контейнера.

## <a name="globalization-and-localization-terms"></a>Глобализация и локализация условия

Процесс локализации приложения необходимо также основные сведения о соответствующей кодировки, обычно используемые в разработке современного программного обеспечения и понимания проблем, связанных с ними. Несмотря на то, что все компьютеры сохранения текста в виде чисел (коды), разных систем хранения тот же текст, с помощью различных чисел. Процесс локализации ссылается на перевод пользовательского интерфейса (UI) приложения для определенного языка и региональных параметров или языкового стандарта.

[Локализуемость](https://docs.microsoft.com/dotnet/standard/globalization-localization/localizability-review) — это промежуточный процесс для проверки того, что международное приложение готово к локализации.

[RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) формат имя языка и региональных параметров является "<languagecode2>-< код_страны_или_региона2 >», где <languagecode2> является кодом языка, а < код_страны_или_региона2 > — код региона. Например `es-CL` испанский (Чили) `en-US` для английского языка (США) и `en-AU` для английского языка (Австралия). [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) представляет собой сочетание ISO 639, кода двух букв нижнего регистра, языка и региональных параметров, связанных с языком и ISO 3166, код страны или региона на прописных букв, связанные с страны или региона.  В разделе [имя языка и региональных параметров языка](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx).

Интернационализация часто называют «I18N». Сокращение принимает первую и последнюю буквы и количеством букв между ними, поэтому 18 означает количество буквы между первым «I» и последнего «N». То же самое происходит (G11N) глобализации и локализации (L10N).

Условия:

* Глобализация (G11N): Процесс создания приложения поддержки различных языков и регионов.
* Локализация (L10N): Процесс настройки приложения для данного языка и региона.
* Интернационализация (I18N): Описание глобализации и локализации.
* Культура:, Это язык, возможно, область.
* Нейтральный язык и региональные параметры: язык и региональные параметры указанного языка, но не область. (например «en», «es»)
* Определенного языка и региональных параметров: языка и региональных параметров, который имеет указанный язык и регион. (для, например «en US», «en-GB», «es-CL»)
* Родительский язык и региональные параметры: нейтральной культуры, содержащий определенного языка и региональных параметров. (например, «en» является родительского языка и региональных параметров «en US» и «en-GB»)
* Язык: Языковой стандарт — совпадает с языком и региональными параметрами.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Проект Localization.StarterWeb](https://github.com/aspnet/entropy) используется в статье.
* [Файлы ресурсов в Visual Studio](https://docs.microsoft.com/cpp/windows/resource-files-visual-studio)
* [Ресурсы в RESX-файлы](https://docs.microsoft.com/dotnet/framework/resources/working-with-resx-files-programmatically)
