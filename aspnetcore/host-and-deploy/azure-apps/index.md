---
title: Развертывание приложений ASP.NET Core в Службе приложений Azure
author: guardrex
description: Эта статья содержит ссылки на ресурсы по размещению и развертыванию в Azure.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2018
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: c70363c9cff94a682553745aba7fd4e396721f4e
ms.sourcegitcommit: 816f39e852a8f453e8682081871a31bc66db153a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/19/2018
ms.locfileid: "53637681"
---
# <a name="deploy-aspnet-core-apps-to-azure-app-service"></a>Развертывание приложений ASP.NET Core в Службе приложений Azure

[Служба приложений Azure](https://azure.microsoft.com/services/app-service/) — это [платформа облачных вычислений Microsoft](https://azure.microsoft.com/), предназначенная для размещения веб-приложений, включая ASP.NET Core.

## <a name="useful-resources"></a>Полезные ресурсы

[Документация по веб-приложениям](/azure/app-service/) Azure — это место, где хранятся документация, учебники, примеры, руководства и другие ресурсы, связанные с приложениями Azure. Размещению приложений ASP.NET Core посвящены следующие два руководства.

[Краткое руководство по созданию веб-приложения ASP.NET Core в Azure](/azure/app-service/app-service-web-get-started-dotnet)  
Создайте веб-приложение ASP.NET Core и разверните его в службе приложений Azure на базе Windows с помощью Visual Studio.

[Краткое руководство по созданию веб-приложения .NET Core в службе приложений на платформе Linux](/azure/app-service/containers/quickstart-dotnetcore)  
Создайте веб-приложение ASP.NET Core и разверните его в службе приложений Azure на базе Linux с помощью командной строки.

Следующие статьи входят в документацию по ASP.NET Core.

<xref:tutorials/publish-to-azure-webapp-using-vs>  
Сведения о публикации приложения ASP.NET Core в службе приложений Azure с помощью Visual Studio.

<xref:host-and-deploy/azure-apps/azure-continuous-deployment>  
Сведения о создании веб-приложения ASP.NET Core с помощью Visual Studio и его развертывании в службе приложений Azure с использованием Git для непрерывного развертывания.

[Создание первого конвейера с помощью Azure Pipelines](/azure/devops/pipelines/get-started-yaml)  
Сведения о настройке сборки CI для приложения ASP.NET Core и последующем создании выпуска непрерывного развертывания в службе приложений Azure.

[Песочница веб-приложений Azure](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
Сведения об ограничениях среды выполнения службы приложений Azure, создаваемых платформой приложений Azure.

## <a name="application-configuration"></a>Настройка приложения

### <a name="platform"></a>Platform

::: moniker range=">= aspnetcore-2.2"

Среды выполнения для 64-разрядных (x64) и 32-разрядных (x86) приложений находятся в Службе приложений Azure. [Пакет SDK для .NET Core](/dotnet/core/sdk), доступный в Службе приложений, является 32-разрядным, но вы можете развертывать 64-разрядные приложения с помощью консоли [Kudu](https://github.com/projectkudu/kudu/wiki) или [MSDeploy с использованием профиля публикации Visual Studio либо интерфейса командной строки](xref:host-and-deploy/visual-studio-publish-profiles).

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Среды выполнения для 32-разрядных (x86) приложений с собственными зависимостями находятся в Службе приложений Azure. [Пакет SDK для .NET Core](/dotnet/core/sdk), доступный в Службе приложений, является 32-разрядным.

::: moniker-end

### <a name="packages"></a>Пакеты

Включите следующие пакеты NuGet, чтобы использовать возможности автоматического ведения журнала для приложений, развернутых в Службе приложений Azure:

* [Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) использует [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) для интеграции подсветки ASP.NET Core в службу приложений Azure. Пакет `Microsoft.AspNetCore.AzureAppServicesIntegration` включает дополнительные функции для ведения журналов.
* [Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) выполняет [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) для добавления поставщиков ведения журналов диагностики из службы приложений Azure в пакет `Microsoft.Extensions.Logging.AzureAppServices`.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) предоставляет реализации средства ведения журналов в поддержку журналов диагностики службы приложений Azure и функций потоковой передачи журналов.

Предыдущие пакеты недоступны в [метапакете Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). Приложения, предназначенные для .NET Framework или ссылающиеся на метапакет `Microsoft.AspNetCore.App`, должны явно ссылаться на отдельные пакеты в файле проекта приложения.

## <a name="override-app-configuration-using-the-azure-portal"></a>Переопределение конфигурации приложения с помощью портала Azure

Параметры приложений на портале Azure позволяют задать переменные среды для приложения. Переменные среды могут использоваться [поставщиком конфигураций переменных среды](xref:fundamentals/configuration/index#environment-variables-configuration-provider).

Когда вы создаете или изменяете параметр приложения на портале Azure, при нажатии кнопки **Сохранить** происходит перезапуск приложения Azure. Переменная среды доступна в приложении после перезапуска службы.

Если приложение использует [Веб-узел](xref:fundamentals/host/web-host) и создает узел с помощью [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), переменные среды, которые настраивают узел, используют префикс `ASPNETCORE_`. Дополнительные сведения см. в разделах <xref:fundamentals/host/web-host> и [Конфигурация для разных сред](xref:fundamentals/configuration/index#environment-variables-configuration-provider).

Если приложение использует [универсальный узел](xref:fundamentals/host/generic-host), переменные среды не загружаются в конфигурацию приложения по умолчанию и поставщик конфигурации должен быть добавлен разработчиком. Разработчик определяет префикс переменной среды при добавлении поставщика конфигурации. Дополнительные сведения см. в разделах <xref:fundamentals/host/generic-host> и [Конфигурация для разных сред](xref:fundamentals/configuration/index#environment-variables-configuration-provider).

## <a name="proxy-server-and-load-balancer-scenarios"></a>Сценарии использования прокси-сервера и подсистемы балансировки нагрузки

ПО промежуточного слоя для интеграции IIS, которое настраивает ПО промежуточного слоя переадресации заголовков, и модуль ASP.NET Core настраиваются на пересылку схемы (HTTP/HTTPS) и удаленного IP-адреса расположения, где был сформирован запрос. Для приложений, размещенных за дополнительными прокси-серверами и подсистемами балансировки нагрузки, может потребоваться дополнительная настройка. Дополнительные сведения см. в разделе [Настройка ASP.NET Core для работы с прокси-серверами и подсистемами балансировки нагрузки](xref:host-and-deploy/proxy-load-balancer).

## <a name="monitoring-and-logging"></a>Мониторинг и ведение журналов

Приложения ASP.NET Core, развернутые в Службе приложений автоматически, получают расширение службы приложений и расширения для **ведения журналов ASP.NET Core**. Расширение обеспечивает ведение журнала Azure.

Сведения о мониторинге, ведении журналов, а также поиске и устранении неполадок см. в следующих статьях.

[Практическое руководство. Мониторинг приложений в Службе приложений Azure](/azure/app-service/web-sites-monitor)  
Сведения о том, как толковать квоты и параметры для приложений и планы службы приложений.

[Включение функции ведения журналов диагностики для веб-приложений в службе приложений Azure](/azure/app-service/web-sites-enable-diagnostic-log)  
Сведения о том, как включать и где искать функцию ведения журнала диагностики для кодов статуса HTTP, невыполненных запросов и активности веб-сервера.

<xref:fundamentals/error-handling>  
Сведения об основных методах обработки ошибок в приложениях ASP.NET Core.

<xref:host-and-deploy/azure-apps/troubleshoot>  
Сведения о диагностике проблем с развертыванием приложений ASP.NET Core в службе приложений Azure.

<xref:host-and-deploy/azure-iis-errors-reference>  
См. распространенные ошибки с конфигурацией развертывания для приложений, размещенных в службе приложений Azure/IIS, и рекомендации по их устранению.

## <a name="data-protection-key-ring-and-deployment-slots"></a>Связка ключей для защиты данных и слоты развертывания

[Ключи для защиты данных](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) хранятся в папке *%HOME%\ASP.NET\DataProtection-Keys*. Эта папка копируется в сетевое хранилище и синхронизируется на всех машинах, где размещается приложение. Во время хранения ключи не защищаются. В этой папке хранится связка ключей для всех экземпляров приложения в одном и том же слоте развертывания. Отдельные слоты развертывания, такие как промежуточное хранение и производство, не используют общую связку ключей.

При переключении между разными слотами развертывания ни одна система с использованием защиты данных не сможет расшифровать хранимые данные, используя связку ключей из предыдущего слота. Для защиты файлов cookie ПО промежуточного слоя ASP.NET Cookie использует защиту данных. В результате пользователей, применяющих ПО промежуточного слоя ASP.NET Cookie, выбрасывает из приложения. Для того чтобы решение связки ключей не зависело от слота, используйте внешнего поставщика связки ключей, например:

* Хранилище больших двоичных объектов Azure;
* Хранилище ключей Azure;
* Хранилище SQL;
* Кэш Redis.

Для получения дополнительной информации см. <xref:security/data-protection/implementation/key-storage-providers>.

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a>Развертывание предварительной версии ASP.NET Core в службе приложений Azure

Воспользуйтесь одним из перечисленных ниже подходов.

* [Установка расширения сайта предварительной версии](#install-the-preview-site-extension).
* [Автономное развертывание приложения](#deploy-the-app-self-contained).
* [Использование Docker с веб-приложениями для контейнеров](#use-docker-with-web-apps-for-containers).

### <a name="install-the-preview-site-extension"></a>Установка расширения сайта предварительной версии

Если у вас возникли проблемы при использовании расширения сайта предварительной версии, сообщите о них на [GitHub](https://github.com/aspnet/azureintegration/issues/new).

1. Перейдите в Службу приложений на портале Azure.
1. Выберите веб-приложение.
1. В поле поиска введите "ex" для фильтрации расширений ("Extensions") или прокрутите вниз список инструментов управления.
1. Выберите **Расширения**.
1. Нажмите **Добавить**.
1. Выберите в списке расширение **Среда выполнения ASP.NET Core {X.Y} ({x64|x86})**, где `{X.Y}` — это предварительная версия ASP.NET Core, а `{x64|x86}` — платформа.
1. Для принятия условий нажмите кнопку **ОК**.
1. Нажмите **OK**, чтобы установить расширение.

По завершении операции устанавливается последняя предварительная версия .NET Core. Проверьте установку:

1. Выберите **Дополнительные инструменты**.
1. В разделе **Дополнительные инструменты** выберите **Перейти**.
1. Выберите пункт меню **Консоль отладки** > **PowerShell**.
1. По запросу PowerShell выполните следующую команду. В следующей команде вместо `{X.Y}` укажите версию среды выполнения ASP.NET Core, а вместо `{PLATFORM}` — платформу:

   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.{PLATFORM}\
   ```
   Эта команда возвращает `True`, если установлена предварительная версия среды выполнения x64.

> [!NOTE]
> Если приложение Службы приложений размещено в среде вычислений серии A или на более высоком уровне, архитектура платформы приложения (x86/x64) задается в его параметрах на портале для приложений Azure. Если приложение выполняется во внутрипроцессном режиме, а архитектура платформы настроена для 64-разрядных версий (x64), модуль ASP.NET Core использует 64-разрядную предварительную версию среды выполнения при ее наличии. Установите расширение **Среда выполнения ASP.NET Core {X.Y} (x64)**.
>
> После установки предварительной версии среды выполнения x64 выполните следующую команду в командном окне Kudu PowerShell, чтобы проверить установку. Замените версию среды выполнения ASP.NET Core на `{X.Y}` в команде:
>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64\
> ```
> Эта команда возвращает `True`, если установлена предварительная версия среды выполнения x64.

> [!NOTE]
> **Расширения ASP.NET Core** включают дополнительные функциональные возможности для ASP.NET Core в службах приложений Azure, например включение ведения журналов Azure. Расширение устанавливается автоматически при развертывании из Visual Studio. Если расширение не установлено, установите его для приложения.

**Использование расширения сайта предварительной версии с шаблоном ARM**

Если вы используете шаблон ARM для создания и развертывания приложений, можно использовать тип ресурса `siteextensions`, чтобы добавить расширение сайта в веб-приложение. Например:

[!code-json[](index/sample/arm.json?highlight=2)]

### <a name="deploy-the-app-self-contained"></a>Автономное развертывание приложения

Объект [автономного развертывания (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd), который предназначен для предварительной версии среды выполнения, включает в развертывание среду выполнения предварительной версии.

При развертывании автономного приложения:

* Сайт в Службе приложений Azure не требует [предварительной версии расширения сайта](#install-the-preview-site-extension).
* Для публикации приложений следует использовать другой подход, нежели при публикации [зависящего от платформы развертывания (FDD)](/dotnet/core/deploying#framework-dependent-deployments-fdd).

#### <a name="publish-from-visual-studio"></a>Публикация из Visual Studio

1. Выберите **Сборка** > **Опубликовать {имя_приложения}** на панели инструментов Visual Studio.
1. В диалоговом оке **Выберите целевой объект публикации** убедитесь, что выбран вариант **Служба приложений**.
1. Выберите **Дополнительно**. Откроется диалоговое окно **Публикация**.
1. В диалоговом окне **Публикация**:
   * Убедитесь, что выбрана конфигурация **Выпуск**.
   * Откройте раскрывающийся список **Режим развертывания** и выберите вариант **Автономное**.
   * Выберите целевую среду выполнения из раскрывающегося списка **Целевая среда выполнения**. Значение по умолчанию — `win-x86`.
   * Если потребуется удалить дополнительные файлы после развертывания, откройте **Параметры публикации файлов** и установите флажок для удаления дополнительных файлов в месте назначения.
   * Нажмите кнопку **Сохранить**.
1. Создайте новый сайт или обновите существующий, следуя остальным подсказкам мастера публикации.

#### <a name="publish-using-command-line-interface-cli-tools"></a>Публикация с помощью средств интерфейса командной строки

1. В файле проекта укажите один или несколько [идентификаторов сред выполнения (RID)](/dotnet/core/rid-catalog). Используйте `<RuntimeIdentifier>` (в единственном числе) для одного RID или `<RuntimeIdentifiers>` (во множественном числе), чтобы предоставить разделенный точкой с запятой список идентификаторов RID. В следующем примере указан RID `win-x86`:

   ```xml
   <PropertyGroup>
     <TargetFramework>netcoreapp2.1</TargetFramework>
     <RuntimeIdentifier>win-x86</RuntimeIdentifier>
   </PropertyGroup>
   ```
1. Из командной оболочки опубликуйте приложение в конфигурации выпуска для среды выполнения узла, выполнив команду [dotnet publish](/dotnet/core/tools/dotnet-publish). В следующем примере публикуется приложение с RID `win-x86`. Предоставленный в параметре `--runtime` RID должены быть указан в свойстве `<RuntimeIdentifier>` (или `<RuntimeIdentifiers>`) в файле проекта.

   ```console
   dotnet publish --configuration Release --runtime win-x86
   ```
1. Переместите содержимое каталога *bin/Release/{целевая_платформа}/{идентификатор_среды_выполнения}/publish* на сайт в Службе приложений.

### <a name="use-docker-with-web-apps-for-containers"></a>Использование Docker с веб-приложениями для контейнеров

[Центр Docker](https://hub.docker.com/r/microsoft/aspnetcore/) содержит образы Docker из последней предварительной версии. Их можно использовать в качестве базового образа. Использование образа и развертывание в веб-приложениях для контейнеров выполняется как обычно.

## <a name="protocol-settings-https"></a>Параметры протокола (HTTPS)

Привязки безопасных протоколов позволяют указать сертификат, который следует использовать при ответе на запросы по HTTPS. Для привязки требуется допустимый закрытый сертификат (*PFX*), выданный для определенного имени узла. Дополнительные сведения см. в статье [Руководство. Привязывание существующего настраиваемого SSL-сертификата к веб-приложениям Azure](/azure/app-service/app-service-web-tutorial-custom-ssl).

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Общие сведения о веб-приложениях (5-минутное видео)](/azure/app-service/app-service-web-overview)
* [Служба приложений Azure: оптимальное место для размещения приложений .NET (55-минутное видео)](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [Пятница с Azure. Практика диагностики и устранения неполадок в Службе приложений Azure (12-минутное видео)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [Общие сведения о диагностике в службе приложений Azure](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

Служба приложений Azure на Windows Server использует [службы IIS](https://www.iis.net/). Технологии IIS посвящены следующие статьи.

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [Библиотека Microsoft TechNet: Windows Server](/windows-server/windows-server-versions)
