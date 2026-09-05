# The Bazaar — BazaarPlusPlus 한글판

[BazaarPlusPlus](https://bazaarplusplus.com) 공식 v5.3.0 빌드에 한글 패치를 적용한 배포본입니다.

## 한글판 추가 기능

- 음성 자막 및 UI 한글화
- 자막 X/Y 오프셋, 울트라와이드 지원
- 태그 정렬 · 태그 강조 기능
- 아이템 잠금 기능
- 사운드 증폭 기능

## 설치

1. 게임을 종료합니다.
2. [최신 릴리스](../../releases/latest)에서 zip을 받습니다.
3. 게임 루트 폴더에 그대로 덮어씁니다. 예) `C:\Steam\steamapps\common\The Bazaar\`
4. 게임을 실행합니다.

자세한 설명(자막 켜기, 설정 순서, 단축키, 자동 업데이트 동작 방식)은 zip 안의 `README-KO.md`에 있습니다.

## 자동 업데이트

새 한글판이 나오면 게임 실행 중에 배경에서 받아두고, **다음에 게임을 켤 때** 설치됩니다.
실행 중에는 이미 불러온 dll을 덮어쓸 수 없어서 이렇게 나눠 놨습니다.

설치 전에 파일마다 SHA256을 확인하고, 기존 파일은 백업한 뒤 교체합니다. 실패하면 되돌립니다.
끄고 싶으면 `BepInEx/config/bazaar.koupdate.cfg`의 `ManifestUrl`을 비우거나
`BepInEx/plugins/BazaarKoUpdate.dll`과 `BepInEx/patchers/BazaarKoUpdate.Patcher.dll`을 지우면 됩니다.

> `manifest.json`은 이 저장소 루트에 고정된 경로로 있어야 합니다. 설치된 플러그인이 그 주소만
> 보기 때문에, 파일을 옮기거나 이름을 바꾸면 기존 사용자의 자동 업데이트가 끊깁니다.


## 라이선스

- BazaarPlusPlus — MIT
- BepInEx — LGPL-2.1

라이선스 전문은 배포 zip의 `LICENSES/` 폴더에 함께 들어 있습니다.
이 저장소는 비공식 한글 패치이며 BazaarPlusPlus 및 The Bazaar 개발사와 관련이 없습니다.
