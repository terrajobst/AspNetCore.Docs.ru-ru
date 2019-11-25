### <a name="use-sqlite-for-development-sql-server-for-production"></a>Использование SQLite для среды разработки и SQL Server для рабочей среды

Если выбрана SQLite, код, созданный шаблоном, будет готов к разработке. В примере кода ниже показано, как внедрить <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> в среду загрузки. Интерфейс `IWebHostEnvironment` внедрен, поэтому `ConfigureServices` может использовать SQLite в среде разработки и SQL Server в рабочей среде.

[!code-csharp[](~/includes/RP/code/StartupDevProd.cs?name=snippet&highlight=5,10,14)]
