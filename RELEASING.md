# 새 한글판 내는 순서

배포하는 쪽(=만드는 사람)이 보는 문서입니다. 사용자용 안내는 패키지 안의 `README-KO.md`.

## 준비: 저장소 한 번만 만들기

1. GitHub에 공개 저장소를 하나 만듭니다 (예: `HanaYou/bpp-korean`).
2. 저장소 루트에 `manifest.json`을 둡니다. **주소가 고정되어야 하므로 파일 이름과 경로를 바꾸지 마세요.**
   플러그인은 이 주소만 봅니다.

   ```
   https://raw.githubusercontent.com/<owner>/<repo>/main/manifest.json
   ```

3. 릴리스마다 zip을 자산(asset)으로 올립니다. `manifest.json`의 `packageUrl`이 그 자산을 가리킵니다.

`manifest.json`은 커밋 한 번으로 바뀌고, zip은 릴리스에 붙습니다. 즉 **새 버전을 알리는 일과
파일을 올리는 일이 분리**되어 있어서, 자산을 다 올린 뒤 마지막에 manifest를 커밋하면
중간 상태가 사용자에게 노출되지 않습니다.

## 매번 하는 일

```bash
# 1. 플러그인을 고쳤다면 빌드
dotnet build plugin/BazaarTagTools.csproj -c Release
dotnet build updater/BazaarKoUpdate.csproj -c Release
dotnet build updater-patcher/BazaarKoUpdate.Patcher.csproj -c Release

# 2. 업데이터를 건드렸다면 테스트
dotnet run --project updater-tests/UpdaterTests.csproj -c Release

# 3. dist/ 에 결과물 복사 후 패키징
python pack.py --version 5.3.0-ko.3 --repo <owner>/<repo> --notes "무엇을 고쳤는지"
```

`pack.py`가 만드는 것:

| 결과물 | 어디에 |
|---|---|
| `BazaarPlusPlus-Korean-v5.3.0-TagTools.zip` | `~/Downloads/` — 릴리스 자산으로 업로드 |
| `manifest.json` | `~/Downloads/bpp-korean-v5.3/` — 저장소 루트에 커밋 |
| `CHECKSUMS.sha256`, `BazaarKorean.version` | zip 안에 포함 |
| `BepInEx/config/bazaar.koupdate.cfg` | zip 안에 포함 (`--repo`를 줬을 때만) |

그리고 **순서대로**:

1. 릴리스를 만들고 (`--tag`가 없으면 태그는 `v<version>`) zip을 자산으로 올린다
2. 자산 업로드가 끝난 뒤에 `manifest.json`을 커밋한다

## 버전 규칙

`5.3.0-ko.N` — 앞은 담고 있는 BazaarPlusPlus 버전, 뒤는 한글판 회차입니다.
비교는 **숫자만** 뽑아서 합니다 (`5.3.0-ko.2` → `[5,3,0,2]`). 그래서 `ko.10`이 `ko.9`보다 큽니다.

BazaarPlusPlus 본체가 5.4로 올라가면 `--bpp-version 5.4.0.prod`도 같이 올려야 합니다.
이 값은 **사용자가 공식 인스톨러로 더 최신 BPP를 깔았을 때 자동 업데이트가 그걸 되돌리지 않도록**
막는 데 쓰입니다.

## 자동으로 설치되지 않는 변경

`BepInEx/core`, `BepInEx/patchers`, `winhttp.dll`, `doorstop_config.ini`가 바뀌면 사용자 쪽에서
자동 설치가 거부되고 "수동 설치 필요" 로그만 남습니다. 게임이 이미 붙잡고 있는 파일들이라
어쩔 수 없습니다. 이런 릴리스는 공지에 **zip을 직접 받아 덮어쓰라**고 적어야 합니다.

특히 `BazaarKoUpdate.Patcher.dll`은 사실상 고정이라고 보고 만들었습니다. 고쳐야 할 일이
생기면 그 회차는 수동 배포입니다.

## 자막만 바꿀 때

자막(`voice-lines.json`)은 BazaarPlusPlus 자체의 원격 카탈로그로도 갱신할 수 있습니다.
그 경로를 쓰면 20시간 안에 재시작 없이 반영되고, 업데이터도 릴리스도 필요 없습니다.

- 본체 dll에 박힌 카탈로그 주소를 우리 JSON 주소로 바꿔 빌드해야 합니다
  (지금은 `http://127.0.0.1:9/...` 라는 죽은 주소로 막아둔 상태)
- 올리는 JSON은 `merge_voice_lines.py`가 계산하는 `contentHash`가 맞아야 합니다
- 받기에 실패하면 dll에 임베드된 한국어 자막으로 되돌아가므로, 주소가 죽어도 중국어로 새지 않습니다
