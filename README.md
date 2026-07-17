# chadon-release

**차돈** (자동차 정비소 매출/매입 관리 프로그램)의 **배포 저장소**입니다.
GitHub Pages로 서비스되는 공식 웹사이트(<https://chadon.mokky.store>)와
사용자 앱이 자동 업데이트에 사용하는 버전 정보를 담습니다.

앱 소스 코드는 [garbage-money](https://github.com/K-Mokky/garbage-money) 저장소에 있습니다.

## 구성

```
index.html          웹사이트 (프로그램 소개 · 사용 방법 · 다운로드)
latest.json         버전 정보 — 배포된 모든 차돈 앱이 이 파일을 보고 새 버전을 감지
release/Chadon.exe  최신 버전 실행 파일 (다운로드 버튼이 가리키는 파일)
CNAME               커스텀 도메인 (chadon.mokky.store)
favicon.ico         사이트 아이콘
```

`latest.json` 형식:

```json
{
  "version": "1.0.1",
  "url": "https://chadon.mokky.store/release/Chadon.exe",
  "page": "https://chadon.mokky.store",
  "notes": "이번 버전에서 바뀐 점 (앱 업데이트 창에 표시)"
}
```

## 새 버전 배포 방법

### 방법 1 — GitHub Actions 자동 배포 (권장)

garbage-money 저장소에서 버전 태그를 푸시하면 빌드 → 이 저장소 커밋까지 자동으로 됩니다.

1. `garbage-money/chadon/__init__.py` 의 `VERSION` 을 올린다.
2. `git tag v1.0.2 && git push origin v1.0.2`
3. 몇 분 뒤 이 저장소에 `release/Chadon.exe` + `latest.json` 이 커밋되고
   웹사이트와 앱 자동 업데이트에 반영된다.

> 최초 1회 설정: garbage-money 저장소의 **Settings → Secrets and variables →
> Actions** 에 이 저장소 쓰기 권한이 있는 PAT을 `RELEASE_REPO_TOKEN` 이름으로 등록.

### 방법 2 — 내 PC에서 직접 배포

garbage-money 를 이 저장소와 같은 부모 폴더에 클론해 두고:

```bat
rem garbage-money 폴더에서
build.bat                                       rem 빌드 + 옆의 chadon-release로 복사
python tools\publish_release.py --notes "바뀐 점" --push   rem 커밋/푸시까지
```

## 최초 웹사이트 설정 (1회)

1. **GitHub Pages 켜기** — 이 저장소 **Settings → Pages** 에서
   Source: `Deploy from a branch`, Branch: `main` / `/ (root)` 선택.
2. **DNS 설정** — mokky.store 도메인의 DNS에 CNAME 레코드 추가:

   | 종류  | 이름(호스트) | 값                  |
   |-------|--------------|---------------------|
   | CNAME | `chadon`     | `k-mokky.github.io` |

3. Pages 설정 화면의 Custom domain 에 `chadon.mokky.store` 가 잡히면
   **Enforce HTTPS** 를 켠다. (저장소의 `CNAME` 파일이 이미 있으므로 자동 인식됨)

## 자동 업데이트 동작

- 차돈 앱은 시작 5초 후(및 메뉴 *도움말 → 업데이트 확인*에서)
  `https://chadon.mokky.store/latest.json` 을 확인한다.
- `version` 이 실행 중인 버전보다 높으면 "새 버전이 나왔습니다" 안내를 띄우고,
  사용자가 수락하면 `url` 의 exe를 내려받아 스스로 교체·재시작한다.
- exe 파일명은 GitHub 자산명 한글 깨짐을 피해 ASCII(`Chadon.exe`)를 사용한다.
