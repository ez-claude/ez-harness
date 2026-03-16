# Harness Effect Measurement (promptfoo)

역추적 규칙의 실제 효과를 측정하는 promptfoo 테스트.

## 측정 목적

"역추적 규칙을 추가했을 때 Claude 응답이 실제로 바뀌는가?"

| 변형 | 설명 |
|------|------|
| 하네스 OFF | 단순 "이 버그 고쳐줘" |
| 하네스 ON | 역추적 3단계(증상/경로/수정위치) 강제 후 수정 |

## 테스트 케이스

실제 발생한 버그 10개 (patterns.json 기반):

| # | 케이스 | 근본원인 유형 |
|---|--------|-------------|
| TC01 | 파일 확장자 하드코딩 | 설계 결함 |
| TC02 | Windows cp949 vs UTF-8 충돌 | 환경 특화 |
| TC03 | 부분 Edit으로 기능 손실 | 프로세스 결함 |
| TC04 | try 블록 안 표준 라이브러리 import | 스코프 오해 |
| TC05 | import os 누락 | 단순 누락 |
| TC06 | JSON 배열 덮어쓰기 | 도구 사용 오류 |
| TC07 | TypeScript 타입 일부 누락 | 전파 누락 |
| TC08 | 배치 선택 항상 동일 50개 | 스키마 불일치 |
| TC09 | API 키 하드코딩 | 보안 위반 |
| TC10 | subprocess 버퍼링 | 환경 특화 |

## 실행 방법

```bash
# ANTHROPIC_API_KEY 설정 필요
export ANTHROPIC_API_KEY=sk-ant-...

cd tests/
promptfoo eval

# 웹 UI로 결과 확인
promptfoo view
```

## 평가 기준

각 케이스에 `llm-rubric` 평가자 적용:
- 근본원인 식별 여부 (증상 패치 vs 근본 수정)
- 수정 방향의 정확성

## 한계

- promptfoo는 프롬프트 수준 테스트 (프로덕션 효과 검증 아님)
- "역추적 규칙이 응답 품질을 바꾸는가"를 측정하는 도구
- 실제 효과는 제어 실험(실 세션 비교)에서 측정 필요
