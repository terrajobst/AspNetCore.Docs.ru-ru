## <a name="troubleshoot"></a><span data-ttu-id="fb3fe-101">Диагностика</span><span class="sxs-lookup"><span data-stu-id="fb3fe-101">Troubleshoot</span></span>

### <a name="cookies-and-site-data"></a><span data-ttu-id="fb3fe-102">Файлы cookie и данные сайта</span><span class="sxs-lookup"><span data-stu-id="fb3fe-102">Cookies and site data</span></span>

<span data-ttu-id="fb3fe-103">Файлы cookie и данные сайта могут сохраняться в обновлениях приложений и мешать тестированию и устранению неполадок.</span><span class="sxs-lookup"><span data-stu-id="fb3fe-103">Cookies and site data can persist across app updates and interfere with testing and troubleshooting.</span></span> <span data-ttu-id="fb3fe-104">Очистите следующее при внесении изменений кода приложения, изменения учетной записи пользователя с поставщиком или изменения конфигурации приложения поставщика:</span><span class="sxs-lookup"><span data-stu-id="fb3fe-104">Clear the following when making app code changes, user account changes with the provider, or provider app configuration changes:</span></span>

* <span data-ttu-id="fb3fe-105">Пользовательские файлы cookie-ввоза</span><span class="sxs-lookup"><span data-stu-id="fb3fe-105">User sign-in cookies</span></span>
* <span data-ttu-id="fb3fe-106">Печенье приложений</span><span class="sxs-lookup"><span data-stu-id="fb3fe-106">App cookies</span></span>
* <span data-ttu-id="fb3fe-107">Кэшированные и сохраненные данные сайта</span><span class="sxs-lookup"><span data-stu-id="fb3fe-107">Cached and stored site data</span></span>

<span data-ttu-id="fb3fe-108">Один из подходов к предотвращению затяжных файлов cookie и данных сайта от вмешательства в тестирование и устранение неполадок заключается в:</span><span class="sxs-lookup"><span data-stu-id="fb3fe-108">One approach to prevent lingering cookies and site data from interfering with testing and troubleshooting is to:</span></span>

* <span data-ttu-id="fb3fe-109">Используйте браузер для тестирования, который можно настроить, чтобы удалить все файлы cookie и данные сайта каждый раз, когда браузер закрыт.</span><span class="sxs-lookup"><span data-stu-id="fb3fe-109">Use a browser for testing that you can configure to delete all cookie and site data each time the browser is closed.</span></span>
* <span data-ttu-id="fb3fe-110">Закройте браузер между любыми изменениями в приложении, тестируемым пользователем или конфигурацией поставщика.</span><span class="sxs-lookup"><span data-stu-id="fb3fe-110">Close the browser between any change to the app, test user, or provider configuration.</span></span>

### <a name="run-the-server-app"></a><span data-ttu-id="fb3fe-111">Выполнить приложение "Сервер"</span><span class="sxs-lookup"><span data-stu-id="fb3fe-111">Run the Server app</span></span>

<span data-ttu-id="fb3fe-112">При тестировании и устранении неполадок в размещенном приложении Blazor убедитесь, что вы работаете с приложением из проекта **Server.**</span><span class="sxs-lookup"><span data-stu-id="fb3fe-112">When testing and troubleshooting a hosted Blazor app, make sure that you're running the app from the **Server** project.</span></span> <span data-ttu-id="fb3fe-113">Например, в Visual Studio подтвердите, что проект Server выделен в **Solution Explorer** перед запуском приложения с любым из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="fb3fe-113">For example in Visual Studio, confirm that the Server project is highlighted in **Solution Explorer** before you start the app with any of the following approaches:</span></span>

* <span data-ttu-id="fb3fe-114">Нажмите кнопку **Запустить**.</span><span class="sxs-lookup"><span data-stu-id="fb3fe-114">Select the **Run** button.</span></span>
* <span data-ttu-id="fb3fe-115">Используйте **debug** > **Start Debugging** из меню.</span><span class="sxs-lookup"><span data-stu-id="fb3fe-115">Use **Debug** > **Start Debugging** from the menu.</span></span>
* <span data-ttu-id="fb3fe-116">Нажмите клавишу <kbd>F5</kbd>.</span><span class="sxs-lookup"><span data-stu-id="fb3fe-116">Press <kbd>F5</kbd>.</span></span>
