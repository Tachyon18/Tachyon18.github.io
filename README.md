# 🎮 Dev Blog v1.0
> 언리얼 / 유니티 개발 기록 블로그 — Hugo 기반

---

## 🚀 빠른 시작

### 1. Hugo 설치
```bash
# macOS
brew install hugo

# Windows (Chocolatey)
choco install hugo-extended

# Windows (Scoop)
scoop install hugo-extended
```

### 2. 테마 설치
```bash
cd hugo-devblog
git init
git submodule add https://github.com/panr/hugo-theme-terminal.git themes/terminal
```

### 3. 로컬 서버 실행
```bash
hugo server -D
# http://localhost:1313 에서 확인
```

---

## 📁 폴더 구조
```
hugo-devblog/
├── content/
│   ├── unreal/           # 언리얼 엔진 포스팅
│   ├── unity/            # 유니티 포스팅
│   └── project-logs/     # 프로젝트 진행 기록
├── archetypes/           # 포스팅 템플릿
│   ├── unreal.md
│   ├── unity.md
│   └── project-logs.md
├── static/               # 이미지, CSS 등 정적 파일
├── layouts/              # 커스텀 레이아웃 (선택)
├── hugo.toml             # 블로그 설정
├── CLAUDE-INSTRUCTIONS.md  # Claude 자동화 지침
└── README.md
```

---

## ✍️ 새 포스팅 작성

```bash
# 언리얼 포스팅
hugo new unreal/제목-영문.md

# 유니티 포스팅
hugo new unity/제목-영문.md

# 프로젝트 로그
hugo new project-logs/2026-04-05-프로젝트명.md
```

---

## 🌐 GitHub Pages 배포

```bash
# 빌드
hugo --minify

# /public 폴더를 gh-pages 브랜치에 배포
# 또는 GitHub Actions로 자동화 (별도 워크플로우 추가)
```

---

## 🤖 Claude와 함께 사용하기
`CLAUDE-INSTRUCTIONS.md` 파일을 참고하세요.
MCP filesystem 커넥터 연결 후 Claude가 직접 파일을 생성/수정할 수 있습니다.

---

## 📌 버전
- **v1.0** — 초기 베이스 템플릿 (언리얼/유니티/프로젝트 로그)
