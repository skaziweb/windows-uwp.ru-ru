---
ms.assetid: 16ad97eb-23f1-0264-23a9-a1791b4a5b95
title: Собственное взаимодействие композиции
description: API Windows.UI.Composition предоставляет собственные интерфейсы взаимодействия, позволяющие перемещать содержимое непосредственно в компоновщик.
ms.date: 06/22/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 51fc077342694b9ddbdbc03863c0db3b8cd9e1c5
ms.sourcegitcommit: 3f7432afaa73083cb9b8331e30c885068c6f6dbf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/08/2019
ms.locfileid: "72023267"
---
# <a name="composition-native-interoperation-with-directx-and-direct2d"></a>Собственное взаимодействие композиции с DirectX и Direct2D

API Windows.UI.Composition предоставляет собственные интерфейсы взаимодействия [**ICompositorInterop**](https://docs.microsoft.com/windows/desktop/api/windows.ui.composition.interop/nn-windows-ui-composition-interop-icompositorinterop), [**ICompositionDrawingSurfaceInterop**](https://docs.microsoft.com/windows/desktop/api/windows.ui.composition.interop/nn-windows-ui-composition-interop-icompositiondrawingsurfaceinterop) и [**ICompositionGraphicsDeviceInterop**](https://docs.microsoft.com/windows/desktop/api/windows.ui.composition.interop/nn-windows-ui-composition-interop-icompositiongraphicsdeviceinterop), позволяющие перемещать содержимое непосредственно в компоновщик.

Структура собственного взаимодействия построена вокруг объектов поверхности, которые поддерживаются текстурами DirectX. Поверхности создаются из объекта фабрики с именем [**CompositionGraphicsDevice**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionGraphicsDevice). Этот объект поддерживается базовым объектом устройства Direct2D или Direct3D, который используется для выделения видеопамяти для поверхностей. API композиции никогда не создает базовое устройство DirectX. Его должно создать приложение и затем передать объекту **CompositionGraphicsDevice**. Приложение может создавать более одного объекта **CompositionGraphicsDevice** одновременно. Приложение может использовать то же устройство DirectX в качестве устройства отрисовки для нескольких объектов **CompositionGraphicsDevice**.

## <a name="creating-a-surface"></a>Создание поверхности

Каждый объект [**CompositionGraphicsDevice**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionGraphicsDevice) выступает в качестве фабрики поверхностей. Каждая поверхность создается с начальными размерами (они могут быть равны 0,0), но без действительных пикселей. Поверхность в своем начальном состоянии может быть сразу же использована в визуальном дереве, например с помощью [**CompositionSurfaceBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) и [**SpriteVisual**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.SpriteVisual), но в своем начальном состоянии она не оказывает никакого влияния на выводимое изображение. Она полностью прозрачна для всех целей, даже когда указанный режим альфа-канала является непрозрачным.

Иногда устройства DirectX могут стать непригодными к использованию. Причиной этого, помимо прочего, может стать ситуация, когда приложение передает недопустимые аргументы некоторым API-интерфейсам DirectX, когда графический адаптер сбрасывается системой или же когда обновляется драйвер. В Direct3D имеется API, с помощью которого приложение может асинхронно определить, является ли устройство недоступным по какой-либо причине. Если устройство DirectX недоступно, приложению необходимо отказаться от его использования, создать новое устройство и передать его всем объектам [**CompositionGraphicsDevice**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionGraphicsDevice), которые ранее были связаны с неисправным устройством DirectX.

## <a name="loading-pixels-into-a-surface"></a>Загрузка пикселей на поверхность

Для загрузки пикселей на поверхность приложение должно вызвать метод [**BeginDraw**](https://docs.microsoft.com/windows/desktop/api/windows.ui.composition.interop/nf-windows-ui-composition-interop-icompositiondrawingsurfaceinterop-begindraw), который возвращает интерфейс DirectX, представляющий текстуру или контекст Direct2D в зависимости от того, что запрашивает приложение. После этого приложение должно отрисовать или передать пиксели на эту текстуру. Когда приложение завершает это действие, оно должно вызвать метод [**EndDraw**](https://docs.microsoft.com/windows/desktop/api/windows.ui.composition.interop/nf-windows-ui-composition-interop-icompositiondrawingsurfaceinterop-enddraw). Только в этот момент новые пиксели становятся доступны композиции, но они по-прежнему не будут отображаться на экране до тех пор, пока не будут зафиксированы изменения в визуальном дереве. Если изменения в визуальном дереве фиксируются перед вызовом **EndDraw** , то обновление, которое выполняется в настоящий момент, не отображается на экране, а поверхность продолжает отображать то содержимое, которое было на ней до вызова **BeginDraw**. При вызове функции **EndDraw** текстура или указатель на контекст Direct2D, возвращаемые функцией "BeginDraw", становятся недействительными. Приложение никогда не должно кэшировать этот указатель после вызова **EndDraw**.

Приложение может вызывать функцию "BeginDraw" только для одной поверхности за один раз для любого данного [**CompositionGraphicsDevice**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionGraphicsDevice). После вызова [**BeginDraw**](https://docs.microsoft.com/windows/desktop/api/windows.ui.composition.interop/nf-windows-ui-composition-interop-icompositiondrawingsurfaceinterop-begindraw) приложение должно вызвать [**EndDraw**](https://docs.microsoft.com/windows/desktop/api/windows.ui.composition.interop/nf-windows-ui-composition-interop-icompositiondrawingsurfaceinterop-enddraw) для этой поверхности, прежде чем вызывать **BeginDraw** для другой поверхности. Так как API работает быстро, приложение должно синхронизировать эти вызовы, если требуется выполнять отрисовку из нескольких рабочих потоков. Если приложению нужно прервать отрисовку одной поверхности и временно переключиться на отрисовку другой поверхности, оно может использовать метод [**SuspendDraw**](https://docs.microsoft.com/windows/desktop/api/windows.ui.composition.interop/nf-windows-ui-composition-interop-icompositiondrawingsurfaceinterop-suspenddraw). Это дает возможность успешно выполнить **BeginDraw**, но делает обновление первой поверхности недоступным для экранной композиции. Это позволяет приложению выполнять несколько обновлений в виде транзакций. Когда работа с поверхностью приостановлена, приложение может продолжить обновление, вызвав метод [**ResumeDraw**](https://docs.microsoft.com/windows/desktop/api/windows.ui.composition.interop/nf-windows-ui-composition-interop-icompositiondrawingsurfaceinterop-resumedraw). Оно также может объявить, что обновление завершено, вызвав **EndDraw**. Это означает, что только одна поверхность может активно обновляться в конкретный момент времени для любого данного **CompositionGraphicsDevice**. Каждое графическое устройство сохраняет это состояние независимо от остальных, поэтому приложение может выполнять отрисовку на двух поверхностях одновременно, если они принадлежат разным графическим устройствам. Однако такая процедура не позволяет совместно использовать видеопамять этих двух поверхностей в пуле и, таким образом, снижает эффективность работы с памятью.

Методы [**BeginDraw**](https://docs.microsoft.com/windows/desktop/api/windows.ui.composition.interop/nf-windows-ui-composition-interop-icompositiondrawingsurfaceinterop-begindraw), [**SuspendDraw**](https://docs.microsoft.com/windows/desktop/api/windows.ui.composition.interop/nf-windows-ui-composition-interop-icompositiondrawingsurfaceinterop-suspenddraw), [**ResumeDraw**](https://docs.microsoft.com/windows/desktop/api/windows.ui.composition.interop/nf-windows-ui-composition-interop-icompositiondrawingsurfaceinterop-resumedraw) и [**EndDraw**](https://docs.microsoft.com/windows/desktop/api/windows.ui.composition.interop/nf-windows-ui-composition-interop-icompositiondrawingsurfaceinterop-enddraw) возвращают ошибку, если приложение выполняет недопустимую операцию (например, передает недопустимые аргументы или вызывает **BeginDraw** для одной поверхности до вызова **EndDraw** для другой). Такие сбои указывают на ошибки в приложении, поэтому ожидается, что эти сбои будут обработаны с помощью быстрого прекращения. Функция **BeginDraw** также может возвращать ошибку, если базовое устройство DirectX недоступно. Этот сбой не является критическим, так как приложение может повторно создать устройство DirectX и повторить попытку. В связи с этим ожидается, что при отсутствии доступа к устройству приложение будет просто пропускать отрисовку. Если по какой-то причине **BeginDraw** завершается с ошибкой, приложение не должно также вызывать **EndDraw**, так как операция с самого начала не была успешной.

## <a name="scrolling"></a>Прокрутка

Когда приложение вызывает [**BeginDraw**](https://docs.microsoft.com/windows/desktop/api/windows.ui.composition.interop/nf-windows-ui-composition-interop-icompositiondrawingsurfaceinterop-begindraw), то из соображений производительности не гарантируется, что содержимое возвращаемой текстуры будет соответствовать предыдущему содержимому поверхности. Приложение должно исходить из того, что содержимое содержит случайные данные. Вследствие этого приложение должно убедиться, что затронуты все пиксели и либо очистить поверхность перед отрисовкой, либо отрисовать достаточный объем непрозрачного содержимого, чтобы покрыть весь обновленный прямоугольник. Это, а также тот факт, что указатель текстуры действителен только между вызовами **BeginDraw** и [**EndDraw**](https://docs.microsoft.com/windows/desktop/api/windows.ui.composition.interop/nf-windows-ui-composition-interop-icompositiondrawingsurfaceinterop-enddraw), не позволяет приложению копировать предыдущее содержимое с поверхности. По этой причине мы рекомендуем использовать метод [**Scroll**](https://docs.microsoft.com/windows/desktop/api/windows.ui.composition.interop/nf-windows-ui-composition-interop-icompositiondrawingsurfaceinterop-scroll), который позволяет приложению выполнять копирование пикселей для этой же поверхности.

## <a name="usage-example"></a>Пример использования

В следующем примере кода показан сценарий взаимодействия. В примере объединяются типы из контактной зоны, основанной на среда выполнения Windows, вместе с типами из заголовков взаимодействия и кодом, который визуализирует текст с помощью интерфейсов API DirectWrite и Direct2D на основе COM. В примере используются [**бегиндрав**](https://docs.microsoft.com/windows/desktop/api/windows.ui.composition.interop/nf-windows-ui-composition-interop-icompositiondrawingsurfaceinterop-begindraw) и [**EndDraw**](https://docs.microsoft.com/windows/desktop/api/windows.ui.composition.interop/nf-windows-ui-composition-interop-icompositiondrawingsurfaceinterop-enddraw) , чтобы облегчить взаимодействие между этими технологиями. В примере используется DirectWrite для размещения текста, а затем для его визуализации используется Direct2D. Графическое устройство композиции принимает устройство Direct2D непосредственно во время инициализации. Это позволяет **бегиндрав** вернуть указатель на интерфейс **ID2D1DeviceContext** , который гораздо более эффективен, чем создание приложением контекста Direct2D для создания оболочки возвращаемого интерфейса ID3D11Texture2D в каждой операции рисования.

Ниже приведены два примера кода. Сначала [ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) пример (который является завершенным), а затем — пример C++кода/CX (в котором опущены DirectWrite и Direct2D части примера).

Чтобы использовать приведенный ниже пример кода C++/WinRT, сначала создайте проект **базового приложения (C++/WinRT)** в Visual Studio (см. Дополнительные сведения о [поддержке Visual Studio для C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)). При создании проекта выберите в качестве целевой версии **Windows 10, версия 1803 (10,0; Сборка 17134)** . Это версия, с которой этот код был создан и протестирован. Замените содержимое файла исходного кода `App.cpp` приведенным ниже кодом, затем выполните сборку и запуск. Приложение визуализирует строку "Hello, World!" в виде черного текста на прозрачном фоне.

```cppwinrt
// App.cpp
//*********************************************************
//
// Copyright (c) Microsoft. All rights reserved.
// This code is licensed under the MIT License (MIT).
// THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, 
// INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, 
// FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. 
// IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, 
// DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, 
// TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH 
// THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
//
//*********************************************************

#include "pch.h"

#include <D2d1_1.h>
#include <D3d11_4.h>
#include <Dwrite.h>
#include <Windows.Graphics.DirectX.Direct3D11.interop.h>
#include <Windows.ui.composition.interop.h>
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Graphics.DirectX.h>
#include <winrt/Windows.Graphics.DirectX.Direct3D11.h>
#include <winrt/Windows.UI.Composition.h>

using namespace winrt;
using namespace winrt::Windows::ApplicationModel::Core;
using namespace winrt::Windows::Foundation;
using namespace winrt::Windows::Foundation::Numerics;
using namespace winrt::Windows::Graphics::DirectX;
using namespace winrt::Windows::Graphics::DirectX::Direct3D11;
using namespace winrt::Windows::UI;
using namespace winrt::Windows::UI::Composition;
using namespace winrt::Windows::UI::Core;

namespace abi
{
    using namespace ABI::Windows::Foundation;
    using namespace ABI::Windows::Graphics::DirectX;
    using namespace ABI::Windows::UI::Composition;
}

// An app-provided helper to render lines of text.
struct SampleText
{
    SampleText(winrt::com_ptr<::IDWriteTextLayout> const& text, CompositionGraphicsDevice const& compositionGraphicsDevice) :
        m_text(text),
        m_compositionGraphicsDevice(compositionGraphicsDevice)
    {
        // Create the surface just big enough to hold the formatted text block.
        DWRITE_TEXT_METRICS metrics;
        winrt::check_hresult(m_text->GetMetrics(&metrics));
        winrt::Windows::Foundation::Size surfaceSize{ metrics.width, metrics.height };

        CompositionDrawingSurface drawingSurface{ m_compositionGraphicsDevice.CreateDrawingSurface(
            surfaceSize,
            DirectXPixelFormat::B8G8R8A8UIntNormalized,
            DirectXAlphaMode::Premultiplied) };

        // Cache the interop pointer, since that's what we always use.
        m_drawingSurfaceInterop = drawingSurface.as<abi::ICompositionDrawingSurfaceInterop>();

        // Draw the text
        DrawText();

        // If the rendering device is lost, the application will recreate and replace it. We then
        // own redrawing our pixels.
        m_deviceReplacedEventToken = m_compositionGraphicsDevice.RenderingDeviceReplaced(
            [this](CompositionGraphicsDevice const&, RenderingDeviceReplacedEventArgs const&)
        {
            // Draw the text again.
            DrawText();
            return S_OK;
        });
    }

    ~SampleText()
    {
        m_compositionGraphicsDevice.RenderingDeviceReplaced(m_deviceReplacedEventToken);
    }

    // Return the underlying surface to the caller.
    auto Surface()
    {
        // To the caller, the fact that we have a drawing surface is an implementation detail.
        // Return the base interface instead.
        return m_drawingSurfaceInterop.as<ICompositionSurface>();
    }

private:
    // The text to draw.
    winrt::com_ptr<::IDWriteTextLayout> m_text;

    // The composition surface that we use in the visual tree.
    winrt::com_ptr<abi::ICompositionDrawingSurfaceInterop> m_drawingSurfaceInterop;

    // The device that owns the surface.
    CompositionGraphicsDevice m_compositionGraphicsDevice{ nullptr };
    //winrt::com_ptr<abi::ICompositionGraphicsDevice> m_compositionGraphicsDevice2;

    // For managing our event notifier.
    winrt::event_token m_deviceReplacedEventToken;

    // We may detect device loss on BeginDraw calls. This helper handles this condition or other
    // errors.
    bool CheckForDeviceRemoved(HRESULT hr)
    {
        if (hr == S_OK)
        {
            // Everything is fine: go ahead and draw.
            return true;
        }

        if (hr == DXGI_ERROR_DEVICE_REMOVED)
        {
            // We can't draw at this time, but this failure is recoverable. Just skip drawing for
            // now. We will be asked to draw again once the Direct3D device is recreated.
            return false;
        }

        // Any other error is unexpected and, therefore, fatal.
        winrt::check_hresult(hr);
        return true;
    }

    // Renders the text into our composition surface
    void DrawText()
    {
        // Begin our update of the surface pixels. If this is our first update, we are required
        // to specify the entire surface, which nullptr is shorthand for (but, as it works out,
        // any time we make an update we touch the entire surface, so we always pass nullptr).
        winrt::com_ptr<::ID2D1DeviceContext> d2dDeviceContext;
        POINT offset;
        if (CheckForDeviceRemoved(m_drawingSurfaceInterop->BeginDraw(nullptr,
            __uuidof(ID2D1DeviceContext), d2dDeviceContext.put_void(), &offset)))
        {
            d2dDeviceContext->Clear(D2D1::ColorF(D2D1::ColorF::Black, 0.f));

            // Create a solid color brush for the text. A more sophisticated application might want
            // to cache and reuse a brush across all text elements instead, taking care to recreate
            // it in the event of device removed.
            winrt::com_ptr<::ID2D1SolidColorBrush> brush;
            winrt::check_hresult(d2dDeviceContext->CreateSolidColorBrush(
                D2D1::ColorF(D2D1::ColorF::Black, 1.0f), brush.put()));

            // Draw the line of text at the specified offset, which corresponds to the top-left
            // corner of our drawing surface. Notice we don't call BeginDraw on the D2D device
            // context; this has already been done for us by the composition API.
            d2dDeviceContext->DrawTextLayout(D2D1::Point2F((float)offset.x, (float)offset.y), m_text.get(),
                brush.get());

            // Our update is done. EndDraw never indicates rendering device removed, so any
            // failure here is unexpected and, therefore, fatal.
            winrt::check_hresult(m_drawingSurfaceInterop->EndDraw());
        }
    }
};

struct DeviceLostEventArgs
{
    DeviceLostEventArgs(IDirect3DDevice const& device) : m_device(device) {}
    IDirect3DDevice Device() { return m_device; }
    static DeviceLostEventArgs Create(IDirect3DDevice const& device) { return DeviceLostEventArgs{ device }; }

private:
    IDirect3DDevice m_device;
};

struct DeviceLostHelper
{
    DeviceLostHelper() = default;

    ~DeviceLostHelper()
    {
        StopWatchingCurrentDevice();
        m_onDeviceLostHandler = nullptr;
    }

    IDirect3DDevice CurrentlyWatchedDevice() { return m_device; }

    void WatchDevice(winrt::com_ptr<::IDXGIDevice> const& dxgiDevice)
    {
        // If we're currently listening to a device, then stop.
        StopWatchingCurrentDevice();

        // Set the current device to the new device.
        m_device = nullptr;
        winrt::check_hresult(::CreateDirect3D11DeviceFromDXGIDevice(dxgiDevice.get(), reinterpret_cast<::IInspectable**>(winrt::put_abi(m_device))));

        // Get the DXGI Device.
        m_dxgiDevice = dxgiDevice;

        // QI For the ID3D11Device4 interface.
        winrt::com_ptr<::ID3D11Device4> d3dDevice{ m_dxgiDevice.as<::ID3D11Device4>() };

        // Create a wait struct.
        m_onDeviceLostHandler = nullptr;
        m_onDeviceLostHandler = ::CreateThreadpoolWait(DeviceLostHelper::OnDeviceLost, (PVOID)this, nullptr);

        // Create a handle and a cookie.
        m_eventHandle.attach(::CreateEvent(nullptr, false, false, nullptr));
        winrt::check_bool(bool{ m_eventHandle });
        m_cookie = 0;

        // Register for device lost.
        ::SetThreadpoolWait(m_onDeviceLostHandler, m_eventHandle.get(), nullptr);
        winrt::check_hresult(d3dDevice->RegisterDeviceRemovedEvent(m_eventHandle.get(), &m_cookie));
    }

    void StopWatchingCurrentDevice()
    {
        if (m_dxgiDevice)
        {
            // QI For the ID3D11Device4 interface.
            auto d3dDevice{ m_dxgiDevice.as<::ID3D11Device4>() };

            // Unregister from the device lost event.
            ::CloseThreadpoolWait(m_onDeviceLostHandler);
            d3dDevice->UnregisterDeviceRemoved(m_cookie);

            // Clear member variables.
            m_onDeviceLostHandler = nullptr;
            m_eventHandle.close();
            m_cookie = 0;
            m_device = nullptr;
        }
    }

    void DeviceLost(winrt::delegate<DeviceLostHelper const*, DeviceLostEventArgs const&> const& handler)
    {
        m_deviceLost = handler;
    }

    winrt::delegate<DeviceLostHelper const*, DeviceLostEventArgs const&> m_deviceLost;

private:
    void RaiseDeviceLostEvent(IDirect3DDevice const& oldDevice)
    {
        m_deviceLost(this, DeviceLostEventArgs::Create(oldDevice));
    }

    static void CALLBACK OnDeviceLost(PTP_CALLBACK_INSTANCE /* instance */, PVOID context, PTP_WAIT /* wait */, TP_WAIT_RESULT /* waitResult */)
    {
        auto deviceLostHelper = reinterpret_cast<DeviceLostHelper*>(context);
        auto oldDevice = deviceLostHelper->m_device;
        deviceLostHelper->StopWatchingCurrentDevice();
        deviceLostHelper->RaiseDeviceLostEvent(oldDevice);
    }

private:
    IDirect3DDevice m_device;
    winrt::com_ptr<::IDXGIDevice> m_dxgiDevice;
    PTP_WAIT m_onDeviceLostHandler{ nullptr };
    winrt::handle m_eventHandle;
    DWORD m_cookie{ 0 };
};

struct SampleApp : implements<SampleApp, IFrameworkViewSource, IFrameworkView>
{
    IFrameworkView CreateView()
    {
        return *this;
    }

    void Initialize(CoreApplicationView const &)
    {
    }

    // Run once when the application starts up
    void Initialize()
    {
        // Create a Direct2D device.
        CreateDirect2DDevice();

        // To create a composition graphics device, we need to QI for another interface
        winrt::com_ptr<abi::ICompositorInterop> compositorInterop{ m_compositor.as<abi::ICompositorInterop>() };

        // Create a graphics device backed by our D3D device
        winrt::com_ptr<abi::ICompositionGraphicsDevice> compositionGraphicsDeviceIface;
        winrt::check_hresult(compositorInterop->CreateGraphicsDevice(
            m_d2dDevice.get(),
            compositionGraphicsDeviceIface.put()));
        m_compositionGraphicsDevice = compositionGraphicsDeviceIface.as<CompositionGraphicsDevice>();
    }

    void Load(hstring const&)
    {
    }

    void Uninitialize()
    {
    }

    void Run()
    {
        CoreWindow window = CoreWindow::GetForCurrentThread();
        window.Activate();

        CoreDispatcher dispatcher = window.Dispatcher();
        dispatcher.ProcessEvents(CoreProcessEventsOption::ProcessUntilQuit);
    }

    void SetWindow(CoreWindow const& window)
    {
        m_compositor = Compositor{};
        m_target = m_compositor.CreateTargetForCurrentView();
        ContainerVisual root = m_compositor.CreateContainerVisual();
        m_target.Root(root);

        Initialize();

        winrt::check_hresult(
            ::DWriteCreateFactory(
                DWRITE_FACTORY_TYPE_SHARED,
                __uuidof(m_dWriteFactory),
                reinterpret_cast<::IUnknown**>(m_dWriteFactory.put())
            )
        );

        winrt::check_hresult(
            m_dWriteFactory->CreateTextFormat(
                L"Segoe UI",
                nullptr,
                DWRITE_FONT_WEIGHT_REGULAR,
                DWRITE_FONT_STYLE_NORMAL,
                DWRITE_FONT_STRETCH_NORMAL,
                36.f,
                L"en-US",
                m_textFormat.put()
            )
        );

        Rect windowBounds{ window.Bounds() };
        std::wstring text{ L"Hello, World!" };

        winrt::check_hresult(
            m_dWriteFactory->CreateTextLayout(
                text.c_str(),
                (uint32_t)text.size(),
                m_textFormat.get(),
                windowBounds.Width,
                windowBounds.Height,
                m_textLayout.put()
            )
        );

        Visual textVisual{ CreateVisualFromTextLayout(m_textLayout) };
        textVisual.Size({ windowBounds.Width, windowBounds.Height });
        root.Children().InsertAtTop(textVisual);
    }

    // Called when Direct3D signals the device lost event.
    void OnDirect3DDeviceLost(DeviceLostHelper const* /* sender */, DeviceLostEventArgs const& /* args */)
    {
        // Create a new Direct2D device.
        CreateDirect2DDevice();

        // Restore our composition graphics device to good health.
        winrt::com_ptr<abi::ICompositionGraphicsDeviceInterop> compositionGraphicsDeviceInterop{ m_compositionGraphicsDevice.as<abi::ICompositionGraphicsDeviceInterop>() };
        winrt::check_hresult(compositionGraphicsDeviceInterop->SetRenderingDevice(m_d2dDevice.get()));
    }

    // Create a surface that is asynchronously filled with an image
    ICompositionSurface CreateSurfaceFromTextLayout(winrt::com_ptr<::IDWriteTextLayout> const& text)
    {
        // Create our wrapper object that will handle downloading and decoding the image (assume
        // throwing new here).
        SampleText textSurface{ text, m_compositionGraphicsDevice };

        // The caller is only interested in the underlying surface.
        return textSurface.Surface();
    }

    // Create a visual that holds an image.
    Visual CreateVisualFromTextLayout(winrt::com_ptr<::IDWriteTextLayout> const& text)
    {
        // Create a sprite visual
        SpriteVisual spriteVisual{ m_compositor.CreateSpriteVisual() };

        // The sprite visual needs a brush to hold the image.
        CompositionSurfaceBrush surfaceBrush{
            m_compositor.CreateSurfaceBrush(CreateSurfaceFromTextLayout(text))
        };

        // Associate the brush with the visual.
        CompositionBrush brush{ surfaceBrush.as<CompositionBrush>() };
        spriteVisual.Brush(brush);

        // Return the visual to the caller as an IVisual.
        return spriteVisual;
    }

private:
    CompositionTarget m_target{ nullptr };
    Compositor m_compositor{ nullptr };
    winrt::com_ptr<::ID2D1Device> m_d2dDevice;
    winrt::com_ptr<::IDXGIDevice> m_dxgiDevice;
    //winrt::com_ptr<abi::ICompositionGraphicsDevice> m_compositionGraphicsDevice;
    CompositionGraphicsDevice m_compositionGraphicsDevice{ nullptr };
    std::vector<SampleText> m_textSurfaces;
    DeviceLostHelper m_deviceLostHelper;
    winrt::com_ptr<::IDWriteFactory> m_dWriteFactory;
    winrt::com_ptr<::IDWriteTextFormat> m_textFormat;
    winrt::com_ptr<::IDWriteTextLayout> m_textLayout;


    // This helper creates a Direct2D device, and registers for a device loss
    // notification on the underlying Direct3D device. When that notification is
    // raised, the OnDirect3DDeviceLost method is called.
    void CreateDirect2DDevice()
    {
        uint32_t createDeviceFlags = D3D11_CREATE_DEVICE_BGRA_SUPPORT;

        // Array with DirectX hardware feature levels in order of preference.
        D3D_FEATURE_LEVEL featureLevels[] =
        {
            D3D_FEATURE_LEVEL_11_1,
            D3D_FEATURE_LEVEL_11_0,
            D3D_FEATURE_LEVEL_10_1,
            D3D_FEATURE_LEVEL_10_0,
            D3D_FEATURE_LEVEL_9_3,
            D3D_FEATURE_LEVEL_9_2,
            D3D_FEATURE_LEVEL_9_1
        };

        // Create the Direct3D 11 API device object and a corresponding context.
        winrt::com_ptr<::ID3D11Device> d3DDevice;
        winrt::com_ptr<::ID3D11DeviceContext> d3DImmediateContext;
        D3D_FEATURE_LEVEL d3dFeatureLevel{ D3D_FEATURE_LEVEL_9_1 };

        winrt::check_hresult(
            ::D3D11CreateDevice(
                nullptr, // Default adapter.
                D3D_DRIVER_TYPE_HARDWARE,
                0, // Not asking for a software driver, so not passing a module to one.
                createDeviceFlags, // Set debug and Direct2D compatibility flags.
                featureLevels,
                ARRAYSIZE(featureLevels),
                D3D11_SDK_VERSION,
                d3DDevice.put(),
                &d3dFeatureLevel,
                d3DImmediateContext.put()
            )
        );

        // Initialize Direct2D resources.
        D2D1_FACTORY_OPTIONS d2d1FactoryOptions{ D2D1_DEBUG_LEVEL_NONE };

        // Initialize the Direct2D Factory.
        winrt::com_ptr<::ID2D1Factory1> d2D1Factory;
        winrt::check_hresult(
            ::D2D1CreateFactory(
                D2D1_FACTORY_TYPE_SINGLE_THREADED,
                __uuidof(d2D1Factory),
                &d2d1FactoryOptions,
                d2D1Factory.put_void()
            )
        );

        // Create the Direct2D device object.
        // Obtain the underlying DXGI device of the Direct3D device.
        m_dxgiDevice = d3DDevice.as<::IDXGIDevice>();

        m_d2dDevice = nullptr;
        winrt::check_hresult(
            d2D1Factory->CreateDevice(m_dxgiDevice.get(), m_d2dDevice.put())
        );

        m_deviceLostHelper.WatchDevice(m_dxgiDevice);
        m_deviceLostHelper.DeviceLost({ this, &SampleApp::OnDirect3DDeviceLost });
    }
};

int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    CoreApplication::Run(winrt::make<SampleApp>());
}
```

```cpp
//------------------------------------------------------------------------------
//
// Copyright (C) Microsoft. All rights reserved.
//
//------------------------------------------------------------------------------

#include "stdafx.h"

using namespace Microsoft::WRL;
using namespace Windows::Foundation;
using namespace Windows::Graphics::DirectX;
using namespace Windows::UI::Composition;

// This is an app-provided helper to render lines of text
class SampleText
{
private:
    // The text to draw
    ComPtr<IDWriteTextLayout> _text;

    // The composition surface that we use in the visual tree
    ComPtr<ICompositionDrawingSurfaceInterop> _drawingSurfaceInterop;

    // The device that owns the surface
    ComPtr<ICompositionGraphicsDevice> _compositionGraphicsDevice;

    // For managing our event notifier
    EventRegistrationToken _deviceReplacedEventToken;

public:
    SampleText(IDWriteTextLayout* text, ICompositionGraphicsDevice* compositionGraphicsDevice) :
        _text(text),
        _compositionGraphicsDevice(compositionGraphicsDevice)
    {
        // Create the surface just big enough to hold the formatted text block.
        DWRITE_TEXT_METRICS metrics;
        FailFastOnFailure(text->GetMetrics(&metrics));
        Windows::Foundation::Size surfaceSize = { metrics.width, metrics.height };
        ComPtr<ICompositionDrawingSurface> drawingSurface;
        FailFastOnFailure(_compositionGraphicsDevice->CreateDrawingSurface(
            surfaceSize,
            DirectXPixelFormat::DirectXPixelFormat_B8G8R8A8UIntNormalized,
            DirectXAlphaMode::DirectXAlphaMode_Ignore,
            &drawingSurface));

        // Cache the interop pointer, since that's what we always use.
        FailFastOnFailure(drawingSurface.As(&_drawingSurfaceInterop));

        // Draw the text
        DrawText();

        // If the rendering device is lost, the application will recreate and replace it. We then
        // own redrawing our pixels.
        FailFastOnFailure(_compositionGraphicsDevice->add_RenderingDeviceReplaced(
            Callback<RenderingDeviceReplacedEventHandler>([this](
                ICompositionGraphicsDevice* source, IRenderingDeviceReplacedEventArgs* args)
                -> HRESULT
            {
                // Draw the text again
                DrawText();
                return S_OK;
            }).Get(),
            &_deviceReplacedEventToken));
    }

    ~SampleText()
    {
        FailFastOnFailure(_compositionGraphicsDevice->remove_RenderingDeviceReplaced(
            _deviceReplacedEventToken));
    }

    // Return the underlying surface to the caller
    ComPtr<ICompositionSurface> get_Surface()
    {
        // To the caller, the fact that we have a drawing surface is an implementation detail.
        // Return the base interface instead
        ComPtr<ICompositionSurface> surface;
        FailFastOnFailure(_drawingSurfaceInterop.As(&surface));
        return surface;
    }

private:
    // We may detect device loss on BeginDraw calls. This helper handles this condition or other
    // errors.
    bool CheckForDeviceRemoved(HRESULT hr)
    {
        if (SUCCEEDED(hr))
        {
            // Everything is fine -- go ahead and draw
            return true;
        }
        else if (hr == DXGI_ERROR_DEVICE_REMOVED)
        {
            // We can't draw at this time, but this failure is recoverable. Just skip drawing for
            // now. We will be asked to draw again once the Direct3D device is recreated
            return false;
        }
        else
        {
            // Any other error is unexpected and, therefore, fatal
            FailFast();
        }
    }

    // Renders the text into our composition surface
    void DrawText()
    {
        // Begin our update of the surface pixels. If this is our first update, we are required
        // to specify the entire surface, which nullptr is shorthand for (but, as it works out,
        // any time we make an update we touch the entire surface, so we always pass nullptr).
        ComPtr<ID2D1DeviceContext> d2dDeviceContext;
        POINT offset;
        if (CheckForDeviceRemoved(_drawingSurfaceInterop->BeginDraw(nullptr,
            __uuidof(ID2D1DeviceContext), &d2dDeviceContext, &offset)))
        {
            // Create a solid color brush for the text. A more sophisticated application might want
            // to cache and reuse a brush across all text elements instead, taking care to recreate
            // it in the event of device removed.
            ComPtr<ID2D1SolidColorBrush> brush;
            FailFastOnFailure(d2dDeviceContext->CreateSolidColorBrush(
                D2D1::ColorF(D2D1::ColorF::Black, 1.0f), &brush));

            // Draw the line of text at the specified offset, which corresponds to the top-left
            // corner of our drawing surface. Notice we don't call BeginDraw on the D2D device
            // context; this has already been done for us by the composition API.
            d2dDeviceContext->DrawTextLayout(D2D1::Point2F(offset.x, offset.y), _text.Get(),
                brush.Get());

            // Our update is done. EndDraw never indicates rendering device removed, so any
            // failure here is unexpected and, therefore, fatal.
            FailFastOnFailure(_drawingSurfaceInterop->EndDraw());
        }
    }
};

class SampleApp
{
    ComPtr<ICompositor> _compositor;
    ComPtr<ID2D1Device> _d2dDevice;
    ComPtr<ICompositionGraphicsDevice> _compositionGraphicsDevice;
    std::vector<ComPtr<SampleText>> _textSurfaces;

public:
    // Run once when the application starts up
    void Initialize(ICompositor* compositor)
    {
        // Cache the compositor (created outside of this method)
        _compositor = compositor;

        // Create a Direct2D device (helper implementation not shown here)
        FailFastOnFailure(CreateDirect2DDevice(&_d2dDevice));

        // To create a composition graphics device, we need to QI for another interface
        ComPtr<ICompositorInterop> compositorInterop;
        FailFastOnFailure(_compositor.As(&compositorInterop));

        // Create a graphics device backed by our D3D device
        FailFastOnFailure(compositorInterop->CreateGraphicsDevice(
            _d2dDevice.Get(),
            &_compositionGraphicsDevice));
    }

    // Called when Direct3D signals the device lost event
    void OnDirect3DDeviceLost()
    {
        // Create a new device
        FailFastOnFailure(CreateDirect2DDevice(_d2dDevice.ReleaseAndGetAddressOf()));

        // Restore our composition graphics device to good health
        ComPtr<ICompositionGraphicsDeviceInterop> compositionGraphicsDeviceInterop;
        FailFastOnFailure(_compositionGraphicsDevice.As(&compositionGraphicsDeviceInterop));
        FailFastOnFailure(compositionGraphicsDeviceInterop->SetRenderingDevice(_d2dDevice.Get()));
    }

    // Create a surface that is asynchronously filled with an image
    ComPtr<ICompositionSurface> CreateSurfaceFromTextLayout(IDWriteTextLayout* text)
    {
        // Create our wrapper object that will handle downloading and decoding the image (assume
        // throwing new here)
        SampleText* textSurface = new SampleText(text, _compositionGraphicsDevice.Get());

        // Keep our image alive
        _textSurfaces.push_back(textSurface);

        // The caller is only interested in the underlying surface
        return textSurface->get_Surface();
    }

    // Create a visual that holds an image
    ComPtr<IVisual> CreateVisualFromTextLayout(IDWriteTextLayout* text)
    {
        // Create a sprite visual
        ComPtr<ISpriteVisual> spriteVisual;
        FailFastOnFailure(_compositor->CreateSpriteVisual(&spriteVisual));

        // The sprite visual needs a brush to hold the image
        ComPtr<ICompositionSurfaceBrush> surfaceBrush;
        FailFastOnFailure(_compositor->CreateSurfaceBrushWithSurface(
            CreateSurfaceFromTextLayout(text).Get(),
            &surfaceBrush));

        // Associate the brush with the visual
        ComPtr<ICompositionBrush> brush;
        FailFastOnFailure(surfaceBrush.As(&brush));
        FailFastOnFailure(spriteVisual->put_Brush(brush.Get()));

        // Return the visual to the caller as the base class
        ComPtr<IVisual> visual;
        FailFastOnFailure(spriteVisual.As(&visual));

        return visual;
    }

private:
    // This helper (implementation not shown here) creates a Direct2D device and registers
    // for a device loss notification on the underlying Direct3D device. When that notification is
    // raised, assume the OnDirect3DDeviceLost method is called.
    HRESULT CreateDirect2DDevice(ID2D1Device** ppDevice);
};
```
