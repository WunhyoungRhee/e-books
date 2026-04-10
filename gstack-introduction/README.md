# gstack 완전 초보자 가이드

> Garry Tan이 만든 오픈소스 스킬 패키지 gstack을 처음부터 배워보는 완전 초보 가이드.

**gstack version:** 0.12.12.0 | **Last updated:** 2026-04-10 | **License:** MIT

---

## About

이 전자책은 ChatGPT를 써본 적은 있지만, AI 에이전트, 스킬, 터미널 등 개발 도구에 대한 사전 지식이 없는 **완전 초보자**를 위해 만들어졌습니다.

gstack의 28개 스킬을 활용하여 혼자서도 20명짜리 개발팀처럼 일하는 방법을 단계별로 안내합니다.

## Structure

4개의 파트, 14개의 챕터, 1개의 부록으로 구성되어 있습니다.

### 제1부: 시작하기 전에 — AI 세계 이해하기

| Chapter | Title | Description |
|---------|-------|-------------|
| 1 | AI와 대화하기 | ChatGPT에서 한 걸음 더 — 에이전트와 스킬 개념 |
| 2 | gstack이란 무엇인가? | 28개 스킬 패키지의 핵심 소개 |
| 3 | 준비하기 | 터미널, Git, Node.js, Bun 기초 설명 |

### 제2부: 첫 걸음 — 설치와 첫 체험

| Chapter | Title | Description |
|---------|-------|-------------|
| 4 | 설치하기 | 30초 설치 가이드 |
| 5 | 처음 만나는 gstack | 따라하기 체험 — 코딩 없이 시작 |
| 6 | 스프린트 모델 이해하기 | 생각-계획-구현-리뷰-테스트-출시-회고 |

### 제3부: 깊이 들어가기 — 스킬 활용법

| Chapter | Title | Description |
|---------|-------|-------------|
| 7 | 핵심 스킬 28개 한눈에 보기 | 난이도별 스킬 가이드 |
| 8 | 사회연대경제 조직 가이드 | 제한된 자원으로 큰 일 해내기 |
| 9 | 스타트업 가이드 | 60일간 60만 줄의 비밀 |
| 10 | 학습자 가이드 | gstack으로 배우는 AI 워크플로 |
| 11 | 교육자 가이드 | 12시간 커리큘럼 |

### 제4부: 철학과 참고

| Chapter | Title | Description |
|---------|-------|-------------|
| 12 | Builder Ethos | 빌더 철학 — 호수를 끓여라, 만들기 전에 찾아라 |
| 13 | 문제 해결 | Troubleshooting |
| 14 | 참고 자료 | 공식 문서, 튜토리얼, 커뮤니티 |

### 부록

| Appendix | Title | Description |
|----------|-------|-------------|
| A | 용어 사전 | 20개 기술 용어의 쉬운 정의 |

## Tech Stack

- Multi-page HTML (skills-creation 스타일)
- External CSS with design tokens + dark mode
- Sidebar navigation with active section tracking
- Inline SVG diagrams
- Responsive design (mobile-first)

## Local Preview

```bash
cd e-books
python -m http.server 9000
# Open http://localhost:9000/gstack-introduction/
```

## Credits

- **gstack**: [Garry Tan](https://github.com/garrytan/gstack) (MIT License)
- **Guide**: Made with AI in Gwangmyeong, South Korea
