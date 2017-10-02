---
title: "Добавление модели в приложение Razor Pages в ASP.NET Core"
author: rick-anderson
description: "Добавление модели в приложение Razor Pages в ASP.NET Core"
keywords: ASP.NET Core,Razor Pages,Razor,MVC
ms.author: riande
manager: wpickett
ms.date: 07/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/model
ms.openlocfilehash: 195128f670223f15e1679c51fb6354c1aa527c01
ms.sourcegitcommit: 3ba32b2b6425ed94604cb0f681db0d5bb5f8ad58
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/28/2017
---
# <a name="adding-a-model-to-a-razor-pages-app"></a>Добавление модели в приложение Razor Pages

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Добавление модели данных

В обозревателе решений щелкните проект **RazorPagesMovie** правой кнопкой мыши и выберите пункт **Добавить** > **Новая папка**. Присвойте папке имя *Models*.

Щелкните правой кнопкой мыши папку *Models*. Выберите **Добавить** > **Класс**. Добавьте класс **Movie** и следующие свойства:

[!INCLUDE[model 2](../../includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a>Добавление строки подключения базы данных

Добавьте строку подключения в файл *appsettings.json*.

[!code-json[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a>Регистрация контекста базы данных

Зарегистрируйте контекст базы данных в контейнере [внедрения зависимостей](xref:fundamentals/dependency-injection) в файле *Startup.cs*.

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-6)]

Выполните сборку проекта, чтобы убедиться в отсутствии ошибок.

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a>Добавление средств формирования шаблонов и выполнение первоначальной миграции

В этом разделе консоль диспетчера пакетов (PMC) используется для выполнения следующих действий.

* Добавление веб-пакета создания кода для Visual Studio. Этот пакет необходим для работы ядра формирования шаблонов.
* Добавление первоначальной миграции.
* Обновления базы данных с помощью первоначальной миграции.

В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**.

  ![Меню PMC](../first-mvc-app/adding-model/_static/pmc.png)

В PMC введите следующие команды.

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.0
Add-Migration Initial
Update-Database
```

Команда `Install-Package` устанавливает средства, необходимые для запуска ядра формирования шаблонов.

Команда `Add-Migration` формирует код для создания схемы исходной базы данных. Схема создается на основе модели, указанной в `DbContext` (в файле *Models/MovieContext.cs*). Аргумент `Initial` используется для присвоения имен миграциям. Можно использовать любое имя, но обычно выбирается имя, описывающее миграцию. Дополнительные сведения см. в статье [Введение в миграции](xref:data/ef-mvc/migrations#introduction-to-migrations).

Команда `Update-Database` выполняет `Up` метод в файле *Migrations/\<отметка-времени>_InitialCreate.cs*, который создает базу данных.

[!INCLUDE[model 4windows](../../includes/RP/model4Win.md)]

[!INCLUDE[model 4](../../includes/RP/model4.md)]

В следующем учебнике рассматриваются файлы, созданные с помощью формирования шаблонов.

>[!div class="step-by-step"]
[Назад: Начало работы](xref:tutorials/razor-pages/razor-pages-start)
[Далее: Сформированные страницы Razor Pages](xref:tutorials/razor-pages/page)    
