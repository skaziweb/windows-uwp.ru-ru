---
title: Разработка собственных приложений Android в Windows
description: Приступая к разработке собственных приложений Android в Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Android, Windows, Android Studio, Visual Studio, c++ Android, защитник Windows, эмулятор, виртуальное устройство, установка, Java, Котлин
ms.date: 04/28/2020
ms.openlocfilehash: 31cdd5124c659b5d35a7e8192db1e87821b891f4
ms.sourcegitcommit: 24b19e7ee06e5bb11a0dae334806741212490ee9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "82255228"
---
# <a name="get-started-with-native-android-development-on-windows"></a>Начало работы с собственными разработками Android в Windows

Это руководство поможет вам приступить к работе с Windows для создания собственных приложений Android. Если вы предпочитаете межплатформенное решение, см. Краткие сведения о некоторых вариантах в разделе [Обзор разработки Android в Windows](./overview.md) .

Самым прямым способом создания собственного приложения Android является использование Android Studio с [Java или Котлин](#java-or-kotlin), хотя можно [использовать C или C++ для разработки Android](#use-c-or-c-for-android-game-development) , если у вас есть конкретная цель. Средства Android Studio SDK компилируют файлы кода, данных и ресурсов в архивный пакет Android с расширением apk. Один файл APK содержит все содержимое приложения Android, а — файл, с помощью которого устройства на платформе Android используют для установки приложения.

## <a name="install-android-studio"></a>Установка Android Studio

Android Studio является официальной интегрированной средой разработки для операционной системы Android под управлением Google. [Скачайте последнюю версию Android Studio для Windows](https://developer.android.com/studio).

- Если вы скачали EXE-файл (рекомендуется), дважды щелкните его, чтобы запустить.
- Если вы загрузили ZIP-файл, распакуйте его, скопируйте папку Android-Studio в папку Program Files, а затем откройте пакет Android-Studio > bin и запустите studio64. exe (для 64-разрядных компьютеров) или Studio. exe (для 32-разрядных компьютеров).

Следуйте указаниям мастера установки в Android Studio и установите все рекомендуемые пакеты SDK. По мере того как становятся доступными новые средства и другие API-интерфейсы, Android Studio уведомляет вас о наличии всплывающего окна или проверку наличия обновлений, выбрав пункт **Справка** > **Проверка обновления**.

## <a name="create-a-new-project"></a>Создание нового проекта

Выберите **файл** > **создать** > **Новый проект**.

В окне **Выбор проекта** вы сможете выбрать один из следующих шаблонов:

- **Основные действия**: создает простое приложение с панелью приложений, плавающей кнопкой действия и двумя файлами макета: один для действия и один для разделения текстового содержимого.

- **Пустое действие**: создает пустое действие и один файл макета с примером текстового содержимого.

- **Действие "Нижняя область навигации**": создает стандартную нижнюю панель навигации для действия. Ознакомьтесь с [нижним компонентом навигации](https://material.io/guidelines/components/bottom-navigation.html) в руководстве по проектированию материалов Google.

- Дополнительные сведения о [выборе шаблона действия](https://developer.android.com/studio/projects/templates#SelectTemplate) в документах Android Studio.

Шаблоны обычно используются для добавления действий в новые и существующие модули приложений. Например, чтобы создать экран входа для пользователей приложения, добавьте действие с [шаблоном действия входа](https://developer.android.com/studio/projects/templates#LoginActivity).

> [!NOTE]
> Операционная система Android основана на идее **компонентов** и использует термины **действие** и **назначение** для определения взаимодействий. **Действие** представляет отдельную задачу, которая может быть сделана пользователем. **Действие** предоставляет окно для создания пользовательского интерфейса с помощью классов, основанных на классе **представления** . Существует жизненный цикл **действий** в операционной системе Android, определяемый набором шести обратных `onCreate()`вызовов:, `onStart()`, `onResume()`, `onPause()` `onStop()`, и. `onDestroy()` Компоненты действия взаимодействуют друг с другом с помощью объектов **намерения** . Намерение определяет, какое действие следует запустить, или описывает тип выполняемого действия (и система выбирает подходящее действие, которое может быть даже из другого приложения). Дополнительные сведения о [действиях](https://developer.android.com/reference/android/app/Activity), [жизненном цикле действий](https://developer.android.com/guide/components/activities/activity-lifecycle) [и способах их работы](https://developer.android.com/reference/android/content/Intent.html) см. в документах Android.

### <a name="java-or-kotlin"></a>Java или Котлин

**Java** стал языком в 1991, разработанным корпорацией Sun Microsystems, но который теперь принадлежит Oracle. Она стала одним из самых популярных и мощных языков программирования с одним из самых крупных сообществ поддержки в мире. Язык Java основан на классах и объектно-ориентированной среде, предназначенный для того, чтобы иметь как можно меньше зависимостей реализации. Синтаксис похож на C и C++, но он имеет меньше низкоуровневых средств, чем любой из них.

**Котлин** был впервые объявлен как новый язык с открытым кодом JetBrains в 2011 и был включен в качестве альтернативы Java в Android Studio с 2017. В 2019 мая, Google объявил о Котлин в качестве предпочтительного языка для разработчиков приложений Android, поэтому, несмотря на более новый язык, он также имеет сообщество поддержки и был идентифицирован как один из самых быстрых растущех языков программирования. Котлин является межплатформенным, статически типизированным и предназначен для полноценного взаимодействия с Java.

Java более широко используется для более широкого спектра приложений и предлагает некоторые функции, которые не Котлин, такие как проверенные исключения, простые типы, не являющиеся классами, статические элементы, незакрытые поля, подстановочные знаки и операторы ternary. Котлин специально разработана для и рекомендуется для Android. Он также предлагает некоторые функции, которые не поддерживаются в Java, такие как ссылки null, контролируемые системой типов, необработанные типы, инвариантные массивы, правильные типы функций (в отличие от SAM-преобразований Java), Использование вариативности сайта без подстановочных знаков, смарт-приведение и многое другое. В документации по Котлин [более подробно рассматривается сравнение с Java](https://kotlinlang.org/docs/reference/comparison-to-java.html).

### <a name="minimum-api-level"></a>Минимальный уровень API

Вам потребуется выбрать минимальный уровень API для приложения. Это определяет версию Android, которую будет поддерживать ваше приложение. Более низкие уровни API устарели и, следовательно, поддерживают больше устройств, но более высокие уровни API более новые и таким образом предоставляют больше возможностей.

![Экран выбора минимального Android Studio API](../images/android-minimum-api-selection.png)

Выберите ссылку " **помогите мне выбрать** ", чтобы открыть диаграмму сравнения, показывающую распределение поддержки устройств и основные функции, связанные с выпуском версии платформы.

![Экран Android Studio минимального сравнения API](../images/android-minimum-api-selection-2.png)

### <a name="instant-app-support-and-androidx-artifacts"></a>Мгновенная поддержка приложений и артефакты Андроидкс

Можно заметить, что флажок для **поддержки мгновенных приложений** и другой для **использования артефактов андроидкс** в параметрах создания проекта. *Поддержка мгновенных приложений* не проверяется, и *андроидкс* проверяется как рекомендуемое по умолчанию.

Google Play **мгновенные приложения** предоставляют пользователям возможность опробовать приложение или игру, не устанавливая их первыми. Эти мгновенные приложения могут быть распределены по Магазин Google Play, поиску Google, социальным сетям и в любом месте, где вы делитесь ссылкой. Установив флажок **поддерживать мгновенные приложения** , вы запрашиваете Android Studio включить в проект пакет SDK для мгновенной разработки Google Play. Дополнительные сведения о [Google Play мгновенных приложениях](https://developer.android.com/topic/google-play-instant) и о [создании пакета приложений с поддержкой мгновенного включения](https://developer.android.com/topic/google-play-instant/getting-started/instant-enabled-app-bundle)см. в документации по Android.

**Артефакты андроидкс** представляют новую версию библиотеки поддержки Android и обеспечивает обратную совместимость между выпусками Android. Андроидкс предоставляет соответствующее пространство имен, начиная со строки андроидкс для всех доступных пакетов.

> [!NOTE]
> Андроидкс теперь является библиотекой по умолчанию. Чтобы снять этот флажок и использовать предыдущую библиотеку поддержки, необходимо удалить последний пакет SDK для Android Q. Инструкции см. в разделе [использование артефактов андроидкс](https://stackoverflow.com/questions/56580980/uncheck-use-androidx-artifacts) в StackOverflow для получения инструкций, но сначала обратите внимание, что самые старые пакеты библиотек поддержки были сопоставлены с соответствующими пакетами андроидкс. *. Полное сопоставление всех старых классов и создание артефактов для новых версий см. в разделе [Миграция в андроидкс](https://developer.android.com/jetpack/androidx/migrate).

## <a name="project-files"></a>Файлы проекта

Окно **проекта** Android Studio содержит следующие файлы (убедитесь, что в раскрывающемся меню выбрано представление Android):

**приложение > Java > com. example. мифирстапп > MainActivity**

Основное действие и точка входа для приложения. При сборке и запуске приложения система запускает экземпляр этого действия и загружает его макет.

**макет > > приложений > activity_main. XML**

XML-файл, определяющий макет пользовательского интерфейса действия. Он содержит элемент TextView с текстом "Hello World"

**манифесты > приложений > AndroidManifest. XML**

Файл манифеста, описывающий фундаментальные характеристики приложения и всех его компонентов.

**Gradle скрипты > сборка. Gradle**

Существует два файла с таким именем: "проект: мое первое приложение", для всего проекта и "модуль: приложение" для каждого модуля приложения. В новом проекте изначально будет только один модуль. Используйте файл build. File модуля, чтобы управлять тем, как подключаемый модуль Gradle создает приложение. Дополнительные сведения см. в документации по Android, настройте статью о [сборке](https://developer.android.com/studio/build/index) .

## <a name="use-c-or-c-for-android-game-development"></a>Использование C или C++ для разработки игр Android

Операционная система Android разработана для поддержки приложений, написанных на Java или Котлин, с помощью инструментов, внедренных в архитектуру системы. Многие системные функции, такие как интерфейс Android и обработка намерений, доступны только через интерфейсы Java. Существует несколько экземпляров, в которых может потребоваться **использовать код C или C++ через пакет Android Native Development Kit (NDK)** , несмотря на некоторые связанные с ним проблемы. Примером является разработка игр, так как игры обычно используют собственную логику отрисовки, написанную на OpenGL или вулкан, и преимущества обширных библиотек C, ориентированных на разработку игр. Использование C или C++ *может* также помочь в сжатии дополнительной производительности устройства для достижения низкой задержки или выполнения ресурсоемких вычислительных приложений, таких как физические модели. Однако в **большинстве новичков программистов Android не подходит** NDK. Если у вас нет конкретной цели для использования NDK, мы рекомендуем придерживаться Java, Котлин или одной из [межплатформенных платформ](./overview.md).

Чтобы создать новый проект с поддержкой C/C++, выполните следующие действия.

- В разделе **Выбор проекта** мастера Android Studio выберите тип проекта " *машинный код C++**". Нажмите кнопку **Далее**, заполните оставшиеся поля, а затем нажмите кнопку **Далее** еще раз.

- В разделе **Настройка поддержки c++** мастера можно настроить проект с помощью **стандартного поля C++** . Используйте раскрывающийся список, чтобы выбрать, какую стандартизацию C++ следует использовать. При выборе значения **цепочки инструментов по умолчанию** используется параметр CMAK по умолчанию. Нажмите кнопку **Готово**.

- После Android Studio создает новый проект, на панели **проекта** можно найти папку **cpp** , которая содержит собственные исходные файлы, заголовки, скрипты сборки для CMAK или NDK-Build, а также предварительно созданные библиотеки, которые являются частью проекта. Вы также можете найти образец исходного файла C++, `native-lib.cpp`в `src/main/cpp/` папке, предоставляющей простую `stringFromJNI()` функцию, возвращающую строку "Hello from C++". Кроме того, вы найдете сценарий сборки CMak, [`CMakeLists.txt`](https://developer.android.com/studio/projects/configure-cmake.html)в корневом каталоге вашего модуля, необходимый для создания собственной библиотеки.

Дополнительные сведения см. в разделе Документация по Android: [Добавление кода C и C++ в проект](https://developer.android.com/studio/projects/add-native-code). Примеры см. в разделе [примеры для Android NDK с репозиторием интеграции с Android Studio C++](https://github.com/android/ndk-samples) на сайте GitHub. Чтобы скомпилировать и запустить игру на C++ в Android, используйте [API Google Play Game Services](https://developers.google.com/games/services/cpp/gettingStartedAndroid).

## <a name="design-guidelines"></a>Рекомендации по проектированию

Пользователи устройств хотят, чтобы приложения выглядели и обвели себя определенным образом... будь то прокрутка или касание или использование элементов управления голоса, пользователи будут иметь определенные ожидания для того, как должно выглядеть приложение и как его использовать. Эти ожидания должны оставаться согласованными, чтобы сократить путаницу и недовольство. Android предлагает руководство по этим платформам и ожиданиям устройств, объединяющее конструкцию Google материала для визуальных элементов и шаблонов навигации, а также рекомендации по качеству совместимости, производительности и безопасности.

Дополнительные сведения см. в [документации по разработке для Android](https://developer.android.com/design).

### <a name="fluent-design-system-for-android"></a>Система разработки Fluent для Android

Кроме того, корпорация Майкрософт предлагает рекомендации по проектированию с целью обеспечения бесперебойной работы по всему портфелю мобильных приложений Майкрософт.

[Система разработки для Android](https://www.microsoft.com/design/fluent/#/android) — это разработка и создание собственных приложений Android, которые по-прежнему уникальны.

- [Набор инструментов для создания эскизов](https://aka.ms/fluenttoolkits/android/sketch)
- [Набор средств фигма](https://aka.ms/fluenttoolkits/android/figma)
- [Шрифт Android](https://fonts.google.com/specimen/Roboto)
- [Рекомендации по пользовательскому интерфейсу Android](https://developer.android.com/design/)
- [Рекомендации по значкам приложений Android](https://developer.android.com/guide/practices/ui_guidelines/icon_design)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Основы приложений для Android](https://developer.android.com/guide/components/fundamentals)

- [Разработка приложений с двумя экранами для Android и получение пакета SDK для устройства Surface Duo](https://docs.microsoft.com/dual-screen/android/)

- [Добавление исключений защитника Windows для повышения производительности](defender-settings.md)

- [Включение поддержки виртуализации для повышения производительности эмулятора](emulator.md#enable-virtualization-support)
