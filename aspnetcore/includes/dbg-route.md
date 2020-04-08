## <a name="debug-diagnostics"></a><span data-ttu-id="d4cdd-101">Отладка диагностики</span><span class="sxs-lookup"><span data-stu-id="d4cdd-101">Debug diagnostics</span></span>

<span data-ttu-id="d4cdd-102">Для подробного вывода диагностики построения маршрутов задайте для `Logging:LogLevel:Microsoft` значение `Debug`.</span><span class="sxs-lookup"><span data-stu-id="d4cdd-102">For detailed routing diagnostic output, set `Logging:LogLevel:Microsoft` to `Debug`.</span></span> <span data-ttu-id="d4cdd-103">Например, в среде разработки задайте *appsettings.Development.json*:</span><span class="sxs-lookup"><span data-stu-id="d4cdd-103">For example, in the development environment, set *appsettings.Development.json*:</span></span>

```JSON
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Debug",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  }
}