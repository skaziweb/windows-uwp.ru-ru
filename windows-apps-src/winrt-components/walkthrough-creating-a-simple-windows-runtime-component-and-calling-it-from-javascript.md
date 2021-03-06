---
title: Пошаговое руководство C# по созданию компонента Visual Basic среда выполнения Windows и его вызову из JavaScript
description: В этом пошаговом руководстве показано, как использовать .NET C# с Visual Basic или создавать собственные типы Среда выполнения Windows, упаковываться в среда выполнения Windows компонент, а также как вызывать компонент из приложения UWP, созданного для Windows с помощью JavaScript.
ms.assetid: 1565D86C-BF89-4EF3-81FE-35367DB8D671
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b7fc8e899b402dea21a11a0c8dce09646a84dae5
ms.sourcegitcommit: cc9f5a16386be78c12821a975e43497a0693abba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/18/2019
ms.locfileid: "72578162"
---
# <a name="walkthrough-of-creating-a-c-or-visual-basic-windows-runtime-component-and-calling-it-from-javascript"></a>Пошаговое руководство C# по созданию компонента Visual Basic среда выполнения Windows и его вызову из JavaScript

В этом пошаговом руководстве показано, как использовать .NET C# с Visual Basic или создавать собственные типы Среда выполнения Windows, упаковываться в среда выполнения Windows компонент и как затем вызывать этот компонент из приложения JavaScript универсальная платформа Windows (UWP).

Visual Studio позволяет легко создавать и развертывать собственные пользовательские типы среда выполнения Windows в проекте среда выполнения Windows Component (ВРК), написанном с помощью C# или Visual Basic, а затем ССЫЛАТЬСЯ на ВРК из проекта приложения JavaScript и использовать эти пользовательские типы из этого приложения.

На внутреннем уровне типы среда выполнения Windows могут использовать любые функции .NET, разрешенные в приложении UWP.

> [!NOTE]
> Дополнительные сведения см. в статьях [Среда выполнения Windows C# компонентов с и Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md) и [Обзор .NET для приложений UWP](/dotnet/api/index?view=dotnet-uwp-10.0).

Внешние члены типа могут предоставлять только типы среда выполнения Windows для их параметров и возвращаемых значений. При сборке решения Visual Studio создает проект .NET ВРК, а затем выполняет шаг сборки, который создает файл метаданных Windows (WinMD). Это компонент среды выполнения Windows, который Visual Studio включает в приложение.

> [!NOTE]
> .NET автоматически сопоставляет некоторые часто используемые типы .NET, такие как примитивные типы данных и типы коллекций, с их среда выполнения Windows эквивалентами. Эти типы .NET можно использовать в открытом интерфейсе среда выполнения Windows компонента, и они будут отображаться для пользователей компонента как соответствующие типы среда выполнения Windows. См. раздел [Среда выполнения Windows C# компонентов с помощью и Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md).

## <a name="prerequisites"></a>Необходимые условия

- Windows 10
- [Microsoft Visual Studio](https://visualstudio.microsoft.com/downloads/)

## <a name="creating-a-simple-windows-runtime-class"></a>Создание простого класса среды выполнения Windows

В этом разделе создается приложение JavaScript UWP и добавляется в решение Visual Basic или C# среда выполнения Windows проект компонента. В нем показано, как определить тип среда выполнения Windows, создать экземпляр типа из JavaScript и вызвать статический и члены экземпляра. Визуальное отображение примера приложения намеренно мало, чтобы сосредоточиться на компоненте.

1. В Visual Studio создайте новый проект JavaScript: в строке меню выберите **Файл, Создать, Проект**. В разделе **Установленные шаблоны** диалогового окна **Создать проект** выберите **JavaScript**, затем выберите **Windows**, после чего выберите **Универсальные**. (Если доступ к Windows ограничен, убедитесь, что используется Windows 8 или Windows более поздней версии). Выберите шаблон **Пустое приложение** и в качестве имени проекта введите SampleApp.
2.  Создайте проект компонента: в Обозревателе решений откройте контекстное меню решения SampleApp и выберите **Добавить**, а затем выберите **Создать проект**, чтобы добавить в решение новый проект C# или Visual Basic. В разделе **Установленные шаблоны** диалогового окна **Добавить новый проект** выберите **Visual Basic** или **Visual C#** , после чего выберите **Windows**, а затем выберите **Универсальные**. Выберите шаблон **Компонент среды выполнения Windows**, а в качестве имени проекта укажите **SampleComponent**.
3.  Измените имя класса на **Example**. Обратите внимание, что по умолчанию класс помечен модификатором **public sealed** (**Public NotInheritable** в Visual Basic). Все классы среды выполнения Windows, доступ к которым предоставляется из компонента, должны быть запечатанными.
4.  Добавьте в класс два простых члена — метод **static** (метод **Shared** в Visual Basic) и свойство экземпляра:

    > [!div class="tabbedCodeSnippets"]
    > ```csharp
    > namespace SampleComponent
    > {
    >     public sealed class Example
    >     {
    >         public static string GetAnswer()
    >         {
    >             return "The answer is 42.";
    >         }
    >
    >         public int SampleProperty { get; set; }
    >     }
    > }
    > ```
    > ```vb
    > Public NotInheritable Class Example
    >     Public Shared Function GetAnswer() As String
    >         Return "The answer is 42."
    >     End Function
    >
    >     Public Property SampleProperty As Integer
    > End Class
    > ```

5.  Необязательно: чтобы разрешить функции IntelliSense для добавляемых членов, в Обозревателе решений откройте контекстное меню проекта SampleComponent и выберите команду **Сборка**.
6.  В Обозревателе решений в проекте JavaScript откройте контекстное меню для элемента **Ссылки** и выберите **Добавить ссылку**, чтобы открыть **Диспетчер ссылок**. Выберите **Проекты**, а затем выберите **Решение**. Установите флажок для проекта SampleComponent и нажмите кнопку **ОК**, чтобы добавить ссылку.

## <a name="call-the-component-from-javascript"></a>Вызов компонента из JavaScript

Для использования типа среды выполнения Windows в коде JavaScript добавьте следующий код после анонимной функции в конце файла default.js (в папке js проекта), предоставляемого шаблоном Visual Studio. Этот код должен размещаться после обработчика событий app.oncheckpoint и перед вызовом app.start.

```javascript
var ex;

function basics1() {
   document.getElementById('output').innerHTML =
        SampleComponent.Example.getAnswer();

    ex = new SampleComponent.Example();

   document.getElementById('output').innerHTML += "<br/>" +
       ex.sampleProperty;

}

function basics2() {
    ex.sampleProperty += 1;
    document.getElementById('output').innerHTML += "<br/>" +
        ex.sampleProperty;
}
```

Обратите внимание, что регистр первой буквы имени каждого члена меняется с верхнего на нижний. Это преобразование является частью возможностей JavaScript, обеспечивающих естественное использование среды выполнения Windows. В именах пространств имен и классов используется нотация в стиле Pascal. Для имен членов применяется «верблюжий» стиль, кроме имен событий, которые полностью записываются в нижнем регистре. См. раздел [Использование среды выполнения Windows в JavaScript](/scripting/jswinrt/using-the-windows-runtime-in-javascript). Правила использования «верблюжьего» стиля могут показаться сложными. Последовательность начальных прописных букв обычно переводится в нижний регистр, но если за тремя прописными буквами следует строчная буква, в нижний регистр переводятся только первые две из них: например, имя члена IDStringKind заменяется на idStringKind. В Visual Studio можно выполнить сборку проекта компонента среды выполнения Windows, чтобы затем с помощью IntelliSense проверять правильность регистра букв в проекте JavaScript.

Аналогичным образом .NET обеспечивает поддержку естественного использования среда выполнения Windows в управляемом коде. Это рассматривается в последующих разделах этой статьи, а также в статьях [Среда выполнения Windows компонентов с C# и Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md) и [поддержкой .net для приложений UWP и среда выполнения Windows](/dotnet/standard/cross-platform/support-for-windows-store-apps-and-windows-runtime).

## <a name="create-a-simple-user-interface"></a>Создание простого пользовательского интерфейса

В проекте JavaScript откройте файл default.html и измените текст блока body, как показано в следующем фрагменте кода. Данный код включает полный набор элементов управления для примера приложения, а также задает имена функций для событий нажатия кнопок.

> **Обратите внимание** ,  при первом запуске приложения поддерживаются только кнопки Basics1 и Basics2.

```html
<body>
            <div id="buttons">
            <button id="button1" >Basics 1</button>
            <button id="button2" >Basics 2</button>

            <button id="runtimeButton1">Runtime 1</button>
            <button id="runtimeButton2">Runtime 2</button>

            <button id="returnsButton1">Returns 1</button>
            <button id="returnsButton2">Returns 2</button>

            <button id="events1Button">Events 1</button>

            <button id="btnAsync">Async</button>
            <button id="btnCancel" disabled="disabled">Cancel Async</button>
            <progress id="primeProg" value="25" max="100" style="color: yellow;"></progress>
        </div>
        <div id="output">
        </div>
</body>
```

В проекте JavaScript в папке css откройте файл default.css. Измените раздел body, как показано ниже, и добавьте стили, определяющие расположение кнопок и выходного текста.

```css
body
{
    -ms-grid-columns: 1fr;
    -ms-grid-rows: 1fr 14fr;
    display: -ms-grid;
}

#buttons {
    -ms-grid-rows: 1fr;
    -ms-grid-columns: auto;
    -ms-grid-row-align: start;
}
#output {
    -ms-grid-row: 2;
    -ms-grid-column: 1;
}
```

Теперь добавьте код регистрации прослушивателя событий, добавив предложение then в вызов processAll функции app.onactivated в файле default.js. Замените существующую строку кода, которая вызывает setPromise, и измените ее на следующий код:

```javascript
args.setPromise(WinJS.UI.processAll().then(function () {
    var button1 = document.getElementById("button1");
    button1.addEventListener("click", basics1, false);
    var button2 = document.getElementById("button2");
    button2.addEventListener("click", basics2, false);
}));
```

Этот способ добавления событий в элементы управления HTML удобнее непосредственного добавления обработчика событий нажатия кнопки в HTML. См. раздел [Создание приложения "Hello, World" (JS)](/windows/uwp/get-started/create-a-hello-world-app-js-uwp).

## <a name="build-and-run-the-app"></a>Сборка и запуск приложения

Перед выполнением сборки измените целевую платформу для всех проектов на ARM, x64 или x86 с учетом характеристик своего компьютера.

Чтобы собрать и запустить решение, нажмите клавишу F5. (Если появляется сообщение об ошибке времени выполнения, указывающее, что компонент SampleComponent не определен, это означает, что отсутствует ссылка на проект библиотеки классов).

Visual Studio сначала компилирует библиотеку классов, а затем выполняет задачу MSBuild, которая запускает [Winmdexp.exe (средство экспорта метаданных среды выполнения Windows)](/dotnet/framework/tools/winmdexp-exe-windows-runtime-metadata-export-tool) для создания компонента среды выполнения Windows. Компонент добавляется в файл WinMD, содержащий управляемый код и метаданные Windows, описывающие этот код. Программа WinMdExp.exe формирует сообщения об ошибках сборки, если код компонента среды выполнения Windows некорректен, и эти сообщения об ошибках отображаются в интегрированной среде разработки Visual Studio. Visual Studio добавит компонент в пакет приложения (Appx-файл) для приложения UWP и создаст соответствующий манифест.

Нажмите кнопку Basics 1, чтобы показать возвращаемое статическим методом GetAnswer значение в области вывода, создать экземпляр класса Example и отобразить в области вывода значение свойства SampleProperty этого экземпляра. Выходные данные приведены здесь:

``` syntax
"The answer is 42."
0
```

Нажмите кнопку Basics 2, чтобы увеличить значение свойства SampleProperty и показать новое значение в области вывода. Простые типы, такие как строки и числа, можно использовать в качестве типов параметров и возвращаемых значений, а также передавать между управляемым кодом и JavaScript. Поскольку в языке JavaScript числа хранятся в формате с плавающей запятой двойной точности, они преобразуются в числовые типы .NET Framework.

> **Примечание** .  по умолчанию можно устанавливать точки останова только в коде JavaScript. Сведения об отладке Visual Basic C# или кода см. в разделе Создание C# среда выполнения Windows компонентов в и Visual Basic.

Чтобы остановить отладку и закрыть приложение, перейдите из приложения в Visual Studio и нажмите сочетание клавиш Shift+F5.

## <a name="using-the-windows-runtime-from-javascript-and-managed-code"></a>Использование среды выполнения Windows в коде JavaScript и в управляемом коде

Среду выполнения Windows можно вызвать из кода JavaScript или из управляемого кода. Объекты среды выполнения Windows можно передавать между двумя видами кода, а события можно обрабатывать на каждой из сторон. Однако способы использования среда выполнения Windows типов в двух средах отличаются в некоторых деталях, так как JavaScript и .NET поддерживают среда выполнения Windows по-разному. В следующем примере эти различия показаны с использованием класса [Windows.Foundation.Collections.PropertySet](/uwp/api/windows.foundation.collections.propertyset). В этом примере создается экземпляр коллекции PropertySet в управляемом коде и регистрируется обработчик событий для отслеживания изменений в коллекции. Затем добавляется код JavaScript, который получает коллекцию, регистрирует собственный обработчик событий и использует коллекцию. Наконец, в код добавляется метод, который вносит изменения в коллекцию из управляемого кода и показывает, как JavaScript обрабатывает управляемое исключение.

> **Важно**  в этом примере событие запускается в ПОТОКЕ пользовательского интерфейса. Если событие вызывается в фоновом потоке, например, в асинхронном вызове, необходимо выполнить некоторые дополнительные действия, чтобы код JavaScript мог обработать это событие. Дополнительные сведения см. [в разделе вызов событий в компонентах среда выполнения Windows](raising-events-in-windows-runtime-components.md).

В проект SampleComponent добавьте новый класс **public sealed** (класс **Public NotInheritable** в Visual Basic) с именем PropertySetStats. Класс создает программу-оболочку для коллекции PropertySet и обрабатывает ее событие MapChanged. Обработчик событий отслеживает число изменений каждого типа, а метод DisplayStats создает отчет в формате HTML. Обратите внимание на дополнительную директиву **using** (директива **Imports** в Visual Basic). Обратите внимание, что ее нужно добавить к имеющимся директивам **using**, а не вместо них.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> using Windows.Foundation.Collections;
>
> namespace SampleComponent
> {
>     public sealed class PropertySetStats
>     {
>         private PropertySet _ps;
>         public PropertySetStats()
>         {
>             _ps = new PropertySet();
>             _ps.MapChanged += this.MapChangedHandler;
>         }
>
>         public PropertySet PropertySet { get { return _ps; } }
>
>         int[] counts = { 0, 0, 0, 0 };
>         private void MapChangedHandler(IObservableMap<string, object> sender,
>             IMapChangedEventArgs<string> args)
>         {
>             counts[(int)args.CollectionChange] += 1;
>         }
>
>         public string DisplayStats()
>         {
>             StringBuilder report = new StringBuilder("<br/>Number of changes:<ul>");
>             for (int i = 0; i < counts.Length; i++)
>             {
>                 report.Append("<li>" + (CollectionChange)i + ": " + counts[i] + "</li>");
>             }
>             return report.ToString() + "</ul>";
>         }
>     }
> }
> ```
> ```vb
> Imports System.Text
>
> Public NotInheritable Class PropertySetStats
>     Private _ps As PropertySet
>     Public Sub New()
>         _ps = New PropertySet()
>         AddHandler _ps.MapChanged, AddressOf Me.MapChangedHandler
>     End Sub
>
>     Public ReadOnly Property PropertySet As PropertySet
>         Get
>             Return _ps
>         End Get
>     End Property
>
>     Dim counts() As Integer = {0, 0, 0, 0}
>     Private Sub MapChangedHandler(ByVal sender As IObservableMap(Of String, Object),
>         ByVal args As IMapChangedEventArgs(Of String))
>
>         counts(CInt(args.CollectionChange)) += 1
>     End Sub
>
>     Public Function DisplayStats() As String
>         Dim report As New StringBuilder("<br/>Number of changes:<ul>")
>         For i As Integer = 0 To counts.Length - 1
>             report.Append("<li>" & CType(i, CollectionChange).ToString() &
>                           ": " & counts(i) & "</li>")
>         Next
>         Return report.ToString() & "</ul>"
>     End Function
> End Class
> ```

Обработчик событий следует привычному шаблону событий .NET Framework, за исключением того, что отправитель события (в данном случае объект набора свойств) приводится к строке IObservableMap&lt;, объектного&gt; интерфейс (IObservableMap (Of String, Object) в Visual Basic), который является экземпляром среда выполнения Windows Interface [IObservableMap&lt;K, V&gt;](/uwp/api/Windows.Foundation.Collections.IObservableMap_K_V_). (При необходимости можно привести параметр sender к его типу.) Кроме того, аргументы события представляются в качестве интерфейса, а не в качестве объекта.

В файл default.js добавьте функцию Runtime1, как показано ниже. Этот код создает объект PropertySetStats, получает коллекцию PropertySet и добавляет собственный обработчик событий (функция onMapChanged), чтобы обрабатывать событие MapChanged. После внесения изменений в коллекцию runtime1 вызывает метод DisplayStats, чтобы показать сводную информацию о типах изменений.

```javascript
var propertysetstats;

function runtime1() {
    document.getElementById('output').innerHTML = "";

    propertysetstats = new SampleComponent.PropertySetStats();
    var propertyset = propertysetstats.propertySet;

    propertyset.addEventListener("mapchanged", onMapChanged);

    propertyset.insert("FirstProperty", "First property value");
    propertyset.insert("SuperfluousProperty", "Unnecessary property value");
    propertyset.insert("AnotherProperty", "A property value");

    propertyset.insert("SuperfluousProperty", "Altered property value")
    propertyset.remove("SuperfluousProperty");

    document.getElementById('output').innerHTML +=
        propertysetstats.displayStats();
}

function onMapChanged(change) {
    var result
    switch (change.collectionChange) {
        case Windows.Foundation.Collections.CollectionChange.reset:
            result = "All properties cleared";
            break;
        case Windows.Foundation.Collections.CollectionChange.itemInserted:
            result = "Inserted " + change.key + ": '" +
                change.target.lookup(change.key) + "'";
            break;
        case Windows.Foundation.Collections.CollectionChange.itemRemoved:
            result = "Removed " + change.key;
            break;
        case Windows.Foundation.Collections.CollectionChange.itemChanged:
            result = "Changed " + change.key + " to '" +
                change.target.lookup(change.key) + "'";
            break;
        default:
            break;
     }

     document.getElementById('output').innerHTML +=
         "<br/>" + result;
}
```

Способ обработки событий среды выполнения Windows в языке JavaScript значительно отличается от способа их обработки в коде .NET Framework. Обработчик событий JavaScript принимает только один аргумент. При просмотре этого объекта в отладчике Visual Studio первое свойство задает отправителя. Члены интерфейса аргумента события также отображаются в этом объекте.

Чтобы запустить приложение, нажмите клавишу F5. Если класс не запечатан, появляется сообщение об ошибке «Экспорт незапечатанных типов не поддерживается. Пометьте тип "SampleComponent.Example" как запечатанный».

Нажмите кнопку **Runtime 1**. Обработчик событий будет отображать изменения при добавлении и изменении элементов, а после завершения будет вызван метод DisplayStats, чтобы показать сводные данные. Чтобы остановить отладку и закрыть приложение, вернитесь в Visual Studio и нажмите сочетание клавиш Shift+F5.

Чтобы из управляемого кода добавить в коллекцию PropertySet еще два элемента, добавьте в класс PropertySetStats следующий код:

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public void AddMore()
> {
>     _ps.Add("NewProperty", "New property value");
>     _ps.Add("AnotherProperty", "A property value");
> }
> ```
> ```vb
> Public Sub AddMore()
>     _ps.Add("NewProperty", "New property value")
>     _ps.Add("AnotherProperty", "A property value")
> End Sub
> ```

Этот код подчеркивает еще одно различие в использовании типов среды выполнения Windows в двух средах. Если вы будете самостоятельно вводить этот код, вы заметите, что IntelliSense не показывает метод insert, который использовался в коде JavaScript. Вместо этого он показывает метод Add, который обычно встречается в коллекциях .NET. Это обусловлено тем, что некоторые часто используемые интерфейсы коллекций имеют разные имена, но аналогичные функции в среда выполнения Windows и .NET. При использовании этих интерфейсов в управляемом коде они представляются в виде эквивалентов .NET Framework. Это рассматривается в [Среда выполнения Windows компонентах с C# и Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md). При использовании тех же интерфейсов в коде JavaScript единственное отличие от среды выполнения Windows будет состоять в том, что буквы в верхнем регистре в начале имен членов будут преобразованы в нижний регистр.

Наконец, чтобы вызвать метод AddMore с обработкой исключений, добавьте в файл default.js функцию runtime2.

```javascript
function runtime2() {
   try {
      propertysetstats.addMore();
    }
   catch(ex) {
       document.getElementById('output').innerHTML +=
          "<br/><b>" + ex + "<br/>";
   }

   document.getElementById('output').innerHTML +=
       propertysetstats.displayStats();
}
```

Еще раз добавьте код регистрации обработчика событий.

```javascript
var runtimeButton1 = document.getElementById("runtimeButton1");
runtimeButton1.addEventListener("click", runtime1, false);
var runtimeButton2 = document.getElementById("runtimeButton2");
runtimeButton2.addEventListener("click", runtime2, false);
```

Чтобы запустить приложение, нажмите клавишу F5. Нажмите кнопку **Runtime 1**, а затем — кнопку **Runtime 2**. Обработчик событий JavaScript сообщит о первом изменении коллекции. Но второе изменение содержит повторяющийся ключ. Пользователи словарей .NET Framework ожидают, что при этом метод Add создаст исключение; именно это и произойдет. JavaScript обрабатывает исключение .NET.

> **Обратите внимание** ,  не удается отобразить сообщение об исключении из кода JavaScript. Текст сообщения заменяется данными трассировки стека. Дополнительные сведения см. в подразделе «Создание исключений» раздела о создании C# среда выполнения Windows компонентов в и Visual Basic.

Напротив, когда код JavaScript вызывает метод insert с повторяющимся ключом, значение элемента изменяется. Это различие в поведении обусловлено различными способами, которые JavaScript и .NET поддерживают среда выполнения Windows, как описано в [Среда выполнения Windows компонентах с C# и Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md).

## <a name="returning-managed-types-from-your-component"></a>Возвращение управляемых типов из компонента

Как отмечалось ранее, собственные типы среды выполнения Windows можно свободно передавать в любом направлении между кодом JavaScript и кодом C# или Visual Basic. В большинстве случаев имена типов и членов в обоих случаях совпадают (кроме имен членов, которые в JavaScript начинаются со строчной буквы). Однако в предыдущем разделе класс PropertySet имел другие члены в управляемом коде. (Например, в JavaScript, который вызывал метод Insert, и в коде .NET, который вызывал метод Add.) В этом разделе рассматриваются способы влияния этих различий на .NET Framework типы, передаваемые в JavaScript.

Помимо возврата типов среды выполнения Windows, созданных в разрабатываемом компоненте или переданных в него из JavaScript, можно также возвращать в JavaScript управляемые типы, созданные в управляемом коде, как если бы они были соответствующими типами среды выполнения Windows. Даже в первом, простом, примере класса среды выполнения параметры и возвращаемые значения имели простые типы Visual Basic и C#, которые являются типами .NET Framework. Чтобы продемонстрировать это для коллекции, добавьте в класс Example следующий код, чтобы создать метод, возвращающий универсальный словарь строк с целочисленными индексами:

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public static IDictionary<int, string> GetMapOfNames()
> {
>     Dictionary<int, string> retval = new Dictionary<int, string>();
>     retval.Add(1, "one");
>     retval.Add(2, "two");
>     retval.Add(3, "three");
>     retval.Add(42, "forty-two");
>     retval.Add(100, "one hundred");
>     return retval;
> }
> ```
> ```vb
> Public Shared Function GetMapOfNames() As IDictionary(Of Integer, String)
>     Dim retval As New Dictionary(Of Integer, String)
>     retval.Add(1, "one")
>     retval.Add(2, "two")
>     retval.Add(3, "three")
>     retval.Add(42, "forty-two")
>     retval.Add(100, "one hundred")
>     Return retval
> End Function
> ```

Обратите внимание, что словарь должен возвращаться в форме интерфейса, реализуемого классом [Dictionary&lt;TKey, TValue&gt;](/dotnet/api/system.collections.generic.dictionary-2) и сопоставляемого с интерфейсом среды выполнения Windows. В данном случае это интерфейс IDictionary&lt;int, string&gt; (IDictionary(Of Integer, String) в Visual Basic). Если тип среды выполнения Windows IMap&lt;int, string&gt; передается управляемому коду, он представляется в виде IDictionary&lt;int, string&gt;, а при передаче управляемого типа в код JavaScript верно обратное.

**Важно** .  Если управляемый тип реализует несколько интерфейсов, JavaScript использует интерфейс, который отображается первым в списке. Например, если в код JavaScript возвращается тип Dictionary&lt;int, string&gt;, он отображается как IDictionary&lt;int, string&gt; независимо от того, какой интерфейс указан в качестве типа возвращаемого значения. Это означает, что если первый интерфейс не включает член, который отображается в последующих интерфейсах, этот член не будет видимым в JavaScript.

 

Чтобы проверить новый метод и использовать словарь, добавьте в файл default.js функции returns1 и returns2:

```javascript
var names;

function returns1() {
    names = SampleComponent.Example.getMapOfNames();
    document.getElementById('output').innerHTML = showMap(names);
}

var ct = 7;

function returns2() {
    if (!names.hasKey(17)) {
        names.insert(43, "forty-three");
        names.insert(17, "seventeen");
    }
    else {
        var err = names.insert("7", ct++);
        names.insert("forty", "forty");
    }
    document.getElementById('output').innerHTML = showMap(names);
}

function showMap(map) {
    var item = map.first();
    var retval = "<ul>";

    for (var i = 0, len = map.size; i < len; i++) {
        retval += "<li>" + item.current.key + ": " + item.current.value + "</li>";
        item.moveNext();
    }
    return retval + "</ul>";
}
```

Добавьте код регистрации события к тому же блоку then, к которому был добавлен другой код регистрации события:

```javascript
var returnsButton1 = document.getElementById("returnsButton1");
returnsButton1.addEventListener("click", returns1, false);
var returnsButton2 = document.getElementById("returnsButton2");
returnsButton2.addEventListener("click", returns2, false);
```

Этот код JavaScript позволяет продемонстрировать несколько интересных особенностей. Во-первых, он включает функцию showMap для отображения содержимого словаря в формате HTML. В коде showMap обратите внимание на шаблон итерации. В .NET нет первого метода в универсальном интерфейсе IDictionary, и размер возвращается свойством Count, а не методом size. В JavaScript IDictionary&lt;int, string&gt; представляется типом среды выполнения Windows IMap&lt;int, string&gt;. (См. описание интерфейса [IMap&lt;K,V&gt;](/uwp/api/Windows.Foundation.Collections.IMap_K_V_)).

В функции returns2, как и в предыдущих примерах, JavaScript вызывает метод Insert (insert на языке JavaScript) для добавления элементов в словарь.

Чтобы запустить приложение, нажмите клавишу F5. Для создания и отображения начального содержимого словаря нажмите кнопку **Returns 1**. Чтобы добавить в словарь еще две записи, нажмите кнопку **Returns 2**. Обратите внимание, что записи отображаются в порядке вставки, как и ожидалось для Dictionary&lt;TKey, TValue&gt;. Если требуется отсортировать их, можно вернуть SortedDictionary&lt;int, string&gt; из GetMapOfNames. (Класс PropertySet, используемый в предыдущих примерах, имеет иную внутреннюю структуру по сравнению с Dictionary&lt;TKey, TValue&gt;.)

JavaScript не является строго типизированным языком, поэтому использование строго типизированных универсальных коллекций может приводить к неожиданным результатам. Еще раз нажмите кнопку **Returns 2**. JavaScript преобразует значение «7» в числовое значение 7, а хранящееся в ct число 7 — в строку. Строка «forty» будет преобразована в ноль. Но это только начало. Нажмите кнопку **Returns 2** еще несколько раз. В управляемом коде метод Add создал бы исключения повторяющихся ключей, даже если бы значения были приведены к правильным типам. В то время как метод Insert изменит значение, связанное с имеющимся ключом, и вернет логическое значение, показывающее, добавлен ли новый ключ в словарь. Именно поэтому значение, связанное с ключом 7, продолжает изменяться.

Другая непредвиденная особенность — если передать неприсвоенную переменную JavaScript в качестве строкового аргумента, появится строка «undefined». Иными словами, следует соблюдать осторожность при передаче типов коллекций .NET Framework в код JavaScript.

> **Обратите внимание** ,  при наличии больших объемов текста для сцепления его можно сделать более эффективным, переместив код в метод .NET Framework и используя класс StringBuilder, как показано в функции шовмап.

Несмотря на то, что из компонента среды выполнения Windows нельзя предоставлять доступ к собственным универсальным типам, можно возвращать универсальные коллекции .NET Framework в классы среды выполнения Windows с помощью кода примерно следующего вида:

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public static object GetListOfThis(object obj)
> {
>     Type target = obj.GetType();
>     return Activator.CreateInstance(typeof(List<>).MakeGenericType(target));
> }
> ```
> ```vb
> Public Shared Function GetListOfThis(obj As Object) As Object
>     Dim target As Type = obj.GetType()
>     Return Activator.CreateInstance(GetType(List(Of )).MakeGenericType(target))
> End Function
> ```

List&lt;T&gt; реализует IList&lt;T&gt;, который представляется в JavaScript как тип среды выполнения Windows (IVector&lt;T&gt;).

## <a name="declaring-events"></a>Объявление событий


Для объявления событий можно использовать стандартные шаблоны событий .NET Framework или другие шаблоны, применяемые в среде выполнения Windows. Платформа .NET Framework поддерживает эквивалентность между делегатом System.EventHandler&lt;TEventArgs&gt; и делегатом среды выполнения Windows EventHandler&lt;T&gt;, поэтому использование EventHandler&lt;TEventArgs&gt; является удобным способом реализации стандартного шаблона .NET Framework. Чтобы продемонстрировать этот подход, добавьте в проект SampleComponent следующие два класса:

> [!div class="tabbedCodeSnippets"]
> ```csharp
> namespace SampleComponent
> {
>     public sealed class Eventful
>     {
>         public event EventHandler<TestEventArgs> Test;
>         public void OnTest(string msg, long number)
>         {
>             EventHandler<TestEventArgs> temp = Test;
>             if (temp != null)
>             {
>                 temp(this, new TestEventArgs()
>                 {
>                     Value1 = msg,
>                     Value2 = number
>                 });
>             }
>         }
>     }
>
>     public sealed class TestEventArgs
>     {
>         public string Value1 { get; set; }
>         public long Value2 { get; set; }
>     }
> }
> ```
> ```vb
> Public NotInheritable Class Eventful
>     Public Event Test As EventHandler(Of TestEventArgs)
>     Public Sub OnTest(ByVal msg As String, ByVal number As Long)
>         RaiseEvent Test(Me, New TestEventArgs() With {
>                             .Value1 = msg,
>                             .Value2 = number
>                             })
>     End Sub
> End Class
>
> Public NotInheritable Class TestEventArgs
>     Public Property Value1 As String
>     Public Property Value2 As Long
> End Class
> ```

Если в среде выполнения Windows предоставляется доступ к событию, то класс аргументов этого события наследуется от System.Object. Он не наследует от System. EventArgs, как в .NET, поскольку EventArgs не является типом среда выполнения Windows.

При объявлении пользовательских методов доступа к событиям (событие объявляется с ключевым словом (**Custom** в Visual Basic) необходимо использовать шаблон событий среды выполнения Windows. См. раздел [пользовательские события и методы доступа к событиям в компонентах среда выполнения Windows](custom-events-and-event-accessors-in-windows-runtime-components.md).

Чтобы обработать событие Test, добавьте в файл default.js функцию events1. Функция events1 создает функцию обработчика событий для события Test и немедленно вызывает метод OnTest, чтобы вызвать это событие. Если в теле обработчика событий установить точку останова, можно увидеть, что объект, передаваемый в качестве единственного параметра, содержит исходный объект и оба члена TestEventArgs.

```javascript
var ev;

function events1() {
   ev = new SampleComponent.Eventful();
   ev.addEventListener("test", function (e) {
       document.getElementById('output').innerHTML = e.value1;
       document.getElementById('output').innerHTML += "<br/>" + e.value2;
   });
   ev.onTest("Number of feet in a mile:", 5280);
}
```

Добавьте код регистрации события к тому же блоку then, к которому был добавлен другой код регистрации события:

```javascript
var events1Button = document.getElementById("events1Button");
events1Button.addEventListener("click", events1, false);
```

## <a name="exposing-asynchronous-operations"></a>Предоставление доступа к асинхронным операциям


Платформа .NET Framework содержит богатый набор средств для асинхронной и параллельной обработки, основанных на классе Task и универсальном классе [Task&lt;TResult&gt;](/dotnet/api/system.threading.tasks.task-1). Для реализации асинхронной обработки на основе задач в компоненте среды выполнения Windows используйте интерфейсы среды выполнения Windows [IAsyncAction](/windows/desktop/api/windows.foundation/nn-windows-foundation-iasyncaction), [IAsyncActionWithProgress&lt;TProgress&gt;](/previous-versions/br205784(v=vs.85)), [IAsyncOperation&lt;TResult&gt;](/previous-versions/br205802(v=vs.85)) и [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](/previous-versions/br205807(v=vs.85)). (В среде выполнения Windows операции возвращают результаты, а действия — нет).

В этом разделе демонстрируется отменяемая асинхронная операция, которая информирует о ходе выполнения и возвращает результаты. Метод GetPrimesInRangeAsync использует класс [AsyncInfo](/dotnet/api/system.runtime.interopservices.windowsruntime) для создания задачи и подключения его функций отмены и хода выполнения к объекту WinJS.Promise. Сначала добавьте метод GetPrimesInRangeAsync в класс Example:

> [!div class="tabbedCodeSnippets"]
> ```csharp
> using System.Runtime.InteropServices.WindowsRuntime;
> using Windows.Foundation;
>
> public static IAsyncOperationWithProgress<IList<long>, double>
> GetPrimesInRangeAsync(long start, long count)
> {
>     if (start < 2 || count < 1) throw new ArgumentException();
>
>     return AsyncInfo.Run<IList<long>, double>((token, progress) =>
>
>         Task.Run<IList<long>>(() =>
>         {
>             List<long> primes = new List<long>();
>             double onePercent = count / 100;
>             long ctProgress = 0;
>             double nextProgress = onePercent;
>
>             for (long candidate = start; candidate < start + count; candidate++)
>             {
>                 ctProgress += 1;
>                 if (ctProgress >= nextProgress)
>                 {
>                     progress.Report(ctProgress / onePercent);
>                     nextProgress += onePercent;
>                 }
>                 bool isPrime = true;
>                 for (long i = 2, limit = (long)Math.Sqrt(candidate); i <= limit; i++)
>                 {
>                     if (candidate % i == 0)
>                     {
>                         isPrime = false;
>                         break;
>                     }
>                 }
>                 if (isPrime) primes.Add(candidate);
>
>                 token.ThrowIfCancellationRequested();
>             }
>             progress.Report(100.0);
>             return primes;
>         }, token)
>     );
> }
> ```
> ```vb
> Imports System.Runtime.InteropServices.WindowsRuntime
>
> Public Shared Function GetPrimesInRangeAsync(ByVal start As Long, ByVal count As Long)
> As IAsyncOperationWithProgress(Of IList(Of Long), Double)
>
>     If (start < 2 Or count < 1) Then Throw New ArgumentException()
>
>     Return AsyncInfo.Run(Of IList(Of Long), Double)( _
>         Function(token, prog)
>             Return Task.Run(Of IList(Of Long))( _
>                 Function()
>                     Dim primes As New List(Of Long)
>                     Dim onePercent As Long = count / 100
>                     Dim ctProgress As Long = 0
>                     Dim nextProgress As Long = onePercent
>
>                     For candidate As Long = start To start + count - 1
>                         ctProgress += 1
>
>                         If ctProgress >= nextProgress Then
>                             prog.Report(ctProgress / onePercent)
>                             nextProgress += onePercent
>                         End If
>
>                         Dim isPrime As Boolean = True
>                         For i As Long = 2 To CLng(Math.Sqrt(candidate))
>                             If (candidate Mod i) = 0 Then
>                                 isPrime = False
>                                 Exit For
>                             End If
>                         Next
>
>                         If isPrime Then primes.Add(candidate)
>
>                         token.ThrowIfCancellationRequested()
>                     Next
>                     prog.Report(100.0)
>                     Return primes
>                 End Function, token)
>         End Function)
> End Function
> ```

GetPrimesInRangeAsync – очень простой метод поиска простых чисел, который намеренно создан таким. Нас интересует в первую очередь реализация асинхронной операции, поэтому простота имеет большое значение, а низкая производительность станет дополнительным преимуществом при демонстрации отмены. Метод GetPrimesInRangeAsync находит простые числа путем простого перебора: каждое число-кандидат делится на все целые числа, которые не превышают квадратный корень из этого числа, а не только на простые числа. Рассмотрим все шаги этого кода:

-   Перед началом асинхронной операции выполните вспомогательные действия, такие как проверка параметров и создание исключений, если заданы недопустимые входные значения.
-   В основе этой реализации лежат метод [AsyncInfo.Run&lt;TResult, TProgress&gt;(Func&lt;CancellationToken, IProgress&lt;TProgress&gt;, Task&lt;TResult&gt;](/dotnet/api/system.runtime.interopservices.windowsruntime)&gt;) и делегат, который является единственным параметром этого метода. Делегат должен принимать токен отмены и интерфейс для информирования о ходе выполнения, а также возвращать запущенную задачу, которая использует эти параметры. Если код JavaScript вызывает метод GetPrimesInRangeAsync, выполняются следующие действия (не обязательно в порядке, приведенном ниже):

    -   Объект [WinJS.Promise](/previous-versions/windows/apps/br211867(v=win.10)) предоставляет функции для обработки возвращаемых результатов, реагирования на отмену и обработки информации о ходе выполнения.
    -   Метод AsyncInfo.Run создает источник отмены и объект, реализующий интерфейс IProgress&lt;T&gt;. Токен [CancellationToken](/dotnet/api/system.threading.cancellationtoken) из источника отмены и интерфейс [IProgress&lt;T&gt;](/dotnet/api/system.iprogress-1) передаются делегату.

        > **Обратите внимание** ,  если объект Promise не предоставляет функцию для реагирования на отмену, АсинЦинфо. Run все равно передает отменяемый маркер, и отмена может произойти. Если объект Promise не предоставляет функцию для обработки обновлений хода выполнения, AsyncInfo.Run все равно предоставляет объект, реализующий IProgress&lt;T&gt;, но его отчеты игнорируются.

    -   Делегат использует метод [Task.Run&lt;TResult&gt;(Func&lt;TResult&gt;, CancellationToken](/dotnet/api/system.threading.tasks.task.run#System_Threading_Tasks_Task_Run__1_System_Func___0__System_Threading_CancellationToken_)) для создания запущенной задачи, которая использует токен и интерфейс хода выполнения. Делегат запущенной задачи предоставляется лямбда-функцией, которая вычисляет требуемый результат. Ниже мы подробнее остановимся на этом.
    -   Метод AsyncInfo.Run создает объект, реализующий интерфейс [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_), связывает механизм отмены среды выполнения Windows с источником токена и связывает функцию информирования о ходе выполнения объекта Promise с интерфейсом IProgress&lt;T&gt;.
    -   Интерфейс IAsyncOperationWithProgress&lt;TResult, TProgress&gt; возвращается в JavaScript.

-   Лямбда-функция, представляемая запущенной задачей, не принимает никаких аргументов. Поскольку это лямбда-функция, у нее есть доступ к токену и интерфейсу IProgress. При вычислении каждого числа-кандидата лямбда-функция выполняет следующие операции:

    -   Проверяет, достигнут ли очередной процентный пункт хода выполнения. Если он достигнут, лямбда-функция вызывает метод IProgress&lt;T&gt;.Report и этот процент передается функции, заданной объектом Promise для информирования о ходе выполнения.
    -   С помощью токена отмены создает исключение, если операция была отменена. Если метод [IAsyncInfo.Cancel](/uwp/api/windows.foundation.iasyncinfo.cancel) (который наследуется интерфейсом IAsyncOperationWithProgress&lt;TResult, TProgress&gt;) был вызван, то подключение, настраиваемое методом AsyncInfo.Run, гарантирует, что токен отмены получает уведомление.
-   Когда лямбда-функция возвращает список простых чисел, список передается функции, которая была задана объектом WinJS.Promise для обработки результатов.

Для создания объекта Promise в JavaScript и настройки механизма отмены добавьте в файл default.js функции asyncRun и asyncCancel.

```javascript
var resultAsync;
function asyncRun() {
    document.getElementById('output').innerHTML = "Retrieving prime numbers.";
    btnAsync.disabled = "disabled";
    btnCancel.disabled = "";

    resultAsync = SampleComponent.Example.getPrimesInRangeAsync(10000000000001, 2500).then(
        function (primes) {
            for (i = 0; i < primes.length; i++)
                document.getElementById('output').innerHTML += " " + primes[i];

            btnCancel.disabled = "disabled";
            btnAsync.disabled = "";
        },
        function () {
            document.getElementById('output').innerHTML += " -- getPrimesInRangeAsync was canceled. -- ";

            btnCancel.disabled = "disabled";
            btnAsync.disabled = "";
        },
        function (prog) {
            document.getElementById('primeProg').value = prog;
        }
    );
}

function asyncCancel() {    
    resultAsync.cancel();
}
```

Не забудьте еще раз добавить код регистрации события.

```javascript
var btnAsync = document.getElementById("btnAsync");
btnAsync.addEventListener("click", asyncRun, false);
var btnCancel = document.getElementById("btnCancel");
btnCancel.addEventListener("click", asyncCancel, false);
```

Путем вызова асинхронного метода GetPrimesInRangeAsync функция asyncRun создает объект WinJS.Promise Метод then объекта принимает три функции, которые обрабатывают возвращаемый результат, реагируют на ошибки (включая отмену) и обрабатывают отчеты о ходе выполнения. В этом примере возвращенные результаты отображаются в области вывода. Отмена или завершение сбрасывают состояние кнопок, которые запускают и отменяют операцию. Информация о ходе выполнения отображается на соответствующем элементе управления.

Функция asyncCancel просто вызывает метод Cancel объекта WinJS.Promise.

Чтобы запустить приложение, нажмите клавишу F5. Для запуска асинхронной операции нажмите кнопку **Async**. Дальнейший ход событий зависит от быстродействия компьютера. Если индикатор выполнения заполнится очень быстро, увеличьте начальное число, передаваемое в GetPrimesInRangeAsync, на один или несколько порядков. Длительность выполнения операции можно постепенно изменять, увеличивая или уменьшая количество проверяемых чисел, но добавление нулей в середину начального числа будет иметь больший эффект. Чтобы отменить операцию, нажмите кнопку **Cancel Async** .

## <a name="related-topics"></a>Статьи по теме

* [Обзор .NET для приложений UWP](/previous-versions/windows/apps/br230302(v=vs.140))
* [.NET для приложений UWP](/dotnet/api/index?view=dotnet-uwp-10.0)