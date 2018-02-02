# <a name="custom-webhost-service-sample"></a><span data-ttu-id="9d14d-101">Образец пользовательского WebHost службы</span><span class="sxs-lookup"><span data-stu-id="9d14d-101">Custom WebHost Service Sample</span></span>

<span data-ttu-id="9d14d-102">В этом примере показан способ, рекомендуемый для размещения приложения ASP.NET Core в Windows без использования IIS в качестве службы Windows.</span><span class="sxs-lookup"><span data-stu-id="9d14d-102">This sample shows the recommended way to host an ASP.NET Core app on Windows without using IIS as a Windows Service.</span></span> <span data-ttu-id="9d14d-103">В этом примере демонстрируется функции, описанные в [размещения приложения ASP.NET Core в службе Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span><span class="sxs-lookup"><span data-stu-id="9d14d-103">This sample demonstrates the features described in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

## <a name="instructions"></a><span data-ttu-id="9d14d-104">Инструкции</span><span class="sxs-lookup"><span data-stu-id="9d14d-104">Instructions</span></span>

<span data-ttu-id="9d14d-105">Пример приложения является простой веб-приложения MVC изменить в соответствии с инструкциями в [размещения приложения ASP.NET Core в службе Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span><span class="sxs-lookup"><span data-stu-id="9d14d-105">The sample app is a simple MVC web app modified according to the instructions in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

<span data-ttu-id="9d14d-106">Чтобы запустить приложение в службе, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="9d14d-106">To run the app in a service, perform the following steps:</span></span>

1. <span data-ttu-id="9d14d-107">Создать папку на *c:\svc*.</span><span class="sxs-lookup"><span data-stu-id="9d14d-107">Create a folder at *c:\svc*.</span></span>

1. <span data-ttu-id="9d14d-108">Опубликуйте приложение в папку с `dotnet publish --configuration Release --output c:\\svc`.</span><span class="sxs-lookup"><span data-stu-id="9d14d-108">Publish the app to the folder with `dotnet publish --configuration Release --output c:\\svc`.</span></span> <span data-ttu-id="9d14d-109">Команда переместит активы приложения для папки, включая необходимые `appsettings.json` файла и `wwwroot` папки с его содержимым.</span><span class="sxs-lookup"><span data-stu-id="9d14d-109">The command will move the app's assets to the folder, including the required `appsettings.json` file and the `wwwroot` folder with its contents.</span></span>

1. <span data-ttu-id="9d14d-110">Откройте **администратора** командной оболочке.</span><span class="sxs-lookup"><span data-stu-id="9d14d-110">Open an **administrator** command shell.</span></span>

1. <span data-ttu-id="9d14d-111">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="9d14d-111">Execute the following commands:</span></span>

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

1. <span data-ttu-id="9d14d-112">В браузере, перейдите к `http://localhost:5000` чтобы убедиться, что служба запущена.</span><span class="sxs-lookup"><span data-stu-id="9d14d-112">In a browser, go to `http://localhost:5000` to verify that the service is running.</span></span>

1. <span data-ttu-id="9d14d-113">Чтобы остановить службу, используйте команду:</span><span class="sxs-lookup"><span data-stu-id="9d14d-113">To stop the service, use the command:</span></span>

   ```console
   sc stop MyService
   ```

<span data-ttu-id="9d14d-114">Если приложение не запускается до должным образом при запуске в службе, быстрый способ сделать доступной сообщения об ошибках является добавление регистратора, таких как [поставщик журнала событий Windows](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span><span class="sxs-lookup"><span data-stu-id="9d14d-114">If the app doesn't start up as expected when running in a service, a quick way to make error messages accessible is to add a logging provider, such as the [Windows EventLog provider](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span></span> <span data-ttu-id="9d14d-115">Другой вариант — Проверьте журнал событий приложений, с помощью средства просмотра событий в системе.</span><span class="sxs-lookup"><span data-stu-id="9d14d-115">Another option is to check the Application Event Log using the Event Viewer on the system.</span></span> <span data-ttu-id="9d14d-116">Например вот необработанное исключение для появление ошибки в журнале событий приложений.</span><span class="sxs-lookup"><span data-stu-id="9d14d-116">For example, here's an unhandled exception for a FileNotFound error in the Application Event Log:</span></span>

```console
Application: AspNetCoreService.exe
Framework Version: v4.0.30319
Description: The process was terminated due to an unhandled exception.
Exception Info: System.IO.FileNotFoundException
   at Microsoft.Extensions.Configuration.FileConfigurationProvider.Load(Boolean)
   at Microsoft.Extensions.Configuration.ConfigurationRoot..ctor(System.Collections.Generic.IList`1<Microsoft.Extensions.Configuration.IConfigurationProvider>)
   at Microsoft.Extensions.Configuration.ConfigurationBuilder.Build()
   ...
```
