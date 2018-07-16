# <a name="custom-webhost-service-sample"></a><span data-ttu-id="db1db-101">Пример пользовательской службы WebHost</span><span class="sxs-lookup"><span data-stu-id="db1db-101">Custom WebHost Service Sample</span></span>

<span data-ttu-id="db1db-102">В этом примере показано, как разместить приложение ASP.NET Core как службу Windows, не используя IIS.</span><span class="sxs-lookup"><span data-stu-id="db1db-102">This sample shows how to host an ASP.NET Core app as a Windows Service without using IIS.</span></span> <span data-ttu-id="db1db-103">В этом примере демонстрируется сценарий, описанный в статье [Размещение приложения ASP.NET Core в службе Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span><span class="sxs-lookup"><span data-stu-id="db1db-103">This sample demonstrates the scenario described in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

## <a name="instructions"></a><span data-ttu-id="db1db-104">Инструкции</span><span class="sxs-lookup"><span data-stu-id="db1db-104">Instructions</span></span>

<span data-ttu-id="db1db-105">Пример приложения представляет собой веб-приложение Razor Pages, измененное в соответствии с инструкциями из статьи [О размещении приложения ASP.NET Core в службе Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span><span class="sxs-lookup"><span data-stu-id="db1db-105">The sample app is a Razor Pages web app modified according to the instructions in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

<span data-ttu-id="db1db-106">Чтобы запустить приложение в службе, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="db1db-106">To run the app in a service, perform the following steps:</span></span>

1. <span data-ttu-id="db1db-107">Создайте папку в расположении *c:\svc*.</span><span class="sxs-lookup"><span data-stu-id="db1db-107">Create a folder at *c:\svc*.</span></span>

1. <span data-ttu-id="db1db-108">Опубликуйте приложение в папку, используя `dotnet publish --configuration Release --output c:\\svc`.</span><span class="sxs-lookup"><span data-stu-id="db1db-108">Publish the app to the folder with `dotnet publish --configuration Release --output c:\\svc`.</span></span> <span data-ttu-id="db1db-109">Команда перемещает ресурсы приложения в папку *svc*, включая необходимый файл `appsettings.json` и папку `wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="db1db-109">The command moves the app's assets to the *svc* folder, including the required `appsettings.json` file and the `wwwroot` folder.</span></span>

1. <span data-ttu-id="db1db-110">Откройте командную строку от имени учетной записи **администратора**.</span><span class="sxs-lookup"><span data-stu-id="db1db-110">Open an **administrator** command prompt.</span></span>

1. <span data-ttu-id="db1db-111">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="db1db-111">Execute the following commands:</span></span>

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

  <span data-ttu-id="db1db-112">*Требуется пробел между знаком равенства и началом строки пути.*</span><span class="sxs-lookup"><span data-stu-id="db1db-112">*The space between the equal sign and the start of the path string is required.*</span></span>

1. <span data-ttu-id="db1db-113">В браузере перейдите по адресу `http://localhost:5000` и убедитесь, что служба запущена.</span><span class="sxs-lookup"><span data-stu-id="db1db-113">In a browser, navigate to `http://localhost:5000` and verify that the service is running.</span></span> <span data-ttu-id="db1db-114">Приложение перенаправляет в защищенную конечную точку `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="db1db-114">The app redirects to the secure endpoint `https://localhost:5001`.</span></span>

1. <span data-ttu-id="db1db-115">Чтобы остановить работу службы, используйте эту команду:</span><span class="sxs-lookup"><span data-stu-id="db1db-115">To stop the service, use the command:</span></span>

   ```console
   sc stop MyService
   ```

<span data-ttu-id="db1db-116">Если приложение не запускается должным образом, быстрый способ вывести сообщения об ошибках — добавить регистратор, например [Windows EventLog](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span><span class="sxs-lookup"><span data-stu-id="db1db-116">If the app doesn't start as expected, a quick way to make error messages accessible is to add a logging provider, such as the [Windows EventLog provider](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span></span> <span data-ttu-id="db1db-117">Можно также просмотреть журнал событий приложения, используя компонент "Просмотр событий" в системе.</span><span class="sxs-lookup"><span data-stu-id="db1db-117">Another option is to check the Application Event Log using the Event Viewer on the system.</span></span> <span data-ttu-id="db1db-118">Ниже в качестве примера приводится необработанное исключение для ошибки FileNotFound в журнале событий приложения:</span><span class="sxs-lookup"><span data-stu-id="db1db-118">For example, here's an unhandled exception for a FileNotFound error in the Application Event Log:</span></span>

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
