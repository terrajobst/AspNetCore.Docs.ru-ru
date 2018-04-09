---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 'Часть 6: Членства в ASP.NET | Документы Microsoft'
author: JoeStagner
description: Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения Tailspin Spyworks. Часть 6 добавляет членства ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: 83e9bc780ea8face3e0f55fdf8c00e13b60f80a7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="part-6-aspnet-membership"></a>Часть 6: Членства в ASP.NET
====================
по [(Joe Stagner)](https://github.com/JoeStagner)

> Tailspin Spyworks показано, как чрезвычайно простой можно создавать мощные и масштабируемые приложения для платформы .NET. Он показывает как использовать новые возможности в ASP.NET 4 для создания Интернет-магазина, включая покупок, извлечения и администрирования.
> 
> Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения Tailspin Spyworks. Часть 6 добавляет членства ASP.NET.


## <a id="_Toc260221672"></a>  Работа с помощью членства ASP.NET

![](tailspin-spyworks-part-6/_static/image1.png)

Перейдите на вкладку Безопасность

![](tailspin-spyworks-part-6/_static/image1.jpg)

Убедитесь в том, что мы используем проверку подлинности форм.

![](tailspin-spyworks-part-6/_static/image2.jpg)

Используйте ссылку «Создать пользователя» для создания нескольких пользователей.

![](tailspin-spyworks-part-6/_static/image3.jpg)

После этого, см. в окне обозревателя решений и обновить представление.

![](tailspin-spyworks-part-6/_static/image2.png)

Обратите внимание, что ASPNETDB. Был создан MDF нормально. Этот файл содержит таблицы, для поддержки базовых служб ASP.NET, как членство.

Теперь мы можем начать реализации процесса извлечения.

Начните с создания CheckOut.aspx страницы.

На странице CheckOut.aspx только должны быть доступны пользователям, выполнившим вход, мы будет ограничивать доступ к вход пользователей и перенаправляет пользователей, которые не вошли на страницу входа.

Для этого мы добавим следующие раздел конфигурации наш файл web.config.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

Шаблон для приложений веб-форм ASP.NET автоматически в наш файл web.config, добавлен раздел проверки подлинности и установить страницы входа по умолчанию.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

Нам необходимо изменить Login.aspx файле кода программной части для переноса анонимный корзину при входе пользователя. Изменить страницу\_загрузить событие следующим образом.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

Затем добавьте обработчик событий «LoggedIn» как этот параметр, чтобы задать имя сеанса для вновь зарегистрированного в системе пользователя и изменить код временного сеанса в корзину для пользователя путем вызова метода MigrateCart в нашем классе MyShoppingCart. (Реализованный в CS-файл)

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

Реализуйте метод MigrateCart() следующим образом.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

В checkout.aspx мы будем использовать EntityDataSource и GridView в нашем извлечение страницы так же, как было сделано в нашу страницу отзывов корзину для покупок.

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

Обратите внимание, что наш элемент управления GridView указывает обработчик событий «ondatabound» с именем экземпляры MyList\_RowDataBound давайте реализации этого обработчика событий следующим образом.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

Этот метод позволяет сохранить корзину покупок промежуточных итогов как каждая строка привязан и обновляет в нижней строке элемента управления GridView.

На этом этапе мы применили заказа, чтобы разместить презентации «отзыв».

Давайте обрабатывать сценарий пустой корзины для покупок, добавив несколько строк кода нашу страницу\_событие загрузки:

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

При нажатии кнопки «Отправить» мы будет выполните следующий код в обработчик события Click кнопки отправки.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

Для реализации в метод SubmitOrder() класса MyShoppingCart — «Молоко» процесс отправки заказа.

SubmitOrder выполняет следующие действия:

- Принимать все элементы в корзину и использовать их для создания новой записи заказа и связанные записи OrderDetails.
- Вычисления, дата отгрузки.
- Очистите корзину.


[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

Для целей этого образца приложения будет вычислять Дата отгрузки, просто добавляя двух дней до текущей даты.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

Запуск приложения теперь позволит нам для тестирования процесса покупок от начала до конца.

> [!div class="step-by-step"]
> [Назад](tailspin-spyworks-part-5.md)
> [Вперед](tailspin-spyworks-part-7.md)
