---
author: anbare
Description: The following article describes all of the properties and elements within toast content.
title: Схема содержимого всплывающего уведомления
ms.assetid: 7CBC3BD5-D9C3-4781-8BD0-1F28039E1FA8
label: Toast content schema
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e51f9e837b64e019c4b6dc1d87e5bf99f5bcf5bb
ms.sourcegitcommit: eead3c00b27d9f66f79ec08c81a97e91dc1fdb3c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/18/2018
ms.locfileid: "1522953"
---
# <a name="toast-content-schema"></a>Схема содержимого всплывающего уведомления

 

В приведенной ниже статье описываются все свойства и элементы содержимого всплывающего уведомления.

Если вы предпочитаете использовать необработанные данные XML вместо [библиотеки уведомлений](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/), см. [схему XML](toast-xml-schema.md).

[ToastContent](#toastcontent)
* [ToastVisual](#toastvisual)
  * [ToastBindingGeneric](#toastbindinggeneric)
    * [IToastBindingGenericChild](#itoastbindinggenericchild)
    * [ToastGenericAppLogo](#toastgenericapplogo)
    * [ToastGenericHeroImage](#toastgenericheroimage)
    * [ToastGenericAttributionText](#toastgenericattributiontext)
* [IToastActions](#itoastactions)
* [ToastAudio](#toastaudio)
* [ToastHeader](#toastheader)


## <a name="toastcontent"></a>ToastContent
ToastContent — это объект верхнего уровня, который описывает содержимое уведомления, включая визуальные элементы, действия и аудио.

| Свойство | Тип | Обязательный | Описание |
|---|---|---|---|
| **Запуск**| строка | false | Строка, которая передается в приложение, когда оно активируется всплывающим уведомлением. Формат и содержимое этой строки определяются приложением для собственного использования. Когда пользователь касается всплывающего уведомления или щелкает его, чтобы запустить связанное с ним приложение, строка запуска предоставляет приложению контекст, позволяющий отобразить для пользователя представление, связанное с содержимым всплывающего уведомления, а не то представление, которое появляется при стандартном запуске приложения. |
| **Объект класса Visual** | [ToastVisual](#toastvisual) | true | Описывает визуальную часть всплывающего уведомления. |
| **Действия** | [IToastActions](#itoastactions) | false | При необходимости создавайте пользовательские действия с помощью кнопок и вводов. |
| **Звук** | [ToastAudio](#toastaudio) | false | Описывает звуковую часть всплывающего уведомления. |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | false | Указывает, какой тип активации будет использоваться, когда пользователь щелкнет текст этого всплывающего уведомления. |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | false | Новые возможности обновления Creators Update: дополнительные параметры, относящиеся к активации всплывающего уведомления. |
| **Сценарий** | [ToastScenario](#toastscenario) | false | Объявляет сценарий, для которого используется всплывающее уведомление, например, для будильника или напоминания. |
| **DisplayTimestamp** | DateTimeOffset? | false | Новые возможности обновления Creators Update: переопределяйте метку времени по умолчанию с помощью пользовательской метки времени, представляющей время фактической доставки содержимого уведомления вместо времени получения уведомления платформой Windows. |
| **Заголовок** | [ToastHeader](#toastheader) | false | Новые возможности обновления Creators Update: добавляйте настраиваемый заголовок к уведомлению, чтобы группировать несколько уведомлений друг с другом в центре поддержки. |


### <a name="toastscenario"></a>ToastScenario
Указывает, какой сценарий представляет всплывающее уведомление.

| Value | Значение |
|---|---|
| **Значение по умолчанию** | Обычное поведение всплывающего уведомления. |
| **Напоминание** | Уведомление с напоминанием. Оно будет отображаться предварительно расширенным и будет оставаться на экране пользователя до закрытия. |
| **Будильник** | Уведомление будильника. Оно будет отображаться предварительно расширенным и будет оставаться на экране пользователя до закрытия. Звук будет воспроизводиться по умолчанию и будет использоваться как звук будильника. |
| **IncomingCall** | Уведомление о входящем звонке. Оно будет отображаться предварительно развернутым в формате специального вызова и будет оставаться на экране пользователя до закрытия. Звук будет воспроизводиться по умолчанию и будет использоваться как мелодия звонка. |


## <a name="toastvisual"></a>ToastVisual
Визуальная часть всплывающих уведомлений содержит привязки, содержащие текст, изображения, адаптивное содержимое и многое другое.

| Свойство | Тип | Обязательный | Описание |
|---|---|---|---|
| **BindingGeneric** | [ToastBindingGeneric](#toastbindinggeneric) | true | Универсальная привязка всплывающего уведомления, которая может быть обработана на всех устройствах. Эта привязка является обязательной и не может иметь значение null. |
| **BaseUri** | Универсальный код ресурса (URI) | false | Базовый URL-адрес по умолчанию, который используется в сочетании с относительными URL-адресами в атрибутах исходного изображения. |
| **AddImageQuery** | bool? | false | Установите значение "true", чтобы позволить Windows добавлять строку запроса к URL-адресу изображения, предоставляемому во всплывающем уведомлении. Этот атрибут используется, если на сервере размещены изображения и он может обрабатывать строки запроса путем получения варианта изображения на основании строки запроса или игнорирования строки запроса и возвращения изображения, указанного без строки запроса. Эта строка запроса указывает масштаб, контрастность и язык. Например значение "www.website.com/images/hello.png" в уведомлении становится "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us" |
| **Язык**| строка | false | Целевой языковой стандарт визуальных полезных данных при использовании локализованных ресурсов, указанных как теги языка BCP-47, например, "en-US" или "fr-FR". Этот язык переопределяется любого языка, указанного в привязки или текста. Если не задано, будет использоваться языковой стандарт системы. |


## <a name="toastbindinggeneric"></a>ToastBindingGeneric
Универсальная привязка — это привязка по умолчанию для всплывающих уведомлений и является тем местом, где указываются текст, изображения, адаптивное содержимое и многое другое.

| Свойство | Тип | Обязательный | Описание |
|---|---|---|---|
| **Дети** | IList<[IToastBindingGenericChild](#itoastbindinggenericchild)> | false | Содержимое текста всплывающего уведомления, которое может содержать текст, изображения и группы (добавлен в юбилейное обновление). Текстовые элементы должны передовать всем остальным элементам. При этом поддерживаются только 3 текстовых элемента. Если текстовый элемент размещается после любого другого элемента, он будет либо тянуться вверх или опускаться вниз. И, наконец, определенные свойства текста, например HintStyle, не поддерживаются в корневых дочерних элементах текста и работают только внутри AdaptiveSubgroup. Если вы используете AdaptiveGroup на устройствах без юбилейного обновления, содержимое группы будет просто игнорироваться. |
| **AppLogoOverride** | [ToastGenericAppLogo](#toastgenericapplogo) | false | Необязательный логотип для переопределения логотипа приложения. |
| **HeroImage** | [ToastGenericHeroImage](#toastgenericheroimage) | false | Необязательное подобранное "главное" изображение, которое отображается на всплывающем уведомлении и в центре поддержки. |
| **Авторство** | [ToastGenericAttributionText](#toastgenericattributiontext) | false | Дополнительный текст атрибута, который будет отображаться в нижней части всплывающего уведомления. |
| **BaseUri** | Универсальный код ресурса (URI) | false | Базовый URL-адрес по умолчанию, который используется в сочетании с относительными URL-адресами в атрибутах исходного изображения. |
| **AddImageQuery** | bool? | false | Установите значение "true", чтобы позволить Windows добавлять строку запроса к URL-адресу изображения, предоставляемому во всплывающем уведомлении. Этот атрибут используется, если на сервере размещены изображения и он может обрабатывать строки запроса путем получения варианта изображения на основании строки запроса или игнорирования строки запроса и возвращения изображения, указанного без строки запроса. Эта строка запроса указывает масштаб, контрастность и язык. Например значение "www.website.com/images/hello.png" в уведомлении становится "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us" |
| **Язык**| строка | false | Целевой языковой стандарт визуальных полезных данных при использовании локализованных ресурсов, указанных как теги языка BCP-47, например, "en-US" или "fr-FR". Этот язык переопределяется любого языка, указанного в привязки или текста. Если не задано, будет использоваться языковой стандарт системы. |


## <a name="itoastbindinggenericchild"></a>IToastBindingGenericChild
Интерфейс маркера для дочерних элементов всплывающего уведомления, которые включают текст, изображения, группы и пр.

| Реализации |
| --- |
| [AdaptiveText](#adaptivetext) |
| [AdaptiveImage](#adaptiveimage) |
| [AdaptiveGroup](#adaptivegroup) |
| [AdaptiveProgressBar](#adaptiveprogressbar) |


## <a name="adaptivetext"></a>AdaptiveText
Адаптивный текстовый элемент. Если размещен в элементе ToastBindingGeneric.Children верхнего уровня, будет применен только параметр HintMaxLines. Но если этот параметр помещается как дочерний элемент группы/подгруппы, поддерживается полный стиль текста.

| Свойство | Тип | Обязательный |Описание |
|---|---|---|---|
| **Текст** | string или [BindableString](#bindablestring) | false | Отображаемый текст. Поддержка привязки данных добавлена в обновление Creators Update, но работает только для элементов текста верхнего уровня. |
| **HintStyle** | [AdaptiveTextStyle](#adaptivetextstyle) | false | Стиль отвечает за размер, насыщенность и непрозрачность шрифта. Работает только для текстовых элементов внутри группы или подгруппы. |
| **HintWrap** | bool? | false | Установите для этого параметра значение true, чтобы активировать обтекание текстом. Текстовые элементы верхнего уровня игнорируют это свойство и всегда обтекают (вы можете использовать HintMaxLines = 1, чтобы отключить обтекание для текстовых элементов верхнего уровня). Текстовые элементы внутри групп и подгрупп возвращаются к значению "false" по умолчанию для переноса. |
| **HintMaxLines** | целое число? | false | Максимальное число строк, которое разрешено текстовому элементу для отображения. |
| **HintMinLines** | целое число? | false | Минимальное число строк, которое текстовый элемент должен отображать. Работает только для текстовых элементов внутри группы или подгруппы. |
| **HintAlign** | [AdaptiveTextAlign](#adaptivetextalign) | false | Выравнивание текста по горизонтали. Работает только для текстовых элементов внутри группы или подгруппы. |
| **Язык** | строка | false | Целевой языковой стандарт полезных данных XML, указанный как теги языка BCP-47, например "en-US" или "fr-FR". Языковой стандарт, указанный здесь, переопределяет все остальные языковые стандарты, например, привязку или визуальные элементы. Если это значение равно строковому литералу, этот атрибут по умолчанию возвращается к языку пользовательского интерфейса. Если это значение равно ссылке строки, этот атрибут по умолчанию возвращается к языковому стандарту, выбранному средой выполнения Windows при разрешении строки. |


### <a name="bindablestring"></a>BindableString
Значение привязки для строк.

| Свойство | Тип | Обязательный | Описание |
|---|---|---|---|
| **BindingName** | string | true | Возвращает или задает имя, которое сопоставляет значение привязки данных. |


### <a name="adaptivetextstyle"></a>AdaptiveTextStyle
Стиль текста контролирует размер шрифта, его насыщенность и непрозрачность. Утонченная непрозрачность — это 60% непрозрачности.

| Value | Значение |
|---|---|
| **Значение по умолчанию** | Стандартное значение. Стиль определяется обработчиком. |
| **Caption** | Меньше размера шрифта абзаца. |
| **CaptionSubtle** | То же, что и Caption, но с утонченной прозрачностью. |
| **Body** | Размер шрифта абзаца. |
| **BodySubtle** | То же, что и Body, но с утонченной прозрачностью. |
| **Base** | Размер шрифта абзаца, полужирная насыщенность. По существу полужирная версия Body. |
| **BaseSubtle** | То же, что и Base, но с утонченной прозрачностью. |
| **Subtitle** | Размер шрифта H4. |
| **SubtitleSubtle** | То же, что и Subtitle, но с утонченной прозрачностью. |
| **Title** | Размер шрифта H3. |
| **TitleSubtle** | То же, что и Title, но с утонченной прозрачностью. |
| **TitleNumeral** | То же, что и Title, но с удаленным заполнением вверху или внизу. |
| **Subheader** | Размер шрифта H2. |
| **SubheaderSubtle** | То же, что и Subheader, но с утонченной прозрачностью. |
| **SubheaderNumeral** | То же, что и Subheader, но с удаленным заполнением вверху или внизу. |
| **Заголовок** | Размер шрифта H1. |
| **HeaderSubtle** | То же, что и Header, но с утонченной прозрачностью. |
| **HeaderNumeral** | То же, что и Header, но с удаленным заполнением вверху или внизу. |


### <a name="adaptivetextalign"></a>AdaptiveTextAlign
Управляет выравниванием текста по горизонтали.

| Value | Значение |
|---|---|
| **Значение по умолчанию** | Стандартное значение. Выравнивание автоматически определяется обработчиком. |
| **Auto** | Выравнивание определяется текущим языком и региональными параметрами. |
| **Left** | Выравнивание текста по горизонтали слева. |
| **Center** | Выравнивание текста по горизонтали по центру. |
| **Right** | Выравнивание текста по горизонтали вправо. |


## <a name="adaptiveimage"></a>AdaptiveImage
Встроенное изображение.

| Свойство | Тип | Обязательный |Описание |
|---|---|---|---|
| **Источник** | строка | true | URL-адрес изображения. Поддерживаются ms-appx, ms-appdata и http. Начиная с обновления Fall Creators Update размер веб-изображений может быть до 3 МБ для обычных подключений и до 1 МБ для лимитных подключений. На устройствах без Fall Creators Update размер веб-изображений не должен превышать 200 КБ. |
| **HintCrop** | [AdaptiveImageCrop](#adaptiveimagecrop) | false | Новые возможности в Юбилейном обновлении: управляйте нужной обрезкой изображения. |
| **HintRemoveMargin** | bool? | false | По умолчанию изображения внутри групп или подгрупп имеют поле 8 пикселей вокруг них. Это поле можно удалить, если установить для этого свойства значение true. |
| **HintAlign** | [AdaptiveImageAlign](#adaptiveimagealign) | false | Выравнивание изображения по горизонтали. Работает только для изображений внутри группы или подгруппы. |
| **AlternateText** | строка | false | Чередуйте текст, описывающий изображение, используемый для поддержки специальных возможностей. |
| **AddImageQuery** | bool? | false | Установите значение "true", чтобы позволить Windows добавлять строку запроса к URL-адресу изображения, предоставляемому во всплывающем уведомлении. Этот атрибут используется, если на сервере размещены изображения и он может обрабатывать строки запроса путем получения варианта изображения на основании строки запроса или игнорирования строки запроса и возвращения изображения, указанного без строки запроса. Эта строка запроса указывает масштаб, контрастность и язык. Например значение "www.website.com/images/hello.png" в уведомлении становится "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us" |


### <a name="adaptiveimagecrop"></a>AdaptiveImageCrop
Указывает нужную обрезку изображения.

| Value | Значение |
|---|---|
| **Значение по умолчанию** | Стандартное значение. Обрезка определяется обработчиком. |
| **None** | Изображение не обрезается. |
| **Circle** | Изображение обрезается до формы круга. |


### <a name="adaptiveimagealign"></a>AdaptiveImageAlign
Задает выравнивание по горизонтали для изображения.

| Value | Значение |
|---|---|
| **Значение по умолчанию** | Стандартное значение. Выравнивание определяется обработчиком. |
| **Stretch** | Изображение растягивается для заполнения доступной ширины (и потенциально доступной высоты, в зависимости от того, где разместить изображения). |
| **Left** | Выравнивание изображения по левому краю с отображением изображения в собственном разрешении. |
| **Center** | Выравнивание изображения в центре по горизонтали с отображением изображения в собственном разрешении. |
| **Right** | Выравнивание изображения по правому краю с отображением изображения в собственном разрешении. |


## <a name="adaptivegroup"></a>AdaptiveGroup
Новые возможности в Юбилейном обновлении: группы семантически определяют, что содержимое в группе должно отображаться полностью или не отображаться совсем, если оно не помещается. Группы также позволяют создавать несколько столбцов.

| Свойство | Тип | Обязательный |Описание |
|---|---|---|---|
| **Дочерние элементы** | IList<[AdaptiveSubgroup](#adaptivesubgroup)> | false | Подгруппы отображаются в виде вертикальных столбцов. Подгруппы необходимо использовать для предоставления любого содержимого внутри AdaptiveGroup. |


## <a name="adaptivesubgroup"></a>AdaptiveSubgroup
Новые возможности в Юбилейном обновлении: подгруппы являются вертикальными столбцами, которые могут содержать текст и изображения.

| Свойство | Тип | Обязательный |Описание |
|---|---|---|---|
| **Дочерние элементы** | IList<[IAdaptiveSubgroupChild](#iadaptivesubgroupchild)> | false | [AdaptiveText](#adaptivetext) и [AdaptiveImage](#adaptiveimage) — это допустимые дочерние элементы подгрупп. |
| **HintWeight** | целое число? | false | Контролируйте ширину этого столбца подгруппы, указав насыщенность относительно других подгрупп. |
| **HintTextStacking** | [AdaptiveSubgroupTextStacking](#adaptivesubgrouptextstacking) | false | Контролируйте вертикальное выравнивание содержимого этой подгруппы. |


### <a name="iadaptivesubgroupchild"></a>IAdaptiveSubgroupChild
Интерфейс маркера для дочерних элементов подгруппы.

| Реализации |
| --- |
| [AdaptiveText](#adaptivetext) |
| [AdaptiveImage](#adaptiveimage) |


### <a name="adaptivesubgrouptextstacking"></a>AdaptiveSubgroupTextStacking
TextStacking задает вертикальное выравнивание содержимого.

| Value | Значение |
|---|---|
| **Значение по умолчанию** | Стандартное значение. Обработчик автоматически выбирает вертикальное выравнивание по умолчанию. |
| **Top** | Вертикальное выравнивание по верхнему краю. |
| **Center** | Вертикальное выравнивание по центру. |
| **Bottom** | Вертикальное выравнивание по нижнему краю. |


## <a name="adaptiveprogressbar"></a>AdaptiveProgressBar
Новые возможности в обновлении Creators Update: индикатор хода выполнения. Поддерживается только для всплывающих уведомлений на настольных компьютерах, сборки 15063 или более поздней версии.

| Свойство | Тип | Обязательный | Описание |
|---|---|---|---|
| **Title** | string или [BindableString](#bindablestring) | false | Получает или задает необязательную строку заголовка. Поддерживает привязку данных. |
| **Value** | double, [AdaptiveProgressBarValue](#adaptiveprogressbarvalue) или [BindableProgressBarValue](#bindableprogressbarvalue) | false | Получает или задает значение индикатора хода выполнения. Поддерживает привязку данных. По умолчанию — 0. |
| **ValueStringOverride** | string или [BindableString](#bindablestring) | false | Получает или задает необязательную строку для отображения вместо строки процентов по умолчанию. Если строка не указана, отображается текст, например "70%". |
| **Status** | string или [BindableString](#bindablestring) | true | Получает или задает строку состояния (обязательную), которая отображается под индикатором хода выполнения слева. Эта строка должна отражать состояние операции, например, "Загрузка..." или "Установка...". |


### <a name="adaptiveprogressbarvalue"></a>AdaptiveProgressBarValue
Класс, представляющий значение индикатора выполнения.

| Свойство | Тип | Обязательный | Описание |
|---|---|---|---|
| **Значение** | double | false | Возвращает или задает значение (0,0 - 1.0), обозначающее степень завершенности операции в процентах. |
| **IsIndeterminate** | bool | false | Возвращает или задает значение — является ли индикатор хода выполнения невычислимым. Если задано значение true, то свойство **Value** игнорируется. |


### <a name="bindableprogressbarvalue"></a>BindableProgressBarValue
Привязываемое значение индикатора выполнения.

| Свойство | Тип | Обязательный | Описание |
|---|---|---|---|
| **BindingName** | string | true | Возвращает или задает имя, которое сопоставляет значение привязки данных. |


## <a name="toastgenericapplogo"></a>ToastGenericAppLogo
Логотип должен отобразиться вместо логотипа приложения.

| Свойство | Тип | Обязательный |Описание |
|---|---|---|---|
| **Источник** | строка | true | URL-адрес изображения. Поддерживаются ms-appx, ms-appdata и http. Изображения HTTP должны иметь размер 200 КБ или меньше. |
| **HintCrop** | [ToastGenericAppLogoCrop](#toastgenericapplogocrop) | false | Укажите, как необходимо обрезать изображение. |
| **AlternateText** | строка | false | Чередуйте текст, описывающий изображение, используемый для поддержки специальных возможностей. |
| **AddImageQuery** | bool? | false | Установите значение "true", чтобы позволить Windows добавлять строку запроса к URL-адресу изображения, предоставляемому во всплывающем уведомлении. Этот атрибут используется, если на сервере размещены изображения и он может обрабатывать строки запроса путем получения варианта изображения на основании строки запроса или игнорирования строки запроса и возвращения изображения, указанного без строки запроса. Эта строка запроса указывает масштаб, контрастность и язык. Например значение "www.website.com/images/hello.png" в уведомлении становится "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us" |


### <a name="toastgenericapplogocrop"></a>ToastGenericAppLogoCrop
Управление обрезкой изображения логотипа приложения.

| Value | Значение |
|---|---|
| **Значение по умолчанию** | Обрезка использует поведение обработчика по умолчанию. |
| **None** | Изображение не обрезается, отображается квадратом. |
| **Circle** | Изображение обрезается до формы круга. |


## <a name="toastgenericheroimage"></a>ToastGenericHeroImage
Подобранное "главное" изображение, которое отображается на всплывающем уведомлении и в центре поддержки.

| Свойство | Тип | Обязательный |Описание |
|---|---|---|---|
| **Источник** | строка | true | URL-адрес изображения. Поддерживаются ms-appx, ms-appdata и http. Изображения HTTP должны иметь размер 200 КБ или меньше. |
| **AlternateText** | строка | false | Чередуйте текст, описывающий изображение, используемый для поддержки специальных возможностей. |
| **AddImageQuery** | bool? | false | Установите значение "true", чтобы позволить Windows добавлять строку запроса к URL-адресу изображения, предоставляемому во всплывающем уведомлении. Этот атрибут используется, если на сервере размещены изображения и он может обрабатывать строки запроса путем получения варианта изображения на основании строки запроса или игнорирования строки запроса и возвращения изображения, указанного без строки запроса. Эта строка запроса указывает масштаб, контрастность и язык. Например значение "www.website.com/images/hello.png" в уведомлении становится "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us" |


## <a name="toastgenericattributiontext"></a>ToastGenericAttributionText
Текст авторства, отображаемый в нижней части всплывающего уведомления.

| Свойство | Тип | Обязательный | Описание |
|---|---|---|---|
| **Text** | строка | true | Отображаемый текст. |
| **Язык** | строка | false | Целевой языковой стандарт визуальных полезных данных при использовании локализованных ресурсов, указанных как теги языка BCP-47, например, "en-US" или "fr-FR". Если не задано, будет использоваться языковой стандарт системы. |


## <a name="itoastactions"></a>IToastActions
Интерфейс маркера для действий и вводов всплывающего уведомления.

| Реализации |
| --- |
| [ToastActionsCustom](#toastactionscustom) |
| [ToastActionsSnoozeAndDismiss](#toastactionssnoozeanddismiss) |


## <a name="toastactionscustom"></a>ToastActionsCustom
*Реализует [IToastActions](#itoastactions)*

Создайте собственные пользовательские действия и входные данные с помощью таких элементов управления, как кнопки, текстовые поля и входные данные выбора.

| Свойство | Тип | Обязательный | Описание |
|---|---|---|---|
| **Входные данные** | IList<[IToastInput](#itoastinput)> | false | Входные данные, такие как текстовые поля и поля ввода выделения. Разрешается до пяти входных данных. |
| **Кнопки** | IList<[IToastButton](#itoastbutton)> | false | Кнопки отображаются после всех входных данных (или рядом с входными данными, если кнопка используется как кнопка быстрого ответа). Разрешается до пяти кнопок (или меньше, если у вас также есть элементы контекстного меню). |
| **ContextMenuItems** | IList<[ToastContextMenuItem](#toastcontextmenuitem)> | false | Новые возможности в Юбилейном обновлении: пользовательские элементы контекстного меню, предоставляющие дополнительные действия, если пользователь щелкнет уведомление правой кнопкой мыши. Можно иметь только до пяти кнопок и элементов контекстного меню *в совокупности*. |


## <a name="itoastinput"></a>IToastInput
Интерфейс маркера для входных данных всплывающего уведомления.

| Реализации |
| --- |
| [ToastTextBox](#toasttextbox) |
| [ToastSelectionBox](#toastselectionbox) |


## <a name="toasttextbox"></a>ToastTextBox
*Реализует [IToastInput](#itoastinput)*

Элемент управления текстового поля, в котором пользователь может ввести текст.

| Свойство | Тип | Обязательный | Описание |
|---|---|---|---|
| **Id** | строка | true | Id является обязательным и используется для сопоставления введенного пользователем текста с парой ключ-значение идентификатора и значения, которое ваше приложение использует позже. |
| **Title** | строка | false | Текст заголовка для отображения над текстовым полем. |
| **PlaceholderContent** | строка | false | Замещающий текст, который будет отображаться в текстовом поле, когда пользователь еще не ввел какой-либо текст. |
| **DefaultInput** | строка | false | Исходный текст для размещения в текстовом поле. Оставьте это значение null для пустого текстового поля. |


## <a name="toastselectionbox"></a>ToastSelectionBox
*Реализует [IToastInput](#itoastinput)*

Элемент управления полем выбора, позволяющий пользователям выбирать из вариантов раскрывающегося списка.

| Свойство | Тип | Обязательный | Описание |
|---|---|---|---|
| **Id** | строка | true | Параметр Id является обязательным. Если пользователь выбрал этот элемент, этот идентификатор будет передаваться обратно в код вашего приложения, представляя выбор пользователя. |
| **Content** | строка | true | Параметр Content является обязательным и является строкой, которая отображается на элементе выбора. |


### <a name="toastselectionboxitem"></a>ToastSelectionBoxItem
Элемент поля выбора (элемент, который пользователь может выбрать из раскрывающегося списка).

| Свойство | Тип | Обязательный | Описание |
|---|---|---|---|
| **Id** | строка | true | Id является обязательным и используется для сопоставления введенного пользователем текста с парой ключ-значение идентификатора и значения, которое ваше приложение использует позже. |
| **Title** | строка | false | Текст заголовка для отображения над полем выбора. |
| **DefaultSelectionBoxItemId** | строка | false | Этот параметр определяет, какой элемент выбирается по умолчанию и ссылается на свойство Id [ToastSelectionBoxItem](#toastselectionboxitem). Если вы не укажете этот параметр, поле будет пустым по умолчанию (пользователь не видит ничего). |
| **Элементы** | IList<[ToastSelectionBoxItem](#toastselectionboxitem)> | false | Элементы выбора, которые пользователь может выбрать в этом поле SelectionBox. Можно добавлять только 5 элементов. |


## <a name="itoastbutton"></a>IToastButton
Интерфейс маркера для кнопок всплывающего уведомления.

| Реализации |
| --- |
| [ToastButton](#toastbutton) |
| [ToastButtonSnooze](#toastbuttonsnooze) |
| [ToastButtonDismiss](#toastbuttondismiss) |


## <a name="toastbutton"></a>ToastButton
*Реализует [IToastButton](#itoastbutton)*

Кнопка, которую пользователь может.

| Свойство | Тип | Обязательный | Описание |
|---|---|---|---|
| **Содержимое** | строка | true | Обязательный. Текст для отображения на кнопке. |
| **Аргументы** | строка | true | Обязательный. Определяемая приложением строка аргументов, которую приложение будет получать позже, если пользователь нажмет эту кнопку. |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | false | Определяет, какой тип активации эта кнопка будет использовать при нажатии. По умолчанию это — "Передний план". |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | false | Новая возможность в обновлении Creators Update: возвращает и получает дополнительные параметры активации кнопки всплывающего уведомления. |


### <a name="toastactivationtype"></a>ToastActivationType
Определяет тип активации, который будет использоваться, когда пользователь взаимодействует с определенным действием.

| Value | Значение |
|---|---|
| **Передний план** | Стандартное значение. Ваше приложение переднего плана запущено. |
| **Фон** | Вызывается соответствующая фоновая задача (при условии, что вы все настроили), и вы можете выполнить код в фоновом режиме (например, отправить сообщение с быстрым ответом пользователя), не прерывая работу пользователя. |
| **Протокол** | Запустите другое приложение с помощью активации протокола. |


### <a name="toastactivationoptions"></a>ToastActivationOptions
Новые возможности обновления Creators Update: дополнительные параметры связанные с активацией.

| Свойство | Тип | Обязательный | Описание |
|---|---|---|---|
| **AfterActivationBehavior** | [ToastAfterActivationBehavior](#toastafteractivationbehavior) | false | Новые возможности в обновлении Fall Creators Update: возвращает или задает поведение всплывающего уведомления, которое будет исполнено, когда пользователь вызовет это действие. Работает только на настольных компьютерах, применяется для [ToastButton](#toastbutton) и [ToastContextMenuItem](#toastcontextmenuitem). |
| **ProtocolActivationTargetApplicationPfn** | строка | false | Если вы используете *ToastActivationType.Protocol*, можно дополнительно указать целевой объект PFN, чтобы независимо от регистрации нескольких приложений для обработки одного и того же URI-протокола всегда запускались нужные приложения. |


### <a name="toastafteractivationbehavior"></a>ToastAfterActivationBehavior
Задает поведение всплывающего уведомления, которое должно быть исполнено, когда пользователь выполняет действие со всплывающим уведомлением.

| Значение | Значение |
|---|---|
| **Default** | Поведение по умолчанию. Всплывающее уведомление будет закрыто после действия пользователя. |
| **PendingUpdate** | После нажатия пользователем кнопки на всплывающем уведомлении, последнее останется на экране в визуальном состоянии "Ожидание обновления". Вы должны немедленно обновить всплывающее уведомление из фоновой задачи, чтобы пользователь не наблюдал это визуальное состояние «ожидание обновления» слишком долго. |


## <a name="toastbuttonsnooze"></a>ToastButtonSnooze
*Реализует [IToastButton](#itoastbutton)*

Обрабатываемая системой кнопка откладывания, которая автоматически обрабатывает откладывание уведомления.

| Свойство | Тип | Обязательный | Описание |
|---|---|---|---|
| **CustomContent** | строка | false | Необязательный пользовательский текст, отображаемый на кнопке, которая переопределяет значение по умолчанию локализованного текста "Snooze". |


## <a name="toastbuttondismiss"></a>ToastButtonDismiss
*Реализует [IToastButton](#itoastbutton)*

Обрабатываемая системой кнопка закрытия, которая закрывает уведомление при нажатии.

| Свойство | Тип | Обязательный | Описание |
|---|---|---|---|
| **CustomContent** | строка | false | Необязательный пользовательский текст, отображаемый на кнопке, которая переопределяет значение по умолчанию локализованного текста "Dismiss". |


## <a name="toastactionssnoozeanddismiss"></a>ToastActionsSnoozeAndDismiss
*Реализует [IToastActions](#itoastactions)

Автоматически выполняет построение поля выделения для интервалов откладывания и кнопок откладывания или закрытия. Все автоматически локализованы и логика откладывания автоматически обрабатываются системой.

| Свойство | Тип | Обязательный | Описание |
|---|---|---|---|
| **ContextMenuItems** | IList<[ToastContextMenuItem](#toastcontextmenuitem)> | false | Новые возможности в Юбилейном обновлении: пользовательские элементы контекстного меню, предоставляющие дополнительные действия, если пользователь щелкнет уведомление правой кнопкой мыши. Может быть только до пяти элементов. |


## <a name="toastcontextmenuitem"></a>ToastContextMenuItem
Запись элемента контекстного меню.

| Свойство | Тип | Обязательный | Описание |
|---|---|---|---|
| **Содержимое** | строка | true | Обязательный. Отображаемый текст. |
| **Аргументы** | строка | true | Обязательный. Определенная приложением строка аргументов, которую приложение может позже восстановить после активации, когда пользователь щелкнет элемент меню. |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | false | Определяет, какой тип активации этот элемент меню будет использовать при нажатии. По умолчанию это — "Передний план". |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | false | Новые возможности обновления Creators Update: дополнительные параметры, связанные с активацией элемента меню контекста всплывающего уведомления. |


## <a name="toastaudio"></a>ToastAudio
Укажите звук, который будет воспроизводиться после получения всплывающего уведомления.

| Свойство | Тип | Обязательный | Описание |
|---|---|---|---|
| **Src** | uri | false | Файл мультимедиа для воспроизведения вместо звука по умолчанию. Поддерживаются только ms-appx и ms-appdata. |
| **Loop** | boolean | false | Задайте значение true, если звук должен повторяться, пока отображается всплывающее уведомление; для разового воспроизведения используйте значение false (вариант по умолчанию). |
| **Silent** | boolean | false | Значение true для отключения звука; значение false, чтобы разрешить воспроизведение звука всплывающего уведомления (по умолчанию). |


## <a name="toastheader"></a>ToastHeader
Новая возможность в обновлении Creators Update: пользовательский заголовок для группировки уведомлений в центре уведомлений.

| Свойство | Тип | Обязательный | Описание |
|---|---|---|---|
| **Id** | строка | true | Идентификатор, созданный разработчиком, который уникально определяет этот заголовок. Если два уведомления имеют один и тот же идентификатор заголовка, то они будет отображаться под одним и тем же заголовком в центре уведомлений. |
| **Title** | строка | true | Название для заголовка. |
| **Arguments**| строка | true | Возвращает или задает определяемую разработчиком строку аргументов, которая возвращается приложению, когда пользователь щелкает этот заголовок. Не может быть равно null. |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | false | Возвращает или задает тип активации этого заголовка при его нажатии. По умолчанию — Foreground (передний план). Обратите внимание, что поддерживаются только варианты Foreground и Protocol. |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | false | Возвращает и получает дополнительные параметры активации заголовка всплывающего уведомления. |


## <a name="related-topics"></a>Статьи по теме

* [Краткое руководство: отправка локального всплывающего уведомления и обработка активации](http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/07/08/quickstart-sending-a-local-toast-notification-and-handling-activations-from-it-windows-10.aspx)
* [Библиотека уведомлений на GitHub](https://github.com/Microsoft/UWPCommunityToolkit/tree/dev/Notifications)