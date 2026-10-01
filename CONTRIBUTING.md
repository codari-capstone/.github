# 협업 규칙

## 브랜치 전략

| 브랜치 | 역할 | 들어오는 방법 |
|---|---|---|
| `develop` (기본) | 개발 통합. 평소 작업은 모두 여기로 | 작업 브랜치 PR을 **Squash 병합** |
| `main` | 배포·시연 버전 | `develop` → `main` PR을 **Merge commit**으로 병합 |

두 브랜치 모두 직접 푸시, 강제 푸시, 삭제가 막혀 있고 PR마다 팀원 1명의 승인이 필요하다. 저장소 설정이 브랜치마다 위 병합 방식만 허용한다.

## 작업 흐름

1. 이슈를 만든다 (템플릿: 기능 / 버그 / 작업). Projects 보드에 담당자와 주차를 넣는다.
2. `develop`에서 작업 브랜치를 만든다.
3. `develop`으로 PR을 열고 팀원 1명의 승인을 받는다.
4. Squash 병합한다. 병합된 브랜치는 자동으로 지워지고, 본문의 `Closes #12`로 이슈도 닫힌다.

## 릴리스 (develop → main)

마일스톤을 마치거나 지도교수 미팅에서 시연하기 전에 `develop` → `main` PR을 연다. 제목은 `release: M1 제출 → 판정` 처럼 쓰고 **Merge commit**으로 병합한다. Squash로 병합하면 두 브랜치의 기록이 갈라져 다음 릴리스 PR에서 충돌이 난다.

`main`에서 급하게 고칠 것이 있으면 `hotfix/<이슈번호>-<설명>` 브랜치를 `main`에서 만들어 `main`으로 PR을 올리고, 병합 후 `main` → `develop` PR로 같은 수정을 되돌려 넣는다.

## 브랜치 이름

`<type>/<이슈번호>-<짧은-설명>`

```
feat/12-login-page
fix/31-judge-timeout
docs/4-api-spec
```

## 커밋과 PR 제목

`type(scope): 요약` 형식을 쓴다. Squash 병합 때 PR 제목이 그대로 develop의 커밋 메시지가 되므로 PR 제목을 특히 지킨다. 요약은 한국어로 써도 된다.

| type | 쓰임 |
|---|---|
| feat | 새 기능 |
| fix | 버그 수정 |
| refactor | 동작은 그대로 두고 구조 개선 |
| docs | 문서 |
| test | 테스트 |
| chore | 설정, 빌드, 의존성 |

scope: `web`, `api`, `judge`, `ai`, `infra`, `contracts`, `docs`, `content`

```
feat(web): 문제 목록 필터 추가
fix(judge): Python 시간 제한 보정 배수 적용
```

PR 본문에 `Closes #12`를 쓰면 병합할 때 이슈가 닫힌다.

## 리뷰

- 리뷰 요청을 받으면 하루 안에 확인한다.
- `contracts/`(API 명세)를 바꾸는 PR은 3명 모두 확인한다.

## 넣으면 안 되는 것

- 비밀값: `.env`, API 키, DB 비밀번호, OAuth 시크릿. 예시는 `.env.example`에 가짜 값으로 둔다.
- 문제 정답 코드와 비공개 테스트: 비공개 `problems` 저장소에만 둔다. `codingmon`은 공개 저장소다.
