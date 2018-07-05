---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: Создание MVC 3 Application with Razor and Unobtrusive JavaScript | Документация Майкрософт
author: microsoft
description: Список пользователей примера веб-приложения показывает, насколько он прост для создания приложений ASP.NET MVC 3, с помощью представлений Razor. Пример преобразования приложения...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/01/2010
ms.topic: article
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: 39ed35c1b7d5c702ffea6908daeac5ca12f1693e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398017"
---
<a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a>Создание MVC 3 приложения с помощью Razor и ненавязчивого JavaScript
====================
по [Microsoft](https://github.com/microsoft)

> Список пользователей примера веб-приложения показывает, насколько он прост для создания приложений ASP.NET MVC 3, с помощью представлений Razor. Пример приложения показано, как использовать новый обработчик представлений Razor в ASP.NET MVC версии 3 и Visual Studio 2010 для создания вымышленной список пользователей веб-сайта, включает в себя функции, такие как создание, отображение, редактирование и удаление пользователей.
> 
> Этот учебник описывает шаги, предпринятые для создания примера приложения ASP.NET MVC 3 список пользователей. Проект Visual Studio с исходным кодом C# и VB в этой статье прилагаются: [загрузить](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114). Если у вас есть вопросы об этом руководстве, сообщите о них для [форум MVC](https://forums.asp.net/1146.aspx).


## <a name="overview"></a>Обзор

Приложение, которое вы будете создавать веб-сайт простой пользовательский список. Пользователи могут введите, просмотра и обновления информации о пользователях.

![Пример узла](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

Можно загрузить готовый проект VB и C# [здесь](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).

## <a name="creating-the-web-application"></a>Создание веб-приложения

Чтобы начать работу с учебником, откройте Visual Studio 2010 и создайте новый проект с помощью *веб-приложение ASP.NET MVC 3* шаблона. Присвойте приложению имя &quot;Mvc3Razor&quot;.

[![Новый проект MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)

В **Создание проекта ASP.NET MVC 3** диалоговом окне выберите **веб-приложение**, выберите представлений Razor и нажмите кнопку **ОК**.

![Диалоговое окно создания проекта ASP.NET MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

В этом руководстве вы не будет использовать поставщик членства ASP.NET, поэтому можно удалить все файлы, связанные с входа в систему и членства. В **обозревателе решений**, удалите следующие файлы и каталоги:

- *Controllers\AccountController*
- *Models\AccountModels*
- *Views\Shared\\_LogOnPartial*
- *Views\Account* (и все файлы в этом каталоге)

![Soln Exp](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

Изменить  <em>\_Layout.cshtml</em> файл и замените существующую разметку внутри `<div>` элемент с именем `logindisplay` с сообщением <em>&quot;</em>имя входа отключено&quot;. В следующем примере Новая разметка:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a>Добавление модели

В **обозревателе решений**, щелкните правой кнопкой мыши *моделей* папку, выберите **добавить**, а затем нажмите кнопку **класс**.

![Новый класс Mdl пользователя](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

Присвойте классу имя `UserModel`. Замените содержимое файла *UserModel* файла следующим кодом:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

`UserModel` Класс представляет пользователей. Каждый член класса, помечается с помощью [требуется](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) из атрибута [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) пространства имен. Атрибуты в [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) пространства имен обеспечивают проверку автоматического клиента и стороне сервера для веб-приложений.

Откройте `HomeController` класса и добавьте `using` директив, позволяя получать доступ к `UserModel` и `Users` классы:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

Сразу после `HomeController` объявление, добавьте следующий комментарий и ссылку на `Users` класса:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

`Users` Класс представляет собой хранилище данных упрощенный, находящееся в памяти, которое будет использоваться в этом руководстве. В реальном приложении можно использовать базу данных для хранения пользовательских данных. Первые несколько строк `HomeController` показаны в следующем примере:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

Постройте приложение таким образом модели пользователя будет доступен мастеру формирования шаблонов в следующем шаге.

## <a name="creating-the-default-view"></a>Создание представления по умолчанию

Следующим шагом является добавление метода действия и используется для отображения пользователей.

Удалите существующие *Views\Home\Index* файла. Вы создадите новый *индекс* файл, чтобы отобразить пользователей.

В `HomeController` класса, замените содержимое файла `Index` метод следующим кодом:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

Щелкните правой кнопкой мыши внутри `Index` метод и нажмите кнопку **Добавление представления**.

![Добавление представления](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

Выберите **создать строго типизированное представление** параметр. Для **просмотреть класс данных**выберите **Mvc3Razor.Models.UserModel**. (Если вы не видите **Mvc3Razor.Models.UserModel** в **просмотреть класс данных** поле, необходимое для создания проекта.) Убедитесь, что обработчик представлений будет присвоено **Razor**. Задайте **просматривать содержимое** для **списка** и нажмите кнопку **добавить**.

![Добавление представления «индекс»](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

Новое представление автоматически формирует шаблоны данные пользователя, который передается `Index` представления. Изучите только что созданный *Views\Home\Index* файла. **Create New**, **изменить**, **сведения**, и **удалить** ссылки не работают, но остальная часть страницы функционирует. Откройте страницу. Появится список пользователей.

![Страница указателя](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

Откройте *Index.cshtml* и заменить `ActionLink` разметку для **изменить**, **сведения**, и **удалить** следующим кодом :

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

Имя пользователя используется как идентификатор для поиска для выбранной записи в **изменить**, **сведения**, и **удалить** ссылки.

## <a name="creating-the-details-view"></a>Создание представления сведений

Следующим шагом является добавление `Details` метода действия и представление для отображения сведений о пользователе.

![Подробные сведения](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

Добавьте следующий `Details` метод в контроллер home:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

Щелкните правой кнопкой мыши внутри `Details` метод, а затем выберите <strong>Добавление представления</strong>. Убедитесь, что <strong>просмотреть класс данных</strong> поле содержит <strong>Mvc3Razor.Models.UserModel</strong><em>.</em> Задайте <strong>просматривать содержимое</strong> для <strong>сведения</strong> и нажмите кнопку <strong>добавить</strong>.

![Добавление представления сведений](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

Запустите приложение и щелкните ссылку details. Автоматическое формирование шаблонов показана каждого свойства в модели.

![Подробные сведения](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a>Создание представления изменения

Добавьте следующий `Edit` метод в контроллер home.

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

Добавить представление как в предыдущих шагах, но задать **просматривать содержимое** для **изменить**.

![Добавление представления изменения](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

Запустите приложение и измените имя и фамилия одного из пользователей. Если вы нарушены `DataAnnotation` ограничения, которые были применены к `UserModel` класса, при отправке формы, вы увидите ошибки, созданные с помощью серверного кода. Например, если изменить имя &quot;Энн&quot; для &quot;объект&quot;, при отправке формы, в форме отображается следующая ошибка:

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

В этом руководстве при обработке имя пользователя в качестве первичного ключа. Таким образом невозможно изменить свойство имени пользователя. В *Edit.cshtml* файл сразу после `Html.BeginForm` оператор, задать имя пользователя, чтобы быть скрытым полем. В результате свойство для передачи в модель. В следующем фрагменте кода показано размещение `Hidden` инструкции:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

Замените `TextBoxFor` и `ValidationMessageFor` разметки для имени пользователя с `DisplayFor` вызова. `DisplayFor` Метод отображает свойство как элемент только для чтения. В следующем примере полная разметка. Исходный `TextBoxFor` и `ValidationMessageFor` вызовы закомментированы символы начала комментария и конец комментария Razor (`@* *@`)

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a>Включение проверки на стороне клиента

Чтобы включить проверку на стороне клиента в ASP.NET MVC 3, необходимо задать два флага и необходимо включить три файла JavaScript.

Откройте приложение *Web.config* файл. Проверьте `that ClientValidationEnabled` и `UnobtrusiveJavaScriptEnabled` присваивается значение true, если в параметрах приложения. В следующем фрагменте из корня *Web.config* файл отображает правильные параметры:

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

Параметр `UnobtrusiveJavaScriptEnabled` значение true, включает ненавязчивый Ajax и ненавязчивой клиентской проверки. При использовании ненавязчивой проверки, правила проверки, преобразуются в атрибуты HTML5. Имена атрибутов HTML5 может состоять из только строчные буквы, цифры и дефисы.

Параметр `ClientValidationEnabled` для true включает клиентскую проверку. Установив эти ключи в приложении *Web.config* файл, включена клиентская проверка и ненавязчивый JavaScript для всего приложения. Кроме того, можно также включить или отключить эти параметры в отдельных представлениях или в методах контроллера, используя следующий код:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

Необходимо также включить несколько файлов JavaScript в отображаемого представления. — Это способ включения JavaScript во всех представлениях, чтобы добавить их в *Views\Shared\\_Layout.cshtml* файла. Замените `<head>` элемент  *\_Layout.cshtml* файла следующим кодом:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

Первые два скриптов jQuery размещаются по Microsoft Ajax доставки содержимого сети (CDN). Используя преимущества сети Microsoft Ajax CDN, может значительно повысить производительность предварительно приложений.

Запустите приложение и щелкните ссылку изменения. Просмотрите исходный код страницы в браузере. Исходного текста в браузере отображаются многие атрибуты формы `data-val` (для проверки данных). Если включена клиентская проверка и ненавязчивый JavaScript, содержащие поля ввода с помощью правила клиентской проверки `data-val="true"` атрибут вызвал ненавязчивой клиентской проверки. Например `City` оформлен поля в модели [требуется](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) атрибут, который приводит к HTML-код, показанный в следующем примере:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

Для каждого правила клиентской проверки, добавляется атрибут, имеет форму `data-val-rulename="message"`. С помощью `City` поля, показанный ранее пример, правило требуется проверка клиента создает `data-val-required` атрибут и сообщение &quot;поле «Город» является обязательным&quot;. Запустите приложение, изменить один из пользователей и снимите `City` поля. При нажатии клавиши tab поля, появится сообщение об ошибке проверки на стороне клиента.

![Требуется Город](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

Аналогичным образом, для каждого параметра в правила клиентской проверки, добавляется атрибут, имеет форму `data-val-rulename-paramname=paramvalue`. Например `FirstName` свойство помечено с использованием [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) атрибут и Указывает минимальную длину 3 и максимальную длину 8. Правила проверки данных с именем `length` имеет имя параметра `max` и его значение 8. Ниже приведен код HTML, который создается для `FirstName` поле при изменении одного из пользователей:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

Дополнительные сведения о ненавязчивой клиентской проверки, см. в записи [ненавязчивой клиентской проверки в ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) в блоге Брэда уилсона:.

> [!NOTE]
> Иногда, в ASP.NET MVC 3 бета-версии, необходимо отправить форму, чтобы запустить проверку на стороне клиента. Это может быть изменено в окончательном выпуске.


## <a name="creating-the-create-view"></a>Создание представления создания

Следующим шагом является добавление `Create` метода действия и представление, чтобы пользователь мог создать нового пользователя. Добавьте следующий `Create` метод в контроллер home:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

Добавить представление как в предыдущих шагах, но задать **просматривать содержимое** для **создать**.

![Создать просмотр](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

Запустите приложение, выберите **создать** связать, и добавить нового пользователя. `Create` Автоматически использует преимущества проверки на стороне клиента и на стороне сервера. Попробуйте ввести имя пользователя, которое содержит пробел, такие как &quot;Бен X&quot;. При нажатии клавиши tab поле имени пользователя, ошибки проверки на стороне клиента (`White space is not allowed`) отображается.

## <a name="add-the-delete-method"></a>Добавьте метод Delete

Для работы с учебником, добавьте следующий `Delete` метод в контроллер home:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

Добавить `Delete` представление как в предыдущих шагах, установка **просматривать содержимое** для **удалить**.

![Удалить представление](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

Теперь у вас есть простой, но полнофункциональное приложение ASP.NET MVC 3 с проверкой.
