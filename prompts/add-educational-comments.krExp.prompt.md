> This document is a Korean explanatory version of the original file.  
> Technical terms, code, and configuration values remain in English.

# Add Educational Comments

_(설명)_ 이 섹션은 코드에 교육적인 주석을 추가하는 방법과 그 목적을 설명합니다. 주석은 코드의 가독성을 높이고, 다른 개발자들이 코드의 의도를 더 잘 이해할 수 있도록 돕습니다.

## Purpose

_(설명)_ 이 섹션은 주석의 목적과 코드에서 주석이 왜 중요한지를 다룹니다. 주석은 다음과 같은 이유로 중요합니다:

- 코드의 의도를 명확히 함
- 복잡한 로직을 설명
- 팀 협업을 촉진

### Key Benefits

_(설명)_ 주석을 추가함으로써 얻을 수 있는 주요 이점은 다음과 같습니다:

- 코드 유지보수성 향상
- 새로운 팀원이 코드를 더 빨리 이해할 수 있음
- 코드 리뷰 과정에서 의사소통 개선

```code
// Example of an educational comment
// This function calculates the factorial of a number recursively.
function factorial(n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1);
}
```

## Best Practices

_(설명)_ 주석을 작성할 때 따라야 할 모범 사례를 설명합니다. 다음은 몇 가지 권장 사항입니다:

- 간결하고 명확하게 작성
- 코드의 '무엇'이 아닌 '왜'를 설명
- 최신 상태로 유지

### Common Pitfalls

_(설명)_ 주석 작성 시 피해야 할 일반적인 실수를 다룹니다:

- 너무 많은 주석으로 코드 가독성 저하
- 코드와 불일치하는 오래된 주석
- 명확하지 않은 설명

```code
// Bad example: This comment is redundant
// Increment the counter by 1
counter++;

// Good example: Explains why the counter is incremented
// Increment the counter to track the number of user actions
counter++;
```

## Conclusion

_(설명)_ 이 섹션은 교육적인 주석을 추가하는 것이 코드 품질과 팀 협업에 미치는 긍정적인 영향을 요약합니다. 주석은 코드의 가치를 높이는 중요한 도구입니다.
