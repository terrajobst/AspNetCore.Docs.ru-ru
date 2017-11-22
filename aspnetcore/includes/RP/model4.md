<span data-ttu-id="96455-101">В следующей таблице представлены параметры генераторов кода ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="96455-101">The following table details the ASP.NET Core code generators\` parameters:</span></span>

| <span data-ttu-id="96455-102">Параметр</span><span class="sxs-lookup"><span data-stu-id="96455-102">Parameter</span></span>               | <span data-ttu-id="96455-103">Описание</span><span class="sxs-lookup"><span data-stu-id="96455-103">Description</span></span>|
| ----------------- | ------------ |
| <span data-ttu-id="96455-104">-m</span><span class="sxs-lookup"><span data-stu-id="96455-104">-m</span></span>  | <span data-ttu-id="96455-105">Имя модели</span><span class="sxs-lookup"><span data-stu-id="96455-105">The name of the model.</span></span> |
| <span data-ttu-id="96455-106">-dc</span><span class="sxs-lookup"><span data-stu-id="96455-106">-dc</span></span>  | <span data-ttu-id="96455-107">Контекст данных</span><span class="sxs-lookup"><span data-stu-id="96455-107">The data context.</span></span> |
| <span data-ttu-id="96455-108">-udl</span><span class="sxs-lookup"><span data-stu-id="96455-108">-udl</span></span> | <span data-ttu-id="96455-109">Использование макета по умолчанию</span><span class="sxs-lookup"><span data-stu-id="96455-109">Use the default layout.</span></span> |
| <span data-ttu-id="96455-110">-outDir</span><span class="sxs-lookup"><span data-stu-id="96455-110">-outDir</span></span> | <span data-ttu-id="96455-111">Относительный путь к папке выходных данных для создания представлений</span><span class="sxs-lookup"><span data-stu-id="96455-111">The relative output folder path to create the views.</span></span> |
| <span data-ttu-id="96455-112">--referenceScriptLibraries</span><span class="sxs-lookup"><span data-stu-id="96455-112">--referenceScriptLibraries</span></span> | <span data-ttu-id="96455-113">Добавляет `_ValidationScriptsPartial` для редактирования и создания страниц.</span><span class="sxs-lookup"><span data-stu-id="96455-113">Adds `_ValidationScriptsPartial` to Edit and Create pages</span></span> |

<span data-ttu-id="96455-114">Чтобы получить справку по команде `aspnet-codegenerator razorpage`, используйте коммутатор `h`.</span><span class="sxs-lookup"><span data-stu-id="96455-114">Use the `h` switch to get help on the `aspnet-codegenerator razorpage` command:</span></span>

```console
dotnet aspnet-codegenerator razorpage -h
```
<a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="96455-115">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="96455-115">Test the app</span></span>

* <span data-ttu-id="96455-116">Запустите приложение и добавьте `/Movies` к URL-адресу в браузере (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="96455-116">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="96455-117">Протестируйте ссылку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="96455-117">Test the **Create** link.</span></span>

 ![Страница "Создать"](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="96455-119">Протестируйте ссылки **Изменить**, **Сведения** и **Удалить**.</span><span class="sxs-lookup"><span data-stu-id="96455-119">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="96455-120">Если появляется ошибка, аналогичная приведенной ниже, убедитесь, что выполнены миграции и база данных обновлена:</span><span class="sxs-lookup"><span data-stu-id="96455-120">If you get the error similar to the following, verify you have run migrations and updated the database:</span></span>

```
An unhandled exception occurred while processing the request.
'no such table: Movie'.
```
