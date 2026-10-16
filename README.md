# MediclassicsText Syntax Highlighting (MCT2 / MCT1)

한의학 고전 텍스트 마크업 규격인 **MediclassicsText v2 (`.mct2`)** 및 구버전 규격인 **MediclassicsText v1 (`mct1`)** 파일을 위한 Visual Studio Code 구문 강조(Syntax Highlighting) 확장 프로그램입니다.

---

## 🚀 빠른 시작 (User Quick Start)

### 1. 확장 프로그램 설치
1. **[GitHub Releases 배포 페이지](https://github.com/pinedance/vscode-extension-mct2-syntax-highlight/releases)**에서 최신 버전의 `.vsix` 파일(예: `vscode-extension-mct2-syntax-highlight-0.0.5.vsix`)을 다운로드합니다.
2. VS Code를 실행하고 단축키 `Ctrl+Shift+X` (macOS: `Cmd+Shift+X`)를 눌러 **확장(Extensions)** 패널을 엽니다.
3. 확장 패널 상단 우측의 `...` (더 보기 메뉴)를 클릭하고 **`VSIX에서 설치... (Install from VSIX...)`**를 선택합니다.
4. 다운로드한 `.vsix` 파일을 선택하면 설치가 완료됩니다.

### 2. 구문 강조 사용하기
- **MCT2 문서 (`.mct2`)**:
  - 확장자가 `.mct2` 또는 `.MCT2`인 파일을 열면 자동으로 구문 강조가 적용됩니다.
- **MCT1 문서 (구버전 문법)**:
  - 파일 확장자가 고정되어 있지 않으므로, 텍스트 파일을 연 뒤 VS Code 우측 하단의 **언어 모드**(예: `Plain Text`)를 클릭하고 **`MediclassicsText.v1`** (또는 `mct1`)을 선택합니다.
- **예제 파일**:
  - 저장소 내 [`syntax_test_file.mct2`](syntax_test_file.mct2) 및 [`syntax_test_file_mct1.txt`](syntax_test_file_mct1.txt)에서 구문 강조 예시를 확인할 수 있습니다.

---

## ✨ 주요 기능
- **MCT2 지원**:
  - 문서 헤더 및 메타데이터 섹션 강조
  - 책 제목 및 권/편 식별자 강조
  - `{{ 삽입구문 }}`, `☆` 특수 기호 강조
  - `|` 파이프 문자 기반의 테이블(표) 구문 강조
- **MCT1 (구버전) 지원**:
  - 구버전 마크업 태그 및 문서 구조 구문 강조
- **기본 에디터 서식 최적화**:
  - 공백 표시(`boundary`), 제어 문자 표시, 들여쓰기 가이드 기본 활성화

---

## 🛠️ 개발자 가이드 (Developer Guide)

### 1. 프로젝트 성격 및 구조
이 확장은 별도의 컴파일/트랜스파일(TypeScript 빌드 등) 단계가 필요 없는 **선언적(Declarative) TextMate Grammar 확장**입니다.

- **문법 정의 (Grammar)**:
  - `syntaxes/mct2.tmLanguage.json`: MCT2 언어의 정규식 기반 토큰화 및 스코프 정의
  - `syntaxes/mct1.tmLanguage.json`: MCT1 언어의 정규식 기반 토큰화 및 스코프 정의
- **언어 설정 (Configuration)**:
  - `language-configuration.json`: MCT2 주석 및 괄호 쌍 규칙
  - `language-configuration-mct1.json`: MCT1 주석 및 괄호 쌍 규칙
- **매니페스트 (`package.json`)**:
  - 언어 ID 등록, 파일 확장자 매핑, TextMate 문법 연결, 에디터 기본 설정 등 정의

### 2. 의존 패키지 안내
- **런타임 의존성 (`dependencies`)**: 없음
  - VS Code 내장 TextMate 파서로 동작하므로 번들러나 런타임 라이브러리가 필요하지 않습니다.
- **개발 의존성 (`devDependencies`)**:
  - `js-yaml` (`^4.1.0`): YAML 형식의 문법 원본을 JSON으로 변환/검증할 때 사용하는 유틸리티
  - `@vscode/vsce`: VS Code 확장을 `.vsix` 파일로 패키징 및 배포하기 위한 CLI 도구 (`npx @vscode/vsce`로 실행 가능)

### 3. 로컬 개발 및 디버깅
1. VS Code에서 이 프로젝트 폴더를 엽니다.
2. `F5` 키를 누르면 **확장 개발 호스트 (Extension Development Host)** 창이 새로 열립니다.
3. 새 창에서 `.mct2` 파일이나 `syntax_test_file.mct2`를 열어 구문 강조가 정상 동작하는지 테스트합니다.
4. 문법 JSON 파일을 수정한 뒤, 개발 호스트 창에서 `Ctrl+R` (macOS: `Cmd+R`)을 누르면 창이 리로드되어 즉시 변경 사항이 반영됩니다.

### 4. 빌드 및 배포 방법

#### 방법 1: GitHub Actions 자동 배포 (권장)
태그를 push하면 `.github/workflows/release.yml` 워크플로우가 자동으로 실행되어 `.vsix` 빌드 후 GitHub Release에 첨부합니다.

```bash
# 1. package.json의 "version" 업데이트 후 커밋
# 2. 태그 생성 및 푸시
git tag 0.0.6
git push origin 0.0.6
```

#### 방법 2: 로컬 수동 패키징
로컬에서 직접 `.vsix` 파일을 빌드할 때는 다음 명령을 실행합니다:

```bash
npx @vscode/vsce package --no-dependencies --skip-license
```
명령을 실행하면 `package.json`의 `name`과 `version`을 기준으로 `vscode-extension-mct2-syntax-highlight-<version>.vsix` 파일이 생성됩니다.

---

## 📜 변경 이력 (Release Notes)

### 0.0.5
- 구(舊) 문법(MediclassicsText.v1, `mct1`) 구문 강조 추가: 확장자 자동 연결 없이 언어 모드에서 선택
- GitHub Actions 기반 자동 릴리스 및 `.vsix` 배포 워크플로우 구축

### 0.0.4
- custom color theme 적용 삭제
- TextMate scope 이름 일반 형식으로 변경

### 0.0.3
- `{{ 삽입구문 }}` 구문 강조 수정: `☆` 기호 강조
- `|`가 포함된 table 구문 강조 추가

### 0.0.2
- `{{ 삽입구문 }}` 구문 강조 추가

### 0.0.1
- 최초 릴리스
- MCT2 파일을 위한 기본 구문 강조 (문서 헤더, 메타데이터, 콘텐츠 구조)
