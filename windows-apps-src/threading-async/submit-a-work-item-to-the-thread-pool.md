---
ms.assetid: E2A1200C-9583-40FA-AE4D-C9E6F6C32BCF
title: Отправка рабочего элемента в пул потоков
description: Узнайте, как выполнить работу в отдельном потоке, отправив рабочий элемент в пул потоков.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, потоки, пул потоков
ms.localizationpriority: medium
ms.openlocfilehash: a9da63e05380987d69d97a74123e593acd0b8cb1
ms.sourcegitcommit: 2dbf4a3f3473c1d3a0ad988bcbae6e75dfee3640
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/01/2020
ms.locfileid: "82619348"
---
# <a name="submit-a-work-item-to-the-thread-pool"></a>Отправка рабочего элемента в пул потоков

\[Обновлено для приложений UWP в Windows 10. Статьи по Windows 8. x см. в [архиве](https://docs.microsoft.com/previous-versions/windows/apps/mt244353(v=win.10)?redirectedfrom=MSDN)\]

<b>Важные API</b>

-   [**RunAsync**](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpool.runasync)
-   [**IAsyncAction**](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncAction)

Узнайте, как выполнить работу в отдельном потоке, отправив рабочий элемент в пул потоков. Используйте этот способ для поддержки реагирующих элементов пользовательского интерфейса при выполнении работы, требующей заметного количества времени, и параллельного выполнения нескольких задач.

## <a name="create-and-submit-the-work-item"></a>Создание и отправка рабочего элемента

Создайте рабочий элемент, вызвав [**RunAsync**](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpool.runasync). Предоставьте делегат, выполняющий работу (можно использовать лямбда-функцию или функцию-делегат). Обратите внимание, что функция **RunAsync** возвращает объект [**IAsyncAction**](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncAction). Сохраните его для использования на следующем шаге.

Доступны три версии функции [**RunAsync**](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpool.runasync), поэтому вы можете при необходимости задать приоритет рабочего элемента и контролировать, выполняется ли он параллельно с другими рабочими элементами.

>[!NOTE]
>Используйте [**CoreDispatcher. RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) для доступа к ПОТОКУ пользовательского интерфейса и отображения хода выполнения рабочего элемента.

В примере ниже создается рабочий элемент и предоставляется лямбда-функция для выполнения работы.

```csharp
// The nth prime number to find.
const uint n = 9999;

// A shared pointer to the result.
// We use a shared pointer to keep the result alive until the
// thread is done.
ulong nthPrime = 0;

// Simulates work by searching for the nth prime number. Uses a
// naive algorithm and counts 2 as the first prime number.
IAsyncAction asyncAction = Windows.System.Threading.ThreadPool.RunAsync(
    (workItem) =>
{
    uint  progress = 0; // For progress reporting.
    uint  primes = 0;   // Number of primes found so far.
    ulong i = 2;        // Number iterator.

    if ((n >= 0) && (n <= 2))
    {
        nthPrime = n;
        return;
    }

    while (primes < (n - 1))
    {
        if (workItem.Status == AsyncStatus.Canceled)
        {
            break;
        }

        // Go to the next number.
        i++;

        // Check for prime.
        bool prime = true;
        for (uint j = 2; j < i; ++j)
        {
            if ((i % j) == 0)
            {
                prime = false;
                break;
            }
        };

        if (prime)
        {
            // Found another prime number.
            primes++;

            // Report progress at every 10 percent.
            uint temp = progress;
            progress = (uint)(10.0*primes/n);

            if (progress != temp)
            {
                String updateString;
                updateString = "Progress to " + n + "th prime: "
                    + (10 * progress) + "%\n";

                // Update the UI thread with the CoreDispatcher.
                CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(
                    CoreDispatcherPriority.High,
                    new DispatchedHandler(() =>
                {
                    UpdateUI(updateString);
                }));
            }
        }
    }

    // Return the nth prime number.
    nthPrime = i;
});

// A reference to the work item is cached so that we can trigger a
// cancellation when the user presses the Cancel button.
m_workItem = asyncAction;
```

```cppwinrt
// The nth prime number to find.
const unsigned int n{ 9999 };

unsigned long nthPrime{ 0 };

// Simulates work by searching for the nth prime number. Uses a
// naive algorithm and counts 2 as the first prime number.

// A reference to the work item is cached so that we can trigger a
// cancellation when the user presses the Cancel button.
m_workItem = Windows::System::Threading::ThreadPool::RunAsync([&](Windows::Foundation::IAsyncAction const& workItem)
{
    unsigned int progress = 0; // For progress reporting.
    unsigned int primes = 0;   // Number of primes found so far.
    unsigned long int i = 2;   // Number iterator.

    if ((n >= 0) && (n <= 2))
    {
        nthPrime = n;
        return;
    }

    while (primes < (n - 1))
    {
        if (workItem.Status() == Windows::Foundation::AsyncStatus::Canceled)
        {
            break;
        }

        // Go to the next number.
        i++;

        // Check for prime.
        bool prime = true;
        for (unsigned int j = 2; j < i; ++j)
        {
            if ((i % j) == 0)
            {
                prime = false;
                break;
            }
        };

        if (prime)
        {
            // Found another prime number.
            primes++;

            // Report progress at every 10 percent.
            unsigned int temp = progress;
            progress = static_cast<unsigned int>(10.f*primes / n);

            if (progress != temp)
            {
                std::wstringstream updateString;
                updateString << L"Progress to " << n << L"th prime: " << (10 * progress) << std::endl;

                // Update the UI thread with the CoreDispatcher.
                Windows::ApplicationModel::Core::CoreApplication::MainView().CoreWindow().Dispatcher().RunAsync(
                    Windows::UI::Core::CoreDispatcherPriority::High,
                    Windows::UI::Core::DispatchedHandler([&]()
                {
                    UpdateUI(updateString.str());
                }));
            }
        }
    }
    // Return the nth prime number.
    nthPrime = i;
});
```

```cpp
// The nth prime number to find.
const unsigned int n = 9999;

// A shared pointer to the result.
// We use a shared pointer to keep the result alive until the
// thread is done.
std::shared_ptr<unsigned long> nthPrime = std::make_shared<unsigned long int>(0);

// Simulates work by searching for the nth prime number. Uses a
// naive algorithm and counts 2 as the first prime number.
auto workItem = ref new Windows::System::Threading::WorkItemHandler(
    \[this, n, nthPrime](IAsyncAction^ workItem)
{
    unsigned int progress = 0; // For progress reporting.
    unsigned int primes = 0;   // Number of primes found so far.
    unsigned long int i = 2;   // Number iterator.

    if ((n >= 0) && (n <= 2))
    {
        *nthPrime = n;
        return;
    }

    while (primes < (n - 1))
    {
        if (workItem->Status == AsyncStatus::Canceled)
       {
           break;
       }

       // Go to the next number.
       i++;

       // Check for prime.
       bool prime = true;
       for (unsigned int j = 2; j < i; ++j)
       {
           if ((i % j) == 0)
           {
               prime = false;
               break;
           }
       };

       if (prime)
       {
           // Found another prime number.
           primes++;

           // Report progress at every 10 percent.
           unsigned int temp = progress;
           progress = static_cast<unsigned int>(10.f*primes / n);

           if (progress != temp)
           {
               String^ updateString;
               updateString = "Progress to " + n + "th prime: "
                   + (10 * progress).ToString() + "%\n";

               // Update the UI thread with the CoreDispatcher.
               CoreApplication::MainView->CoreWindow->Dispatcher->RunAsync(
                   CoreDispatcherPriority::High,
                   ref new DispatchedHandler([this, updateString]()
               {
                   UpdateUI(updateString);
               }));
           }
       }
   }
   // Return the nth prime number.
   *nthPrime = i;
});

auto asyncAction = ThreadPool::RunAsync(workItem);

// A reference to the work item is cached so that we can trigger a
// cancellation when the user presses the Cancel button.
m_workItem = asyncAction;
```

После вызова [**RunAsync**](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpool.runasync) рабочий элемент помещается в очередь пулом потоков и выполняется, когда становится доступным поток. Рабочие элементы в пуле потоков выполняются асинхронно и в любом порядке, поэтому обеспечьте независимое функционирование своих рабочих элементов.

Обратите внимание, что рабочий элемент проверяет значение свойства [**IAsyncInfo.Status**](https://docs.microsoft.com/uwp/api/windows.foundation.iasyncinfo.status) и завершает работу при отмене рабочего элемента.

## <a name="handle-work-item-completion"></a>Обработка завершения рабочего элемента

Предоставьте обработчик завершения, задав свойство [**IAsyncAction.Completed**](https://docs.microsoft.com/uwp/api/windows.foundation.iasyncaction.completed) рабочего элемента. Предоставьте делегат (можно использовать лямбда-функцию или функцию-делегат) для обработки завершения рабочего элемента. Например, используйте [**CoreDispatcher.RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) для доступа к потоку пользовательского интерфейса и отображения результатов.

В следующем примере пользовательский интерфейс обновляется на основе результатов выполнения рабочего элемента, отправленного на шаге 1:

```cpp
asyncAction->Completed = ref new AsyncActionCompletedHandler(
    \[this, n, nthPrime](IAsyncAction^ asyncInfo, AsyncStatus asyncStatus)
{
    if (asyncStatus == AsyncStatus::Canceled)
    {
        return;
    }

    String^ updateString;
    updateString = "\n" + "The " + n + "th prime number is "
        + (*nthPrime).ToString() + ".\n";

    // Update the UI thread with the CoreDispatcher.
    CoreApplication::MainView->CoreWindow->Dispatcher->RunAsync(
        CoreDispatcherPriority::High,
        ref new DispatchedHandler([this, updateString]()
    {
        UpdateUI(updateString);
    }));
});
```

```cppwinrt
m_workItem.Completed([&](Windows::Foundation::IAsyncAction const& asyncInfo, Windows::Foundation::AsyncStatus const& asyncStatus)
{
    if (asyncStatus == Windows::Foundation::AsyncStatus::Canceled)
    {
        return;
    }

    std::wstringstream updateString;
    updateString << std::endl << L"The " << n << L"th prime number is " << nthPrime << std::endl;

    // Update the UI thread with the CoreDispatcher.
    Windows::ApplicationModel::Core::CoreApplication::MainView().CoreWindow().Dispatcher().RunAsync(
        Windows::UI::Core::CoreDispatcherPriority::High,
        Windows::UI::Core::DispatchedHandler([&]()
    {
        UpdateUI(updateString.str());
    }));
});
```

```c#
asyncAction.Completed = new AsyncActionCompletedHandler(
    (IAsyncAction asyncInfo, AsyncStatus asyncStatus) =>
{
    if (asyncStatus == AsyncStatus.Canceled)
    {
        return;
    }

    String updateString;
    updateString = "\n" + "The " + n + "th prime number is "
        + nthPrime + ".\n";

    // Update the UI thread with the CoreDispatcher.
    CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(
        CoreDispatcherPriority.High,
        new DispatchedHandler(()=>
    {
        UpdateUI(updateString);
    }));
});
```

Обратите внимание, что обработчик завершения проверяет, был ли отменен рабочий элемент, перед тем как выполнять обновление пользовательского интерфейса.

## <a name="summary-and-next-steps"></a>Сводка и дальнейшие действия

Дополнительные сведения можно узнать, загрузив код из этого краткого руководства, в [примере создания рабочего элемента ThreadPool](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Thread%20pool%20sample) , написанного для Windows 8.1, и повторно использовать исходный код в приложении\_Win УНАП Windows 10.

## <a name="related-topics"></a>Связанные темы

* [Отправка рабочего элемента в пул потоков](submit-a-work-item-to-the-thread-pool.md)
* [Рекомендации по использованию пула потоков](best-practices-for-using-the-thread-pool.md)
* [Отправка рабочего элемента по таймеру](use-a-timer-to-submit-a-work-item.md)
 
