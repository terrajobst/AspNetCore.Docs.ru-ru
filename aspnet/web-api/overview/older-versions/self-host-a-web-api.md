---
uid: web-api/overview/older-versions/self-host-a-web-api
title: "Резидентной ASP.NET Web API 1 (C#) | Документы Microsoft"
author: MikeWasson
description: "Веб-API ASP.NET не требуются службы IIS. Самостоятельно, можно разместить веб-API в свои собственные хост-процесса. Этот учебник показывает, как для размещения веб-API в консоли паролей..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2012
ms.topic: article
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: 564f859e73a88ac9c5f27e9b8f7409ec126642f8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="self-host-aspnet-web-api-1-c"></a>Резидентной ASP.NET Web API 1 (C#)
====================
по [Mike Wasson](https://github.com/MikeWasson)

> Веб-API ASP.NET не требуются службы IIS. Самостоятельно, можно разместить веб-API в свои собственные хост-процесса. Этот учебник показывает, как для размещения веб-API внутри консольного приложения.
> 
> **Новые приложения должны использовать OWIN для самостоятельного размещения веб-API.** В разделе [использовать OWIN для самостоятельного размещения ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемая в этом учебнике
> 
> 
> - Веб-API 1
> - Visual Studio 2012


## <a name="create-the-console-application-project"></a>Создайте проект консольного приложения

Запустите Visual Studio и выберите **новый проект** из **запустить** страницы. Или из **файл** последовательно выберите пункты **New** и затем **проекта**.

В **шаблоны** выберите **установленные шаблоны** и разверните **Visual C#** узла. В разделе **Visual C#**выберите **Windows**. В списке шаблонов проектов выберите **консольное приложение**. Назовите проект &quot;SelfHost&quot; и нажмите кнопку **ОК**.

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a>Требуемая версия (Visual Studio 2010)

Если вы используете Visual Studio 2010, необходимо измените целевую платформу на .NET Framework 4.0. (По умолчанию предназначен шаблон проекта [профиля .net Framework клиента](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)

В обозревателе решений щелкните правой кнопкой мыши проект и выберите **свойства**. В **требуемой версии .NET framework** раскрывающемся списке, измените целевую платформу .NET Framework 4.0. Для применения изменений нажмите кнопку **Да**.

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a>Установка диспетчера пакетов NuGet

Диспетчер пакетов NuGet является самым простым способом добавления сборок веб-API в проект не ASP.NET.

Чтобы проверить, установлены ли диспетчер пакетов NuGet, щелкните **средства** меню в Visual Studio. Если вы видите пункт меню **диспетчер пакетов библиотеки**, то диспетчер пакетов NuGet.

Чтобы установить диспетчер пакетов NuGet:

1. Запустите Visual Studio.
2. Из **средства** последовательно выберите пункты **расширения и обновления**.
3. В **расширения и обновления** диалогового окна выберите **Online**.
4. Если вы не видите «диспетчер пакетов NuGet», введите «Диспетчер пакетов nuget» в поле поиска.
5. Выберите диспетчер пакетов NuGet и нажмите кнопку **загрузить**.
6. После завершения загрузки, будет предложено установить.
7. После завершения установки может потребоваться перезапустить Visual Studio.

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a>Добавить веб-пакет NuGet интерфейса API

После установки диспетчера пакетов NuGet добавьте Web API Self-Host пакет в проект.

1. Из **средства** последовательно выберите пункты **диспетчер пакетов библиотеки**. *Примечание*: Если вы не отображается это меню элемента, убедитесь, что этот диспетчер пакетов NuGet установлены правильно.
2. Выберите **управление пакетами NuGet для решения...**
3. В **управление пакетами фрагментом** диалогового окна выберите **Online**.
4. В поле поиска введите &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.
5. Выберите пакет ASP.NET Web API Self Host и нажмите кнопку **установить**.
6. После установки пакета, нажмите кнопку **закрыть** чтобы закрыть диалоговое окно.

> [!NOTE]
> Не забудьте установить пакет с именем Microsoft.AspNet.WebApi.SelfHost, не AspNetWebApi.SelfHost.


![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a>Создание модели и контроллера

В этом учебнике используются те же классы модели и контроллера, как [Приступая к работе](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) учебника.

Добавьте открытый класс с именем `Product`.

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

Добавьте открытый класс с именем `ProductsController`. Наследовать этот класс из **System.Web.Http.ApiController**.

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

Дополнительные сведения о коде в этом контроллере см. в разделе [Приступая к работе](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) учебника. Этот контроллер определяет три действия GET:

| URI | Описание: |
| --- | --- |
| / api/продуктов | Получение списка всех продуктов. |
| /api/products/*id* | Получение продукта по идентификатору. |
| /API/продукты /? категории =*категории* | Получение списка продуктов по категориям. |

## <a name="host-the-web-api"></a>Размещения веб-API

Откройте файл Program.cs и добавьте следующие операторы using:

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

Добавьте следующий код в **программы** класса.

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a>(Необязательно) Добавить резервирование пространства имен HTTP URL-адрес

Это приложение прослушивает `http://localhost:8080/`. По умолчанию прослушивание по определенному адресу HTTP требуются права администратора. При запуске учебника, таким образом, можно получить эту ошибку: «HTTP не удалось зарегистрировать URL-адрес http://+:8080/» существует два способа, чтобы избежать этой ошибки:

- Запустите Visual Studio с расширенными правами администратора, или
- Используйте Netsh.exe для предоставления разрешений учетной записи для резервирования URL-адрес.

Чтобы использовать Netsh.exe, откройте командную строку с правами администратора и введите следующую команду: следующее:

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

где *компьютер\имя_пользователя* вашей учетной записи пользователя.

По окончании размещении, не забудьте удалить резервирование:

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a>Вызов веб-API из клиентского приложения (C#)

Напишем простое консольное приложение, которое вызывает веб-API.

Добавьте в решение новый проект консольного приложения:

- В обозревателе решений щелкните правой кнопкой мыши решение и выберите **Добавление нового проекта**.
- Создайте новое консольное приложение с именем &quot;ClientApp&quot;.

![](self-host-a-web-api/_static/image5.png)

Используйте диспетчер пакетов NuGet для добавления пакета ASP.NET Web API Core Libraries:

- В меню «Сервис» выберите **диспетчер пакетов библиотеки**.
- Выберите **управление пакетами NuGet для решения...**
- В **управление пакетами NuGet** диалогового окна выберите **Online**.
- В поле поиска введите &quot;Microsoft.AspNet.WebApi.Client&quot;.
- Выберите пакет Microsoft ASP.NET Web API Client Libraries и нажмите кнопку **установить**.

Добавьте ссылку в ClientApp SelfHost проект:

- В обозревателе решений щелкните правой кнопкой мыши проект ClientApp.
- Выберите **добавить ссылку**.
- В **диспетчер ссылок** диалогового окна в разделе **решения**выберите **проекты**.
- Выберите проект SelfHost.
- Нажмите кнопку **ОК**.

![](self-host-a-web-api/_static/image6.png)

Откройте файл Client/Program.cs. Добавьте следующие **с помощью** инструкции:

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

Добавьте статическое **HttpClient** экземпляр:

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

Добавьте следующие методы для получения списка всех продуктов, список продуктов по Идентификатору и перечислены продукты по категориям.

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

Каждый из этих методов следует той же схеме:

1. Вызовите **HttpClient.GetAsync** для отправки запроса GET с соответствующим URI.
2. Вызовите **HttpResponseMessage.EnsureSuccessStatusCode**. Этот метод вызывает исключение, если состояние ответа HTTP представляет собой код ошибки.
3. Вызовите **ReadAsAsync&lt;T&gt;**  для десериализации типа CLR из HTTP-ответа. Этот метод является методом расширения, определенные в **System.Net.Http.HttpContentExtensions**.

**GetAsync** и **ReadAsAsync** методы являются как асинхронные. Они возвращают **задачи** объектов, представляющих асинхронную операцию. Получение **результат** свойство блокирует поток до завершения операции.

Дополнительные сведения об использовании HttpClient, включая внесение неблокирующих вызовов в разделе [вызов Web API из клиента .NET](../advanced/calling-a-web-api-from-a-net-client.md).

Перед вызовом этих методов, задайте свойство BaseAddress на экземпляре HttpClient «`http://localhost:8080`». Пример:

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

Это должно получен следующий результат. (Не забудьте сначала запустите приложение SelfHost.)

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
