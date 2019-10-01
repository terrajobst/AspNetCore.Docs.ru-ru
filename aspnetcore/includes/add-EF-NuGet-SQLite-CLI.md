<span data-ttu-id="0745f-101">Выполните следующие команды интерфейса командной строки .NET Core:</span><span class="sxs-lookup"><span data-stu-id="0745f-101">Run the following .NET Core CLI commands:</span></span>

```dotnetcli
dotnet tool install --global dotnet-ef
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="0745f-102">Приведенные выше команды добавляют следующие компоненты:</span><span class="sxs-lookup"><span data-stu-id="0745f-102">The preceding commands add:</span></span>

* <span data-ttu-id="0745f-103">[средство формирования шаблонов](xref:fundamentals/tools/dotnet-aspnet-codegenerator);</span><span class="sxs-lookup"><span data-stu-id="0745f-103">The [aspnet-codegenerator scaffolding tool](xref:fundamentals/tools/dotnet-aspnet-codegenerator).</span></span>
* <span data-ttu-id="0745f-104">средства Entity Framework Core для .NET Core CLI;</span><span class="sxs-lookup"><span data-stu-id="0745f-104">The Entity Framework Core Tools for the .NET Core CLI.</span></span>
* <span data-ttu-id="0745f-105">поставщик EF Core для SQLite, который устанавливает пакет EF Core в качестве зависимости;</span><span class="sxs-lookup"><span data-stu-id="0745f-105">The EF Core SQLite provider, which installs the EF Core package as a dependency.</span></span>
* <span data-ttu-id="0745f-106">Пакеты, необходимые для формирования шаблонов: `Microsoft.VisualStudio.Web.CodeGeneration.Design` и `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="0745f-106">Packages needed for scaffolding: `Microsoft.VisualStudio.Web.CodeGeneration.Design` and `Microsoft.EntityFrameworkCore.SqlServer`.</span></span>
