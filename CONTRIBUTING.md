# 협업 규칙

## 브랜치 전략: Git Flow

Git Flow를 따른다. 팀 규모와 10주 일정에 맞춰 `release` 브랜치는 쓰지 않고, `develop` → `main` PR이 릴리스를 대신한다.

| 브랜치 | 역할 | 만드는 곳 | 합치는 곳 | 병합 방식 |
|---|---|---|---|---|
| `main` | 배포·시연 버전 | — | — | Merge commit |
| `develop` (기본) | 개발 통합 | — | `main` | Squash |
| `feature/*` | 기능, 버그 수정, 문서 등 모든 작업 | `develop` | `develop` | Squash |
| `hotfix/*` | `main`의 긴급 수정 | `main` | `main`, 그다음 `develop` | Merge commit → Squash |

`main`과 `develop`은 직접 푸시, 강제 푸시, 삭제가 막혀 있고 PR마다 팀원 1명의 승인이 필요하다. 저장소 설정이 브랜치마다 위 병합 방식만 허용한다.

### 브랜치 이름

`<종류>/<이슈번호>-<짧은-설명>` 형식이다. 설명은 영어 소문자와 하이픈으로 쓴다.

```
feature/12-login-page
feature/31-judge-timeout
hotfix/40-submit-500-error
```

### 작업 흐름 (feature)

1. 이슈를 만든다 (템플릿: 기능 / 버그 / 작업). Projects 보드에 담당자와 주차를 넣는다.
2. `develop`에서 `feature/<이슈번호>-<설명>` 브랜치를 만든다.
3. `develop`으로 PR을 열고 팀원 1명의 승인을 받는다.
4. Squash 병합한다. 병합된 브랜치는 자동으로 지워지고, PR 본문의 `Closes #12`로 이슈도 닫힌다.

### 릴리스 (develop → main)

마일스톤을 마치거나 지도교수 미팅에서 시연하기 전에 `develop` → `main` PR을 연다. 제목은 `chore(release): M1 제출 → 판정`처럼 쓰고 **Merge commit**으로 병합한다. Squash로 병합하면 두 브랜치의 기록이 갈라져 다음 릴리스 PR에서 충돌이 난다.

### 긴급 수정 (hotfix)

1. `main`에서 `hotfix/<이슈번호>-<설명>` 브랜치를 만든다.
2. `main`으로 PR을 열어 승인 후 Merge commit으로 병합한다.
3. 같은 수정이 `develop`에도 들어가도록 `main` → `develop` PR을 열어 Squash 병합한다.

## 커밋 컨벤션

### 형식

```
type(scope): 요약

본문 (선택)
```

- type과 scope는 영어 소문자, 요약은 **한국어**로 쓴다.
- 요약은 50자 안팎으로, 끝에 마침표를 찍지 않고 "추가", "수정"처럼 끝맺는다.
- 커밋 하나에는 한 가지 변경만 담는다.
- 본문이 필요하면 제목 아래 한 줄을 비우고 무엇을, 왜 바꿨는지 적는다.
- Squash 병합하면 **PR 제목이 develop에 남는 커밋 메시지**가 된다. PR 제목은 반드시 이 형식을 지킨다.

### type

| type | 쓰임 |
|---|---|
| feat | 새 기능 |
| fix | 버그 수정 |
| refactor | 동작은 그대로 두고 코드 구조 개선 |
| style | 동작에 영향 없는 코드 형식 수정 (포맷, 공백, 세미콜론). 화면 디자인 변경은 feat나 fix |
| docs | 문서 |
| test | 테스트 추가·수정 |
| chore | 빌드, 설정, 의존성, 그 밖의 잡무 |

### scope

| scope | 영역 |
|---|---|
| web | 프론트엔드 (`web/`) |
| api | 백엔드 (`api/`) |
| judge | 채점 서버 (`judge/`) |
| ai | AI 분석·힌트·리뷰·리포트 (프롬프트 포함) |
| infra | Docker, AWS, CI/CD (`infra/`) |
| contracts | API 명세 (`contracts/`) |
| docs | 설계 문서 (`docs/`) |
| content | 문제·테스트 (`problems` 저장소) |
| release | `develop` → `main` 릴리스 PR에만 쓴다 |

여러 영역에 걸친 변경은 scope를 생략한다.

### 예시

```
feat(web): 문제 목록 필터 추가
fix(judge): Python 시간 제한 보정 배수 적용
refactor(api): 제출 처리 서비스 분리
style(web): Prettier 포맷 적용
docs(contracts): 제출 API 응답 예시 추가
test(api): 판정 비교 단위 테스트 추가
chore: Gradle 의존성 업데이트
chore(release): M1 제출 → 판정
```

PR 본문에 `Closes #12`를 쓰면 병합할 때 이슈가 닫힌다. 이슈를 닫지 않고 연결만 하려면 `Ref #12`를 쓴다.

## 리뷰

- 리뷰 요청을 받으면 하루 안에 확인한다.
- `contracts/`(API 명세)를 바꾸는 PR은 3명 모두 확인한다.

## 넣으면 안 되는 것

- 비밀값: `.env`, API 키, DB 비밀번호, OAuth 시크릿. 예시는 `.env.example`에 가짜 값으로 둔다.
- 문제 정답 코드와 비공개 테스트: 비공개 `problems` 저장소에만 둔다. `codingmon`은 공개 저장소다.
