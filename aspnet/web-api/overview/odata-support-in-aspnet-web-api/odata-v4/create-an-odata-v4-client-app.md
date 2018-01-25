---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: "Создание приложения клиента OData версии 4 (C#) | Документы Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2014
ms.topic: article
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: 51a3c7b9c5b6525d6d82b9a45910f58b71268b7f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="create-an-odata-v4-client-app-c"></a>Создание приложения клиента OData версии 4 (C#)
====================
по [Mike Wasson](https://github.com/MikeWasson)

В предыдущем учебнике вы создали базовую службу OData, поддерживающий операции CRUD. Теперь давайте создадим клиент для службы.

Запустите новый экземпляр Visual Studio и создайте новый проект консольного приложения. В **новый проект** диалогового окна выберите **установленные** &gt; **шаблоны** &gt; **Visual C#** &gt; **Windows Desktop**и выберите **консольное приложение** шаблона. Назовите проект &quot;ProductsApp&quot;.

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> Можно также добавить консольное приложение, в то же решение Visual Studio, которая содержит службу OData.


## <a name="install-the-odata-client-code-generator"></a>Установка клиента OData генератора кода.

Из **средства** последовательно выберите пункты **расширения и обновления**. Выберите **Online** &gt; **коллекции Visual Studio**. В поле поиска поиск &quot;генератор кода клиента OData&quot;. Нажмите кнопку **загрузки** выполнить установку VSIX. Может быть предложено перезапустить Visual Studio.

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a>Запустите службу OData локально

Запустите проект ProductService из Visual Studio. По умолчанию Visual Studio запускает браузер к корневому каталогу приложения. Обратите внимание, универсальный код Ресурса; он потребуется на следующем шаге. Не закрывайте приложение.

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> При размещении обоих проектов в том же решении, убедитесь, что для запуска проекта ProductService без отладки. На следующем шаге необходимо продолжить использование службы, выполнение, пока изменение проекта консольного приложения.


## <a name="generate-the-service-proxy"></a>Создать прокси-службы

Прокси-службы представляет собой класс .NET, который определяет методы для доступа к службе OData. Прокси-сервер преобразует вызовы метода в HTTP-запросов. Будет создан класс прокси-сервера, запустив [шаблон T4](https://msdn.microsoft.com/library/bb126445.aspx).

Щелкните правой кнопкой мыши проект. Выберите **добавить** &gt; **новый элемент**.

![](create-an-odata-v4-client-app/_static/image5.png)

В **Добавление нового элемента** диалогового окна выберите **элементы Visual C#** &gt; **кода** &gt; **клиента OData**. Имя шаблона &quot;ProductClient.tt&quot;. Нажмите кнопку **добавить** и просмотрите предупреждение безопасности.

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

На этом этапе вы получите ошибку, можно пропустить. Visual Studio автоматически запускает шаблона, но шаблон должен некоторые параметры конфигурации первого.

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

Откройте файл ProductClient.odata.config. В `Parameter` элемент, вставить в URI из проекта ProductService (на предыдущем шаге). Пример:

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

Еще раз запустите шаблон. В обозревателе решений щелкните правой кнопкой мыши файл ProductClient.tt и выберите **запустить пользовательский инструмент**.

Этот шаблон создает файл кода с именем ProductClient.cs, который определяет прокси-сервер. При разработке приложения, при изменении конечной точки OData, запустите шаблон еще раз, чтобы обновить прокси-сервер.

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a>Использовать прокси-службы для вызова службы OData

Откройте файл Program.cs и замените стандартный код следующим кодом.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

Замените значение *serviceUri* с URI службы из более ранних версий.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

При запуске приложения должны выводить следующее:

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
