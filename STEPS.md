# STEPS — 만들 순서

## 1단계. 맛집 기록 (localStorage)
- `index.html` 하나로 제목, 입력 폼(이름/메뉴/별점 1~5/한줄평), 카드 목록 화면 구성
- [추가]: 입력값 검사(이름 필수 30자, 메뉴 30자, 별점 필수, 한줄평 100자) 후 목록 맨 위에 추가
- [삭제] 버튼으로 카드 제거
- localStorage 저장·복원 (새로고침 후에도 유지)
- 주황색 포인트 카드 디자인, 휴대폰 폭 대응
- 완료 확인: 추가·삭제·새로고침 유지·잘못된 입력 차단

## 2단계. Supabase 저장
- `restaurants` 표 생성 (id, name ≤30 필수, menu ≤30, rating 1~5 필수, review ≤100, created_at 자동)
- CHECK 제약으로 글자 수·별점 범위 차단
- RLS 켜고 anon의 조회·추가·삭제 허용 정책 작성
- `index.html`을 localStorage 대신 Supabase 읽기/쓰기로 전환
- 완료 확인: 다른 브라우저·기기에서 같은 목록이 보임

## 3단계. AI 한줄평
- Edge Function 1개 작성, `verify_jwt` 끔
- Secrets에 `OPENROUTER_API_KEY` 사용 (코드에 키 넣지 않기)
- 모델 순서대로 시도: nemotron-3-ultra → gemma-4-26b → gemma-4-31b
- 프롬프트: 이름·메뉴·별점 기반 한국어 한 문장, 40자 이내 (초과 시 잘라내기)
- 화면에 AI 추천 버튼 추가 → 결과를 한줄평 입력칸에 채움, 실패 시 안내 문구
- 완료 확인: 버튼 클릭 시 40자 이내 한 문장이 채워짐

## 4단계. 배포
- GitHub 저장소 생성 후 push (키·비밀값 포함 여부 점검)
- Vercel에서 저장소 연결해 배포
- 배포 주소에서 보기·추가·삭제·AI 추천 동작 확인, 휴대폰 확인
