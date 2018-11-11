---
title: Размещение ASP.NET Core в службе Windows
author: guardrex
description: Узнайте, как разместить приложение ASP.NET Core в службе Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/30/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 11913019bfe5d06c259b806fce9cc580a8280ad5
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758197"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="b101e-103">Размещение ASP.NET Core в службе Windows</span><span class="sxs-lookup"><span data-stu-id="b101e-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="b101e-104">Авторы: [Люк Латэм](https://github.com/guardrex) (Luke Latham) и [Tom Dykstra](https://github.com/tdykstra) (Том Дайкстра)</span><span class="sxs-lookup"><span data-stu-id="b101e-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="b101e-105">Приложение ASP.NET Core можно разместить в Windows без использования IIS в качестве [службы Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="b101e-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="b101e-106">При размещении в качестве службы Windows приложение автоматически запускается после перезагрузки.</span><span class="sxs-lookup"><span data-stu-id="b101e-106">When hosted as a Windows Service, the app automatically starts after reboots.</span></span>

<span data-ttu-id="b101e-107">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b101e-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="b101e-108">Преобразование проекта в службу Windows</span><span class="sxs-lookup"><span data-stu-id="b101e-108">Convert a project into a Windows Service</span></span>

<span data-ttu-id="b101e-109">Чтобы настроить запуск в виде службы для существующего проекта ASP.NET Core, выполните следующий минимальный набор изменений.</span><span class="sxs-lookup"><span data-stu-id="b101e-109">The following minimum changes are required to set up an existing ASP.NET Core project to run as a service:</span></span>

1. <span data-ttu-id="b101e-110">В файле проекта:</span><span class="sxs-lookup"><span data-stu-id="b101e-110">In the project file:</span></span>

   * <span data-ttu-id="b101e-111">Подтвердите наличие [идентификатора среды выполнения](/dotnet/core/rid-catalog) Windows или добавьте его в `<PropertyGroup>`, где содержится требуемая версия платформы:</span><span class="sxs-lookup"><span data-stu-id="b101e-111">Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add it to the `<PropertyGroup>` that contains the target framework:</span></span>

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.2</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      <span data-ttu-id="b101e-112">Чтобы выполнить публикацию для нескольких идентификаторов RID, сделайте следующее.</span><span class="sxs-lookup"><span data-stu-id="b101e-112">To publish for multiple RIDs:</span></span>

      * <span data-ttu-id="b101e-113">Укажите список идентификаторов RID, разделив их точкой с запятой.</span><span class="sxs-lookup"><span data-stu-id="b101e-113">Provide the RIDs in a semicolon-delimited list.</span></span>
      * <span data-ttu-id="b101e-114">Укажите имя свойства `<RuntimeIdentifiers>` (множественное число).</span><span class="sxs-lookup"><span data-stu-id="b101e-114">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

      <span data-ttu-id="b101e-115">Дополнительные сведения см. в [каталоге RID для .NET Core](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="b101e-115">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

   * <span data-ttu-id="b101e-116">Добавьте ссылку на пакет для [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span><span class="sxs-lookup"><span data-stu-id="b101e-116">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

1. <span data-ttu-id="b101e-117">Внесите следующие изменения в `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="b101e-117">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="b101e-118">Вызовите [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) вместо `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="b101e-118">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="b101e-119">Вызовите [UseContentRoot](xref:fundamentals/host/web-host#content-root) и используйте путь к расположению публикации приложения вместо `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="b101e-119">Call [UseContentRoot](xref:fundamentals/host/web-host#content-root) and use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,16)]

1. <span data-ttu-id="b101e-120">Опубликуйте приложение с помощью команды [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), [профиля публикации Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) или Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="b101e-120">Publish the app using [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles), or Visual Studio Code.</span></span> <span data-ttu-id="b101e-121">Если вы используете Visual Studio, выберите **FolderProfile** и настройте **целевое расположение**, прежде чем нажимать кнопку **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="b101e-121">When using Visual Studio, select the **FolderProfile** and configure the **Target Location** before selecting the **Publish** button.</span></span>

   <span data-ttu-id="b101e-122">Чтобы опубликовать пример приложения через интерфейс командной строки (CLI), выполните в командной строке команду [dotnet publish](/dotnet/core/tools/dotnet-publish) в папке проекта.</span><span class="sxs-lookup"><span data-stu-id="b101e-122">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command at a command prompt from the project folder.</span></span> <span data-ttu-id="b101e-123">Идентификатор RID следует указать в свойстве `<RuntimeIdenfifier>` (или `<RuntimeIdentifiers>`) в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="b101e-123">The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="b101e-124">В следующем примере публикуется приложение в конфигурации выпуска для среды выполнения `win7-x64` в папке, созданной в каталоге *c:\\svc*:</span><span class="sxs-lookup"><span data-stu-id="b101e-124">In the following example, the app is published in Release configuration for the `win7-x64` runtime to a folder created at *c:\\svc*:</span></span>

   ```console
   dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
   ```

1. <span data-ttu-id="b101e-125">Создайте учетную запись пользователя для службы с помощью команды `net user`:</span><span class="sxs-lookup"><span data-stu-id="b101e-125">Create a user account for the service using the `net user` command:</span></span>

   ```console
   net user {USER ACCOUNT} {PASSWORD} /add
   ```

   <span data-ttu-id="b101e-126">Для примера приложения создайте учетную запись пользователя с именем `ServiceUser` и пароль.</span><span class="sxs-lookup"><span data-stu-id="b101e-126">For the sample app, create a user account with the name `ServiceUser` and a password.</span></span> <span data-ttu-id="b101e-127">В следующей команде замените `{PASSWORD}` на [надежный пароль](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span><span class="sxs-lookup"><span data-stu-id="b101e-127">In the following command, replace `{PASSWORD}` with a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span></span>

   ```console
   net user ServiceUser {PASSWORD} /add
   ```

   <span data-ttu-id="b101e-128">Если вам нужно добавить пользователя в группу, используйте команду `net localgroup`, где `{GROUP}` — это имя группы:</span><span class="sxs-lookup"><span data-stu-id="b101e-128">If you need to add the user to a group, use the `net localgroup` command, where `{GROUP}` is the name of the group:</span></span>

   ```console
   net localgroup {GROUP} {USER ACCOUNT} /add
   ```

   <span data-ttu-id="b101e-129">Дополнительные сведения см. в статье [Service User Accounts](/windows/desktop/services/service-user-accounts) (Учетные записи пользователей службы).</span><span class="sxs-lookup"><span data-stu-id="b101e-129">For more information, see [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

1. <span data-ttu-id="b101e-130">Предоставьте доступ на запись, чтение и выполнение для папки приложения с помощью команды [icacls](/windows-server/administration/windows-commands/icacls):</span><span class="sxs-lookup"><span data-stu-id="b101e-130">Grant write/read/execute access to the app's folder using the [icacls](/windows-server/administration/windows-commands/icacls) command:</span></span>

   ```console
   icacls "{PATH}" /grant {USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS} /t
   ```

   * <span data-ttu-id="b101e-131">`{PATH}` &ndash; путь к папке приложения.</span><span class="sxs-lookup"><span data-stu-id="b101e-131">`{PATH}` &ndash; Path to the app's folder.</span></span>
   * <span data-ttu-id="b101e-132">`{USER ACCOUNT}` &ndash; учетная запись пользователя (SID).</span><span class="sxs-lookup"><span data-stu-id="b101e-132">`{USER ACCOUNT}` &ndash; The user account (SID).</span></span>
   * <span data-ttu-id="b101e-133">`(OI)` &ndash; флаг наследования объекта, который распространяет разрешения на вложенные файлы.</span><span class="sxs-lookup"><span data-stu-id="b101e-133">`(OI)` &ndash; The Object Inherit flag propagates permissions to subordinate files.</span></span>
   * <span data-ttu-id="b101e-134">`(CI)` &ndash; флаг наследования контейнера, который распространяет разрешения на вложенные папки.</span><span class="sxs-lookup"><span data-stu-id="b101e-134">`(CI)` &ndash; The Container Inherit flag propagates permissions to subordinate folders.</span></span>
   * <span data-ttu-id="b101e-135">`{PERMISSION FLAGS}` &ndash; устанавливает разрешения для доступа к приложениям.</span><span class="sxs-lookup"><span data-stu-id="b101e-135">`{PERMISSION FLAGS}` &ndash; Sets the app's access permissions.</span></span>
     * <span data-ttu-id="b101e-136">Запись (`W`)</span><span class="sxs-lookup"><span data-stu-id="b101e-136">Write (`W`)</span></span>
     * <span data-ttu-id="b101e-137">Чтение (`R`)</span><span class="sxs-lookup"><span data-stu-id="b101e-137">Read (`R`)</span></span>
     * <span data-ttu-id="b101e-138">Выполнение (`X`)</span><span class="sxs-lookup"><span data-stu-id="b101e-138">Execute (`X`)</span></span>
     * <span data-ttu-id="b101e-139">Полное (`F`)</span><span class="sxs-lookup"><span data-stu-id="b101e-139">Full (`F`)</span></span>
     * <span data-ttu-id="b101e-140">Изменение (`M`)</span><span class="sxs-lookup"><span data-stu-id="b101e-140">Modify (`M`)</span></span>
   * <span data-ttu-id="b101e-141">`/t` &ndash; применяется рекурсивно к имеющимся вложенным папкам и файлам.</span><span class="sxs-lookup"><span data-stu-id="b101e-141">`/t` &ndash; Apply recursively to existing subordinate folders and files.</span></span>

   <span data-ttu-id="b101e-142">Для примера приложения, опубликованного в папке *c:\\svc*, и учетной записи `ServiceUser` с разрешениями на запись, чтение и выполнение используйте следующую команду:</span><span class="sxs-lookup"><span data-stu-id="b101e-142">For the sample app published to the *c:\\svc* folder and the `ServiceUser` account with write/read/execute permissions, use the following command:</span></span>

   ```console
   icacls "c:\svc" /grant ServiceUser:(OI)(CI)WRX /t
   ```

   <span data-ttu-id="b101e-143">Дополнительные сведения см. в статье об [icacls](/windows-server/administration/windows-commands/icacls).</span><span class="sxs-lookup"><span data-stu-id="b101e-143">For more information, see [icacls](/windows-server/administration/windows-commands/icacls).</span></span>

1. <span data-ttu-id="b101e-144">Используйте программу командной строки [sc.exe](https://technet.microsoft.com/library/bb490995), чтобы создать службу.</span><span class="sxs-lookup"><span data-stu-id="b101e-144">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="b101e-145">Значение `binPath` обозначает путь к исполняемому файлу приложения, включая имя самого файла.</span><span class="sxs-lookup"><span data-stu-id="b101e-145">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="b101e-146">**Требуется пробел между знаком равенства и кавычками для каждого обязательного параметра и значения.**</span><span class="sxs-lookup"><span data-stu-id="b101e-146">**The space between the equal sign and the quote character of each parameter and value is required.**</span></span>

   ```console
   sc create {SERVICE NAME} binPath= "{PATH}" obj= "{DOMAIN}\{USER ACCOUNT}" password= "{PASSWORD}"
   ```

   * <span data-ttu-id="b101e-147">`{SERVICE NAME}` &ndash; имя, присваиваемое службе в [диспетчере служб](/windows/desktop/services/service-control-manager).</span><span class="sxs-lookup"><span data-stu-id="b101e-147">`{SERVICE NAME}` &ndash; The name to assign to the service in [Service Control Manager](/windows/desktop/services/service-control-manager).</span></span>
   * <span data-ttu-id="b101e-148">`{PATH}` &ndash; путь к исполняемому файлу.</span><span class="sxs-lookup"><span data-stu-id="b101e-148">`{PATH}` &ndash; The path to the service executable.</span></span>
   * <span data-ttu-id="b101e-149">`{DOMAIN}` (если компьютер не присоединен к домену, имя локального компьютера) и `{USER ACCOUNT}` &ndash; домен (или имя локального компьютера) и учетная запись пользователя, под которой выполняется служба.</span><span class="sxs-lookup"><span data-stu-id="b101e-149">`{DOMAIN}` (or if the machine isn't domain joined, the local machine name) and `{USER ACCOUNT}` &ndash; The domain (or local machine name) and user account under which the service runs.</span></span> <span data-ttu-id="b101e-150">**Не** пропускайте параметр `obj`.</span><span class="sxs-lookup"><span data-stu-id="b101e-150">Do **not** omit the `obj` parameter.</span></span> <span data-ttu-id="b101e-151">Значение по умолчанию параметра `obj` — это [учетная запись LocalSystem](/windows/desktop/services/localsystem-account).</span><span class="sxs-lookup"><span data-stu-id="b101e-151">The default value for `obj` is the [LocalSystem account](/windows/desktop/services/localsystem-account) account.</span></span> <span data-ttu-id="b101e-152">Выполнение службы под учетной записью `LocalSystem` представляет собой серьезную угрозу безопасности.</span><span class="sxs-lookup"><span data-stu-id="b101e-152">Running a service under the `LocalSystem` account presents a significant security risk.</span></span> <span data-ttu-id="b101e-153">Всегда запускайте службу под учетной записью пользователя с ограниченными привилегиями на сервере.</span><span class="sxs-lookup"><span data-stu-id="b101e-153">Always run a service under a user account with restricted privileges on the server.</span></span>
   * <span data-ttu-id="b101e-154">`{PASSWORD}` &ndash; пароль учетной записи пользователя.</span><span class="sxs-lookup"><span data-stu-id="b101e-154">`{PASSWORD}` &ndash; The user account password.</span></span>

   <span data-ttu-id="b101e-155">В следующем примере:</span><span class="sxs-lookup"><span data-stu-id="b101e-155">In the following example:</span></span>

   * <span data-ttu-id="b101e-156">Служба называется **MyService**.</span><span class="sxs-lookup"><span data-stu-id="b101e-156">The service is named **MyService**.</span></span>
   * <span data-ttu-id="b101e-157">Опубликованная служба размещается в папке *c:\\svc*.</span><span class="sxs-lookup"><span data-stu-id="b101e-157">The published service resides in the *c:\\svc* folder.</span></span> <span data-ttu-id="b101e-158">Имя исполняемого файла приложения — *AspNetCoreService.exe*.</span><span class="sxs-lookup"><span data-stu-id="b101e-158">The app executable is named *AspNetCoreService.exe*.</span></span> <span data-ttu-id="b101e-159">Значение `binPath` заключено в прямые кавычки (").</span><span class="sxs-lookup"><span data-stu-id="b101e-159">The `binPath` value is enclosed in straight quotation marks (").</span></span>
   * <span data-ttu-id="b101e-160">Служба работает под учетной записью `ServiceUser`.</span><span class="sxs-lookup"><span data-stu-id="b101e-160">The service runs under the `ServiceUser` account.</span></span> <span data-ttu-id="b101e-161">Замените `{DOMAIN}` на домен учетной записи пользователя или имя локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="b101e-161">Replace `{DOMAIN}` with the user account's domain or local machine name.</span></span> <span data-ttu-id="b101e-162">Заключите значение `obj` в прямые кавычки (").</span><span class="sxs-lookup"><span data-stu-id="b101e-162">Enclose the `obj` value in straight quotation marks (").</span></span> <span data-ttu-id="b101e-163">Пример: если система размещения — это локальный компьютер с именем `MairaPC`, задайте для параметра `obj` значение `"MairaPC\ServiceUser"`.</span><span class="sxs-lookup"><span data-stu-id="b101e-163">Example: If the hosting system is a local machine named `MairaPC`, set `obj` to `"MairaPC\ServiceUser"`.</span></span>
   * <span data-ttu-id="b101e-164">Замените `{PASSWORD}` на пароль учетной записи пользователя.</span><span class="sxs-lookup"><span data-stu-id="b101e-164">Replace `{PASSWORD}` with the user account's password.</span></span> <span data-ttu-id="b101e-165">Значение `password` заключено в прямые кавычки (").</span><span class="sxs-lookup"><span data-stu-id="b101e-165">The `password` value is enclosed in straight quotation marks (").</span></span>

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe" obj= "{DOMAIN}\ServiceUser" password= "{PASSWORD}"
   ```

   > [!IMPORTANT]
   > <span data-ttu-id="b101e-166">Убедитесь, что между знаками равенства и значениями параметров есть пробелы.</span><span class="sxs-lookup"><span data-stu-id="b101e-166">Make sure that the spaces between the parameters' equal signs and the parameters' values are present.</span></span>

1. <span data-ttu-id="b101e-167">Запустите службу с помощью команды `sc start {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="b101e-167">Start the service with the `sc start {SERVICE NAME}` command.</span></span>

   <span data-ttu-id="b101e-168">Чтобы запустить пример службы приложения, используйте следующую команду:</span><span class="sxs-lookup"><span data-stu-id="b101e-168">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="b101e-169">Команде потребуется несколько секунд, чтобы запустить службу.</span><span class="sxs-lookup"><span data-stu-id="b101e-169">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="b101e-170">Чтобы проверить состояние службы, используйте команду `sc query {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="b101e-170">To check the status of the service, use the `sc query {SERVICE NAME}` command.</span></span> <span data-ttu-id="b101e-171">Состояние отображается одним из следующих значений:</span><span class="sxs-lookup"><span data-stu-id="b101e-171">The status is reported as one of the following values:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="b101e-172">Чтобы проверить состояние примера службы приложения, используйте следующую команду:</span><span class="sxs-lookup"><span data-stu-id="b101e-172">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="b101e-173">Если служба находится в состоянии `RUNNING` и является веб-приложением, найдите приложение по его пути (по умолчанию `http://localhost:5000`, который перенаправляет на `https://localhost:5001` при использовании [ПО промежуточного слоя перенаправления на HTTPS](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="b101e-173">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="b101e-174">Чтобы получить пример службы приложений, найдите приложение по адресу `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="b101e-174">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="b101e-175">Остановите службу с помощью команды `sc stop {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="b101e-175">Stop the service with the `sc stop {SERVICE NAME}` command.</span></span>

   <span data-ttu-id="b101e-176">Чтобы остановить пример службы приложения, используйте следующую команду:</span><span class="sxs-lookup"><span data-stu-id="b101e-176">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="b101e-177">После небольшой задержки для остановки службы удалите службу с помощью команды `sc delete {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="b101e-177">After a short delay to stop a service, uninstall the service with the `sc delete {SERVICE NAME}` command.</span></span>

   <span data-ttu-id="b101e-178">Проверьте состояние примера службы приложений:</span><span class="sxs-lookup"><span data-stu-id="b101e-178">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="b101e-179">Когда пример службы приложений находится в состоянии `STOPPED`, используйте следующую команду для удаления примера службы приложений:</span><span class="sxs-lookup"><span data-stu-id="b101e-179">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="run-the-app-outside-of-a-service"></a><span data-ttu-id="b101e-180">Запуск приложения вне службы</span><span class="sxs-lookup"><span data-stu-id="b101e-180">Run the app outside of a service</span></span>

<span data-ttu-id="b101e-181">Тестирование и отладку проще выполнять при работе вне службы. Поэтому разработчики обычно добавляют код, который вызывает `RunAsService` только при соблюдении определенных условий.</span><span class="sxs-lookup"><span data-stu-id="b101e-181">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="b101e-182">Например, приложение может выполняться как консольное приложение с аргументом командной строки `--console` или при присоединении отладчика:</span><span class="sxs-lookup"><span data-stu-id="b101e-182">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

<span data-ttu-id="b101e-183">Так как для конфигурации ASP.NET Core требуется пара имя-значение для аргументов командной строки, параметр `--console` удаляется до передачи аргументов в [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="b101e-183">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

> [!NOTE]
> <span data-ttu-id="b101e-184">`isService` не передается из `Main` в `CreateWebHostBuilder`, так как для правильного [тестирования интеграции](xref:test/integration-tests) сигнатура `CreateWebHostBuilder` должна иметь значение `CreateWebHostBuilder(string[])`.</span><span class="sxs-lookup"><span data-stu-id="b101e-184">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="b101e-185">Обработка событий остановки и запуска</span><span class="sxs-lookup"><span data-stu-id="b101e-185">Handle stopping and starting events</span></span>

<span data-ttu-id="b101e-186">Для обработки событий [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted) и [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) внесите следующие дополнительные изменения:</span><span class="sxs-lookup"><span data-stu-id="b101e-186">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="b101e-187">Создайте класс, производный от [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span><span class="sxs-lookup"><span data-stu-id="b101e-187">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="b101e-188">Создайте метод расширения для [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost), который передает пользовательский параметр `WebHostService` в [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span><span class="sxs-lookup"><span data-stu-id="b101e-188">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="b101e-189">В `Program.Main` измените вызов [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) на вызов нового метода расширения `RunAsCustomService`:</span><span class="sxs-lookup"><span data-stu-id="b101e-189">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=17)]

   > [!NOTE]
   > <span data-ttu-id="b101e-190">`isService` не передается из `Main` в `CreateWebHostBuilder`, так как для правильного [тестирования интеграции](xref:test/integration-tests) сигнатура `CreateWebHostBuilder` должна иметь значение `CreateWebHostBuilder(string[])`.</span><span class="sxs-lookup"><span data-stu-id="b101e-190">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

<span data-ttu-id="b101e-191">Если пользовательский код `WebHostService` обращается к службе путем внедрения зависимости (например, к средству ведения журнала), ее можно получить из свойства [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services):</span><span class="sxs-lookup"><span data-stu-id="b101e-191">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="b101e-192">Сценарии использования прокси-сервера и подсистемы балансировки нагрузки</span><span class="sxs-lookup"><span data-stu-id="b101e-192">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="b101e-193">Для служб, которые взаимодействуют с запросами из Интернета или корпоративной сети и размещаются за прокси-сервером или подсистемой балансировки нагрузки, может потребоваться дополнительная настройка.</span><span class="sxs-lookup"><span data-stu-id="b101e-193">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="b101e-194">Дополнительные сведения см. в разделе <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="b101e-194">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="b101e-195">Настройка HTTPS</span><span class="sxs-lookup"><span data-stu-id="b101e-195">Configure HTTPS</span></span>

<span data-ttu-id="b101e-196">Чтобы настроить службу с защищенной конечной точкой, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="b101e-196">To configure the service with a secure endpoint:</span></span>

1. <span data-ttu-id="b101e-197">Создайте сертификат X.509 для системы размещения с помощью механизмов получения и развертывания сертификата вашей платформы.</span><span class="sxs-lookup"><span data-stu-id="b101e-197">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>
1. <span data-ttu-id="b101e-198">Укажите [конфигурацию конечной точки HTTPS для сервера Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration), чтобы использовать сертификат.</span><span class="sxs-lookup"><span data-stu-id="b101e-198">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="b101e-199">Использование сертификата разработки ASP.NET Core HTTPS для защиты конечной точки службы не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="b101e-199">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="b101e-200">Текущий каталог и корневой каталог содержимого</span><span class="sxs-lookup"><span data-stu-id="b101e-200">Current directory and content root</span></span>

<span data-ttu-id="b101e-201">Для службы Windows `Directory.GetCurrentDirectory()` возвращает текущий рабочий каталог *C:\\WINDOWS\\system32*.</span><span class="sxs-lookup"><span data-stu-id="b101e-201">The current working directory returned by calling `Directory.GetCurrentDirectory()` for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="b101e-202">Папка *system32* не подходит для хранения файлов службы (например, файлов параметров).</span><span class="sxs-lookup"><span data-stu-id="b101e-202">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="b101e-203">Используйте один из следующих методов для сохранения и доступа к ресурсам и файлам параметров службы с помощью [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) при использовании [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="b101e-203">Use one of the following approaches to maintain and access a service's assets and settings files with [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) when using an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):</span></span>

* <span data-ttu-id="b101e-204">Используйте путь корневого каталога содержимого.</span><span class="sxs-lookup"><span data-stu-id="b101e-204">Use the content root path.</span></span> <span data-ttu-id="b101e-205">`IHostingEnvironment.ContentRootPath` — это тот же путь, предоставленный аргументом `binPath` при создании службы.</span><span class="sxs-lookup"><span data-stu-id="b101e-205">The `IHostingEnvironment.ContentRootPath` is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="b101e-206">Вместо использования `Directory.GetCurrentDirectory()` для создания путей к файлам параметров используйте путь корневого каталога содержимого и храните файлы в корневом каталоге содержимого приложения.</span><span class="sxs-lookup"><span data-stu-id="b101e-206">Instead of using `Directory.GetCurrentDirectory()` to create paths to settings files, use the content root path and maintain the files in the app's content root.</span></span>
* <span data-ttu-id="b101e-207">Храните файлы в подходящем месте на диске.</span><span class="sxs-lookup"><span data-stu-id="b101e-207">Store the files in a suitable location on disk.</span></span> <span data-ttu-id="b101e-208">Укажите абсолютный путь с помощью `SetBasePath` к папке, содержащей файлы.</span><span class="sxs-lookup"><span data-stu-id="b101e-208">Specify an absolute path with `SetBasePath` to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b101e-209">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="b101e-209">Additional resources</span></span>

* <span data-ttu-id="b101e-210">[Конфигурация конечных точек Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (включает конфигурацию HTTPS и поддержку SNI)</span><span class="sxs-lookup"><span data-stu-id="b101e-210">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
