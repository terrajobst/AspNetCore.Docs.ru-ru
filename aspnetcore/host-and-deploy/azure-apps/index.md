---
title: Размещение ASP.NET Core в службе приложений Azure
author: guardrex
description: Узнайте, как размещать приложения ASP.NET Core в службе приложений Azure, с помощью ссылок на полезные ресурсы.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: c2675f73880a41ee75f6ec13155419945387e109
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
# <a name="host-aspnet-core-on-azure-app-service"></a>Размещение ASP.NET Core в службе приложений Azure

[Служба приложений Azure](https://azure.microsoft.com/services/app-service/) — это [платформа облачных вычислений Microsoft](https://azure.microsoft.com/), предназначенная для размещения веб-приложений, включая ASP.NET Core.

## <a name="useful-resources"></a>Полезные ресурсы

[Документация по веб-приложениям](/azure/app-service/) Azure — это место, где хранятся документация, учебники, примеры, руководства и другие ресурсы, связанные с приложениями Azure. Размещению приложений ASP.NET Core посвящены следующие два руководства.

[Краткое руководство. Создание веб-приложения ASP.NET Core в Azure](/azure/app-service/app-service-web-get-started-dotnet)  
Создайте веб-приложение ASP.NET Core и разверните его в службе приложений Azure на базе Windows с помощью Visual Studio.

[Краткое руководство. Создание веб-приложения .NET Core в службе приложений на базе Linux](/azure/app-service/containers/quickstart-dotnetcore)  
Создайте веб-приложение ASP.NET Core и разверните его в службе приложений Azure на базе Linux с помощью командной строки.

Следующие статьи входят в документацию по ASP.NET Core.

[Опубликовать в Azure с помощью Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs)  
Сведения о публикации приложения ASP.NET Core в службе приложений Azure с помощью Visual Studio.

[Публикация в Azure с инструментами командной строки](xref:tutorials/publish-to-azure-webapp-using-cli)  
Сведения о публикации приложения ASP.NET Core в службе приложений Azure с помощью клиента командной строки Git.

[Непрерывное развертывание в Azure с помощью Visual Studio и Git](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
Сведения о создании веб-приложения ASP.NET Core с помощью Visual Studio и его развертывании в службе приложений Azure с использованием Git для непрерывного развертывания.

[Непрерывное развертывание в Azure с помощью VSTS](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)  
Сведения о настройке сборки CI для приложения ASP.NET Core и последующем создании выпуска непрерывного развертывания в службе приложений Azure.

[Песочница веб-приложений Azure](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
Сведения об ограничениях среды выполнения службы приложений Azure, создаваемых платформой приложений Azure.

## <a name="application-configuration"></a>Настройка приложения

При использовании ASP.NET Core 2.0 и более поздних версий три пакета в составе [метапакета Microsoft.AspNetCore.All](xref:fundamentals/metapackage) предоставляют функции автоматического ведения журналов для приложений, развернутых в службе приложений Azure:

* [Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) использует [IHostingStartup](xref:host-and-deploy/platform-specific-configuration) для интеграции подсветки ASP.NET Core в службу приложений Azure. Пакет `Microsoft.AspNetCore.AzureAppServicesIntegration` включает дополнительные функции для ведения журналов.
* [Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) выполняет [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) для добавления поставщиков ведения журналов диагностики из службы приложений Azure в пакет `Microsoft.Extensions.Logging.AzureAppServices`.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) предоставляет реализации средства ведения журналов в поддержку журналов диагностики службы приложений Azure и функций потоковой передачи журналов.

## <a name="proxy-server-and-load-balancer-scenarios"></a>Сценарии использования прокси-сервера и подсистемы балансировки нагрузки

ПО промежуточного слоя для интеграции IIS, которое настраивает ПО промежуточного слоя переадресации заголовков, и модуль ASP.NET Core настраиваются на пересылку схемы (HTTP/HTTPS) и удаленного IP-адреса расположения, где был сформирован запрос. Для приложений, размещенных за дополнительными прокси-серверами и подсистемами балансировки нагрузки, может потребоваться дополнительная настройка. Дополнительные сведения см. в разделе [Настройка ASP.NET Core для работы с прокси-серверами и подсистемами балансировки нагрузки](xref:host-and-deploy/proxy-load-balancer).

## <a name="monitoring-and-logging"></a>Мониторинг и ведение журналов

Сведения о мониторинге, ведении журналов, а также поиске и устранении неполадок см. в следующих статьях.

[Мониторинг приложений в службе приложений Azure](/azure/app-service/web-sites-monitor)  
Сведения о том, как толковать квоты и параметры для приложений и планы службы приложений.

[Включение функции ведения журналов диагностики для веб-приложений в службе приложений Azure](/azure/app-service/web-sites-enable-diagnostic-log)  
Сведения о том, как включать и где искать функцию ведения журнала диагностики для кодов статуса HTTP, невыполненных запросов и активности веб-сервера.

[Общие сведения об обработке ошибок в ASP.NET Core](xref:fundamentals/error-handling)  
Сведения об основных методах обработки ошибок в приложениях ASP.NET Core.

[Устранение неполадок ASP.NET Core в службе приложений Azure](xref:host-and-deploy/azure-apps/troubleshoot)  
Сведения о диагностике проблем с развертыванием приложений ASP.NET Core в службе приложений Azure.

[Справочник по общим ошибкам для службы приложений Azure и служб IIS с ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)  
См. распространенные ошибки с конфигурацией развертывания для приложений, размещенных в службе приложений Azure/IIS, и рекомендации по их устранению.

## <a name="data-protection-key-ring-and-deployment-slots"></a>Связка ключей для защиты данных и слоты развертывания

[Ключи для защиты данных](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) хранятся в папке *%HOME%\ASP.NET\DataProtection-Keys*. Эта папка копируется в сетевое хранилище и синхронизируется на всех машинах, где размещается приложение. Во время хранения ключи не защищаются. В этой папке хранится связка ключей для всех экземпляров приложения в одном и том же слоте развертывания. Отдельные слоты развертывания, такие как промежуточное хранение и производство, не используют общую связку ключей.

При переключении между разными слотами развертывания ни одна система с использованием защиты данных не сможет расшифровать хранимые данные, используя связку ключей из предыдущего слота. Для защиты файлов cookie ПО промежуточного слоя ASP.NET Cookie использует защиту данных. В результате пользователей, применяющих ПО промежуточного слоя ASP.NET Cookie, выбрасывает из приложения. Для того чтобы решение связки ключей не зависело от слота, используйте внешнего поставщика связки ключей, например:

* Хранилище больших двоичных объектов Azure;
* Хранилище ключей Azure;
* Хранилище SQL;
* Кэш Redis.

Дополнительные сведения см. в разделе [Поставщики хранилища ключей](xref:security/data-protection/implementation/key-storage-providers).

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a>Развертывание предварительной версии ASP.NET Core в службе приложений Azure

Предварительную версию приложения ASP.NET Core можно развернуть в службе приложений Azure, используя следующие методы.

* [Установка расширения сайта предварительной версии](#site-x)
* [Автономное развертывание приложения](#self)
* [Использование Docker с веб-приложениями для контейнеров](#docker)

Если у вас возникли проблемы при использовании расширения сайта предварительной версии, сообщите о них на [GitHub](https://github.com/aspnet/azureintegration/issues/new).

<a name="site-x"></a>
### <a name="install-the-preview-site-extention"></a>Установка расширения сайта предварительной версии

* На портале Azure перейдите к колонке "Служба приложений".
* Введите "ex" в поле поиска.
* Выберите **Расширения**.
* Выберите "Добавить".

![Колонка "Приложение Azure" с предыдущими шагами](index/_static/x1.png)

* Выберите **Расширения среды выполнения ASP.NET Core**.
* Нажмите **ОК** > **ОК**.

Когда операции добавления будут выполнены, будет установлена последняя предварительная версия .NET Core 2.1. Чтобы проверить установку, запустите `dotnet --info` в консоли. В колонке "Служба приложений":

* Введите "con" в поле поиска.
* Выберите **Консоль**.
* Введите `dotnet --info` в консоли.

![Колонка "Приложение Azure" с предыдущими шагами](index/_static/cons.png)

Это изображение было актуальным на момент написания статьи. Вы можете увидеть другую версию.

`dotnet --info` отображает путь к расширению сайта, где установлена предварительная версия. Он показывает, что приложение выполняется из расширения сайта, а не из расположения *ProgramFiles* по умолчанию. Если вы видите *ProgramFiles*, перезапустите сайт и выполните `dotnet --info`.

#### <a name="use-the-preview-site-extention-with-an-arm-template"></a>Использование расширения сайта предварительного просмотра с шаблоном ARM

Если вы используете шаблон ARM для создания и развертывания приложений, вы можете использовать тип ресурса `siteextensions`, чтобы добавить расширение сайта в веб-приложение. Пример:

[!code-json[Main](index/sample/arm.json?highlight=2)]

<a name="self"></a>
### <a name="deploy-the-app-self-contained"></a>Автономное развертывание приложения

Вы можете развернуть [автономное приложение](/dotnet/core/deploying/#self-contained-deployments-scd), которое при развертывании содержит в себе среду выполнения предварительной версии. При развертывании автономного приложения.

* Не нужно подготавливать сайт.
* Необходимо опубликовать приложение не так, как при развертывании приложения после установки пакета SDK на сервере.

Автономные приложения — это вариант для всех приложений .NET Core.

<a name="docker"></a>
### <a name="use-docker-with-web-apps-for-containers"></a>Использование Docker с веб-приложениями для контейнеров

[Центр Docker](https://hub.docker.com/r/microsoft/aspnetcore/) содержит образы Docker из последней предварительной версии 2.1. Можно использовать их в качестве базового образа и развертывать в веб-приложения для контейнеров, как обычно.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Общие сведения о веб-приложениях (5-минутное видео)](/azure/app-service/app-service-web-overview)
* [Служба приложений Azure: оптимальное место для размещения приложений .NET (55-минутное видео)](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [Azure, пятница: практики диагностики и устранения неполадок в службе приложений Azure (12-минутное видео)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [Общие сведения о диагностике в службе приложений Azure](/azure/app-service/app-service-diagnostics)

Служба приложений Azure на Windows Server использует [службы IIS](https://www.iis.net/). Технологии IIS посвящены следующие статьи.

* [Размещение ASP.NET Core в Windows со службами IIS](xref:host-and-deploy/iis/index)
* [Введение в модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [Справочник по конфигурации модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Модули IIS с ASP.NET Core](xref:host-and-deploy/iis/modules)
* [Библиотека Microsoft TechNet: Windows Server](/windows-server/windows-server-versions)
