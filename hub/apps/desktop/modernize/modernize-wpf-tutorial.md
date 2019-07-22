---
description: Этот учебник демонстрирует добавление пользовательских интерфейсах XAML UWP, создание MSIX пакеты и другие современные компоненты включить в приложении WPF.
title: Учебник. Модернизация приложения WPF
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, универсальной платформы Windows, windows forms, wpf, о-ва xaml
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 5e7179d4aeb66cad547e31e2456da2e8264ebbcd
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420084"
---
# <a name="tutorial-modernize-a-wpf-app"></a>Учебник. Модернизация приложения WPF 

Существует много способов [модернизировать](index.md) существующих классических приложений путем интеграции новых функций Windows в существующий исходный код вместо перезаписи приложения с нуля. В этом руководстве мы рассмотрим несколько способов для модернизации существующих бизнес-приложения WPF с помощью этих функций:

* .NET Core 3
* Элементы управления XAML UWP с помощью XAML о-ва
* Адаптивная карты и Windows 10 уведомления
* Развертывание MSIX

В этом руководстве требуется следующие навыки разработки:

* Опыт в разработке классических приложений Windows с WPF.
* Базовые знания о C# и XAML.
* Общие сведения об универсальной платформы Windows.

## <a name="overview"></a>Обзор

Этот учебник содержит код для простого бизнес-приложения WPF с именем Contoso расходы. В вымышленной сценарии учебника расходы Contoso является внутренним используется руководителями корпорации Contoso для отслеживания расходов, отправленных в своих отчетах. Менеджеры теперь в руках все сенсорные устройства, и они хотели бы использовать приложение Contoso расходы без мыши или клавиатуры. К сожалению, текущая версия приложения не touch понятное.

Contoso необходимо модернизировать это приложение с новыми функциями Windows, чтобы пользователи могли более эффективно создавать отчеты расходы. Многие функции можно легко реализовать при создании нового приложения универсальной платформы Windows. Тем не менее существующее приложение сложна и является результатом многих лет разработки разными командами. Таким образом его перезаписи с нуля с помощью новой технологии не представляется возможным. Команда ищет лучший способ добавления новых функций в существующей базе кода.

В начале этого руководства, Contoso расходы на платформе .NET Framework 4.7.2 и использует следующие сторонние библиотеки:

* MVVM Light базовую реализацию для шаблона MVVM.
* Unity, контейнер внедрения зависимостей.
* LiteDb внедренного решения NoSQL для хранения данных.
* Фиктивный, средства создания фиктивными данными.

В этом руководстве вы улучшите Contoso расходы с новыми функциями Windows:

* Перенос существующего приложения WPF в .NET Core 3.0. Откроется новых и важных сценариев в будущем.
* О-ва XAML используется для размещения **InkCanvas** и **MapControl** элементы управления, предоставляемые Windows Community Toolkit в оболочку.
* О-ва XAML используется для размещения любого стандартного элемента управления XAML универсальной платформы Windows (в этом случае **CalendardView**).
* Интегрируйте адаптивной карты и Windows 10 уведомления в приложение.
* Пакет приложения с MSIX и настройка Непрерывной интеграции и конвейера на DevOps в Azure, таким образом, можно автоматически передавать новые версии приложения для тест-инженеров и пользователей, как только он доступен.

## <a name="prerequisites"></a>предварительные требования

Для работы с этим учебником на компьютере разработки должен быть установлены следующие необходимые компоненты:

* Windows 10, версия 1903 (сборка 18362) или более поздней версии.
* [Visual Studio 2019](https://www.visualstudio.com).
* [Пакет SDK для предварительной версии 3 .NET core](https://dotnet.microsoft.com/download/dotnet-core/3.0) (установите последнюю версию доступна Предварительная версия).

Убедитесь, что установлены следующие рабочие нагрузки и дополнительные функции с помощью Visual Studio 2019:

* Разработка классических приложений .NET
* "Разработка приложений для универсальной платформы Windows".
* Пакет SDK для Windows 10 (10.0.18362.0 или более поздней версии)

## <a name="get-the-contoso-expenses-sample-app"></a>Получение примера приложения Contoso расходы

Прежде чем приступить к этому руководству, скачайте исходный код для приложения Contoso и убедитесь, что можно создать код в Visual Studio.

1. Скачать исходный код приложения из **выпуски** вкладке [AppConsult WinAppsModernization семинар репозитория](https://github.com/Microsoft/AppConsult-WinAppsModernizationWorkshop). Прямая ссылка является [ https://aka.ms/wamwc ](https://aka.ms/wamwc).
2. Откройте ZIP-файл и извлеките все содержимое в корне вашего **C:\\**  диска. Он создаст папку с именем **C:\WinAppsModernizationWorkshop**.
3. Откройте Visual Studio 2019 и дважды щелкните **C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses\ContosoExpenses.sln** файл, чтобы открыть решение.
4. Убедитесь, что можно создавать, запуска и отладки проекта WPF расходы компании Contoso, нажав клавишу **запустить** кнопку или CTRL + F5.

## <a name="get-started"></a>Начало работы

После того как у вас есть исходный код для примера приложения Contoso расходы и убедитесь, что его можно создать в Visual Studio, вы будете готовы начать работу с учебником:

* [Часть 1. Перенос Contoso расходы приложений в .NET Core 3](modernize-wpf-tutorial-1.md)
* [Часть 2. Добавьте элемент управления InkCanvas универсальной платформы Windows, с помощью XAML о-ва](modernize-wpf-tutorial-2.md)
* [Часть 3. Добавьте элемент управления представления календаря для универсальной платформы Windows, с помощью XAML о-ва](modernize-wpf-tutorial-3.md)
* [Часть 4. Добавление действия пользователей Windows 10 и уведомления](modernize-wpf-tutorial-4.md)
* [Часть 5. Упаковка и развертывание с помощью MSIX](modernize-wpf-tutorial-5.md)

## <a name="important-concepts"></a>Основные понятия

Следующие разделы содержат фона для некоторых основных понятий, обсуждавшимся в этом учебнике. Если вы уже знакомы с этими понятиями, этот раздел можно пропустить.

### <a name="universal-windows-platform-uwp"></a>Универсальная платформа Windows (UWP)

В Windows 8 корпорация Microsoft представила новый платформу, называемую среды выполнения Windows (WinRT). В отличие от .NET Framework WinRT — это собственный уровень API-интерфейсов, которые представлены непосредственно в приложения. WinRT также появился языковых проекций, которые будут добавлены на основе среды выполнения, чтобы позволить разработчикам взаимодействовать с ним с помощью языков, таких как уровня C# и JavaScript в дополнение к C++. Проекции позволяют разработчикам создавать приложения на базе WinRT, воспользуйтесь той C# полученным опытом и XAML знаниями они в создание приложений с помощью .NET Framework. 

В Windows 10, корпорация Майкрософт представила [универсальной платформы Windows (UWP)](/windows/uwp/get-started/universal-application-platform-guide), которая опирается на WinRT. Наиболее важной возможностью UWP являются общий набор интерфейсов API на любых платформах устройств: независимо от того, если приложение выполняется на настольном компьютере, на Xbox One или HoloLens, вы сможете использовать те же API.

Забегая вперед, для большинства новых Windows 10 функции предоставляются через API-интерфейсы WinRT, включая функции, такие как временная шкала, рим проекта и Windows Hello.

### <a name="msix-packaging"></a>Упаковка MSIX

[MSIX](http://aka.ms/msix) (прежнее название AppX) — это модель современных упаковки для приложений Windows. MSIX поддерживает приложений универсальной платформы Windows, а также Классические приложения, сборка выполняется с использованием технологий, таких как Win32, WPF, Windows Forms, Java, Electron и многое другое. При создании пакета классического приложения в пакет MSIX, можно опубликовать приложение для Microsoft Store. Классические приложения также получить удостоверение пакета при установке, что позволяет использовать более широкого набора API-интерфейсы WinRT свое приложение.

Дополнительные сведения см. в статьях:

* [Пакет настольных приложений](/windows/uwp/porting/desktop-to-uwp-root)
* [За кулисами упакованные приложения рабочего стола](/windows/uwp/porting/desktop-to-uwp-behind-the-scenes)

### <a name="xaml-islands"></a>О-ва XAML

Начиная с Windows 10, версия 1903, вы можете размещать элементы управления универсальной платформы Windows в классических приложениях не универсальной платформы Windows, используя компонент, именуемый *XAML о-ва*. Эта функция позволяет улучшить представление и функциональные возможности существующих классических приложений с новейшими функциями пользовательского интерфейса Windows 10, которые доступны только через элементы управления универсальной платформы Windows. Это означает, что можно использовать функции универсальной платформы Windows, такие как Windows рукописного ввода и элементов управления, поддерживающих Fluent Design System в существующих WPF, Windows Forms, и C++ приложения Win32.

Дополнительные сведения см. в разделе [управления универсальной платформы Windows в настольных приложениях (XAML о-ва)](/windows/uwp/xaml-platform/xaml-host-controls). Этот учебник поможет выполнить процесс использования два различных типа элементов остров XAML:

* [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) и [MapControl](https://docs.microsoft.com/en-us/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) в наборе средств сообщества Windows. Эти элементы управления WPF wrap интерфейс и функциональные возможности соответствующих элементов управления универсальной платформы Windows и могут быть использованы как любой другой элемент управления WPF в конструкторе Visual Studio.

* UWP [представлении календаря](/windows/uwp/design/controls-and-patterns/calendar-view) элемента управления. Это стандартный элемент управления универсальной платформы Windows, где будет размещаться с помощью [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) элемента управления в наборе средств сообщества Windows.

### <a name="net-core-3"></a>.NET Core 3

[.NET core](https://docs.microsoft.com/dotnet/core/) — это платформа с открытым исходным кодом, которая реализует кросс платформенных, упрощенная и легко расширяется версия полной версии платформы .NET Framework. По сравнению с полной версии платформы .NET Framework, время запуска .NET Core выполняется гораздо быстрее, и многие из API-интерфейсы оптимизированы.

Через ее первого несколько выпусков фокус .NET Core был для поддержки веб- или серверной части приложения. С помощью .NET Core можно легко создавать масштабируемые веб-приложения или API, которые можно размещать на Windows, Linux, или в микрослужбы архитектурах, таких как контейнеры Docker.

.NET Core 3 — это следующий основной выпуск .NET Core. Основным преимуществом этого предстоящего выпуска является поддержка классических приложений для Windows, включая приложения Windows Forms и WPF. Можно запустить новые и существующие Классические приложения Windows на .NET Core 3 и пользоваться всеми преимуществами, которые может предложить .NET Core. В приложениях для Windows Forms и WPF, предназначенных для .NET Core 3, вы сможете использовать элементы управления UWP, доступные в [XAML Islands](xaml-islands.md).

> [!NOTE]
> WPF и Windows Forms не становятся кросс платформенной и не может выполнить WPF или Windows Forms в Linux и MacOS. Компоненты пользовательского интерфейса WPF и Windows Forms по-прежнему зависят от Windows система отрисовки.

Дополнительные сведения см. в следующих статьях:

* [Объявление о выпуске .NET Core 3.0 (предварительная версия 1)](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-1-and-open-sourcing-windows-desktop-frameworks/)
* [Объявление о выпуске .NET Core 3.0 (предварительная версия 2)](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-2/)
* [Объявление о выпуске .NET Core 3.0 (предварительная версия 2)](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-3/)
* [Объявление о выпуске .NET Core 3.0 (предварительная версия 4)](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-4/)
* [Новые возможности в .NET Core 3.0](https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0)