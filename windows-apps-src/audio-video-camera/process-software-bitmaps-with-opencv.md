---
ms.assetid: ''
description: В этой статье рассказывается, как использовать класс SoftwareBitmap с библиотекой компьютерного зрения с открытым исходным кодом (OpenCV).
title: Обработка точечных рисунков с помощью OpenCV
ms.date: 03/19/2018
ms.topic: article
keywords: windows 10, uwp, opencv, softwarebitmap
ms.localizationpriority: medium
ms.openlocfilehash: 823468f7d18dcfb4c9379a981d6c2da7a250fe22
ms.sourcegitcommit: ef723e3d6b1b67213c78da696838a920c66d5d30
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/02/2020
ms.locfileid: "82730317"
---
# <a name="process-bitmaps-with-opencv"></a>Обработка точечных рисунков с помощью OpenCV

В этой статье объясняется, как использовать класс **[софтваребитмап](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)** , который используется многими различными среда выполнения Windows API-интерфейсами для представления изображений с помощью библиотеки компьютерное зрение с открытым исходным кодом (opencv), библиотеки машинного кода, предоставляющей широкий спектр алгоритмов обработки изображений. 

Примеры в этой статье посвящены созданию собственного кода среда выполнения Windows компонента, который можно использовать из приложения UWP, включая приложения, созданные с помощью C#. Этот вспомогательный компонент будет предоставлять лишь один метод **Blur**, который будет использовать функции обработки OpenCV для размытия изображения. Этот компонент реализует частные методы, позволяющие получать указатель на базовый буфер данных изображений, который можно использовать непосредственно в библиотеке OpenCV, что упрощает развитие вспомогательного компонента для реализации других функций обработки OpenCV. 

* Введение в использование **SoftwareBitmap** см. в разделе [Создание, редактирование и сохранение растровых изображений](imaging.md). 
* Чтобы узнать, как использовать библиотеку OpenCV, перейдите по [https://opencv.org](https://opencv.org)адресу.
* Чтобы узнать, как использовать описанный в этой статье вспомогательный компонент OpenCV вместе с **[MediaFrameReader](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader)** для реализации обработки изображений на поступающих с камеры кадрах в режиме реального времени, см. раздел [Использование OpenCV с MediaFrameReader](use-opencv-with-mediaframereader.md).
* Полный пример кода, реализующий некоторые различные эффекты, см. в разделе [Пример кадров камеры + OpenCV](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraOpenCV) в репозитории универсальных примеров Windows на GitHub.

> [!NOTE] 
> Прием, используемый компонентом OpenCVHelper, подробно описанный в этом разделе, требует, чтобы данные изображения, которые необходимо обработать, находились в памяти ЦП, а не в памяти графического процессора. Таким образом, для API-интерфейсов, позволяющих запрашивать расположение изображений в памяти, таких как класс **[MediaCapture](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture)**, необходимо указать память ЦП.

## <a name="create-a-helper-windows-runtime-component-for-opencv-interop"></a>Создание вспомогательного компонента среда выполнения Windows для взаимодействия OpenCV

### <a name="1-add-a-new-native-code-windows-runtime-component-project-to-your-solution"></a>1. Добавление нового собственного кода среда выполнения Windows проекта компонента в решение

1. Добавьте новый проект в свое решение в Visual Studio, щелкнув правой кнопкой мыши по решению в Обозревателе решений и выбрав **Добавить -> Новый проект**. 
2. В категории **Visual C++** выберите **Windows Runtime Component (Universal Windows)**. Для этого примера назовите проект OpenCVBridge и нажмите **ОК**. 
3. В диалоговом окне **Новый проект Windows Universal** выберите целевую и минимальную версии ОС для приложения и нажмите кнопку **ОК**.
4. Щелкните правой кнопкой мыши по автоматически созданному файлу Class1.cpp в Обозревателе решений и выберите **Удалить**; когда появится диалоговое окно подтверждения, выберите **Удалить**. Удалите файл заголовка Class1.h.
5. Щелкните правой кнопкой мыши значок проекта OpenCVBridge и выберите **Добавить -> Класс...**. В диалоговом окне **Добавить класс** введите OpenCVHelper в поле **Имя класса**, а затем нажмите **OK**. Код добавляется в файлы созданного класса позднее.

### <a name="2-add-the-opencv-nuget-packages-to-your-component-project"></a>2. Добавление пакетов OpenCV NuGet в проект компонента

1. В Обозревателе решений щелкните правой кнопкой мыши значок проекта OpenCVBridge и выберите **Управление пакетами NuGet...**
2. Когда откроется диалоговое окно "Диспетчер пакетов Nuget", выберите вкладку **Обзор** и введите в поле поиска OpenCV.Win.
3. Выберите OpenCV.Win.Core и нажмите **Установить**. В диалоговом окне **Предварительный просмотр** нажмите **ОК**.
4. Используйте эту же процедуру для установки пакета OpenCV.Win.ImgProc.

>[!NOTE]
>OpenCV. Win. Core и OpenCV. Win. Имгпрок не обновляются регулярно и не проходят проверки соответствия магазина, поэтому эти пакеты предназначены только для экспериментов.

### <a name="3-implement-the-opencvhelper-class"></a>3. Реализация класса OpenCVHelper

Вставьте следующий код в файл заголовка OpenCVHelper.h. Этот код включает в себя файлы заголовков OpenCV для пакетов *Core* и *ImgProc*, которые мы установили, и объявляет три метода, которые будут показаны на следующих этапах.

[!code-cpp[OpenCVHelperHeader](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.h#SnippetOpenCVHelperHeader)]

Удалите существующее содержимое файла OpenCVHelper.cpp и добавьте следующие директивы include. 

[!code-cpp[OpenCVHelperInclude](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperInclude)]

После директив include добавьте следующие директивы **using**. 

[!code-cpp[OpenCVHelperUsing](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperUsing)]

Затем добавьте метод **GetPointerToPixelData** в OpenCVHelper.cpp. Этот метод принимает **[SoftwareBitmap](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)** и путем ряда преобразований получает представление COM-интерфейса для пиксельных данных, с помощью которого мы можем получить указатель на базовый буфер данных как массив **char**. 

Первый **[BitmapBuffer](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapbuffer)**, содержащий пиксельные данные, можно получить путем вызова **[LockBuffer](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.lockbuffer)**, запрашивая буфер чтения и записи, чтобы библиотека OpenCV могла изменять пиксельные данные.  **[CreateReference](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapbuffer.CreateReference)** вызывается для получения объекта **[IMemoryBufferReference](https://docs.microsoft.com/uwp/api/windows.foundation.imemorybufferreference)**. Далее интерфейс **IMemoryBufferByteAccess** приводится к типу **IInspectable**, базовому интерфейсу всех классов среды выполнения Windows, и вызывается **[QueryInterface](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))** для получения COM-интерфейса **[IMemoryBufferByteAccess](https://docs.microsoft.com/previous-versions/mt297505(v=vs.85))**, который позволит нам получить буфер пиксельных данных как массив **char**. Наконец, заполните массив **char**, вызвав метод **[IMemoryBufferByteAccess::GetBuffer](https://docs.microsoft.com/windows/desktop/WinRT/imemorybufferbyteaccess-getbuffer)**. Если какие-либо действия преобразования в этом методе завершатся сбоем, метод возвращает **false**, сообщая о том, что продолжить дальнейшую обработку не удастся.

[!code-cpp[OpenCVHelperGetPointerToPixelData](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperGetPointerToPixelData)]

Затем добавьте метод **TryConvert**, как показано ниже. Этот метод принимает **SoftwareBitmap** и пытается преобразовать его в объект **Mat**, то есть в объект матрицы, который OpenCV использует для представления буферов данных изображений. Этот метод вызывает метод **GetPointerToPixelData**, описанный выше, чтобы получить представление массива **char** для буфера пиксельных данных. В случае успеха вызывается конструктор класса **Mat** с передачей ему ширины и высоты в пикселях, полученных из исходного объекта **SoftwareBitmap**. 

> [!NOTE] 
> В этом примере задается константа CV_8UC4 как формат в пикселях для созданного **Mat**. Это означает, что для работы в этом примере объект **SoftwareBitmap**, переданный в этот метод, должен иметь для свойства **[BitmapPixelFormat](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.BitmapPixelFormat)** значение **[BGRA8](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapPixelFormat)** с предварительным умножением альфа-канала, что является эквивалентом CV_8UC4.

Из метода возвращается пустая копия созданного объекта **Mat**, чтобы дальнейшая обработка воздействовала на тот же буфер пиксельных данных, на который ссылается **SoftwareBitmap**, а не на копию этого буфера.

[!code-cpp[OpenCVHelperTryConvert](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperTryConvert)]

Наконец, вспомогательный класс в этом примере реализует один метод обработки изображения **Blur**, который просто использует метод **TryConvert**, описанный выше, чтобы получить объект **Mat**, представляющий исходный точечный рисунок и целевой точечный рисунок для операции размытия, а затем вызывает метод **Blur** из библиотеки OpenCV ImgProc. Другой параметр для **Blur** задает размер эффекта размытия в направлениях X и Y.

[!code-cpp[OpenCVHelperBlur](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperBlur)]


## <a name="a-simple-softwarebitmap-opencv-example-using-the-helper-component"></a>Простой пример SoftwareBitmap OpenCV с использованием вспомогательного компонента
После создания компонента OpenCVBridge можно создать простое приложение на C#, использующее OpenCV-метод **Blur** для изменения **SoftwareBitmap**. Чтобы получить доступ к компоненту среда выполнения Windows из приложения UWP, необходимо сначала добавить ссылку на этот компонент. В Обозревателе решений щелкните правой кнопкой мыши по узлу **Ссылки** в проекте приложения UWP и выберите **Добавить ссылку...**. В диалоговом окне Диспетчера ссылок выберите **Проекты -> Решение**. Установите флажок рядом с проектом OpenCVBridge и нажмите **ОК**.

В примере кода ниже пользователь выбирает файл изображения, а затем для создания представления **SoftwareBitmap** этого изображения используется **[BitmapDecoder](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder)**. Дополнительные сведения о работе с **SoftwareBitmap** см. в разделе [Создание, редактирование и сохранение растровых изображений](https://docs.microsoft.com/windows/uwp/audio-video-camera/imaging).

Как отмечено ранее в этой статье, класс **OpenCVHelper** требует, чтобы все предоставляемые изображения **SoftwareBitmap** были закодированы в пиксельном формате BGRA8 с предварительным умножением альфа-канала; поэтому, если изображение еще не представлено в этом формате, в примере кода вызывается **[Convert](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.BitmapAlphaMode)**, чтобы преобразовать изображение в ожидаемый формат.

Затем создается **SoftwareBitmap** для использования в качестве целевого объекта операции размытия. Свойства исходного изображения используются в качестве аргументов для конструктора в целях создания точечного рисунка соответствующего формата.

Создается новый экземпляр **OpenCVHelper**, и вызывается метод **Blur** с передачей ему исходного и целевого точечных рисунков. Наконец, создается **SoftwareBitmapSource**, чтобы назначить выходное изображение элементу управления **Image** на XAML.

В этом примере кода используются API из следующих пространств имен в дополнение к пространствам имен, входящим в шаблон проекта по умолчанию.

[!code-cs[OpenCVMainPageUsing](./code/ImagingWin10/cs/MainPage.OpenCV.xaml.cs#SnippetOpenCVMainPageUsing)]

[!code-cs[OpenCVBlur](./code/ImagingWin10/cs/MainPage.OpenCV.xaml.cs#SnippetOpenCVBlur)]

## <a name="related-topics"></a>Связанные темы

* [Справочник по параметрам BitmapEncoder](bitmapencoder-options-reference.md)
* [Метаданные изображений](image-metadata.md)
 

 




