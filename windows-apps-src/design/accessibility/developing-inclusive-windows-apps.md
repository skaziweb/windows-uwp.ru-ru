---
author: Xansky
Description: Learn to develop accessible Windows 10 UWP apps that include keyboard navigation, color and contrast settings, and support for assistive technologies.
ms.assetid: 9311D23A-B340-42F0-BEFE-9261442AF108
title: Разработка инклюзивных приложений для Windows10
label: Developing inclusive Windows 10 apps
template: detail.hbs
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4a8bafb02baf4d70c41cd7a75c283903c05680b0
ms.sourcegitcommit: f9a4854b6aecfda472fb3f8b4a2d3b271b327800
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/12/2017
ms.locfileid: "1394303"
---
# <a name="developing-inclusive-windows-apps"></a>Разработка инклюзивных приложений для Windows  

Эта статья посвящена разработке доступных приложений универсальной платформы Windows (UWP). В частности, предполагается, что вы понимаете, как проектировать логическую иерархию своего приложения. Узнайте, как разрабатывать доступные приложения UWP для Windows10, в которых реализована навигация с помощью клавиатуры, поддержка специальных возможностей, а также присутствуют параметры настройки цвета и контрастности.

Если это не так, начните с изучения раздела [Проектирование инклюзивного программного обеспечения](designing-inclusive-software.md).

Нужно выполнить три задачи, чтобы ваше приложение считалось доступным:

1. предоставить [программный доступ](#programmatic-access) к элементам пользовательского интерфейса;
2. обеспечить поддержку в приложении [навигации с помощью клавиатуры](#keyboard-navigation) для людей, которые не могут использовать мышь или сенсорный экран;
3. убедиться, что ваше приложение поддерживает параметры [цветов и контрастности](#color-and-contrast).

## <a name="programmatic-access"></a>Программный доступ  
Программный доступ критически необходим для реализации специальных возможностей в приложении. Этого можно достичь, задав доступное имя (обязательно) и описание (необязательно) для содержимого и интерактивных элементов пользовательского интерфейса своей программы. Это гарантирует доступность элементов управления пользовательского интерфейса для средств, обеспечивающих специальные возможности, таких как средства чтения с экрана (например, экранный диктор) или альтернативных устройств вывода (например, дисплеи Брайля). Без программного доступа интерфейсы API специальных возможностей не могут правильно интерпретировать информацию, не позволяя пользователю эффективно использовать продукт, или задействуют недокументированные интерфейсы или методы программирования, которые не предназначались для реализации интерфейсов специальных возможностей. Если элементы управления пользовательского интерфейса поддерживают специальные возможности, последние позволяют определять те действия и параметры, которые будут доступны пользователю.  

Дополнительные сведения о поддержке специальных возможностей элементами пользовательского интерфейса приложения см. в разделе [Предоставление основных сведений о специальных возможностях](basic-accessibility-information.md).

## <a name="keyboard-navigation"></a>Навигация с помощью клавиатуры  
Для пользователей с нарушениями зрения или подвижности возможность навигации по пользовательскому интерфейсу с помощью клавиатуры играет очень важную роль. Однако получать фокус клавиатуры должны только те элементы управления, которые необходимы для взаимодействия с пользователем. Компонентам, которые не требуют каких-либо действий, например статическим изображениям, не нужно получать фокус клавиатуры.  

Важно помнить, что, в отличие от навигации с помощью мыши или сенсорного управления, навигация с помощью клавиатуры является линейной. Проектируя возможности навигации с помощью клавиатуры следует оценить, как пользователь будет взаимодействовать с продуктом и какова будет логическая навигация. В западных странах люди читают слева направо и сверху вниз. Поэтому при навигации с помощью клавиатуры, как правило, используется тот же самый принцип.  

Проектируя возможности навигации с помощью клавиатуры, изучите пользовательский интерфейс и рассмотрите следующие вопросы.
* Как эти элементы управления скомпонованы или сгруппированы в пользовательском интерфейсе?
* Имеются ли какие-либо значимые группы элементов управления?
    * Если да, то имеют ли эти группы вложенные группы?
*   Если говорить об одноранговых элементах управления, должна ли навигация осуществляться с помощью табуляции или с помощью специальных навигационных возможностей (клавишей со стрелками), либо же следует задействовать и то, и другое?

Основная цель — помочь пользователям понять, как организован пользовательский интерфейс, а также указать элементы управления, для которых доступны какие-либо действия. Если вы заметите, что для завершения полного цикла навигации пользователю необходимо пройти слишком много точек табуляции, рассмотрите возможность объединения связанных элементов управления в группы. Некоторые связанные элементы управления, например гибридные элементы, необходимо рассмотреть уже на этом этапе. После начала разработки продукта очень трудно переработать навигацию с помощью клавиатуры, поэтому выполняйте тщательное планирование начиная с самых ранних этапов!  

Чтобы узнать больше о навигации по элементам пользовательского интерфейса с помощью клавиатуры, изучите раздел [Специальные возможности клавиатуры](keyboard-accessibility.md).  

Кроме того, в электронной книге [Разработка программного обеспечения с учетом специальных возможностей](https://www.microsoft.com/download/details.aspx?id=19262) есть глава под названием _Проектирование логической иерархии_, которая посвящена этой теме.

## <a name="color-and-contrast"></a>Цвет и контрастность  
Одной из встроенных специальных возможностей Windows является режим высокой контрастности, который повышает цветовую контрастность текста и изображений на экране компьютера. Для некоторых людей увеличение цветовой контрастности позволяет снизить зрительное напряжение и облегчить чтение. При проверке пользовательского интерфейса в режиме высокой контрастности следует проверить, что код элементов управления единообразен и что используются системные (а не жестко заданные) цвета. Это позволит убедиться, что пользователи увидят на экране все те элементы управления, которые доступны без использования режима высокой контрастности.  

XAML
```xaml
<Button Background="{ThemeResource ButtonBackgroundThemeBrush}">OK</Button>
```
Дополнительные сведения об использовании системных цветов и ресурсов см. в разделе [Ресурсы темы XAML](../controls-and-patterns/xaml-theme-resources.md).

Если вы не переопределили системные цвета, приложение UWP по умолчанию поддерживает темы высокой контрастности. Если пользователь выбрал тему с высокой контрастностью в системных параметрах или в средствах специальных возможностей, платформа автоматически использует настройки цветов и стиля, обеспечивающие высокую контрастность макета, элементов управления и компонентов в пользовательском интерфейсе.   

Дополнительные сведения см. в разделе [Темы с высокой контрастностью](high-contrast-themes.md).  

Если вы решили использовать собственную цветовую тему вместо системных цветов, придерживайтесь следующих рекомендаций.  

**Коэффициент цветовой контрастности** — обновленный раздел 508 Закона о защите прав граждан США с ограниченными возможностями, а также другие законодательные акты требуют, чтобы коэффициент цветовой контрастности между текстом и фоном составлял не менее 5:1. Для больших текстов (размер шрифта 18 точек или 14 точек при полужирном начертании) требуемая контрастность по умолчанию составляет 3:1.  

**Сочетание цветов** — около 7 % мужчин (и меньше 1 % женщин) в той или иной форме страдают недостатком цветового зрения. Пользователи с нарушениями цветоощущения не различают определенные цвета, поэтому важно, чтобы сам цвет никогда не передавал какое-либо состояние или значение в приложении. Что касается декоративных изображений (таких как значки или фоны), цветовые сочетания необходимо выбирать таким образом, чтобы максимально повышать восприятие изображения пользователями с нарушениями цветоощущения.  

## <a name="accessibility-checklist"></a>Контрольный список специальных возможностей  
Ниже приведена сокращенная версия контрольного списка специальных возможностей.

1. Задайте доступное имя (обязательно) и описание (необязательно) для содержимого и интерактивных элементов пользовательского интерфейса своей программы.
2. Реализуйте специальные возможности клавиатуры.
3. Визуально проверьте свой пользовательский интерфейс, чтобы удостовериться, что используется адекватный уровень контрастности текста, элементы в темах с высокой контрастностью отображаются правильно, а цвета используются надлежащим образом.
4. Запустите средства специальных возможностей, устраните выявленные проблемы и проверьте процесс чтения с экрана. (См. раздел, посвященный тестированию специальных возможностей.)
5. Убедитесь, что параметры манифеста вашего приложения соответствуют рекомендациям по реализации специальных возможностей.
6. Объявите о специальных возможностях своей программы в Microsoft Store. (См. раздел [Специальные возможности в Store](accessibility-in-the-store.md).)

Дополнительные сведения см. в [полном контрольном списке специальных возможностей](accessibility-checklist.md) в соответствующем разделе.

## <a name="related-topics"></a>Еще по теме  
* [Проектирование инклюзивного программного обеспечения](designing-inclusive-software.md)  
* [Инклюзивное проектирование](http://design.microsoft.com/inclusive)
* [Рекомендации по специальным возможностям, которых следует избегать](practices-to-avoid.md)
* [Создание программного обеспечения с учетом специальных возможностей](https://www.microsoft.com/download/details.aspx?id=19262)
* [Информация о специальных возможностях Microsoft в центре разработчиков](https://msdn.microsoft.com/enable)
* [Специальные возможности](accessibility.md)