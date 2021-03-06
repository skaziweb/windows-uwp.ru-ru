---
Description: Использование распознавания рукописного ввода и его анализа для распознавания росчерков пера Windows Ink как текста и фигур.
title: Распознавание росчерков пера Windows Ink как текста и фигур
ms.assetid: C2F3F3CE-737F-4652-98B7-5278A462F9D3
label: Recognize Windows Ink strokes as text
template: detail.hbs
keywords: Windows Ink, Windows Inking, DirectInk, InkPresenter, InkCanvas, распознавание рукописного ввода, взаимодействие с пользователем, ввод
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d496051a066ffcf9e8df5d4798415e6089cad4d1
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970919"
---
# <a name="recognize-windows-ink-strokes-as-text-and-shapes"></a>Распознавание росчерков пера Windows Ink как текста и фигур

Преобразуйте росчерки пера в текст и фигуры с помощью встроенных функций распознавания Windows Ink.

> **Важные API-интерфейсы**: [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas), [**Windows.UI.Input.Inking**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking)

## <a name="free-form-recognition-with-ink-analysis"></a>Произвольное распознавание с помощью средства анализа рукописного ввода

Здесь мы покажем, как использовать модуль анализа Windows Ink ([Windows.UI.Input.Inking.Analysis](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis)) для классификации, анализа и распознавания произвольного набора росчерков на [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) как текста или фигур. (Помимо распознавания текста и фигуры, анализ рукописного ввода можно использовать для распознавания структуры документа, маркированных списков и общих рисунков.)

> [!NOTE]
> Для получения сведений о сценариях с простым однострочным обычным текстом, например ввода в форму, см. в разделе [Ограниченное распознавание рукописного ввода](#constrained-handwriting-recognition) далее.

В этом примере распознавание инициируется, когда пользователь нажимает кнопку, чтобы указать, что рисование завершено.

**Скачать этот пример из [примера анализа рукописного ввода (базовый)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-analysis-basic.zip)**

1. Сначала мы настраиваем пользовательский интерфейс (MainPage.xaml). 

   Пользовательский интерфейс включает кнопку "Распознать", [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) и стандартный [**холст**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.canvas). При нажатии кнопки "Распознать" все росчерки рукописного ввода на холсте рукописного ввода анализируются, и (если будут распознаны) на стандартном холсте рисуются соответствующие фигуры и пишется текст. Затем исходные росчерки пера удаляются с холста рукописного ввода.

   ```xaml
   <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel" 
                    Orientation="Horizontal" 
                    Grid.Row="0">
            <TextBlock x:Name="Header" 
                        Text="Basic ink analysis sample" 
                        Style="{ThemeResource HeaderTextBlockStyle}" 
                        Margin="10,0,0,0" />
            <Button x:Name="recognize" 
                    Content="Recognize" 
                    Margin="50,0,10,0"/>
        </StackPanel>
        <Grid x:Name="drawingCanvas" Grid.Row="1">

            <!-- The canvas where we render the replacement text and shapes. -->
            <Canvas x:Name="recognitionCanvas" />
            <!-- The canvas for ink input. -->
            <InkCanvas x:Name="inkCanvas" />

        </Grid>
   </Grid>
   ```

2. В этом примере мы сначала добавим ссылки на тип пространства имен, необходимые для наших функций рукописного ввода и его анализа, в файл кода программной части пользовательского интерфейса (MainPage.xaml.cs):
    - [Windows.UI.Input.Inking](https://docs.microsoft.com/uwp/api/windows.ui.input.inking)
    - [Windows. UI. input. Ввод рукописного ввода. Analysis](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis)
    - [Windows.UI.Xaml.Shapes](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes)

3. Затем мы определим наши глобальные переменные:

   ```csharp
    InkAnalyzer inkAnalyzer = new InkAnalyzer();
    IReadOnlyList<InkStroke> inkStrokes = null;
    InkAnalysisResult inkAnalysisResults = null;
   ```

4. Затем мы зададим некоторые основные реакции на рукописный ввод:
    - [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) настраивается для интерпретации входных данных от пера, мыши и сенсорного устройства как росчерков пера ([**InputDeviceTypes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.inputdevicetypes)). 
    - Росчерки пера выводятся на [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) с помощью указанных атрибутов [**InkDrawingAttributes**](https://docs.microsoft.com/windows/desktop/tablet/inkdrawingattributes-class). 
    - Также объявляется прослушиватель для события нажатия кнопки "Распознать".

    ```csharp
    /// <summary>
    /// Initialize the UI page.
    /// </summary>
    public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen | 
            Windows.UI.Core.CoreInputDeviceTypes.Touch;

        // Set initial ink stroke attributes.
        InkDrawingAttributes drawingAttributes = new InkDrawingAttributes();
        drawingAttributes.Color = Windows.UI.Colors.Black;
        drawingAttributes.IgnorePressure = false;
        drawingAttributes.FitToCurve = true;
        inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);

        // Listen for button click to initiate recognition.
        recognize.Click += RecognizeStrokes_Click;
    }
    ```

5. В этом примере мы выполним анализ чернил в обработчике событий нажатия кнопки "Распознать".
    - Сначала вызовите [**GetStrokes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstrokecontainer.GetStrokes) на [**StrokeContainer**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.StrokeContainer) из [**InkCanvas.InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.InkPresenter), чтобы получить коллекцию всех текущих росчерков пера.
    - Если имеются росчерки пера, передайте их в вызов [**AddDataForStrokes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalyzer#Windows_UI_Input_Inking_Analysis_InkAnalyzer_AddDataForStrokes_Windows_Foundation_Collections_IIterable_Windows_UI_Input_Inking_InkStroke__) из InkAnalyzer.
    - Мы пытаемся распознать и рисунки, и текст, но вы можете использовать метод [**SetStrokeDataKind**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.setstrokedatakind), чтобы указать, интересует ли вас только текст (в том числе структура документа и маркированные списки) или только рисунки (в том числе для распознавания фигур).
    - Вызовите [**AnalyzeAsync**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.AnalyzeAsync) для запуска анализа рукописного ввода и получения [**InkAnalysisResult**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult).
    - Если [**Status**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult.Status) возвращает состояние **Updated**, вызовите [**FindNodes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalysisroot.findnodes) для [**InkAnalysisNodeKind.InkWord**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalysisnodekind) и [**InkAnalysisNodeKind.InkDrawing**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalysisnodekind).
    - Пройдите через оба набора типов узлов и нарисуйте соответствующий текст или фигуру на холсте распознавания (ниже холста рукописного ввода).
    - И, наконец, удалите распознанные узлы из InkAnalyzer и соответствующие росчерки с холста рукописного ввода.

    ```csharp
    /// <summary>
    /// The "Analyze" button click handler.
    /// Ink recognition is performed here.
    /// </summary>
    /// <param name="sender">Source of the click event</param>
    /// <param name="e">Event args for the button click routed event</param>
    private async void RecognizeStrokes_Click(object sender, RoutedEventArgs e)
    {
        inkStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
        // Ensure an ink stroke is present.
        if (inkStrokes.Count > 0)
        {
            inkAnalyzer.AddDataForStrokes(inkStrokes);

            // In this example, we try to recognizing both 
            // writing and drawing, so the platform default 
            // of "InkAnalysisStrokeKind.Auto" is used.
            // If you're only interested in a specific type of recognition,
            // such as writing or drawing, you can constrain recognition 
            // using the SetStrokDataKind method as follows:
            // foreach (var stroke in strokesText)
            // {
            //     analyzerText.SetStrokeDataKind(
            //      stroke.Id, InkAnalysisStrokeKind.Writing);
            // }
            // This can improve both efficiency and recognition results.
            inkAnalysisResults = await inkAnalyzer.AnalyzeAsync();

            // Have ink strokes on the canvas changed?
            if (inkAnalysisResults.Status == InkAnalysisStatus.Updated)
            {
                // Find all strokes that are recognized as handwriting and 
                // create a corresponding ink analysis InkWord node.
                var inkwordNodes = 
                    inkAnalyzer.AnalysisRoot.FindNodes(
                        InkAnalysisNodeKind.InkWord);

                // Iterate through each InkWord node.
                // Draw primary recognized text on recognitionCanvas 
                // (for this example, we ignore alternatives), and delete 
                // ink analysis data and recognized strokes.
                foreach (InkAnalysisInkWord node in inkwordNodes)
                {
                    // Draw a TextBlock object on the recognitionCanvas.
                    DrawText(node.RecognizedText, node.BoundingRect);

                    foreach (var strokeId in node.GetStrokeIds())
                    {
                        var stroke = 
                            inkCanvas.InkPresenter.StrokeContainer.GetStrokeById(strokeId);
                        stroke.Selected = true;
                    }
                    inkAnalyzer.RemoveDataForStrokes(node.GetStrokeIds());
                }
                inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();

                // Find all strokes that are recognized as a drawing and 
                // create a corresponding ink analysis InkDrawing node.
                var inkdrawingNodes =
                    inkAnalyzer.AnalysisRoot.FindNodes(
                        InkAnalysisNodeKind.InkDrawing);
                // Iterate through each InkDrawing node.
                // Draw recognized shapes on recognitionCanvas and
                // delete ink analysis data and recognized strokes.
                foreach (InkAnalysisInkDrawing node in inkdrawingNodes)
                {
                    if (node.DrawingKind == InkAnalysisDrawingKind.Drawing)
                    {
                        // Catch and process unsupported shapes (lines and so on) here.
                    }
                    // Process generalized shapes here (ellipses and polygons).
                    else
                    {
                        // Draw an Ellipse object on the recognitionCanvas (circle is a specialized ellipse).
                        if (node.DrawingKind == InkAnalysisDrawingKind.Circle || node.DrawingKind == InkAnalysisDrawingKind.Ellipse)
                        {
                            DrawEllipse(node);
                        }
                        // Draw a Polygon object on the recognitionCanvas.
                        else
                        {
                            DrawPolygon(node);
                        }
                        foreach (var strokeId in node.GetStrokeIds())
                        {
                            var stroke = inkCanvas.InkPresenter.StrokeContainer.GetStrokeById(strokeId);
                            stroke.Selected = true;
                        }
                    }
                    inkAnalyzer.RemoveDataForStrokes(node.GetStrokeIds());
                }
                inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();
            }
        }
    }
    ```

6. Ниже показана функция для рисования TextBlock на холсте распознавания. Мы используем ограничивающий прямоугольник связанного рукописного штриха на холсте рукописного ввода, чтобы задать расположение и размер шрифта TextBlock.

   ```csharp
    /// <summary>
    /// Draw ink recognition text string on the recognitionCanvas.
    /// </summary>
    /// <param name="recognizedText">The string returned by text recognition.</param>
    /// <param name="boundingRect">The bounding rect of the original ink writing.</param>
    private void DrawText(string recognizedText, Rect boundingRect)
    {
        TextBlock text = new TextBlock();
        Canvas.SetTop(text, boundingRect.Top);
        Canvas.SetLeft(text, boundingRect.Left);
    
        text.Text = recognizedText;
        text.FontSize = boundingRect.Height;
    
        recognitionCanvas.Children.Add(text);
    }
   ```

7. Ниже приведены функции для рисования эллипсов и многоугольников на холсте распознавания. Мы используем ограничивающий прямоугольник связанного рукописного штриха на холсте рукописного ввода, чтобы задать расположение и размер шрифта для фигур.

   ```csharp
    // Draw an ellipse on the recognitionCanvas.
    private void DrawEllipse(InkAnalysisInkDrawing shape)
    {
        var points = shape.Points;
        Ellipse ellipse = new Ellipse();

        ellipse.Width = shape.BoundingRect.Width;
        ellipse.Height = shape.BoundingRect.Height;

        Canvas.SetTop(ellipse, shape.BoundingRect.Top);
        Canvas.SetLeft(ellipse, shape.BoundingRect.Left);

        var brush = new SolidColorBrush(Windows.UI.ColorHelper.FromArgb(255, 0, 0, 255));
        ellipse.Stroke = brush;
        ellipse.StrokeThickness = 2;
        recognitionCanvas.Children.Add(ellipse);
    }

    // Draw a polygon on the recognitionCanvas.
    private void DrawPolygon(InkAnalysisInkDrawing shape)
    {
        List<Point> points = new List<Point>(shape.Points);
        Polygon polygon = new Polygon();

        foreach (Point point in points)
        {
            polygon.Points.Add(point);
        }

        var brush = new SolidColorBrush(Windows.UI.ColorHelper.FromArgb(255, 0, 0, 255));
        polygon.Stroke = brush;
        polygon.StrokeThickness = 2;
        recognitionCanvas.Children.Add(polygon);
    }
   ```

Ниже показан этот пример в действии:

| До анализа | После анализа |
| --- | --- |
| ![До анализа](images/ink/ink-analysis-raw2-small.png) | ![После анализа](images/ink/ink-analysis-analyzed2-small.png) |

## <a name="constrained-handwriting-recognition"></a>Ограниченное распознавание рукописного ввода

В предыдущем разделе ([Произвольное распознавание с помощью средства анализа рукописного ввода](#free-form-recognition-with-ink-analysis)) мы показали, как использовать [API-интерфейсы анализа рукописного ввода](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis) для анализа и распознавания произвольных росчерков пера в области InkCanvas.

В этом разделе мы покажем, как использовать модуль распознавания рукописного ввода Windows Ink (не анализ рукописного ввода) для преобразования набора росчерков на объекте [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) в текст (в зависимости от установленного языкового пакета по умолчанию).

> [!NOTE]
> Базовое распознавание рукописного ввода, показанное в этом разделе, лучше всего подходит для сценариев с вводом однострочного текста, например в форму. Для получения сведений о более сложных сценариях распознавания, которые включают анализ и интерпретацию структуры документа, элементы списка, фигуры и рисунки (помимо распознавания текста), см. в предыдущем разделе: [Произвольное распознавание с помощью анализа рукописного ввода](#free-form-recognition-with-ink-analysis).

В этом примере распознавание инициируется, когда пользователь нажимает кнопку, чтобы указать, что рукописный ввод завершен.

**Загрузить этот пример из [примера распознавания рукописного ввода рукописного текста](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-handwriting-reco.zip)**

1. Сначала мы настраиваем пользовательский интерфейс.

   Пользовательский интерфейс включает кнопку "Распознать", [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) и область для отображения результатов распознавания.    

   ```xaml
   <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel"
                    Orientation="Horizontal"
                    Grid.Row="0">
                <TextBlock x:Name="Header"
                    Text="Basic ink recognition sample"
                    Style="{ThemeResource HeaderTextBlockStyle}"
                    Margin="10,0,0,0" />
                <Button x:Name="recognize"
                    Content="Recognize"
                    Margin="50,0,10,0"/>
        </StackPanel>
        <Grid Grid.Row="1">
            <Grid.RowDefinitions>
                <RowDefinition Height="*"/>
                <RowDefinition Height="Auto"/>
            </Grid.RowDefinitions>
            <InkCanvas x:Name="inkCanvas"
                Grid.Row="0"/>
            <TextBlock x:Name="recognitionResult"
                Grid.Row="1"
                Margin="50,0,10,0"/>
        </Grid>
    </Grid>
    ```

2. В этом примере необходимо сначала добавить ссылки на тип пространства имен, необходимые для функций рукописного ввода.
    - [Windows.UI.Input](https://docs.microsoft.com/uwp/api/windows.ui.input)
    - [Windows.UI.Input.Inking](https://docs.microsoft.com/uwp/api/windows.ui.input.inking)

3. Затем мы задаем некоторые основные реакции на рукописный ввод.

    Элемент [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) настраивается интерпретировать данные, вводимые пером или мышью, как росчерки пера ([**InputDeviceTypes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.inputdevicetypes)). Росчерки пера выводятся на [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) с помощью указанных атрибутов [**InkDrawingAttributes**](https://docs.microsoft.com/windows/desktop/tablet/inkdrawingattributes-class). Также объявляется прослушиватель для события нажатия кнопки "Распознать".

    ```csharp
    public MainPage()
        {
            this.InitializeComponent();

            // Set supported inking device types.
            inkCanvas.InkPresenter.InputDeviceTypes =
                Windows.UI.Core.CoreInputDeviceTypes.Mouse |
                Windows.UI.Core.CoreInputDeviceTypes.Pen;

            // Set initial ink stroke attributes.
            InkDrawingAttributes drawingAttributes = new InkDrawingAttributes();
            drawingAttributes.Color = Windows.UI.Colors.Black;
            drawingAttributes.IgnorePressure = false;
            drawingAttributes.FitToCurve = true;
            inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);

            // Listen for button click to initiate recognition.
            recognize.Click += Recognize_Click;
        }
    ```

4. Наконец, мы выполняем базовое распознавание рукописного ввода. В этом примере для выполнения распознавания рукописного ввода мы используем обработчик событий для нажатия кнопки "Распознать".

   - [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) сохраняет все росчерки пера в объекте [**InkStrokeContainer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer). Росчерки отображаются через свойство [**StrokeContainer**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.strokecontainer)**InkPresenter** и извлекаются методом [**GetStrokes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstrokecontainer.getstrokes). 

    ```csharp
    // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
    ```

    - Объект [**InkRecognizerContainer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkRecognizerContainer) создается для управления процессом распознавания рукописного ввода.

    ```csharp
    // Create a manager for the InkRecognizer object
        // used in handwriting recognition.
        InkRecognizerContainer inkRecognizerContainer =
            new InkRecognizerContainer();
    ```

    - [**Рекогнизеасинк**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkmanager.recognizeasync) вызывается для получения набора объектов [**инкрекогнитионресулт**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkRecognitionResult) . Результаты распознавания формируются для каждого слова, обнаруженного [**инкрекогнизер**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkRecognizer).

    ```csharp
    // Recognize all ink strokes on the ink canvas.
        IReadOnlyList<InkRecognitionResult> recognitionResults =
            await inkRecognizerContainer.RecognizeAsync(
                inkCanvas.InkPresenter.StrokeContainer,
                InkRecognitionTarget.All);
    ```

    - Каждый объект [**инкрекогнитионресулт**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkRecognitionResult) содержит набор текстовых кандидатов. Самый верхний элемент в этом списке рассматривается подсистемой распознавания как наилучшее соответствие, за которой следуют оставшиеся кандидаты в порядке уменьшения достоверности.

       Мы перебираем все [**инкрекогнитионресулт**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkRecognitionResult) и компилируем список кандидатов. Затем отображаются кандидаты, а [**инкстрокеконтаинер**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer) очищается (что также очищает [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)).

    ```csharp
    string str = "Recognition result\n";
        // Iterate through the recognition results.
        foreach (var result in recognitionResults)
        {
            // Get all recognition candidates from each recognition result.
            IReadOnlyList<string> candidates = result.GetTextCandidates();
            str += "Candidates: " + candidates.Count.ToString() + "\n";
            foreach (string candidate in candidates)
            {
                str += candidate + " ";
            }
        }
        // Display the recognition candidates.
        recognitionResult.Text = str;
        // Clear the ink canvas once recognition is complete.
        inkCanvas.InkPresenter.StrokeContainer.Clear();
    ```

    - Ниже приведен пример обработчика щелчка Full.

    ```csharp
    // Handle button click to initiate recognition.
        private async void Recognize_Click(object sender, RoutedEventArgs e)
        {
            // Get all strokes on the InkCanvas.
            IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();

            // Ensure an ink stroke is present.
            if (currentStrokes.Count > 0)
            {
                // Create a manager for the InkRecognizer object
                // used in handwriting recognition.
                InkRecognizerContainer inkRecognizerContainer =
                    new InkRecognizerContainer();

                // inkRecognizerContainer is null if a recognition engine is not available.
                if (!(inkRecognizerContainer == null))
                {
                    // Recognize all ink strokes on the ink canvas.
                    IReadOnlyList<InkRecognitionResult> recognitionResults =
                        await inkRecognizerContainer.RecognizeAsync(
                            inkCanvas.InkPresenter.StrokeContainer,
                            InkRecognitionTarget.All);
                    // Process and display the recognition results.
                    if (recognitionResults.Count > 0)
                    {
                        string str = "Recognition result\n";
                        // Iterate through the recognition results.
                        foreach (var result in recognitionResults)
                        {
                            // Get all recognition candidates from each recognition result.
                            IReadOnlyList<string> candidates = result.GetTextCandidates();
                            str += "Candidates: " + candidates.Count.ToString() + "\n";
                            foreach (string candidate in candidates)
                            {
                                str += candidate + " ";
                            }
                        }
                        // Display the recognition candidates.
                        recognitionResult.Text = str;
                        // Clear the ink canvas once recognition is complete.
                        inkCanvas.InkPresenter.StrokeContainer.Clear();
                    }
                    else
                    {
                        recognitionResult.Text = "No recognition results.";
                    }
                }
                else
                {
                    Windows.UI.Popups.MessageDialog messageDialog = new Windows.UI.Popups.MessageDialog("You must install handwriting recognition engine.");
                    await messageDialog.ShowAsync();
                }
            }
            else
            {
                recognitionResult.Text = "No ink strokes to recognize.";
            }
        }
    ```

## <a name="international-recognition"></a>Международное распознавание

Распознавание рукописного ввода встроено в платформу Windows Ink и поддерживает множество языковых стандартов и языков, которые поддерживает Windows.

Список языков, поддерживаемых [**InkRecognizer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkRecognizer), см. в описании свойства [**InkRecognizer.Name**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkrecognizer.name).

Ваше приложение может запросить набор установленных модулей распознавания рукописного ввода и использовать один из них или позволить пользователю выбрать предпочитаемый язык.

**Примечание**    . пользователи могут просмотреть список установленных языков, перейдя к **языку&gt; & времени**. Установленные языки перечислены в разделе **Языки**.

Вот как установить новые языковые пакеты и включить распознавание рукописного ввода для конкретного языка.

1. Перейдите в меню **Параметры &gt; Время и язык &gt; Язык и региональные стандарты**.
2. Выберите **Добавить язык**.
3. Выберите язык из списка, а затем — региональную версию. Язык теперь указан на странице **Язык и региональные стандарты**.
4. Щелкните язык и выберите **Параметры**.
5. На странице **Параметры языка** скачайте **Модуль распознавания рукописного ввода** (здесь также можно скачать полный языковой пакет, модуль распознавания речи и раскладку клавиатуры).

Здесь мы покажем, как использовать модуль распознавания рукописного ввода для интерпретации набора росчерков на [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) на основе выбранного распознавателя.

Распознавание вызывается пользователем путем нажатия кнопки после того, как он закончил писать.

1. Сначала мы настраиваем пользовательский интерфейс.

   Пользовательский интерфейс включает кнопку "Распознать", поле со списком всех установленных распознавателей рукописного ввода, [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) и область для отображения результатов распознавания.

    ```xaml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            <StackPanel x:Name="HeaderPanel"
                        Orientation="Horizontal"
                        Grid.Row="0">
                <TextBlock x:Name="Header"
                           Text="Advanced international ink recognition sample"
                           Style="{ThemeResource HeaderTextBlockStyle}"
                           Margin="10,0,0,0" />
                <ComboBox x:Name="comboInstalledRecognizers"
                         Margin="50,0,10,0">
                    <ComboBox.ItemTemplate>
                        <DataTemplate>
                            <StackPanel Orientation="Horizontal">
                                <TextBlock Text="{Binding Name}" />
                            </StackPanel>
                        </DataTemplate>
                    </ComboBox.ItemTemplate>
                </ComboBox>
                <Button x:Name="buttonRecognize"
                        Content="Recognize"
                        IsEnabled="False"
                        Margin="50,0,10,0"/>
            </StackPanel>
            <Grid Grid.Row="1">
                <Grid.RowDefinitions>
                    <RowDefinition Height="*"/>
                    <RowDefinition Height="Auto"/>
                </Grid.RowDefinitions>
                <InkCanvas x:Name="inkCanvas"
                           Grid.Row="0"/>
                <TextBlock x:Name="recognitionResult"
                           Grid.Row="1"
                           Margin="50,0,10,0"/>
            </Grid>
        </Grid>
    ```

2. Затем мы задаем некоторые основные реакции на рукописный ввод.

   Элемент [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) настраивается интерпретировать данные, вводимые пером или мышью, как росчерки пера ([**InputDeviceTypes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.inputdevicetypes)). Росчерки пера выводятся на [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) с помощью указанных атрибутов [**InkDrawingAttributes**](https://docs.microsoft.com/windows/desktop/tablet/inkdrawingattributes-class).

   Мы вызываем функцию `InitializeRecognizerList`, чтобы заполнить поле со списком распознавателя списком установленных распознавателей рукописного ввода.

   Мы также объявляем прослушиватели для события нажатия кнопки "Распознать" и для события изменения выбора в поле со списком распознавателя.

    ```csharp
     public MainPage()
     {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

        // Set initial ink stroke attributes.
        InkDrawingAttributes drawingAttributes = new InkDrawingAttributes();
        drawingAttributes.Color = Windows.UI.Colors.Black;
        drawingAttributes.IgnorePressure = false;
        drawingAttributes.FitToCurve = true;
        inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);

        // Populate the recognizer combo box with installed recognizers.
        InitializeRecognizerList();

        // Listen for combo box selection.
        comboInstalledRecognizers.SelectionChanged +=
            comboInstalledRecognizers_SelectionChanged;

        // Listen for button click to initiate recognition.
        buttonRecognize.Click += Recognize_Click;
    }
    ```

3. Мы заполняем поле со списком распознавателя списком установленных распознавателей рукописного ввода.

   Объект [**InkRecognizerContainer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkRecognizerContainer) создается для управления процессом распознавания рукописного ввода. Используйте этот объект, чтобы вызвать [**GetRecognizers**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkrecognizercontainer.getrecognizers) и извлечь список установленных распознавателей для заполнения поля со списком распознавателя.

    ```csharp
    // Populate the recognizer combo box with installed recognizers.
    private void InitializeRecognizerList()
    {
        // Create a manager for the handwriting recognition process.
        inkRecognizerContainer = new InkRecognizerContainer();
        // Retrieve the collection of installed handwriting recognizers.
        IReadOnlyList<InkRecognizer> installedRecognizers =
            inkRecognizerContainer.GetRecognizers();
        // inkRecognizerContainer is null if a recognition engine is not available.
        if (!(inkRecognizerContainer == null))
        {
            comboInstalledRecognizers.ItemsSource = installedRecognizers;
            buttonRecognize.IsEnabled = true;
        }
    }
    ```
    
4. Обновите распознаватель рукописного ввода при изменении выбора в поле со списком распознавателя.

   Используйте [**InkRecognizerContainer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkRecognizerContainer), чтобы вызвать [**SetDefaultRecognizer**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkrecognizercontainer.setdefaultrecognizer) на основе распознавателя, выбранного в поле со списком распознавателя.

    ```csharp
    // Handle recognizer change.
        private void comboInstalledRecognizers_SelectionChanged(
            object sender, SelectionChangedEventArgs e)
        {
            inkRecognizerContainer.SetDefaultRecognizer(
                (InkRecognizer)comboInstalledRecognizers.SelectedItem);
        }
    ```

5. Наконец, мы выполняем распознавание рукописного ввода на основе выбранного распознавателя рукописного ввода. В этом примере для выполнения распознавания рукописного ввода мы используем обработчик событий для нажатия кнопки "Распознать".

   - [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) сохраняет все росчерки пера в объекте [**InkStrokeContainer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer). Росчерки отображаются через свойство [**StrokeContainer**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.strokecontainer)**InkPresenter** и извлекаются методом [**GetStrokes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstrokecontainer.getstrokes).

    ```csharp
    // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes =
            inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
    ```

    - [**Рекогнизеасинк**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkmanager.recognizeasync) вызывается для получения набора объектов [**инкрекогнитионресулт**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkRecognitionResult) .

      Результаты распознавания формируются для каждого слова, обнаруженного [**инкрекогнизер**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkRecognizer).

    ```csharp
    // Recognize all ink strokes on the ink canvas.
    IReadOnlyList<InkRecognitionResult> recognitionResults =
        await inkRecognizerContainer.RecognizeAsync(
            inkCanvas.InkPresenter.StrokeContainer,
            InkRecognitionTarget.All);
    ```

    - Каждый объект [**инкрекогнитионресулт**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkRecognitionResult) содержит набор текстовых кандидатов. Самый верхний элемент в этом списке рассматривается подсистемой распознавания как наилучшее соответствие, за которой следуют оставшиеся кандидаты в порядке уменьшения достоверности.

       Мы перебираем все [**инкрекогнитионресулт**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkRecognitionResult) и компилируем список кандидатов. Затем отображаются кандидаты, а [**инкстрокеконтаинер**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer) очищается (что также очищает [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)).

    ```csharp
    string str = "Recognition result\n";
    // Iterate through the recognition results.
    foreach (InkRecognitionResult result in recognitionResults)
    {
            // Get all recognition candidates from each recognition result.
            IReadOnlyList<string> candidates =
                result.GetTextCandidates();
            str += "Candidates: " + candidates.Count.ToString() + "\n";
            foreach (string candidate in candidates)
            {
                str += candidate + " ";
            }
    }
    // Display the recognition candidates.
    recognitionResult.Text = str;
    // Clear the ink canvas once recognition is complete.
    inkCanvas.InkPresenter.StrokeContainer.Clear();
    ```

    - Ниже приведен пример обработчика щелчка Full.

    ```csharp
    // Handle button click to initiate recognition.
    private async void Recognize_Click(object sender, RoutedEventArgs e)
    {
        // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes =
            inkCanvas.InkPresenter.StrokeContainer.GetStrokes();

        // Ensure an ink stroke is present.
        if (currentStrokes.Count > 0)
        {
            // inkRecognizerContainer is null if a recognition engine is not available.
            if (!(inkRecognizerContainer == null))
            {
                // Recognize all ink strokes on the ink canvas.
                IReadOnlyList<InkRecognitionResult> recognitionResults =
                    await inkRecognizerContainer.RecognizeAsync(
                        inkCanvas.InkPresenter.StrokeContainer,
                        InkRecognitionTarget.All);
                // Process and display the recognition results.
                if (recognitionResults.Count > 0)
                {
                    string str = "Recognition result\n";
                    // Iterate through the recognition results.
                    foreach (InkRecognitionResult result in recognitionResults)
                    {
                        // Get all recognition candidates from each recognition result.
                        IReadOnlyList<string> candidates =
                            result.GetTextCandidates();
                        str += "Candidates: " + candidates.Count.ToString() + "\n";
                        foreach (string candidate in candidates)
                        {
                            str += candidate + " ";
                        }
                    }
                    // Display the recognition candidates.
                    recognitionResult.Text = str;
                    // Clear the ink canvas once recognition is complete.
                    inkCanvas.InkPresenter.StrokeContainer.Clear();
                }
                else
                {
                    recognitionResult.Text = "No recognition results.";
                }
            }
            else
            {
                Windows.UI.Popups.MessageDialog messageDialog =
                    new Windows.UI.Popups.MessageDialog(
                        "You must install handwriting recognition engine.");
                await messageDialog.ShowAsync();
            }
        }
        else
        {
            recognitionResult.Text = "No ink strokes to recognize.";
        }
    }
    ```

## <a name="dynamic-recognition"></a>Динамическое распознавание

Хотя в двух предыдущих примерах пользователю требуется нажать кнопку для запуска распознавания, вы также может выполнять динамическое распознавание, используя ввод росчерков в сочетании с базовой функцией синхронизации.

В этом примере мы будем использовать тот же пользовательский интерфейс и параметры росчерка, что и в предыдущем примере международного распознавания.

1. Эти глобальные объекты ([InkAnalyzer](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalyzer), [InkStroke](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstroke), [InkAnalysisResult](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult), [DispatcherTimer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dispatchertimer)) часто используются в нашем приложении.

    ```csharp
    // Stroke recognition globals.
    InkAnalyzer inkAnalyzer;
    DispatcherTimer recoTimer;
    ```

2. Вместо кнопки для запуска распознавания, мы добавили прослушиватели для двух событий росчерка [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) ([**StrokesCollected**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.strokescollected) и [**StrokeStarted**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstrokeinput.strokestarted)) и настроили основной таймер ([**DispatcherTimer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DispatcherTimer)) на секундный интервал [**Tick**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dispatchertimer.tick).

    ```csharp
    public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

        // Listen for stroke events on the InkPresenter to 
        // enable dynamic recognition.
        // StrokesCollected is fired when the user stops inking by 
        // lifting their pen or finger, or releasing the mouse button.
        inkCanvas.InkPresenter.StrokesCollected += inkCanvas_StrokesCollected;
        // StrokeStarted is fired when ink input is first detected.
        inkCanvas.InkPresenter.StrokeInput.StrokeStarted +=
            inkCanvas_StrokeStarted;

        inkAnalyzer = new InkAnalyzer();

        // Timer to manage dynamic recognition.
        recoTimer = new DispatcherTimer();
        recoTimer.Interval = TimeSpan.FromSeconds(1);
        recoTimer.Tick += recoTimer_TickAsync;
    }
    ```

3. Затем мы определяем обработчики событий InkPresenter, которые мы объявили на первом шаге (мы также переопределим событие страницы [**OnNavigatingFrom**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatingfrom) для управления таймером).

    - [**StrokesCollected**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.strokescollected)  
    Добавьте росчерки пера ([**AddDataForStrokes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.adddataforstrokes)) в InkAnalyzer и запустите таймер распознавания, когда пользователь прерывает рукописный ввод, поднимая перо или палец либо отпуская кнопку мыши. Распознавание вызывается через одну секунду отсутствия рукописного ввода.  

        Используйте метод [**SetStrokeDataKind**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.setstrokedatakind), чтобы указать, интересует ли вас только текст (в том числе структура документа и маркированные списки) или только рисунки (в том числе для распознавания фигур).

    - [**StrokeStarted**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstrokeinput.strokestarted)  
    Если новый росчерк начинается до следующего события такта таймера, останавливать таймер, так как новый росчерк — это, скорее всего, продолжение одной рукописной записи.

    ```csharp
    // Handler for the InkPresenter StrokeStarted event.
    // Don't perform analysis while a stroke is in progress.
    // If a new stroke starts before the next timer tick event,
    // stop the timer as the new stroke is likely the continuation
    // of a single handwriting entry.
    private void inkCanvas_StrokeStarted(InkStrokeInput sender, PointerEventArgs args)
    {
        recoTimer.Stop();
    }
    // Handler for the InkPresenter StrokesCollected event.
    // Stop the timer and add the collected strokes to the InkAnalyzer.
    // Start the recognition timer when the user stops inking (by 
    // lifting their pen or finger, or releasing the mouse button).
    // If ink input is not detected after one second, initiate recognition.
    private void inkCanvas_StrokesCollected(InkPresenter sender, InkStrokesCollectedEventArgs args)
    {
        recoTimer.Stop();
        // If you're only interested in a specific type of recognition,
        // such as writing or drawing, you can constrain recognition 
        // using the SetStrokDataKind method, which can improve both 
        // efficiency and recognition results.
        // In this example, "InkAnalysisStrokeKind.Writing" is used.
        foreach (var stroke in args.Strokes)
        {
            inkAnalyzer.AddDataForStroke(stroke);
            inkAnalyzer.SetStrokeDataKind(stroke.Id, InkAnalysisStrokeKind.Writing);
        }
        recoTimer.Start();
    }
    // Override the Page OnNavigatingFrom event handler to 
    // stop our timer if user leaves page.
    protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
    {
        recoTimer.Stop();
    }
    ```

4. Наконец, мы выполняем распознавание рукописного ввода. В этом примере мы используем обработчик событий [**Tick**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dispatchertimer.tick) элемента [**DispatcherTimer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DispatcherTimer) для вызова функции распознавания рукописного ввода.
    - Вызовите [**AnalyzeAsync**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.AnalyzeAsync) для запуска анализа рукописного ввода и получения [**InkAnalysisResult**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult).
    - Если [**Status**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult.Status) возвращает состояние **Updated**, вызовите [**FindNodes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalysisroot.findnodes) для узлов типа [**InkAnalysisNodeKind.InkWord**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalysisnodekind).
    - Теперь пройдемся по узлам и покажем распознанный текст.
    - И, наконец, удалите распознанные узлы из InkAnalyzer и соответствующие росчерки с холста рукописного ввода.

    ```csharp
    private async void recoTimer_TickAsync(object sender, object e)
    {
        recoTimer.Stop();
        if (!inkAnalyzer.IsAnalyzing)
        {
            InkAnalysisResult result = await inkAnalyzer.AnalyzeAsync();

            // Have ink strokes on the canvas changed?
            if (result.Status == InkAnalysisStatus.Updated)
            {
                // Find all strokes that are recognized as handwriting and 
                // create a corresponding ink analysis InkWord node.
                var inkwordNodes =
                    inkAnalyzer.AnalysisRoot.FindNodes(
                        InkAnalysisNodeKind.InkWord);

                // Iterate through each InkWord node.
                // Display the primary recognized text (for this example, 
                // we ignore alternatives), and then delete the 
                // ink analysis data and recognized strokes.
                foreach (InkAnalysisInkWord node in inkwordNodes)
                {
                    string recognizedText = node.RecognizedText;
                    // Display the recognition candidates.
                    recognitionResult.Text = recognizedText;

                    foreach (var strokeId in node.GetStrokeIds())
                    {
                        var stroke =
                            inkCanvas.InkPresenter.StrokeContainer.GetStrokeById(strokeId);
                        stroke.Selected = true;
                    }
                    inkAnalyzer.RemoveDataForStrokes(node.GetStrokeIds());
                }
                inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();
            }
        }
        else
        {
            // Ink analyzer is busy. Wait a while and try again.
            recoTimer.Start();
        }
    }
    ```

## <a name="related-articles"></a>Похожие статьи

- [Взаимодействие с помощью пера](pen-and-stylus-interactions.md)

### <a name="topic-samples"></a>Примеры в статье

- [Пример анализа рукописного ввода (базовый) (C#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-analysis-basic.zip)
- [Пример распознавания рукописного ввода (C#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-handwriting-reco.zip)

### <a name="other-samples"></a>Другие примеры

- [Простой пример рукописного ввода (C#/C++)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk)
- [Сложный пример рукописного ввода (C++)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk)
- [Пример рукописного ввода (JavaScript)](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BJavaScript%5D-Windows%208%20app%20samples/JavaScript/Windows%208%20app%20samples/Input%20Ink%20sample%20(Windows%208))
- [Руководство по началу работы. Поддержка рукописного ввода в приложении Windows](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-Ink)
- [Пример раскраски](https://github.com/Microsoft/Windows-appsample-coloringbook)
- [Пример семейных заметок](https://github.com/Microsoft/Windows-appsample-familynotes)
