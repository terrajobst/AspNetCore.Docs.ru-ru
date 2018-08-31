---
title: DevOps с помощью ASP.NET Core и Azure | Средства и файлы для загрузки
author: CamSoper
description: Рекомендации по созданию сквозного решения конвейера DevOps для приложения ASP.NET Core, размещенного в Azure.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/tools-and-downloads
ms.openlocfilehash: a63e97d9ab9eb0ed2fbd30e8c2e033f0c048d33e
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312305"
---
# <a name="tools-and-downloads"></a>Средства и файлы для загрузки

Azure имеет несколько интерфейсов для подготовки и управления ресурсами, такими как [портала Azure](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/cli/azure/), [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview), [облако Azure Оболочка](https://shell.azure.com/bash)и Visual Studio. В этом руководстве принимает минималистский подход и использует Azure Cloud Shell, чтобы уменьшить шаги. Тем не менее портала Azure необходимо использовать для некоторых частей.

## <a name="prerequisites"></a>Предварительные требования

Необходимы следующие подписки:

* Azure &mdash; Если у вас нет учетной записи, [получить бесплатную пробную версию](https://azure.microsoft.com/free/).
* Visual Studio Team Services (VSTS) &mdash; эта учетная запись создается в главе 4.
* GitHub &mdash; Если у вас нет учетной записи, [Подпишитесь на бесплатную версию](https://github.com/join).

Требуются следующие средства:

* [Git](https://git-scm.com/downloads) &mdash; основные сведения об Git рекомендуется использовать в этом руководстве. Просмотрите [документации по Git](https://git-scm.com/doc), в частности [удаленный репозиторий git](https://git-scm.com/docs/git-remote) и [отправку](https://git-scm.com/docs/git-push).
* [Пакет SDK для .NET core](https://www.microsoft.com/net/download/) &mdash; версии 2.1.300 или более поздней версии необходим для построения и запуска примера приложения. Если Visual Studio устанавливается вместе с **кросс Платформенная разработка .NET Core** уже установлена рабочая нагрузка, пакет SDK для .NET Core.

    Проверьте правильность установки пакета SDK для .NET Core. Откройте командную строку и выполните следующую команду:

    ```console
    dotnet --version
    ```

## <a name="recommended-tools-windows-only"></a>Рекомендуемые средства (только Windows)

* [Visual Studio](https://www.visualstudio.com/)в надежные средства Azure предоставляют графический интерфейс пользователя для большинства функций, описанных в этом руководстве. Подойдет любой выпуск Visual Studio, включая бесплатный выпуск Visual Studio Community. Учебники, записываются для демонстрации разработки, развертывания и инструменты DevOps с и без Visual Studio.

  Убедитесь, что Visual Studio имеет следующие [рабочих нагрузок](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) установлен:

  * ASP.NET и веб-разработка
  * Разработка для Azure
  * Кроссплатформенная разработка .NET Core
