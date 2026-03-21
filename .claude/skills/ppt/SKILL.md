---
name: ppt
description: 주제나 원고를 받아 단일 HTML 파일로 된 프레젠테이션을 생성하는 스킬. "발표자료 만들어줘", "PPT 만들어줘", "슬라이드 만들어줘" 등의 요청에 사용한다.
disable-model-invocation: false
argument-hint: "<주제 또는 원고 파일 경로>"
---

# PPT 생성 스킬

주제, 원고, 또는 파일을 기반으로 **단일 HTML 프레젠테이션**을 생성한다.

## 핵심 원칙

1. **단일 파일** — CSS/JS 모두 인라인. 외부 의존성 없음 (폰트 CDN만 허용)
2. **뷰포트 피팅** — 모든 슬라이드는 `100vh`에 맞춤. 스크롤 없음. 넘치면 슬라이드 분리
3. **반응형** — 모든 폰트/간격에 `clamp()` 사용. 고정 px 금지
4. **접근성** — `prefers-reduced-motion` 지원

## 슬라이드 콘텐츠 밀도 제한

| 슬라이드 유형 | 최대 콘텐츠 |
|------------|-----------|
| 타이틀 | 제목 1 + 부제 1 |
| 콘텐츠 | 제목 1 + 불릿 4~6개 |
| 코드 | 제목 1 + 코드 8~10줄 |
| 인용 | 인용문 3줄 이내 + 출처 |
| 이미지 | 제목 1 + 이미지 1 (max 60vh) |

**초과 시 무조건 슬라이드 분리. 절대 구겨 넣지 않는다.**

---

## Step 1: 입력 분석

`$ARGUMENTS`를 확인한다:

- **주제만 있음** → 슬라이드 구성을 직접 설계
- **파일 경로** → 해당 파일을 읽어 내용 기반으로 구성
- **없음** → 사용자에게 주제 또는 원고를 요청

## Step 2: 슬라이드 구성안 제시

슬라이드 목록을 마크다운으로 작성하여 사용자에게 보여준다:

```
1. 타이틀 — 제목, 부제
2. 개요 — 다룰 내용 3가지
3. 섹션 A — 핵심 내용
...
```

사용자 확인 후 다음 단계로 진행한다.

## Step 3: 스타일 결정

사용자에게 분위기를 묻는다:

- **다크 & 임팩트** — 어두운 배경, 강렬한 액센트
- **라이트 & 클린** — 밝은 배경, 미니멀
- **컬러풀 & 에너지** — 생동감 있는 색상

사용자가 특별한 선호가 없으면 주제에 맞게 자동 선택한다.

## Step 4: HTML 생성

아래 구조를 따른다:

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>프레젠테이션 제목</title>
    <!-- 폰트: Google Fonts 또는 Fontshare -->
    <style>
        :root {
            --bg-primary: #...;
            --text-primary: #...;
            --accent: #...;
            --font-display: '...', sans-serif;
            --font-body: '...', sans-serif;
            --title-size: clamp(2rem, 6vw, 5rem);
            --body-size: clamp(0.75rem, 1.2vw, 1rem);
            --slide-padding: clamp(1.5rem, 4vw, 4rem);
        }

        /* 베이스 */
        * { margin: 0; padding: 0; box-sizing: border-box; }
        .slide {
            height: 100vh; height: 100dvh;
            overflow: hidden;
            display: flex; align-items: center; justify-content: center;
            padding: var(--slide-padding);
        }

        /* 반응형 */
        @media (max-height: 700px) { ... }
        @media (max-height: 500px) { ... }
        @media (prefers-reduced-motion: reduce) {
            *, *::before, *::after {
                animation: none !important;
                transition: none !important;
            }
        }
    </style>
</head>
<body>
    <div class="slide" id="slide-1">...</div>
    <div class="slide" id="slide-2">...</div>
    <!-- 네비게이션 -->
    <nav class="slide-nav">...</nav>
    <script>
        // 키보드(←→), 스와이프, 클릭 네비게이션
    </script>
</body>
</html>
```

### 필수 포함 기능

- **키보드 네비게이션**: ← → 화살표, Space
- **네비게이션 도트**: 현재 슬라이드 위치 표시
- **애니메이션**: 슬라이드 전환 + 요소 등장 효과
- **섹션 코멘트**: 각 섹션에 `/* === SECTION NAME === */` 주석

### 디자인 규칙

- 시스템 폰트(Arial, Inter 등) 사용 금지 — Fontshare/Google Fonts에서 개성 있는 폰트 선택
- 보라색 그라데이션 + 흰 배경 같은 뻔한 AI 스타일 금지
- CSS 변수로 테마 일괄 관리
- 이미지 사용 시 `max-height: min(50vh, 400px)`

## Step 5: 파일 저장 및 열기

1. 파일명을 정한다 (주제 기반, 예: `ai-intro-slides.html`)
2. 프로젝트 루트에 저장
3. `open <파일명>.html`로 브라우저에서 열기
4. 사용자에게 요약 전달:
   - 파일 위치
   - 슬라이드 수
   - 네비게이션 방법 (화살표, Space, 도트 클릭)
   - 커스터마이징: `:root` CSS 변수 수정

---

## 주의사항

- 슬라이드가 10개 이상이면 진행 바(progress bar) 추가
- 코드 슬라이드에는 신택스 하이라이팅 CSS 포함
- 한글 콘텐츠는 `lang="ko"` 설정
- `-clamp()` 직접 사용 금지 → `calc(-1 * clamp(...))` 사용
