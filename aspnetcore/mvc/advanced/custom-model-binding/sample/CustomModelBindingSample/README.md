# <a name="custom-model-binding-demo"></a>Демонстрация пользовательской привязки модели

Чтобы протестировать `ByteArrayModelBinder`, можно запустить приложение и выполнить запрос POST для отправки строки в кодировке base64 в конечную точку ImageController (/api/image/). В теле запроса необходимо задать свойства файла и имени файла в виде данных формы (с помощью Postman или аналогичного средства). Можно использовать [этот пример строки](Base64String.txt). Результат будет сохранен в папке wwwroot/images/upload под указанным именем файла.

Чтобы протестировать пример пользовательской привязки, попробуйте следующие конечные точки: /api/authors/1 /api/authors/2 (NOT FOUND) /api/boundauthors/1 /api/boundauthors/2 (NOT FOUND) /api/boundauthors/get/1 /api/boundauthors/get/2 (NO CONTENT) — это действие не проверяет значение NULL и возвращает ошибку "Не найдено"
