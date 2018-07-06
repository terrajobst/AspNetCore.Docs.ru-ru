---
uid: web-api/overview/older-versions/self-host-a-web-api
title: Резидентного размещения ASP.NET веб-API 1 (C#) | Документация Майкрософт
author: MikeWasson
description: Веб-API ASP.NET не требуются службы IIS. Вы можете самостоятельно размещать веб-API в свои собственные хост-процессе. Этот учебник показывает, как для размещения веб-API внутри консоли рабоче...
ms.author: aspnetcontent
ms.date: 01/26/2012
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: 50681dcd89dfed480cf343f753371af384fd3e68
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811741"
---
<a name="self-host-aspnet-web-api-1-c"></a>Резидентного размещения ASP.NET веб-API 1 (C#)
====================
по [Майк Уоссон](https://github.com/MikeWasson)

> Веб-API ASP.NET не требуются службы IIS. Вы можете самостоятельно размещать веб-API в свои собственные хост-процессе. Этом руководстве показано, как разместить веб-API внутри консольного приложения.
> 
> **Новые приложения должны использовать OWIN для резидентного размещения веб-API.** См. в разделе [использовать OWIN для резидентного размещения веб-API 2 ASP.NET](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемые в этом руководстве
> 
> 
> - Веб-API 1
> - Visual Studio 2012


## <a name="create-the-console-application-project"></a>Создайте проект консольного приложения

Запустите Visual Studio и выберите **новый проект** из **запустить** страницы. Или с **файл** меню, выберите **New** и затем **проекта**.

В **шаблоны** области выберите **установленные шаблоны** и разверните **Visual C#** узла. В разделе **Visual C#** выберите **Windows**. В списке шаблонов проектов выберите **консольное приложение**. Назовите проект &quot;SelfHost&quot; и нажмите кнопку **ОК**.

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a>Задать целевую платформу (Visual Studio 2010)

Если вы используете Visual Studio 2010, измените целевую платформу .NET Framework 4.0. (По умолчанию, которой предназначен шаблон проекта [профиль .net Framework клиента](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)

В обозревателе решений щелкните правой кнопкой мыши проект и выберите **свойства**. В **требуемой версии .NET framework** раскрывающемся списке, изменить целевую платформу .NET Framework 4.0. Для применения изменений нажмите кнопку **Да**.

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a>Установить диспетчер пакетов NuGet

Диспетчер пакетов NuGet — это самый простой способ добавить сборки веб-API в проект не ASP.NET.

Чтобы проверить, установлен ли диспетчер пакетов NuGet, щелкните **средства** меню в Visual Studio. Если вы видите пункт меню **диспетчер пакетов библиотеки**, то у вас есть диспетчер пакетов NuGet.

Чтобы установить диспетчер пакетов NuGet:

1. Запустите Visual Studio.
2. Из **средства** меню, выберите **расширения и обновления**.
3. В **расширения и обновления** диалоговом окне выберите **Online**.
4. Если вы не видите «диспетчер пакетов NuGet», введите «Диспетчер пакетов nuget» в поле поиска.
5. Выберите диспетчер пакетов NuGet и нажмите кнопку **загрузить**.
6. После завершения загрузки, вам будет предложено установить.
7. После завершения установки может быть предложено перезапустить Visual Studio.

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a>Добавление веб-пакета NuGet для API

После установки диспетчер пакетов NuGet, добавьте пакет Web API Self-Host в проект.

1. Из **средства** меню, выберите **диспетчер пакетов библиотеки**. *Примечание*: Если вы не видите это меню товара, убедитесь, что диспетчер пакетов NuGet, неправильно установлен.
2. Выберите **управление пакетами NuGet для решения...**
3. В **управление пакетами NugGet** диалоговом окне выберите **Online**.
4. В поле поиска введите &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.
5. Выберите пакет API Self ASP.NET веб-узел и нажмите кнопку **установить**.
6. После установки пакета, нажмите кнопку **закрыть** чтобы закрыть диалоговое окно.

> [!NOTE]
> Убедитесь в том, что для установки пакета с именем Microsoft.AspNet.WebApi.SelfHost, не AspNetWebApi.SelfHost.


![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a>Создание модели и контроллера

В этом учебнике используется те же классы модели и контроллера, как [Приступая к работе](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) руководства.

Добавьте открытый класс с именем `Product`.

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

Добавьте открытый класс с именем `ProductsController`. Этот класс из **System.Web.Http.ApiController**.

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

Дополнительные сведения о коде в этом контроллере, см. в разделе [Приступая к работе](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) руководства. Этот контроллер определяет три действия GET:

| URI | Описание: |
| --- | --- |
| / api/продуктов | Получение списка всех продуктов. |
| /API/продукты/*идентификатор* | Получить продукт по идентификатору. |
| / API/продукты /? категории =*категории* | Получение списка продуктов по категориям. |

## <a name="host-the-web-api"></a>Размещение веб-API

Откройте файл Program.cs и добавьте следующие операторы using:

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

Добавьте следующий код, чтобы **программы** класса.

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a>(Необязательно) Добавить резервирование пространства имен URL-адрес HTTP

Это приложение прослушивает `http://localhost:8080/`. По умолчанию ожидающей передачи данных по конкретному адресу HTTP требуются права администратора. При запуске учебника, таким образом, может появиться следующая ошибка: «HTTP не удалось зарегистрировать URL-адрес http://+:8080/» существует два способа, чтобы избежать этой ошибки:

- Запуск Visual Studio с повышенными полномочиями администратора, или
- Используйте Netsh.exe для предоставления разрешений учетной записи для резервирования URL-адрес.

Чтобы использовать Netsh.exe, откройте командную строку с правами администратора и введите следующую команду: следующее:

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

где *компьютер\имя_пользователя* является учетная запись пользователя.

По завершении размещения на собственном сервере, не забудьте удалить резервирование:

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a>Вызов веб-API из клиентского приложения (C#)

Давайте напишем простое консольное приложение, вызывающее веб-API.

Добавьте новый проект консольного приложения в решение:

- В обозревателе решений щелкните решение правой кнопкой мыши и выберите **Добавление нового проекта**.
- Создайте новое консольное приложение с именем &quot;ClientApp&quot;.

![](self-host-a-web-api/_static/image5.png)

С помощью диспетчера пакетов NuGet для добавления пакета ASP.NET Web API основных библиотек:

- В меню "Сервис" выберите **диспетчер пакетов библиотеки**.
- Выберите **управление пакетами NuGet для решения...**
- В **управление пакетами NuGet** диалоговом окне выберите **Online**.
- В поле поиска введите &quot;Microsoft.AspNet.WebApi.Client&quot;.
- Выберите пакет Microsoft ASP.NET Web API клиентских библиотек и нажмите кнопку **установить**.

Добавьте ссылку в ClientApp SelfHost проект:

- В обозревателе решений щелкните правой кнопкой мыши проект ClientApp.
- Выберите команду **Добавить ссылку**.
- В **диспетчер ссылок** диалогового окна в разделе **решение**выберите **проекты**.
- Выберите проект SelfHost.
- Нажмите кнопку **ОК**.

![](self-host-a-web-api/_static/image6.png)

Откройте файл Client/Program.cs. Добавьте следующий **с помощью** инструкции:

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

Добавить статический **HttpClient** экземпляр:

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

Добавьте следующие методы для получения списка всех продуктов, список продуктов по Идентификатору и перечислены продукты по категориям.

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

Каждый из этих методов следует той же схеме:

1. Вызовите **HttpClient.GetAsync** для отправки запроса GET с соответствующим URI.
2. Вызовите **HttpResponseMessage.EnsureSuccessStatusCode**. Этот метод создает исключение, если состояние ответа HTTP является кодом ошибки.
3. Вызовите **ReadAsAsync&lt;T&gt;**  десериализовать тип среды CLR из HTTP-ответа. Этот метод является методом расширения, определенные в **System.Net.Http.HttpContentExtensions**.

**GetAsync** и **ReadAsAsync** методы являются как асинхронные. Они возвращают **задачи** объекты, представляющие асинхронной операции. Начало **результат** оно блокирует поток до завершения операции.

Дополнительные сведения об использовании HttpClient, включая вызов без блокировки, см. в разделе [вызова Web API из клиента .NET](../advanced/calling-a-web-api-from-a-net-client.md).

Прежде чем выполнять эти методы, свойства BaseAddress на экземпляре HttpClient "`http://localhost:8080`«. Пример:

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

Это должен отобразиться следующий результат. (Не забудьте сначала запустить SelfHost приложение.)

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
