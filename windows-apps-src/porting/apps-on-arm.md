---
title: Windows 10 на архитектуре ARM
description: В этой статье представлен обзор поведения различных взаимодействий и приложений в ARM, а также ограничений, приводятся ссылки на источники дополнительной информации.
ms.date: 02/15/2018
ms.topic: article
keywords: windows 10 s, постоянно подключенный, ARM, ARM64, эмуляция x86
ms.localizationpriority: medium
ms.openlocfilehash: 1a72fbaf4982f2a053298f10279eacba6a46d05d
ms.sourcegitcommit: 8a88a05ad89aa180d41a93152632413694f14ef8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726007"
---
# <a name="windows-10-on-arm"></a>Windows 10 на архитектуре ARM
Изначально Windows 10 (в отличие от Windows 10 Mobile) могла выполняться только на ПК с процессорами x86 или x64. Теперь Windows 10 Desktop может работать на компьютерах, работающих под управлением процессоров ARM64 с обновлениями или более поздними версиями. Архитектура ЦП ARM обладает энергосберегающими свойствами, что позволяет этим компьютерам работать от аккумулятора весь день и поддерживать мобильные сети передачи данных. Эти ПК обеспечивают высокую совместимость приложений и позволяют выполнять существующие приложения win32 x86 без изменений. Дополнительные сведения или демонстрация см. на [видеоролике Channel 9 для постоянного подключенного ПК](https://channel9.msdn.com/Events/Build/2017/P4171).

Мы используем здесь термин *ARM* в качестве сокращения для ПК с настольной версией Windows 10 на процессорах ARM64 (которую также называют *AArch64*).  Мы используем здесь термин *ARM32* в качестве сокращения для 32-разрядной архитектуры ARM (которую в другой документации обычно называют *ARM*).

## <a name="apps-and-experiences-on-arm"></a>Приложения и взаимодействия в ARM

### <a name="built-in-windows-10-experiences-apps-and-drivers"></a>Встроенные возможности, приложения и драйверы Windows 10
Встроенные возможности Windows 10, такие как "ребро", "Кортана", "главное меню" и "Проводник", являются собственными и выполняются как ARM64. Сюда также входят все драйверы устройств, такие как графика, сеть или жесткий диск. Это гарантирует лучшее взаимодействие с пользователем и сроком службы аккумулятора устройства, работающего на полной скорости процессора Qualcomm Снапдрагон.

### <a name="universal-windows-platform-uwp-apps"></a>Приложения универсальной платформы Windows (UWP)
Windows 10 на ARM запускает все [приложения UWP](../get-started/universal-application-platform-guide.md) для x86, ARM32 и ARM64 из Microsoft Store. Приложения ARM32 и ARM64 работают изначально без эмуляции, тогда как приложения x86 работают в режиме эмуляции. Разработчикам UWP необходимо отправить пакет ARM для своего приложения, поскольку это обеспечит максимальное удобство использования приложения на устройстве. Дополнительные сведения см. в статье [Архитектуры пакета приложения](/windows/msix/package/device-architecture).

>[!NOTE]
> Чтобы создать приложение UWP для платформы ARM64, необходимо иметь Visual Studio 2017 версии 15,9 или более поздней или Visual Studio 2019. Дополнительные сведения см. в [этой записи блога](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development).


>[!IMPORTANT]
> Windows 10 на ARM поддерживает приложения UWP x86, ARM32 и ARM64 из Store на устройствах ARM64. Когда пользователь скачивает приложение UWP на устройстве ARM64, ОС автоматически устанавливает оптимальную версию доступного приложения. Если вы отправляете в магазин версии x86, ARM32 и ARM64, ОС автоматически установит версию ARM64 приложения. Если вы отправляете только версии x86 и ARM32 для приложения, ОС установит версию ARM32. Если вы отправляете только версию x86 приложения, ОС установит эту версию и запустит ее в процессе эмуляции. Дополнительные сведения об архитектурах см. в статье [Архитектуры пакета приложения](/windows/msix/package/device-architecture).

### <a name="win32-apps"></a>Приложения Win32
Помимо приложений UWP, Windows 10 на ARM также может работать с неизмененными приложениями Win32 x86 без изменений, что обеспечивает хорошую производительность и удобство работы пользователей, как и любой компьютер. Эти приложения Win32 x86 не должны перекомпилироваться для ARM и даже не осознают, что они работают на процессоре ARM. Обратите внимание, что 64-разрядные приложения Win32 x64 не поддерживаются, но в подавляющем большинстве приложений доступны версии x86.  При выборе архитектуры приложения просто выберите 32-разрядную версию x86 для запуска приложения на Windows 10 на компьютере ARM.

## <a name="in-this-section"></a>Содержание раздела
|Раздел | Описание |
|-----|-----|
|[Принцип работы эмуляции x86 на архитектуре ARM](apps-on-arm-x86-emulation.md)|Обзор эмуляции приложений x86 в ARM.|
|[Устранение неполадок в приложениях x86 на архитектуре ARM](apps-on-arm-troubleshooting-x86.md)|Распространенные проблемы с приложениями x86 при работе в ARM и способы их устранения. |
|[Устранение неполадок приложений ARM на ARM](apps-on-arm-troubleshooting-arm32.md)|Распространенные проблемы с ARM32 и ARM64 приложениями при работе в ARM и способах их устранения. |
|[Средство устранения неполадок с совместимостью программ на архитектуре ARM](apps-on-arm-program-compat-troubleshooter.md)|Руководство по регулированию настроек совместимости в случае неправильной работы приложения в ARM. |

## <a name="related-topics"></a>Связанные разделы
|Раздел | Описание |
|-----|-----|
|[Создание драйверов ARM64 с помощью WDK](https://docs.microsoft.com/windows-hardware/drivers/develop/building-arm64-drivers)|Инструкции по созданию драйвера в ARM64. |
| [Отладка приложений x86 на ARM](https://docs.microsoft.com/windows-hardware/drivers/debugger/debugging-arm64) | Инструкции по отладке приложений x86 в ARM. |
