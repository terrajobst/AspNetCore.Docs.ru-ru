---
title: Начало работы с ASP.NET Core
author: rick-anderson
description: Краткий учебник, в котором с помощью ASP.NET Core создается и запускается простое приложение Hello World.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 22e9c982921cc03d89506e18ff99bf481027dda6
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/18/2018
ms.locfileid: "38216217"
---
# <a name="get-started-with-aspnet-core"></a>Начало работы с ASP.NET Core

::: moniker range=">= aspnetcore-2.1"

1. Установите [!INCLUDE [](~/includes/2.1-SDK.md)].

2. Создайте проект ASP.NET Core. Откройте окно командной оболочки и введите следующую команду.

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    [!INCLUDE [](~/includes/webapp-alias-notice.md) [](~/includes/webapp-alias-notice.md)]

3. Установите доверие к сертификату разработки HTTPS.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following dialog:

    ![Security warning dialog](_static/cert.png)

    Select **Yes** if you agree to trust the development certificate.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following message:

    *Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:*
    `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`
    *This command might prompt you for your password to install the certificate on the system keychain.
    Password:*

    Enter your password if you agree to trust the development certificate.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

    See the documentation for your Linux distribution on how to trust the HTTPS development certificate
---

4. Запустите приложение:

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. Перейдите по адресу [http://localhost:5001](http://localhost:5001).  Щелкните **Принять**, чтобы принять политику конфиденциальности и использования файлов cookie. Это приложение не хранит персональные данные.

6. Откройте *Pages/About.cshtml* и измените страницу, добавив выделенное исправление.

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. Перейдите к [http://localhost:5001/About](http://localhost:5001/About) и проверьте, отобразились ли изменения.

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. Установите [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].

2. Создайте новый проект ASP.NET Core.

   Откройте командную оболочку. Введите следующую команду:

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. Запустите приложение с помощью следующих команд:

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. Перейдите по адресу [http://localhost:5000](http://localhost:5000).

5. Откройте файл *Pages/About.cshtml* и измените страницу, чтобы на ней отображалось сообщение "Hello, world! Время на сервере — @DateTime.Now" :

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. Перейдите к [http://localhost:5000/About](http://localhost:5000/About) и проверьте изменения.

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. Установите **установщик пакета SDK** версии 1.0.4 для .NET Core со [страницы "Все загрузки .NET Core"](https://www.microsoft.com/net/download/all).

2. Создайте папку для нового проекта ASP.NET Core.

   Откройте командную оболочку. Введите следующие команды.

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. Если на компьютере установлена более поздняя версия пакета SDK, создайте файл *global.json*, чтобы выбрать пакет SDK версии 1.0.4.

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. Создайте новый проект ASP.NET Core.

   ```console
   dotnet new web
   ```

5. Восстановите пакеты.

    ```console
    dotnet restore
    ```

6. Запустите приложение.

   ```console
   dotnet run
   ```

   При необходимости команда [dotnet run](/dotnet/core/tools/dotnet-run) сначала выполняет сборку приложения.

7. Перейдите по адресу `http://localhost:5000`.

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
