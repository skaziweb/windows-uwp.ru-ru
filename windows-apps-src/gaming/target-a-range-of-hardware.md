---
title: Поддержка карт теней на различном оборудовании
description: Более высокое качество отображения теней на быстрых устройствах и быстрое отображение на менее мощных устройствах.
ms.assetid: d97c0544-44f2-4e29-5e02-54c45e0dff4e
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, игры, карты теней, directx
ms.localizationpriority: medium
ms.openlocfilehash: 1087a063fa19bea716b86143c10097711cef9205
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/29/2019
ms.locfileid: "66367899"
---
# <a name="support-shadow-maps-on-a-range-of-hardware"></a>Поддержка карт теней на различном оборудовании




Более высокое качество отображения теней на быстрых устройствах и быстрое отображение на менее мощных устройствах. Часть 4 из [Пошаговое руководство: Реализовать тома с помощью буферов глубины в Direct3D 11](implementing-depth-buffers-for-shadow-mapping.md).

## <a name="comparison-filter-types"></a>Типы фильтров сравнения


Линейную фильтрацию используйте только на устройствах, где допустимо снижение производительности. Как правило, Direct3D функциональный уровень 9\_1 устройства не имеют достаточно возможностей, чтобы сэкономить на стоимости для линейной фильтрации на теней. На таких устройствах используйте точечную фильтрацию. Если используется линейная фильтрация, настройте пиксельный шейдер так, чтобы он обрабатывал переход границ тени.

Создайте сравнительный образец для точечной фильтрации:

```cpp
D3D11_SAMPLER_DESC comparisonSamplerDesc;
ZeroMemory(&comparisonSamplerDesc, sizeof(D3D11_SAMPLER_DESC));
comparisonSamplerDesc.AddressU = D3D11_TEXTURE_ADDRESS_BORDER;
comparisonSamplerDesc.AddressV = D3D11_TEXTURE_ADDRESS_BORDER;
comparisonSamplerDesc.AddressW = D3D11_TEXTURE_ADDRESS_BORDER;
comparisonSamplerDesc.BorderColor[0] = 1.0f;
comparisonSamplerDesc.BorderColor[1] = 1.0f;
comparisonSamplerDesc.BorderColor[2] = 1.0f;
comparisonSamplerDesc.BorderColor[3] = 1.0f;
comparisonSamplerDesc.MinLOD = 0.f;
comparisonSamplerDesc.MaxLOD = D3D11_FLOAT32_MAX;
comparisonSamplerDesc.MipLODBias = 0.f;
comparisonSamplerDesc.MaxAnisotropy = 0;
comparisonSamplerDesc.ComparisonFunc = D3D11_COMPARISON_LESS_EQUAL;
comparisonSamplerDesc.Filter = D3D11_FILTER_COMPARISON_MIN_MAG_MIP_POINT;

// Point filtered shadows can be faster, and may be a good choice when
// rendering on hardware with lower feature levels. This sample has a
// UI option to enable/disable filtering so you can see the difference
// in quality and speed.

DX::ThrowIfFailed(
    pD3DDevice->CreateSamplerState(
        &comparisonSamplerDesc,
        &m_comparisonSampler_point
        )
    );
```

Затем создайте образец для линейной фильтрации:

```cpp
comparisonSamplerDesc.Filter = D3D11_FILTER_COMPARISON_MIN_MAG_MIP_LINEAR;
DX::ThrowIfFailed(
    pD3DDevice->CreateSamplerState(
        &comparisonSamplerDesc,
        &m_comparisonSampler_linear
        )
    );
```

Выберите образец:

```cpp
ID3D11PixelShader* pixelShader;
ID3D11SamplerState** comparisonSampler;
if (m_useLinear)
{
    pixelShader = m_shadowPixelShader_linear.Get();
    comparisonSampler = m_comparisonSampler_linear.GetAddressOf();
}
else
{
    pixelShader = m_shadowPixelShader_point.Get();
    comparisonSampler = m_comparisonSampler_point.GetAddressOf();
}

// Attach our pixel shader.
context->PSSetShader(
    pixelShader,
    nullptr,
    0
    );

context->PSSetSamplers(0, 1, comparisonSampler);
context->PSSetShaderResources(0, 1, m_shadowResourceView.GetAddressOf());
```

Обработайте границы тени с помощью линейной фильтрации:

```cpp
// Blends the shadow area into the lit area.
float3 light = lighting * (ambient + DplusS(N, L, NdotL, input.view));
float3 shadow = (1.0f - lighting) * ambient;
return float4(input.color * (light + shadow), 1.f);
```

## <a name="shadow-buffer-size"></a>Размер буфера теней


Крупные карты теней выглядят менее угловато, но занимают больше графической памяти. Попробуйте в своей игре карты теней разных размеров и сравните результаты на разных устройствах с разными размерами экранов. Чтобы добиться лучшего результата при меньшей загрузке графической памяти, попробуйте применить оптимизацию, например реализовать каскадные карты теней. См. раздел [Распространенные методики улучшения карт глубины теней](https://docs.microsoft.com/windows/desktop/DxTechArts/common-techniques-to-improve-shadow-depth-maps).

## <a name="shadow-buffer-depth"></a>Глубина буфера теней


Более высокая точность буфера теней дает более достоверные результаты проверки глубины и позволяет избежать конфликта z-буфера и других проблем. Но, как и большие карты теней, при повышенной точности буфера используется больше памяти. Поэкспериментируйте с типами точности разных глубины в игре - DXGI\_ФОРМАТ\_R24G8\_TYPELESS и DXGI\_ФОРМАТ\_R16\_TYPELESS - и посмотрите на скорость и качество различные функциональные уровни.

## <a name="optimizing-precompiled-shaders"></a>Оптимизация предварительно скомпилированных шейдеров


Приложения универсальной платформы Windows (UWP) могут использовать динамическую компиляцию шейдеров, но быстрее использовать динамическое связывание шейдеров. Можно также использовать директивы компилятора и блоки `#ifdef` для создания различных версий шейдеров. Для этого нужно открыть файл проекта Visual Studio в текстовом редакторе и добавить несколько записей `<FxcCompiler>` для HLSL (каждая с соответствующими определениями препроцессора). Обратите внимание, что это требуются разные имена файлов; в этом случае Visual Studio добавляет \_точки и \_линейной к разным версиям шейдера.

В записи файла проекта для версии с линейной фильтрацией шейдера определяется LINEAR:

```xml
<FxCompile Include="Content\ShadowPixelShader.hlsl">
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Release|ARM'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Release|Win32'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Debug|x64'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Release|x64'">Pixel</ShaderType>
  <DisableOptimizations Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">false</DisableOptimizations>
  <EnableDebuggingInformation Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">true</EnableDebuggingInformation>
  <DisableOptimizations Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">false</DisableOptimizations>
  <DisableOptimizations Condition="'$(Configuration)|$(Platform)'=='Debug|x64'">false</DisableOptimizations>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">$(OutDir)%(Filename)_linear.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Release|ARM'">$(OutDir)%(Filename)_linear.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">$(OutDir)%(Filename)_linear.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Release|Win32'">$(OutDir)%(Filename)_linear.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Debug|x64'">$(OutDir)%(Filename)_linear.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Release|x64'">$(OutDir)%(Filename)_linear.cso</ObjectFileOutput>
  <PreprocessorDefinitions Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">LINEAR</PreprocessorDefinitions>
  <PreprocessorDefinitions Condition="'$(Configuration)|$(Platform)'=='Release|ARM'">LINEAR</PreprocessorDefinitions>
  <PreprocessorDefinitions Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">LINEAR</PreprocessorDefinitions>
  <PreprocessorDefinitions Condition="'$(Configuration)|$(Platform)'=='Release|Win32'">LINEAR</PreprocessorDefinitions>
  <PreprocessorDefinitions Condition="'$(Configuration)|$(Platform)'=='Debug|x64'">LINEAR</PreprocessorDefinitions>
  <PreprocessorDefinitions Condition="'$(Configuration)|$(Platform)'=='Release|x64'">LINEAR</PreprocessorDefinitions>
</FxCompile>
```

В записи файла проекта для версии с линейной фильтрацией не задаются определения препроцессора:

```xml
<FxCompile Include="Content\ShadowPixelShader.hlsl">
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Release|ARM'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Release|Win32'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Debug|x64'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Release|x64'">Pixel</ShaderType>
  <DisableOptimizations Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">false</DisableOptimizations>
  <EnableDebuggingInformation Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">true</EnableDebuggingInformation>
  <DisableOptimizations Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">false</DisableOptimizations>
  <DisableOptimizations Condition="'$(Configuration)|$(Platform)'=='Debug|x64'">false</DisableOptimizations>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">$(OutDir)%(Filename)_point.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Release|ARM'">$(OutDir)%(Filename)_point.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">$(OutDir)%(Filename)_point.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Release|Win32'">$(OutDir)%(Filename)_point.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Debug|x64'">$(OutDir)%(Filename)_point.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Release|x64'">$(OutDir)%(Filename)_point.cso</ObjectFileOutput>
</FxCompile>
```

 

 




