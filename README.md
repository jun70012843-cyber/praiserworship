# PRAISER Worship Team — 프로젝트 인수인계 문서

> 작성일: 2026년 6월 27일  
> 작성자: 초기 개발자  
> 현재 상태: **초안 완성 / 운영 중**

---

## 📌 프로젝트 개요

종교/사역 아티스트 홈페이지. 예배팀 PRAISER의 공식 웹사이트로, 팀 소개 / 앨범 발매 / 악보 공유 / 사역 이력 / 후원 연결 기능을 포함한다.

---

## 🌐 배포 URL

| 구분 | URL |
|------|-----|
| 메인 사이트 | https://praiserworship.vercel.app |
| 관리자 페이지 | https://praiserworship.vercel.app/admin.html |

---

## 🔑 계정 정보

### GitHub
- **계정**: jun70012843-cyber
- **저장소**: https://github.com/jun70012843-cyber/praiserworship

### Vercel
- GitHub 계정과 동일하게 연동됨
- GitHub에 push하면 자동 배포됨 (별도 작업 불필요)

### Supabase
- **프로젝트 URL**: https://ylixwsraddpmtlglddyq.supabase.co
- **Project ID**: ylixwsraddpmtlglddyq
- **대시보드**: https://supabase.com/dashboard/project/ylixwsraddpmtlglddyq

> ⚠️ API Key 및 비밀번호는 별도 보안 문서로 관리할 것

### Admin 로그인 계정
- **이메일**: jun70012843@gmail.com
- **비밀번호**: 별도 관리

---

## 📁 파일 구조

```
praiser/
├── index.html        ← 방문자용 메인 페이지
├── admin.html        ← 관리자 콘텐츠 관리 페이지
├── assets/
│   ├── images/       ← 로컬 이미지 (현재 미사용, Supabase Storage 사용)
│   └── pdfs/         ← 로컬 PDF (현재 미사용, Supabase Storage 사용)
└── README.md         ← 이 문서
```

---

## 🗄️ Supabase 구조

### 데이터베이스 테이블

| 테이블 | 용도 |
|--------|------|
| `hero_slides` | 메인 풀스크린 슬라이더 사진 |
| `banners` | 4분할 배너 (제목/카테고리/링크) |
| `videos` | YouTube 영상 링크 |
| `scores` | 악보 PDF 파일 정보 |
| `contacts` | 방문자 문의 접수 |

### Storage 버킷

| 버킷 | 용도 |
|------|------|
| `worship-assets` | 히어로 사진(`/hero/`) + 악보 PDF(`/scores/`) |

### 주요 컬럼 설명

**hero_slides**
```
image_url   — Supabase Storage 공개 URL
order_num   — 슬라이드 순서 (숫자 낮을수록 먼저)
is_active   — false면 화면에 표시 안 됨
```

**banners**
```
title          — 큰 글씨 제목
category       — 영문 소카테고리 (예: MINISTRY INFO)
link_url       — 클릭 시 이동할 URL
is_highlighted — true면 밝은 배경 강조 카드
order_num      — 배너 순서
```

**videos**
```
youtube_url    — YouTube 전체 URL
thumbnail_url  — 자동 추출됨 (admin에서 입력 불필요)
order_num      — 표시 순서
```

**scores**
```
title       — 악보 제목
file_url    — Supabase Storage PDF URL
description — 부가 설명 (예: PDF · 무료 나눔)
```

**contacts**
```
name      — 문의자 이름
title     — 문의 제목
content   — 문의 내용
is_read   — admin에서 확인 처리 여부
```

---

## ⚙️ 기술 스택

| 역할 | 기술 |
|------|------|
| 프론트엔드 | 순수 HTML / CSS / Vanilla JS |
| 데이터베이스 | Supabase PostgreSQL |
| 파일 저장 | Supabase Storage |
| 인증 | Supabase Auth (Email/Password) |
| 배포 | Vercel (GitHub 자동 연동) |
| 프레임워크 | 없음 (순수 HTML) |

> 별도 빌드 과정 없음. 파일 수정 후 GitHub push만 하면 자동 배포됨.

---

## 🖥️ 관리자 페이지 사용법

### 접속
1. https://praiserworship.vercel.app/admin.html 접속
2. 등록된 이메일/비밀번호로 로그인

### 주요 기능

**히어로 사진**
- 사진 파일 선택 → 순서 입력 → "사진 추가" 클릭
- 권장 사이즈: 1920×1080px 이상, JPG/PNG
- 여러 장 올리면 자동 슬라이드됨

**배너 관리**
- 제목(한글) + 카테고리(영문) 입력 → 링크 URL 입력 → "배너 추가"
- "강조 카드" 체크하면 밝은 배경으로 표시됨
- 최대 4개 권장 (4분할 그리드)

**영상 관리**
- YouTube URL 붙여넣기 → "영상 추가"
- 썸네일은 자동으로 YouTube에서 가져옴

**악보 나눔**
- 제목 입력 + PDF 파일 선택 → "악보 추가"
- 업로드된 PDF는 방문자가 바로 다운로드 가능

**문의 목록**
- 방문자가 보낸 문의 확인
- "확인" 버튼 클릭 시 읽음 처리
- 미확인 문의는 대시보드에 숫자로 표시됨

---

## 🔧 코드 수정 방법 (비개발자용)

### 텍스트/색상만 바꾸고 싶을 때
1. VS Code에서 `praiser` 폴더 열기
2. `index.html` 수정
3. 저장 후 터미널에서:
```bash
cd ~/Desktop/praiser
git add .
git commit -m "수정 내용 간단히 작성"
git push origin main
```
4. 1~2분 후 사이트에 자동 반영

### 로고 이름 변경
`index.html`에서 `PRAISER` 검색 후 원하는 이름으로 교체

### 색상 변경
`index.html` 상단 `<style>` 태그 안에서:
- 배경색: `background: #0a0a0a` 부분
- 강조색: `background: #d8d8d8` 부분 (배너 강조 카드)

---

## 🚧 현재 미완성 / 추후 작업 목록

- [ ] 팀 소개(About) 전용 페이지
- [ ] 앨범 발매 섹션 (음원 스트리밍 임베드)
- [ ] 사역 이력 타임라인 페이지
- [ ] 모바일 햄버거 메뉴
- [ ] SNS 링크 (인스타그램, 유튜브 채널)
- [ ] 도메인 연결 (커스텀 도메인)
- [ ] 배너 순서 드래그앤드롭 정렬
- [ ] 히어로 사진 순서 변경 기능
- [ ] 이메일 문의 자동 알림 (Supabase Edge Function)

---

## 🆘 자주 발생하는 문제

### 업로드가 안 될 때
→ Supabase Storage 버킷 `worship-assets`가 Public으로 설정되어 있는지 확인  
→ Storage RLS 정책 3개가 등록되어 있는지 확인

### 로그인이 안 될 때
→ Supabase Authentication → Providers → Email → Confirm email이 OFF인지 확인

### 사이트가 안 바뀔 때
→ GitHub push 후 Vercel 대시보드에서 배포 상태 확인  
→ 브라우저에서 Cmd+Shift+R (강력 새로고침)

### admin 페이지가 index처럼 보일 때
→ 브라우저 캐시 문제일 수 있음 → 강력 새로고침  
→ GitHub에서 admin.html 파일 내용 직접 확인

---

## 📞 참고 링크

| 서비스 | 링크 |
|--------|------|
| Vercel 대시보드 | https://vercel.com/dashboard |
| Supabase 대시보드 | https://supabase.com/dashboard |
| GitHub 저장소 | https://github.com/jun70012843-cyber/praiserworship |
| Supabase 문서 | https://supabase.com/docs |
