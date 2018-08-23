---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 'Часть 6: Членства ASP.NET | Документация Майкрософт'
author: JoeStagner
description: В этой серии руководств описаны все действия, предпринимаемые для создайте пример приложения Tailspin Spyworks. Часть 6 добавляет членства ASP.NET.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: c847db058bc03115210f12eeb0c3c76fecc8a31e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838433"
---
<a name="part-6-aspnet-membership"></a>Часть 6: Членства ASP.NET
====================
по [(Joe Stagner)](https://github.com/JoeStagner)

> Tailspin Spyworks демонстрирует, как чрезвычайно прост процесс создания мощных и масштабируемых приложений для платформы .NET. Он демонстрирует способы использования новые возможности в ASP.NET 4 для создание Интернет-магазина, включая покупок, извлечения и администрирования.
> 
> В этой серии руководств описаны все действия, предпринимаемые для создайте пример приложения Tailspin Spyworks. Часть 6 добавляет членства ASP.NET.


## <a id="_Toc260221672"></a>  Работа с ASP.NET Membership

![](tailspin-spyworks-part-6/_static/image1.png)

Нажмите кнопку безопасности

![](tailspin-spyworks-part-6/_static/image1.jpg)

Убедитесь, что мы используем проверку подлинности форм.

![](tailspin-spyworks-part-6/_static/image2.jpg)

Используйте ссылку «Создать пользователя» для создания нескольких пользователей.

![](tailspin-spyworks-part-6/_static/image3.jpg)

После этого, см. в окне обозревателя решений и обновить представление.

![](tailspin-spyworks-part-6/_static/image2.png)

Обратите внимание, что ASPNETDB. Будет создана MDF нормально. Этот файл содержит таблицы для поддержки служб ASP.NET core как членства.

Теперь мы можем начать процесс подсчета стоимости покупок реализации.

Начните с создания CheckOut.aspx страницы.

На странице CheckOut.aspx только должны быть доступны для пользователей, которые выполнили вход, поэтому мы будем ограничивать доступ к пользователям и перенаправляет пользователей, которые не вошли на страницу входа.

Для этого мы добавим следующее в раздел конфигурации из нашего файла web.config.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

Шаблон для приложений ASP.NET Web Forms автоматически добавить раздел проверки подлинности в наш файл web.config и установить страницы входа по умолчанию.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

Нам необходимо изменить Login.aspx файл кода программной части для переноса анонимный корзины, при входе пользователя. Изменение страницы\_загрузить событий следующим образом.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

Затем добавьте обработчик событий «LoggedIn» следующим образом присвоено вновь пользователь имя сеанса и изменить его временного сеанса в корзине пользователя путем вызова метода MigrateCart в нашем классе MyShoppingCart. (Реализованный в CS-файл)

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

Реализуйте метод MigrateCart() следующим образом.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

В checkout.aspx мы будем использовать EntityDataSource и GridView в нашей извлечение страницы так же, как мы делали в нашей корзину для покупок.

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

Обратите внимание, что наш элемент управления GridView указывает обработчик событий «ondatabound», с именем MyList\_RowDataBound так что давайте реализуем этот обработчик событий следующим образом.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

Этот метод сохраняет, промежуточных итогов по покупок корзину, как каждая строка привязан и обновляет нижняя строка GridView.

На этом этапе мы реализовали заказа должна быть помещена презентации «review».

Давайте обрабатывать сценарии пустой корзины для покупок, добавив несколько строк кода к нашей странице\_событие загрузки:

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

Когда пользователь нажимает кнопку «Отправить» мы выполним следующий код в обработчике события Click кнопки отправки.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

Для реализации в метод SubmitOrder() нашего класса MyShoppingCart является «Основу» процесс отправки заказа.

SubmitOrder выполняет следующие действия:

- Примите все элементы в Корзина для покупок и использовать их для создания новой записи заказа и связанные записи OrderDetails.
- Вычисления, дата отгрузки.
- Очистите корзину для покупок.


[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

Для целей этого примера приложения мы вычисляем Дата отгрузки, просто добавляя двух дней до текущей даты.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

Запуск приложения теперь позволит нам протестировать корзину процесс от начала до конца.

> [!div class="step-by-step"]
> [Назад](tailspin-spyworks-part-5.md)
> [Вперед](tailspin-spyworks-part-7.md)
