# 사용 가이드 (한국어)

> "몇 년 뒤에 다시 열어봐도 이해할 수 있도록" 정리한 문서입니다.

이 저장소는 [Millennial](https://github.com/LeNPaul/Millennial) 테마를 기반으로
한 **Jekyll** 사이트입니다. GitHub Pages 에서 자동 빌드되어 배포됩니다.

---

## 1. 구조 한 눈에 보기

```
.
├── _config.yml            # 사이트 전역 설정 (제목/플러그인/페이지네이션 등)
├── _data/settings.yml     # 메뉴, 소셜, GA, 댓글 등 자주 바꾸는 설정
├── _includes/             # 부분 템플릿 (header / footer / 카드 / 공유 등)
├── _layouts/              # 페이지 레이아웃 (default / home / post / page / category)
├── _posts/                # ✍️ 블로그 글이 들어가는 곳  *가장 자주 만지는 폴더*
│   └── YYYY-MM-DD-제목.md
├── _sass/                 # SCSS 파셜
│   └── _variables.scss    #   → 색상/폰트/간격 등 디자인 토큰
├── assets/
│   ├── css/main.scss      # 진입 SCSS (여기서 파셜들을 import)
│   └── img/               # 포스트/썸네일 이미지
├── pages/                 # 카테고리 페이지, About 페이지 등
├── index.html             # 홈(home 레이아웃 진입점)
├── 404.md                 # 404 페이지
└── Gemfile                # 로컬 실행용 의존성
```

---

## 2. 새 글 쓰는 법

1. `_posts/` 에 파일을 만듭니다. 파일명은 **반드시** `YYYY-MM-DD-slug.md` 형식.
   - 예: `2026-04-17-hello.md`
2. 파일 최상단에 front matter 를 넣습니다.

```markdown
---
layout: post
title: "글 제목"
author: "피터 파크"
categories: tech        # daily | discovery | tech | reference 중 택1
tags: [jekyll, blog]
image: view-0001.jpg    # (선택) assets/img/ 안의 파일명
---

본문은 마크다운으로 자유롭게.
```

3. 저장하면 끝. `categories` 에 `tech` 를 넣으면 `/tech` 페이지에도 자동 노출됩니다.

### 이미지 추가

`assets/img/` 폴더에 파일을 넣고, 본문에서는 다음처럼 참조합니다.

```markdown
![설명](/assets/img/your-image.jpg)
```

썸네일로 쓰고 싶다면 front matter 의 `image:` 항목에 파일명을 적으세요.

---

## 3. 메뉴·소셜 링크 바꾸기

`_data/settings.yml` 한 파일만 만지면 됩니다.

```yaml
menu:
  - {name: '일상',     url: 'daily'}
  - {name: '기술',     url: 'tech'}

social:
  - {icon: 'github',  link: 'https://github.com/your-id'}
  - {icon: 'envelope', link: 'mailto:you@example.com'}
```

아이콘 이름은 [Font Awesome 6](https://fontawesome.com/search?o=r&m=free) 의
브랜드/솔리드 아이콘 이름을 그대로 씁니다. `envelope`, `rss` 는 솔리드로, 나머지
소셜은 브랜드 아이콘으로 자동 처리됩니다.

---

## 4. 디자인 커스터마이즈

모든 색/폰트는 CSS 변수로 관리합니다. **`_sass/_variables.scss`** 의 값을
바꾸는 것만으로 라이트/다크 테마가 모두 바뀝니다.

- 본문 폰트: `Pretendard Variable` (한글 최적화)
- 코드 폰트: `JetBrains Mono`
- 오른쪽 위 🌙/☀️ 버튼으로 라이트/다크 전환 (선택은 `localStorage` 에 저장)

---

## 5. 로컬에서 미리보기

루비(Ruby) 가 설치돼 있어야 합니다. macOS 에는 기본 설치돼 있지만
`rbenv` 등으로 3.x 대의 루비를 쓰시는 걸 권장합니다.

```bash
# 의존성 설치 (최초 1회)
bundle install

# 로컬 서버 실행 → http://localhost:4000
bundle exec jekyll serve --livereload
```

> GitHub Pages 가 쓰는 Jekyll 버전과 동일하게 맞추고 싶다면
> `github-pages` 젬을 `Gemfile` 에 추가하세요.

---

## 6. 배포

- `master` (또는 `main`) 브랜치에 푸시하면 GitHub Pages 가 자동 빌드합니다.
- 저장소 이름이 `USERNAME.github.io` 라면 루트에 배포됩니다.
- 커스텀 도메인을 쓰려면 `CNAME` 파일을 루트에 추가하세요.

---

## 7. 댓글 (선택)

### Giscus (추천: GitHub Discussions 기반)

1. 저장소에서 Discussions 활성화
2. <https://giscus.app> 에서 `repo`, `repo_id`, `category`, `category_id` 발급
3. `_data/settings.yml` 의 `giscus:` 블록 주석을 풀고 값 입력

### Disqus (레거시)

`_data/settings.yml` 에서 `disqus.comments: true`, `disqus_shortname` 설정.

---

## 8. 자주 만지는 파일 치트시트

| 하고 싶은 일                | 열어야 할 파일                         |
| ---------------------------- | -------------------------------------- |
| 새 글 쓰기                   | `_posts/YYYY-MM-DD-제목.md`            |
| 사이트 제목/설명             | `_config.yml`                          |
| 메뉴 / 소셜 / GA / 댓글      | `_data/settings.yml`                   |
| 색상 / 다크 모드 팔레트      | `_sass/_variables.scss`                |
| 폰트                         | `assets/css/main.scss` 상단            |
| 홈 레이아웃                  | `_layouts/home.html`                   |
| 포스트 레이아웃              | `_layouts/post.html`                   |
| 카테고리 페이지 설명         | `pages/{daily,discovery,tech,reference}.md` |
| 404 페이지                   | `404.md`                               |

---

## 9. 앞으로 해볼 만한 것

- [ ] `info.md` 를 주제별로 쪼개서 포스트로 분리
- [ ] Obsidian 볼트에서 `_posts/` 로 자동 동기화하는 스크립트
- [ ] Lunr.js 기반 사이트 내 검색
- [ ] `utterances` / `giscus` 댓글 활성화
- [ ] 태그 페이지 (`/tags/*`) 자동 생성

즐거운 블로깅 되세요. ✍️
