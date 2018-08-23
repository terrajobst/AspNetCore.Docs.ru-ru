---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: Размещение ASP.NET Web API 2 в рабочей роли Azure | Документация Майкрософт
author: MikeWasson
description: Этом руководстве показано, как разместить веб-API ASP.NET в рабочей роли Azure, с помощью OWIN для резидентного размещения платформа веб-API. Откройте веб-интерфейс для .NET (OWIN) de...
ms.author: riande
ms.date: 04/02/2014
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: cabf88e4e6c946f92a9e4534a4db5ae15dd8cae5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829115"
---
<a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a>Размещение ASP.NET Web API 2 в рабочей роли Azure
====================
по [Майк Уоссон](https://github.com/MikeWasson)

> Этом руководстве показано, как разместить веб-API ASP.NET в рабочей роли Azure, с помощью OWIN для резидентного размещения платформа веб-API.
> 
> [Открыть веб-интерфейс .NET](http://owin.org/) (OWIN) определяет абстракции между веб-серверов .NET и веб-приложений. OWIN отделяет веб-приложения на сервер, который идеально OWIN для резидентного размещения веб-приложения в собственном процессе, за пределами IIS — например, внутри рабочей роли Azure.
> 
> В этом руководстве вы используете пакет Microsoft.Owin.Host.HttpListener, предоставляющую HTTP-сервер, который используется для резидентного размещения приложения OWIN.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемые в этом руководстве
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - Веб-API 2
> - [Пакет Azure SDK для .NET 2.3](https://azure.microsoft.com/downloads/)


## <a name="create-a-microsoft-azure-project"></a>Создание проекта Microsoft Azure

Запустите Visual Studio с правами администратора. Для отладки приложения в локальной среде, с помощью эмулятора вычислений Azure требуются права администратора.

На **файл** меню, щелкните **New**, затем нажмите кнопку **проекта**. Из **установленные шаблоны**, в разделе Visual C#, щелкните **Cloud** и нажмите кнопку **облачной службы Windows Azure**. Присвойте проекту имя «AzureApp» и нажмите кнопку **ОК**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

В **новая облачная служба Windows Azure** диалоговое окно, дважды щелкните **рабочей роли**. Оставьте имя по умолчанию («WorkerRole1»). Этот шаг добавляет рабочей роли в решение. Нажмите кнопку **ОК**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

Решение Visual Studio, который создается содержит два проекта:

- &quot;AzureApp&quot; определяет роли и конфигурации для приложения Azure.
- &quot;WorkerRole1&quot; содержит код для рабочей роли.

Как правило приложение Azure может содержать несколько ролей, несмотря на то, что в этом руководстве используется одна роль.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a>Добавьте веб-API и пакеты OWIN

Из **средства** меню, щелкните **диспетчер пакетов библиотеки**, затем нажмите кнопку **консоль диспетчера пакетов**.

В окне консоли диспетчера пакетов введите следующую команду:

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Добавить конечную точку HTTP

В обозревателе решений разверните проект AzureApp. Разверните узел роли, щелкните правой кнопкой мыши WorkerRole1 и выберите **свойства**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

Нажмите кнопку **конечные точки**, а затем нажмите кнопку **добавить конечную точку**.

В **протокола** раскрывающегося списка выберите «http». В **общий порт** и **частный порт**, введите 80. Эти номера портов могут отличаться. Общий порт — новые клиенты будут использовать при отправке запроса к роли.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a>Настройка веб-API для резидентного размещения

В обозревателе решений щелкните правой кнопкой мыши проект WorkerRole1 и выберите **добавить** / **класс** для добавления нового класса. Присвойте классу имя `Startup`.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

Замените весь код шаблона в этом файле следующим кодом:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a>Добавить контроллер веб-API

Добавьте класс контроллера веб-API. Щелкните правой кнопкой мыши проект WorkerRole1 и выберите **добавить** / **класс**. Имя класса TestController. Замените весь код шаблона в этом файле следующим кодом:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

Для простоты этот контроллер определяет только два метода GET, возвращающие обычного текста.

## <a name="start-the-owin-host"></a>Запустить узел OWIN

Откройте файл WorkerRole.cs. Этот класс определяет код, выполняемый при рабочей роли запущено и остановлено.

Добавьте следующий оператор using:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

Добавить **IDisposable** члена `WorkerRole` класса:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

В `OnStart` метод, добавьте следующий код для запуска узла:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

**WebApp.Start** метод начинает хоста OWIN. Имя `Startup` класс является параметром типа метода. По соглашению, главное приложение выполняет вызов `Configure` метод этого класса.

Переопределить `OnStop` для удаления  *\_приложения* экземпляр:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

Ниже приведен полный код для WorkerRole.cs:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

Выполните сборку решения и нажмите клавишу F5 для запуска приложения локально в эмуляторе вычислений Azure. В зависимости от настроек брандмауэра может потребоваться предоставление эмулятору через брандмауэр.

> [!NOTE]
> Если вы получаете исключение, как показано ниже, см. раздел [этой записи блога](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) решение этой проблемы. «Не удалось загрузить файл или сборку "Microsoft.Owin, Version = 2.0.2.0, язык и региональные параметры = neutral, PublicKeyToken = 31bf3856ad364e35" или одну из ее зависимостей. Определение манифеста расположены сборки не соответствует ссылки на сборку. (Исключение из HRESULT: 0x80131040)»


Эмулятор вычислений назначает локальный IP-адрес конечной точки. IP-адрес можно найти, просмотрев пользовательский Интерфейс эмулятора вычислений. Щелкните правой кнопкой мыши значок эмулятора в задаче области уведомлений и выберите **Показать пользовательский Интерфейс эмулятора вычислений**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

Найти IP-адрес в группе развертывания служб, развертывания [id], сведения о службе. Откройте веб-браузер и перейдите на http://<em>адрес</em>/test/1, где <em>адрес</em> IP-адрес, назначенный с помощью эмулятора вычислений; например, `http://127.0.0.1:80/test/1`. Вы увидите ответ от контроллера веб-API:

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a>Развертывание в Azure

Для выполнения этого шага необходимо иметь учетную запись Azure. Если ее еще нет, можно создать бесплатную пробную учетную запись всего за несколько минут. Дополнительные сведения см. в разделе [бесплатной пробной версии Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

В обозревателе решений щелкните правой кнопкой мыши проект AzureApp. Нажмите **Публиковать**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

Если вы не вошли учетную запись Azure, щелкните **Sign In**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

После входа, выберите подписку и нажмите кнопку **Далее**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

Введите имя для облачной службы и выбрать регион. Нажмите кнопку **Создать**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

Нажмите кнопку **Опубликовать**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

Окно журнала действий Azure показывает ход выполнения развертывания. При развертывании приложения, перейдите к http://appname.cloudapp.net/test/1.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Обзор проекта Katana](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [Проект Katana на GitHub](https://github.com/aspnet/AspNetKatana)
