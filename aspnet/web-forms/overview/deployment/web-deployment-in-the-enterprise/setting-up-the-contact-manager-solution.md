---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
title: "Настройка диспетчера контактов решения | Документы Microsoft"
author: jrjlee
description: "В этом разделе описывается загрузка и настройка решения диспетчера контактов для локального выполнения на рабочей станции разработчика."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 200b973c-776b-4a9b-9e82-39fda6120a52
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: b8176b3b8622e21187a91647323322e55582373c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="setting-up-the-contact-manager-solution"></a>Настройка решения диспетчера контактов
====================
по [Джейсон Lee](https://github.com/jrjlee)

[Скачать PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> В этом разделе описывается загрузка и настройка решения диспетчера контактов для локального выполнения на рабочей станции разработчика.


## <a name="system-requirements"></a>Требования к системе

Для локального запуска решения диспетчера контактов и выполнять другие задачи, описанные в этом учебнике, необходимо будет установить это программное обеспечение на рабочей станции разработчика:

- Visual Studio 2010 с пакетом 1, Premium или Ultimate Edition
- Internet Information Services (IIS) 7.5 Express
- SQL Server Express 2008 R2
- IIS средство веб-развертывания (Web Deploy) 2.1 или более поздней версии
- ASP.NET 4.0
- ASP.NET MVC 3
- .NET Framework 4
- .NET Framework 3.5 SP1

За исключением Visual Studio 2010 можно загрузить и установить последние версии всех этих продуктов и компонентов с помощью [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).

## <a name="download-and-extract-the-solution"></a>Загрузите и извлеките решения

Можно загрузить пример приложения диспетчера контактов из коллекции кода MSDN [здесь](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).

## <a name="configure-and-run-the-solution"></a>Настройка и запуск решения

Для настройки и запуска диспетчера контактов решения на локальном компьютере, необходимо выполнить следующие общие действия:

1. Если у еще его нет, создание локальной базы данных служб приложения ASP.NET с помощью функции управления членством и ролями, доступные.
2. Изменение строк подключения в *web.config* файлы, чтобы она указывала на локальном экземпляре SQL Server Express.
3. Запустите решение из Visual Studio 2010.

Оставшаяся часть этого подраздела предоставляет дополнительные рекомендации о том, как выполнить каждую из этих задач.

**Для создания базы данных служб приложений**

1. Откройте командную строку Visual Studio 2010. Для этого на **запустить** последовательно выберите пункты **все программы**, нажмите кнопку **Microsoft Visual Studio 2010**, нажмите кнопку **набора средств Visual Studio**, а затем Нажмите кнопку **командной строки Visual Studio (2010)**.
2. В командной строке введите следующую команду и нажмите клавишу ВВОД:

    [!code-console[Main](setting-up-the-contact-manager-solution/samples/sample1.cmd)]

    1. Используйте **-C** коммутатора, чтобы указать строку подключения для сервера базы данных.
    2. Используйте **–A** для указания функций, которые требуется добавить в базу данных служб приложения. В этом случае **m** указывает, что для добавления поддержки поставщик членства и **r** указывает, что вы хотите добавить поддержку для диспетчера ролей.
    3. Используйте **– d** коммутатора, чтобы указать имя для базы данных приложения службы. Если параметр не задан, программа создаст базу данных с именем по умолчанию **aspnetdb**.
3. Если база данных успешно создана, командной строки будут выведены подтверждения.

    ![](setting-up-the-contact-manager-solution/_static/image1.png)

> [!NOTE]
> Дополнительные сведения о aspnet\_regsql программы, в разделе [средство регистрации ASP.NET SQL Server (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).


Следующим шагом является убедитесь в том, что строки подключения в решении диспетчера контактов указывают на локальном экземпляре SQL Server Express.

**Чтобы обновить строки соединения**

1. Откройте Диспетчер контактов решение в Visual Studio 2010.
2. В **обозревателе решений** окна, разверните **ContactManager.Mvc** проекта, а затем дважды щелкните **Web.config** узла.

    > [!NOTE]
    > ContactManager.Mvc проект содержит две *web.config* файлов. Необходимо изменить файл проекта.

    ![](setting-up-the-contact-manager-solution/_static/image2.png)
3. В **connectionStrings** элемент, убедитесь, что строка подключения с именем **ApplicationServices** указывает на локальной базы данных служб приложения ASP.NET.

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample2.xml)]
4. В **обозревателе решений** окна, разверните **ContactManager.Service** проекта, а затем дважды щелкните **Web.config** узла.

    ![](setting-up-the-contact-manager-solution/_static/image3.png)
5. В **connectionStrings** элемент в строке подключения с именем **ContactManagerContext**, убедитесь, что **источника данных** свойству локального экземпляра SQL Экспресс-выпуск сервера. Нет необходимости изменять что-либо еще в строке подключения.

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample3.xml)]
6. Сохраните все открытые файлы.

Теперь можно запустить диспетчер контактов решение на локальном компьютере.

> [!NOTE]
> Если без предварительного создания базы данных служб приложения, выполните следующие действия, ASP.NET создаст базу данных при первом создании пользователя. Тем не менее вручную создав базу данных предоставляет гораздо больший контроль над набора средств служб приложения, которые требуется поддерживать.


**Чтобы запустить решение диспетчера контактов**

1. В Visual Studio 2010 нажмите клавишу F5.
2. Internet Explorer запускается и запрашивает URL-адрес приложения, обратитесь в службу диспетчера ASP.NET MVC 3. По умолчанию приложение отображает **все контакты** страницы.

    ![](setting-up-the-contact-manager-solution/_static/image4.png)
3. Добавьте несколько контактов и убедитесь, что приложение работает правильно.

    ![](setting-up-the-contact-manager-solution/_static/image5.png)
4. Перейдите к `http://localhost:50114/Account/Register` (Если вы используете приложение на другой порт, измените URL-адрес). Добавьте имя пользователя, адрес электронной почты и пароль и убедитесь, что вы можете зарегистрировать учетную запись успешно.

    ![](setting-up-the-contact-manager-solution/_static/image6.png)
5. Перейдите к `http://localhost:50114/Account/LogOn` (Если вы используете приложение на другой порт, измените URL-адрес). Убедитесь, что вы можете выполнить вход с помощью учетной записи, которую вы только что создали.

    ![](setting-up-the-contact-manager-solution/_static/image7.png)
6. Закройте Internet Explorer, чтобы остановить отладку.

## <a name="conclusion"></a>Заключение

На этом этапе диспетчера контактов решения должны быть полностью настроены для запуска на локальном компьютере. При работе через другие подразделы в этом учебнике, можно использовать решение как ссылка.

Следующий раздел [основные сведения о файле проекта](understanding-the-project-file.md), объясняется, как можно использовать пользовательские файлы проекта Microsoft Build Engine (MSBuild) в рамках решения диспетчера контактов для управления процессом развертывания.

>[!div class="step-by-step"]
[Назад](the-contact-manager-solution.md)
[Вперед](understanding-the-project-file.md)
