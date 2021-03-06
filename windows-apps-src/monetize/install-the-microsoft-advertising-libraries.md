---
ms.assetid: 3aeddb83-5314-447b-b294-9fc28273cd39
description: Узнайте о том, как устанавливать Microsoft Advertising SDK.
title: Установить пакет Microsoft Advertising SDK
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp, реклама, рекламные объявления, установка, SDK, рекламная библиотека
ms.localizationpriority: medium
ms.openlocfilehash: 109ddbd3551dbd4304b86e56ace40f39e1b71211
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/13/2020
ms.locfileid: "79209640"
---
# <a name="install-the-microsoft-advertising-sdk"></a>Установить пакет Microsoft Advertising SDK

>[!WARNING]
> Начиная с 1 июня 2020 г. платформа Microsoft AD монетизацию для приложений Windows UWP будет выключена. [Подробнее](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Для отображения рекламы в приложениях UWP для Windows 10 установите [Microsoft Advertising SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK). Этот пакет SDK является расширением Visual Studio 2015 и последующих версий.

> [!NOTE]
> Если вы разрабатываете приложение UWP на JavaScript или HTML и установили пакет SDK для Windows 10 версии 10.0.14393 (годовщина) или более позднюю версию, необходимо также установить библиотеку [WinJS](https://github.com/winjs/winjs) . Эта библиотека ранее включалась в предыдущие версии Windows 10 SDK, но, начиная с Windows 10 SDK версии 10.0.14393 (юбилейное обновление), ее необходимо устанавливать отдельно.

<span id="install-msi" />

## <a name="install-via-msi"></a>Установка с помощью MSI

Установка Microsoft Advertising SDK с помощью установщика MSI.

1.  Закройте все экземпляры Visual Studio.

2. Если вы ранее устанавливали какую-либо из предыдущих версий пакетов Microsoft Advertising SDK, Universal Ad Client SDK, расширения Ad Mediator или Microsoft Store Engagement and Monetization SDK, теперь необходимо удалить эти версии пакетов SDK. Другой вариант: откройте окно **командной строки** и выполните эти команды для удаления всех более ранних версий пакетов рекламных SDK, которые могли быть установлены вместе с Visual Studio, но, возможно, не отображаются в списке установленных программ на компьютере:
    ```console
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Скачайте и установите пакет [Microsoft Advertising SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK). Установка может занять несколько минут. Обязательно дождитесь завершения процесса.

4.  Перезапустите Visual Studio.

5.  Если у вас есть существующий проект, который ссылается на рекламные библиотеки из какой-либо более ранней версии пакетов Microsoft Advertising SDK, Universal Ad Client SDK или Microsoft Store Engagement and Monetization SDK, рекомендуется открыть этот проект в Visual Studio, очистить и заново собрать проект (в **Обозревателе решений** щелкните правой кнопкой мыши по узлу проекта и выберите пункт **Очистить**, затем снова щелкните правой кнопкой мыши по узлу проекта и выберите **Пересобрать**).

  В противном случае, если вы используете пакет Microsoft Advertising SDK в первый раз в своем проекте, теперь вы готовы [добавить ссылку на Microsoft Advertising SDK](#reference).

<span id="install-nuget" />

## <a name="install-via-nuget"></a>Установка с помощью NuGet

Установка библиотек Microsoft Advertising SDK в конкретный проект UWP с помощью NuGet.

1.  Закройте все экземпляры Visual Studio.

2.  Если вы ранее устанавливали какую-либо из предыдущих версий пакетов Microsoft Advertising SDK, Universal Ad Client SDK, расширения Ad Mediator или Microsoft Store Engagement and Monetization SDK, теперь необходимо удалить эти версии пакетов SDK. Другой вариант: откройте окно **командной строки** и выполните эти команды для удаления всех более ранних версий пакетов рекламных SDK, которые могли быть установлены вместе с Visual Studio, но, возможно, не отображаются в списке установленных программ на компьютере:
    ```console
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Запустите Visual Studio и откройте проект, в котором вы хотите использовать библиотеки Microsoft Advertising SDK.
    > [!NOTE]
    > Если ваш проект уже содержит ссылки на библиотеки из предыдущей установки пакета SDK с помощью MSI, удалите эти ссылки из проекта. Рядом с этими ссылками будут расположены предупреждающие значки, поскольку библиотеки, на которые они ссылаются, были удалены ранее.

4. В Visual Studio щелкните **Проект** и выберите параметр **Управление пакетами NuGet**.

5. В поле поиска введите **Microsoft.Advertising.XAML** (для проектов на XAML) или **Microsoft.Advertising.JS** (для проектов на JavaScript и HTML) и установите соответствующий пакет. Когда установка пакета завершится, сохраните решение.
    > [!NOTE]
    > Если в окне **Вывод** содержится ошибка *Install-Package*, указывающая, что заданный путь слишком длинный, вам может потребоваться настроить NuGet, чтобы извлечь пакеты в альтернативное расположение с более коротким путем, чем расположение по умолчанию. Для этого добавьте значение `repositoryPath` в файл nuget.config на компьютере и задайте ему более короткий путь к папке, куда можно извлечь пакеты NuGet. Дополнительные сведения см. в [этой статье](https://docs.microsoft.com/nuget/consume-packages/configuring-nuget-behavior) в документации NuGet. Кроме того можно попробовать переместить проект Visual Studio в другую папку с более коротким путем.

6. Закройте решение и снова откройте его.

7.  Если проект уже содержит ссылки на библиотеки из более ранней версии Microsoft Advertising SDK, которая была установлена с помощью NuGet, и вы обновили свой проект до более нового выпуска SDK, рекомендуется очистить и пересобрать проект (в **Обозревателе решений** щелкните правой кнопкой мыши по узлу проекта и выберите пункт **Очистить**, затем снова щелкните правой кнопкой мыши по узлу проекта и выберите **Пересобрать**).

  В противном случае, если вы используете пакет SDK в первый раз в своем проекте, теперь вы готовы [добавить ссылку на Microsoft Advertising SDK](#reference).

<span id="reference" />

## <a name="add-a-reference-to-the-microsoft-advertising-sdk"></a>Добавление ссылки на Microsoft Advertising SDK

После установки пакета Microsoft Advertising SDK следуйте этим инструкциям, чтобы создать ссылку на SDK в своем проекте и иметь возможность использовать рекламные API-интерфейсы.

1. Откройте свой проект в Visual Studio.
    > [!NOTE]
    > Если ваш проект направлен на работу на **Любом ЦП**, обновите его, чтобы он использовал результаты сборки, предназначенные для определенной архитектуры (например **x86**). Если ваш проект направлен на работу на **Любом ЦП**, вам не удастся надлежащим образом добавить ссылку на Microsoft Advertising SDK в приведенных ниже шагах. Дополнительные сведения см. в разделе [Ошибки, вызванные указанием Любого ЦП как целевого в вашем проекте](known-issues-for-the-advertising-libraries.md#reference_errors).

2. В **обозревателе решений**щелкните правой кнопкой мыши пункт **Ссылки** и выберите **Добавить ссылку...** .

3. В **диспетчере ссылок** разверните **Universal Windows**, нажмите **Расширения** и затем установите флажок рядом с **Microsoft Advertising SDK для XAML** (для приложений, использующих XAML) или **Microsoft Advertising SDK для JavaScript** (для приложений, созданных с помощью JavaScript и HTML).

4.  В **диспетчере ссылок** нажмите "ОК".

Руководства, в которых рассказывается, как приступить к использованию API рекламы, см. в следующих статьях.

* [Межстраничные объявления](interstitial-ads.md)
* [Собственные объявления](native-ads.md)
* [Адконтрол в XAML и .NET](adcontrol-in-xaml-and--net.md)
* [Адконтрол в HTML 5 и JavaScript](adcontrol-in-html-5-and-javascript.md)

<span id="framework" />

## <a name="understanding-framework-packages-in-the-microsoft-advertising-sdk"></a>Общие сведения о пакетах платформы в Microsoft Advertising SDK

Библиотека Microsoft.Advertising.dll в [Microsoft Advertising SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) (только UWP) настроена как *пакет платформы*. Эта библиотека содержит API-интерфейсы рекламы в пространствах имен [Microsoft.Advertising](https://docs.microsoft.com/uwp/api/microsoft.advertising) и [Microsoft.Advertising.WinRT.UI](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui).

Поскольку библиотека представляет собой пакет платформы, это означает, что после установки пользователем версии вашего приложения, которое применяет эту библиотеку, библиотека будет автоматически обновляться на устройстве пользователя через Центр обновления Windows, когда мы опубликуем новую версию библиотеки с исправлениями и улучшенной производительностью. Это позволяет гарантировать, что ваши клиенты всегда будут иметь последнюю доступную версию библиотеки на своих устройствах.

Если мы выпустим новую версию SDK с новыми API или функциями в этой библиотеке, вам придется установить последнюю версию пакета SDK, чтобы использовать их. В этом случае вам также понадобится опубликовать обновленное приложение в Магазине.
