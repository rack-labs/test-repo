# 온보딩 실습 — `rack-labs/test-repo`

GitHub 워크플로우와 Claude Code(AI agent) 활용을 **연습용 레포에서 부담 없이** 익히는 가이드입니다.
실수해도 괜찮은 샌드박스이니, 막히면 일단 따라하고 끝까지 한 사이클을 돌려보세요.

> 목표: **이슈 만들기 → 브랜치 → 변경 → PR → 머지** 한 바퀴를 Claude Code에게 시켜본다.

이 레포에는 `AGENTS.md`와 `CLAUDE.md`가 있어서 Claude Code가 시작할 때 자동으로 읽고 컨벤션(브랜치명·커밋·PR 포맷)을 따릅니다. 아래 단계에서 사용하는 포맷은 모두 `AGENTS.md`의 **Format Conventions** 섹션에서 가져온 것입니다.

---

## 0. 사전 준비 (한 번만)

### 0-1. GitHub CLI 설치 & 인증
```bash
gh --version          # 설치 확인 (없으면 https://cli.github.com 에서 설치)
gh auth status        # 인증 확인 (없으면 아래 명령)
gh auth login         # 브라우저로 GitHub 로그인 → repo, workflow scope 체크
```

**확인 포인트**
- `Logged in to github.com account <본인-아이디>` 가 보이면 OK
- `Token scopes`에 최소 `repo`, `workflow` 포함

### 0-2. Claude Code 설치
- 설치 가이드: https://docs.claude.com/claude-code
- 설치 후 터미널에서 `claude` 명령이 작동해야 합니다

### 0-3. 이 레포 클론
```bash
git clone https://github.com/rack-labs/test-repo.git
cd test-repo
claude                # 이 디렉토리에서 Claude Code 실행
```

---

## 1. 실습 시나리오

루트에 **본인 이름 마크다운 파일**(`<your-name>.md`)을 추가하는 PR 한 개를 만듭니다.
앞선 팀원들 결과물(`Hyowon.md`, `jiwon.md` 등)이 좋은 예시입니다.

---

## 2. 단계별 가이드

각 단계에서 **Claude에게 시킬 한국어 프롬프트**와, Claude가 뒤에서 실행하는 **`gh`/`git` 명령**을 같이 보여드립니다.
"명령은 직접 안 쳐도 되고, Claude가 알아서 해줍니다." — 다만 무슨 일이 일어나는지는 이해하고 가세요.

### Step 1. 실습용 이슈 만들기

Claude 프롬프트 예시:
> `rack-labs/test-repo`에 `[CHORE] 온보딩 실습: <내 이름> 인사 파일 추가` 제목으로 이슈를 만들어줘. 본문은 AGENTS.md의 Issue 템플릿(Goal/Scope/Done Criteria/References) 형식으로 작성해줘.

생성될 이슈 제목 형식:
```text
[CHORE] 온보딩 실습: <내 이름> 인사 파일 추가
```

본문 예시:
```text
## Goal
GitHub 워크플로우와 AI agent 사용을 한 사이클 연습한다.

## Scope
- 루트에 `<내 이름>.md` 추가
- 짧은 인사말 한 줄

## Done Criteria
- [ ] 파일 추가
- [ ] PR 생성 후 본 이슈와 연결 (Closes #N)
- [ ] 머지

## References
- ONBOARDING.md
```

Claude가 실행하는 명령 (참고):
```bash
gh issue create --title "[CHORE] ..." --body "..." --repo rack-labs/test-repo
```

**확인 포인트**
- 터미널에 생성된 이슈 URL이 나옵니다 → 브라우저에서 한 번 열어 확인

### Step 2. 이슈 기반 브랜치 만들기

Claude 프롬프트 예시:
> 방금 만든 이슈 번호로 브랜치 만들어줘. AGENTS.md의 브랜치 컨벤션(`<이슈번호>-<type>-<짧은-설명>`)을 따라서 `<번호>-chore-onboarding-<내이름>` 형식으로.

Claude가 실행하는 명령:
```bash
git checkout develop
git pull origin develop
git checkout -b <이슈번호>-chore-onboarding-<내이름>
```

브랜치명 예: `52-chore-onboarding-hyowon`

**왜 type을 넣나요?** `AGENTS.md`의 Format Conventions에 따르면 모든 브랜치는 `<번호>-<type>-<설명>` 형식입니다.
이번 작업은 새 기능이 아니라 환경/연습 작업이므로 `chore` 타입을 사용합니다.
Type 값 전체 목록은 `AGENTS.md` 참고.

### Step 3. 파일 추가 & 커밋

Claude 프롬프트 예시:
> 루트에 `<내 이름>.md` 파일 만들어서 짧은 인사말 한 줄 써줘. 그리고 AGENTS.md의 커밋 메시지 포맷으로 커밋해줘.

커밋 메시지 형식 (AGENTS.md Format Conventions):
```text
chore: <내 이름> 인사 파일 추가 (#<이슈번호>)

- 루트에 <내 이름>.md 추가
- 짧은 인사말 한 줄 작성
```

Claude가 실행하는 명령:
```bash
# 파일 생성 후
git add <내이름>.md
git commit -F <메시지파일>     # Windows 멀티라인은 -F 파일 방식 권장
# 또는 한 줄짜리:
git commit -m "chore: <내이름> 인사 파일 추가 (#<이슈번호>)"
```

### Step 4. 푸시 & PR 생성

Claude 프롬프트 예시:
> 원격에 푸시하고 develop 브랜치로 PR 만들어줘. 제목/본문은 AGENTS.md의 PR 템플릿 형식으로 만들어줘.

PR 제목 형식:
```text
[CHORE] 온보딩 실습: <내 이름> 인사 파일 추가 (#<이슈번호>)
```

PR 본문 형식 (AGENTS.md Format Conventions):
```text
## Related Issue
Closes #<이슈번호>

## Work Summary
- 루트에 `<내 이름>.md` 추가
- 짧은 인사말 작성

## Change Details
- 신규 파일 1개 추가 (마크다운)
- 동작 변경 없음

## Test Method
1. 브라우저에서 GitHub 파일 보기로 내용 확인
2. PR 사이드바에서 이슈 #N 연결 확인
```

Claude가 실행하는 명령:
```bash
git push -u origin <브랜치이름>
gh pr create --base develop --title "[CHORE] ..." --body "..."
```

**확인 포인트**
- PR URL이 출력됩니다 → 브라우저에서 열어 **이슈 #N 이 자동 연결**됐는지 확인 (사이드바 "Linked issues")

### Step 5. (선택) 머지

샌드박스 레포라 본인 PR을 본인이 머지해도 됩니다. 실제 프로젝트에서는 리뷰를 받습니다.

Claude 프롬프트 예시:
> 방금 만든 PR squash 머지하고, 머지되면 로컬 브랜치 정리해줘.

```bash
gh pr merge --squash --delete-branch
git checkout develop && git pull
```

---

## 3. 자주 만나는 에러 & 해결

| 증상 | 원인 | 해결 |
|---|---|---|
| `gh: command not found` | gh CLI 미설치 | https://cli.github.com 에서 설치 후 새 터미널 |
| `gh auth status` 가 빨갛게 나옴 | 로그인 안 됨 | `gh auth login` |
| `permission denied` 또는 푸시 거부 | 권한 부족 또는 SSH 키 미설정 | `gh auth login` 다시, 프로토콜 **HTTPS** 선택 |
| PR 생성 시 `no commits between develop and <branch>` | 커밋을 안 했거나 develop과 같음 | Step 3 다시 (변경 → add → commit) |
| Claude가 엉뚱한 레포에 이슈 만들려고 함 | 현재 디렉토리가 잘못됨 | `cd test-repo` 후 다시, 또는 프롬프트에 `--repo rack-labs/test-repo` 명시 |
| 한글 브랜치명에서 깨짐 | 일부 터미널 인코딩 문제 | 영문/숫자/하이픈만 사용 권장 |

---

## 4. 다음에 시도해볼 것

한 사이클을 마쳤다면, AI agent 활용을 좀 더 깊이 가보세요:

- **여러 단계를 한 번에 시키기**: "이슈 만들고, 브랜치 파고, 파일 추가하고, PR까지 한 번에 해줘"
- **PR 코멘트로 수정 받기**: 다른 팀원 PR에 `gh pr review` 로 코멘트 달아보기
- **Claude에게 코드 리뷰 시키기**: `/review` (Claude Code 내장 슬래시 커맨드)
- **실제 작업 레포로 옮기기**: 본 레포에서 익숙해졌다면 `rack-labs/rack-tracker` 의 워크플로우 문서 참고 (이 레포의 `reference/rack-tracker/` 에 사본이 있습니다)

---

## 5. 도움 요청

- 막히면 본인이 보고 있는 **에러 메시지 전체**를 Claude에게 그대로 붙여넣기 → 거의 다 해결됩니다
- Claude도 모를 때: 팀 채널에 "에러 메시지 + 직전에 시킨 프롬프트" 같이 공유
