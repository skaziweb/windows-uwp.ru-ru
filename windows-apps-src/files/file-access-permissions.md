---
ms.assetid: 3A404CC0-A997-45C8-B2E8-44745539759D
title: Разрешения на доступ к файлам
description: Приложения могут иметь доступ к определенным расположениям в файловой системе по умолчанию. Приложения также могут получить доступ к дополнительным расположениям через средство выбора файлов или с помощью объявления возможностей.
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
- javascript
ms.openlocfilehash: b9529632fe582da438d6d17e31ffbdd963714a7c
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "81608649"
---
# <a name="file-access-permissions"></a>Разрешения на доступ к файлам

Приложения UWP могут иметь доступ к определенным расположениям в файловой системе по умолчанию. Приложения также могут получить доступ к дополнительным расположениям через средство выбора файлов или с помощью объявления возможностей.

## <a name="the-locations-that-all-apps-can-access"></a>Расположения, доступные всем приложениям

Создавая новое приложение, вы по умолчанию получаете доступ к следующим расположениям в файловой системе:

### <a name="application-install-directory"></a>Папка установки приложения
Папка, в которую устанавливается ваше приложение в системе пользователя.

Существуют два основных способа получения доступа к файлам и папкам в папке установки вашего приложения.

1. Так вы можете получить класс [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder), соответствующий папке установки вашего приложения:

    ```csharp
    Windows.Storage.StorageFolder installedLocation = Windows.ApplicationModel.Package.Current.InstalledLocation;
    ```
    
    ```javascript
    var installDirectory = Windows.ApplicationModel.Package.current.installedLocation;
    ```
    
    ```cppwinrt
    #include <winrt/Windows.Storage.h>
    ...
    Windows::Storage::StorageFolder installedLocation{ Windows::ApplicationModel::Package::Current().InstalledLocation() };
    ```
    
    ```cpp
    Windows::Storage::StorageFolder^ installedLocation = Windows::ApplicationModel::Package::Current->InstalledLocation;
    ```

    Затем вы можете получить доступ к файлам и папкам каталога с помощью методов [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder). В примере этот класс **StorageFolder** хранится в переменной `installDirectory`. Вы можете узнать больше о работе с пакетом приложения и папкой установки из [примера сведений о пакете приложения](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Package) на сайте GitHub.

2. Так вы можете загрузить файл непосредственно из каталога установки приложения с помощью URI приложения:

    ```csharp
    using Windows.Storage;            
    StorageFile file = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///file.txt"));
    ```
    
    ```javascript
    Windows.Storage.StorageFile.getFileFromApplicationUriAsync("ms-appx:///file.txt").done(
        function(file) {
            // Process file
        }
    );
    ```
    
    ```cppwinrt
    Windows::Foundation::IAsyncAction ExampleCoroutineAsync()
    {
        Windows::Storage::StorageFile file{
            co_await Windows::Storage::StorageFile::GetFileFromApplicationUriAsync(Windows::Foundation::Uri{L"ms-appx:///file.txt"})
        };
        // Process file
    }
    ```
    
    ```cpp
    auto getFileTask = create_task(StorageFile::GetFileFromApplicationUriAsync(ref new Uri("ms-appx:///file.txt")));
    getFileTask.then([](StorageFile^ file) 
    {
        // Process file
    });
    ```

    После завершения метод [**GetFileFromApplicationUriAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefromapplicationuriasync) возвращает класс [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile), который представляет файлу `file.txt` в папке установки приложения (`file` в примере).
    
    Префикс «ms-appx:///» в универсальном коде ресурса относится к папке установки приложения. Дополнительные сведения об использовании универсальных кодов ресурсов: [Добавление ссылок на содержимое с помощью URI](https://docs.microsoft.com/previous-versions/windows/apps/hh781215(v=win.10)).

Кроме того, доступ к файлам в каталоге установки приложения (в отличие от иных расположений) также можно получить, используя некоторые [модели Win32 и COM для приложений универсальной платформы Windows (UWP)](https://docs.microsoft.com/uwp/win32-and-com/win32-and-com-for-uwp-apps) и [функции стандартной библиотеки C/C++ из Microsoft Visual Studio](https://docs.microsoft.com/cpp/cpp/c-cpp-language-and-standard-libraries).

Папка установки приложения предназначена только для чтения. Невозможно получить доступ к папке установки с помощью средства выбора файлов.

### <a name="application-data-locations"></a>Расположения данных приложения
Папки, в которых ваше приложение может хранить данные. Эти папки (локальная, перемещаемая и временная) создаются при установке вашего приложения.

Существуют два основных способа получения доступа к файлам и папкам из расположения данных вашего приложения.

1.  Используйте свойства [**ApplicationData**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData), чтобы получить папку данных приложения.

    Например, вы можете использовать [**ApplicationData**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData).[**LocalFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localfolder), чтобы получить класс [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder), соответствующий локальной папке вашего приложения, следующим образом:
    
    ```csharp
    using Windows.Storage;
    StorageFolder localFolder = ApplicationData.Current.LocalFolder;
    ```
    
    ```javascript
    var localFolder = Windows.Storage.ApplicationData.current.localFolder;
    ```
    
    ```cppwinrt
    Windows::Storage::StorageFolder storageFolder{
        Windows::Storage::ApplicationData::Current().LocalFolder()
    };
    ```
    
    ```cpp
    using namespace Windows::Storage;
    StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
    ```
    
    Если вы хотите получить доступ к перемещаемой или временной папке вашего приложения, вместо этого используйте свойство [**RoamingFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingfolder) или [**TemporaryFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.temporaryfolder).
    
    Загрузив класс [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder), соответствующий расположению данных приложения, вы получите доступ к файлам и папкам этого расположения с помощью методов **StorageFolder**. В примере эти объекты **StorageFolder** хранятся в переменной `localFolder`. Больше об использовании расположений данных приложений можно узнать в рекомендации на странице [Класс ApplicationData](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata), а также скачав [пример данных приложения](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ApplicationData) с сайта GitHub.

2. Вы можете получить файл непосредственно из локальной папки своего приложения с помощью универсального кода ресурса (URI) приложения, как показано ниже.
    
    ```csharp
    using Windows.Storage;
    StorageFile file = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appdata:///local/file.txt"));
    ```
    
    ```javascript
    Windows.Storage.StorageFile.getFileFromApplicationUriAsync("ms-appdata:///local/file.txt").done(
        function(file) {
            // Process file
        }
    );
    ```
    
    ```cppwinrt
    Windows::Storage::StorageFile file{
        co_await Windows::Storage::StorageFile::GetFileFromApplicationUriAsync(Windows::Foundation::Uri{ L"ms-appdata:///local/file.txt" })
    };
    // Process file
    ```
    
    ```cpp
    using Windows::Storage;
    auto getFileTask = create_task(StorageFile::GetFileFromApplicationUriAsync(ref new Uri("ms-appdata:///local/file.txt")));
    getFileTask.then([](StorageFile^ file) 
    {
        // Process file
    });
    ```
    
    После завершения метод [**GetFileFromApplicationUriAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefromapplicationuriasync) возвращает класс [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile), представляющий файл `file.txt` в локальной папке приложения (`file` в примере).
    
    Префикс «ms-appdata:///local/» в универсальном коде ресурса относится к локальной папке приложения. Чтобы получить доступ к файлам в перемещаемой или временной папке приложения, используйте вместо этого «ms-appdata:///roaming/» или «ms-appdata:///temporary/». Дополнительные сведения об использовании универсальных кодов ресурсов приложения см. в разделе [Загрузка файловых ресурсов](https://docs.microsoft.com/previous-versions/windows/apps/hh781229(v=win.10)).

Кроме того, доступ к файлам в каталоге установки приложения (в отличие от иных расположений) также можно получить, используя некоторые [модели Win32 и COM для приложений UWP](https://docs.microsoft.com/uwp/win32-and-com/win32-and-com-for-uwp-apps) и функции стандартной библиотеки C/C++ из Visual Studio.

Невозможно получить доступ к локальной, перемещаемой и временной папкам с помощью средства выбора файлов.

### <a name="removable-devices"></a>Съемные устройства

Кроме того, ваше приложение по умолчанию имеет доступ к некоторым файлам на подключенных устройствах. Этот вариант подходит, если ваше приложение использует [расширение "Автозапуск"](https://docs.microsoft.com/previous-versions/windows/apps/hh464906(v=win.10)), чтобы запускаться автоматически при подключении к системе устройства, например камеры или USB-устройства флэш-памяти. Доступ вашего приложения к файлам ограничен определенными типами файлов, указанными с помощью объявлений сопоставлений типов файлов в манифесте приложения.

Конечно, вы также можете получить доступ к файлам и папкам на съемном устройстве, вызвав средство выбора файлов (с помощью [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) и [**FolderPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FolderPicker)) и позволив пользователю выбирать файлы или папки, предоставляя приложению доступ к ним. Сведения об использовании средства выбора файлов см. в статье [Открытие файлов и папок с помощью средства выбора](quickstart-using-file-and-folder-pickers.md).

> [!NOTE]
> Подробнее о доступе к SD-карте или другим съемным устройствам рассказывается в статье [Доступ к SD-карте](access-the-sd-card.md).

## <a name="locations-that-uwp-apps-can-access"></a>Расположения, доступные приложениям UWP

### <a name="users-downloads-folder"></a>Папка скачиваемых файлов пользователя

Папка, в которой по умолчанию сохраняются скачиваемые файлы.

По умолчанию ваше приложение имеет доступ к файлам и папкам только в папке скачиваемых файлов пользователя, созданной этим приложением. Однако вы можете получить доступ к файлам и папкам в папке скачиваемых файлов пользователя, вызвав средство выбора файлов ([**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) или [**FolderPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FolderPicker)), чтобы пользователь мог осуществлять навигацию и выбирать файлы или папки, предоставляя приложению доступ к ним.

- Вы можете создать файл в папке скачиваемых файлов пользователя следующим образом:

    ```csharp
    using Windows.Storage;
    StorageFile newFile = await DownloadsFolder.CreateFileAsync("file.txt");
    ```
    
    ```javascript
    Windows.Storage.DownloadsFolder.createFileAsync("file.txt").done(
        function(newFile) {
            // Process file
        }
    );
    ```
    
    ```cppwinrt
    Windows::Storage::StorageFile newFile{
        co_await Windows::Storage::DownloadsFolder::CreateFileAsync(L"file.txt")
    };
    // Process file
    ```
    
    ```cpp
    using Windows::Storage;
    auto createFileTask = create_task(DownloadsFolder::CreateFileAsync(L"file.txt"));
    createFileTask.then([](StorageFile^ newFile)
    {
        // Process file
    });
    ```

    [**DownloadsFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.DownloadsFolder).[**CreateFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.downloadsfolder.createfileasync) перегружен таким образом, чтобы вы могли указать системе алгоритм действий, если в папке скачиваемых файлов уже существует файл с таким же именем. После завершения эти методы возвращают класс [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile), соответствующий созданному файлу. В примере этот файл назван `newFile`.

- Вы можете создать вложенную папку в папке скачиваемых файлов пользователя следующим образом:

    ```csharp
    using Windows.Storage;
    StorageFolder newFolder = await DownloadsFolder.CreateFolderAsync("New Folder");
    ```
    
    ```javascript
    Windows.Storage.DownloadsFolder.createFolderAsync("New Folder").done(
        function(newFolder) {
            // Process folder
        }
    );
    ```
    
    ```cppwinrt
    Windows::Storage::StorageFolder newFolder{
        co_await Windows::Storage::DownloadsFolder::CreateFolderAsync(L"New Folder")
    };
    // Process folder
    ```
    
    ```cpp
    using Windows::Storage;
    auto createFolderTask = create_task(DownloadsFolder::CreateFolderAsync(L"New Folder"));
    createFolderTask.then([](StorageFolder^ newFolder)
    {
        // Process folder
    });
    ```

    [**DownloadsFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.DownloadsFolder).[**CreateFolderAsync**](https://docs.microsoft.com/uwp/api/windows.storage.downloadsfolder.createfolderasync) перегружен таким образом, чтобы вы могли указать системе алгоритм действий, если в папке скачиваемых файлов уже существует вложенная папка с таким же именем. После завершения эти методы возвращают класс [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder), соответствующий созданной вложенной папке. В примере этот файл назван `newFolder`.

Если вы создаете файл или папку в папке скачиваемых файлов, мы рекомендуем добавить этот элемент в [**FutureAccessList**](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) вашего приложения, чтобы в дальнейшем оно могло быстро получать доступ к этому элементу.

## <a name="accessing-additional-locations"></a>Доступ к дополнительным расположениям

Кроме расположений по умолчанию, приложение может получить доступ к дополнительным файлам и папкам с помощью [объявления возможностей в манифесте приложения](../packaging/app-capability-declarations.md) или с помощью [вызова средства выбора файлов](quickstart-using-file-and-folder-pickers.md), что позволяет пользователю выбирать файлы и папки, предоставляя приложению доступ к ним.

Приложения, объявляющие расширение [AppExecutionAlias](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-appexecutionalias), имеют разрешения файловой системы начиная с каталога, откуда они запускаются в консольном окне, и далее вниз по иерархии.

В таблице ниже перечислены дополнительные расположения, к которым можно получить доступ, объявив возможности и используя связанные API [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage).

| Расположение | Возможности | API Windows.Storage |
|----------|------------|---------------------|
| Все файлы, к которым у пользователя имеется доступ. Например: документы, изображения, фотографии, скачанные файлы, рабочий стол, OneDrive и т. д. | **broadFileSystemAccess**<br><br>Это ограниченная возможность. Доступ настраивается в разделе **Параметры** > **Конфиденциальность** > **Файловая система**. Так как пользователи могут предоставить или отменить разрешение в любой момент в разделе **Параметры**, следует убедиться, что приложение устойчиво к этим изменениям. Если вы обнаружили, что у приложения нет доступа, вы можете предложить пользователю изменить этот параметр, предоставив ссылку на статью [Доступ к файловой системе и конфиденциальность в Windows 10](https://support.microsoft.com/help/4468237/windows-10-file-system-access-and-privacy-microsoft-privacy). Обратите внимание на то, что пользователь должен закрыть приложение, изменить параметр и перезапустить приложение. Если он изменит параметр во время работы приложения, платформа приостановит ваше приложение, чтобы можно было сохранить состояние, а затем принудительно завершит его, чтобы применить новый параметр. В обновлении за апрель 2018 года по умолчанию это разрешение включено. В обновлении октябрь 2018 по умолчанию оно отключено.<br /><br />Если приложение отправляется в Store, объявляющий эту возможность, потребуется дополнительно обосновать, зачем приложению нужна эта возможность и как планируется ее использовать.<br/><br/>Эта возможность подходит для интерфейсов API в пространстве имен [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage). В разделе **Пример** в конце этой статьи показано, как включить эту возможность в приложении.<br/><br/>**Примечание**. Эта возможность не поддерживается в Xbox. | Неприменимо |
| Documents | **documentsLibrary**<br><br>Примечание. Необходимо добавить сопоставления типов файлов в манифест приложения, в котором указаны конкретные типы файлов, доступные приложению в этом расположении. <br><br>Используйте данную возможность, если ваше приложение:<br>- поддерживает кросс-платформенный автономный доступ к конкретному содержимому OneDrive, используя допустимые URL-адреса OneDrive или идентификаторы ресурсов;<br>- автоматически сохраняет открытые файлы в OneDrive пользователя при работе в автономном режиме. | [KnownFolders.DocumentsLibrary](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.documentslibrary) |
| Музыка     | **musicLibrary** <br>См. также статью [Файлы и папки в библиотеках музыки, изображений и видео](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md) | [KnownFolders.MusicLibrary](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.musiclibrary) |    
| Изображения  | **picturesLibrary**<br> См. также статью [Файлы и папки в библиотеках музыки, изображений и видео](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md) | [KnownFolders.PicturesLibrary](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.pictureslibrary) |  
| Видео    | **videosLibrary**<br>См. также статью [Файлы и папки в библиотеках музыки, изображений и видео](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md) | [KnownFolders.VideosLibrary](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.videoslibrary) |   
| Съемные устройства  | **removableStorage**  <br><br>Примечание. Необходимо добавить сопоставления типов файлов в манифест приложения, в котором указаны конкретные типы файлов, доступные приложению в этом расположении. <br><br>См. также статью [Получение доступа к SD-карте](access-the-sd-card.md) | [KnownFolders.RemovableDevices](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.removabledevices) |  
| Библиотеки домашней группы  | Требуется по меньшей мере одна из следующих возможностей: <br>- **musicLibrary** <br>- **picturesLibrary** <br>- **videosLibrary** | [KnownFolders.HomeGroup](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.homegroup) |      
| Устройства сервера мультимедиа (DLNA) | Требуется по меньшей мере одна из следующих возможностей: <br>- **musicLibrary** <br>- **picturesLibrary** <br>- **videosLibrary** | [KnownFolders.MediaServerDevices](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.mediaserverdevices) |
| Папки UNC | Требуется комбинация следующих возможностей: <br><br>Возможность домашней и рабочей сетей: <br>- **privateNetworkClientServer** <br><br>Добавьте по меньшей мере одну возможность общедоступных сетей и Интернета: <br>- **internetClient** <br>- **internetClientServer** <br><br>Если применимо, добавьте возможность учетных данных домена:<br>- **enterpriseAuthentication** <br><br>**Примечание**. Необходимо добавить сопоставления типов файлов в манифест приложения, в котором указаны конкретные типы файлов, доступные приложению в этом расположении. | Получение папки с помощью: <br>[StorageFolder.GetFolderFromPathAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfolderfrompathasync) <br><br>Получение файла с помощью: <br>[StorageFile.GetFileFromPathAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefrompathasync) |

### <a name="example"></a>Пример

В этом примере добавляется возможность с ограниченным доступом **broadFileSystemAccess**. Нужно не только указать возможность, но и добавить пространство имен `rescap`, а также добавить его в `IgnorableNamespaces`.

```xaml
<Package
  ...
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
  IgnorableNamespaces="uap mp rescap">
...
<Capabilities>
    <rescap:Capability Name="broadFileSystemAccess" />
</Capabilities>
```

> [!NOTE]
> Полный список возможностей приложения приведен в разделе [Объявления возможностей приложения](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations).
