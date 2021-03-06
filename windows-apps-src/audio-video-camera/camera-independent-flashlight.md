---
ms.assetid: D20C8E01-4E78-4115-A2E8-07BB3E67DDDC
description: В этой статье можно найти информацию о том, как получить доступ к фонарику устройства (при наличии) и использовать его. Управление фонариком осуществляется отдельно от управления камерой и вспышкой устройства.
title: Независимый от камеры фонарик
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ddc7ad87a883c3512c719167975428b6c7746031
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/29/2019
ms.locfileid: "66359061"
---
# <a name="camera-independent-flashlight"></a>Независимый от камеры фонарик



В этой статье можно найти информацию о том, как получить доступ к фонарику устройства (при наличии) и использовать его. Управление фонариком осуществляется отдельно от управления камерой и вспышкой устройства. Кроме информации о фонарике и его настройках, в этой статье приведены сведения о том, как правильно освободить ресурс фонарика, когда он не используется, а также как определить, доступен ли фонарик, если он используется другим приложением.

## <a name="get-the-devices-default-lamp"></a>Как получить доступ к фонарику устройства по умолчанию

Для доступа к фонарику устройства по умолчанию вызовите метод [**Lamp.GetDefaultAsync**](https://docs.microsoft.com/uwp/api/windows.devices.lights.lamp.getdefaultasync). API фонарика находятся в пространстве имен [**Windows.Devices.Lights**](https://docs.microsoft.com/uwp/api/Windows.Devices.Lights). Прежде чем получить доступ к этим API, обязательно добавьте для этого пространства имен директиву using.

[!code-cs[LightsNamespace](./code/Lamp/cs/MainPage.xaml.cs#SnippetLightsNamespace)]


[!code-cs[DeclareLamp](./code/Lamp/cs/MainPage.xaml.cs#SnippetDeclareLamp)]


[!code-cs[GetDefaultLamp](./code/Lamp/cs/MainPage.xaml.cs#SnippetGetDefaultLamp)]

Если возвращается объект **null**, API **Lamp** не поддерживается на устройстве. Некоторые устройства могут не поддерживать API **Lamp**, даже если фонарик присутствует на устройстве.

## <a name="get-a-specific-lamp-using-the-lamp-selector-string"></a>Как получить доступ к определенному фонарику с помощью строки селектора фонарика

На некоторых устройствах может быть несколько фонариков. Чтобы увидеть список доступных на устройстве фонариков, получите строку средства выбора устройств, вызвав метод [**GetDeviceSelector**](https://docs.microsoft.com/uwp/api/windows.devices.lights.lamp.getdeviceselector). Эту строку средства выбора можно затем передать в [**DeviceInformation.FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync). Этот метод используется для нумерации различных типов устройств, а благодаря строке селектора он возвращает только список фонариков устройства. Объект [**DeviceInformationCollection**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationCollection), возвращаемый методом **FindAllAsync**, представляет собой коллекцию объектов [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation), которые представляют доступные на устройстве фонарики. Выберите один из объектов в списке, а затем передайте свойство [**Id**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) в метод [**Lamp.FromIdAsync**](https://docs.microsoft.com/uwp/api/windows.devices.lights.lamp.fromidasync) для получения ссылки на запрашиваемый фонарик. В этом примере метод расширения **GetFirstOrDefault** из пространства имен **System.Linq** используется для выбора объекта **DeviceInformation**, в котором для свойства [**EnclosureLocation.Panel**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.enclosurelocation.panel) установлено значение **Back**. С помощью этого значения выбирается фонарик на задней панели корпуса устройства, если он доступен.

Обратите внимание, что API [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) находятся в пространстве имен [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration).

[!code-cs[EnumerationNamespace](./code/Lamp/cs/MainPage.xaml.cs#SnippetEnumerationNamespace)]

[!code-cs[GetLampWithSelectionString](./code/Lamp/cs/MainPage.xaml.cs#SnippetGetLampWithSelectionString)]

## <a name="adjust-lamp-settings"></a>Как настроить параметры фонарика

После получения экземпляра класса [**Lamp**](https://docs.microsoft.com/uwp/api/Windows.Devices.Lights.Lamp) включите фонарик, установив для свойства [**IsEnabled**](https://docs.microsoft.com/uwp/api/windows.devices.lights.lamp.isenabled) значение **true**.

[!code-cs[LampSettingsOn](./code/Lamp/cs/MainPage.xaml.cs#SnippetLampSettingsOn)]

Чтобы отключить его, задайте для свойства [**IsEnabled**](https://docs.microsoft.com/uwp/api/windows.devices.lights.lamp.isenabled) значение **false**.

[!code-cs[LampSettingsOff](./code/Lamp/cs/MainPage.xaml.cs#SnippetLampSettingsOff)]

На некоторых устройствах доступны фонарики, которые поддерживают значения цвета. Узнайте, поддерживает ли фонарик цвет, проверив свойство [**IsColorSettable**](https://docs.microsoft.com/uwp/api/windows.devices.lights.lamp.iscolorsettable). Если для него установлено значение **true**, вы можете задать цвет фонарика с помощью свойства [**Color**](https://docs.microsoft.com/uwp/api/windows.devices.lights.lamp.color).

[!code-cs[LampSettingsColor](./code/Lamp/cs/MainPage.xaml.cs#SnippetLampSettingsColor)]

## <a name="register-to-be-notified-if-the-lamp-availability-changes"></a>Как зарегистрировать обработчик для получения уведомлений об изменениях доступности фонарика

Доступ к фонарику предоставляется последнему приложению, которое его запрашивает. Поэтому если при запуске другого приложения оно запрашивает ресурс фонарика, который в данный момент уже используется, ваше приложение больше не сможет контролировать фонарик, пока другое приложение не освободит его ресурс. Чтобы получать уведомления при изменении доступности фонарика, зарегистрируйте обработчик для события [**Lamp.AvailabilityChanged**](https://docs.microsoft.com/uwp/api/windows.devices.lights.lamp.availabilitychanged).

[!code-cs[AvailabilityChanged](./code/Lamp/cs/MainPage.xaml.cs#SnippetAvailabilityChanged)]

В обработчике события проверьте свойство [**LampAvailabilityChanged.IsAvailable**](https://docs.microsoft.com/uwp/api/windows.devices.lights.lampavailabilitychangedeventargs.isavailable), чтобы определить, доступен ли фонарик. В этом примере переключатель, предназначенный для включения и отключения фонарика, активируется и деактивируется в зависимости от его доступности.

[!code-cs[AvailabilityChangedHandler](./code/Lamp/cs/MainPage.xaml.cs#SnippetAvailabilityChangedHandler)]

## <a name="properly-dispose-of-the-lamp-resource-when-not-in-use"></a>Как передать ресурс фонарика, если он не используется

Если вы больше не пользуетесь фонариком, его необходимо отключить, а затем вызвать метод [**Lamp.Close**](https://docs.microsoft.com/uwp/api/windows.devices.lights.lamp.close) , чтобы освободить ресурс фонарика и разрешить другим приложениям получать к нему доступ. Если вы используете C#, это свойство сопоставляется с методом **Dispose**. Если вы зарегистрировали обработчик для получения сведений о [**AvailabilityChanged**](https://docs.microsoft.com/uwp/api/windows.devices.lights.lamp.availabilitychanged) и передаете ресурс фонарика, регистрацию следует отменить. Строка кода, с помощью которой можно передать ресурс лампы, зависит от приложения. Чтобы определить область доступа фонарика к одной странице, освободите ресурс в событии [**OnNavigatingFrom**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatingfrom).

[!code-cs[DisposeLamp](./code/Lamp/cs/MainPage.xaml.cs#SnippetDisposeLamp)]

## <a name="related-topics"></a>См. также
- [Воспроизведение мультимедиа](media-playback.md)

 




