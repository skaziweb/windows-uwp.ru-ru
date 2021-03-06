---
ms.assetid: E0189423-1DF3-4052-AB2E-846EA18254C4
description: В этом разделе показано, как применять эффекты к предварительному просмотру камеры и потокам видеозаписи, а также как использовать эффект стабилизации видео.
title: Эффекты для видеозахвата
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ea79794f18da01438392bb0bc37b127668a53538
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/29/2019
ms.locfileid: "66361769"
---
# <a name="effects-for-video-capture"></a>Эффекты для видеозахвата


В этом разделе показано, как применять эффекты к предварительному просмотру камеры и потокам видеозаписи, а также как использовать эффект стабилизации видео.

> [!NOTE] 
> В этой статье используются понятия и код из статьи [Основные принципы фото-, аудио- и видеозахвата с помощью MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md), в которой описаны этапы реализации основных принципов фото- и видеозахвата. Мы рекомендуем ознакомиться с базовым шаблоном захвата мультимедиа в этой статье, прежде чем перейти к более сложным сценариям захвата. Код в этой статье подразумевает, что ваше приложение уже содержит экземпляр MediaCapture, инициализированный надлежащим образом.

## <a name="adding-and-removing-effects-from-the-camera-video-stream"></a>Добавление и удаление эффектов из видеопотока камеры
Для записи или предварительно просмотра видео с камеры устройства используйте объект [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture), как описано в статье [Основные принципы фото-, аудио- и видеозахвата с помощью MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md). После инициализации объекта **MediaCapture** можно добавить один или несколько видеоэффектов в поток предварительного просмотра или захвата, вызвав метод [**AddVideoEffectAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.addvideoeffectasync) и передав объект [**IVideoEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Effects.IVideoEffectDefinition), представляющий нужный эффект, и член перечисления [**MediaStreamType**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaStreamType), указывающий, следует ли добавить эффект в потока предварительного просмотра или записи камеры.

> [!NOTE]
> На некоторых устройствах поток предварительного просмотра и поток захвата совпадают, т. е. если указать **MediaStreamType.VideoPreview** или **MediaStreamType.VideoRecord** при вызове **AddVideoEffectAsync**, эффект будет применен к обоим потокам. Вы можете определить, совпадают ли потоки предварительного просмотра и записи на текущем устройстве, проверив свойство [**VideoDeviceCharacteristic**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapturesettings.videodevicecharacteristic) в [**MediaCaptureSettings**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.mediacapturesettings) для объекта **MediaCapture**. Если значение этого свойства равно **VideoDeviceCharacteristic.AllStreamsIdentical** или **VideoDeviceCharacteristic.PreviewRecordStreamsIdentical**, то потоки одинаковые, и все эффекты будут применяться к обоим потокам.

В следующем примере эффект добавляется в потоки предварительного просмотра и записи камеры. В примере показано, как проверить, совпадают ли потоки предварительного просмотра и записи.

[!code-cs[BasicAddEffect](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetBasicAddEffect)]

Обратите внимание, что **AddVideoEffectAsync** возвращает объект, реализующий расширение [**IMediaExtension**](https://docs.microsoft.com/uwp/api/Windows.Media.IMediaExtension), которое представляет добавленный видеоэффект. Некоторые эффекты позволяют изменить свои параметры, передав [**PropertySet**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.PropertySet) методу [**SetProperties**](https://docs.microsoft.com/uwp/api/windows.media.imediaextension.setproperties).

Начиная с Windows 10 версии 1607 можно также использовать объект, возвращенный **AddVideoEffectAsync**, чтобы удалить эффект из видеоконвейера, передав его в [**RemoveEffectAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.removeeffectasync). Метод **RemoveEffectAsync** автоматически определяет, был ли параметр объекта эффекта добавлен в поток предварительного просмотра или записи, чтобы вам не нужно было указывать тип потока при вызове.

[!code-cs[RemoveOneEffect](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetRemoveOneEffect)]

Вы также можете удалить все эффекты из потока предварительного просмотра или записи, вызвав метод [**ClearEffectsAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.cleareffectsasync) и указав поток, из которого необходимо удалить все эффекты.

[!code-cs[ClearAllEffects](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetClearAllEffects)]

## <a name="video-stabilization-effect"></a>Эффект стабилизации видео

Эффект стабилизации видео управляет кадрами видеопотока, чтобы минимизировать подергивание, когда вы держите снимающее устройство в руке. Поскольку этот метод перемещает пиксели вправо, влево, вверх и вниз и эффект не учитывает содержимое за пределами видеокадра, стабилизированное видео получается несколько обрезанным по сравнению с исходным. Имеется служебная функция, которая позволяет настраивать параметры кодирования видео для оптимального управления обрезкой, выполняемой эффектом.

На устройствах с поддержкой функции оптической стабилизации изображений (OIS) видео стабилизируется посредством механического управления записывающим устройством, поэтому нет необходимости обрезать края видеокадров. Дополнительные сведения см. в статье [Доступ к элементам управления видеозахватом на устройстве](capture-device-controls-for-video-capture.md).

### <a name="set-up-your-app-to-use-video-stabilization"></a>Настройка приложения для использования стабилизации видео

Помимо пространства имен, необходимого для основного захвата мультимедиа, для использования эффекта стабилизации видео нужно следующее пространство имен.

[!code-cs[VideoStabilizationEffectUsing](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetVideoStabilizationEffectUsing)]

Объявите переменную-член для хранения объекта [**VideoStabilizationEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.VideoStabilizationEffect). В рамках реализации эффекта измените свойства кодирования, применяемые для кодирования захваченного видео. Объявите две переменные для хранения резервной копии исходных свойств кодирования входного и выходного потока, чтобы их можно было восстановить после отключения эффекта. Наконец объявите переменную-член типа [**MediaEncodingProfile**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile), поскольку этот объект будет доступен из разных мест кода.

[!code-cs[DeclareVideoStabilizationEffect](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetDeclareVideoStabilizationEffect)]

Для этого сценария следует назначать объект профиля кодирования мультимедиа переменной-члену, чтобы можно было получить к нему доступ позже.

[!code-cs[EncodingProfileMember](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetEncodingProfileMember)]

### <a name="initialize-the-video-stabilization-effect"></a>Инициализация эффекта стабилизации видео

После инициализации объекта **MediaCapture** создайте новый экземпляр объекта [**VideoStabilizationEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.VideoStabilizationEffectDefinition). Вызовите [**MediaCapture.AddVideoEffectAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.addvideoeffectasync) для добавления эффекта к видеоконвейеру и получения экземпляра класса [**VideoStabilizationEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.VideoStabilizationEffect). Определите [**MediaStreamType.VideoRecord**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaStreamType), чтобы указать, что эффект должен применяться к потоку видеозаписи.

Зарегистрируйте обработчик для события [**EnabledChanged**](https://docs.microsoft.com/uwp/api/windows.media.core.videostabilizationeffect.enabledchanged) и вызовите вспомогательный метод **SetUpVideoStabilizationRecommendationAsync**, которые рассматриваются ниже в этой статье. В заключение установите для свойства [**Enabled**](https://docs.microsoft.com/uwp/api/windows.media.core.videostabilizationeffect.enabled) значение true, чтобы включить эффект.

[!code-cs[CreateVideoStabilizationEffect](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetCreateVideoStabilizationEffect)]

### <a name="use-recommended-encoding-properties"></a>Использование рекомендуемых свойств кодирования

Как было сказано выше в этой статье, метод, используемый для эффекта стабилизации видео, неизбежно приводит к небольшой обрезке исходного видео. Определите следующую вспомогательную функцию в коде, чтобы настроить свойства кодирования видео и перекрыть это ограничение эффекта наилучшим образом. Эффект стабилизации видео можно использовать и без этого этапа, но если вы его пропустите, полученное видео будет слегка растянутым, то есть его визуальное качество будет несколько ниже.

Вызовите [**GetRecommendedStreamConfiguration**](https://docs.microsoft.com/uwp/api/windows.media.core.videostabilizationeffect.getrecommendedstreamconfiguration) для экземпляра эффекта стабилизации видео, передавая объект [**VideoDeviceController**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.VideoDeviceController), который сообщает эффекту информацию о текущих свойствах кодирования входного потока и вашем профиле **MediaEncodingProfile**, благодаря чему эффект получает информацию о текущих свойствах кодирования на выходе. Этот метод возвращает объект [**VideoStreamConfiguration**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.VideoStreamConfiguration), содержащий новые рекомендуемые свойства кодирования входящего и выходящего потоков.

Рекомендуемые свойства кодирования входящего потока обеспечивают более высокое разрешение (если такая возможность поддерживается устройством) по сравнению с заданными исходными параметрами, чтобы минимизировать потери разрешения из-за обрезки в результате применения эффекта.

Вызовите [**VideoDeviceController.SetMediaStreamPropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.media.devices.videodevicecontroller.setmediastreampropertiesasync) для установки новых свойств кодирования. Перед установкой новых свойств используйте переменную-член для хранения исходных свойств кодирования, чтобы иметь возможность вернуть настройки при отключении эффекта.

Если эффект стабилизации должен обрезать выходящее видео, то рекомендуемые свойства кодирования выходящего потока будут обеспечивать размер обрезанного видео. Это означает, что выходящее разрешение будет соответствовать размеру обрезанного видео. Если не использовать рекомендованные свойства выходного потока, видео будет растянуто для соответствия исходному размеру выходного потока, что приведет к потере визуального качества.

Задайте свойство [**Video**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.video) объекта **MediaEncodingProfile**. Перед установкой новых свойств используйте переменную-член для хранения исходных свойств кодирования, чтобы иметь возможность вернуть настройки при отключении эффекта.

[!code-cs[SetUpVideoStabilizationRecommendationAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetSetUpVideoStabilizationRecommendationAsync)]

### <a name="handle-the-video-stabilization-effect-being-disabled"></a>Обработка отключения эффекта стабилизации видео

Система может автоматически отключать эффект стабилизации видео, если поток пикселей слишком высок и эффект не может с ним справиться либо работает медленно. В этом случае вызывается событие EnabledChanged. Экземпляр **VideoStabilizationEffect** в параметре *sender* показывает новое состояние эффекта: включен или выключен. [  **VideoStabilizationEffectEnabledChangedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.VideoStabilizationEffectEnabledChangedEventArgs) имеет значение [**VideoStabilizationEffectEnabledChangedReason**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.VideoStabilizationEffectEnabledChangedReason), показывающее, по какой причине включен или выключен эффект. Обратите внимание, что это событие также вызывается при включении или выключении эффекта программным способом. В этом случае причина будет указана как **Programmatic**.

Как правило, это событие используется для того, чтобы настроить в пользовательском интерфейсе приложения отображение текущего состояния стабилизации видео.

[!code-cs[VideoStabilizationEnabledChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetVideoStabilizationEnabledChanged)]

### <a name="clean-up-the-video-stabilization-effect"></a>Удаление эффекта стабилизации видео

Для удаления эффекта стабилизации видео вызовите [**RemoveEffectAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.removeeffectasync), чтобы удалить эффект из видеоконвейера. Если значения переменных-членов, содержащих начальные свойства кодирования, не равны null, используйте их для восстановления свойств кодирования. Наконец удалите обработчик событий **EnabledChanged** и установите для эффекта значение null.

[!code-cs[CleanUpVisualStabilizationEffect](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetCleanUpVisualStabilizationEffect)]

## <a name="related-topics"></a>См. также

* [Камера](camera.md)
* [Основные фото, видео и аудио захвата с MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 




