---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: "Создание MVC 3 Application with Razor and Unobtrusive JavaScript | Документы Microsoft"
author: microsoft
description: "Список пользователей примера веб-приложения показано, как простой является создание приложения ASP.NET MVC 3 с помощью представлений Razor. В образце приложения s..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/01/2010
ms.topic: article
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: 68870caf1608e596962650cf653e5b455b82382a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a>Создание MVC 3 Application with Razor and Unobtrusive JavaScript
====================
по [Microsoft](https://github.com/microsoft)

> Список пользователей примера веб-приложения показано, как простой является создание приложения ASP.NET MVC 3 с помощью представлений Razor. Образец приложения показано, как использовать новый обработчик представлений Razor с ASP.NET MVC версии 3 и Visual Studio 2010 для создания вымышленной список пользователей веб-сайта, включающий функции, такие как создание, отображение, редактирование и удаление пользователей.
> 
> Этот учебник описывает шаги, которые были выполнены для создания примера приложения ASP.NET MVC 3 списка пользователей. Проект Visual Studio с исходным кодом C# и VB доступна по следующему адресу: [загрузить](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114). Если у вас есть вопросы о этого учебника, опубликуйте их [форум MVC](https://forums.asp.net/1146.aspx).


## <a name="overview"></a>Обзор

Приложение, которое вы будете построение имеет простой пользовательский список веб-сайта. Пользователи могут вводить, просматривать и обновлять сведения о пользователе.

![Пример узла](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

Завершенный проект VB и C# можно загрузить [здесь](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).

## <a name="creating-the-web-application"></a>Создание веб-приложения

Чтобы запустить учебник, откройте Visual Studio 2010 и создайте новый проект с помощью *веб-приложение ASP.NET MVC 3* шаблона. Назовите приложение &quot;Mvc3Razor&quot;.

[![Новый проект MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)

В **нового проекта ASP.NET MVC 3** диалогового окна выберите **веб-приложение**выберите представлений Razor и нажмите кнопку **ОК**.

![Диалоговое окно нового проекта ASP.NET MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

В этом учебнике будет не использоваться поставщик членства ASP.NET, можно удалить все файлы, связанные с входа и членства. В **обозревателе решений**, удалите следующие файлы и каталоги:

- *Controllers\AccountController*
- *Models\AccountModels*
- *Одну\\_LogOnPartial*
- *Views\Account* (и все файлы в этом каталоге)

![Soln Exp](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

Изменить  *\_Layout.cshtml* и замените разметку в `<div>` элемента с именем `logindisplay` с сообщением  *&quot;* входа отключено&quot;. В следующем примере показано новый разметку:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a>Добавление модели

В **обозревателе решений**, щелкните правой кнопкой мыши *моделей* выберите **добавить**и нажмите кнопку **класса**.

![Новый класс Mdl пользователя](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

Присвойте классу имя `UserModel`. Замените содержимое *UserModel* файла следующим кодом:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

`UserModel` Класс представляет пользователей. Каждый член класса помечается с помощью [необходимые](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.requiredattribute.aspx) атрибута из [DataAnnotations](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) пространства имен. Атрибуты в [DataAnnotations](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) пространства имен обеспечивают проверку автоматического клиента и стороне сервера для веб-приложений.

Откройте `HomeController` и добавьте `using` директив, так что можно получить доступ к `UserModel` и `Users` классы:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

Сразу после `HomeController` объявление, добавьте следующий комментарий и ссылку на `Users` класса:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

`Users` Класса является хранилищем данных упрощенной и в памяти, который будет использоваться в этом учебнике. В реальном приложении используется базы данных для хранения информации о пользователях. Первые несколько строк `HomeController` показаны в следующем примере:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

Постройте приложение, чтобы модель пользователя будут доступны в мастере формирования шаблонов в следующем шаге.

## <a name="creating-the-default-view"></a>Создание представления по умолчанию

Следующим шагом является добавление метода действия и представления для отображения пользователей.

Удалите существующую *Views\Home\Index* файла. Будет создана новая *индекс* файл для отображения пользователей.

В `HomeController` класса, замените содержимое `Index` метод следующим кодом:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

Щелкните правой кнопкой мыши внутри `Index` метода и нажмите кнопку **добавить представление**.

![Добавить представление](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

Выберите **создать строго типизированное представление** параметр. Для **просматривать данные класс**выберите **Mvc3Razor.Models.UserModel**. (Если вы не видите **Mvc3Razor.Models.UserModel** в **просматривать данные класс** поле, необходимое для создания проекта.) Убедитесь, что обработчик представлений присвоено **Razor**. Задать **просматривать содержимое** для **списка** и нажмите кнопку **добавить**.

![Добавить представление индекса](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

Новое представление автоматически scaffolds данные пользователя, передаваемое `Index` представления. Изучите вновь созданным *Views\Home\Index* файла. **Создать новый**, **изменить**, **сведения**, и **удалить** связи не работают, но функционирует остальной части страницы. Запустите страницу. Появится список пользователей.

![Страница индекса](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

Откройте *Index.cshtml* и замените `ActionLink` разметку для **изменить**, **сведения**, и **удалить** с помощью следующего кода :

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

Имя пользователя используется как идентификатор для найдете выбранную запись в **изменить**, **сведения**, и **удалить** ссылки.

## <a name="creating-the-details-view"></a>Создание представления сведений

Следующим шагом является добавление `Details` метода действия и представления для отображения сведений о пользователе.

![Подробные сведения](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

Добавьте следующие `Details` метод в контроллер home:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

Щелкните правой кнопкой мыши внутри `Details` метода, а затем выберите **добавить представление**. Убедитесь, что **просматривать данные класс** поле содержит **Mvc3Razor.Models.UserModel***.* Задать **просматривать содержимое** для **сведения** и нажмите кнопку **добавить**.

![Добавление представления сведений](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

Запустите приложение и выберите ссылку сведения. Автоматическое формирование шаблонов показана каждого свойства в модели.

![Подробные сведения](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a>Создание представления изменения

Добавьте следующие `Edit` метод в контроллер home.

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

Добавление представления, как в предыдущих шагах, но задать **просматривать содержимое** для **изменить**.

![Добавление представления изменения](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

Запустите приложение и измените имя и фамилия одного из пользователей. Если вы нарушены `DataAnnotation` ограничения, которые были применены к `UserModel` класса, при отправке формы, вы увидите ошибки проверки, созданные с помощью серверного кода. Например, если изменить имя &quot;Анна&quot; для &quot;A&quot;, при отправке формы, в форме отображается следующая ошибка:

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

В этом учебнике обработке имя пользователя в качестве первичного ключа. Таким образом невозможно изменить свойство имени пользователя. В *Edit.cshtml* файл сразу после `Html.BeginForm` инструкции, задайте имя пользователя в качестве скрытого поля. Это приводит к свойства должны быть переданы в модели. В следующем фрагменте кода показано размещение `Hidden` инструкции:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

Замените `TextBoxFor` и `ValidationMessageFor` разметки для имени пользователя с `DisplayFor` вызова. `DisplayFor` Метод свойство отображается как элемент только для чтения. В следующем примере полная разметка. Исходный `TextBoxFor` и `ValidationMessageFor` вызовы закомментированы с символы начала комментария и конец комментария Razor (`@* *@`)

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a>Включение проверки на стороне клиента

Чтобы включить проверку на стороне клиента в ASP.NET MVC 3, необходимо задать два флага и необходимо включить три файла JavaScript.

Откройте приложение *Web.config* файла. Проверьте `that ClientValidationEnabled` и `UnobtrusiveJavaScriptEnabled` присвоено значение true, если в параметрах приложения. В следующем фрагменте из корня *Web.config* файле отображаются правильные параметры:

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

Параметр `UnobtrusiveJavaScriptEnabled` значение true включает ненавязчивого Ajax и ненавязчивой клиентской проверки. При использовании ненавязчивой проверки правил проверки, преобразуются в атрибуты HTML5. Имена атрибутов HTML5 могут включать только строчные буквы, цифры и дефисы.

Параметр `ClientValidationEnabled` на true включает клиентскую проверку. Можно установить эти ключи в приложение *Web.config* файл включена клиентская проверка и ненавязчивый JavaScript для всего приложения. Кроме того, можно включить или отключить эти параметры в отдельных представлений или методы контроллера, используя следующий код:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

Необходимо также включить несколько файлов JavaScript в режиме отображения. Является простой способ включения JavaScript во всех представлениях, чтобы добавить их в *представления\общие\\_Layout.cshtml* файла. Замените `<head>` элемент  *\_Layout.cshtml* файла следующим кодом:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

Первые два скриптов jQuery размещаются с Microsoft Ajax доставки содержимого сети (CDN). Используя преимущества сети Microsoft Ajax CDN, может значительно повысить первого нажатия производительность приложений.

Запустите приложение и щелкните ссылку Изменить. Просмотрите исходный код страницы в браузере. Исходный браузера содержит множество атрибутов формы `data-val` (для проверки данных). Если включена клиентская проверка и ненавязчивый JavaScript, содержат поля ввода с помощью правила клиентской проверки `data-val="true"` атрибут для запуска ненавязчивой клиентской проверки. Например `City` декорированных поля в модели [необходимые](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.requiredattribute.aspx) атрибут, который приводит к HTML-код, показанный в следующем примере:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

Для каждого правила клиентской проверки добавить атрибут, который имеет форму `data-val-rulename="message"`. С помощью `City` поле пример, приведенный выше, правило требуется проверка клиента создает `data-val-required` атрибут и сообщение &quot;поле «Город» является обязательным&quot;. Запустите приложение, изменить один из пользователей, а затем снимите `City` поля. При нажатии клавиши tab поле, появится сообщение об ошибке проверки на стороне клиента.

![Требуется Город](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

Аналогичным образом, для каждого параметра в правила клиентской проверки, добавляется атрибут, имеет форму `data-val-rulename-paramname=paramvalue`. Например `FirstName` свойство помечается с помощью [StringLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) атрибут и указывает длину не менее 3 и максимальной длиной 8. Правила проверки данных с именем `length` имеет имя параметра `max` и значение параметра 8. В следующем примере показаны HTML, который создается для `FirstName` поля при изменении одного из пользователей:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

Дополнительные сведения о ненавязчивой клиентской проверки, см. в записи [Ненавязчивая клиентская проверка в ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) в блоге Брэда Уилсона.

> [!NOTE]
> В ASP.NET MVC 3 бета-версии иногда необходимо для отправки формы, чтобы запустить проверку на стороне клиента. Это может быть изменено в окончательной версии.


## <a name="creating-the-create-view"></a>Создание представления создания

Следующим шагом является добавление `Create` метода действия и представление, чтобы пользователь мог создать нового пользователя. Добавьте следующие `Create` метод в контроллер home:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

Добавление представления, как в предыдущих шагах, но задать **просматривать содержимое** для **создать**.

![Создать просмотр](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

Запустите приложение, выберите **создать** связать, и добавить нового пользователя. `Create` Метод автоматически использует преимущества проверки на стороне клиента и на стороне сервера. Введите имя пользователя, которое содержит пробел, таких как &quot;Бен X&quot;. При нажатии клавиши tab поля имени пользователя, ошибки проверки на стороне клиента (`White space is not allowed`) отображается.

## <a name="add-the-delete-method"></a>Добавьте метод Delete

Для прохождения этого учебника, добавьте следующий `Delete` метод в контроллер home:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

Добавить `Delete` представление, как показано выше, установка **просматривать содержимое** для **удалить**.

![Удаление представления](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

Теперь у вас есть простой, но полнофункциональные приложения ASP.NET MVC 3 с проверкой.
