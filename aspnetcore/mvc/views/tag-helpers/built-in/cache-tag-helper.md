---
title: Вспомогательная функция тегов кэша в MVC-моделях ASP.NET Core
author: pkellner
description: Сведения о работе со вспомогательной функцией тега кэша
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: 6f19a989c9bdfddea7609c5571cdd49de29e036b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a>Вспомогательная функция тегов кэша в MVC-моделях ASP.NET Core

Автор: [Питер Кельнер (Peter Kellner)](http://peterkellner.net) 

Вспомогательная функция тегов кэша позволяет существенно повысить производительность приложения ASP.NET Core за счет кэширования его содержимого во внутренний поставщик кэша ASP.NET Core.

Модуль представлений Razor задает для `expires-after` по умолчанию значение 20 минут.

Приведенная ниже разметка Razor кэширует значения даты и времени:

```cshtml
<cache>@DateTime.Now</cache>
```

Первый запрос к странице, содержащей `CacheTagHelper`, отобразит текущую дату и время. Последующие запросы будут показывать кэшированное значение, пока срок действия кэша не истечет (по умолчанию — 20 минут) или пока кэш не будет удален из-за нехватки памяти.

Вы можете задать продолжительность хранения кэша с помощью следующих атрибутов:

## <a name="cache-tag-helper-attributes"></a>Атрибуты вспомогательной функции тегов кэша

- - -

### <a name="enabled"></a>enabled    


| Тип атрибута    | Допустимые значения      |
|----------------   |----------------   |
| boolean           | "true" (по умолчанию)  |
|                   | "false"   |


Определяет, кэшируется ли содержимое, охватываемое вспомогательной функцией тегов кэша. Значение по умолчанию — `true`.  Если задано значение `false`, функция не применяет кэширование к выводимым данным.

Пример

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-on"></a>expires-on 

| Тип атрибута |           Пример значения            |
|----------------|------------------------------------|
| DateTimeOffset | "@new DateTime(2025,1,29,17,02,0)" |

Задает абсолютную дату окончания срока действия. В следующем примере содержимое вспомогательной функции тегов кэша будет кэшировано до 29 января 2025 г., 17:02.

Пример

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-after"></a>expires-after

| Тип атрибута |        Пример значения         |
|----------------|------------------------------|
|    TimeSpan    | "@TimeSpan.FromSeconds(120)" |

Задает интервал времени для кэширования содержимого с момента первого запроса. 

Пример

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-sliding"></a>expires-sliding

| Тип атрибута |        Пример значения        |
|----------------|-----------------------------|
|    TimeSpan    | "@TimeSpan.FromSeconds(60)" |

Задает время, по истечении которого запись кэша следует удалить, если к ней не было обращений.

Пример

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-header"></a>vary-by-header

| Тип атрибута    | Примеры значений                |
|----------------   |----------------               |
| String            | "User-Agent"                  |
|                   | "User-Agent,content-encoding" |

Принимает одно значение заголовка или список разделенных запятыми значений заголовков, запускающих обновление кэша при их изменении. Следующий пример показывает отслеживание значения заголовка `User-Agent`. Содержимое будет кэшироваться для каждого отдельного заголовка `User-Agent`, представленного на веб-сервере.

Пример

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-query"></a>vary-by-query

| Тип атрибута    | Примеры значений                |
|----------------   |----------------               |
| String            | "Make"                |
|                   | "Make,Model" |

Принимает одно значение заголовка или список разделенных запятыми значений заголовков, запускающих обновление кэша при их изменении. Следующий пример рассматривает значения `Make` и `Model`.

Пример

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-route"></a>vary-by-route

| Тип атрибута    | Примеры значений                |
|----------------   |----------------               |
| String            | "Make"                |
|                   | "Make,Model" |

Принимает одно значение заголовка или список разделенных запятыми значений заголовков, запускающих обновление кэша при изменении значений параметров данных маршрута. Пример

*Startup.cs* 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```

*Index.cshtml*

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-cookie"></a>vary-by-cookie

| Тип атрибута    | Примеры значений                |
|----------------   |----------------               |
| String            | ".AspNetCore.Identity.Application"                |
|                   | ".AspNetCore.Identity.Application,HairColor" |

Принимает одно значение заголовка или список разделенных запятыми значений заголовков, запускающих обновление кэша при их изменении. Следующий пример рассматривает файл cookie, связанный с ASP.NET Identity. Когда пользователь проходит проверку подлинности, запрос задает файл cookie, запускающий обновление кэша.

Пример

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-user"></a>vary-by-user

| Тип атрибута    | Примеры значений                |
|----------------   |----------------               |
| Boolean             | "true"                  |
|                     | "false" (по умолчанию) |

Указывает, следует ли сбрасывать кэш при изменении вошедшего в систему пользователя (или участника контекста). Текущий пользователь также называется участником контекста запроса и доступен для просмотра в представлении Razor с помощью ссылки на `@User.Identity.Name`.

В следующем примере показан текущий пользователь, выполнивший вход.  

Пример

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

При использовании этого атрибута содержимое сохраняется в кэше в течение цикла входа и выхода.  При использовании `vary-by-user="true"` действие входа или выхода сбрасывает кэш для прошедшего проверку подлинности пользователя.  Кэш становится недействительным, так как при входе в систему создается уникальное значение файла cookie. Кэш сохраняется в состоянии анонимности, когда значения cookie не существует или срок его действия истек. Это означает, что когда никто не вошел в систему, кэш будет сохраняться.

- - -

### <a name="vary-by"></a>vary-by

| Тип атрибута | Примеры значений |
|----------------|----------------|
|     String     |    "@Model"    |

Позволяет настраивать, какие данные кэшируются. Содержимое вспомогательной функции тегов кэша обновляется при изменении объекта, на который ссылается строковое значение атрибута. Часто этому атрибуту назначается объединенная строка значений модели.  По сути это означает, что обновление любого из объединенных значений приводит к сбросу кэша.

В следующем примере предполагается, что метод контроллера, визуализирующий представление, суммирует целочисленные значения двух параметров маршрута (`myParam1` и `myParam2`) и возвращает итог как одно свойство модели. При изменении этой суммы содержимое вспомогательной функции тегов кэша визуализируется и кэшируется заново.  

Пример

Действие:

```csharp
public IActionResult Index(string myParam1,string myParam2,string myParam3)
{
    int num1;
    int num2;
    int.TryParse(myParam1, out num1);
    int.TryParse(myParam2, out num2);
    return View(viewName, num1 + num2);
}
```

*Index.cshtml*

```cshtml
<cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="priority"></a>priority

| Тип атрибута    | Примеры значений                |
|----------------   |----------------               |
| CacheItemPriority  | "High"                   |
|                    | "Low" |
|                    | "NeverRemove" |
|                    | "Normal" |

Предоставляет встроенному поставщику кэша инструкции по удалению кэша. При нехватке памяти веб-сервер будет первыми удалять записи кэша с приоритетом `Low`.

Пример

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

Атрибут `priority` не гарантирует определенный уровень периода удержания кэша. `CacheItemPriority` носит лишь рекомендательный характер. Установка для этого атрибута значения `NeverRemove` не гарантирует постоянное хранение кэша. См. [Дополнительные ресурсы](#additional-resources).

Вспомогательная функция тегов кэша зависит от [службы кэширования в памяти](xref:performance/caching/memory). Вспомогательная функция тегов кэша добавляет эту службу, если она не добавлена.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Кэш в памяти](xref:performance/caching/memory)
* [Общие сведения об Identity](xref:security/authentication/identity)
