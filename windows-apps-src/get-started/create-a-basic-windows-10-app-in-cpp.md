---
ms.assetid: DC235C16-8DAF-4078-9365-6612A10F3EC3
title: Создание приложения Hello World на C++/CX (Windows 10)
description: В Microsoft Visual Studio 2019 вы можете использовать язык C++/CX для разработки приложения, которое работает в Windows 10, в том числе на телефонах под управлением Windows 10. Пользовательский интерфейс в этих приложениях определен на языке XAML.
ms.date: 06/11/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4c388b9b81744c0d27d96c1f97b4e405af63eaef
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "80524087"
---
# <a name="create-a-hello-world-app-in-ccx"></a>Создание приложения Hello world на C++/CX

> [!IMPORTANT]
> В этом руководстве используется язык C++/CX. Корпорация Майкрософт выпустила C++/WinRT. Это полностью соответствующая стандартам современная проекция языка C++17 для интерфейсов API среды выполнения Windows (WinRT). Дополнительные сведения об этом языке см. в разделе [C++/WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/).

В Microsoft Visual Studio вы можете использовать C++/CX для разработки приложения для Windows 10 с пользовательским интерфейсом, определенным на языке XAML.

> [!NOTE]
> В этом руководстве используется Visual Studio Community 2019. Если вы используете другую версию Visual Studio, она может выглядеть иначе.

## <a name="before-you-start"></a>Прежде чем начать

-   Для работы с этим руководством необходимо использовать Visual Studio Community (или более позднюю версию) либо одну из других версий Visual Studio на компьютере с Windows 10. Для скачивания перейдите на страницу [Получение инструментов](https://visualstudio.microsoft.com/downloads/).
-   Предполагается, что вы обладаете общими знаниями о C++/CX, XAML и вам известны основные понятия из [обзора XAML](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-overview).
-   Также предполагается, что вы используете в Visual Studio макет окна по умолчанию. Чтобы вернуться к макету по умолчанию, выберите в строке меню пункт **Окно** > **Сброс макета окон**.

## <a name="comparing-c-desktop-apps-to-windows-apps"></a>Сравнение классических приложений на C++ с приложениями для Windows

Если у вас есть опыт создания классических приложений для Windows на C++, то некоторые аспекты создания приложений для UWP вам, вероятно, покажутся хорошо знакомыми, а другие потребуют изучения.

### <a name="whats-the-same"></a>Сходство с традиционным программированием на C++

-   Вы можете использовать STL, CRT (за некоторыми исключениями) и любую другую библиотеку C++, если код вызывает только функции Windows, которые доступны из среды выполнения Windows.

-   Если вы привыкли работать с визуальными конструкторами, можно использовать конструктор, встроенный в Microsoft Visual Studio, или использовать полнофункциональное приложение Blend для Visual Studio. Если вы обычно пишете код для пользовательского интерфейса вручную, вы точно так же можете писать код XAML.

-   Создаваемые приложения могут использовать типы операционной системы Windows и ваши собственные настраиваемые типы.

-   Вы по-прежнему можете использовать отладчик, профилировщик и другие инструменты разработки Visual Studio.

-   Для компиляции созданных вами приложений в машинный код так же используется компилятор Visual C++. Выполнение приложений UWP на C++/CX не поддерживается в управляемой среде выполнения.

### <a name="whats-new"></a>Новые возможности

-   Принципы разработки приложений UWP и универсальных приложений для Windows существенно отличаются от принципов разработки классических приложений. Утрачивают значение границы окон, подписи, диалоговые окна и прочее. Наибольшее значение приобретает содержимое. При создании универсальных приложений для Windows этим принципам необходимо следовать с самого начала стадии планирования.

-   Для определения всего пользовательского интерфейса используется XAML. Разделение между пользовательским интерфейсом и основной логикой в универсальном приложении для Windows намного более очевидно, чем в приложениях MFC или Win32. В то время как вы работаете над поведением приложения в файле кода, другие специалисты могут работать над внешним видом пользовательского интерфейса в файле XAML.

-   Программирование выполняется главным образом с помощью нового объектно-ориентированного API среды выполнения Windows с улучшенной навигацией, хотя на устройствах Windows для ряда функций можно по-прежнему использовать Win32.

-   С помощью C++/CX можно использовать и создавать объекты среды выполнения Windows. C++/CX разрешает обработку исключений C++, делегаты, события и автоматический подсчет ссылок для динамически создаваемых объектов. При использовании C++/CX особенности базовой архитектуры COM и Windows скрыты от кода приложения. Подробнее: [Справочник по языку C++/CX](https://docs.microsoft.com/cpp/cppcx/visual-c-language-reference-c-cx).

-   Приложение компилируется в пакет, также включающий метаданные о типах, которые содержатся в приложении, используемых ресурсах и требуемых возможностях (доступ к файловой системе, Интернету, камере и т. д.).

-   В Microsoft Store и Windows Phone Store приложение проходит сертификацию для подтверждения его надежности, после чего оно становится доступным для миллионов потенциальных клиентов.

## <a name="hello-world-store-app-in-ccx"></a>Приложение Hello World на C++/CX для Store

Наше первое приложение Hello World демонстрирует некоторые основные возможности взаимодействия, стилей и макета. Мы создадим приложение, используя шаблон проекта универсального приложения для Windows. Если вы разрабатывали приложения для Windows 8.1 и Windows Phone 8.1 ранее, возможно, вы помните, что вам нужны были три проекта в Visual Studio: один для приложения для Windows, один для приложения для телефона и еще один с общим кодом. Благодаря универсальной платформе Windows (UWP) в Windows 10 можно создать всего один проект, который работает на всех устройствах, в том числе на компьютерах и ноутбуках под управлением Windows 10, таких устройствах, как планшеты, мобильные телефоны, устройства виртуальной реальности и т. д.

Начнем с основ.

-   Создание проекта универсального приложения для Windows в Visual Studio.

-   Общее представление о проектах и создаваемых файлах.

-   Общее представление о расширениях в расширениях компонентов Visual C++ (C++/CX) и способах их использования

**Сначала создайте решение в Visual Studio**

1.  В Visual Studio в строке меню последовательно выберите **Файл** > **Создать** > **Проект**.

2.  В диалоговом окне **Создание проекта** выберите **Пустое приложение (универсальное приложение для Windows — C++/CX)** .  Если этот параметр не отображается, убедитесь, что вы установили средства разработки универсальных приложений для Windows. Дополнительные сведения см. в статье [Подготовка](get-set-up.md).

![Шаблоны проектов C++/CX в диалоговом окне "Создание проекта" ](images/vs2019-uwp-01.png)

3.  Нажмите кнопку **Далее** и введите имя для проекта. Он будет называться HelloWorld.

4.  Нажмите кнопку **Создать**.

> [!NOTE]
> Если вы используете Visual Studio впервые, может открыться диалоговое окно с запросом включить параметр **Режим разработчика**. Режим разработчика — это специальный параметр, включающий определенные функции, например разрешение на непосредственный запуск приложений, а не только через Store. Дополнительные сведения см. в разделе [Подготовка устройства к разработке](enable-your-device-for-development.md). Чтобы продолжить работу с этим руководством, выберите **Режим разработчика**, нажмите **Да** и закройте диалоговое окно.

   Файлы вашего проекта созданы.

Перед тем как продолжить работу, давайте изучим файлы решения.

![Решение для универсального приложения со свернутыми узлами](images/vs2017-uwp-02.png)

### <a name="about-the-project-files"></a>Информация о файлах проекта

Каждый файл с расширением XAML в папке проекта имеет соответствующий файл .XAML.H и файл .XAML.CPP в той же папке, а также файл .G и файл .G.HPP в папке "Generated Files", которая находится на диске, но не является частью проекта. Следует изменить эти XAML-файлы, чтобы создать элементы пользовательского интерфейса и соединить их с источниками данных (привязка данных). Следует изменить файлы .H и .CPP, чтобы добавить настраиваемую логику для обработчиков событий. Автоматически создаваемые файлы представляют преобразование разметки XAML в C++/CX. Не изменяйте эти файлы, но вы можете изучить их, чтобы лучше понимать, как работает код программной части. В основном созданный файл содержит частичное определение класса для корневого элемента XAML. Это тот же самый класс, который вы изменяете в \*XAML.H- и CPP-файлах. Созданные файлы объявляют дочерние элементы пользовательского интерфейса XAML как члены класса, чтобы вы могли ссылаться на них в своем коде. Во время выполнения сборки созданный код и ваш код объединяются в полное определение класса, а затем компилируются.

Давайте сначала посмотрим на файлы проекта.

-   **App.xaml, App.xaml.h, App.xaml.cpp:** представляют объект приложения, который является точкой входа приложения. App.xaml не содержит разметки пользовательского интерфейса, относящейся к отдельным страницам, но вы можете добавить стили и другие элементы пользовательского интерфейса, которые следует сделать доступными с любой страницы. Файлы кода программной части содержат обработчики для событий **OnLaunched** и **OnSuspending**. Обычно сюда добавляется пользовательский код для инициализации приложения при запуске, а также здесь выполняется очистка при приостановке и завершении работы.
-   **MainPage.xaml, MainPage.xaml.h, MainPage.xaml.cpp** — содержат разметку XAML и код программной части для стандартной начальной страницы в приложении. Эта страница не поддерживает навигацию и не имеет встроенных элементов управления.
-   **pch.h, pch.cpp:** предварительно скомпилированный файл заголовка и файл, который включает его в ваш проект. В файле pch.h вы можете добавлять любые заголовки, которые не изменяются часто и которые добавлены в другие файлы в решении.
-   **Package.appxmanifest:** XML-файл, описывающий возможности устройства, которые необходимы вашему приложению. Также содержит информацию о версии приложения и другие метаданные. Чтобы открыть этот файл в **конструкторе манифеста**, просто дважды щелкните его.
-   **HelloWorld\_TemporaryKey.pfx:** ключ, разрешающий развертывание приложения на этом компьютере из Visual Studio.

## <a name="a-first-look-at-the-code"></a>Начало работы с программным кодом

Если вы изучите код в файлах App.xaml.h, App.xaml.cpp в общем проекте, то заметите, что в основном понимаете код C++. Тем не менее есть и некоторые незнакомые элементы синтаксиса, если вы ранее не сталкивались с приложениями для среды выполнения Windows, или если вы работали с C++/CLI. Вот общие нестандартные элементы синтаксиса, которые вы можете увидеть в C++/CX:

**Ссылочные классы**

Почти все классы среды выполнения Windows, которые включают в себя все типы в API Windows (элементы управления XAML, страницы в вашем приложении, сам класс App, все объекты устройств и сетевые объекты, все типы контейнеров), объявляются как **ref class**. (Несколько типов в Windows: **класс значения** или **структура** значения). Класс ref может использоваться из любого языка. В C++/CX время существования этих типов регулируется с помощью автоматического подсчета ссылок (а не сборки мусора), чтобы не приходилось явно удалять эти объекты. Вы также можете создавать свои собственные классы "ref".

```cpp
namespace HelloWorld
{
   /// <summary>
   /// An empty page that can be used on its own or navigated to within a Frame.
   /// </summary>
   public ref class MainPage sealed
   {
      public:
      MainPage();
   };
}
```    

Все типы среды выполнения Windows должны объявляться в пространстве имен и, в отличие от самих типов в C++ стандарта ISO, иметь модификатор доступа. Модификатор **public** делает класс видимым для компонентов среды выполнения Windows, которые находятся вне пространства имен. Ключевое слово **sealed** означает, что класс не может быть базовым классом. Почти все классы ref запечатаны. Наследование классов широко не используется, потому что JavaScript его не понимает.

**ref new** и **^ (крышка)**

Переменная класса ref объявляется с помощью оператора «^» («крышка»), и создается экземпляр объекта с помощью ключевого слова «ref new». После этого вы получаете доступ к методам экземпляров объекта, используя оператор "->" почти так же, как указатель C++. Доступ к статическим методам можно получить с помощью оператора "::", как в C++ стандарта ISO.

В следующем коде мы используем полное имя, чтобы создать экземпляр объекта, и оператор "->", чтобы вызвать метод экземпляров.

```cpp
Windows::UI::Xaml::Media::Imaging::BitmapImage^ bitmapImage =
     ref new Windows::UI::Xaml::Media::Imaging::BitmapImage();

bitmapImage->SetSource(fileStream);
```

Обычно в файле .CPP мы бы добавили директиву `using namespace  Windows::UI::Xaml::Media::Imaging` и ключевое слово "auto", чтобы тот же код выглядел так:

```cpp
auto bitmapImage = ref new BitmapImage();
bitmapImage->SetSource(fileStream);
```

**Свойства**

Класс "ref" может иметь свойства, которые, как и в управляемых языках, являются специальными функциями-членами. Они отображаются в виде полей в потребляющем коде.

```cpp
public ref class SaveStateEventArgs sealed
{
   public:
   // Declare the property
   property Windows::Foundation::Collections::IMap<Platform::String^, Platform::Object^>^ PageState
   {
      Windows::Foundation::Collections::IMap<Platform::String^, Platform::Object^>^ get();
   }
   ...
};

   ...
   // consume the property like a public field
   void PhotoPage::SaveState(Object^ sender, Common::SaveStateEventArgs^ e)
   {    
      if (mruToken != nullptr && !mruToken->IsEmpty())
   {
      e->PageState->Insert("mruToken", mruToken);
   }
}
```

**Делегаты**

Как и в управляемых языках, делегат — это ссылочный тип, который инкапсулирует функцию с помощью специальной подписи. Делегаты часто используются с событиями и обработчиками событий.

```cpp
// Delegate declaration (within namespace scope)
public delegate void LoadStateEventHandler(Platform::Object^ sender, LoadStateEventArgs^ e);

// Event declaration (class scope)
public ref class NavigationHelper sealed
{
   public:
   event LoadStateEventHandler^ LoadState;
};

// Create the event handler in consuming class
MainPage::MainPage()
{
   auto navigationHelper = ref new Common::NavigationHelper(this);
   navigationHelper->LoadState += ref new Common::LoadStateEventHandler(this, &MainPage::LoadState);
}
```

## <a name="adding-content-to-the-app"></a>Добавление содержимого в приложение

Давайте добавим содержимое в приложение.

**Шаг 1. Изменение начальной страницы**

1.  В **Обозревателе решений** откройте файл MainPage.xaml.
2.  Создайте элементы управления для пользовательского интерфейса, добавив следующий код XAML в корневой элемент [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) сразу после его закрывающего тега. В нем содержится [**StackPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) с [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock), который запрашивает имя пользователя; элемент [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox), который принимает имя пользователя; элемент [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) и еще один элемент **TextBlock**.

    ```xaml
    <StackPanel x:Name="contentPanel" Margin="120,30,0,0">
        <TextBlock HorizontalAlignment="Left" Text="Hello World" FontSize="36"/>
        <TextBlock Text="What's your name?"/>
        <StackPanel x:Name="inputPanel" Orientation="Horizontal" Margin="0,20,0,20">
            <TextBox x:Name="nameInput" Width="300" HorizontalAlignment="Left"/>
            <Button x:Name="inputButton" Content="Say &quot;Hello&quot;"/>
        </StackPanel>
        <TextBlock x:Name="greetingOutput"/>
    </StackPanel>
    ```

3.  На этот момент вы создали очень простое универсальное приложение для Windows. Если вы хотите увидеть, как выглядит приложение UWP, нажмите клавишу F5, чтобы выполнить сборку, развернуть и запустить приложение в режиме отладки.

Сначала появится экран-заставка по умолчанию. На нем будет присутствовать изображение (Assets\\SplashScreen.scale-100.png) и цвет фона, указанный в файле манифеста приложения. Подробнее о том, как настраивать экран-заставку, см. в разделе о [добавлении экрана-заставки](https://docs.microsoft.com/previous-versions/windows/apps/hh465332(v=win.10)).

После того как экран-заставка исчезнет, появится ваше приложение. Отображается основная страница приложения.

![Экран приложения UWP с элементами управления](images/xaml-hw-app2.png)

Пока это приложение мало что умеет, но вас можно поздравить с успешным созданием первого универсального приложения для Windows.

Чтобы завершить отладку и закрыть приложение, вернитесь в Visual Studio и нажмите клавиши SHIFT+F5.

Дополнительные сведения см. в статье [Выполнение приложения Магазина в Visual Studio](https://msdn.microsoft.com/library/windows/apps/xaml/Hh441477(v=VS.140).aspx).

В приложении можно вводить информацию в [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox), но при нажатии [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) ничего не происходит. Позднее мы создадим для кнопки обработчик события [**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click), который отображает персонализированное приветствие.

## <a name="step-2-create-an-event-handler"></a>Шаг 2. Создание обработчика событий

1.  В файле MainPage.xaml в коде XAML или в представлении конструирования выберите элемент Say Hello [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) в добавленном ранее элементе [**StackPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel).
2.  Нажмите клавишу F4, чтобы открыть окно **Свойства**, а затем нажмите кнопку ![Events](images/eventsbutton.png) (События).
3.  Найдите событие [**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click). В текстовом поле введите имя функции, которая обрабатывает событие **Click**. Для этого примера введите "Button\_Click".

    ![Окно “Свойства”, представление “События”](images/xaml-hw-event.png)

4.  Нажмите клавишу ВВОД. Метод обработчика событий создается в файле MainPage.xaml.cpp и открывается в редакторе кода. Вы можете добавить код, который выполняется при возникновении события.

   В то же время в MainPage.xaml код XAML для [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) обновляется, чтобы объявить обработчик событий [**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) следующим образом.

    ```xaml
    <Button Content="Say &quot;Hello&quot;" Click="Button_Click"/>
    ```

   Вы также можете просто добавить это значение к коду xaml вручную. Это может быть удобно, если не загружается конструктор. При вводе вручную введите "Щелкнуть" и позвольте IntelliSense отобразить параметр, чтобы добавить новый обработчик событий. Таким образом, Visual Studio создает необходимую декларацию о методе и заглушку.

   Конструктор не сможет загрузиться, если во время отрисовки возникнет необработанное исключение. Отрисовка в конструкторе предусматривает запуск версии времени разработки страницы. Это может быть удобно для отключения запущенного кода пользователя. Это можно сделать, изменив значение параметра в диалоговом окне **"Сервис ", "Параметры"** . В разделе **Конструктор XAML** снимите флажок **Запускать код проекта в конструкторе XAML (если поддерживается)** .

5.  В файле MainPage.xaml.cpp добавьте следующий код в обработчик событий **Button\_Click**, который вы только что создали. В этом коде вы получаете имя пользователя из элемента управления `nameInput` [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) и используете его, чтобы создать приветствие. `greetingOutput` [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) отображает результат.

    ```cpp
    void HelloWorld::MainPage::Button_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e)
    {
        greetingOutput->Text = "Hello, " + nameInput->Text + "!";
    }
    ```

6.  Назначьте проект запускаемым, а затем нажмите клавишу F5, чтобы выполнить сборку и запустить приложение. При вводе имени в текстовое поле и нажатии кнопки приложение отображает персонализированное приветствие.

![Экран приложения с отображаемым сообщением](images/xaml-hw-app4.png)

## <a name="step-3-style-the-start-page"></a>Шаг 3. Стиль начальной страницы

### <a name="choosing-a-theme"></a>Выбор темы

Внешний вид и функциональность приложения можно легко изменить. По умолчанию ваше приложение использует ресурсы со светлым стилем. Системные ресурсы также включают светлую тему. Примените ее и посмотрите, как будет выглядеть приложение.

**Переключение на темную тему**

1.  Откройте файл App.xaml.
2.  В открывающем теге [**Application**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Application) измените свойство [**RequestedTheme**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.requestedtheme) и установите для него значение **Dark**:

    ```xaml
    RequestedTheme="Dark"
    ```

    Вот полный тег [**Application**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Application) с темной темой:

    ```xaml
        <Application
        x:Class="HelloWorld.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:HelloWorld"
        RequestedTheme="Dark">
    ```

3.  Чтобы выполнить сборку и запустить приложение, нажмите клавишу F5. Обратите внимание, что проект использует темную тему.

![Экран приложения с темной темой](images/xaml-hw-app3.png)

Какую тему использовать? Любую, какую пожелаете! Мы рекомендуем для приложений, которые главным образом отображают изображения или видео, лучше использовать темную тему, а для приложений с большим объемом текста — светлую тему. Если применяется пользовательская цветовая схема, используйте тему, которая лучше всего сочетается с внешним видом вашего приложения. В остальных разделах настоящего руководства мы используем светлую тему на снимках экрана.

**Примечание.**    Тема применяется после запуска приложения. Ее нельзя изменить, пока приложение работает.

### <a name="using-system-styles"></a>Использование системных стилей

В данный момент в приложении для Windows текст является слишком мелким и трудным для чтения. Давайте исправим это, применив системный стиль.

**Изменение стиля элемента**

1.  Откройте файл MainPage.xaml в проекте Windows.
2.  В коде XAML или в представлении конструирования выберите добавленный ранее элемент What's your name? [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock).
3.  В окне **Свойства** (**F4**) нажмите кнопку «Свойства» (![Properties button](images/propertiesbutton.png)) в правом верхнем углу.
4.  Разверните группу **Текст** и установите размер шрифта 18 пикселей.
5.  Разверните группу **Разное** и найдите свойство **Style**.
6.  Щелкните маркер свойств (зеленый прямоугольник справа от свойства **Стиль**), а затем в меню выберите пункты **Системный ресурс** > **BaseTextBlockStyle**.

     **BaseTextBlockStyle** — это ресурс, который определен в [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) в файле <root>\\Program Files\\Windows Kits\\10\\Include\\winrt\\xaml\\design\\generic.xaml.

    ![Окно “Свойства”, представление “Свойства”](images/xaml-hw-style-cpp.png)

     Вид текста в рабочей области конструирования XAML изменится. В редакторе XAML обновляется код XAML для [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock).

    ```xaml
    <TextBlock Text="What's your name?" Style="{ThemeResource BaseTextBlockStyle}"/>
    ```

7.  Повторите процесс, чтобы установить размер шрифта, и назначьте значение **BaseTextBlockStyle** элементу `greetingOutput`[**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock).

    **Совет.**    Хотя в этом элементе [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock), нет текста, при наведении указателя мыши на поверхность разработки XAML голубой контур показывает его расположение, чтобы его можно было выбрать.  

    XAML-код теперь выглядит так:

    ```xaml
    <StackPanel x:Name="contentPanel" Margin="120,30,0,0">
        <TextBlock Style="{ThemeResource BaseTextBlockStyle}" FontSize="18" Text="What's your name?"/>
        <StackPanel x:Name="inputPanel" Orientation="Horizontal" Margin="0,20,0,20">
            <TextBox x:Name="nameInput" Width="300" HorizontalAlignment="Left"/>
            <Button x:Name="inputButton" Content="Say &quot;Hello&quot;" Click="Button_Click"/>
        </StackPanel>
        <TextBlock Style="{ThemeResource BaseTextBlockStyle}" FontSize="18" x:Name="greetingOutput"/>
    </StackPanel>
    ```

8.  Чтобы выполнить сборку и запустить приложение, нажмите клавишу F5. Теперь оно выглядит следующим образом:

![Экран приложения с более крупным текстом](images/xaml-hw-app5.png)

### <a name="step-4-adapt-the-ui-to-different-window-sizes"></a>Шаг 4. Адаптация пользовательского интерфейса к различным размерам окна

Теперь мы адаптируем пользовательский интерфейс к различным размерам экрана, чтобы приложение выглядело хорошо на мобильных устройствах. Для этого добавьте [**VisualStateManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualStateManager) и установите свойства, применяемые для различных визуальных состояний.

**Настройка макета пользовательского интерфейса**

1.  В редакторе XAML добавьте этот блок XAML после открывающего тега корневого элемента [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid).

    ```xaml
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState x:Name="wideState">
                <VisualState.StateTriggers>
                    <AdaptiveTrigger MinWindowWidth="641" />
                </VisualState.StateTriggers>
            </VisualState>
            <VisualState x:Name="narrowState">
                <VisualState.StateTriggers>
                    <AdaptiveTrigger MinWindowWidth="0" />
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="contentPanel.Margin" Value="20,30,0,0"/>
                    <Setter Target="inputPanel.Orientation" Value="Vertical"/>
                    <Setter Target="inputButton.Margin" Value="0,4,0,0"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
    ```

2.  Выполните отладку приложения на локальном компьютере. Обратите внимание, что пользовательский интерфейс выглядит так же, как и раньше, пока ширина окна не станет меньше 641 аппаратно-независимого пикселя (DIP).
3.  Выполните отладку приложения на эмуляторе мобильного устройства. Обратите внимание, что пользовательский интерфейс использует свойства, указанные вами в `narrowState`, и правильно отображается на маленьком экране.

![Экран мобильного приложения с текстом после применения стиля](images/hw10-screen2-mob.png)

Если вы использовали [**VisualStateManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualStateManager) в предыдущих версиях XAML, можно заметить, что в XAML здесь используется упрощенный синтаксис.

У [**VisualState**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState) с именем `wideState` имеется [**AdaptiveTrigger**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.AdaptiveTrigger), для свойства [**MinWindowWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth) которого задано значение 641. Это означает, что состояние будет применяться только тогда, когда ширина окна составляет не менее минимального значения 641 DIP. Вы не указываете никакие объекты [**Setter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Setter) для этого состояния, поэтому оно использует свойства макета, заданные в XAML для содержимого страницы.

У второго [**VisualState**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState), `narrowState`, имеется [**AdaptiveTrigger**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.AdaptiveTrigger), для свойства [**MinWindowWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth) которого задано значение 0. Это состояние применяется, когда ширина окна больше 0, но меньше 641 DIP. (Для 641 DIP-пикселя применяется `wideState`.) В этом состоянии необходимо указать некоторые объекты [**Setter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Setter), чтобы изменить свойства макета элементов управления в пользовательском интерфейсе.

-   Вы уменьшаете левое поле элемента `contentPanel` со 120 до 20.
-   Вы изменяете [**Orientation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.stackpanel.orientation) для элемента `inputPanel` с **Horizontal** на **Vertical**.
-   Вы добавляете верхнее поле высотой 4 DIP к элементу `inputButton`.

### <a name="summary"></a>Сводка

Поздравляем, вы завершили изучение первого учебника. Вы научились добавлять содержимое в универсальные приложения для Windows, менять их внешний вид и реализовывать возможности взаимодействия.

## <a name="next-steps"></a>Дальнейшие действия

Если у вас есть проект универсального приложения для Windows, ориентированный на Windows 8.1 и (или) Windows Phone 8.1, то вы можете перенести его в Windows 10. Для этого действия нет автоматического процесса, но его можно выполнить вручную. Начните новый проект универсального приложения для Windows, чтобы получить последнюю структуру системы и файлы манифеста проекта, скопируйте файлы кода в структуру каталогов проекта, добавьте элементы в проект и перепишите код XAML, используя [**VisualStateManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualStateManager) в соответствии с рекомендациями в этом разделе. Дополнительные сведения см. в разделах: [Перенос проекта среды выполнения Windows 8 в проект универсальной платформы Windows (UWP)](https://docs.microsoft.com/windows/uwp/porting/w8x-to-uwp-porting-to-a-uwp-project) и [Перенос на универсальную платформу Windows (C++)](https://msdn.microsoft.com/library/mt186164.aspx).

Если у вас есть код на C++, который вы хотите включить в приложение UWP, например для создания нового пользовательского интерфейса UWP для существующего приложения, изучите раздел [Практическое руководство. Использование существующего кода C++ в приложении универсальной платформы Windows](https://msdn.microsoft.com/library/mt186162.aspx).
