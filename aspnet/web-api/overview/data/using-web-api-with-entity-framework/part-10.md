---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Опубликуйте приложение в Azure службы приложений Azure | Документы Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: cc8a9199144e9fac041435938ea8899374ea199f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30867818"
---
<a name="publish-the-app-to-azure-azure-app-service"></a>Опубликуйте приложение в Azure службы приложений Azure
====================
по [Mike Wasson](https://github.com/MikeWasson)

[Загрузка завершенного проекта](https://github.com/MikeWasson/BookService)

На последнем этапе будет публиковать приложение в Azure. В обозревателе решений щелкните правой кнопкой мыши проект и выберите **публикации**.

![](part-10/_static/image1.png)

Щелкнув **публикации** вызывает **веб-публикация** диалогового окна. Если вы установили флажок **узлов в облаке** при первом создании проекта, а затем соединение и параметры уже настроены. В этом случае просто щелкните **параметры** вкладку и проверьте &quot;выполнять миграции Code First&quot;. (Если вы не выполняется проверка **узлов в облаке** в начале, выполните действия в [разделу](#new-website).)

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

Чтобы развернуть приложение, нажмите кнопку **публикации**. Можно просмотреть ход выполнения публикации в **действия публикации Web** окна. (От **представление** последовательно выберите пункты **другие окна**, а затем выберите **действия публикации Web**.)

![](part-10/_static/image4.png)

По завершении развертывание приложения Visual Studio автоматически откроется браузер по умолчанию URL-адрес веб-сайта, развернутые и созданное приложение теперь работает в облаке. URL-адрес в адресной строке браузера показывает, что сайт загружается из Интернета.

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a>Развертывание для нового веб-сайта

Если не были проверены **узлов в облаке** при создании проекта новое веб-приложение можно настроить сейчас. В обозревателе решений щелкните правой кнопкой мыши проект и выберите **публикации**. Выберите **профиль** и нажмите кнопку **веб-сайтов Microsoft Azure**. Если вы не вошли в настоящее время на Azure, будет предложено войти.

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

В **существующих веб-сайтов** диалоговое окно, нажмите кнопку **New**.

![](part-10/_static/image9.png)

Введите имя сайта. Выберите подписки Azure и регион. В разделе **сервера базы данных**выберите **создать новый сервер**, или выберите существующий сервер. Нажмите кнопку **Создать**.

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

Нажмите кнопку **параметры** вкладку и проверьте &quot;выполнять миграции Code First&quot;. Нажмите кнопку **публикации**.

> [!div class="step-by-step"]
> [Назад](part-9.md)
