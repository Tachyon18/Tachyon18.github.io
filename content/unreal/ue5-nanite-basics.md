---
title: "UE5 Nanite 기초 - 메시 최적화 설정"
date: 2026-04-05
draft: false
categories: ["Unreal"]
tags: ["Nanite", "최적화", "UE5", "렌더링"]
series: "UE5 렌더링 기초"
summary: "Nanite 활성화 방법과 주의사항을 정리합니다"
engine_version: "UE5.3"
difficulty: "beginner"
estimated_read: "7min"
---

## 개요
> UE5의 Nanite 가상 지오메트리 시스템을 메시에 적용하는 방법과, 사용 시 주의해야 할 케이스를 정리합니다.

---

## 환경
- **언리얼 버전:** UE5.3
- **언어:** C++ / Blueprint 혼용
- **플러그인:** 없음 (기본 내장)

---

## 문제 상황
폴리곤 수가 많은 환경 에셋을 배치했을 때 씬 복잡도가 급격히 올라가는 문제.
LOD를 수동으로 세팅하기엔 에셋 수가 너무 많았고, Nanite 적용으로 해결할 수 있었음.

---

## 해결 방법

### 1단계 — 에셋에 Nanite 활성화

콘텐츠 브라우저에서 스태틱 메시 더블클릭 → 디테일 패널에서 `Nanite Settings` 섹션 확인

```cpp
// C++에서 런타임 활성화 (에디터 외)
UStaticMesh* Mesh = LoadObject<UStaticMesh>(...);
Mesh->NaniteSettings.bEnabled = true;
```

### 2단계 — 프로젝트 세팅 확인

`Project Settings → Rendering → Nanite` 에서 활성화 여부 체크.

---

## ⚠️ 주의사항
- **투명 머티리얼**에는 Nanite 적용 불가
- **스켈레탈 메시** 미지원 (스태틱 메시 전용)
- 모바일 플랫폼 지원 제한 있음

---

## 결과
씬 내 환경 에셋 300개 기준, 드로우콜 약 60% 감소 확인.

---

## 참고
- [UE5 공식 Nanite 문서](https://docs.unrealengine.com/5.0/en-US/nanite-virtualized-geometry-in-unreal-engine/)
