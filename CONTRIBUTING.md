# 협업 규칙

## 작업 흐름

1. 이슈를 만든다 (템플릿: 기능 / 버그 / 작업). Projects 보드에 담당자와 주차를 넣는다.
2. develop에서 브랜치를 만든다.
3. PR을 열고 팀원 1명의 승인을 받는다.
4. Squash 병합한다. 병합된 브랜치는 자동으로 지워진다.

main에는 직접 푸시할 수 없다.

## 브랜치 이름

`<type>/<이슈번호>-<짧은-설명>`

```
feat/12-login-page
fix/31-judge-timeout
docs/4-api-spec
```

## 커밋과 PR 제목

`type(scope): 요약` 형식을 쓴다. Squash 병합 때 PR 제목이 그대로 main의 커밋 메시지가 되므로 PR 제목을 특히 지킨다. 요약은 한국어로 써도 된다.

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
- 문제 정답 코드와 비공개 테스트: 비공개 `problems` 저장소에만 둔다. `codari`는 공개 저장소다.
