---
author: mijacobs
Description: Use fade animations to bring items into a view or to take items out of a view. The two common fade animations are fade-in and fade-out.
title: Анимации исчезания в приложениях UWP
ms.assetid: 975E5EE3-EFBE-4159-8D10-3C94143DD07F
label: Motion--fades
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 211e0b6dc2c6e2872ca20d4711ef6d41d4974e5c
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/15/2018
ms.locfileid: "1652463"
---
# <a name="fade-animations"></a>Анимации исчезания



Анимация угасания используется для ввода элементов в поле зрения и вывода из него. Две распространенные анимации такого типа— появление и исчезание.

> **Важные API-интерфейсы**: [**класс FadeInThemeAnimation **](https://msdn.microsoft.com/library/windows/apps/br210298), [**класс FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210302)


## <a name="dos-and-donts"></a>Рекомендации


-   При переходе приложения между несвязанными элементами или элементами, содержащими большой объем текста, используйте исчезание с последующим появлением. Это позволит исходящему объекту полностью исчезнуть, прежде чем входящий объект станет видимым.
-   Отобразите входящий элемент или элементы поверх исходящих элементов, если размер элементов остается постоянным и требуется сделать так, чтобы пользователь думал, что смотрит на один и тот же элемент. После завершения прорисовки входящего элемента исходящий элемент можно удалить. Этот вариант работает, только если исходящий элемент полностью закрывается входящим элементом.
-   Избегайте анимации исчезания для добавления или удаления элементов в списке. Вместо этого используйте анимации списка, созданные для этой цели.
-   Избегайте анимации исчезания для изменения всего содержимого страницы. Вместо этого используйте анимации перехода страницы, созданные для этой цели.
-   Исчезание— это тонкий способ удалить элемент.
## <a name="related-articles"></a>Еще по теме

* [Обзор анимаций](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Анимация появления или исчезания](https://msdn.microsoft.com/library/windows/apps/xaml/jj649429)
* [Краткое руководство: анимация пользовательского интерфейса с помощью анимаций библиотеки](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**Класс FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210298)
* [**Класс FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210302)

 

 



