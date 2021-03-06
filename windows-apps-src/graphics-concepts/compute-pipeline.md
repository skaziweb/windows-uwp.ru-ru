---
title: Конвейер вычислений
description: Конвейер вычислений Direct3D предназначен для обработки вычислений, которые в большинстве случаев можно выполнять параллельно графическому конвейеру.
ms.assetid: 355B66C6-C0DF-47BA-A9C9-7AFA50B5B614
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 911546f1c2973a79aea4b597a47352149a4e4210
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651119"
---
# <a name="compute-pipeline"></a>Конвейер вычислений


\[Некоторая информация имеет отношение к предварительному выпуску продукта, который может быть значительно изменен перед коммерческим выпуском. Майкрософт не предоставляет никаких гарантий, явных или подразумеваемых, относительно предоставленной здесь информации.\]


Конвейер вычислений Direct3D предназначен для обработки вычислений, которые в большинстве случаев можно выполнять параллельно графическому конвейеру. В конвейере вычислений всего несколько шагов: данные перемещаются из точки входа в точку входа через этап программируемого шейдера вычислений.

| | |
|-|-|
|Описание|Как и другие программируемые шейдеры, [этап шейдера вычислений (CS)](compute-shader-stage--cs-.md) предназначен для HLSL и реализован с помощью этого языка. Шейдер вычислений позволяет выполнять высокоскоростные универсальные вычисления и использует большое количество параллельных процессоров в графическом процессоре. Шейдер вычислений предоставляет функции совместного использования памяти и синхронизации потоков, чтобы разрешить использование более эффективных методов параллельного программирования.|
|Input|В отличие от других программируемых шейдеров определение ввода является абстрактным. Ввод может иметь одно, два или три измерения, что определяет количество вызовов шейдера вычислений для выполнения. Можно определить общие данные для считывания одним набором вызовов.|
|Выходные данные|Выходные данные из шейдера вычислений, которые могут варьироваться достаточно сильно, можно синхронизировать с конвейером отрисовки графики, если требуются вычисленные данные.|
| | |




<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">Like other programmable shaders, <a href="#compute-shader-stage--cs-.md">Compute Shader (CS) stage</a> is designed and implemented with HLSL. A compute shader provides high-speed general purpose computing and takes advantage of the large numbers of parallel processors on the graphics processing unit (GPU). The compute shader provides memory sharing and thread synchronization features to allow more effective parallel programming methods.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">Unlike other programmable shaders, the definition of input is abstract. The input can be one, two or three-dimensional in nature, determining the number of invocations of the compute shader to execute. It is possible to define shared data for one set of invocations to read.</td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">Output data from the compute shader, which can be highly varied, can be synchronized with the graphics rendering pipeline when the computed data is required.</td>
</tr>
</tbody>
</table>
-->

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Связанные разделы


[Руководство по обучения графики Direct3D](index.md)

 

 
