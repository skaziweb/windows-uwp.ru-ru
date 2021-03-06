---
ms.assetid: 1f970d38-2338-470e-b5ba-811402752fc4
description: Узнайте, как включить межстраничные объявления в приложение UWP для Windows 10 с помощью Microsoft Advertising SDK.
title: Межстраничные объявления
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp, рекламные объявления, реклама, элемент управления рекламой, межстраничные
ms.localizationpriority: medium
ms.openlocfilehash: 608933ca60532d0840a2a7bb66f13389d9f12dbd
ms.sourcegitcommit: 71f9013c41fc1038a9d6c770cea4c5e481c23fbc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/20/2020
ms.locfileid: "77506760"
---
# <a name="interstitial-ads"></a>Межстраничные объявления

>[!WARNING]
> Начиная с 1 июня 2020 г. платформа Microsoft AD монетизацию для приложений Windows UWP будет выключена. [Подробнее](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

В этом пошаговом руководстве показано, как включить межстраничные объявления в приложения универсальной платформы Windows (UWP) и игры для Windows 10. Полные примеры проектов, демонстрирующие способы добавления промежуточной рекламы в приложения на JavaScript/HTML, а также в приложения на XAML с помощью языков C# и C++, см. в [примерах рекламы на GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising).

<span id="whatareinterstitialads10"/>

## <a name="what-are-interstitial-ads"></a>Что такое промежуточная реклама?

В отличие от рекламных баннеров, которые занимают часть пользовательского интерфейса в приложении или программе, межстраничные объявления отображаются во весь экран. В играх, в основном, используются две основные формы такой рекламы.

* Рекламу *Paywall* пользователь обязан смотреть с определенной регулярной периодичностью, например между разными уровнями игры:

    ![whatisaninterstitial](images/13-ed0a333b-0fc8-4ca9-a4c8-11e8b4392831.png)

* Реклама *за вознаграждения* отображается тогда, когда пользователь намеренно ищет определенные преимущества, например подсказку или дополнительное время для прохождения уровня, и запускает рекламу из пользовательского интерфейса приложения.

Мы предоставляем два типа межстраничных объявлений для использования в приложениях и играх: **промежуточная видеореклама** и **межстраничные баннеры**.

> [!NOTE]
> API для межстраничных объявлений не обрабатывает никакие элементы пользовательского интерфейса, кроме как во время воспроизведения. См. [передовые практики размещения промежуточной рекламы](ui-and-user-experience-guidelines.md#interstitialbestpractices10), чтобы получить информацию о рекомендуемых и нерекомендуемых действиях при добавлении промежуточной рекламы в приложение.

## <a name="prerequisites"></a>Предварительные требования

* Установка [Microsoft Advertising SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) с помощью Visual Studio 2015 или более поздней версии Visual Studio. Инструкции по установке см. в [этой статье](install-the-microsoft-advertising-libraries.md).

## <a name="integrate-an-interstitial-ad-into-your-app"></a>Интеграция межстраничного объявления в приложение

Чтобы реализовать отображение промежуточной рекламы в вашем приложении, следуйте инструкциям для конкретного типа проекта:

* [XAML/. NET](#interstitialadsxaml10)
* [HTML/JavaScript](#interstitialadshtml10)
* [C++(Взаимодействие с DirectX)](#interstitialadsdirectx10)

<span id="interstitialadsxaml10"/>

### <a name="xamlnet"></a>XAML/.NET

В этом разделе приводятся примеры кода на C#, однако Visual Basic и C++ также поддерживаются для создания проектов XAML/.NET. Полный пример кода на C# см. в разделе [Пример кода промежуточной рекламы в C#](interstitial-ad-sample-code-in-c.md).

1. Откройте свой проект в Visual Studio.
    > [!NOTE]
    > Если вы используете существующий проект, откройте файл Package.appxmanifest в проекте и убедитесь, что возможность **Интернет (клиент)** выбрана. Вашему приложению эта возможность требуется для получения тестовых объявлений и настоящей рекламы.

2. Если ваш проект направлен на работу на **Любом ЦП**, обновите его, чтобы он использовал результаты сборки, предназначенные для определенной архитектуры (например **x86**). Если ваш проект направлен на работу на **Любом ЦП**, вам не удастся надлежащим образом добавить ссылку на Microsoft Advertising в приведенных ниже шагах. Дополнительные сведения см. в разделе [Ошибки, вызванные указанием Любого ЦП как целевого в вашем проекте](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Добавьте ссылку на Microsoft Advertising SDK в свой проект.

    1. В **Обозревателе решений** щелкните правой кнопкой мыши элемент **Ссылки** и выберите **Добавить ссылку...** .
    2.  В **Диспетчере ссылок** разверните раздел **Универсальная платформа Windows**, нажмите **Расширения** и выберите флажок рядом с **SDK Microsoft Advertising для XAML** (версия 10.0).
    3.  В **диспетчере ссылок** нажмите "ОК".

3.  В соответствующий файл с кодом в вашем приложении (например, в файл MainPage.xaml.cs или в файл с кодом для другой страницы) добавьте следующую ссылку на пространство имен.

    [!code-csharp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet1)]

4.  В соответствующем месте вашего приложения (например, в ```MainPage``` или на другой странице) объявите объект [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad) и несколько строковых полей, которые будут представлять собой идентификатор приложения и идентификатор рекламного блока вашей промежуточной рекламы. Следующий код присваивает полям `myAppId` и `myAdUnitId`[тестовые значения](set-up-ad-units-in-your-app.md#test-ad-units) для межстраничных объявлений.

    > [!NOTE]
    > Каждый элемент **InterstitialAd** имеет соответствующую *группу объявлений*, используемую нашими службами для передачи рекламы этому элементу управления, и каждая группа объявлений состоит из *идентификатора группы объявлений* и *идентификатора приложения*. На этих этапах вы задаете тестовые значения идентификатора группы объявлений и идентификатора приложения для своего элемента управления. Эти тестовые значения можно использовать только в тестовой версии приложения. Перед публикацией приложения в хранилище [эти тестовые значения необходимо заменить на значения в реальном времени](#release) из центра партнеров.

    [!code-csharp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet2)]

5.  В коде, который выполняется при запуске (например, в конструкторе для страницы), создайте объект **InterstitialAd** и привяжите обработчики событий к событиям этого объекта.

    [!code-csharp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet3)]

6.  Если вы хотите показать *межстраничную видеорекламу*, используйте метод [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) приблизительно за 30–60 секунд до того, как она должна отобразиться, чтобы предварительно получить рекламное объявление. Этого времени будет достаточно для того, чтобы запросить и подготовить рекламное объявление до его отображения. Не забудьте указать **AdType.Video** для этого типа рекламы.

    [!code-csharp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet4)]

    Если вы хотите показать *межстраничный баннер*, используйте метод [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) приблизительно за 5–8 секунд до того, как он должен отобразиться, чтобы предварительно получить рекламное объявление. Этого времени будет достаточно для того, чтобы запросить и подготовить рекламное объявление до его отображения. Не забудьте указать **AdType.Display** для этого типа рекламы.

    ```csharp
    myInterstitialAd.RequestAd(AdType.Display, myAppId, myAdUnitId);
    ```

6.  Проверьте место в вашем коде, где должна отображаться межстраничная видеореклама или баннер. Убедитесь, что реклама **InterstitialAd** готова к отображению, а затем отобразите ее, используя метод [Show](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show).

    [!code-csharp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet5)]

7.  Определите обработчики событий для объекта **InterstitialAd**.

    [!code-csharp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet6)]

8.  Создайте и протестируйте приложение, чтобы убедиться, что в нем отображается реклама.

<span id="interstitialadshtml10"/>

### <a name="htmljavascript"></a>HTML/JavaScript

В приведенных ниже инструкциях подразумевается, что вы уже создали проект универсального приложения для JavaScript в Visual Studio и для конкретного процессора. Полный пример кода см. в разделе [Пример кода промежуточной рекламы в JavaScript](interstitial-ad-sample-code-in-javascript.md).

1. Откройте свой проект в Visual Studio.

2. Если ваш проект направлен на работу на **Любом ЦП**, обновите его, чтобы он использовал результаты сборки, предназначенные для определенной архитектуры (например **x86**). Если ваш проект направлен на работу на **Любом ЦП**, вам не удастся надлежащим образом добавить ссылку на Microsoft Advertising в приведенных ниже шагах. Дополнительные сведения см. в разделе [Ошибки, вызванные указанием Любого ЦП как целевого в вашем проекте](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Добавьте ссылку на Microsoft Advertising SDK в свой проект.

    1. В **Обозревателе решений** щелкните правой кнопкой мыши элемент **Ссылки** и выберите **Добавить ссылку...** .
    2.  В **Диспетчере ссылок** разверните раздел **Универсальная платформа Windows**, нажмите **Расширения** и выберите флажок рядом с **SDK Microsoft Advertising для JavaScript** (версия 10.0).
    3.  В **диспетчере ссылок** нажмите "ОК".

3.  В разделе **&lt;head&gt;** HTML-файла в проекте после ссылок на JavaScript проекта из файлов default.css и default.js добавьте ссылку на файл ad.js.

    ``` HTML
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

4.  В файле JS проекта объявите объект [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad), а также несколько полей, которые будут содержать идентификатор приложения и идентификатор рекламного блока вашей промежуточной рекламы. Следующий код присваивает полям `applicationId` и `adUnitId`[тестовые значения](set-up-ad-units-in-your-app.md#test-ad-units) для межстраничных объявлений.

    > [!NOTE]
    > Каждый элемент **InterstitialAd** имеет соответствующую *группу объявлений*, используемую нашими службами для передачи рекламы этому элементу управления, и каждая группа объявлений состоит из *идентификатора группы объявлений* и *идентификатора приложения*. На этих этапах вы задаете тестовые значения идентификатора группы объявлений и идентификатора приложения для своего элемента управления. Эти тестовые значения можно использовать только в тестовой версии приложения. Перед публикацией приложения в хранилище [эти тестовые значения необходимо заменить на значения в реальном времени](#release) из центра партнеров.

    [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet1)]

5.  В коде, который выполняется при запуске (например, в конструкторе для страницы), создайте объект **InterstitialAd** и привяжите обработчики событий к событиям этого объекта.

    [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet2)]

5. Если вы хотите показать *межстраничную видеорекламу*, используйте метод [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) приблизительно за 30–60 секунд до того, как она должна отобразиться, чтобы предварительно получить рекламное объявление. Этого времени будет достаточно для того, чтобы запросить и подготовить рекламное объявление до его отображения. Не забудьте указать **InterstitialAdType.video** для этого типа рекламы.

    [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet3)]

    Если вы хотите показать *межстраничный баннер*, используйте метод [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) приблизительно за 5–8 секунд до того, как он должен отобразиться, чтобы предварительно получить рекламное объявление. Этого времени будет достаточно для того, чтобы запросить и подготовить рекламное объявление до его отображения. Не забудьте указать **InterstitialAdType.display** для этого типа рекламы.

    ```js
    if (interstitialAd) {
        interstitialAd.requestAd(MicrosoftNSJS.Advertising.InterstitialAdType.display, applicationId, adUnitId);
    }
    ```

6.  Проверьте то место вашего когда, в котором должна отображаться ваша реклама, на предмет того, что реклама **InterstitialAd** готова к отображению, а затем отобразите ее, используя метод [Show](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show).

    [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/samples.js#Snippet4)]

7.  Определите обработчики событий для объекта **InterstitialAd**.

    [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/samples.js#Snippet5)]

9.  Создайте и протестируйте приложение, чтобы убедиться, что в нем отображается реклама.

<span id="interstitialadsdirectx10"/>

### <a name="c-directx-interop"></a>C++ (межпрограммное взаимодействие DirectX)

В этом примере предполагается, что вы уже создали проект **приложения DirectX и XAML (универсальное приложение для Windows)** на C++ в Visual Studio и для определенной архитектуры ЦП.
 
1. Откройте свой проект в Visual Studio.

3. Добавьте ссылку на Microsoft Advertising SDK в свой проект.

    1. В **Обозревателе решений** щелкните правой кнопкой мыши элемент **Ссылки** и выберите **Добавить ссылку...** .
    2.  В **Диспетчере ссылок** разверните раздел **Универсальная платформа Windows**, нажмите **Расширения** и выберите флажок рядом с **SDK Microsoft Advertising для XAML** (версия 10.0).
    3.  В **диспетчере ссылок** нажмите "ОК".

2.  В соответствующем файле заголовка для приложения (например, в файле DirectXPage.xaml.h) объявите объект [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad) и соответствующие методы обработчика событий.  

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h#Snippet1)]

3.  В этом же файле заголовка объявите несколько строковых полей, которые будут представлять собой идентификатор приложения и идентификатор рекламного блока вашей промежуточной рекламы. Следующий код присваивает полям `myAppId` и `myAdUnitId`[тестовые значения](set-up-ad-units-in-your-app.md#test-ad-units) для межстраничных объявлений.

    > [!NOTE]
    > Каждый элемент **InterstitialAd** имеет соответствующую *группу объявлений*, используемую нашими службами для передачи рекламы этому элементу управления, и каждая группа объявлений состоит из *идентификатора группы объявлений* и *идентификатора приложения*. На этих этапах вы задаете тестовые значения идентификатора группы объявлений и идентификатора приложения для своего элемента управления. Эти тестовые значения можно использовать только в тестовой версии приложения. Перед публикацией приложения в хранилище [эти тестовые значения необходимо заменить на значения в реальном времени](#release) из центра партнеров.

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h#Snippet2)]

4.  В файле CPP, в который вы хотите добавить код для отображения промежуточной рекламы, добавьте следующую ссылку на пространство имен. В следующих примерах кода предполагается, что вы добавляете код в файл DirectXPage.xaml.cpp вашего приложения.

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet3)]

6.  В коде, который выполняется при запуске (например, в конструкторе для страницы), создайте объект **InterstitialAd** и привяжите обработчики событий к событиям этого объекта. В следующем примере кода ```InterstitialAdSamplesCpp``` представляет собой пространство имен для вашего проекта. Измените это имя с учетом содержания вашего кода.

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet4)]

7. Если вы хотите показать *межстраничную видеорекламу*, используйте метод [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) приблизительно за 30–60 секунд до того, как она должна отобразиться, чтобы предварительно получить рекламное объявление. Этого времени будет достаточно для того, чтобы запросить и подготовить рекламное объявление до его отображения. Не забудьте указать **AdType::Video** для этого типа рекламы.

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet5)]

    Если вы хотите показать *межстраничный баннер*, используйте метод [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) приблизительно за 5–8 секунд до того, как он должен отобразиться, чтобы предварительно получить рекламное объявление. Этого времени будет достаточно для того, чтобы запросить и подготовить рекламное объявление до его отображения. Не забудьте указать **AdType::Display** для этого типа рекламы.

    ```cpp
    m_interstitialAd->RequestAd(AdType::Display, myAppId, myAdUnitId);
    ```

7.  Проверьте то место вашего когда, в котором должна отображаться ваша реклама, на предмет того, что реклама **InterstitialAd** готова к отображению, а затем отобразите ее, используя метод [Show](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show).

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet6)]

8.  Определите обработчики событий для объекта **InterstitialAd**.

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet7)]

9. Создайте и протестируйте приложение, чтобы убедиться, что в нем отображается реклама.

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>Выпуск приложения с реальной рекламой

1. Убедитесь, что использование вами межстраничных объявлений в приложении соответствует нашим [рекомендациям для межстраничных объявлений](ui-and-user-experience-guidelines.md#interstitialbestpractices10).

2.  В центре партнеров перейдите на страницу [рекламных объявлений в приложении](../publish/in-app-ads.md) и [Создайте модуль AD](set-up-ad-units-in-your-app.md#live-ad-units). В качестве типа группы объявлений выберите пункт **Вставка видео** или **Вставка баннера**в зависимости от того, какой тип межстраничного объявления необходимо показать. Запишите идентификатор рекламного блока и идентификатор приложения.
    > [!NOTE]
    > Значения идентификатора приложения для тестовых рекламных блоков и реальных рекламных блоков UWP имеют разные форматы. Тестовые значения идентификатора приложения представляют собой элементы GUID. При создании активной единицы AD UWP в центре партнеров значение идентификатора приложения для единицы AD всегда совпадает с ИДЕНТИФИКАТОРом магазина для вашего приложения (пример значения идентификатора магазина выглядит как 9NBLGGH4R315).

3. Вы можете при необходимости включить рекламный посредник для **InterstitialAd**, настроив параметры в разделе [Параметры посредника](../publish/in-app-ads.md#mediation) на странице [Реклама в приложении](../publish/in-app-ads.md). С помощью рекламного посредника можно максимально увеличить выручку от рекламы и возможности ее продвижения, отображая рекламу от нескольких рекламных сетей, в том числе других платных рекламных сетей, например Taboola и Smaato, и рекламных объявлений для кампаний по продвижению приложения Microsoft.

4.  В коде замените значения тестовой единицы тестирования на значения Live, созданные в центре партнеров.

5.  [Отправьте приложение](../publish/app-submissions.md) в магазин с помощью центра партнеров.

6.  Изучите [отчеты о производительности рекламы](../publish/advertising-performance-report.md) в центре партнеров.

<span id="manage" />

## <a name="manage-ad-units-for-multiple-interstitial-ad-controls-in-your-app"></a>Управление группами объявлений для нескольких элементов управления межстраничными объявлениями в приложении

В одном приложении можно использовать несколько элементов управления **InterstitialAd**. В этом случае рекомендуется назначить каждому элементу управления свою группу объявлений. Использование различных групп объявлений для каждого элемента управления позволяет по отдельности [настраивать параметры посредника](../publish/in-app-ads.md#mediation) и получать раздельные [данные отчетности](../publish/advertising-performance-report.md) для каждого элемента управления. Это также позволяет нашим службам лучше оптимизировать рекламные объявления, которые мы передаем вашему приложению.

> [!IMPORTANT]
> Одну группу объявлений можно использовать только в одном приложении. Если использовать одну группу объявлений в нескольких приложениях, объявления для этой группы объявлений предоставляться не будут.

## <a name="related-topics"></a>Связанные разделы

* [Рекомендации по внутреннего AD](ui-and-user-experience-guidelines.md#interstitialbestpractices10)
* [Пример кода внутреннего AD вC#](interstitial-ad-sample-code-in-c.md)
* [Пример кода внутреннего AD на JavaScript](interstitial-ad-sample-code-in-javascript.md)
* [Примеры рекламы на GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)
* [Настройка единиц AD для приложения](set-up-ad-units-in-your-app.md)
