<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="08883-101">Модель создания фильма</span><span class="sxs-lookup"><span data-stu-id="08883-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="08883-102">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="08883-102">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="08883-103">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="08883-103">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```