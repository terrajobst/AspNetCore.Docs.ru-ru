---
uid: web-api/samples-list
title: "Веб-API образцы списка | Документы Microsoft"
author: rick-anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/18/2012
ms.topic: article
ms.assetid: 8cbd9d7f-7027-4390-b098-cb81a63ecd6f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/samples-list
msc.type: content
ms.openlocfilehash: 2f40cd4bebdd64c3a4b94cfc1e717fa4b304e57e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="web-api-samples-list"></a>Список примеров веб-API
====================
## <a name="httpclient-samples"></a>Примеров HttpClient

**Пример преобразования Bing** | [источника Visual STUDIO 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fBingTranslateSample%2fReadMe.txt)

Показано, как вызвать [службы Microsoft Translator](https://msdn.microsoft.com/en-us/library/ff512419.aspx) с помощью **HttpClient** класса. API службы Microsoft Translator требует токена OAuth, который получает приложение, отправляя запрос к серверу Azure маркера для каждого запроса к службе преобразователя. Результат на сервере маркера передавались в запрос, отправляемый службе перевода. Перед запуском этого образца необходимо получить [ключ приложения из Azure Marketplace](https://msdn.microsoft.com/en-us/library/hh454950.aspx) и введите сведения в AccessTokenMessageHandler пример класса.

**Пример сопоставления Google** | [подробное описание](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/downloading-a-google-map-to-local-file.aspx) | [источника Visual STUDIO 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fGoogleMapsSample%2fReadMe.txt)

Использует **HttpClient** загрузить карту Redmond, WA из [Google Maps API](https://developers.google.com/maps/), сохраняет его как локальный файл и открывает средство просмотра изображений по умолчанию.

**Пример клиента Twitter** | [подробное описание](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/extending-httpclient-with-oauth-to-access-twitter.aspx) | [источника Visual STUDIO 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fTwitterSample%2fReadMe.txt)

Показывает способы написания простого клиента Twitter с помощью **HttpClient**. В этом образце используется **HttpMessageHandler** для вставки данных проверки подлинности OAuth в исходящий **HttpRequestMessage**. Результат из Twitter считывается с помощью JSON.NET. Перед запуском этого образца необходимо получить [ключ приложения из Twitter](https://dev.twitter.com/)и введите сведения в OAuthMessageHandler пример класса.

**Образец банком** | [подробное описание](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/httpclient-is-here.aspx) | [источника VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fHttpClient%2fWorldBankSample%2fReadMe.txt) | [источника Visual STUDIO 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fWorldBankSample%2fReadMe.txt)

Показывает способ получения данных с банком данных сайта, с помощью JSON.NET анализируемый результат.

## <a name="web-api-samples"></a>Образцы веб-API

**Приступая к работе с веб-API ASP.NET** | [источника Visual STUDIO 2012](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)

Показано, как создать базовый веб-API, который поддерживает запросы HTTP GET. Содержит исходный код для учебника [свой первый веб-API ASP.NET](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).

**Сценарии ASP.NET Web API JavaScript — комментарии** | [источника Visual STUDIO 2012](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7)

Показано, как использовать веб-API ASP.NET для создания веб-API, которые поддерживают клиентов браузера и можно легко вызывать с помощью jQuery.

**Диспетчер контактов** | [источника VS 2010](https://code.msdn.microsoft.com/Contact-Manager-Web-API-0e8e373d)

В этом примере используется веб-API ASP.NET для построения приложения простой диспетчера контактов. Приложение состоит из веб-службу диспетчера API, используемый приложением ASP.NET MVC и приложения Windows Phone для отображения и управления списка контактов.

**Пакетная обработка образец** | [подробное описание](http://trocolate.wordpress.com/2012/07/19/mitigate-issue-260-in-batching-scenario/) | [источника Visual STUDIO 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fHostedBatchSample%2fReadMe.txt)

Показан способ реализации пакетной обработки HTTP в ASP.NET. Пакетирование состоит из размещение нескольких HTTP-запросов в один текст составных сущностей MIME, который затем отправляется на сервер как HTTP POST. Запросы обрабатываются по отдельности, а ответы помещаются в другой текст составных сущностей MIME, который возвращается клиенту.

**Пример контроллера содержимого** | [подробное описание](https://blogs.msdn.com/b/henrikn/archive/2012/02/24/async-actions-in-asp-net-web-api.aspx) | [источника VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fContentControllerSample%2fReadMe.txt) | [источника Visual STUDIO 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fContentControllerSample%2fReadMe.txt)

Показано, как считывать и записывать сущности запроса и ответа асинхронно с помощью потоков. Пример контроллера имеет два действия: действия PUT, который асинхронно считывает тело сущности запроса и сохраняет его в локальный файл и GET действие, которое возвращает содержимое локального файла.

**Пользовательский Сопоставитель образец сборки** | [источника Visual STUDIO 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fCustomAssemblyResolverSample%2fReadMe.txt)

Показано, как изменить веб-API ASP.NET для поддержки обнаружения контроллеров из сборки динамически загружаемые библиотеки. В этом образце реализуется пользовательский **IAssembliesResolver** которого вызывает реализацию по умолчанию, а затем добавляет сборку библиотеки по умолчанию.

**Пользовательский пример форматирования типа мультимедиа** | [подробное описание](https://blogs.msdn.com/b/henrikn/archive/2012/04/23/using-cookies-with-asp-net-web-api.aspx) | [источника VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fCustomMediaTypeFormatterSample%2fReadMe.txt)

Демонстрируется создание настраиваемых тип модуля форматирования, используя **BufferedMediaTypeFormatter** базового класса. Этот базовый класс предназначен для форматирования данных, которые в основном используйте синхронного ввода и операций записи. Помимо отображение модули форматирования типа мультимедиа, образце показано, как подключить его, зарегистрировав его как часть **HttpConfiguration** для вашего приложения. Обратите внимание, что также можно использовать **MediaTypeFormatter** напрямую, базовый класс для модулей форматирования, которые в основном используют асинхронных операций чтения и записи.

**Образец пользовательского параметра привязки** | [подробное описание](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx) | [источника VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fCustomParameterBinding%2fReadMe.txt)

Показано, как настроить процесс привязки параметра — процесс, который определяет, как данные из запроса будут привязаны к параметров действий. В этом образце контроллера Home имеет четыре действия:

1. BindPrincipal показано, как привязать параметр IPrincipal из пользовательских универсального участника не из сообщения HTTP GET;
2. BindCustomComplexTypeFromUriOrBody показано, как выполнить привязку параметра сложного типа, который могут поступать из тела сообщения или из запроса URI сообщение HTTP POST;
3. BindCustomComplexTypeFromUriWithRenamedProperty показано, как выполнить привязку параметра сложного типа с переименованные свойства, которое поступает из запроса URI сообщение HTTP POST;
4. PostMultipleParametersFromBody показано, как привязать несколько параметров из тела сообщения POST;

**Загрузить образец файла** | [подробное описание](https://blogs.msdn.com/b/henrikn/archive/2012/03/01/file-upload-and-asp-net-web-api.aspx) | [источника Visual STUDIO 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fFileUploadSample%2fReadMe.txt)

Показано, как отправить файлы **ApiController** с помощью отправки файлов MIME Multipart и как настроить уведомления о ходе выполнения с **HttpClient** с помощью **ProgressNotificationHandler**. Контроллер асинхронно считывает содержимое передачи файла HTML и записывает один или несколько частей текста в локальный файл. Ответ содержит сведения о загруженного файла (или файлов).

**Отправка в образце хранилища больших двоичных объектов Azure файла** | [подробное описание](https://blogs.msdn.com/b/yaohuang1/archive/2012/07/02/asp-net-web-api-and-azure-blob-storage.aspx) | [источника Visual STUDIO 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/61dfed023e50#Samples%2fNet45%2fCS%2fWebApi%2fAzureBlobsFileUploadSample%2fReadMe.txt)

Этот пример аналогичен образец отправки файла, но вместо сохранение переданные файлы на локальном диске, он асинхронно передает файлы в [хранилища больших двоичных объектов Azure](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs) с помощью [Windows Azure SDK для .NET](https://www.windowsazure.com/en-us/develop/net/). Он также предоставляет механизм для перечисления больших двоичных объектов, находящихся в данный момент в [контейнера хранилища больших двоичных объектов Azure](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Попробуйте выполнить пример, запущенным по отношению к **эмулятор хранилища Azure** , который поставляется вместе с Azure SDK. Если у вас есть [учетной записи хранилища Azure](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs), можно выполнять на службу действительное хранилище.

**Пример конвейера обработчик сообщений HTTP** | [подробное описание](https://blogs.msdn.com/b/henrikn/archive/2012/08/07/httpclient-httpclienthandler-and-httpwebrequesthandler.aspx) | [источника VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fHttpMessageHandlerPipelineSample%2fReadMe.txt)

Показано, как подключить **HttpMessageHandler** экземпляров на клиенте (**HttpClient**) и сервера (веб-API ASP.NET). В этом образце тот же обработчик используется на клиенте и сервере. Хотя маловероятно, что точное же обработчик будет выполняться в обоих местах, объектной модели одинаково на стороне клиента и сервера.

**Пример отправки JSON** | [источника Visual STUDIO 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fJsonUploadSample%2fReadMe.txt)

Показано, как отправлять и загружать JSON с **ApiController**. В этом образце используется минимальным **ApiController** и осуществляет доступ к нему с помощью **HttpClient**.

**Пример гибридного** | [подробное описание](https://blogs.msdn.com/b/henrikn/archive/2012/03/03/async-mashups-using-asp-net-web-api.aspx) | [источника Visual STUDIO 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fMashupSample%2fReadMe.txt)

Показано, как получить доступ к нескольким удаленных сайтов асинхронно в **ApiController** действия. При каждом попадании действие запросы выполняются асинхронно, чтобы потоки не блокируются.

**Образец трассировки памяти** | [подробное описание](https://blogs.msdn.com/b/roncain/archive/2012/04/12/tracing-in-asp-net-web-api.aspx) | [источника VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fMemoryTracingSample%2fReadMe.txt)

Данный пример создает пакета Nuget, который устанавливает записи отслеживания в памяти в приложениях ASP.NET Web API.

**Образец MongoDB** | [подробное описание](https://blogs.msdn.com/b/henrikn/archive/2012/02/19/using-web-api-with-mongodb.aspx) | [источника Visual STUDIO 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fMongoSample%2fReadMe.txt)

Показано, как использовать в качестве постоянного хранилища для MongoDB **ApiController**, используя шаблон репозитория.

**Образец процессора текст ответа** | [источника Visual STUDIO 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fResponseEntityProcessorSample%2fReadMe.txt)

Показано, как скопировать объект ответа (то есть, тело ответа HTTP) в локальный файл, перед их передачей клиенту и дополнительной обработки для этого файла асинхронно. В этом образце реализуется **HttpMessageHandler** которая упаковывает объект ответа с одним, что оба записывает сам в выходные данные в обычном режиме и в локальный файл.

**Загрузить пример XDocument** | [подробное описание](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/push-and-pull-streams-using-httpclient.aspx) | [источника Visual STUDIO 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fUploadXDocumentSample%2fReadMe.txt)

Показано, как отправить XDocument для **ApiController** с помощью **PushStreamContent** и **HttpClient**.

**Пример проверки** | [источника VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fValidationSample%2fReadMe.txt)

Показывает, как можно использовать атрибуты проверки моделей в ASP.NET WebAPI для проверки содержимого HTTP-запроса. Демонстрирует пометить свойства как обязательный, как использовать оба определяемые платформой и настраиваемых атрибутов проверки для создания заметок к модели и возвращает сообщения об ошибках для состояний Недопустимая модель.

**Веб-формы образец** | [подробное описание](https://blogs.msdn.com/b/henrikn/archive/2012/02/23/using-asp-net-web-api-with-asp-net-web-forms.aspx) | [источника VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fWebFormSample%2fReadMe.txt)

Показывает ApiController добавлены в проект веб-форм.

**[Образец RestBugs](https://github.com/howarddierking/RestBugs)**

RestBugs — это простая ошибка, приложение, который показывает, как использовать веб-API ASP.NET и в новой библиотеке HTTP-клиента для создания управляемых гипермедиа системы отслеживания. Образец включает реализации клиента и сервера, с помощью веб-API ASP.NET. Сервер использует пользовательский модуль форматирования Razor для создания представления ресурсов. В примере также показан сервер node.js, чтобы продемонстрировать преимущества, полученные с помощью конструктора гипермедиа отделять клиентами и серверами.

## <a name="web-api-extensions-preview-samples"></a>Примеры предварительной версии расширений веб-API

**Образец поддерживает запросы OData** | [подробное описание](https://blogs.msdn.com/b/alexj/archive/2012/08/15/odata-support-in-asp-net-web-api.aspx) | [источника VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fODataQueryableSample%2fReadMe.txt)

Показано, как вызвать запросов OData в веб-API ASP.NET с помощью `[Queryable]` атрибут или с помощью **ODataQueryOptions** действие параметра, который позволяет проверить запрос вручную перед его выполнением действие.

CustomerController показано использование атрибута [Queryable] и OrderController показано, как использовать параметр ODataQueryOptions. Аналогично ResponseController CustomerController, но вместо возвращения действия GET `IEnumerable<Customer>` возвращается **HttpResponseMessage**. Это позволяет добавить дополнительный заголовок поля, управлять код состояния, т. д., то же время использовать функциональность запроса. В образце показаны запросы с использованием $orderby, $skip, $top, any(), all() и $filter.

**Образец службы OData** | [подробное описание](https://blogs.msdn.com/b/alexj/archive/2012/08/15/odata-support-in-asp-net-web-api.aspx) | [источника VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fODataServiceSample%2fReadMe.txt)

В этом примере показано, как создать службу OData, состоящий из трех сущностей и три контроллера веб-API. Контроллеры обеспечивают различные уровни функциональных возможностей с точки зрения функциональности OData, предоставляемых ими:

SupplierController предоставляет подмножество функций, включая запрос, получить или создать, путем обработки этих запросов:

- ПОЛУЧИТЬ /Suppliers
- ПОЛУЧИТЬ /Suppliers(key)
- GET /Suppliers? $filter =... &amp;$orderby =... &amp;$top =... &amp;$skip =...
- POST /Suppliers

Предоставляет ProductsController GET, PUT, POST, удаление и ИСПРАВИТЬ путем реализации действия для каждой из этих операций непосредственно.

ProductFamilesController использует EntitySetController базового класса, который предоставляет полезные шаблона для реализации широкие возможности службы OData.

Кроме того служба OData предоставляет документ $metadata, который позволяет данные, используемые клиентами службы данных WCF и других клиентов, которые распознает формат $metadata.
