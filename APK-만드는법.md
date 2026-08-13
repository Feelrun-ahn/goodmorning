# 모닝 브리핑 — 진짜 앱으로 만들기

파일 5개(`index.html`, `manifest.json`, `sw.js`, `icon-192.png`, `icon-512.png`)가 한 폴더에 같이 있어야 한다.

---

## 1단계 (공통) — GitHub Pages에 올리기

이미 쓰고 있는 `feelrun-ahn.github.io` 레포에 폴더 하나 만들어서 파일 5개를 넣는다.

```
feelrun-ahn.github.io/
└── morning/
    ├── index.html
    ├── manifest.json
    ├── sw.js
    ├── icon-192.png
    └── icon-512.png
```

→ 주소: `https://feelrun-ahn.github.io/morning/`

https여야 하는 이유: 서비스워커(오프라인 캐시), 화면 꺼짐 방지, 설치 배너가 전부 https에서만 동작한다. 로컬 `file://`로 열면 날씨·경기 API도 CORS에 막힌다.

---

## 2단계 — 셋 중 하나 고르기

### A. PWA 설치 (제일 빠름, 5분)
태블릿 Chrome으로 위 주소 접속 → 우측 상단 ⋮ → **앱 설치**.
홈 화면에 아이콘 생기고, 주소창 없는 전체화면으로 뜬다. 체감상 APK와 차이가 거의 없다.

### B. PWABuilder로 APK 뽑기 (추천, 15분)
1. PC에서 `https://www.pwabuilder.com` 접속
2. 주소 입력 → Start
3. **Package For Stores → Android → Generate**
4. 받은 zip 안의 `.apk`를 태블릿에 복사해서 설치
   (설정 → 앱 → 특별 권한 → 알 수 없는 앱 설치 → 파일 관리자 허용)
5. zip 안에 있는 `assetlinks.json`을 `feelrun-ahn.github.io/.well-known/assetlinks.json`에 올리면 주소창이 완전히 사라진다

이 방식은 Chrome 엔진을 그대로 쓰는 TWA라서 웹 그대로 동작하고, 앱을 고칠 때 GitHub에 push만 하면 APK 재설치 없이 반영된다.

### C. Capacitor로 오프라인 완전 내장 APK (와이파이 없어도 실행)
PC에 Node.js + Android Studio 필요.

```bash
npm create @capacitor/app morning
cd morning
# www/ 폴더 안을 파일 5개로 교체
npx cap add android
npx cap sync
npx cap open android      # Android Studio에서 Build > Build APK
```

앱 껍데기가 기기 안에 들어가서 비행기 모드에서도 열린다. 날씨·경기만 네트워크가 필요.

---

## 아침 대시보드처럼 쓰는 설정 (MIUI / HyperOS)

- 설정 → 디스플레이 → **화면 자동 꺼짐: 10분** (앱이 wakeLock을 걸지만 백업용)
- 설정 → 배터리 → 앱 배터리 최적화 → 모닝 브리핑 **제한 없음**
- 충전 중 항상 켜짐을 쓰려면 개발자 옵션 → **화면 켜짐 상태 유지**
- 알람이 확실히 울리게 하려면 앱을 최근 앱 목록에서 **잠금(자물쇠)** 걸어두기

> 웹 알람은 화면이 꺼지면 소리가 끊길 수 있다. 진짜 기상 알람은 시계 앱을 백업으로 같이 걸어두고, 이 앱은 "일어나서 켜는 브리핑 + 기상 미션"으로 쓰는 게 안전하다.

---

## 데이터 출처

- 날씨·미세먼지: Open-Meteo (키 불필요)
- KBO 일정: 네이버 스포츠 비공식 게이트웨이 (`api-gw.sports.naver.com`). 직접 호출이 CORS로 막히면 자동으로 프록시(allorigins → corsproxy) 순으로 재시도한다.
- 운세: 앱 내부 명리 계산 (외부 통신 없음)
