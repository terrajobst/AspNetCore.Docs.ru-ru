---
title: "Страницы справки по веб-API ASP.NET Core с использованием Swagger"
author: spboyer
description: "В этом учебнике приводится пошаговое руководство по добавлению Swagger для составления документации и страниц справки к приложению веб-API."
keywords: "ASP.NET Core, Swagger, Swashbuckle, страницы справки, веб-API"
ms.author: spboyer
manager: wpickett
ms.date: 09/01/2017
ms.topic: article
ms.assetid: 54bb961d-29d9-4dee-8e2c-a93fc33c16f2
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: 8a87972a7394246ece2af3485d93739975ba5383
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-core-web-api-help-pages-using-swagger"></a>Страницы справки по веб-API ASP.NET Core с использованием Swagger

<a name="web-api-help-pages-using-swagger"></a>

Авторы: [Шейн Бойер](https://twitter.com/spboyer) (Shayne Boyer) и [Скотт Эдди](https://twitter.com/Scott_Addie) (Scott Addie)

При сборке пользовательского приложения разработчик может обнаружить, что разобраться в различных методах API не так-то просто.

Используя [Swagger](https://swagger.io/) с реализацией .NET Core [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore), можно получить хорошую документацию и справку по веб-API, добавив пару пакетов NuGet и скорректировав файл *Startup.cs*.

* [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) — это проект с открытым исходным кодом, предназначенный для создания документов Swagger по веб-API ASP.NET Core.

* [Swagger](https://swagger.io/) — это машиночитаемое представление API RESTful, которое обеспечивает поддержку интерактивной документации, создание клиентских пакетов SDK и возможность обнаружения.

В этом учебнике используется пример из статьи [Сборка первого веб-API с использованием ASP.NET Core MVC и Visual Studio](xref:tutorials/first-web-api). Если вы хотите повторить описанные действия, загрузите этот пример со страницы [https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample).

## <a name="getting-started"></a>Начало работы

Swashbuckle включает три основных компонента.

* `Swashbuckle.AspNetCore.Swagger`: объектная модель Swagger и ПО промежуточного слоя для предоставления объектов `SwaggerDocument` как конечных точек JSON.

* `Swashbuckle.AspNetCore.SwaggerGen`: генератор Swagger, создающий объекты `SwaggerDocument` непосредственно из ваших маршрутов, контроллеров и моделей. Как правило, он комбинируется с ПО промежуточного слоя конечной точки Swagger и за счет этого предоставляет Swagger JSON автоматически.

* `Swashbuckle.AspNetCore.SwaggerUI`: встроенная версия пользовательского интерфейса Swagger, которая интерпретирует Swagger JSON и обеспечивает удобную, настраиваемую среду для описания функциональности веб-API. Включает встроенные окружения тестов для открытых методов.

## <a name="nuget-packages"></a>Пакеты NuGet

Swashbuckle можно добавить одним из описанных ниже способов.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* В окне **Консоль диспетчера пакетов**

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* В диалоговом окне **Управление пакетами NuGet**

     * Щелкните правой кнопкой мыши проект в **обозревателе решений** > **Управление пакетами NuGet**.
     * В качестве **источника пакета** выберите "nuget.org".
     * В поле поиска введите "Swashbuckle.AspNetCore".
     * Выберите пакет "Swashbuckle.AspNetCore" на вкладке **Обзор** и нажмите кнопку **Установить**.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio для Mac](#tab/visual-studio-mac)

* Щелкните правой кнопкой мыши папку *Пакеты* на **панели решения** > **Добавление пакетов...**.
* В раскрывающемся списке **Источник** в окне **Добавление пакетов** выберите вариант "nuget.org".
* В поле поиска введите "Swashbuckle.AspNetCore".
* В области результатов выберите пакет "Swashbuckle.AspNetCore" и нажмите кнопку **Добавить пакет**.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code)

Во **встроенном терминале** выполните следующую команду.

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

Выполните следующую команду:

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-to-the-middleware"></a>Добавление и настройка Swagger для ПО промежуточного слоя

Добавьте генератор Swagger в коллекцию служб в методе `ConfigureServices` файла *Startup.cs*:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_ConfigureServices&highlight=7-10)]

Добавьте следующий оператор using для класса `Info`:

```csharp
using Swashbuckle.AspNetCore.Swagger;
```

В методе `Configure` файла *Startup.cs* включите ПО промежуточного слоя для обслуживания созданного документа JSON и SwaggerUI:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_Configure&highlight=4,7-10)]

Запустите приложение и перейдите к файлу `http://localhost:<random_port>/swagger/v1/swagger.json`. Появится созданный документ с описанием конечных точек.

**Примечание**. Microsoft Edge, Google Chrome и Firefox отображают документы JSON изначально. Существуют расширения для Chrome, которые форматируют документ для удобства чтения. *Для краткости следующий пример приводится в сокращенном варианте.*

```json
{
   "swagger": "2.0",
   "info": {
       "version": "v1",
       "title": "API V1"
   },
   "basePath": "/",
   "paths": {
       "/api/Todo": {
           "get": {
               "tags": [
                   "Todo"
               ],
               "operationId": "ApiTodoGet",
               "consumes": [],
               "produces": [
                   "text/plain",
                   "application/json",
                   "text/json"
               ],
               "responses": {
                   "200": {
                       "description": "Success",
                       "schema": {
                           "type": "array",
                           "items": {
                               "$ref": "#/definitions/TodoItem"
                           }
                       }
                   }
                }
           },
           "post": {
               ...
           }
       },
       "/api/Todo/{id}": {
           "get": {
               ...
           },
           "put": {
               ...
           },
           "delete": {
               ...
   },
   "definitions": {
       "TodoItem": {
           "type": "object",
            "properties": {
                "id": {
                    "format": "int64",
                    "type": "integer"
                },
                "name": {
                    "type": "string"
                },
                "isComplete": {
                    "default": false,
                    "type": "boolean"
                }
            }
       }
   },
   "securityDefinitions": {}
}
```

Этот документ запускает пользовательский интерфейс Swagger, для просмотра которого нужно перейти к `http://localhost:<random_port>/swagger`.

![Пользовательский интерфейс Swagger](web-api-help-pages-using-swagger/_static/swagger-ui.png)

Каждый метод открытого действия в `TodoController` можно протестировать в пользовательском интерфейсе. Щелкните имя метода, чтобы развернуть соответствующий раздел. Добавьте все необходимые параметры и нажмите кнопку "Попробуйте!".

![Пример тестирования в Swagger метода GET](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

## <a name="customization--extensibility"></a>Настройка и расширяемость

Swagger предоставляет параметры для документирования объектной модели и настройки пользовательского интерфейса в соответствии с вашим фирменным стилем.

### <a name="api-info-and-description"></a>Данные и описание API

Действия по настройке, передаваемые в метод `AddSwaggerGen`, можно использовать для добавления таких сведений, как автор, лицензия и описание:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?range=20-30,36)]

Ниже показан фрагмент пользовательского интерфейса Swagger, в котором отображаются сведения о версии.

![Пользовательский интерфейс Swagger с данными версии: описание, автор и ссылка на дополнительные сведения](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a>XML-комментарии

XML-комментарии можно включить следующим образом.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* В **обозревателе решений** щелкните проект правой кнопкой мыши и выберите пункт **Свойства**.
* Установите флажок **Файл XML-документации** в разделе **Вывод** на вкладке **Сборка**:

![Свойства проекта, вкладка "Сборка"](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio для Mac](#tab/visual-studio-mac)

* Откройте диалоговое окно **Параметры проекта** > **Сборка** > **Компилятор**.
* Установите флажок **Сформировать документацию XML** в разделе **Общие параметры**:

![Параметры проекта, раздел "Общие параметры"](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code)

Вручную добавьте в файл *.csproj* следующий фрагмент кода:

[!code-xml[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/TodoApi.csproj?range=7-9)]

---

Настройте Swagger на использование созданного XML-файла. В Linux и других операционных системах, отличных от Windows, имена и пути файлов могут быть чувствительны к регистру. Например, файл *ToDoApi.XML* будет найден в Windows, но не в CentOS.

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_ConfigureServices&highlight=20-22)]

В представленном выше коде `ApplicationBasePath` получает базовый путь приложения. Базовый путь используется для поиска файла XML-комментариев. *TodoApi.xml* работает только в этом примере, так как имя созданного файла комментариев XML основано на имени приложения.

Включение в метод комментариев с тройной косой чертой улучшает пользовательский интерфейс Swagger, так как позволяет добавить описание к заголовку раздела:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete&highlight=2)]

![Пользовательский интерфейс Swagger с XML-комментарием "Удаляет указанный элемент TodoItem" для метода DELETE](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

Пользовательский интерфейс определяется созданным файлом JSON, который также содержит следующие комментарии.

```json
"delete": {
    "tags": [
        "Todo"
    ],
    "summary": "Deletes a specific TodoItem.",
    "operationId": "ApiTodoByIdDelete",
    "consumes": [],
    "produces": [],
    "parameters": [
        {
            "name": "id",
            "in": "path",
            "description": "",
            "required": true,
            "type": "integer",
            "format": "int64"
        }
    ],
    "responses": {
        "200": {
            "description": "Success"
        }
    }
}
```

Добавьте тег [<remarks>](https://docs.microsoft.com/dotnet/csharp/programming-guide/xmldoc/remarks) в документацию к методу действия `Create`. Он дополняет сведения, указанные в теге `<summary>`, и предоставляет более надежный пользовательский интерфейс Swagger. Содержимое тега `<remarks>` может включать текст, JSON или XML.

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

Обратите внимание, как эти дополнительные комментарии улучшают пользовательский интерфейс.

![Пользовательский интерфейс Swagger с показанными дополнительными комментариями](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a>Заметки к данным

Добавьте к модели атрибуты из `System.ComponentModel.DataAnnotations`, чтобы упростить реализацию компонентов пользовательского интерфейса Swagger.

Добавьте атрибут `[Required]` к свойству `Name` класса `TodoItem`.

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Models/TodoItem.cs?highlight=10)]

Наличие этого атрибута изменяет поведение пользовательского интерфейса и схему базового JSON.

```json
"definitions": {
    "TodoItem": {
        "required": [
            "name"
        ],
        "type": "object",
        "properties": {
            "id": {
                "format": "int64",
                "type": "integer"
            },
            "name": {
                "type": "string"
            },
            "isComplete": {
                "default": false,
                "type": "boolean"
            }
        }
    }
},
```

Добавьте к контроллеру API атрибут `[Produces("application/json")]`. Он позволяет объявить, что действия контроллера поддерживают возврат типа содержимого *application/json*:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_TodoController&highlight=3)]

В раскрывающемся списке **Тип содержимого ответа** этот тип содержимого выбирается по умолчанию для операций контроллера GET.

![Пользовательский интерфейс Swagger с типом содержимого ответа по умолчанию](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

Чем больше заметок к данным используется в веб-API, тем более содержательными и полезными становятся пользовательский интерфейс и страницы справки по API.

### <a name="describing-response-types"></a>Описания типов ответов

Разработчиков пользовательских приложений в первую очередь волнуют возвращаемые данные &mdash; а именно типы ответов и коды ошибок (если они не стандартны). Все это указывается в XML-комментарии и заметках к данным.

Действие `Create` возвращает `201 Created` в случае успеха и `400 Bad Request`, если текст опубликованного запроса пустой. Без надлежащей документации в пользовательском интерфейсе Swagger пользователь не будет знать, чего ожидать. Эту проблему решает добавление строк, выделенных в следующем примере:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

Теперь пользовательский интерфейс Swagger четко документирует ожидаемые коды HTTP-ответов.

![Пользовательский интерфейс Swagger, показывающий описание класса ответа POST "Возвращает вновь созданный элемент Todo" и "400 — Если элемент имеет значение NULL" для кода состояния и причины в разделе "Сообщения ответов"](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customizing-the-ui"></a>Настройка пользовательского интерфейса

Стандартный пользовательский интерфейс функционален и представителен, но при сборке документации для своего API вы, скорее всего, захотите отобразить свой бренд или фирменный стиль. Для решения этой задачи с использованием компонентов Swashbuckle необходимо добавить ресурсы для обслуживания статических файлов, а затем построить структуру папок для их размещения.

Если документация создается для платформы .NET Framework, добавьте в проект пакет NuGet `Microsoft.AspNetCore.StaticFiles`.

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

Включите ПО промежуточного слоя для статических файлов:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_Configure&highlight=3)]

Получите содержимое папки *dist* из [репозитория GitHub пользовательского интерфейса Swagger](https://github.com/swagger-api/swagger-ui/tree/2.x/dist). Эта папка содержит ресурсы, необходимые для страниц пользовательского интерфейса Swagger.

Создайте папку *wwwroot/swagger/ui* и скопируйте в нее содержимое папки *dist*.

Создайте файл *wwwroot/swagger/ui/css/custom.css* со следующим кодом CSS для настройки заголовка страницы:

[!code-css[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/css/custom.css)]

Сошлитесь на файл *custom.css* в файле *index.html*:

[!code-html[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/index.html?range=14)]

Перейдите на страницу *index.html* в `http://localhost:<random_port>/swagger/ui/index.html`. Введите `http://localhost:<random_port>/swagger/v1/swagger.json` в текстовое поле заголовка и нажмите кнопку **Проводник**. Полученная в итоге страница будет выглядеть следующим образом.

![Пользовательский интерфейс Swagger с настроенным заголовком](web-api-help-pages-using-swagger/_static/custom-header.png)

С этой страницей можно сделать гораздо больше. Все возможности ресурсов пользовательского интерфейса см. в статье о [репозитории GitHub пользовательского интерфейса Swagger](https://github.com/swagger-api/swagger-ui).
