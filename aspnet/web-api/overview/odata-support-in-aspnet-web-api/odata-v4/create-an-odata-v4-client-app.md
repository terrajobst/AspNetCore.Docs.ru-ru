---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: Создание клиентского приложения OData v4 (C#) | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/26/2014
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: 4d62a64006e2a500e0379419dbebe7ddff16fba5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824062"
---
<a name="create-an-odata-v4-client-app-c"></a>Создание клиентского приложения OData v4 (C#)
====================
по [Майк Уоссон](https://github.com/MikeWasson)

В предыдущем руководстве вы создали базовую службу OData, который поддерживает операции CRUD. Теперь давайте создадим клиент для службы.

Запустите новый экземпляр Visual Studio и создайте новый проект консольного приложения. В **новый проект** диалоговом окне выберите **установленные** &gt; **шаблоны** &gt; **Visual C#** &gt; **Windows Desktop**и выберите **консольное приложение** шаблона. Назовите проект &quot;ProductsApp&quot;.

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> Можно также добавить консольное приложение, в то же решение Visual Studio, которая содержит службу OData.


## <a name="install-the-odata-client-code-generator"></a>Установите генератор кода клиент OData

Из **средства** меню, выберите **расширения и обновления**. Выберите **Online** &gt; **коллекции Visual Studio**. В поле поиска найдите &quot;генератор кода клиента OData&quot;. Нажмите кнопку **загрузить** установить VSIX. Может быть предложено перезапустить Visual Studio.

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a>Запуск службы OData локально

Запустите проект ProductService из Visual Studio. По умолчанию Visual Studio запустит браузер к корню приложения. Обратите внимание, универсальный код Ресурса; он потребуется на следующем шаге. Не закрывайте приложение.

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> Если поместить обоих проектов в одном решении, убедитесь, что запустить ProductService проект без отладки. На следующем шаге необходимо сохранить во время внесения изменений в проект консольного приложения службы.


## <a name="generate-the-service-proxy"></a>Создание прокси-службы

Прокси-службы представляет собой класс .NET, который определяет методы для доступа к службе OData. Прокси-сервер преобразует вызовы метода в HTTP-запросов. Будет создан класс прокси-сервера, выполнив [шаблон T4](https://msdn.microsoft.com/library/bb126445.aspx).

Щелкните правой кнопкой мыши проект. Выберите **добавить** &gt; **новый элемент**.

![](create-an-odata-v4-client-app/_static/image5.png)

В **Добавление нового элемента** диалоговом окне выберите **элементы Visual C#** &gt; **кода** &gt; **клиент OData**. Назовите шаблон &quot;ProductClient.tt&quot;. Нажмите кнопку **добавить** и просмотрите предупреждение безопасности.

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

На этом этапе вы получите ошибку, которое можно игнорировать. Visual Studio автоматически выполняет шаблон, но некоторые параметры конфигурации, нужно шаблон первой.

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

Откройте файл ProductClient.odata.config. В `Parameter` элемент, вставьте URI из проекта ProductService (предыдущий шаг). Пример:

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

Еще раз запустите шаблон. В обозревателе решений щелкните правой кнопкой мыши файл ProductClient.tt и выберите **пользовательское средство**.

Этот шаблон создает файл кода с именем ProductClient.cs, который определяет прокси-сервер. При разработке приложения, при изменении конечной точки OData, запустите шаблон еще раз, чтобы обновить прокси-сервер.

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a>Использовать прокси-службы для вызова службы OData

Откройте файл Program.cs и замените код следующим.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

Замените значение *serviceUri* с URI службы выше.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

При запуске приложения, должно отобразиться следующее:

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
