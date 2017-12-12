---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: "Размещения ASP.NET Web API 2 в рабочей роли Azure | Документы Microsoft"
author: MikeWasson
description: "Этого учебника показано, как разместить веб-API ASP.NET в рабочей роли Azure, с помощью OWIN для самостоятельного размещения платформа веб-API. Открыть веб-интерфейс .NET (OWIN) de..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/02/2014
ms.topic: article
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 326c4a4e274dbc1aa6e09f1d07c4d135e4304484
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a>Размещения ASP.NET Web API 2 в рабочей роли Azure
====================
по [Mike Wasson](https://github.com/MikeWasson)

> Этого учебника показано, как разместить веб-API ASP.NET в рабочей роли Azure, с помощью OWIN для самостоятельного размещения платформа веб-API.
> 
> [Открыть веб-интерфейс .NET](http://owin.org/) (OWIN) определяет абстракцию между .NET веб-серверов и веб-приложений. OWIN разрывает связь веб-приложения с сервера, что делает OWIN идеальной для резидентного размещения веб-приложения в свой собственный процесс, за пределами служб IIS — например, внутри рабочей роли Azure.
> 
> В этом учебнике будет использоваться Microsoft.Owin.Host.HttpListener пакета, который предоставляет HTTP-сервер используется для резидентного размещения приложения OWIN.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемая в этом учебнике
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - Веб-API 2
> - [Azure SDK для .NET 2.3](https://azure.microsoft.com/en-us/downloads/)


## <a name="create-a-microsoft-azure-project"></a>Создание проекта Microsoft Azure

Запустите Visual Studio с правами администратора. Для отладки приложения локально, с помощью эмулятора вычислений Azure требуются права администратора.

На **файл** меню, нажмите кнопку **New**, нажмите кнопку **проекта**. Из **установленные шаблоны**, в Visual C#, нажмите кнопку **облака** и нажмите кнопку **облачных служб Windows Azure**. Назовите проект «AzureApp» и нажмите кнопку **ОК**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

В **новой облачной службы Windows Azure** диалоговое окно, дважды щелкните **рабочей роли**. Оставьте имя по умолчанию («WorkerRole1»). Этот шаг добавляет рабочей роли в решение. Нажмите кнопку **ОК**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

Решение Visual Studio, которая создается содержит два проекта:

- &quot;AzureApp&quot; определяет роли и конфигурации для приложения Azure.
- &quot;WorkerRole1&quot; содержит код для рабочей роли.

Как правило приложение Azure может содержать несколько ролей, несмотря на то, что в этом учебнике используется одна роль.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a>Добавление веб-API и пакеты OWIN

Из **средства** меню, нажмите кнопку **диспетчер пакетов библиотеки**, нажмите кнопку **консоль диспетчера пакетов**.

В окне консоли диспетчера пакетов введите следующую команду:

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Добавить конечную точку HTTP

В обозревателе решений разверните проект AzureApp. Разверните узел роли, щелкните правой кнопкой мыши WorkerRole1 и выберите **свойства**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

Нажмите кнопку **конечные точки**, а затем нажмите кнопку **добавить конечную точку**.

В **протокола** раскрывающемся списке выберите «http». В **общий порт** и **частный порт**, введите 80. Эти номера портов могут быть разными. Открытый порт является то, что клиенты будут использовать при отправке запроса к роли.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a>Настройка веб-API для резидентного размещения

В обозревателе решений щелкните правой кнопкой мыши проект WorkerRole1 и выберите **добавить** / **класса** для добавления нового класса. Присвойте классу имя `Startup`.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

Замените весь код шаблона в этом файле следующим кодом:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a>Добавить контроллер Web API

Добавьте класс контроллера веб-API. Щелкните правой кнопкой мыши проект WorkerRole1 и выберите **добавить** / **класса**. Имя класса TestController. Замените весь код шаблона в этом файле следующим кодом:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

Для простоты этот контроллер просто определяет два метода GET, которые возвращают обычного текста.

## <a name="start-the-owin-host"></a>Запустить узел OWIN

Откройте файл WorkerRole.cs. Этот класс определяет код, который выполняется при остановке и запуске рабочей роли.

Добавьте следующий код с помощью инструкции:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

Добавить **IDisposable** члена `WorkerRole` класса:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

В `OnStart` метода, добавьте следующий код для запуска узла:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

**WebApp.Start** метод запускает узел OWIN. Имя `Startup` класса является параметром типа метода. По соглашению, узел будет вызывать `Configure` метода этого класса.

Переопределить `OnStop` для удаления  *\_приложения* экземпляр:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

Ниже приведен полный код для WorkerRole.cs:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

Выполните сборку решения и нажмите клавишу F5 для запуска приложения локально в эмуляторе вычислений Azure. В зависимости от настроек брандмауэра может потребоваться разрешить эмулятор через брандмауэр.

> [!NOTE]
> Если вы получаете исключение, как показано ниже, см. в разделе [этой записи блога](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) для решения этой проблемы. «Не удалось загрузить файл или сборку ' Microsoft.Owin, версия = 2.0.2.0, язык и региональные параметры = neutral, PublicKeyToken = 31bf3856ad364e35 "или одну из ее зависимостей. Определение расположения сборки манифеста не соответствует ссылке сборки. (Исключение из HRESULT: 0x80131040)»


Эмулятор вычислений назначает локальный IP-адрес конечной точки. IP-адрес можно найти, просмотрев пользовательский Интерфейс эмулятора вычислений. Щелкните правой кнопкой мыши значок эмулятора в задаче панели области уведомлений и выберите **Показать пользовательский Интерфейс эмулятора вычислений**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

Поиск IP-адреса в развертываниях службы развертывания [id], сведения о службе. Откройте веб-браузер и перейдите к http://*адрес*/test/1, где *адрес* IP-адрес, назначенный эмулятор вычислений; например, `http://127.0.0.1:80/test/1`. Вы увидите ответа от контроллера веб-API:

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a>Развертывание в Azure

Для выполнения этого шага необходимо иметь учетную запись Azure. Если у вас еще нет один, можно создать бесплатную пробную учетную запись всего за несколько минут. Дополнительные сведения см. в разделе [бесплатная пробная версия Microsoft](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A261C142F).

В обозревателе решений щелкните правой кнопкой мыши проект AzureApp. Нажмите **Публиковать**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

Если вы не вошли учетную запись Azure, щелкните **входа**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

После входа, выберите подписку и нажмите кнопку **Далее**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

Введите имя для облачной службы и выберите область. Нажмите кнопку **Создать**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

Нажмите кнопку **Опубликовать**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

В окне Журнал действий Azure отображается ход выполнения развертывания. При развертывании приложения, перейдите к http://appname.cloudapp.net/test/1.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Общие сведения о проекте Katana](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [Katana проекта на GitHub](https://github.com/aspnet/AspNetKatana)
