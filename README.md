# Gemini CLI 및 Git 연동 설정

이 문서는 Gemini CLI를 설정하고 Git과 연동하는 과정을 안내합니다.

## 1. Gemini CLI 설정

Gemini CLI를 사용하기 위한 초기 설정 과정입니다.

```bash
# Gemini CLI 초기화
gcloud alpha code dev-tools gemini init
```

위 명령어를 실행하면 Gemini CLI 사용에 필요한 기본 설정이 완료됩니다.

## 2. Git 연동

Gemini CLI와 Git을 연동하여 버전 관리를 효율적으로 할 수 있습니다.

```bash
# Git 저장소 초기화
git init

# 원격 저장소 연결 (예: GitHub)
git remote add origin <REMOTE_REPOSITORY_URL>

# 변경사항 스테이징
git add .

# 첫 커밋
git commit -m "Initial commit with Gemini CLI setup"

# 원격 저장소로 푸시
git push -u origin main
```

## 3. Windows에서 Gemini CLI 전역 설정

Windows 환경에서 Gemini CLI를 어떤 경로에서든 사용할 수 있도록 전역으로 설정하는 방법입니다.

1.  **Gemini CLI 설치 경로 확인**: `gcloud` 명령어를 통해 설치된 경로를 찾습니다. 일반적으로 `C:\Users\<YourUser>\AppData\Local\Google\Cloud SDK\google-cloud-sdk\bin` 와 유사한 경로에 있습니다.
2.  **환경 변수 설정**:
    *   `시스템 속성` > `고급` > `환경 변수`로 이동합니다.
    *   `시스템 변수` 섹션에서 `Path` 변수를 찾아 `편집`을 클릭합니다.
    *   `새로 만들기`를 클릭하고 위에서 확인한 Gemini CLI 설치 경로를 추가합니다.
3.  **확인**: 새로운 명령 프롬프트(cmd) 또는 PowerShell 창을 열고 다음 명령어를 실행하여 설정이 올바르게 되었는지 확인합니다.

    ```bash
    gcloud alpha code dev-tools gemini --help
    ```

이제 어느 위치에서든 `gcloud alpha code dev-tools gemini` 명령어를 사용할 수 있습니다.
