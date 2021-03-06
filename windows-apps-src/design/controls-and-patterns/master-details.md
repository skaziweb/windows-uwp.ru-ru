---
Description: В шаблоне основных и подробных данных отображается основной список и подробные сведения о выбранном элементе. Этот шаблон часто используется для работы с почтой, списками контактов и адресными книгами.
title: Основные и подробные данные
ms.assetid: 45C9FE8B-ECA6-44BF-8DDE-7D12ED34A7F7
label: Master/details
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5ae8094ac3fbb1de8958b1cc138953d3e1b887cc
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970389"
---
# <a name="masterdetails-pattern"></a>Шаблон основных и подробных данных

 

В шаблоне основных и подробных данных есть главная панель (обычно с [представлением списка](lists.md)) и область сведений для содержимого. При выборе элемента в главном списке область сведений обновляется. Этот шаблон часто используется для работы с электронной почтой и адресными книгами.

> **Важные API**: [класс ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView), [класс SplitView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.splitview)

![Пример шаблона основных и подробных данных](images/HIGSecOne_MasterDetail.png)

> [!TIP]
> Если вы хотите использовать элемент управления XAML, который реализует этот шаблон, мы рекомендуем выбрать [элемент управления XAML MasterDetailsView](/windows/communitytoolkit/controls/masterdetailsview) из набора средств сообщества Windows.

## <a name="is-this-the-right-pattern"></a>Подходит ли этот шаблон?

Шаблон основных и подробных данных подходит, если вам нужно сделать следующее:

-   создать приложение электронной почты, адресной книги или любого приложения на основе макета списка;
-   найти и назначить приоритеты для большой коллекции содержимого;
-   разрешить быстрое добавление и удаление элементов из списка при работе между контекстами;

## <a name="choose-the-right-style"></a>выбрать правильный стиль.

При реализации шаблона основных и подробных данных рекомендуется использовать стиль "стопкой" или "рядом" в зависимости от доступного места на экране.

| Доступная ширина окна | Рекомендуемый стиль |
|------------------------|-------------------|
| 320 epx-640 epx        | Стопкой           |
| 641 epx или шире       | Рядом      |

 
## <a name="stacked-style"></a>Стиль "стопкой"

При использовании расположения стопкой отображается только одна панель: главная или область сведений.

![Основные данные в режиме отображения стопкой](images/patterns-md-stacked.png)

Пользователь начинает работу с главной панели и разворачивает сведения до нужной области, выбирая элемент в главном списке. Пользователям кажется, что основное и подробное представления существуют на двух отдельных страницах.

### <a name="create-a-stacked-masterdetails-pattern"></a>Создание шаблона отображения основных и подробных данных стопкой

Один из способов создания шаблона основных и подробных данных заключается в использовании отдельных страниц для главной панели и области сведений. Разместите представление основной информации на одной странице, а область подробных сведений на другой отдельной странице.

![Части основного и подробного представления с расположением стопкой](images/patterns-md-stacked-parts.png)

На странице основной информации в [представлении списка](lists.md) удобно размещать списки, которые могут содержать изображения и текст. 

На странице с подробными сведениями целесообразнее всего использовать [элемент содержимого](../layout/layout-panels.md). Если есть много отдельных полей, можно использовать макет **Сетка** для размещения элементов в форме.

Сведения о навигации между страницами см. в статье о [журнале навигации и навигации в обратном направлении для приложений Windows](../basics/navigation-history-and-backwards-navigation.md).

## <a name="side-by-side-style"></a>Стиль рядом

В стиле рядом главная панель и область сведений отображается одновременно.

![Шаблон основных и подробных данных](images/patterns-masterdetail-400x227.png)

В списке на главной панели есть визуальный элемент выбора для указания выбранного элемента. При выборе нового элемента в главном списке обновляется область сведений.

### <a name="create-a-side-by-side-masterdetails-pattern"></a>Создание шаблона отображения основных и подробных данных рядом

Чтобы создать шаблон основной информации и подробных сведений, размещенных рядом, можно использовать элемент управления [Комбинированный режим](split-view.md). Разместите представление основной информации в области комбинированного режима, а представление подробных сведений в разделе содержимого комбинированного режима.

![части представлений основной информации и подробных сведений в комбинированном режиме](images/patterns_md_splitview_parts.png)

На главной панели элемент управления [Представление списка](lists.md) хорошо подходит для отображения списков, которые могут содержать изображения и текст.

Для подробных сведений используйте [элемент содержимого](../layout/layout-panels.md), который подходит лучше всего. Если есть много отдельных полей, можно использовать макет **Сетка** для размещения элементов в форме.

## <a name="adaptive-layout"></a>Адаптивный макет

Чтобы реализовать шаблон основной информации и подробных сведений для экрана любого размера, создайте пользовательский интерфейс с быстрым откликом, используя [адаптивный макет](../layout/layouts-with-xaml.md).

![адаптивный макет основной информации и подробных сведений](images/patterns_masterdetail.png)

### <a name="create-an-adaptive-masterdetails-pattern"></a>Создание адаптивного шаблона основной информации и подробных сведений
Чтобы создать адаптивный макет, определите разные состояния [**VisualStates**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.visualstate) для пользовательского интерфейса и объявите точки останова для различных состояний с помощью [**AdaptiveTriggers**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.AdaptiveTrigger).

## <a name="get-the-sample-code"></a>Получение примера кода

В приведенных ниже примерах показана реализация шаблона основной информации и подробных сведений с помощью адаптивных макетов, а также демонстрируется привязывание данных к статическим ресурсам, веб-ресурсам и ресурсам базы данных. 
- [Master/detail sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlMasterDetail) (Пример шаблона основной информации и подробных сведений) 
- [Пример шаблона основной информации и подробных сведений и пример элемента выбора](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView)
- [Пример шаблона основной информации и подробных сведений в Windows Template Studio](https://github.com/Microsoft/WindowsTemplateStudio/tree/master/templates/Uwp/Pages/MasterDetail)
- [Customers Orders Database sample](https://github.com/Microsoft/Windows-appsample-customers-orders-database) (Пример базы данных Customer Orders)
- [RSSReader sample](https://github.com/Microsoft/Windows-appsample-rssreader) (Пример средства чтения RSS)

> [!TIP]
> Если вы хотите использовать элемент управления XAML, который реализует этот шаблон, мы рекомендуем выбрать [элемент управления XAML MasterDetailsView](/windows/communitytoolkit/controls/masterdetailsview) из набора средств сообщества Windows.

## <a name="related-articles"></a>Похожие статьи

- [Списки](lists.md)
- [Поиск](search.md)
- [Панель приложения и панель команд](app-bars.md)
- [ListView class](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) (Класс ListView)
- [SplitView class](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.splitview) (Класс SplitView)
