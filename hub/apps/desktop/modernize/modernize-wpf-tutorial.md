---
description: В этом руководстве показано, как добавлять пользовательские интерфейсы XAML UWP, создавать MSIX-пакеты и включать другие актуальные компоненты в приложения WPF.
title: 'Руководство: Модернизация приложения WPF'
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, UWP, Windows Forms, WPF, объекты XAML Island
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 21049c995d467209b22fe8ea5c40d303911f2c2c
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "77521293"
---
# <a name="tutorial-modernize-a-wpf-app"></a>Руководство: Модернизация приложения WPF 

Существует множество способов [модернизировать](index.md) имеющиеся классические приложения путем интеграции новейших функций Windows в существующий исходный код, вместо того чтобы создавать эти приложения с нуля. В этом руководстве мы рассмотрим несколько способов модернизации существующих бизнес-приложений WPF с помощью следующих компонентов:

* .NET Core 3
* элементы управления XAML платформы UWP с XAML Islands;
* адаптивные карточки и уведомления Windows 10.
* Развертывание MSIX

Для работы с этим руководством нужны следующие навыки разработки:

* опыт создания классических приложений для Windows на основе WPF;
* базовые навыки разработки на C# и XAML;
* базовое навыки работы с UWP.

## <a name="overview"></a>Обзор

В этом руководстве представлен код для простого бизнес-приложения WPF с именем Contoso Expenses. В нашем вымышленном сценарии для этого руководства менеджеры компании Contoso Corporation используют внутреннее приложение Contoso Expenses, чтобы отслеживать включенные в отчет расходы. Менеджеры получили в свое распоряжение сенсорные устройства, и теперь им нужно использовать приложение Contoso Expenses без мыши и клавиатуры. К сожалению, текущая версия приложения не поддерживает сенсорный ввод.

Компания Contoso решила модернизировать это приложение, добавив новые функции Windows для более эффективного создания отчетов о расходах. Многие из этих функций можно легко реализовать в новом приложении UWP. Но существующее приложение очень сложное и является результатом многих лет работы нескольких команд. Поэтому вариант переделки с нуля под новые технологии даже не рассматривается. Команда ищет оптимальный подход к добавлению новых возможностей в существующую базу кода.

На момент, соответствующий началу этого руководства, Contoso Expenses нацелено на платформу .NET Framework версии 4.7.2 и использует следующие сторонние библиотеки:

* MVVM Light — базовая реализация шаблона MVVM;
* Unity — контейнер внедрения зависимостей;
* LiteDb — встроенное решение NoSQL для хранения данных;
* Bogus — инструмент для создания фиктивных данных.

В этом руководстве объясняется, как улучшить приложение Contoso Expenses, дополнив его новыми функциями Windows.

* Миграция существующего приложения WPF на .NET Core 3.0. Это откроет вам новые и важные возможности на дальнейшую перспективу.
* Применение XAML Islands для размещения заключенных в оболочку элементов управления **InkCanvas** и **MapControl**, которые входят в набор инструментов сообщества Windows.
* Применение XAML Islands для размещения всех стандартных элементов управления XAML UWP (в нашем примере это **CalendardView**).
* Интеграция адаптивных карточек и уведомлений Windows 10 в приложение.
* Упаковка приложения с помощью MSIX и настройка конвейера CI/CD в Azure DevOps, который позволит автоматически предоставлять тест-инженерам и пользователям новые версии приложения по мере их появления.

## <a name="prerequisites"></a>Предварительные условия

Для работы с этим руководством на компьютере разработки должны быть установлены следующие компоненты:

* Windows 10, версия 1903 (сборка 18362) или более поздняя;
* [Visual Studio 2019](https://www.visualstudio.com);
* [пакет SDK для .NET Core 3](https://dotnet.microsoft.com/download/dotnet-core/3.0) (установите последнюю версию).

Обеспечьте наличие следующих рабочих нагрузок и дополнительных компонентов Visual Studio 2019.

* "Разработка классических приложений .NET".
* "Разработка приложений для универсальной платформы Windows".
* Пакет SDK для Windows 10 (версии 10.0.18362.0 и выше).

## <a name="get-the-contoso-expenses-sample-app"></a>Получение примера приложения Contoso Expenses

Перед началом работы с этим руководством скачайте исходный код приложения Contoso Expenses и убедитесь, что этот код успешно компилируется в Visual Studio.

1. Исходный код приложения можно скачать на вкладке **Releases** (Выпуски) в репозитории [AppConsult-WinAppsModernizationWorkshop](https://github.com/Microsoft/AppConsult-WinAppsModernizationWorkshop). Вот прямая ссылка: [https://github.com/microsoft/AppConsult-WinAppsModernizationWorkshop/releases](https://github.com/microsoft/AppConsult-WinAppsModernizationWorkshop/releases).
2. Откройте ZIP-файл и извлеките все его содержимое в корневую папку диска **C:\\** . В результате создастся папка **C:\WinAppsModernizationWorkshop**.
3. Откройте Visual Studio 2019 и дважды щелкните файл **C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses\ContosoExpenses.sln**, чтобы открыть это решение.
4. Убедитесь, что проект WPF для Contoso Expenses успешно компилируется, запускается и отлаживается по нажатию кнопки **Запуск** или клавиш CTRL+F5.

## <a name="get-started"></a>Приступая к работе

Получив исходный код для примера приложения Contoso Expenses и убедившись, что он компилируется в Visual Studio, переходите к работе с нашим руководством:

* [Часть 1. Перенос приложения Contoso Expenses в .NET Core 3](modernize-wpf-tutorial-1.md)
* [Часть 2. Добавление элемента управления InkCanvas универсальной платформы Windows с помощью XAML Islands](modernize-wpf-tutorial-2.md)
* [Часть 3. Добавление элемента управления CalendarView универсальной платформы Windows с помощью XAML Islands](modernize-wpf-tutorial-3.md)
* [Часть 4. Добавление действий пользователей и уведомлений для Windows 10](modernize-wpf-tutorial-4.md)
* [Часть 5. Упаковка и развертывание с помощью MSIX](modernize-wpf-tutorial-5.md)

## <a name="important-concepts"></a>Важные понятия

Следующие разделы содержат общие сведения о некоторых важных понятиях, обсуждаемых в этом руководстве. Если вы уже знакомы с этими понятиями, можете пропустить этот раздел.

### <a name="universal-windows-platform-uwp"></a>Универсальная платформа Windows (UWP)

В ОС Windows 8 корпорация Майкрософт представила новую платформу — среда выполнения Windows (WinRT). В отличие от .NET Framework платформа WinRT реализована как собственный уровень интерфейсов API, напрямую предоставляемых приложениям. Также в WinRT реализованы языковые проекции, которые добавляются поверх среды выполнения и позволяют разработчикам взаимодействовать с ними не только на C++, но и на других языках, как например C# и JavaScript. Эти проекции позволяют разработчикам создавать приложения на основе WinRT, используя те же навыки работы с C# и XAML, которые они получили при создании приложений для .NET Framework. 

В ОС Windows 10 Корпорация Майкрософт представила созданную на основе WinRT [универсальную платформу Windows](/windows/uwp/get-started/universal-application-platform-guide) (UWP). Самой важной функцией UWP является то, что она предоставляет общий набор интерфейсов API для всех платформ устройств. Вы можете использовать одни и те же интерфейсы API в приложении на настольном компьютере, Xbox One или HoloLens.

Теперь большинство новых функций Windows 10, в том числе временная шкала, Project Rome и Windows Hello, предоставляются через интерфейсы API WinRT.

### <a name="msix-packaging"></a>Упаковка MSIX

[MSIX](/windows/msix/) — современная модель упаковки для приложений Windows. MSIX поддерживает приложения UWP и классические приложения, в которых используются технологии Win32, WPF, Windows Forms, Java, Electron и др. Классическое приложение, упакованное в пакет MSIX, можно опубликовать в Microsoft Store. Также классическое приложение при установке получает идентификатор пакета, что позволяет классическому приложению использовать более широкий набор интерфейсов API WinRT.

Дополнительные сведения доступны в следующих статьях.

* [Упаковка классических приложений](/windows/uwp/porting/desktop-to-uwp-root)
* [Принцип работы упакованного классического приложения](/windows/uwp/porting/desktop-to-uwp-behind-the-scenes)

### <a name="xaml-islands"></a>XAML Islands

Начиная с Windows 10 версии 1903, вы можете размещать элементы управления универсальной платформы Windows (UWP) в классических приложениях, созданных не на платформе UWP, используя функцию *XAML Island*. Эта функция позволяет улучшить внешний вид и функциональность существующих классических приложений, дополнив их новейшими возможностями пользовательского интерфейса Windows 10, которые доступны только через элементы управления UWP. Это позволяет применять в имеющихся приложениях WPF, Windows Forms и Win32 на C++ такие функции UWP, как Windows Ink, и элементы управления, которые поддерживают систему Fluent Design.

Дополнительные сведения см. в статье [Элементы управления UWP в классических приложениях (XAML Islands)](/windows/uwp/xaml-platform/xaml-host-controls). В этом руководстве описаны процессы применения двух разных типов элементов управления XAML Islands.

* Это [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) и [MapControl](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) из набора инструментов сообщества Windows. Эти элементы управления WPF служат оболочкой для интерфейса и функциональных возможностей соответствующих элементов управления UWP, и их можно использовать в конструкторе Visual Studio, как любой другой элемент управления WPF.

* Элемент управления UWP [Представление календаря](/windows/uwp/design/controls-and-patterns/calendar-view). Это стандартный элемент управления UWP, для размещения которого применяется [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) из набора инструментов сообщества Windows.

### <a name="net-core-3"></a>.NET Core 3

Платформа с открытым исходным кодом [.NET Core](https://docs.microsoft.com/dotnet/core/) реализует кроссплатформенную, простую и расширяемую версию полной .NET Framework. По сравнению с полной платформой .NET Framework, .NET Core запускается намного быстрее и в ней оптимизированы многие из интерфейсов API.

В первых нескольких выпусках основное внимание в .NET Core уделялось поддержке веб-приложений и (или) приложений серверной части. С помощью .NET Core вы можете легко создавать масштабируемые веб-приложения или API, которые могут размещаться на Windows, Linux или в архитектурах микрослужб, таких как контейнеры Docker.

Последний выпуск .NET Core — .NET Core 3. Основным преимуществом этого выпуска является поддержка классических приложений для Windows, включая приложения Windows Forms и WPF. Используя .NET Core 3, вы можете запускать новые и существующие классические приложения для Windows и пользоваться всеми преимуществами этой платформы. В приложениях для Windows Forms и WPF, предназначенных для .NET Core 3, вы сможете использовать элементы управления UWP, доступные в [XAML Islands](xaml-islands.md).

> [!NOTE]
> WPF и Windows Forms не будут кроссплатформенными, то есть вы не сможете запускать WPF или Windows Forms в Linux или MacOS. Компоненты пользовательского интерфейса WPF и Windows Forms по-прежнему зависят от системы визуализации Windows.

Дополнительные сведения см. в статье [Новые возможности .NET Core 3.0](https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0).
