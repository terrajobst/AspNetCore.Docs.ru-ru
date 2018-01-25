---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: "Вызов веб-API из Windows Phone 8 приложения (C#) | Документы Microsoft"
author: rmcmurray
description: "Создание полного сценария начала до конца, состоящий из приложения веб-API ASP.NET, которое содержит каталог книг для приложения Windows Phone 8."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/09/2013
ms.topic: article
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: 6e5a936decb27fd2e3b8cdcea44db8db822c98eb
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="calling-web-api-from-a-windows-phone-8-application-c"></a>Вызов веб-API из приложения Windows Phone 8 (C#)
====================
по [Robert McMurray](https://github.com/rmcmurray)

В этом учебнике вы узнаете, как создать полного сценария начала до конца, состоящий из приложения веб-API ASP.NET, которое содержит каталог книг для приложения Windows Phone 8.

### <a name="overview"></a>Обзор

RESTful служб, таких как веб-API ASP.NET упрощают создание приложений на основе HTTP для разработчиков посредством абстрагирования архитектуры для серверных и клиентских приложений. Вместо создания связи собственный протокол на основе сокетов, разработчиков веб-API просто должен быть опубликован необходимые методы HTTP для своего приложения (например: GET, POST, PUT, DELETE), и только нужно использовать разработчики клиентских приложений методы HTTP, которые необходимы для своего приложения.

В этом учебнике начала до конца вы узнаете, как использовать веб-API для создания следующих проектов:

- В [первой части этого учебника](#STEP1), вы создадите приложение веб-API ASP.NET, которое поддерживает все операции создания, чтения, обновления и удаления (CRUD) для управления каталогом книги. Это приложение будет использовать [пример XML-файла (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) с MSDN.
- В [второй части этого учебника](#STEP2), вы создадите интерактивным приложением Windows Phone 8, извлекает данные из приложения веб-API.

#### <a name="prerequisites"></a>Предварительные требования

- Visual Studio 2013 с Windows Phone 8 установленный пакет SDK
- Windows 8 или более поздней версии на 64-разрядной системы с Hyper-V установлено
- Список дополнительных требованиях см. в разделе *требования к системе для* статьи на [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) странице загрузки.

> [!NOTE]
> Если проверка подключения между веб-API и проектов Windows Phone 8 в локальной системе, необходимо будет следуйте инструкциям в  *[подключение к веб-API приложения на локальном эмуляторе Windows Phone 8 Компьютер](https://go.microsoft.com/fwlink/?LinkId=324014)*  статьи для настройки тестовой среды.


<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a>Шаг 1: Создание веб-проекта API Bookstore

Первым шагом данного руководства конца в конец является создание проекта веб-API, который поддерживает все операции CRUD; Обратите внимание, что вы добавите проект приложения Windows Phone в это решение в [шаг 2](#STEP2) этого учебника.

1. Откройте **Visual Studio 2013**.
2. Нажмите кнопку **файл**, затем **новый**, а затем **проекта**.
3. При **новый проект** диалоговое окно отображается, разверните **установленные**, затем **шаблоны**, затем **Visual C#**, а затем **Web**.

    | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
    | --- |
    | Щелкните изображение, чтобы развернуть |
4. Выделите **веб-приложение ASP.NET**, введите **BookStore** для имени проекта и нажмите кнопку **ОК**.
5. Когда **новый проект ASP.NET** отображается диалоговое окно, выберите пункт **веб-API** шаблона и нажмите кнопку **ОК**.

    | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
    | --- |
    | Щелкните изображение, чтобы развернуть |
6. При открытии проекта веб-API, удалите из проекта пример контроллера:

    1. Разверните **контроллеров** папки в обозревателе решений.
    2. Щелкните правой кнопкой мыши **ValuesController.cs** файла и нажмите кнопку **удалить**.
    3. Нажмите кнопку **ОК** при появлении запроса на подтверждение удаления.
7. Добавьте файл XML-данных в проект веб-API; Этот файл содержит содержимое каталога bookstore.

    1. Щелкните правой кнопкой мыши **приложения\_данные** папки в обозревателе решений щелкните **добавить**и нажмите кнопку **новый элемент**.
    2. Когда **Добавление нового элемента** диалоговое окно, выделите **XML-файл** шаблона.
    3. Назовите файл **Books.xml**, а затем нажмите кнопку **добавить**.
    4. Когда **Books.xml** открыть файл, замените код в файле XML-кодом из примера **books.xml** файла на сайте MSDN: 

        [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
    5. Сохраните и закройте XML-файл.
8. Добавить модель bookstore проект веб-API; Эта модель содержит логику для приложение bookstore создания, чтения, обновления и удаления (CRUD):

    1. Щелкните правой кнопкой мыши **моделей** папки в обозревателе решений щелкните **добавить**и нажмите кнопку **класса**.
    2. При **Добавление нового элемента** диалоговое окно, введите имя файла класса **BookDetails.cs**, а затем нажмите кнопку **добавить**.
    3. Когда **BookDetails.cs** открыть файл, замените код в файле следующим: 

        [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
    4. Сохраните и закройте **BookDetails.cs** файла.
9. Добавьте в проект веб-API bookstore контроллера:

    1. Щелкните правой кнопкой мыши **контроллеров** папки в обозревателе решений щелкните **добавить**и нажмите кнопку **контроллера**.
    2. При **Добавление формирования шаблонов** диалоговое окно, выделите **контроллер 2 API веб - пустой**, а затем нажмите кнопку **добавить**.
    3. При **добавления контроллера** диалоговое окно, введите имя контроллера **BooksController**, а затем нажмите кнопку **добавить**.
    4. Когда **BooksController.cs** открыть файл, замените код в файле следующим: 

        [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
    5. Сохраните и закройте **BooksController.cs** файла.
10. Постройте приложение веб-API для проверки ошибок.

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a>Шаг 2: Добавление проекта Windows Phone 8 Bookstore каталога

Этот сценарий конца в конец Далее необходимо для создания каталога приложения для Windows Phone 8. Это приложение будет использовать *с привязкой к данным приложения Windows Phone* шаблон для пользовательского интерфейса по умолчанию, который будет использовать веб-API приложения, созданный в [шаг 1](#STEP1) этого учебника в качестве источника данных.

1. Щелкните правой кнопкой мыши **BookStore** в решение в обозревателе решений, а затем нажмите **добавить**, а затем **новый проект**.
2. При **новый проект** диалоговое окно отображается, разверните **установленные**, затем **Visual C#**, а затем **Windows Phone**.
3. Выделите **с привязкой к данным приложения Windows Phone**, введите **BookCatalog** для имени и затем **ОК**.
4. Добавьте в пакет Json.NET NuGet **BookCatalog** проекта:

    1. Щелкните правой кнопкой мыши **ссылки** для **BookCatalog** проекта в обозревателе решений и выберите **управление пакетами NuGet**.
    2. Когда **управление пакетами NuGet** диалоговое окно отображается, разверните **Online** раздела и выделите **nuget.org**.
    3. Введите **Json.NET** в поиск поля и щелкните значок поиска.
    4. Выделите **Json.NET** в результаты поиска, а затем выберите **установить**.
    5. После завершения установки, нажмите кнопку **закрыть**.
5. Добавить **BookDetails** модели **BookCatalog** проекта; содержит общую модель bookstore класса:

    1. Щелкните правой кнопкой мыши **BookCatalog** проекта в обозревателе решений, а затем нажмите кнопку **добавить**, а затем нажмите кнопку **новую папку**.
    2. Имя новой папки **моделей**.
    3. Щелкните правой кнопкой мыши **моделей** папки в обозревателе решений щелкните **добавить**и нажмите кнопку **класса**.
    4. При **Добавление нового элемента** диалоговое окно, введите имя файла класса **BookDetails.cs**, а затем нажмите кнопку **добавить**.
    5. Когда **BookDetails.cs** открыть файл, замените код в файле следующим: 

        [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
    6. Сохраните и закройте **BookDetails.cs** файла.
6. Обновление **MainViewModel.cs** класса для включения функции для взаимодействия с приложением BookStore веб-API:

    1. Разверните **ViewModels** папки в обозревателе решений, а затем дважды щелкните **MainViewModel.cs** файла.
    2. Когда **MainViewModel.cs** файл открыт, замените код в файле следующим; Обратите внимание, что необходимо обновить значение `apiUrl` постоянное фактический URL-адрес веб-API: 

        [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
    3. Сохраните и закройте **MainViewModel.cs** файла.
7. Обновление **MainPage.xaml** файл, чтобы настроить имя приложения:

    1. Дважды щелкните **MainPage.xaml** файл в обозревателе решений.
    2. Когда **MainPage.xaml** файл открыт, найдите следующие строки кода: 

        [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
    3. Замените эти строки со следующими: 

        [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
    4. Сохраните и закройте **MainPage.xaml** файла.
8. Обновление **DetailsPage.xaml** файл, чтобы настроить отображаемые элементы:

    1. Дважды щелкните **DetailsPage.xaml** файл в обозревателе решений.
    2. Когда **DetailsPage.xaml** файл открыт, найдите следующие строки кода: 

        [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
    3. Замените эти строки со следующими: 

        [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
    4. Сохраните и закройте **DetailsPage.xaml** файла.
9. Создание приложения Windows Phone, проверка на наличие ошибок.

### <a name="step-3-testing-the-end-to-end-solution"></a>Шаг 3: Тестирование решения конца в конец

Как упоминалось в *необходимые компоненты* проекты разделе этого учебника, при тестировании подключения между веб-API и Windows Phone 8 в локальной системе, вы должны будете следовать инструкциям в разделе  *[ Подключение к веб-API приложения на локальном компьютере в эмуляторе Windows Phone 8](https://go.microsoft.com/fwlink/?LinkId=324014)*  статьи для настройки тестовой среды.

После настройки среды тестирования, необходимо установить как запускаемый проект приложения Windows Phone. Чтобы сделать это, выделите **BookCatalog** приложения в обозревателе решений, а затем выберите **Назначить запускаемым проектом**:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| Щелкните изображение, чтобы развернуть |

При нажатии клавиши F5, Visual Studio запускает оба Windows Phone эмулятор, который будет отображать &quot;Подождите&quot; сообщения при получении данных приложения из веб-API:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| Щелкните изображение, чтобы развернуть |

Если все успешно, вы увидите каталог отображается:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| Щелкните изображение, чтобы развернуть |

Если коснуться любой название книги, в приложении отображается описание книги:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| Щелкните изображение, чтобы развернуть |

Если приложение не может взаимодействовать с веб-API, отображается сообщение об ошибке:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| Щелкните изображение, чтобы развернуть |

Если коснуться сообщение об ошибке, будет отображаться дополнительная информация об ошибке:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
| --- |
| Щелкните изображение, чтобы развернуть |
