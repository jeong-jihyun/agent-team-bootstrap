# agent-team-bootstrap

새 프로젝트에 에이전트 팀을 처음 세팅할 때 쓰는 **분석 전용 서브에이전트**입니다.
코드를 읽기만 하고(수정 불가), 구조와 실행 방법을 파악해서 필요한 역할 구성을 제안합니다.
Claude Code와 opencode 둘 다 지원합니다.

## 들어 있는 파일

| 도구 | 위치 |
|---|---|
| Claude Code | `.claude/agents/project-analyzer.md` |
| opencode | `.opencode/agents/project-analyzer.md` |

## 사용 방법

1. 쓰는 도구의 폴더(`.claude` 또는 `.opencode`)를 분석할 프로젝트 루트에 복사합니다.
   ```bash
   cp -r .claude 내프로젝트폴더/      # Claude Code
   cp -r .opencode 내프로젝트폴더/    # opencode
   ```
2. 프로젝트 폴더에서 세션을 새로 시작합니다. 에이전트 파일은 시작할 때만 읽힙니다.
   ```bash
   cd 내프로젝트폴더
   claude        # 또는 opencode
   ```
3. 인식 확인: Claude Code는 `/agents`, opencode는 `@` 입력 후 후보 목록을 봅니다.
4. 분석을 요청합니다.
   - Claude Code: `project-analyzer로 이 프로젝트 분석해줘`
   - opencode: `@project-analyzer 이 프로젝트 분석해줘`
5. 보고서를 확인하고 고칠 부분을 말합니다. 괜찮으면 CLAUDE.md(opencode는 AGENTS.md)와 역할별 에이전트 파일 생성을 요청합니다.
6. 새로 만든 역할 파일을 인식시키려면 세션을 한 번 더 재시작합니다.

## 분석 보고서 내용

1. 프로젝트 한 줄 요약
2. 기술 스택
3. 실행 / 빌드 / 테스트 명령어 (확인 못 한 것은 "확인 불가")
4. 구조와 모듈 경계
5. 주의가 필요한 영역
6. 추천 역할 구성과 그 이유
7. 상황별 처리 규칙 초안

## 역할 수 기준

- 파일 약 20개 미만, 모듈 1개: developer, tester
- 모듈 2~3개: 모듈별 developer + tester + reviewer
- 화면 작업이 많음: designer 추가
- 모듈 4개 이상: 묶음 기준을 먼저 사람에게 확인

역할이 늘면 인수인계 비용도 늘어나므로, 역할을 늘릴 때는 이유를 적습니다.

## 두 도구의 문법 차이

| 항목 | Claude Code | opencode |
|---|---|---|
| 에이전트 이름 | `name` 필드 | 파일명 |
| 도구 권한 | `tools: Read, Grep, Glob` | `read: true`, `write: false` 맵 |
| 호출 | 자연어로 지시 | `@이름` |
