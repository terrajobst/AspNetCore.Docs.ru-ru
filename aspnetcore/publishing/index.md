---
title: "Общие сведения о размещении и развертывании в ASP.NET Core"
author: tdykstra
description: "Общие сведения о настройке сред размещения и развертывании приложений ASP.NET Core в этих средах."
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.assetid: f0930c68-4d17-4748-adbf-801e17601eb6
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/index
ms.openlocfilehash: d030b4f16727080488056c9cde48c31a14a166bf
ms.sourcegitcommit: bd05f7ea8f87ad076ef6e8b704698ebcba5ca80c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/25/2017
---
# <a name="hosting-and-deployment-overview-for-aspnet-core-apps"></a>Общие сведения о размещении и развертывании приложений ASP.NET Core

При развертывании приложения ASP.NET Core в среде внешнего размещения выполняются следующие действия.

* Публикация приложения в папку на сервере размещения.
* Настройка диспетчера процессов, который запускает приложение при поступлении запросов и перезапускает его после аварийного завершения или после перезагрузки сервера.
* Настройка обратного прокси-сервера, который перенаправляет запросы в приложение.

## <a name="publish-to-a-folder"></a>Публикация в папку 

Команда интерфейса командной строки [dotnet publish](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) компилирует код приложения и копирует файлы, необходимые для его выполнения, в папку в *публикации*. При развертывании из Visual Studio шаг `dotnet publish` выполняется автоматически перед копированием файлов место развертывания.

### <a name="folder-contents"></a>Содержимое папки

Папка *публикации* содержит *EXE*- и *DLL*-файлы и зависимости приложения, а также может включать среду выполнения .NET.

Приложения .NET Core могут публиковаться как *автономные* или *зависящие от платформы*. Если приложение автономное, в папку *публикации* добавляются *DLL*-файлы, содержащие среду выполнения .NET.  Если приложение зависит от платформы, файлы среды выполнения .NET не добавляются, так как приложение ссылается на версию .NET, установленную на компьютере. По умолчанию используется модель развертывания с зависимостью от платформы. Дополнительные сведения см. в статье [Развертывание приложений .NET Core](https://docs.microsoft.com/dotnet/articles/core/deploying/index).

В дополнение к *EXE*- и *DLL*-файлам папка *публикации* для приложения ASP.NET Core обычно содержит файлы конфигурации, статические ресурсы и представления MVC.  Дополнительные сведения см. в статье [Структура каталогов](xref:hosting/directory-structure).

## <a name="set-up-a-process-manager"></a>Настройка диспетчер процессов

Приложение ASP.NET Core — это консольное приложение, которое должно запускаться при загрузке сервера и перезапускаться после его аварийного завершения. Для автоматического запуска и перезапуска требуется диспетчер процессов. Чаще всего для ASP.NET Core используются такие диспетчеры запросов, как [Nginx](xref:publishing/linuxproduction) и [Apache](xref:publishing/apache-proxy) в Linux, а также [IIS](xref:publishing/iis) и [Windows Service](xref:hosting/windows-service) в Windows.

## <a name="set-up-a-reverse-proxy"></a>Настройка обратного прокси-сервера

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Если ваше приложение использует веб-сервер [Kestrel](xref:fundamentals/servers/kestrel), в качестве обратного прокси-сервера можно использовать [Nginx](xref:publishing/linuxproduction), [Apache](xref:publishing/apache-proxy) или [IIS](xref:publishing/iis). Обратный прокси-сервер получает HTTP-запросы из Интернета и пересылает их в Kestrel после определенной предварительной обработки. Дополнительные сведения см. в статье [Использование Kestrel с обратным прокси-сервером](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#when-to-use-kestrel-with-a-reverse-proxy).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Если ваше приложение использует веб-сервер [Kestrel](xref:fundamentals/servers/kestrel) и выходит в Интернет, в качестве обратного прокси-сервера необходимо использовать [Nginx](xref:publishing/linuxproduction), [Apache](xref:publishing/apache-proxy) или [IIS](xref:publishing/iis). Обратный прокси-сервер получает HTTP-запросы из Интернета и пересылает их в Kestrel после определенной предварительной обработки. Основной причиной использования обратного прокси-сервера является безопасность. Дополнительные сведения см. в статье [Использование Kestrel с обратным прокси-сервером](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy).

---

## <a name="using-visual-studio-and-msbuild-to-automate-deployment"></a>Использование Visual Studio и MSBuild для автоматизации развертывания

Помимо копирования выходных данных `dotnet publish` на сервер в процессе развертывания часто требуется выполнение и других задач. Это может быть, например, включение дополнительных файлов в папку *публикации* или исключение из нее каких-либо файлов. Visual Studio использует для веб-развертывания MSBuild и настраивает MSBuild для решения многих других задач в процессе развертывания. Дополнительные сведения см. в статье [Профили публикации в Visual Studio](xref:publishing/web-publishing-vs) и в книге [Using MSBuild and Team Foundation Build](http://msbuildbook.com/) (Использование MSBuild и сборки Team Foundation).

Развертывание можно выполнять напрямую из Visual Studio в службу приложений Azure, используя [компонент веб-публикации](xref:tutorials/publish-to-azure-webapp-using-vs) или [встроенную поддержку Git](xref:publishing/azure-continuous-deployment). Visual Studio Team Services поддерживает [непрерывное развертывание в службе приложений Azure](https://www.visualstudio.com/en-us/docs/build/aspnet/core/quick-to-azure).

## <a name="additional-resources"></a>Дополнительные ресурсы

Сведения об использовании Docker в качестве среды размещения см. в статье [Размещение приложений ASP.NET Core в Docker](xref:publishing/docker).
