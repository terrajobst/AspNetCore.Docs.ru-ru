Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

В этом разделе мы добавим классы для управления фильмами в базе данных. Эти классы используются в [Entity Framework Core](/ef/core) (EF Core) для работы с базой данных. EF Core —это платформа объектно реляционного сопоставления (ORM), которая позволяет упростить необходимый код доступа к данным.

Создаваемые вами классы моделей называются классами POCO (от "plain old CLR objects" — старые добрые объекты CLR), так как они не зависят от EF Core. Эти классы определяют свойства данных, которые хранятся в базе данных.

Работая с этим учебником, вы напишете классы моделей, а затем EF Core создаст базу данных. Другой вариант, который здесь не рассматривается: [создание классов моделей из существующей базы данных](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db).

[Просмотрите или скачайте](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) пример.
