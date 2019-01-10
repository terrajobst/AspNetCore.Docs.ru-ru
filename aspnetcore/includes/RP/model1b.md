<span data-ttu-id="d4368-101"><!-- THIS INCLUDE USED BY MVC AND RP -->Добавьте в класс `Movie` следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="d4368-101"><!-- THIS INCLUDE USED BY MVC AND RP --> Add the following properties to the `Movie` class:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/Movie.cs?name=snippet1)]

<span data-ttu-id="d4368-102">Класс `Movie` содержит:</span><span class="sxs-lookup"><span data-stu-id="d4368-102">The `Movie` class contains:</span></span>

* <span data-ttu-id="d4368-103">Поле `ID` является обязательным для первичного ключа базы данных.</span><span class="sxs-lookup"><span data-stu-id="d4368-103">The `ID` field is required by the database for the primary key.</span></span>
* <span data-ttu-id="d4368-104">`[DataType(DataType.Date)]`:  Атрибут [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) указывает тип данных (Date).</span><span class="sxs-lookup"><span data-stu-id="d4368-104">`[DataType(DataType.Date)]`:  The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute specifies the type of the data (Date).</span></span> <span data-ttu-id="d4368-105">С этим атрибутом:</span><span class="sxs-lookup"><span data-stu-id="d4368-105">With this attribute:</span></span>

  * <span data-ttu-id="d4368-106">пользователю не требуется вводить сведения о времени в поле даты.</span><span class="sxs-lookup"><span data-stu-id="d4368-106">The user is not required to enter time information in the date field.</span></span>
  * <span data-ttu-id="d4368-107">Отображается только дата, а не время.</span><span class="sxs-lookup"><span data-stu-id="d4368-107">Only the date is displayed, not time information.</span></span>

<span data-ttu-id="d4368-108">[DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) рассматриваются в следующем руководстве.</span><span class="sxs-lookup"><span data-stu-id="d4368-108">[DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) are covered in a later tutorial.</span></span>