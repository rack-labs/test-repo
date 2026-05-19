# Agent Entry Point — `rack-labs/test-repo` (Sandbox)

이 레포는 **GitHub 워크플로우와 AI agent 사용을 연습하는 샌드박스**입니다.
실제 프로덕트 레포(`rack-labs/rack-tracker`)의 무거운 규칙은 제거하고, **포맷 컨벤션만** 가져와 가볍게 유지합니다.

> 실수해도 괜찮습니다. 한 사이클을 끝까지 돌려보는 것이 목표입니다.

## Document Relations

- 사람이 따라할 단계별 가이드: `ONBOARDING.md`
- 원본 워크플로우 (졸업 후 참고): `reference/rack-tracker/AGENTS.md`, `reference/rack-tracker/agent-workflow/`

## Sandbox Differences (vs `rack-labs/rack-tracker`)

| 항목 | rack-tracker (실 운영) | test-repo (샌드박스) |
|---|---|---|
| Pre-Execution Gate | 파일 변경 전마다 관리 문서 매칭 확인 | **생략** |
| Pre-Commit Review Gate | 커밋 전 사용자 명시 승인 | **생략** |
| 관리 문서 (`docs/mvp-v2/issues/...`) | 모든 작업 단위마다 갱신 의무 | **없음** |
| 브랜치/커밋/PR 포맷 | 엄격 적용 | **동일하게 적용** ← 여기서 연습 |
| Issue-first 원칙 | 강제 | 권장 |

샌드박스에서는 포맷·이름·흐름을 익히는 데 집중하고, 관리 문서 같은 무거운 규칙은 졸업 후 실제 레포에서 따릅니다.

## Start Here

1. 처음이라면 `ONBOARDING.md`의 **0~5단계**를 그대로 따라합니다.
2. AI agent에게 일을 시킬 때는 아래 **Core Rules**와 **Format Conventions** 안에서 움직이도록 지시합니다.
3. 본격적인 워크플로우가 궁금해지면 `reference/rack-tracker/` 의 원본 문서를 읽습니다.

## Core Rules

- **Issue first**: 새 작업은 가급적 GitHub 이슈로 시작합니다. `gh issue create` 사용.
- **이슈 번호 확정 후 브랜치**: 이슈 번호가 정해지면 그 번호로 브랜치를 만듭니다. 임시로 일할 땐 `<issue-number>-` 자리표시자를 둡니다.
- **하나의 PR은 하나의 이슈를 닫습니다**: PR 본문에 `Closes #<번호>`.
- **타인 PR을 임의로 머지/리베이스하지 않기**: 본인 PR만.
- **파괴적 히스토리 편집은 명시 요청 시에만**: `git reset --hard`, force push, 브랜치 삭제 등.

## Format Conventions (rack-tracker에서 발췌)

### Issue Title
```text
[TYPE] Short work summary
```

### Branch
```text
<issue-number>-<type>-<short-description>
예: 49-chore-onboarding-practice
```

### Commit Message
```text
type: short summary (#<issue-number>)

- change item 1
- change item 2
```

### PR Title
```text
[TYPE] Short work summary (#<issue-number>)
```

### PR Body
```text
## Related Issue
Closes #<issue-number>

## Work Summary
- 주요 구현/수정 내용

## Change Details
- 구조 또는 모듈 변경
- API 또는 동작 변경

## Test Method
1. 실행 방법
2. 확인한 시나리오
```

### Type 값

**Issue Type**: `feature`, `fix`, `refactor`, `docs`, `test`, `chore`, `ci`, `perf`
**Commit Type**: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`, `revert`, `repo`

## Forbidden Actions (샌드박스에서도 유지)

- `main`, `develop`에 직접 push 금지 — 반드시 PR
- 다른 사람 브랜치를 임의로 force push 하지 않기
- 다른 사람 커밋 히스토리를 임의로 재작성하지 않기

## Default Branches

- 기본 브랜치: `develop`
- PR target: `develop` (샌드박스이므로 `main`은 사용하지 않습니다)
- 머지 방식: squash 권장
