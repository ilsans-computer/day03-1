# CLAUDE.md

이 파일은 Claude Code(claude.ai/code)가 이 저장소에서 작업할 때 참고할 안내를 제공합니다.

## 현재 상태

기획 단계입니다. 저장소에는 [PRD.md](PRD.md)(요구사항)와 [STEPS.md](STEPS.md)(4단계 제작 순서)만 있습니다. 코드, 빌드, 린트, 테스트 도구는 아직 없습니다. 구현 전에 두 파일을 먼저 읽으세요. 두 파일(한국어로 작성됨)이 기준 문서입니다.

## 아키텍처 (계획)

- **단일 `index.html`**: npm도 번들러도 없습니다. UI, 스타일, 로직이 모두 이 파일 하나에 들어갑니다.
- **저장소는 단계별로 전환**: 1단계는 `localStorage`, 2단계는 Supabase(`restaurants` 표)로 교체합니다. 교체해도 UI 코드가 바뀌지 않도록 데이터 접근을 작은 계층 뒤에 두세요.
- **AI 한줄평**: Supabase Edge Function 1개(`verify_jwt` 끔)가 OpenRouter를 호출하며, 모델을 `nvidia/nemotron-3-ultra-550b-a55b:free` → `google/gemma-4-26b-a4b-it:free` → `google/gemma-4-31b-it:free` 순서로 시도합니다. 결과는 한국어 한 문장, 40자 이내여야 합니다(넘으면 자릅니다). 키는 Supabase Secrets의 `OPENROUTER_API_KEY`에 두고 코드에는 넣지 않습니다.
- **배포**: GitHub → Vercel(정적 배포).

## 기억해 둘 제약

- `restaurants`: `name` 필수 30자 이하, `menu` 30자 이하, `rating` 필수 1~5, `review` 100자 이하, `created_at` 자동. 제한은 화면과 DB CHECK 제약 양쪽에서 모두 적용합니다.
- 로그인 없음. 누구나 보기·추가·삭제가 가능하며, RLS를 켜고 그 동작만 허용하는 정책을 둡니다.
- 새 기록이 목록 맨 위에 표시됩니다.
- 범위 밖: 로그인, 수정, 사진 업로드, 검색.
