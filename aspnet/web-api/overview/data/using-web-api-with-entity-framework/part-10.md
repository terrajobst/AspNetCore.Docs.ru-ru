---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Публикация приложения в Azure службы приложений Azure | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: 0290b392c1b292d0f3cc080dbfa25ec6103b2751
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400808"
---
<a name="publish-the-app-to-azure-azure-app-service"></a>Публикация приложения в Azure службы приложений Azure
====================
по [Майк Уоссон](https://github.com/MikeWasson)

[Скачать завершенный проект](https://github.com/MikeWasson/BookService)

На последнем этапе вы опубликуете приложение в Azure. В обозревателе решений щелкните правой кнопкой мыши проект и выберите **публикации**.

![](part-10/_static/image1.png)

Щелкнув **публикации** вызывает **веб-публикации** диалоговое окно. Если вы установили флажок **узел в облаке** при первом создании проекта, а затем соединение и параметры уже настроены. В этом случае просто щелкните **параметры** вкладку и проверьте &quot;выполнить Code First Migrations&quot;. (Если вы не проверили **узел в облаке** в начале, выполните действия в [разделу](#new-website).)

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

Чтобы развернуть приложение, нажмите кнопку **публикации**. Можно просмотреть ход выполнения публикации в **действий веб-публикации** окна. (Из **представление** меню, выберите **Other Windows**, а затем выберите **действий веб-публикации**.)

![](part-10/_static/image4.png)

Когда Visual Studio завершит развертывание приложения, автоматически откроется браузер по умолчанию URL-адрес развернутого веб-сайта и созданного приложения теперь работает в облаке. URL-адрес в адресной строке браузера показывает, что сайт загружается из Интернета.

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a>Развертывание новой веб-сайте

Если вы не проверили **узел в облаке** при первоначальном создании проекта, можно настроить новое веб-приложение теперь. В обозревателе решений щелкните правой кнопкой мыши проект и выберите **публикации**. Выберите **профиль** вкладку и нажмите кнопку **веб-сайтов Microsoft Azure**. Если вы не выполнили вход в настоящее время Azure, вам будет предложено войти.

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

В **существующие веб-сайты** диалоговое окно, нажмите кнопку **New**.

![](part-10/_static/image9.png)

Введите имя сайта. Выберите свою подписку Azure и регион. В разделе **сервер базы данных**выберите **создать новый сервер**, или выберите существующий сервер. Нажмите кнопку **Создать**.

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

Нажмите кнопку **параметры** вкладку и проверьте &quot;выполнить Code First Migrations&quot;. Нажмите кнопку **публикации**.

> [!div class="step-by-step"]
> [Назад](part-9.md)
