---
author: awkoren
Description: Развертывание и отладка приложений универсальной платформы Windows (UWP), преобразованных из классических приложений для Windows (Win32, WPF и формы Windows Forms) с помощью расширений для преобразования классических приложений.
Search.Product: eADQiWindows 10XVcnh
title: Развертывание и отладка приложений универсальной платформы Windows (UWP), преобразованных из классических приложений для Windows
---

# Развертывание и отладка преобразованного приложения UWP (Project Centennial)

\[Некоторые сведения относятся к предварительным версиям продуктов, в которые перед коммерческим выпуском могут быть внесены существенные изменения. Майкрософт не дает никаких гарантий, прямых или косвенных, в отношении указанной здесь информации.\]

Этот раздел содержит информацию, которая поможет вам успешно разворачивать и выполнять отладку своих приложений после их преобразования. Этот раздел также подойдет вам, если вам интересны какие-либо подробные сведения о расширениях для преобразования классических приложений.

## Отладка преобразованных приложений UWP

Есть 2 основных варианта отладки преобразованного приложения с помощью Visual Studio.

### Прикрепление к процессу

Когда Microsoft Visual Studio выполняется «от имени администратора», команды __Начать отладку__ и __Запуск без отладки__ будут работать для преобразованного проекта, однако запущенное приложение будет выполняться на [среднем уровне целостности](https://msdn.microsoft.com/library/bb625963). Это значит, что у приложения _не_ будет более высокого уровня привилегий. Чтобы предоставить права администратора запущенному приложению, сначала необходимо запустить приложение «от имени администратора» с помощью ярлыка или плитки. После запуска приложения из экземпляра Microsoft Visual Studio, выполняющегося «от имени администратора», вызовите команду __Прикрепить к процессу__ и выберите процесс приложения в диалоговом окне.

### Отладка с помощью клавиши F5

Теперь Visual Studio поддерживает новый пакетный проект, который позволяет автоматически копировать все обновления, вносимые при создании вашего приложения, в пакет AppX при запуске преобразователя в установщике приложения. После настройки пакетного проекта теперь можно также использовать клавишу F5 для отладки приложений непосредственно в пакете AppX. 

Порядок начала работы: 

1. Сначала убедитесь, что Centennial настроен для использования. Инструкции см. в статье [Desktop App Converter Preview (Project Centennial)](https://msdn.microsoft.com/windows/uwp/porting/desktop-to-uwp-run-desktop-app-converter). 

2. Запустите преобразователь, а затем установщик для своего приложения Win32. Преобразователь выполнит захват макета и все изменения, внесенные в реестр, а затем выведет Appx с манифестом и файлом registery.dat для виртуализации реестра:

![alt](images/desktop-to-uwp/debug-1.png)

3. Установка и запуск [Visual Studio "15" Preview 2](https://www.visualstudio.com/downloads/visual-studio-next-downloads-vs.aspx). 

4. Установите классическое приложение в проект VSIX пакета UWP из [коллекции Visual Studio](http://go.microsoft.com/fwlink/?LinkId=797871). 

5. Откройте соответствующее решение Win32, преобразованное в Visual Studio.
 
6. Добавьте новый пакетный проект в решение, щелкнув правой кнопкой мыши решение и выбрав пункт «Добавить новый проект». Затем выберите пункт «Преобразование классического приложения в пакетный проект UWP» в разделе «Проекты установки и развертывания»:

    ![alt](images/desktop-to-uwp/debug-2.png)

    Созданный проект будет добавлен в ваше решение:

    ![alt](images/desktop-to-uwp/debug-3.png)

    В пакетном проекте AppXFileList отображает сопоставление файлов в виде макета AppX. Раздел «Ссылки» будет пустым и его будет необходимо вручную связать с проектом .exe для расстановки компонентов сборки. 

7. Проект DesktopToUWPPackaging содержит страницу свойств, позволяющую настраивать корень пакета AppX и задавать плитки, которые необходимо выполнить:

    ![alt](images/desktop-to-uwp/debug-4.png)

    Установите PackageLayout корневую папку AppX, которая была создана преобразователем (выше). Затем выберите плитку, которую необходимо выполнить.

8.  Откройте и отредактируйте файл AppXFileList.xml. Этот файл определяет, как копировать выходные данные сборки отладки Win32 в макет AppX, созданный преобразователем. По умолчанию в файле имеется заполнитель с примером тега и содержимого:

    ```XML
    <?xml version="1.0" encoding="utf-8"?>
    <Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
      <ItemGroup>
    <!— Use the following syntax to copy debug output to the AppX layout
       <AppxPackagedFile Include="$(outdir)\App.exe">
          <PackagePath>App.exe</PackagePath>
        </AppxPackagedFile> 
        See http://etc...
    -->
      </ItemGroup>
    </Project>
    ```

    Ниже приводится пример создания сопоставления. В этом случае мы копируем файлы .exe и .dll из папки сборки Win32 в папку макета пакета. 

    ```XML
    <?xml version="1.0" encoding=utf-8"?>
    <Project ToolsVersion=14.0" xmlns="http://scehmas.microsoft.com/developer/msbuild/2003">
        <PropertyGroup>
            <MyProjectOutputPath>C:\{path}</MyProjectOutputPath>
        </PropertyGroup>
        <ItemGroup>
            <LayoutFile Include="$(MyProjectOutputPath)\ProjectTracker.exe">
                <PackagePath>$(PackageLayout)\VFS\Program Files (x86)\Contoso Software\Project Tracker\ProjectTracker.exe</PackagePath>
            </LayoutFile>
            <LayoutFile Include="$(MyProjectOutputPath)\ProjectTracker.Models.dll">
                <PackagePath>$(PackageLayout)\VFS\Program Files (x86)\Contoso Software\Project Tracker\ProjectTracker.Models.dll</PackagePath>
            </LayoutFile>
        </ItemGroup>
    </Project>
    ```

    Файл определяется следующим образом: 

    Сначала выполняется определение объекта *MyProjectOutputPath*, чтобы он указывал на расположение, в котором выполняется сборка проекта Win32:

    ```XML
    <?xml version="1.0" encoding="utf-8"?>
    <Project ToolsVersion="14.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
        <PropertyGroup>
            <MyProjectOutputPath>C:\Users\peterfar\Desktop\ProjectTracker\ProjectTracker\bin\DesktopUWP</MyProjectOutputPath>
        </PropertyGroup>
    ```

    После этого каждый объект *LayoutFile* определяет файл для копирования из папки сборки Win32 в макет пакета AppX. В этом случае сначала копируется файл a .exe, а затем файл a .dll. 

    ```XML
        <ItemGroup>
            <LayoutFile Include="$(MyProjectOutputPath)\ProjectTracker.exe">
                <PackagePath>$(PackageLayout)\VFS\Program Files (x86)\Contoso Software\Project Tracker\ProjectTracker.exe</PackagePath>
            </LayoutFile>
            <LayoutFile Include="$(MyProjectOutputPath)\ProjectTracker.Models.dll">
                <PackagePath>$(PackageLayout)\VFS\Program Files (x86)\Contoso Software\Project Tracker\ProjectTracker.Models.dll</PackagePath>
            </LayoutFile>
        </ItemGroup>
    </Project>
    ```

9. Установите пакетный проект в качестве запускаемого проекта. При этом файлы Win32 будут скопированы в AppX и после создания и запуска проекта необходимо запустить отладчик.  

    ![alt](images/desktop-to-uwp/debug-5.png)

10. Наконец, вы можете задать точку останова в коде Win32 и нажать клавишу F5, чтобы запустить отладчик. Он скопирует все обновления, которые вы внесли в приложение Win32 во всем пакете AppX, и вы сможете выполнить отладку непосредственно из Visual Studio.

11. Если вы обновите приложение, будет необходимо использовать программу MakeAppx, чтобы перепаковать приложение повторно. Дополнительные сведения см. в статье [упаковщик приложений (MakeAppx.exe)](https://msdn.microsoft.com/en-us/library/windows/desktop/hh446767(v=vs.85).aspx). 

Если у вас несколько конфигураций сборки (например, для выпуска и отладки), можно добавить следующий код в файл AppXFileList.xml, чтобы скопировать сборку Win32 из различных расположений:

```XML
<PropertyGroup>
    <MyProjectOutputPath Condition="$(Configuration) == 'DesktopUWP'">C:\Users\peterfar\Desktop\ProjectTracker\ProjectTracker\bin\DesktopUWP>
    </MyProjectOutputPath>
    <MyProjectOutputPath Condition="$(Configuration) == 'ReleaseDesktopUWP'"> C:\Users\peterfar\Desktop\ProjectTracker\ProjectTracker\bin\ReleaseDesktopUWP</MyProjectOutputPath>
</PropertyGroup>
```

Вы также можете применить условную компиляцию для включения определенных путей кода в случае обновления приложения до приложения UWP, но также при необходимости его сборки для Win32. 

1.  В примере ниже код будет скомпилирован только для DesktopUWP и будет показывать плитку с помощью API-интерфейса WinRT. 

    ```C#
    [Conditional("DesktopUWP")]
    private void showtile()
    {
        XmlDocument tileXml = TileUpdateManager.GetTemplateContent(TileTemplateType.TileSquare150x150Text01);
        XmlNodeList textNodes = tileXml.GetElementsByTagName("text");
        textNodes[0].InnerText = string.Format("Welcome to DesktopUWP!");
        TileNotification tileNotification = new TileNotification(tileXml);
        TileUpdateManager.CreateTileUpdaterForApplication().Update(tileNotification);
    }
    ```

2.  Вы можете использовать Configuration Manager, чтобы добавить новую конфигурацию сборки:

    ![alt](images/desktop-to-uwp/debug-6.png)

    ![alt](images/desktop-to-uwp/debug-7.png)

3.  Затем в разделе свойств проекта добавьте поддержку символов условной компиляции:

    ![alt](images/desktop-to-uwp/debug-8.png)

4.  Теперь вы можете переключиться между целью сборки и DesktopUWP, если необходимо, чтобы целью сборки был API-интерфейс UWP, который вы добавили.

## Развертывание преобразованного приложения UWP

Чтобы развернуть приложение во время разработки, запустите следующий командлет PowerShell: 

```Add-AppxPackage –Register AppxManifest.xml```

Для обновления файлов .exe или .dll приложения просто замените существующие файлы в пакете новыми, повысьте номер версии в AppxManifest.xml, а затем выполните указанную выше команду повторно.

Примите к сведению следующее. 

Любой диск, на котором выполняется установка преобразованного приложения должен быть отформатирован в формат NTFS.

Преобразованное приложение всегда выполняется как текущий пользователь. Это имеет особую значимость для приложений .NET, манифест которых определяет уровень выполнения __requireAdministrator__. Если у текущего пользователя есть права администратора _при каждом запуске приложения_ будет отображаться запрос UAC. Обычные пользователи не смогут запустить приложение.

При попытке запуска командлета Add-AppxPackage на компьютере, на который вы не импортировали созданный сертификат, будет выводиться ошибка.

Вот как импортировать сертификат, созданный ранее. Можно установить его напрямую или из appx, в который вошли как пользователь.
1.  В проводнике щелкните правой кнопкой мыши appx, в который вы вошли с проверочным сертификатом, и выберите в контекстном меню пункт **Свойства**.
2.  Щелкните вкладку **Цифровые подписи** или коснитесь ее.
3.  Щелкните сертификат или коснитесь его и выберите пункт **Сведения**.
4.  Щелкните пункт **Просмотр сертификата** или коснитесь его.
5.  Щелкните пункт **Установить сертификат** или коснитесь его.
6.  В группе **Расположение хранилища** выберите пункт **Локальный компьютер**.
7.  Щелкните пункт **Далее** или коснитесь его, а затем нажмите кнопку **ОК**, чтобы подтвердить запрос диалогового окна «Контроль учетных записей».
8.  На следующем экране мастера импорта сертификатов измените выбранный вариант на **Поместить все сертификаты в следующее хранилище**.
9.  Выберите пункт **Обзор**. В окне «Выбор хранилища сертификата» прокрутите вниз и выберите пункт **Доверенные лица**, а затем коснитесь кнопки **ОК**.
10. Выберите пункт **Далее**. Отобразится новый экран Выберите пункт **Готово**.
11. Отобразится диалоговое окно подтверждения. Если окно отобразилось, нажмите кнопку **OK**. Если появляется другое диалоговое окно, сообщающее о проблеме с сертификатом, вам может понадобиться выполнить действия по устранению ошибок сертификата.

### Дополнительная информация

При работе в Windows, чтобы доверить сертификат, он должен размещаться в узле **Сертификаты (Локальная машина) > Доверенные корневые центры сертификации > Сертификаты** или в узле **Сертификаты (Локальная машина) > Доверенные лица > Сертификаты**. Только сертификаты в этих двух расположениях могут проверить доверие сертификатов в контексте локального компьютера. В противном случае выводится сообщение об ошибке, которое напоминает следующей строки:
```CMD
"Add-AppxPackage : Deployment failed with HRESULT: 0x800B0109, A certificate chain processed,
but terminated in a rootcertificate which is not trusted by the trust provider.
(Exception from HRESULT: 0x800B0109) error 0x800B0109: The root certificate of the signature
in the app package must be trusted."
```

### Как это делается

Когда вы запускаете свое преобразованное приложение, ваш пакет приложения UWP запускается из \Program Files\WindowsApps\\&lt;_package name_&gt;\\&lt;_appname_&gt;.exe. Если взглянуть на содержание этой папки, вы увидите, что приложение содержит манифест пакета приложения (с именем AppxManifest.xml), который указывает на особое пространство имен xml, используемое для преобразованных приложений. Внутри этого файла манифеста расположен элемент __&lt;EntryPoint&gt;__, который указывает на приложение с полным доверием. Когда это приложение запускается, оно не выполняется в контейнере приложения, однако работает как обычный пользователь.

Однако приложение запускается в особой среде, в которой любые обращения, которые выполняет приложение к файловой системе, а также к реестру, перенаправляются. Файл с именем Registry.dat используется для перенаправления реестра. Фактически это куст реестра, поэтому его можно просмотреть в редакторе реестра Windows (Regedit). Обратите внимание на то, что этот механизм означает, что вы не можете использовать реестр для межпроцессных взаимодействий. В любом случае реестр не предназначен и не подходит для этого. Говоря о файловой системе, единственным перенаправляемым элементом является папка AppData и она перенаправляется в то же расположение, в котором хранятся приложений для всех приложений UWP. Это расположение известно как локальное хранилище данных приложений и доступ к нему осуществляется с помощью свойства [ApplicationData.LocalFolder](https://msdn.microsoft.com/library/windows/apps/br241621). Таким образом код уже перенесен для чтения и записи данных приложения в соответствующее место и выполнять какие-либо действия не требуется. Также запись можно выполнять непосредственно в это место. Одним из преимуществ перенаправления файловой системы является более полное удаление данных.

В папке с именем VFS будут находиться папки, содержащие фалы DLL, с которыми связано ваше приложение. Эти файлы DLL устанавливаются в системные папки, за счет чего создается классическая версия вашего приложения. Однако, как и приложение UWP, файлы DLL являются локальными относительно вашего приложения. Таким образом при установке и удалении приложений UWP удается избежать проблем несоответствия версий.


<!--HONumber=May16_HO2-->

