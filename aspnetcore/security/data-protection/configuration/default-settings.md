---
title: Управление ключами и время существования защиты данных в ASP.NET Core
author: rick-anderson
description: Сведения об управлении ключами защиты данных и времени существования в ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/configuration/default-settings
ms.openlocfilehash: 2f022a4c7519485fe629ce47c27d214c8c27d5bc
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78655072"
---
# <a name="data-protection-key-management-and-lifetime-in-aspnet-core"></a>Управление ключами и время существования защиты данных в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT)

## <a name="key-management"></a>Управление ключами

Приложение пытается обнаружить свою рабочую среду и самостоятельно управлять основной конфигурацией.

1. Если приложение размещено в [приложениях Azure](https://azure.microsoft.com/services/app-service/), ключи сохраняются в папке *%Хоме%\асп.нет\датапротектион-Кэйс* Эта папка копируется в сетевое хранилище и синхронизируется на всех машинах, где размещается приложение.
   * Во время хранения ключи не защищаются.
   * Папка " *Защита* данных" предоставляет ключевые кольца всем экземплярам приложения в одном слоте развертывания.
   * Отдельные слоты развертывания, такие как промежуточное хранение и производство, не используют общую связку ключей. При переключении между слотами развертывания, например переключение промежуточного хранения в рабочую среду или с помощью тестирования/B, любое приложение, использующее защиту данных, не сможет расшифровывать сохраненные данные с помощью звонка в предыдущем слоте. Это ведет к выходу пользователей из приложения, использующего стандартную проверку подлинности ASP.NET Core файлов cookie, так как она использует защиту данных для защиты своих файлов cookie. Если вам нужны независимые от гнезда кольца, используйте внешний поставщик звонков, например хранилище BLOB-объектов Azure, Azure Key Vault, хранилище SQL или кэш Redis.

1. Если профиль пользователя доступен, ключи сохраняются в папке *%локалаппдата%\асп.нет\датапротектион-Кэйс* Если операционной системой является Windows, ключи шифруются при хранении с помощью DPAPI.

   Также необходимо включить атрибут [setProfileEnvironment](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration) пула приложений. Значение `setProfileEnvironment` по умолчанию — `true`. В некоторых сценариях (например, в ОС Windows) для параметра `setProfileEnvironment` установлено значение `false`. Если ключи не хранятся в каталоге профиля пользователя:

   1. Перейдите к папке *%windir%/system32/inetsrv/config*.
   1. Откройте файл *applicationHost.config*.
   1. Найдите элемент `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>`.
   1. Убедитесь, что атрибут `setProfileEnvironment` отсутствует и установлено значение по умолчанию `true`, или же явно задайте для атрибута значение `true`.

1. Если приложение размещено в службах IIS, ключи сохраняются в реестре HKLM в специальном разделе реестра, доступен только учетной записи рабочего процесса. Неактивные ключи шифруются с помощью API защиты данных.

1. Если ни одно из этих условий не найдено, ключи не сохраняются вне текущего процесса. При завершении работы процесса все созданные ключи теряются.

Разработчик всегда находится в режиме полного доступа и может переопределить способ и место хранения ключей. Первые три приведенных выше параметра должны предоставлять хорошие значения по умолчанию для большинства приложений, аналогично тому, как ASP.NET **\<machineKey >** подпрограммы автоматического создания, которые работали в прошлом. Последним вариантом является единственный сценарий, в котором разработчику необходимо указать предварительную [конфигурацию](xref:security/data-protection/configuration/overview) , если требуется сохранение ключей, но этот откат выполняется только в редких ситуациях.

При размещении в контейнере DOCKER ключи должны храниться в папке, которая является томом DOCKER (общий том или подключенный к узлу том, который сохраняется за пределами времени существования контейнера) или во внешнем поставщике, например [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) или [Redis](https://redis.io/). Внешний поставщик также полезен в сценариях веб-фермы, если приложения не могут получить доступ к общему сетевому тому (Дополнительные сведения см. в [персисткэйстофилесистем](xref:security/data-protection/configuration/overview#persistkeystofilesystem) ).

> [!WARNING]
> Если разработчик переопределяет описанные выше правила и указывает систему защиты данных в определенном репозитории ключей, автоматическое шифрование ключей при хранении отключается. Защиту от неактивных компонентов можно включить повторно с помощью [конфигурации](xref:security/data-protection/configuration/overview).

## <a name="key-lifetime"></a>Время существования ключа

По умолчанию для ключей установлено время жизни в 90 дней. По истечении срока действия ключа приложение автоматически создает новый ключ и назначает новый ключ в качестве активного. Если в системе остаются устаревшие ключи, приложение может расшифровывать любые данные, защищенные с помощью. Дополнительные сведения см. в разделе [Управление ключами](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling) .

## <a name="default-algorithms"></a>Алгоритмы по умолчанию

По умолчанию используется алгоритм защиты полезной нагрузки AES-256-CBC для обеспечения конфиденциальности и HMACSHA256 для подлинности. 512-битный главный ключ, измененный каждые 90 дней, используется для получения двух подразделов, используемых для этих алгоритмов, на основе полезной нагрузки. Дополнительные сведения см. в разделе о [наследовании подраздела](xref:security/data-protection/implementation/subkeyderivation#additional-authenticated-data-and-subkey-derivation) .

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:security/data-protection/extensibility/key-management>
* <xref:host-and-deploy/web-farm>
