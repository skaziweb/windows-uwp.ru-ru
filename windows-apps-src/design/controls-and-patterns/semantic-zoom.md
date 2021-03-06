---
Description: Элемент управления Semantic Zoom дает пользователю возможность изменять масштаб, переходя от одного семантического представления одного и того же набора данных к другому.
title: Контекстное масштабирование
ms.assetid: B5C21FE7-BA83-4940-9CC1-96F6A2DC28C7
label: Semantic zoom
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: predavid
design-contact: kimsea
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 35613366078531a9e342df0b4314d049fde844e9
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "80081566"
---
# <a name="semantic-zoom"></a>Контекстное масштабирование

 

Контекстное масштабирование позволяет пользователю переключаться между двумя представлениями одного и того же содержимого с разной детализацией и быстро переходить между разделами большого набора сгруппированных данных.
 
- Представление с увеличенным масштабом — это основное представление содержимого. Это основное представление, где вы размещаете отдельные элементы данных. 
- Уменьшенное представления — это более высокий уровень представления того же содержимого. В этом представлении обычно отображаются заголовки группы для набора сгруппированных данных. 

Например, при просмотре адресной книги пользователь может уменьшить масштаб, чтобы быстро перейти к списку имен на букву "Б", а затем, увеличив масштаб представления, просмотреть имена на эту букву. 

> **Важные API**: [класс SemanticZoom](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom), [класс ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView), [класс GridView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView).

**Возможности**:

-   Размер уменьшенного представления не может выходить за границы элемента управления "Контекстное масштабирование".
-   Касание заголовка группы переключает представления. Как способ переключения между представлениями можно включить сжатие.
-   Активные заголовки переключаются между представлениями.

## <a name="is-this-the-right-control"></a>Выбор правильного элемента управления

Используйте элемент управления **SemanticZoom**, если необходимо показывать набор сгруппированных данных, который настолько большой, что его не возможно полностью показать на одной или двух страницах.

Не путайте контекстное масштабирование с оптическим масштабированием. Хотя обе технологии используют одни и те же механизмы взаимодействия и базовое поведение (отображение более или менее детализированного представления с учетом коэффициента масштабирования), оптическим масштабированием называется настройка увеличения какой-либо области содержимого или объекта, например фотографии.  Подробнее об элементах управления для визуального масштабирования см. в разделе об элементе управления [ScrollViewer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer).

## <a name="examples"></a>Примеры

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Если у вас установлено приложение <strong style="font-weight: semi-bold">галереи элементов управления XAML</strong>, щелкните здесь, чтобы <a href="xamlcontrolsgallery:/item/SemanticZoom">открыть приложение и увидеть SemanticZoom в действии</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Получить приложение XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Получить исходный код (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

**Приложение "Фотографии"**

Это контекстное масштабирование в приложении "Фотографии". Фотографии группируются по месяцам. При выборе названия месяца в представлении таблицы по умолчанию изображение уменьшается до представления списка месяцев для ускорения навигации.

![Контекстное масштабирование в приложении "Фотографии"](images/control-examples/semantic-zoom-photos.png)

**Адресная книга**

Адресная книга — еще один пример набора данных, по которому может быть гораздо удобнее перемещаться при помощи контекстного масштабирования. Уменьшенное представление можно использовать для быстрого перехода к определенной букве (левое изображение), а в представлении с увеличенным масштабом отображаются отдельные элементы данных (правое изображение).

![Пример контекстного масштабирования, используемого в списке контактов](images/semanticzoom-win10.png)

## <a name="create-a-semantic-zoom"></a>Создание контекстного масштабирования

У элемента управления **SemanticZoom** нет собственного визуального представления. Это размещаемый элемент управления, который управляет переходами между двумя другими элементами управления, позволяющими просматривать содержимое. Как правило, это элементы **ListView** или **GridView**.  Для элементов управления просмотром необходимо задать свойства [ZoomedInView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.semanticzoom.zoomedinview) и [ZoomedOutView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.semanticzoom.zoomedoutview) объекта SemanticZoom.

Для контекстного масштабирования необходимы три элемента:
- Сгруппированный источник данных (группы задаются определением GroupStyle в масштабированном представлении).
- Увеличенное представление, в котором отображаются данные на уровне элемента.
- Уменьшенное представление, в котором отображаются данные уровня группы.

Прежде чем использовать контекстное масштабирование, вы должны знать, как использовать представление списка со сгруппированными данными. Дополнительные сведения см. в статье о [представлении списка и сетки](listview-and-gridview.md). 

> **Примечание.** &nbsp;&nbsp;Чтобы определить увеличенное и уменьшенное представление элемента управления SemanticZoom, можно использовать любые два элемента управления, реализующие интерфейс [ISemanticZoomInformation](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ISemanticZoomInformation). Платформа XAML предоставляет три элемента управления, которые реализуют этот интерфейс: ListView, GridView и Hub.
 
 Данный пример XAML демонстрирует структуру элемента управления SemanticZoom. Другие элементы управления назначаются свойствам ZoomedInView и ZoomedOutView.
 
 ```xaml
<SemanticZoom>
    <SemanticZoom.ZoomedInView>
        <!-- Put the GridView for the zoomed in view here. -->   
    </SemanticZoom.ZoomedInView>

    <SemanticZoom.ZoomedOutView>
        <!-- Put the ListView for the zoomed out view here. -->       
    </SemanticZoom.ZoomedOutView>
</SemanticZoom>
 ```
 
Приведенные здесь примеры взяты со страницы SemanticZoom [Пример базовых характеристик пользовательского интерфейса XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics). Чтобы просмотреть код полностью, включая источник данных, нужно загрузить этот пример. Это контекстное масштабирование использует класс GridView для представления увеличенного представления, а класс ListView — для уменьшенного представления.
  
**Определение увеличенного представления**

Это элемент управления GridView для увеличенного представления. Увеличенное представление должно отображать отдельные элементы данных в группах. В этом примере показано, как отображать элементы в сетке с изображением и текстом. 

```xaml
<SemanticZoom.ZoomedInView>
    <GridView ItemsSource="{x:Bind cvsGroups.View}" 
              ScrollViewer.IsHorizontalScrollChainingEnabled="False" 
              SelectionMode="None" 
              ItemTemplate="{StaticResource ZoomedInTemplate}">
        <GridView.GroupStyle>
            <GroupStyle HeaderTemplate="{StaticResource ZoomedInGroupHeaderTemplate}"/>
        </GridView.GroupStyle>
    </GridView>
</SemanticZoom.ZoomedInView>
```
 
Внешний вид заголовков группы определяется в ресурсе `ZoomedInGroupHeaderTemplate`. Внешний вид элементов определяется в ресурсе `ZoomedInTemplate`. 

```xaml
<DataTemplate x:Key="ZoomedInGroupHeaderTemplate" x:DataType="data:ControlInfoDataGroup">
    <TextBlock Text="{x:Bind Title}" 
               Foreground="{ThemeResource ApplicationForegroundThemeBrush}" 
               Style="{StaticResource SubtitleTextBlockStyle}"/>
</DataTemplate>

<DataTemplate x:Key="ZoomedInTemplate" x:DataType="data:ControlInfoDataItem">
    <StackPanel Orientation="Horizontal" MinWidth="200" Margin="12,6,0,6">
        <Image Source="{x:Bind ImagePath}" Height="80" Width="80"/>
        <StackPanel Margin="20,0,0,0">
            <TextBlock Text="{x:Bind Title}" 
                       Style="{StaticResource BaseTextBlockStyle}"/>
            <TextBlock Text="{x:Bind Subtitle}" 
                       TextWrapping="Wrap" HorizontalAlignment="Left" 
                       Width="300" Style="{StaticResource BodyTextBlockStyle}"/>
        </StackPanel>
    </StackPanel>
</DataTemplate>
```

**Определение уменьшенного представления**

Этот код XAML определяет элемент управления ListView для уменьшенного представления. Этот пример показывает, как отобразить заголовки группы в качестве текста в списке.

```xaml
<SemanticZoom.ZoomedOutView>
    <ListView ItemsSource="{x:Bind cvsGroups.View.CollectionGroups}" 
              SelectionMode="None" 
              ItemTemplate="{StaticResource ZoomedOutTemplate}" />
</SemanticZoom.ZoomedOutView>
```

 Внешний вид определяется в ресурсе `ZoomedOutTemplate`.
 
```xaml    
<DataTemplate x:Key="ZoomedOutTemplate" x:DataType="wuxdata:ICollectionViewGroup">
    <TextBlock Text="{x:Bind Group.(data:ControlInfoDataGroup.Title)}" 
               Style="{StaticResource SubtitleTextBlockStyle}" TextWrapping="Wrap"/>
</DataTemplate>
```

**Синхронизация представлений**

Представления с увеличенным и с уменьшенным масштабом следует синхронизировать. Тогда, если пользователь выбирает группу в представлении с уменьшенным масштабом, подробная информация о той же группе выводится в представлении с увеличенным масштабом. Для синхронизации представлений можно использовать класс [CollectionViewSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) или добавить код.

Любые элементы управления, привязанные к одному и тому же классу CollectionViewSource, будут всегда иметь один и тот же текущий элемент. Если оба представления используют один и тот же CollectionViewSource в качестве источника данных, CollectionViewSource синхронизирует представления автоматически. Дополнительные сведения см. в разделе [CollectionViewSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource).

Если вы не используете CollectionViewSource для синхронизации представлений, вы должны обработать событие [ViewChangeStarted](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.semanticzoom.viewchangestarted) и синхронизировать элементы в обработчике событий, как показано здесь.

```xaml
<SemanticZoom x:Name="semanticZoom" ViewChangeStarted="SemanticZoom_ViewChangeStarted">
```

```csharp
private void SemanticZoom_ViewChangeStarted(object sender, SemanticZoomViewChangedEventArgs e)
{
    if (e.IsSourceZoomedInView == false)
    {
        e.DestinationItem.Item = e.SourceItem.Item;
    }
}
```

## <a name="recommendations"></a>Рекомендации

-   При использовании контекстного масштабирования в приложении убедитесь, что макет элемента и направление сдвига не зависят от уровня масштабирования. Макеты и сдвиг в обоих режимах масштабирования должны быть последовательными и предсказуемыми.
-   Контекстное масштабирование позволяет пользователю быстро переходить к необходимому содержимому, поэтому ограничьте число страниц или экранов в режиме масштабирования до трех. Необходимость выполнения большого количества сдвигов снижает практическую ценность контекстного масштабирования.
-   Не используйте контекстное масштабирование для изменения объема содержимого. Например, фотоальбом не должен переходить в режим представления папок в проводнике.
-   Используйте структуру и семантику, необходимые для представления.
-   Используйте групповые названия для элементов, объединенных в коллекцию.
-   Применяйте упорядочение для коллекции несгруппированных, но упорядоченных элементов (например, хронологический порядок для дат и алфавитный порядок для списка имен).


## <a name="get-the-sample-code"></a>Получение примера кода

- [Пример из коллекции элементов управления XAML](https://github.com/Microsoft/Xaml-Controls-Gallery) — ознакомьтесь со всеми элементами управления XAML в интерактивном режиме.


## <a name="related-articles"></a>Похожие статьи

- [Основы проектирования навигации](../basics/navigation-basics.md)
- [Представления списка и сетки](listview-and-gridview.md)
- [Контейнеры и шаблоны элементов](item-containers-templates.md)





