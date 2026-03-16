---
description: MD → 전자책 변환 — Markdown 파일을 EPUB/PDF 전자책으로 변환 (Pandoc 기반)
---

# Markdown to Ebook Skill

Markdown 파일(들)을 **EPUB 또는 PDF 전자책**으로 변환합니다. Pandoc을 핵심 엔진으로 사용합니다.

---

## 사용 시점

- "이 md 파일을 전자책으로 만들어줘" 또는 "EPUB으로 변환해줘"라고 요청받았을 때
- 여러 차시/챕터 md 파일을 하나의 책으로 합칠 때
- PDF 또는 EPUB 출력이 필요할 때

## 사용하지 않을 때

- 발표 슬라이드가 필요할 때 → `/notebooklm-slide` 사용
- 웹에서 읽는 온라인 문서(HTML)만 필요할 때
- md 파일의 내용 자체를 수정·편집할 때

---

## 사전 요구 사항

### 필수: Pandoc 설치

```bash
# Windows (Chocolatey)
choco install pandoc

# Windows (winget)
winget install --id JohnMacFarlane.Pandoc

# macOS
brew install pandoc

# Linux (Ubuntu/Debian)
sudo apt-get install pandoc
```

### 선택: PDF 변환 시 LaTeX 필요

```bash
# Windows
choco install miktex

# macOS
brew install --cask mactex

# Linux
sudo apt-get install texlive-full
```

> **💡 TIP:** EPUB만 필요하면 LaTeX 없이 Pandoc만으로 충분합니다.

---

## 워크플로우

```
1. 환경 확인 (Check)
   ↓
2. 소스 분석 (Analyze)
   ↓
3. 메타데이터 구성 (Metadata)
   ↓
4. 변환 실행 (Convert)
   ↓
5. 결과 확인 (Verify)
```

---

## Step 1: 환경 확인 (Check)

Pandoc이 설치되어 있는지 확인합니다.

// turbo
```bash
pandoc --version
```

- 설치되어 있지 않으면 사용자에게 설치 방법을 안내합니다.
- PDF 출력 요청 시 LaTeX 설치 여부도 확인합니다.

---

## Step 2: 소스 분석 (Analyze)

변환할 Markdown 파일을 파악합니다.

### 확인 항목

| 항목 | 설명 | 예시 |
|:---:|---|---|
| **입력 파일** | 변환할 md 파일 목록 | `1차시.md`, `2차시.md`, ... |
| **파일 순서** | 책의 챕터 순서 | 파일명 또는 사용자 지정 순서 |
| **출력 형식** | EPUB / PDF / 둘 다 | 기본값: EPUB |
| **이미지 포함 여부** | md 내 이미지 경로 확인 | `images/` 폴더 |
| **출력 파일명** | 결과 파일 이름 | `{프로젝트명}.epub` |

### 파일 순서 결정 규칙

1. 사용자가 순서를 지정한 경우 → 그대로 사용
2. 파일명에 숫자가 있는 경우 → 숫자 순 정렬 (예: `1차시.md`, `2차시.md`)
3. 그 외 → 사용자에게 순서 확인

---

## Step 3: 메타데이터 구성 (Metadata)

전자책의 메타 정보를 YAML 파일로 구성합니다.

### 메타데이터 파일 (`metadata.yaml`) 생성

```yaml
---
title: "책 제목"
subtitle: "부제목"
author: "저자명"
date: "2026-03-16"
lang: ko-KR
description: "책에 대한 간단한 설명"
rights: "© 2026 저자명. All rights reserved."
---
```

### 메타데이터 수집 규칙

| 항목 | 필수 | 기본값 |
|:---:|:---:|---|
| **title** | ✅ | 프로젝트 폴더명 또는 첫 번째 `# 제목` |
| **author** | ✅ | 사용자에게 확인 |
| **date** | ❌ | 오늘 날짜 |
| **lang** | ❌ | `ko-KR` |
| **subtitle** | ❌ | 생략 |
| **description** | ❌ | 생략 |

> **💡 TIP:** 사용자가 메타데이터를 별도로 지정하지 않으면, md 파일의 첫 번째 `#` 제목을 `title`로 사용합니다.

---

## Step 4: 변환 실행 (Convert)

### 4.1 EPUB 변환

```bash
# 단일 파일
pandoc input.md -o output.epub \
  --metadata-file=metadata.yaml \
  --toc \
  --toc-depth=2

# 여러 파일 → 하나의 EPUB
pandoc 1차시.md 2차시.md 3차시.md 4차시.md \
  -o output.epub \
  --metadata-file=metadata.yaml \
  --toc \
  --toc-depth=2

# 표지 이미지 포함
pandoc *.md -o output.epub \
  --metadata-file=metadata.yaml \
  --toc \
  --epub-cover-image=cover.png
```

### 4.2 PDF 변환 (LaTeX 필요)

```bash
# 한국어 PDF (XeLaTeX 사용)
pandoc input.md -o output.pdf \
  --metadata-file=metadata.yaml \
  --toc \
  --pdf-engine=xelatex \
  -V mainfont="NanumGothic" \
  -V geometry:margin=2.5cm
```

### 4.3 커스텀 CSS 적용 (EPUB)

더 나은 스타일링이 필요한 경우 CSS 파일을 적용합니다:

```bash
pandoc *.md -o output.epub \
  --metadata-file=metadata.yaml \
  --toc \
  --css=ebook-style.css
```

### 변환 옵션 레퍼런스

| 옵션 | 설명 | 기본값 |
|:---:|---|:---:|
| `--toc` | 목차 자동 생성 | 사용 |
| `--toc-depth=N` | 목차에 포함할 헤더 깊이 | 2 |
| `--epub-cover-image` | 표지 이미지 경로 | 없음 |
| `--css` | 커스텀 CSS 파일 | 없음 |
| `--metadata-file` | 메타데이터 YAML 파일 | 없음 |
| `--pdf-engine` | PDF 엔진 (xelatex 권장) | pdflatex |
| `-V mainfont` | PDF 본문 폰트 | 시스템 기본 |
| `-V geometry:margin` | PDF 여백 설정 | LaTeX 기본 |
| `--number-sections` | 섹션 번호 자동 부여 | 사용 안 함 |
| `--highlight-style` | 코드 하이라이트 스타일 | pygments |

---

## Step 5: 결과 확인 (Verify)

변환된 파일이 정상인지 확인합니다.

// turbo
```bash
# 파일 존재 여부 확인 (Windows)
dir output.epub

# 파일 크기 확인
# EPUB: 최소 10KB 이상이어야 정상
```

### 확인 체크리스트

- [ ] 파일이 정상적으로 생성되었는가?
- [ ] 파일 크기가 합리적인가? (빈 파일이 아닌가?)
- [ ] 목차(TOC)가 올바르게 생성되었는가?
- [ ] 이미지가 포함되어 있는 경우 정상 표시되는가?
- [ ] 한국어 텍스트가 깨지지 않는가?

---

## 사용자 프롬프트 예시

### 기본 요청
```
이 md 파일을 전자책으로 만들어줘
```

### 상세 요청
```
1차시부터 4차시까지 md 파일을 하나의 EPUB 전자책으로 만들어줘.
- 제목: "OpenClaw 101"
- 저자: "OpenClaw School"
- 목차 포함
- 표지 이미지: cover.png
```

### 부분 요청
```
이 md 파일을 PDF로 변환해줘
EPUB에 커스텀 CSS를 적용해줘
메타데이터 파일만 만들어줘
```

---

## 출력 파일명 규칙

| 입력 | 출력 파일명 |
|:---:|:---:|
| 단일 파일 `report.md` | `report.epub` |
| 여러 파일 (프로젝트명 사용) | `{프로젝트폴더명}.epub` |
| 사용자 지정 | 사용자가 지정한 이름 |

---

## 품질 체크리스트

- [ ] Pandoc이 정상 설치되어 있는가?
- [ ] 입력 파일 **순서**가 올바른가?
- [ ] **메타데이터** (제목, 저자)가 설정되었는가?
- [ ] **목차**(TOC)가 포함되었는가?
- [ ] md 내 **이미지 경로**가 유효한가? (상대 경로 확인)
- [ ] **한국어**가 깨지지 않는가?
- [ ] 출력 파일이 정상 열리는가?
- [ ] PDF 변환 시 **한국어 폰트**가 지정되었는가?
