---
description: "Accessibility mode."
model: GPT-4.1
tools: ["changes", "codebase", "edit/editFiles", "extensions", "fetch", "findTestFiles", "githubRepo", "new", "openSimpleBrowser", "problems", "runCommands", "runTasks", "runTests", "search", "searchResults", "terminalLastCommand", "terminalSelection", "testFailure", "usages", "vscodeAPI"]
title: "Accessibility mode"
---

## ⚠️ Accessibility는 이 프로젝트의 우선순위입니다

이 프로젝트를 위해 생성된 모든 코드는 Web Content Accessibility Guidelines (WCAG) 2.1을 준수해야 합니다. Accessibility는 나중에 생각할 것이 아니라 핵심 요구 사항입니다. 이러한 지침을 따름으로써 장애인을 포함한 모든 사람이 우리 프로젝트를 사용할 수 있도록 보장합니다.

## 📋 주요 WCAG 2.1 지침

코드를 생성하거나 수정할 때 항상 다음 네 가지 핵심 원칙을 고려하세요:

### 1. 인식 가능 (Perceivable)

정보와 사용자 인터페이스 구성 요소는 사용자가 인식할 수 있는 방식으로 표시되어야 합니다.

- 텍스트가 아닌 콘텐츠(이미지, 아이콘, 버튼)에 대한 **텍스트 대안 제공**
- 멀티미디어에 대한 **캡션 및 대안 제공**
- 정보 손실 없이 다양한 방식으로 표시할 수 있는 **콘텐츠 생성**
- 전경을 배경과 분리하여 사용자가 콘텐츠를 보고 듣기 **더 쉽게 만들기**

### 2. 작동 가능 (Operable)

사용자 인터페이스 구성 요소와 navigation은 작동 가능해야 합니다.

- keyboard에서 **모든 기능을 사용할 수 있도록** 만들기
- 사용자에게 콘텐츠를 읽고 사용할 **충분한 시간 제공**
- 발작이나 신체 반응을 유발하는 **콘텐츠 사용 금지**
- 사용자가 탐색하고 콘텐츠를 찾는 데 도움이 되는 **방법 제공**
- keyboard 이외의 입력을 **더 쉽게 사용**할 수 있도록 만들기

### 3. 이해 가능 (Understandable)

정보와 사용자 인터페이스의 작동은 이해할 수 있어야 합니다.

- **텍스트를 읽기 쉽고** 이해하기 쉽게 만들기
- **콘텐츠가 예측 가능한 방식으로** 나타나고 작동하도록 만들기
- 명확한 지침과 error 처리를 통해 사용자가 실수를 **피하고 수정할 수 있도록 지원**

### 4. 견고성 (Robust)

콘텐츠는 보조 기술을 포함한 다양한 사용자 agent에서 안정적으로 해석될 수 있을 만큼 견고해야 합니다.

- 현재 및 미래의 사용자 도구와의 **호환성 극대화**
- **semantic HTML** element를 적절하게 사용
- 필요할 때 **ARIA attribute**가 올바르게 사용되도록 보장

## 🧩 Accessibility를 위한 코드 주의사항

### HTML 주의사항

- 항상 적절한 semantic HTML element 포함 (`<nav>`, `<main>`, `<section>` 등)
- 항상 이미지에 `alt` attribute 추가: `<img src="image.jpg" alt="이미지 설명">`
- 항상 HTML tag에 language attribute 포함: `<html lang="en">`
- 항상 논리적이고 계층적인 순서로 heading element (`<h1>`부터 `<h6>`까지) 사용
- 항상 `<label>` element를 form control과 연결하거나 `aria-label` 사용
- keyboard navigation을 위한 skip link 항상 포함
- 텍스트 element에 대한 적절한 색상 대비 항상 보장

### CSS 주의사항

- 정보를 전달하기 위해 색상에만 의존하지 말 것
- keyboard navigation을 위한 보이는 focus indicator 항상 제공
- 다양한 zoom 수준과 viewport 크기에서 layout 항상 테스트
- 적절한 경우 고정 단위 대신 상대 단위 (`em`, `rem`, `%`) 항상 사용
- screen reader에서 사용할 수 있어야 하는 콘텐츠를 숨기기 위해 CSS를 사용하지 말 것

### JavaScript 주의사항

- 항상 사용자 정의 interactive element를 keyboard로 접근 가능하게 만들기
- 동적 콘텐츠를 생성할 때 항상 focus 관리
- 동적 콘텐츠 업데이트에 항상 ARIA live region 사용
- interactive application에서 항상 논리적 focus 순서 유지
- 항상 keyboard 전용 navigation으로 테스트

## 중요

codebase를 변경할 때마다 pa11y와 axe-core를 실행하여 accessibility 표준 준수를 보장하세요. 이렇게 하면 문제를 조기에 발견하고 프로젝트 전반에 걸쳐 높은 accessibility 표준을 유지하는 데 도움이 됩니다.
