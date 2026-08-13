# 모닝/나이트 브리핑 대시보드

샤오미 패드5 가로 화면용 아침·밤 대시보드. 5시~18시는 밝은 화면(Morning Briefing), 18시~5시는 어두운 화면(Night Briefing)으로 자동 전환.

## 올리는 법

이 폴더를 통째로 GitHub 레포에 넣고 Pages를 켜면 끝.

```
feelrun-ahn.github.io/
└── morning/
    ├── index.html
    ├── manifest.json
    ├── sw.js
    ├── icon-192.png
    └── icon-512.png
```

주소: `https://feelrun-ahn.github.io/morning/`

`file://`로 열면 날씨·경기·뉴스가 CORS에 막히고 서비스워커도 안 붙는다. 반드시 https로.

## 처음 한 번 할 것

1. 태블릿에서 위 주소 접속 → **설정**
2. 태어난 시각 (알면 입력, 모르면 비워 둠 — 사주가 3주에서 4주로 정확해짐)
3. 기상 알람 / 등교 시각 / 목표 취침 시각
4. 동기화 프로젝트 ID + API 키 → `동기화-설정법.md` 참고
5. Chrome ⋮ → **앱 설치** 또는 `APK-만드는법.md`로 APK 제작

## 자동으로 들어오는 것

| 항목 | 출처 | 갱신 |
|---|---|---|
| 날씨·미세먼지·자외선·일출입 | Open-Meteo | 15분 |
| 삼성 라이온즈 경기 | 네이버 스포츠 (비공식) | 6시간 |
| 뉴스 3건 | 구글 뉴스 토픽 RSS | 3시간 |
| 운세 | 앱 내부 명리 계산 (통신 없음) | 매일 |

## 직접 넣는 것

할 일 · 내일 할 일 · 수행평가 · D-day · 수면 점수 · (필요시) 라이온즈 수동 값

## 파일

- `index.html` — 앱 전체 (단일 파일, 외부 라이브러리 없음)
- `manifest.json`, `sw.js`, `icon-*.png` — 앱 설치 및 오프라인 캐시용
- `APK-만드는법.md` — PWA 설치 / PWABuilder APK / Capacitor
- `동기화-설정법.md` — Firestore로 폰과 목록 공유
