---
author: stevewhims
Description: "Если вы хотите, чтобы ваше приложение поддерживало разные языки интерфейса, а в вашем коде или разметке XAML либо манифесте пакета приложения есть строковые литералы, переместите эти строки из кода или разметки в файл ресурсов (.resw). Затем можно создать переведенную копию этого файла ресурсов для каждого языка, поддерживаемого вашим приложением."
title: "Локализация строк в манифесте пакета приложения и интерфейсе пользователя"
ms.assetid: E420B9BB-C0F6-4EC0-BA3A-BA2875B69722
label: Localize strings in your UI and app package manifest
template: detail.hbs
ms.author: stwhi
ms.date: 11/01/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, ресурс, изображение, средство, MRT, квалификатор"
localizationpriority: medium
ms.openlocfilehash: cca9c9b731831ce1c87ee74b7f8b502130ef97fd
ms.sourcegitcommit: d0c93d734639bd31f264424ae5b6fead903a951d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/03/2017
---
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

# <a name="localize-strings-in-your-ui-and-app-package-manifest"></a>Локализация строк в манифесте пакета приложения и интерфейсе пользователя

Дополнительные сведения о преимуществах локализации приложений см. в разделе [Глобализация и локализация](../globalizing/globalizing-portal.md).

Если вы хотите, чтобы ваше приложение поддерживало разные языки интерфейса, а в вашем коде или разметке XAML либо манифесте пакета приложения есть строковые литералы, переместите эти строки из кода или разметки в файл ресурсов (.resw). Затем можно создать переведенную копию этого файла ресурсов для каждого языка, поддерживаемого вашим приложением.

Жестко закодированные строковые литералы могут появляться в императивном коде или в разметке XAML, например в виде свойства **Text** объекта **TextBlock**. Они также могут появляться в манифесте пакета приложения (файле `Package.appxmanifest`), например в качестве значения для отображаемого имени на вкладке "Приложение" в конструкторе манифеста Visual Studio. Переместите эти строки в файл ресурсов (.resw) и замените жестко закодированные строковые литералы в своем приложении и в манифесте ссылками на идентификаторы ресурсов.

В отличие от ресурсов изображений, где в файле ресурсов содержится только один ресурс изображения, в файле строковых ресурсов содержится *много* строковых ресурсов. Файл строковых ресурсов представляет собой файл ресурсов (.resw), и обычно такой файл ресурсов создается в папке \Strings проекта. Общие сведения об использовании квалификаторов в именах файлов ресурсов (.resw) приводятся в разделе [Адаптация ресурсов для языка, масштаба и других квалификаторов](tailor-resources-lang-scale-contrast.md).

## <a name="create-a-resources-file-resw-and-put-your-strings-in-it"></a>Создайте файл ресурсов (.resw) и поместите в него свои строковые ресурсы

1. Установите язык по умолчанию для приложения.
    1. Открыв решение в Visual Studio, откройте файл `Package.appxmanifest`.
    2. На вкладке «Приложения» убедитесь, что язык по умолчанию задан соответствующим образом (например, en или en-US). На дальнейших этапах предполагается, что вы задали язык по умолчанию en-US.
    <br>**Примечание**. Как минимум, необходимо предоставить строковые ресурсы, локализованные для данного языка по умолчанию. Это и есть ресурсы, которые будут загружены в том случае, если не будет найдено более подходящих для выбранного пользователем языка или языка отображения ресурсов.
2. Создайте файл ресурсов (.resw) для языка по умолчанию.
    1. В узле проекта создайте новую папку и назовите ее Strings.
    2. В разделе `Strings` создайте новую вложенную папку и назовите ее en-US.
    3. В разделе `en-US` создайте новый файл ресурсов (.resw) и убедитесь, что он называется Resources.resw.
    <br>**Примечание**. При наличии файлов ресурсов .NET (.resx), которые требуется перенести, см. раздел [Перенос XAML и пользовательского интерфейса](../porting/wpsl-to-uwp-porting-xaml-and-ui.md#localization-and-globalization).
3.  Откройте `Resources.resw` и добавьте эти строковые ресурсы.

    `Strings/en-US/Resources.resw`

    ![Добавление ресурса (английский язык)](images/addresource-en-us.png)

    В этом примере Greeting является идентификатором строкового ресурса, на который можно ссылаться из разметки, как мы увидим далее. Для идентификатора Greeting предоставляются строка для свойства Text и строка для свойства Width. Greeting.Text — пример идентификатора свойства, так как он соответствует свойству элемента пользовательского интерфейса. Можно также добавить, например Greeting.Foreground в столбце Name и присвоить ему значение Red. Идентификатор Farewell представляет собой идентификатор простого строкового ресурса; у него нет собственных свойств, и его можно загружать из императивного кода, как мы увидим далее. Столбец Comment будет удобен для предоставления особых инструкций переводчикам.

    В этом примере, так как у нас есть идентификатор простого строкового ресурса с именем Farewell, мы не может *также* использовать идентификаторы свойств на основе этого же идентификатора. Таким образом, добавление Farewell.Text вызовет ошибку дубликата элемента при построении `Resources.resw`.

## <a name="refer-to-a-string-resource-identifier-from-xaml-markup"></a>Ссылка на идентификатор строкового ресурса из разметки XAML

Вы используете [x:Uid directive](../xaml-platform/x-uid-directive.md) для привязки элемента управления или другого элемента в разметке к идентификатору строкового ресурса.

**XAML**
```xml
<TextBlock x:Uid="Greeting"/>
```

Во время выполнения загружается `\Strings\en-US\Resources.resw` (поскольку сейчас это единственный файл ресурсов в проекте). Директива **X:Uid** для **TextBlock** вызывает выполнение поиска в `Resources.resw` для обнаружения идентификаторов свойств, содержащих идентификатор строкового ресурса Greeting. Обнаруживаются идентификаторы свойств Greeting.Text и Greeting.Width, и их значения применяются к **TextBlock**, переопределяя значения, заданные локально в разметке. Значение Greeting.Foreground также будет применяться, если оно добавлено. Однако только идентификаторы свойств используются для задания свойств элементов разметки XAML, поэтому установка **x:Uid** со значением Farewell для этого TextBlock не даст никакого эффекта. `Resources.resw` *does* содержит идентификатор строкового ресурса Farewell, но не содержит идентификаторы свойств для него.

При назначении идентификатора строкового ресурса элементу XAML следите за тем, чтобы *все* идентификаторы свойств для этого идентификатора подходили этому элементу XAML. Например, если установить `x:Uid="Greeting"` для **TextBlock**, то значение Greeting.Text будет обработано, поскольку у типа **TextBlock** есть свойство Text. Но если вы задали `x:Uid="Greeting"` для **Button**, то Greeting.Text приведет к ошибке во время выполнения, поскольку у типа **Button** нет свойства Text. Возможное решение проблемы в этом случае — создать идентификатор свойства с именем ButtonGreeting.Content и задать `x:Uid="ButtonGreeting"` для **Button**.

Вместо определения значения **Width** из файла ресурсов, возможно, логично будет разрешить динамическую адаптацию размера элементов управления по содержимому.

**Примечание**. Для [присоединенных свойств](../xaml-platform/attached-properties-overview.md) требуется особый синтаксис в столбце Name в файле .resw. Например, вот что надо ввести в столбце Name, чтобы задать значение для присоединенного свойства [AutomationProperties.Name](/uwp/api/windows.ui.xaml.automation.automationproperties?branch=live#Windows_UI_Xaml_Automation_AutomationProperties_NameProperty) для идентификатора Greeting.

```
Greeting.[using:Windows.UI.Xaml.Automation]AutomationProperties.Name
```

## <a name="refer-to-a-string-resource-identifier-from-code"></a>Ссылка на идентификатор строкового ресурса из кода

Можно загрузить строковый ресурс явным образом по идентификатору простого строкового ресурса.

**C#**
```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("Farewell");
```

**C++**
```cpp
auto resourceLoader = Windows::ApplicationModel::Resources::ResourceLoader::GetForCurrentView();
this->myXAMLTextBlockElement->Text = resourceLoader->GetString("Farewell");
```

Тот же самый код можно использовать из проекта с библиотекой классов (Universal Windows) или [библиотекой среды выполнения Windows (универсальная платформа Windows)](../winrt-components/index.md). Во время выполнения загружаются ресурсы приложения, в котором размещается библиотека. Рекомендуется, чтобы библиотека загружала ресурсы из приложения, в котором она расположена, поскольку выше вероятность, что это приложение локализовано в большей степени. Если библиотеке не требуется предоставлять ресурсы, то она должна обеспечить приложению возможность заменять их при вызове.

**Примечание**. Таким путем можно загружать только значение для идентификатора простого строкового ресурса, но не для идентификатора свойства. Таким образом, мы можем загрузить значение для Farewell с помощью подобного кода, но не можем сделать это для Greeting.Text. Попытка сделать это возвращает пустую строку.

## <a name="refer-to-a-string-resource-identifier-from-your-app-package-manifest"></a>Ссылка на идентификатор строкового ресурса из манифеста пакета приложения

1. Откройте манифест пакета приложения (файл `Package.appxmanifest`), в котором по умолчанию отображаемое имя вашего приложения выражено в виде строкового литерала.

   ![Добавление ресурса (английский язык)](images/display-name-before.png)

2. Чтобы создать локализованную версию этой строки, откройте `Resources.resw` и добавьте новый строковый ресурс с именем AppDisplayName и значением Adventure Works Cycles.

3. Замените строковый литерал отображаемого имени ссылкой на идентификатор строкового ресурса, который вы только что создали (AppDisplayName). Для этого необходимо использовать схему `ms-resource` URI (универсальный код ресурса).

   ![Добавление ресурса (английский язык)](images/display-name-after.png)

4. Повторите эту процедуру для каждой строки в манифесте, которую требуется локализовать. Например, короткое название вашего приложения (которое может отображаться на плитке приложения на начальном экране). Список всех элементов в манифесте пакета приложения, которые можно локализовать, см. в разделе [Локализуемые элементы манифеста](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live).

## <a name="localize-the-string-resources"></a>Локализация строковых ресурсов

1. Создайте копию файла ресурсов (.resw) для другого языка.
    1. В разделе Strings создайте новую вложенную папку и назовите ее de-DE для немецкого языка (Германия).
   <br>**Примечание**. В имени папки можно использовать любой [языковой тег BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302). Дополнительные сведения о языковых квалификаторах и список распространенных языковых тегов приводятся в разделе [Адаптация ресурсов с учетом языка, масштаба и других квалификаторов](tailor-resources-lang-scale-contrast.md).
   2. Создайте копию `Strings/en-US/Resources.resw` в папке `Strings/de-DE`.
2. Переведите строки.
    1. Откройте `Strings/de-DE/Resources.resw` и переведите значения в столбце Value. Комментарии переводить не нужно.

    `Strings/de-DE/Resources.resw`

    ![Добавление ресурса (немецкий язык)](images/addresource-de-de.png)

При желании можно повторить шаги 1 и 2 для еще одного языка.

`Strings/fr-FR/Resources.resw`

![Добавление ресурса (французский язык)](images/addresource-fr-fr.png)

## <a name="test-your-app"></a>Тестирование приложения

Протестируйте приложение для языка интерфейса по умолчанию. Можно изменить язык отображения в разделе **Параметры** > **Время и язык** > **Регион и язык** (или **Язык**) и повторно протестировать приложение. Рассмотрим строки в пользовательском интерфейсе и в оболочке (например, заголовок окна&mdash;это отображаемое имя&mdash;и сокращенное имя на плитках).

**Примечание**. Если можно найти папку с именем, соответствующим заданному языку отображения, загружается файл ресурсов из этой папки. В противном случае происходит возврат к исходным значениям, и используются ресурсы для языка по умолчанию вашего приложения.

## <a name="factoring-strings-into-multiple-resources-files"></a>Распределение строк по нескольким файлам ресурсов

Все строки можно хранить в одном файле ресурсов (resw) либо распределить их по нескольким файлам ресурсов. Например, вы решите хранить сообщения об ошибках в одном файле ресурсов, строки из манифеста пакета приложения — в другом, а строки пользовательского интерфейса — в третьем. Вот как будет выглядеть структура папок в таком случае.

![Добавление ресурса (английский язык)](images/manifest-resources.png)

Чтобы ссылка на идентификатор строкового ресурса указывала на определенный файл, просто добавьте `/<resources-file-name>/` перед идентификатором. В примере разметки ниже предполагается, что `ErrorMessages.resw` содержит ресурс с именем PasswordTooWeak.Text, значение которого описывает ошибку.

**XAML**
```xml
<TextBlock x:Uid="/ErrorMessages/PasswordTooWeak"/>
```

В коде ниже предполагается, что `ErrorMessages.resw` содержит ресурс с именем MismatchedPasswords, значение которого описывает ошибку.

**C#**
```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("ManifestResources");
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("/ErrorMessages/MismatchedPasswords");
```

**C++**
```cpp
auto resourceLoader = Windows::ApplicationModel::Resources::ResourceLoader::GetForCurrentView("ManifestResources");
this->myXAMLTextBlockElement->Text = resourceLoader->GetString("/ErrorMessages/MismatchedPasswords");
```

Если вы решите переместить ресурс AppDisplayName из `Resources.resw` в `ManifestResources.resw`, то в манифесте пакета приложения необходимо изменить `ms-resource:AppDisplayName` на `ms-resource:/ManifestResources/AppDisplayName`.

Необходимо лишь добавить `/<resources-file-name>/` перед идентификатором строкового ресурса для файлов ресурсов, *отличных от* `Resources.resw`. Это вызвано тем, что Resources.resw — имя файла по умолчанию, и оно используется, если имя файла не указано (как в предыдущих примерах в этом разделе).

## <a name="load-a-string-for-a-specific-language-or-other-context"></a>Загрузка строки для конкретного языка или другого контекста

По умолчанию [**ResourceContext**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live) (полученный из [**ResourceContext.GetForCurrentView**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_GetForCurrentView)) содержит значение квалификатора для каждого имени квалификатора, представляя контекст среды выполнения по умолчанию (другими словами, параметры для текущего пользователя и компьютера). Файлы ресурсов (.resw) сопоставляются&mdash;на основании квалификаторов в именах&mdash;со значениями квалификаторов в этом контексте среды выполнения.

Но в некоторых ситуациях вашему приложению требуется переопределить параметры системы и явно задать использование языка, масштаба или другого значения квалификатора при поиске подходящего файла ресурсов для загрузки. Например, вы решите предоставить пользователю возможность выбрать другой язык для подсказок или сообщений об ошибках.

Это можно сделать, создав новый **ResourceContext** (вместо используемого по умолчанию) и переопределив его значения, а затем использовать этот объект контекста при поиске строки.

**C#**
```csharp
var resourceContext = new Windows.ApplicationModel.Resources.Core.ResourceContext(); // not using ResourceContext.GetForCurrentView
resourceContext.QualifierValues["Language"] = "de-DE";
var resourceMap = Windows.ApplicationModel.Resources.Core.ResourceManager.Current.MainResourceMap.GetSubtree("Resources");
this.myXAMLTextBlockElement.Text = resourceMap.GetValue("Farewell", resourceContext).ValueAsString;
```

Использование **QualifierValues**, как в примере кода выше, работает для любого квалификатора. В случае с языком можно также применить следующий метод.

**C#**
```csharp
resourceContext.Languages = new string[] { "de-DE" };
```

Для обеспечения того же эффекта на глобальном уровне *можно* переопределить значения квалификатора в **ResourceContext** по умолчанию. Но вместо этого рекомендуется вызвать [**ResourceContext.SetGlobalQualifierValue**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_). Следует задать значения один раз с помощью вызова **SetGlobalQualifierValue**, и затем эти значения всегда используются в **ResourceContext** по умолчанию для поиска.

**C#**
```csharp
Windows.ApplicationModel.Resources.Core.ResourceContext.SetGlobalQualifierValue("Language", "de-DE");
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("Farewell");
```

У некоторых квалификаторов есть системный поставщик данных. Поэтому вместо вызова **SetGlobalQualifierValue** можно настроить поставщик через его собственный API. Например, в этом коде показано, как задать [**PrimaryLanguageOverride**](/uwp/api/Windows.Globalization.ApplicationLanguages?branch=live#Windows_Globalization_ApplicationLanguages_PrimaryLanguageOverride).

**C#**
```csharp
Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride = "de-DE";
```

## <a name="updating-strings-in-response-to-qualifier-value-change-events"></a>Обновление строк в качестве реакции на события изменения значений квалификаторов

Запущенное приложение может реагировать на изменения в параметрах системы, которые влияют на значения квалификаторов в **ResourceContext** по умолчанию. Любой из этих параметров системы вызывает событие [**MapChanged**](/uwp/api/windows.foundation.collections.iobservablemap_k_v_?branch=live#Windows_Foundation_Collections_IObservableMap_2_MapChanged) на [**ResourceContext.QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_QualifierValues).

В ответ на это событие можно снова загрузить строки из **ResourceContext** по умолчанию.

**C#**
```csharp
public MainPage()
{
    this.InitializeComponent();

    ...

    // Subscribe to the event that's raised when a qualifier value changes.
    var qualifierValues = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues;
    qualifierValues.MapChanged += new Windows.Foundation.Collections.MapChangedEventHandler<string, string>(QualifierValues_MapChanged);
}

private async void QualifierValues_MapChanged(IObservableMap<string, string> sender, IMapChangedEventArgs<string> @event)
{
    var dispatcher = this.myXAMLTextBlockElement.Dispatcher;
    if (dispatcher.HasThreadAccess)
    {
        this.RefreshUIText();
    }
    else
    {
        await dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () => this.RefreshUIText());
    }
}

private void RefreshUIText()
{
    var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();
    this.myXAMLTextBlockElement.Text = resourceLoader.GetString("Farewell");
}
```

## <a name="loading-strings-from-a-class-library-or-a-windows-runtime-library"></a>Загрузка строк из библиотеки классов или библиотеки среды выполнения Windows

Строковые ресурсы в указываемой ссылкой библиотеке классов (универсальная платформа Windows) или [библиотеке среды выполнения Windows (универсальная платформа Windows)](../winrt-components/index.md) обычно добавляются в подпапку пакета, в который они включаются во время сборки. Идентификатор ресурса такой строки обычно принимает форму *LibraryName/ResourcesFileName/ResourceIdentifier*.

Библиотека может получить ResourceLoader для собственных ресурсов. Например, в код ниже показано, как библиотека или приложение, которое ссылается на нее, может получить ResourceLoader для строковых ресурсов библиотеки.

**C#**
```csharp
var resourceLoader = new Windows.ApplicationModel.Resources.ResourceLoader("ContosoControl/Resources");
resourceLoader.GetString("string1");
```

## <a name="loading-strings-from-other-packages"></a>Загрузка строк из других пакетов

Управление ресурсами для пакета приложения и получение доступа к ним осуществляется посредством [ResourceMap](/uwp/api/windows.applicationmodel.resources.core.resourcemap?branch=live) верхнего уровня этого пакета, доступ к которому можно получить из текущего [ResourceManager](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live). В каждом пакете различные компоненты могут иметь собственные поддеревья ResourceMap, доступ к которым можно получить через [ResourceMap.GetSubtree](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceMap#Windows_ApplicationModel_Resources_Core_ResourceMap_GetSubtree_System_String_).

Платформенный пакет может обращаться к собственным ресурсам с использованием абсолютного идентификатора ресурса (URI). См. также раздел [Схемы URI](uri-schemes.md).

## <a name="important-apis"></a>Важные API

* [ApplicationModel.Resources.ResourceLoader](https://msdn.microsoft.com/library/windows/apps/br206014)

## <a name="related-topics"></a>Статьи по теме

* [Перенос XAML и пользовательского интерфейса](../porting/wpsl-to-uwp-porting-xaml-and-ui.md#localization-and-globalization)
* [Директива x:Uid](../xaml-platform/x-uid-directive.md)
* [вложенные свойства](../xaml-platform/attached-properties-overview.md)
* [Локализуемые элементы манифеста](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live)
* [Тег языка BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302)
* [Адаптация ресурсов с учетом языка, масштаба и других квалификаторов](tailor-resources-lang-scale-contrast.md)
* [Загрузка строковых ресурсов](https://msdn.microsoft.com/library/windows/apps/xaml/hh965323)