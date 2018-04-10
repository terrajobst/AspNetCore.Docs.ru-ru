---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
title: Создание пользовательских AJAX элемента набора средств управления расширителем (C#) | Документы Microsoft
author: microsoft
description: Пользовательские расширения позволяют настроить и расширить возможности элементов управления ASP.NET без необходимости создавать новые классы.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 96b56eca-a892-45a4-96b4-67e61178650a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: dc058d1d19df880109352caf2dc7d1860121a104
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-custom-ajax-control-toolkit-control-extender-c"></a>Создание набора средств управления расширитель элемента управления пользовательского AJAX (C#)
====================
по [Microsoft](https://github.com/microsoft)

> Пользовательские расширения позволяют настроить и расширить возможности элементов управления ASP.NET без необходимости создавать новые классы.


В этом учебнике вы научитесь создавать расширитель пользовательских элементов управления AJAX-элемента управления. Мы создадим простой, но полезным, новый расширений, который изменяет состояние кнопки из отключенного состояния во включенное при вводе текста в текстовое поле. После прочтения этого учебника, можно расширить набор помощи собственных средств управления расширения AJAX ASP.NET.

Можно создать расширителей пользовательского элемента управления с помощью Visual Studio или Visual Web Developer (Убедитесь, что имеется последнюю версию Visual Web Developer).

## <a name="overview-of-the-disabledbutton-extender"></a>Общие сведения о модуле DisabledButton

Наш новый элемент управления расширителя называется DisabledButton расширителя. Этого расширителя будет иметь три свойства:

- TargetControlID - текстовое поле, которое расширяет элемент управления.
- TargetButtonIID - кнопки, отключен или включен.
- DisabledText - текст, который отображается на кнопке. При вводе, кнопки отображает значение свойства текст кнопки.

Можно подключить расширитель DisabledButton к элементу управления текстового поля и кнопку. Перед тем как любой текст, кнопка становится недоступной, и текстовое поле и кнопку выглядеть следующим образом:


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image1.png)

([Просмотр полноразмерное изображение](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image3.png))


После начала ввода текста, эта кнопка включена, и текстовое поле и кнопку выглядеть следующим образом:


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image4.png)

([Просмотр полноразмерное изображение](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image6.png))


Для создания нашей расширитель элемента управления, необходимо создать следующие три файла:

- DisabledButtonExtender.cs - этот файл является класса серверный элемент управления, который будет управлять Создание медиаприставки и позволяют задавать свойства во время разработки. Он также определяет свойства, которые могут быть установлены на вашего расширения. Эти свойства доступны через код, а также во время разработки и соответствуют свойствам, определенным в файле DisableButtonBehavior.js.
- DisabledButtonBehavior.js--Этот файл является добавляется вся логика сценария вашего клиента.
- DisabledButtonDesigner.cs - этот класс включает функциональные возможности времени разработки. Этот класс требуется в том случае, если требуется, чтобы элемент управления расширителя для правильной работы с Visual Studio или Visual Web Developer Designer.

Поэтому расширитель элемента управления состоит из серверный элемент управления, поведение клиентских и серверных конструктора класса. Вы узнаете, как создать все три из этих файлов в следующих разделах.

## <a name="creating-the-custom-extender-website-and-project"></a>Создание настраиваемого расширения веб-сайта и проект

Первым шагом является создание проекта библиотеки классов и веб-сайта в Visual Studio или Visual Web Developer. Мы ll создания пользовательских расширений в проекте библиотеки классов и тестирования пользовательских расширений в веб-сайта.

Разрешить s начинаться с веб-сайта. Выполните следующие действия для создания веб-сайта.

1. Выберите пункт меню в **файл, создать веб-сайт**.
2. Выберите **веб-сайт ASP.NET** шаблона.
3. Назовите новый веб-сайт *Website1*.
4. Нажмите кнопку **ОК** кнопки.

Далее необходимо создать проект библиотеки классов, который будет содержать код для управления расширителя:

1. Выберите пункт меню в **файл, добавить новый проект**.
2. Выберите **библиотеки классов** шаблона.
3. Назовите новую библиотеку классов с именем **CustomExtenders**.
4. Нажмите кнопку **ОК** кнопки.

После выполнения этих действий в окне обозревателя решений должна выглядеть как на рис. 1.


[![Решения с использованием проекта библиотеки веб-сайт и класса](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image7.png)

**На рисунке 01**: решение с проектом библиотеки веб-сайта и класс ([щелкните, чтобы просмотреть полноразмерное изображение](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image9.png))


Далее необходимо добавить все ссылки на необходимые сборки в проект библиотеки классов:

1. Щелкните правой кнопкой мыши проект CustomExtenders и выберите пункт меню в **добавить ссылку**.
2. Перейдите на вкладку .NET.
3. Добавьте ссылки на следующие сборки:

    1. System.Web.dll.
    2. System.Web.Extensions.dll.
    3. System.Design.dll
    4. System.Web.Extensions.Design.dll
4. Выберите вкладку "Обзор".
5. Добавьте ссылку на файл AjaxControlToolkit.dll сборку. Эта сборка находится в папке, куда вы загрузили элементов управления AJAX.

После выполнения этих шагов папка ссылок проекта библиотеки классов должна выглядеть как на рис. 2.


[![Ссылки на папку с необходимые ссылки](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image10.png)

**Рисунок 02**: папка ссылок с необходимые ссылки ([Просмотр полноразмерное изображение](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image12.png))


## <a name="creating-the-custom-control-extender"></a>Создание пользовательского элемента управления расширителем

Теперь, когда у нас есть нашей библиотеки классов, мы можно приступать к созданию элемента-расширителя. Разрешить s начинаться с костей bare класса элемента управления пользовательские расширения (см. список 1).

**Листинг 1 - MyCustomExtender.cs**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample1.cs)]

Существует несколько вещей, которые можно заметить о класс элемента управления расширителем в список 1. Во-первых Обратите внимание, что класс наследует от базового класса ExtenderControlBase. Все элементы управления расширителем элементов управления AJAX являются производными от этого базового класса. Например базовый класс содержит TargetID свойство, которое является обязательным свойством расширитель каждого элемента управления.

Далее Обратите внимание, что класс включает следующие два атрибута, связанные с клиентского скрипта:

- В результате WebResource - файл в качестве внедренного ресурса в сборку.
- В результате ClientScriptResource - сценария ресурса должно быть извлечено из сборки.

Указав используется для внедрения в сборку файла MyControlBehavior.js JavaScript, при компиляции пользовательских расширений. Атрибут ClientScriptResource используется для получения сценария MyControlBehavior.js из сборки при использовании пользовательских расширений в веб-страницы.


Чтобы WebResource и ClientScriptResource атрибуты для работы необходимо скомпилировать файл JavaScript в качестве внедренного ресурса. В окне обозревателя решений выберите файл, откройте окно свойств и присвоить значение *внедренный ресурс* для **действие при построении** свойство.


Обратите внимание, что расширитель элемента управления также включает атрибут TargetControlType. Этот атрибут используется для указания типа элемента управления, который расширяется за счет управления расширителя. В случае список 1 расширитель элемента управления используется для расширения элемента управления TextBox.

И, наконец Обратите внимание, что пользовательские расширения содержит свойство с именем MyProperty. Свойство помечено атрибутом ExtenderControlProperty. Методы GetPropertyValue() и SetPropertyValue() используются для передачи значения свойства из серверного элемента управления расширителем поведение клиентского.

Разрешить s пойти дальше и реализация кода для наших DisabledButton расширителя. Код для данного объекта расширения можно найти в списке 2.

**Листинг 2 - DisabledButtonExtender.cs**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample2.cs)]

Расширитель DisabledButton в списке 2 имеет два свойства с именем TargetButtonID и DisabledText. IDReferenceProperty, применяемой к свойству TargetButtonID предотвращает назначение ничего, кроме идентификатора элемента управления Button для этого свойства.

Атрибуты WebResource и ClientScriptResource связать клиентского поведения, расположенных в файле с именем DisabledButtonBehavior.js с этого расширителя. Мы рассмотрим этот файл JavaScript в следующем разделе.

## <a name="creating-the-custom-extender-behavior"></a>Создание пользовательских расширений поведения

Компонент клиентских расширений управления называется поведение. В поведении DisabledButton содержится фактической логики для отключения и включения кнопки. В списке 3 включается код JavaScript для поведения.

**Листинг 3 - DisabledButton.js**

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample3.js)]

Файл JavaScript в списке 3 содержит клиентский класс с именем DisabledButtonBehavior. Этот класс, как его две серверные включает два свойства с именем TargetButtonID и получить к нему можно получить с помощью DisabledText\_TargetButtonID/set\_TargetButtonID и получить\_DisabledText/set\_ DisabledText.

Метод initialize() связывает с обработчиком событий keyup с целевой элемент поведения. Каждый раз, введите букву в текстовое поле, связанное с этим поведением keyup обработчик выполняет. Обработчик keyup включает или отключает кнопку в зависимости от того, содержит ли текстовое поле, связанных с данным поведением любой текст.

Помните, что необходимо скомпилировать файл JavaScript в списке 3 в качестве внедренного ресурса. В окне обозревателя решений выберите файл, откройте окно свойств и присвоить значение *внедренный ресурс* для **действие при построении** свойство (см. рис. 3). Этот параметр доступен в Visual Studio и Visual Web Developer.


[![Добавление файла JavaScript в качестве внедренного ресурса](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image13.png)

**На рисунке 03**: Добавление файла JavaScript в качестве внедренного ресурса ([Просмотр полноразмерное изображение](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image15.png))


## <a name="creating-the-custom-extender-designer"></a>Создание пользовательского расширяющего конструктора

Имеется один последний класс, необходимо создать для завершения нашей расширителя. Необходимо создать класс конструктора в листинге 4. Этот класс обязательно расширителя правильно работать с Visual Studio или Visual Web Developer Designer.

**Листинг 4 - DisabledButtonDesigner.cs**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample4.cs)]

Связать конструктора в листинге 4 с DisabledButton расширитель с помощью атрибута конструктора. Необходимо применить атрибут конструктор в класс DisabledButtonExtender следующим образом:

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample5.cs)]

## <a name="using-the-custom-extender"></a>С помощью пользовательских расширений

Теперь, когда мы завершения создания расширения элемента управления DisabledButton, пришло время для их использования в наш веб-узла ASP.NET. Во-первых необходимо добавить настраиваемые расширения в область элементов. Выполните следующие действия.

1. Откройте страницу ASP.NET, дважды щелкнув страницы в окне обозревателя решений.
2. Щелкните правой кнопкой мыши область элементов и выберите пункт меню в **Выбор элементов**.
3. В диалоговом окне Выбор элементов панели элементов перейдите к сборке CustomExtenders.dll.
4. Нажмите кнопку **ОК** кнопку, чтобы закрыть диалоговое окно.

После выполнения этих шагов DisabledButton управления расширителя должен отображаться в панели элементов (см. рис. 4).


[![DisabledButton на панели инструментов](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image16.png)

**На рисунке 04**: DisabledButton на панели инструментов ([Просмотр полноразмерное изображение](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image18.png))


Теперь нам нужны для создания новой страницы ASP.NET. Выполните следующие действия.

1. Создайте новую страницу ASP.NET с именем ShowDisabledButton.aspx.
2. Перетащите наличия ScriptManager на странице.
3. Перетащите элемент управления TextBox на страницу.
4. Перетащите элемент управления Button на страницу.
5. В окне свойств измените значение свойства Button ID <em>btnSave</em> и свойству Text значение *Сохранить\**.
  

Мы создали страницу стандартных элементов управления ASP.NET текстовое поле и кнопку.

Далее нам нужно расширить элемент управления TextBox со DisabledButton расширителя:

1. Выберите **Добавить расширитель** задач, чтобы открыть диалоговое окно мастера расширения (см. рис. 5). Обратите внимание, что у диалогового окна есть наших пользовательских расширений DisabledButton.
2. Выберите расширитель DisabledButton и нажмите кнопку **ОК** кнопки.


[![Диалоговое окно мастера расширения](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image19.png)

**На рисунке 05**: диалоговое окно мастера расширителя ([Просмотр полноразмерное изображение](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image21.png))


Наконец мы можно задать свойства DisabledButton расширителя. Можно изменить свойства расширителя DisabledButton путем изменения свойств элемента управления TextBox:

1. Выберите текстовое поле в конструкторе.
2. В окне «Свойства» разверните узел расширения (см. рис. 6).
3. Присвойте значение *Сохранить* DisabledText свойства и значения *btnSave* TargetButtonID свойству.


[![Настройка свойств расширителя](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image22.png)

**На рисунке 06**: задание свойств расширителя ([Просмотр полноразмерное изображение](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image24.png))


При выполнении страницы (с нажимать клавишу F5), изначально отключен элемент управления Button. Сразу запустить, введя текст в текстовое поле, элемент управления является кнопка (см. рис. 7).


[![Расширитель DisabledButton в действии](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image25.png)

**На рисунке 07**: DisabledButton расширений в действии ([Просмотр полноразмерное изображение](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image27.png))


## <a name="summary"></a>Сводка

Целью данного учебника было объясняется, как можно расширить набор элементов управления AJAX с помощью пользовательских расширений элементов управления. В этом учебнике мы создали простого расширения элемента управления DisabledButton. Мы реализовали этого расширителя путем создания класса DisabledButtonExtender, поведение DisabledButtonBehavior JavaScript и класс DisabledButtonDesigner. Выполните аналогичный набор шагов, каждый раз при создании пользовательского элемента управления расширителем.

> [!div class="step-by-step"]
> [Назад](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
> [Вперед](get-started-with-the-ajax-control-toolkit-vb.md)
