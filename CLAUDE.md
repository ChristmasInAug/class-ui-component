# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 프로젝트 개요

CareerFoundry의 [32 UI Elements Designers Need To Know](https://careerfoundry.com/en/blog/ui-design/ui-element-glossary/)를 기반으로, 32가지 핵심 프론트엔드 UI 컴포넌트를 한국어로 정리한 **인터랙티브 컴포넌트 레퍼런스/포트폴리오 사이트**를 만드는 프로젝트다. 각 컴포넌트는 미리보기(iframe) + HTML/CSS/JS 소스 탭 + 코드 복사 버튼을 제공해야 하며, 좌측에는 컴포넌트 목록과 함께 사이트에 필수적인 공통 레이어(모달·토스트·스낵바·바텀시트 등 알림/오버레이) 관련 UX 컴포넌트도 포함한다.

빌드 도구·패키지 매니저·프레임워크가 없는 **순수 정적 HTML/CSS/JS 프로젝트**다 (`package.json` 없음). 결과물은 하나 이상의 자기완결형(self-contained) HTML 파일이며, 최종적으로 GitHub Pages로 게시된다.

## 현재 상태

- `docs/resource/`에 서로 다른 스타일로 구현된 **참고용 프로토타입**들만 존재하고, 실제로 게시할 최종 페이지(entry point, `index.html` 등)는 아직 없다.
- 최종 산출물은 이 참고 자료들의 구조를 통합·정제해서 새로 만드는 것이 목표다. 기존 참고 파일을 직접 수정하지 말고, 필요한 패턴만 가져와 새 파일을 작성한다.

## 명령어

빌드/린트/테스트 파이프라인이 없다. 브라우저에서 HTML 파일을 직접 열거나, 필요 시 로컬 서버로 확인한다.

```bash
python3 -m http.server 8000   # 로컬에서 정적 파일 서빙 (또는 npx serve 등)
```


## `docs/resource/` 참고 자료 구조

최종 페이지를 설계할 때 아래 참고 파일들의 **패턴을 조합**해서 사용한다. 각 파일은 인라인 `<style>`/`<script>`만으로 완결되는 단일 HTML이며, 새로 만들 결과물도 이 자기완결형(self-contained) 방식을 따라야 한다(정적 배포 특성상 외부 빌드 산출물 없이 파일 자체가 바로 서빙됨).

- **`component-portfolio.html`** (`ui-elements-demo.html`과 완전히 동일한 파일, 중복본) — **가장 기준이 되는 구조**. 좌측 사이드바(카테고리별 그룹: `input`/`nav`/`info`/`container`) + 우측 상세 뷰. `ELEMENTS`/`ELEMENTS_2` 배열에 32개 요소 전체가 `{ n, name, ko, cat, desc, html, css, js }` 형태로 정의되어 있어, 요청한 "컴포넌트명-정의-설명 + 미리보기/HTML/CSS/JS 탭 + 복사 버튼" 요구사항의 데이터 스키마로 그대로 재사용할 만하다. 미리보기는 `iframe.srcdoc`으로 격리 렌더링하고, 복사는 `navigator.clipboard.writeText`, 탭 전환은 `data-tab` 속성 매칭 방식이다.
- **`ui-component.html`** — Tailwind CDN + Lucide 아이콘 기반 구현. `components` 배열의 각 항목이 `script()` 클로저를 가지며, 이 함수를 `toString()`으로 문자열화해 코드 탭에 JS 로직까지 노출하는 기법을 쓴다(HTML/CSS/JS 소스를 한 곳에서 관리하면서도 실제 실행과 코드 표시를 동시에 처리).
- **`alert-ui-showcase.html`** — 다크 테마. 요청에서 언급한 "공통 모달/알림/레이어" 요구사항의 핵심 참고 소스로, 토스트/스낵바, 상단 배너, 인라인 얼럿, 슬라이드오버 패널, 입력 흔들림(shake) 에러 피드백, 풀스크린 테이크오버, 팝오버 경고, 커맨드바(⌘K 스타일) 알림, 알림 리스트, 삭제 컨페티 등 **레이어/알림 계열 UI를 별도로 폭넓게** 다룬다. `demos` 객체에 `render()`/`fire()`/`code` 필드를 두는 패턴.
- **`ui-components-32-catalog.html`** — 카테고리 필터 버튼 + 검색창이 있는 카탈로그형 UI. 카테고리 4종(`input`/`nav`/`info`/`container`)에 색상 코드를 매핑하는 디자인 토큰 참고.
- **`label-chip-system.html`** — 칩/라벨 컴포넌트와 색상 시스템(디자인 토큰) 전용 참고 자료.
- **`colorpicker.html`**, **`color-personal-card.html`** — 32개 UI 요소 목록과는 별개로, 색상 선택/팔레트 UI를 다루는 보조 참고 자료(프로필 배경색 팔레트 등).

## 강제 디자인 규칙 — anti-ai-slop

[.claude/rules/anti-ai-slop.md](.claude/rules/anti-ai-slop.md)는 이 저장소의 모든 이미지·HTML·SVG·슬라이드 생성에 적용되는 **강제 규칙**이며 기본 동작을 override한다. 요지:

- **금지**: 그라데이션 일체, 색 들어간 그림자·글로우·`blur≥20px`, backdrop-filter 글래스모피즘, hover/load 시 transform·fade·키프레임 장식 모션, 배경 워터마크/그리드/광선, 카드 상단 컬러 액센트 바, 이모지 불릿, 마케팅 상투어.
- **강제**: 무채색 베이스 + 액센트 1색(색은 의미에만), 그림자는 중성 회색 1단계(`0 1px 2px rgba(0,0,0,.06)`)만, 구획은 `1px solid border`+여백으로, `border-radius` 0~8px, 위계는 크기·굵기·여백·정렬로, 폰트는 기본값(Inter/Roboto/Arial/system) 수렴 금지하고 목적에 맞게 의도적으로 고르고 이유를 한 줄 밝힘.

**주의**: `alert-ui-showcase.html`과 카탈로그 일부(헤더 숫자·카드 이미지의 `linear-gradient`)는 이 규칙 이전 자료라 위반 요소가 있다. 동작·구조는 참고하되 **비주얼은 규칙에 맞게 다시 작성**한다. 출력 전 `anti-ai-slop.md`의 자가 점검 체크리스트(그라데이션/컬러 그림자/장식 모션/배경 장식 등)를 통과시킨다.

## 컨벤션

- 모든 UI 텍스트와 컴포넌트 설명은 **한국어**로 작성한다(`lang="ko"`).
- 한글 가독성을 위해 Noto Sans KR / Pretendard 계열 웹폰트, 코드에는 JetBrains Mono를 우선 사용한다(참고 파일들의 공통 관례).
- 32개 요소는 4개 카테고리로 분류한다: `input`(입력 컨트롤), `nav`(내비게이션), `info`(정보 표시), `container`(컨테이너). 좌측 목록의 "공통 모달/알림 레이어" 컴포넌트는 이 4分類와 별도 그룹으로 취급한다(참고: `alert-ui-showcase.html`의 Lightweight/Interactive/Heavy 그룹 구분).
