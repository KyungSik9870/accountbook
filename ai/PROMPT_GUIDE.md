# Spring + Kotlin AI Prompt Guide

## 목적
- Spring 기반 서비스의 코드/설계 품질을 AI로 보조.
- 모든 출력은 **검증 가능(JSON Schema, 코드 컴파일 가능)** 해야 함.
- 사람 리뷰 없이 프로덕션에 바로 반영되지 않도록 설계.

---

## 기본 원칙
1. **명확한 목표 + 제한된 컨텍스트 + 구조화된 출력**
    - 예: “Hikari 설정 검토” → context/db_standards.md 참고 → JSON으로 리스크 요약
2. **재현 가능성**: 같은 입력 → 항상 유사한 출력.
3. **보안 및 프라이버시**: 내부 URL, 계정, API 키 절대 포함 금지.
4. **명시적 가정**: AI가 추측한 내용은 “assumptions” 섹션에 명확히 분리.
5. **스키마 우선**: schemas/*.json 을 만족하지 않으면 실패로 간주.

---

## 출력 형식 규칙
| 유형 | 형식 | 예시 |
|------|------|------|
| 코드 리뷰 | JSON | schemas/code_review.schema.json |
| 설계 문서 | Markdown | design_doc.md 템플릿 |
| 테스트 케이스 | Kotlin 코드블록 | ```kotlin ... ``` |
| API 스펙 | YAML (OpenAPI 형식) | paths:/api/... |

---

## 금지 항목
- `application.yml` 내부 민감 값 노출
- DB 커넥션 문자열, S3 키, 내부 도메인
- “추측”을 사실처럼 단정
- 외부 코드 복붙 (라이선스 불명)

---

## 권장 워크플로
1. PR 생성 전 `make code-review FILES=...`
2. AI 결과를 `/tmp/_ai_output.json` 으로 저장
3. `validate.sh` 로 스키마 통과 확인
4. 리뷰어가 AI output을 참고하여 최종 PR 코멘트 작성
