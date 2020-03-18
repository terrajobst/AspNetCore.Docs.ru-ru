> [!WARNING]
> <span data-ttu-id="313b4-101">В целях безопасности вам следует задать привязку данных запроса `GET` к свойствам страничной модели.</span><span class="sxs-lookup"><span data-stu-id="313b4-101">For security reasons, you must opt in to binding `GET` request data to page model properties.</span></span> <span data-ttu-id="313b4-102">Проверьте введенные данные пользователя, прежде чем сопоставлять их со свойствами.</span><span class="sxs-lookup"><span data-stu-id="313b4-102">Verify user input before mapping it to properties.</span></span> <span data-ttu-id="313b4-103">Привязка `GET` полезна при обращении к сценариям, использующим строку запроса или значения маршрутов.</span><span class="sxs-lookup"><span data-stu-id="313b4-103">Opting into `GET` binding is useful when addressing scenarios that rely on query string or route values.</span></span>
>
> <span data-ttu-id="313b4-104">Чтобы привязать свойство к запросам `GET`, задайте для свойства `SupportsGet` атрибута [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) значение `true`:</span><span class="sxs-lookup"><span data-stu-id="313b4-104">To bind a property on `GET` requests, set the [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) attribute's `SupportsGet` property to `true`:</span></span>
>
> ```csharp
> [BindProperty(SupportsGet = true)]
> ```
>
> <span data-ttu-id="313b4-105">Дополнительные сведения см. в видео, посвященном [обсуждению привязки к запросу GET](https://www.youtube.com/watch?v=p7iHB9V-KVU&feature=youtu.be&t=54m27s), на канале YouTube.</span><span class="sxs-lookup"><span data-stu-id="313b4-105">For more information, see [ASP.NET Core Community Standup: Bind on GET discussion (YouTube)](https://www.youtube.com/watch?v=p7iHB9V-KVU&feature=youtu.be&t=54m27s).</span></span>
