---
title: "Unity URP Custom Shader 기초"
date: 2026-04-05
draft: false
categories: ["Unity"]
tags: ["Shader", "URP", "HLSL", "렌더링"]
series: "URP 셰이더 입문"
summary: "URP 환경에서 커스텀 Unlit 셰이더를 작성하는 방법을 정리합니다"
unity_version: "6.0"
render_pipeline: "URP"
difficulty: "intermediate"
estimated_read: "10min"
---

## 개요
> Unity 6 URP 환경에서 ShaderLab + HLSL로 커스텀 Unlit 셰이더를 작성하는 기본 구조를 정리합니다.

---

## 환경
- **유니티 버전:** Unity 6.0
- **렌더 파이프라인:** URP 17.x
- **언어:** ShaderLab / HLSL

---

## 문제 상황
Built-in 파이프라인용 셰이더를 URP로 이전하면서 기존 문법이 동작하지 않는 문제.
URP 전용 구조로 재작성 필요.

---

## 해결 방법

### 기본 URP Unlit 셰이더 구조

```hlsl
Shader "Custom/MyUnlit"
{
    Properties
    {
        _BaseColor ("Base Color", Color) = (1,1,1,1)
        _BaseMap ("Base Texture", 2D) = "white" {}
    }

    SubShader
    {
        Tags { "RenderType"="Opaque" "RenderPipeline"="UniversalPipeline" }

        Pass
        {
            HLSLPROGRAM
            #pragma vertex vert
            #pragma fragment frag
            #include "Packages/com.unity.render-pipelines.universal/ShaderLibrary/Core.hlsl"

            CBUFFER_START(UnityPerMaterial)
                float4 _BaseColor;
                float4 _BaseMap_ST;
            CBUFFER_END

            TEXTURE2D(_BaseMap);
            SAMPLER(sampler_BaseMap);

            struct Attributes {
                float4 positionOS : POSITION;
                float2 uv : TEXCOORD0;
            };

            struct Varyings {
                float4 positionHCS : SV_POSITION;
                float2 uv : TEXCOORD0;
            };

            Varyings vert(Attributes IN) {
                Varyings OUT;
                OUT.positionHCS = TransformObjectToHClip(IN.positionOS.xyz);
                OUT.uv = TRANSFORM_TEX(IN.uv, _BaseMap);
                return OUT;
            }

            half4 frag(Varyings IN) : SV_Target {
                half4 tex = SAMPLE_TEXTURE2D(_BaseMap, sampler_BaseMap, IN.uv);
                return tex * _BaseColor;
            }
            ENDHLSL
        }
    }
}
```

---

## 핵심 포인트
- Built-in의 `UnityObjectToClipPos` → URP에서는 `TransformObjectToHClip`
- `#include "UnityCG.cginc"` → `#include ".../Core.hlsl"`
- `CBUFFER_START` 블록으로 SRP Batcher 호환성 확보

---

## 결과
URP에서 정상 렌더링 확인, SRP Batcher 호환 상태.

---

## 참고
- [Unity URP ShaderLibrary 공식 문서](https://docs.unity3d.com/Packages/com.unity.render-pipelines.universal@17.0/manual/)
