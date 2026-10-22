# chadon-release

**차돈** (자동차 정비소 매출/매입 관리 프로그램)의 **배포 저장소**입니다.
GitHub Pages로 서비스되는 공식 웹사이트(<https://chadon.mokky.store>)와
사용자 앱이 자동 업데이트에 사용하는 버전 정보를 담습니다.

소스 코드는 옆 폴더 `garbage-money/` 입니다. 이 저장소에는 **내려받는 파일과
소개 페이지만** 둡니다. 새 버전 발행은 `garbage-money/tools/publish_release.py`.

## 파일 지도

| 경로 | 역할 |
|------|------|
| `index.html` | 소개·다운로드 페이지. 버전 숫자는 `latest.json` 을 fetch 해서 표시. |
| `latest.json` | 앱 자동 업데이트 출처. `version` / `url` / `sha256` / `sig` 가 필수. |
| `release/Chadon.exe` | 사용자가 받는 실행 파일 (ASCII 이름 — GitHub Pages 한글 파일명 깨짐 방지). |
| `review/index.html` | 모바일 조회 틀. iframe 으로 인증 서버 `/m` 을 담는다. `VIEW_SRC` 한 줄만 바꾸면 서버 주소를 옮길 수 있다. |
| `CNAME` | GitHub Pages 커스텀 도메인 `chadon.mokky.store`. |
| `robots.txt` / `sitemap.xml` | 소개 페이지만 검색에 올리고, exe·latest.json 은 크롤하지 않는다. |
| `.well-known/security.txt` | 취약점 제보 연락처. 비밀·서버 주소는 적지 않는다. |

## 손댈 때

- **새 버전**: `garbage-money` 에서 `build.bat` → `publish_release.py` 가
  `release/Chadon.exe` 와 `latest.json` 을 덮어쓴다. 직접 고치지 않는다.
- **소개 문구**: `index.html` 만 고친다. 자료 저장 위치는
  `%APPDATA%\chadon\chadon.db` (exe 옆 `data/` 가 아님).
- **모바일 조회 서버 주소**: `review/index.html` 의 `VIEW_SRC`.
- 개인키(`.key`/`.pem`)와 `.env` 는 이 저장소에 올리지 않는다 (`.gitignore`).

모바일 조회는 공식 도메인 한 곳으로만 안내합니다:
<https://chadon.mokky.store/review>
(조회 화면은 인증서버가 그려 이 페이지 안에 담기며, 사용자가 보는 주소는
언제나 위 도메인 하나입니다.)
