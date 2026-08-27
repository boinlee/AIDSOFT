# AIDSOFT 홈페이지

에이아이디소프트(AIDSOFT)의 수능절 서비스 소개 및 제휴 문의용 정적 홈페이지입니다.

- 운영 사이트: https://aidsoft.vercel.app/
- 소스 저장소: https://github.com/boinlee/AIDSOFT
- 배포: Vercel
- 문의 접수: Formspree + Supabase

## 구성

| 파일 | 용도 |
| --- | --- |
| `index.html` | 홈페이지 화면, 문의 양식 및 브라우저 동작 |
| `style.css` | 추가 스타일 |
| `robots.txt` | 검색 엔진 접근 정책 |
| `sitemap.xml` | 사이트맵 |
| `.vercelignore` | 배포에서 제외할 파일 설정 |

별도 빌드 과정이 없는 정적 사이트입니다. `index.html`을 수정해 확인하거나, GitHub의 기본 브랜치에 반영하면 Vercel 배포에 적용됩니다.

## 문의폼 동작

문의가 제출되면 아래 순서로 처리됩니다.

1. Formspree로 문의를 전송해 담당자 이메일로 전달합니다.
2. Formspree 전송이 성공하면 같은 문의를 Supabase Edge Function으로 전송합니다.
3. Edge Function이 서버 내부 권한으로 `contact_inquiries` 테이블에 저장합니다.
4. 화면에는 Formspree 접수 성공 또는 실패 안내가 표시됩니다.

Formspree 전송이 우선이며, Supabase 저장 오류는 브라우저 콘솔에 기록됩니다. 이 설계는 DB 저장 문제 때문에 이메일 문의 접수까지 실패하거나 중복 전송되는 일을 막기 위한 것입니다.

## Supabase 보안

- 프로젝트 리전: 서울 (`ap-northeast-2`)
- 테이블: `public.contact_inquiries`
- RLS: 활성화
- 공개 정책: 없음
- 직접 DB 열람: 차단
- 저장 권한: `contact-inquiry` Edge Function만 내부 비밀 키로 수행

브라우저에는 Supabase Publishable Key만 사용합니다. 서비스 키·비밀 키는 절대 HTML, GitHub, Vercel의 공개 환경 변수에 넣지 않습니다.

## 운영 확인 절차

문의 기능을 변경했거나 배포를 확인할 때:

1. 운영 사이트에서 테스트 문의를 전송합니다.
2. Formspree 수신 이메일(스팸함 포함)을 확인합니다.
3. Supabase에서 `contact_inquiries`에 동일 문의가 저장됐는지 확인합니다.
4. 테스트용으로 만든 DB 행은 확인 후 삭제합니다.

테스트용 개인정보는 사용하지 말고, 예: `test@example.com`과 식별 가능한 테스트 문구를 사용합니다.

## 개인정보 안내

문의 양식에는 개인정보 수집·이용 동의가 필요합니다. 개인정보 처리방침에는 다음 저장·전송 사실이 고지되어 있습니다.

- Formspree를 통한 문의 접수 및 이메일 전달(미국)
- Supabase 서울 리전 데이터베이스를 통한 문의 접수 기록 저장(대한민국)

정식 운영 전에는 실제 개인정보 처리방침과 담당자 연락처를 최종 검토해야 합니다.

## 검색 노출 정책

현재는 정식 도메인과 운영 승인 전 단계이므로 검색 노출을 의도적으로 보류하고 있습니다.

- `<meta name="robots" content="noindex, nofollow">`
- `robots.txt` 전체 차단
- 빈 사이트맵

정식 도메인 연결 및 운영 승인 뒤에만 위 설정을 해제하고, OG 이미지·카카오톡 등 공유 미리보기를 실제 채널에서 확인합니다.

## 배포

GitHub 저장소와 Vercel 프로젝트를 연결해 운영합니다.

1. GitHub 기본 브랜치에 변경을 반영합니다.
2. Vercel 배포 상태가 Ready인지 확인합니다.
3. https://aidsoft.vercel.app/ 에서 변경 내용을 확인합니다.

## 주의 사항

- Formspree 폼 ID와 수신 이메일은 Formspree 계정 소유자가 관리합니다.
- Supabase Edge Function 및 DB 테이블을 삭제하거나 공개 정책을 추가하기 전에 개인정보 처리 영향과 접근 권한을 검토합니다.
- 사이트 내 현재 회사·연락처·정책 문구는 정식 운영 정보로 최종 확정한 뒤 유지·갱신합니다.
