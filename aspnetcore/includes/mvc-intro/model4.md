<span data-ttu-id="75538-101">В следующей таблице представлены параметры генератора кода ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="75538-101">The following table details the ASP.NET Core code generator parameters:</span></span>

| <span data-ttu-id="75538-102">Параметр</span><span class="sxs-lookup"><span data-stu-id="75538-102">Parameter</span></span>               | <span data-ttu-id="75538-103">Описание</span><span class="sxs-lookup"><span data-stu-id="75538-103">Description</span></span>|
| ----------------- | ------------ |
| <span data-ttu-id="75538-104">-m</span><span class="sxs-lookup"><span data-stu-id="75538-104">-m</span></span>  | <span data-ttu-id="75538-105">Имя модели</span><span class="sxs-lookup"><span data-stu-id="75538-105">The name of the model.</span></span> |
| <span data-ttu-id="75538-106">-dc</span><span class="sxs-lookup"><span data-stu-id="75538-106">-dc</span></span>  | <span data-ttu-id="75538-107">Контекст данных</span><span class="sxs-lookup"><span data-stu-id="75538-107">The data context.</span></span> |
| <span data-ttu-id="75538-108">-udl</span><span class="sxs-lookup"><span data-stu-id="75538-108">-udl</span></span> | <span data-ttu-id="75538-109">Использование макета по умолчанию</span><span class="sxs-lookup"><span data-stu-id="75538-109">Use the default layout.</span></span> |
| <span data-ttu-id="75538-110">--relativeFolderPath</span><span class="sxs-lookup"><span data-stu-id="75538-110">--relativeFolderPath</span></span> | <span data-ttu-id="75538-111">Относительный путь к папке выходных данных для создания представлений</span><span class="sxs-lookup"><span data-stu-id="75538-111">The relative output folder path to create the views.</span></span> |
| <span data-ttu-id="75538-112">--useDefaultLayout</span><span class="sxs-lookup"><span data-stu-id="75538-112">--useDefaultLayout</span></span> | <span data-ttu-id="75538-113">Макет по умолчанию следует использовать для представлений.</span><span class="sxs-lookup"><span data-stu-id="75538-113">The default layout should be used for the views.</span></span> |
| <span data-ttu-id="75538-114">--referenceScriptLibraries</span><span class="sxs-lookup"><span data-stu-id="75538-114">--referenceScriptLibraries</span></span> | <span data-ttu-id="75538-115">Добавляет `_ValidationScriptsPartial` для страниц редактирования и создания</span><span class="sxs-lookup"><span data-stu-id="75538-115">Adds `_ValidationScriptsPartial` to Edit and Create pages</span></span> |

<span data-ttu-id="75538-116">Чтобы получить справку по команде `aspnet-codegenerator controller`, используйте коммутатор `h`.</span><span class="sxs-lookup"><span data-stu-id="75538-116">Use the `h` switch to get help on the `aspnet-codegenerator controller` command:</span></span>

```console
dotnet aspnet-codegenerator controller -h
```