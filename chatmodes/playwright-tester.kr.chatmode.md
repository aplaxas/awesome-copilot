---
description: "Playwright 테스터를 위한 지침"
applyTo: "**"
---

## 테스트 작성 지침

### 코드 품질 표준

- **로케이터**: 사용자 중심, 역할 기반 로케이터(`getByRole`, `getByLabel`, `getByText` 등)를 우선적으로 사용하여 접근성과 안정성을 확보합니다. `test.step()`을 사용하여 상호작용을 그룹화하고 테스트 가독성과 보고를 개선합니다.
- **어설션**: 자동 재시도 웹 우선 어설션을 사용합니다. 이러한 어설션은 `await` 키워드로 시작합니다(예: `await expect(locator).toHaveText()`). 가시성 변화를 구체적으로 테스트하지 않는 한 `expect(locator).toBeVisible()`은 피합니다.
- **타임아웃**: Playwright의 내장 자동 대기 메커니즘을 신뢰합니다. 하드코딩된 대기나 기본 타임아웃 증가를 피합니다.
- **명확성**: 테스트 및 단계 제목을 명확히 작성하여 의도를 분명히 합니다. 복잡한 로직이나 명확하지 않은 상호작용을 설명하는 경우를 제외하고는 주석을 추가하지 않습니다.

### 테스트 구조

- **임포트**: `import { test, expect } from '@playwright/test';`로 시작합니다.
- **조직화**: 관련 테스트를 `test.describe()` 블록 아래에 그룹화합니다.
- **훅**: `describe` 블록의 모든 테스트에 공통적인 설정 작업에 대해 `beforeEach`를 사용합니다(예: 페이지 탐색).
- **제목**: `기능 - 특정 작업 또는 시나리오`와 같은 명명 규칙을 따릅니다.

### 파일 구성

- **위치**: 모든 테스트 파일을 `tests/` 디렉토리에 저장합니다.
- **명명**: `<기능-또는-페이지>.spec.ts` 명명 규칙을 사용합니다(예: `login.spec.ts`, `search.spec.ts`).
- **범위**: 주요 애플리케이션 기능 또는 페이지당 하나의 테스트 파일을 목표로 합니다.

### 어설션 모범 사례

- **UI 구조**: `toMatchAriaSnapshot`을 사용하여 컴포넌트의 접근성 트리 구조를 검증합니다. 이는 포괄적이고 접근 가능한 스냅샷을 제공합니다.
- **요소 개수**: `toHaveCount`를 사용하여 로케이터로 찾은 요소의 개수를 어설션합니다.
- **텍스트 내용**: 정확한 텍스트 일치를 위해 `toHaveText`를 사용하고, 부분 일치를 위해 `toContainText`를 사용합니다.
- **탐색**: 작업 후 페이지 URL을 확인하려면 `toHaveURL`을 사용합니다.

## 예제 테스트 구조

```typescript
import { test, expect } from "@playwright/test";

test.describe("영화 검색 기능", () => {
  test.beforeEach(async ({ page }) => {
    // 각 테스트 전에 애플리케이션으로 이동
    await page.goto("https://debs-obrien.github.io/playwright-movies-app");
  });

  test("제목으로 영화 검색", async ({ page }) => {
    await test.step("검색 활성화 및 수행", async () => {
      await page.getByRole("search").click();
      const searchInput = page.getByRole("textbox", { name: "Search Input" });
      await searchInput.fill("Garfield");
      await searchInput.press("Enter");
    });

    await test.step("검색 결과 확인", async () => {
      // 검색 결과의 접근성 트리를 확인합니다
      await expect(page.getByRole("main")).toMatchAriaSnapshot(`
        - main:
          - heading "Garfield" [level=1]
          - heading "search results" [level=2]
          - list "movies":
            - listitem "movie":
              - link "poster of The Garfield Movie The Garfield Movie rating":
                - /url: /playwright-movies-app/movie?id=tt5779228&page=1
                - img "poster of The Garfield Movie"
                - heading "The Garfield Movie" [level=2]
      `);
    });
  });
});
```

## 테스트 실행 전략

1. **초기 실행**: `npx playwright test --project=chromium`으로 테스트를 실행합니다.
2. **실패 디버그**: 테스트 실패를 분석하고 근본 원인을 식별합니다.
3. **반복**: 로케이터, 어설션 또는 테스트 로직을 필요에 따라 수정합니다.
4. **검증**: 테스트가 일관되게 통과하고 의도한 기능을 커버하는지 확인합니다.
5. **보고**: 테스트 결과 및 발견된 문제에 대한 피드백을 제공합니다.

## 품질 체크리스트

테스트를 최종 확정하기 전에 다음을 확인하세요:

- [ ] 모든 로케이터가 접근 가능하고 구체적이며 엄격 모드 위반을 피합니다.
- [ ] 테스트가 논리적으로 그룹화되고 명확한 구조를 따릅니다.
- [ ] 어설션이 의미 있고 사용자 기대를 반영합니다.
- [ ] 테스트가 일관된 명명 규칙을 따릅니다.
- [ ] 코드가 적절히 포맷되고 주석이 추가되었습니다.
