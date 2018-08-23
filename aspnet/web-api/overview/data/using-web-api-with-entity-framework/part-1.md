---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Использование веб-API 2 с Entity Framework 6 | Документация Майкрософт
author: MikeWasson
description: Этот учебник поможет, что основы создания веб-приложения с помощью ASP.NET Web API серверной части. В этом руководстве используется Entity Framework 6 для макета данных...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 4abe0e06dfd927765efd8e566584e111cf4117d5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829000"
---
<a name="using-web-api-2-with-entity-framework-6"></a>Использование веб-API 2 с Entity Framework 6
====================
по [Майк Уоссон](https://github.com/MikeWasson)

[Скачать завершенный проект](https://github.com/MikeWasson/BookService)

> Этот учебник поможет, что основы создания веб-приложения с помощью ASP.NET Web API серверной части. В руководстве используется Entity Framework 6 для уровня данных и Knockout.js для клиентского приложения JavaScript. Этом руководстве также показано, как развернуть приложение в веб-приложениях службы приложений Azure.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемые в этом руководстве
> 
> 
> - Веб-API 2.1
> - [Visual Studio 2013 с обновлением 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> - [Knockout.js](http://knockoutjs.com/) 3.1


Этом руководстве используется ASP.NET Web API 2 с Entity Framework 6 создание веб-приложения, который управляет серверной базы данных. Ниже приведен снимок экрана приложения, который будет создан.

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

Приложение использует разработки одностраничное приложение (SPA). «Одностраничного приложения» — это общий термин для веб-приложения, который загружает единственную HTML-страницу и затем обновляет страницу динамически, вместо загрузки новых страниц. После загрузки начальной страницы приложение взаимодействует с сервером с помощью запросов AJAX. AJAX запрашивает возвращаемых данных JSON, которые приложение использует для обновления пользовательского интерфейса.

AJAX – не Новинка, но на сегодняшний день существует платформ JavaScript, которые упрощают создание и обслуживание большого сложные приложения SPA. В этом руководстве используется [Knockout.js](http://knockoutjs.com/), но вы можете использовать любую платформу клиента JavaScript.

Ниже приведены основные строительные блоки для этого приложения.

- ASP.NET MVC создает HTML-страницы.
- Веб-API ASP.NET обрабатывает запросы AJAX и возвращает данные JSON.
- Knockout.js привязывает данные HTML-элементов для данных JSON.
- Платформа Entity Framework обращается к базе данных.

## <a name="see-this-app-running-on-azure"></a>Это приложение работает в Azure см. в разделе

Вы действительно хотите см. по завершении сайт, запущенный в качестве активного веб-приложения? Полную версию приложения можно развернуть учетную запись Azure, просто нажав кнопку ниже.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

Требуется учетная запись Azure для развертывания этого решения в Azure. Если у вас еще нет учетной записи, у вас есть следующие параметры:

- [Открыть учетную запись Azure бесплатно](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -вы получаете кредиты можно использовать для опробования платных служб Azure и даже в том случае, если они используются, вы сохраняете учетную запись и использовать бесплатные службы Azure.
- [Активировать преимущества подписчика MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -ваша подписка MSDN предоставляет вам кредиты каждый месяц, который можно использовать для оплаты служб Azure.

## <a name="create-the-project"></a>Создание проекта

Запустите Visual Studio. Из **файл** меню, выберите **New**, а затем выберите **проекта**. (Или нажмите кнопку **новый проект** на начальной странице.)

В **новый проект** диалоговое окно, нажмите кнопку **Web** в левой области и **веб-приложение ASP.NET** в средней области. Назовите проект BookService и нажмите кнопку **ОК**.

[![](part-1/_static/image4.png)](part-1/_static/image3.png)

В **новый проект ASP.NET** диалоговом окне выберите **веб-API** шаблона.

[![](part-1/_static/image6.png)](part-1/_static/image5.png)

Если вы хотите разместить проект в службе приложений Azure, оставьте **разместить в облаке** флажком.

Нажмите кнопку **ОК**, чтобы создать проект.

## <a name="configure-azure-settings-optional"></a>Настройте параметры Azure (необязательно)

Если оставить **узел в облаке** флажок установлен, Visual Studio будет предложено войти в Microsoft Azure

[![](part-1/_static/image8.png)](part-1/_static/image7.png)

После входа в Azure, Visual Studio предложит Настройка веб-приложения. Введите имя для сайта, выберите свою подписку Azure и выберите географический регион. В разделе **сервер базы данных**выберите **создать сервер**. Введите имя пользователя администратора и пароль.

[![](part-1/_static/image10.png)](part-1/_static/image9.png)

> [!div class="step-by-step"]
> [Вперед](part-2.md)
