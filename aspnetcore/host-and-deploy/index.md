---
title: Размещение и развертывание ASP.NET Core
author: rick-anderson
description: Сведения о настройке сред размещения и развертывании приложений ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/index
ms.openlocfilehash: 1ffc7f9f2dc2a06dddb629d2d2553964b56cec05
ms.sourcegitcommit: 1b94305cc79843e2b0866dae811dab61c21980ad
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/24/2018
---
# <a name="host-and-deploy-aspnet-core"></a>Размещение и развертывание ASP.NET Core

В общем при развертывании приложения ASP.NET Core в среде внешнего размещения выполняются следующие действия.

* Публикация приложения в папку на сервере размещения.
* Настройка диспетчера процессов, который запускает приложение при поступлении запросов и перезапускает его после аварийного завершения или после перезагрузки сервера.
* Если требуется конфигурация обратного прокси-сервера, настройте такой прокси-сервер, который перенаправляет запросы в приложение.

## <a name="publish-to-a-folder"></a>Публикация в папку

Команда интерфейса командной строки [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) компилирует код приложения и копирует файлы, необходимые для его выполнения, в папку *publish*. При развертывании из Visual Studio шаг [dotnet publish](/dotnet/core/tools/dotnet-publish) выполняется автоматически перед копированием файлов в место развертывания.

### <a name="folder-contents"></a>Содержимое папки

Папка *publish* содержит *EXE*- и *DLL*-файлы и зависимости приложения, а также может включать среду выполнения .NET.

Приложения .NET Core могут публиковаться как *автономные* или *зависящие от платформы*. Если приложение автономное, в папку *публикации* добавляются *DLL*-файлы, содержащие среду выполнения .NET. Если приложение зависит от платформы, файлы среды выполнения .NET не добавляются, так как приложение ссылается на версию .NET, установленную на сервере. По умолчанию используется модель развертывания с зависимостью от платформы. Дополнительные сведения см. в статье [Развертывание приложений .NET Core](/dotnet/articles/core/deploying/index).

В дополнение к *EXE*- и *DLL*-файлам папка *публикации* для приложения ASP.NET Core обычно содержит файлы конфигурации, статические ресурсы и представления MVC. Дополнительные сведения см. в статье [Структура каталогов](xref:host-and-deploy/directory-structure).

## <a name="set-up-a-process-manager"></a>Настройка диспетчер процессов

Приложение ASP.NET Core — это консольное приложение, которое должно запускаться при загрузке сервера и перезапускаться после его аварийного завершения. Для автоматического запуска и перезапуска требуется диспетчер процессов. Далее приведены наиболее распространенные диспетчеры процессов для ASP.NET Core.

* Linux
  * [Nginx](xref:host-and-deploy/linux-nginx)
  * [Apache](xref:host-and-deploy/linux-apache)
* Windows
  * [Службы IIS](xref:host-and-deploy/iis/index)
  * [Служба Windows](xref:host-and-deploy/windows-service)

## <a name="set-up-a-reverse-proxy"></a>Настройка обратного прокси-сервера

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Если приложение использует веб-сервер [Kestrel](xref:fundamentals/servers/kestrel), вы можете использовать [nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache) или [IIS](xref:host-and-deploy/iis/index) в качестве обратного прокси-сервера. Обратный прокси-сервер получает HTTP-запросы из Интернета и пересылает их в Kestrel после определенной предварительной обработки.

Любая из этих конфигураций&mdash;с обратным прокси-сервером и без него&mdash;является допустимой и поддерживаемой для размещения основных компонентов приложений ASP.NET версии 2.0 и выше. Дополнительные сведения см. в статье [Использование Kestrel с обратным прокси-сервером](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Если приложение использует веб-сервер [Kestrel](xref:fundamentals/servers/kestrel) и к приложению открыт доступ из Интернета, используйте в качестве обратного прокси-сервера [nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache) или [IIS](xref:host-and-deploy/iis/index). Обратный прокси-сервер получает HTTP-запросы из Интернета и пересылает их в Kestrel после определенной предварительной обработки. Основной причиной использования обратного прокси-сервера является безопасность. Дополнительные сведения см. в статье [Использование Kestrel с обратным прокси-сервером](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy).

---

## <a name="proxy-server-and-load-balancer-scenarios"></a>Сценарии использования прокси-сервера и подсистемы балансировки нагрузки

Для приложений, размещенных за прокси-серверами и подсистемами балансировки нагрузки, может потребоваться дополнительная настройка. Без дополнительной настройки приложение может не иметь доступ к схеме (HTTP/HTTPS) и удаленному IP-адресу, где был сформирован запрос. Дополнительные сведения см. в разделе [Настройка ASP.NET Core для работы с прокси-серверами и подсистемами балансировки нагрузки](xref:host-and-deploy/proxy-load-balancer).

## <a name="using-visual-studio-and-msbuild-to-automate-deployment"></a>Использование Visual Studio и MSBuild для автоматизации развертывания

Помимо копирования выходных данных из [dotnet publish](/dotnet/core/tools/dotnet-publish) на сервер в процессе развертывания часто требуется выполнение и других задач. Например, может потребоваться включить дополнительные файлы в папку *publish* или исключить их из нее. Visual Studio использует для веб-развертывания MSBuild и настраивает MSBuild для решения многих других задач в процессе развертывания. Дополнительные сведения см. в статье [Профили публикации в Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) и в книге [Using MSBuild and Team Foundation Build](http://msbuildbook.com/) (Использование MSBuild и сборки Team Foundation).

Развертывание приложений можно выполнять напрямую из Visual Studio в службу приложений Azure, используя [компонент веб-публикации](xref:tutorials/publish-to-azure-webapp-using-vs) или [встроенную поддержку Git](xref:host-and-deploy/azure-apps/azure-continuous-deployment). Visual Studio Team Services поддерживает [непрерывное развертывание в службе приложений Azure](/vsts/build-release/apps/cd/azure/aspnet-core-to-azure-webapp?tabs=vsts).

## <a name="publishing-to-azure"></a>Публикация в Azure

Инструкции о публикации приложения в Azure с помощью Visual Studio см. в руководстве по [публикации веб-приложения ASP.NET Core в службе приложений Azure с помощью Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs). Приложение можно также опубликовать в Azure из [командной строки](xref:tutorials/publish-to-azure-webapp-using-cli).

## <a name="additional-resources"></a>Дополнительные ресурсы

Сведения об использовании Docker в качестве среды размещения см. в статье [Размещение приложений ASP.NET Core в Docker](xref:host-and-deploy/docker/index).
