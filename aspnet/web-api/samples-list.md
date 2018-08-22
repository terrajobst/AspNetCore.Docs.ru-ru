---
uid: web-api/samples-list
title: Список примеров веб-API | Документация Майкрософт
author: rick-anderson
description: ''
ms.author: riande
ms.date: 09/18/2012
ms.assetid: 8cbd9d7f-7027-4390-b098-cb81a63ecd6f
msc.legacyurl: /web-api/samples-list
msc.type: content
ms.openlocfilehash: d25e0890a1b8d42cc638117f7bef9cf6457f3d75
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41839066"
---
<a name="web-api-samples-list"></a>Список примеров веб-API
====================
## <a name="httpclient-samples"></a>Примеров HttpClient

**Перевод примера приложения Bing** | [источника Visual STUDIO 2012](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/BingTranslateSample)

Показан способ вызова [службы Microsoft Translator](https://msdn.microsoft.com/library/ff512419.aspx) с помощью **HttpClient** класса. API службы Microsoft Translator требуется токен OAuth, который приложение получает путем отправки запроса к серверу Azure маркера для каждого запроса в службу translator. В результате сервер маркера подается на запрос, отправленный в службу перевода. Перед запуском этого образца необходимо получить [ключ приложения из Azure Marketplace](https://msdn.microsoft.com/library/hh454950.aspx) и заполните поля в классе образцов AccessTokenMessageHandler.

**Пример приложения Google Maps** | [подробное описание](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/downloading-a-google-map-to-local-file.aspx) | [источника Visual STUDIO 2012](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/GoogleMapsSample)

Использует **HttpClient** загрузить карту Редмонд, штат Вашингтон из [Google Maps API](https://developers.google.com/maps/), сохраняет его в виде локального файла и открывает средство просмотра изображения по умолчанию.

**Пример клиента Twitter** | [подробное описание](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/extending-httpclient-with-oauth-to-access-twitter.aspx) | [источника Visual STUDIO 2012](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/TwitterSample)

Показывает способы написания простого клиента Twitter с помощью **HttpClient**. В образце используется **HttpMessageHandler** вставляемый сведения для проверки подлинности OAuth в исходящий **HttpRequestMessage**. В результате Twitter считывается с помощью JSON.NET. Перед запуском этого образца необходимо получить [ключ приложения из Twitter](https://dev.twitter.com/)и заполните поля в классе образцов OAuthMessageHandler.

**Пример всемирного банка** | [подробное описание](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/httpclient-is-here.aspx) | [источника VS 2010](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/WorldBankSample/Net40) | [источника Visual STUDIO 2012](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/WorldBankSample/Net45)

Показывает способ получения данных с сайта всемирного банка данных, с помощью JSON.NET мог проанализировать результат.

## <a name="web-api-samples"></a>Примеры веб-API

**Приступая к работе с веб-API ASP.NET** | [источника Visual STUDIO 2012](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)

Показано, как создать базовый веб-API, которая поддерживает запросы HTTP GET. Содержит исходный код для работы с учебником [свой первый веб-API ASP.NET](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).

**Сценарии ASP.NET Web API JavaScript — комментарии** | [источника Visual STUDIO 2012](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7)

Показано, как использовать веб-API ASP.NET для создания веб-API, которые поддерживают клиентов браузера и можно легко вызывать с помощью jQuery.

**Диспетчер контактов** | [источника VS 2010](https://code.msdn.microsoft.com/Contact-Manager-Web-API-0e8e373d)

Создание приложения простой Диспетчер контактов в этом примере используется веб-API ASP.NET. Приложение состоит из веб-диспетчера контактов API, используемый для отображения и управления этим списком контактов с приложением ASP.NET MVC и приложения Windows Phone.

**Пакетная обработка образца** | [подробное описание](http://trocolate.wordpress.com/2012/07/19/mitigate-issue-260-in-batching-scenario/) | [источника Visual STUDIO 2012](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/BatchSample)

Показан способ реализации пакетной обработки HTTP в ASP.NET. Пакетная обработка состоит из размещение нескольких HTTP-запросов в один текст составных сущностей MIME, который затем отправляется на сервер как запрос HTTP POST. Запросы обрабатываются по отдельности, а ответы помещаются в другой текст составных сущностей MIME, который возвращается клиенту.

**Содержимое образце контроллера** | [подробное описание](https://blogs.msdn.com/b/henrikn/archive/2012/02/24/async-actions-in-asp-net-web-api.aspx) | [источника VS 2010](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/ContentControllerSample/Net40) | [источника Visual STUDIO 2012](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/ContentControllerSample/Net45)

Показано, как для чтения и записи запросов и ответов сущностей, асинхронно с помощью потоков. Пример контроллера имеет два действия: действие PUT, который асинхронно считывает тело сущности запроса и сохраняет его в локальном файле и действие GET, возвращающий содержимое локального файла.

**Пример сопоставителя пользовательские сборки** | [источника Visual STUDIO 2012](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/CustomAssemblyResolverSample)

Показано, как изменить веб-API ASP.NET для поддержки обнаружения контроллеров из динамически загружаемые библиотеки сборки. В этом образце реализуется пользовательский **IAssembliesResolver** которого вызывает реализацию по умолчанию, а затем добавляет сборку библиотеки по умолчанию.

**Пользовательский образец модуля форматирования типа мультимедиа** | [подробное описание](https://blogs.msdn.com/b/henrikn/archive/2012/04/23/using-cookies-with-asp-net-web-api.aspx) | [источника VS 2010](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/CustomMediaTypeFormatterSample)

Показано, как создать модуль форматирования типа пользовательских мультимедиа с помощью **BufferedMediaTypeFormatter** базового класса. Этот базовый класс предназначен для модулей форматирования, которые в основном используют синхронного ввода и операции записи. В дополнение к отображение модули форматирования типа мультимедиа, образце показано, как подключить его, зарегистрировав его как часть **HttpConfiguration** для вашего приложения. Обратите внимание, что также можно использовать **MediaTypeFormatter** напрямую, базовый класс для модулей форматирования, которые в основном используют асинхронные операции чтения и записи.

**Пример привязки настраиваемых параметров** | [подробное описание](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx) | [источника VS 2010](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/CustomParameterBinding)

Описание способов настройки процесса привязки параметров, который является процесс, который определяет, каким образом сведения из запроса привязан к параметрам действия. В этом образце контроллера Home имеет четыре действия:

1. BindPrincipal показано, как привязать параметр IPrincipal на основе пользовательского универсального участника, не из сообщение HTTP GET;
2. BindCustomComplexTypeFromUriOrBody показано, как выполнить привязку параметра сложного типа, который могут быть получены из сообщения или запрос URI метода сообщения HTTP POST;
3. BindCustomComplexTypeFromUriWithRenamedProperty показано, как привязать параметр сложного типа со свойством переименован, происходящий из запрос URI метода сообщения HTTP POST;
4. PostMultipleParametersFromBody показано, как привязать несколько параметров из тела сообщения POST;

**Пример отправки в файл** | [подробное описание](https://blogs.msdn.com/b/henrikn/archive/2012/03/01/file-upload-and-asp-net-web-api.aspx) | [источника Visual STUDIO 2012](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/FileUploadSample)

Показано, как передать файлы **ApiController** с помощью отправки файлов MIME Multipart и как настроить уведомления о ходе выполнения с **HttpClient** с помощью **ProgressNotificationHandler**. Контроллер асинхронно считывает содержимое данных, передаваемых файлов HTML и записывает один или несколько разделов тела в локальный файл. Ответ содержит сведения о загруженный файл (или файлы).

**Отправка Azure Blob Store пример файла** | [подробное описание](https://blogs.msdn.com/b/yaohuang1/archive/2012/07/02/asp-net-web-api-and-azure-blob-storage.aspx) | [источника Visual STUDIO 2012](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/AzureBlobsFileUploadSample)

Этот пример похож на пример отправки файла, но вместо сохранения загруженные файлы на локальном диске, он асинхронно передает файлы в [Azure Blob Store](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs) с помощью [Windows Azure SDK для .NET](https://www.windowsazure.com/develop/net/). Он также предоставляет механизм для перечисления больших двоичных объектов, находящегося в [контейнера больших двоичных объектов Azure](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Вы можете опробовать работу примера от **эмулятора хранения Azure** , который поставляется вместе с пакетом SDK для Azure. Если у вас есть [учетной записи хранения Azure](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs), можно запускать службу реальной хранилища.

**Пример конвейера обработчика сообщений HTTP** | [подробное описание](https://blogs.msdn.com/b/henrikn/archive/2012/08/07/httpclient-httpclienthandler-and-httpwebrequesthandler.aspx) | [источника VS 2010](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/HttpMessageHandlerPipelineSample)

Показано, как подключить **HttpMessageHandler** экземпляров на оба клиента (**HttpClient**) и сервера (веб-API ASP.NET). В примере и тот же обработчик используется на клиенте и сервере. Это случается редко, точно один обработчик будет выполняться в обоих местах, объектной модели является одинаковым на стороне клиента и сервера.

**Пример отправки в JSON** | [источника Visual STUDIO 2012](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/JsonUploadSample)

Показано, как отправлять и загружать JSON и из **ApiController**. В образце используется минимальным **ApiController** и обращается к нему с помощью **HttpClient**.

**Пример гибридного веб-приложения** | [подробное описание](https://blogs.msdn.com/b/henrikn/archive/2012/03/03/async-mashups-using-asp-net-web-api.aspx) | [источника Visual STUDIO 2012](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/MashupSample)

Показано, как получить доступ к несколько удаленных сайтов асинхронно в **ApiController** действие. Каждый раз, когда действие будет достигнута, запросы выполняются асинхронно, таким образом, чтобы потоки не блокируются.

**Пример трассировки памяти** | [подробное описание](https://blogs.msdn.com/b/roncain/archive/2012/04/12/tracing-in-asp-net-web-api.aspx) | [источника VS 2010](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/MemoryTracingSample)

Этот пример проекта создает пакет Nuget, который устанавливает модуль записи трассировки в памяти в приложениях ASP.NET Web API.

**Пример MongoDB** | [подробное описание](https://blogs.msdn.com/b/henrikn/archive/2012/02/19/using-web-api-with-mongodb.aspx) | [источника Visual STUDIO 2012](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/MongoSample)

Показано, как использовать MongoDB в качестве постоянного хранилища для **ApiController**, используя шаблон репозитория.

**Пример процессора тело ответа** | [источника Visual STUDIO 2012](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/ResponseEntityProcessorSample)

Показано, как скопировать объект ответа (то есть, тело ответа HTTP) в локальный файл, перед их передачей клиенту и дополнительной обработки для этого файла асинхронно. В этом примере реализован **HttpMessageHandler** , который создает оболочку объект ответа с одним, что оба записывает сама в выходные данные в обычном режиме и в локальный файл.

**Загрузить пример XDocument** | [подробное описание](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/push-and-pull-streams-using-httpclient.aspx) | [источника Visual STUDIO 2012](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/UploadXDocumentSample)

Приводятся сведения об отправке XDocument для **ApiController** с помощью **PushStreamContent** и **HttpClient**.

**Пример проверки** | [источника VS 2010](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/ValidationSample)

Показано, как можно использовать атрибуты проверки для моделей в веб-API ASP.NET для проверки содержимого HTTP-запроса. Демонстрирует, как пометить свойства как обязательный, как использовать оба определяемые платформой и пользовательские атрибуты проверки заметок к модели и как вернуть ответы на ошибки для недопустимых состояниях модели.

**Веб-пример формы** | [подробное описание](https://blogs.msdn.com/b/henrikn/archive/2012/02/23/using-asp-net-web-api-with-asp-net-web-forms.aspx) | [источника VS 2010](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/WebFormSample)

Показывает ApiController добавить в проект веб-форм.

**[Пример RestBugs](https://github.com/howarddierking/RestBugs)**

RestBugs является простой отслеживания приложение, которое показывает, как использовать веб-API ASP.NET и новую библиотеку HTTP-клиент для создания системы на основе гиперсред ошибок. Пример включает реализации клиента и сервера, с помощью веб-API ASP.NET. Сервер использует пользовательский модуль форматирования Razor для создания представлений ресурсов. Пример также содержит сервер node.js для демонстрации преимуществ, которые поступают от разработки гипермедиа для отделения клиентов и серверов.
