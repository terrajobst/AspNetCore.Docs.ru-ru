---
uid: mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
title: Как обновлять ASP.NET MVC 4 и веб-API проекта ASP.NET MVC 5 и веб-API 2 | Документация Майкрософт
author: Rick-Anderson
description: ASP.NET MVC 5 и веб-API 2 перевести множество новых функций, включая маршрутизации с помощью атрибутов, фильтры проверки подлинности и многое другое.
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: db0d02d9-58e8-4a0b-8d7d-b8df8ea97b88
msc.legacyurl: /mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 8a50c188a2283af46789739e710be69159799695
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826748"
---
<a name="how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2"></a>Обновление ASP.NET MVC 4 и проекта веб-API до ASP.NET MVC 5 и веб-API 2
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

> ASP.NET MVC 5 и веб-API 2 перевести множество новых функций, включая маршрутизации с помощью атрибутов, фильтры проверки подлинности и многое другое. См. в разделе [ https://www.asp.net/vnext ](https://www.asp.net/core) для получения дополнительных сведений.
> 
> В этом пошаговом руководстве поможет вам с шаги, необходимые для обновления до последней версии приложения.  
> 
> > [!NOTE]
> > См. в разделе [ASP.NET and Web Tools для заметки о выпуске Visual Studio 2013](../../../visual-studio/overview/2013/release-notes.md) сведения о критических изменениях из MVC 4 и веб-API до следующей версии.
> 
>   
> 
> Эта статья написана, Гонконг Youngjune и Рик Андерсон ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )


## <a name="upgrade-steps"></a>Действия по обновлению

1. Создайте резервную копию проекта. В этом пошаговом руководстве потребуется внести изменения в файл проекта, конфигурацию пакета и файлов web.config.
2. Для обновления с веб-API для веб-API 2 в файле global.asax, измените:

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample1.cs)]

   в

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample2.cs)]
3. Убедитесь, что все пакеты, использующие проекты совместимы с MVC 5 и веб-API 2. Ниже показано в таблице MVC 4 и веб-API, связанные с пакетами не должны быть изменены. Если у вас есть пакет, который зависит от одной из перечисленных ниже пакетов, обратитесь в службу издателей, чтобы получить более новых версий, совместимых с MVC 5 и веб-API 2. Если у вас есть исходный код для этих пакетов, их следует перекомпилировать с новыми сборками MVC 5 и веб-API 2.   

    | **Идентификатор пакета** | **Старая версия** | **Новая версия** |
    | --- | --- | --- |
    | Microsoft.AspNet.Razor | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.WebPages | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.WebPages.WebData | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.WebPages.OAuth | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.Mvc | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.Mvc.Facebook | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.Core | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.SelfHost | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.Client | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.OData | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.WebHost | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.Tracing | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.HelpPage | 4.0.x.x | 5.0.0 |
    | Microsoft.Net.Http | 2.0.x. | 2.2.x. |
    | Microsoft.Data.OData | 5.2.x | 5.6.x |
    | System.Spatial | 5.2.x | 5.6.x |
    | Microsoft.Data.Edm | 5.2.x | 5.6.x |
    | Microsoft.AspNet.Mvc.FixedDisplayModes | < o:p >< / o:p > | Удаленная |
    | Microsoft.AspNet.WebPages.Administration | < o:p >< / o:p > | Удаленная |
    | Microsoft Web вспомогательные функции. | < o:p >< / o:p > | Microsoft.AspNet.WebHelpers |

    > [!NOTE]
    > Microsoft Web вспомогательные методы были заменены Microsoft.AspNet.WebHelpers. Сначала удалите старый пакет и затем установить новый пакет.   
    >   
    > Есть совместимость не перекрестного версии среди основных пакетов ASP.NET. Например MVC 5 совместим с только Razor 3, а не 2 Razor.
4. Откройте проект в Visual Studio 2013.
5. Удалите любые из следующих пакетов ASP.NET NuGet, которые установлены. С помощью консоли диспетчера пакетов (PMC) будут удалены. Чтобы открыть консоль диспетчера пакетов, выберите **средства** меню и выберите **диспетчер пакетов библиотеки,** выберите **консоль диспетчера пакетов**. Проект может включать не все из них.

    1. `Microsoft.AspNet.WebPages.Administration`  
   Этот пакет обычно добавляется при обновлении MVC 3 до MVC 4. Чтобы удалить его, выполните следующую команду в PMC:  
        `Uninstall-Package -Id Microsoft.AspNet.WebPages.Administration`
    2. `Microsoft-Web-Helpers`   
   Этот пакет изменено на `Microsoft.AspNet.WebHelpers`. Чтобы удалить его, выполните следующую команду в PMC:  
        `Uninstall-Package -Id Microsoft-Web-Helpers`
    3. `Microsoft.AspNet.Mvc.FixedDisplayMode`  
   Этот пакет содержит решения для ошибки в MVC 4, которая была исправлена в MVC 5. Чтобы удалить его, выполните следующую команду в PMC:  
        `Uninstall-Package -Id Microsoft.AspNet.Mvc.FixedDisplayModes`
6. Обновите все пакеты ASP.NET NuGet, используя консоль диспетчера пакетов. В PMC выполните следующую команду:  
    `Update-Package`  
   `Update-Package` Команду без параметров будет обновлять каждого пакета. Пакеты можно обновить по отдельности, используя аргумент идентификатора. Дополнительные сведения о команде обновления выполняют `get-help update-package` .

## <a name="update-the-application-webconfig-file"></a>Обновить приложение *web.config* файла

Убедитесь, что для внесения этих изменений в приложении *web.config* запись в файл, *web.config* файл *представления* папки.

Найдите `<runtime>/<assemblyBinding>` раздела и внесите следующие изменения:

1. В элементы с атрибутом имени «System.Web.Mvc» измените номер версии «4.0.0.0» до «5.0.0.0». (Два изменения в этом элементе.)
2. В элементах с атрибутом имени &quot;System.Web.Helpers» и &quot;System.Web.WebPages&quot; изменить номер версии из «2.0.0.0» до «3.0.0.0». Четыре будут внесены изменения, в каждом элементе.

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample3.xml?highlight=6,10,14)]
3. Найдите `<appSettings>` раздела и обновить webpages:version из 2.0.0.0.0 для 3.0.0.0, как показано ниже:

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample4.xml?highlight=2)]
4. Удалите все уровни доверия, отличные от полного. Пример:

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample5.xml?highlight=2)]

## <a name="update-the-webconfig-files-under-the-views-folder"></a>Обновление *web.config* файлы в папке представления

Если приложение использует областей, также необходимо будет обновить каждый *web.config* файл *представления* вложенной папке каждой папке области.

1. Обновите все элементы, содержащие «System.Web.Mvc» версии «4.0.0.0» до версии «5.0.0.0».  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample6.xml?highlight=2)]

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample7.xml?highlight=4-6,8)]
2. Обновите все элементы, содержащие «System.Web.WebPages.Razor» версии «2.0.0.0» до версии «3.0.0.0». Если этот раздел содержит «System.Web.WebPages», обновите эти элементы из версии «2.0.0.0» до версии «3.0.0.0»  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample8.xml?highlight=3-5)]
3. Если вы удалили `Microsoft-Web-Helpers` установить пакет NuGet на предыдущем шаге, `Microsoft.AspNet.WebHelpers` , выполнив следующую команду в PMC:  
    `Install-Package -Id Microsoft.AspNet.WebHelpers`
4. Если приложение использует [User.IsInRole()](https://msdn.microsoft.com/en-us/library/system.web.security.roleprincipal.isinrole(v=vs.110).aspx) метод, добавьте следующий код в *Web.config* файл.

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample9.xml)]

## <a name="final-steps"></a>Заключительные действия

Сборка и тестирование приложения.

Удалите идентификатор GUID типа проекта MVC 4 из файлов проекта.

1. В обозревателе решений щелкните правой кнопкой мыши имя проекта и выберите **выгрузить проект**.
2. Щелкните правой кнопкой мыши проект и выберите команду Изменить имя_проекта.csproj.
3. Найдите `ProjectTypeGuids` элемент, а затем идентификатор GUID проекта MVC 4 remove `{E3E379DF-F4C6-4180-9B81-6769533ABE47}`.
4. Сохраните и закройте файл открыть проект.
5. Щелкните правой кнопкой мыши проект и выберите **перезагрузить проект**.
