# MFDS Regulatory Dashboard - Supabase 운영형

## 핵심 변경
- Supabase PostgreSQL을 외부 DB로 사용합니다.
- 앱 접속 시 무조건 수집하지 않습니다.
- DB에 저장된 데이터를 먼저 빠르게 표시합니다.
- [수동 재수집]을 누르면 선택 기간을 수집하여 Supabase에 신규 항목만 누적합니다.
- `AUTO_COLLECT_ON_LOAD = true`를 설정하면 1일 1회 자동 수집도 가능합니다.

## GitHub 업로드 파일
아래 4개 파일만 GitHub repository에 드래그 업로드하면 됩니다.

```text
app.py
requirements.txt
README.md
SECRETS_EXAMPLE.txt
```

## Streamlit Cloud 설정
- Repository: 본 GitHub repository
- Branch: `main`
- Main file path: `app.py`
- Python version: `3.11` 또는 `3.12` 권장

## Supabase 사용 방법

### 1. Supabase 프로젝트 생성
1. Supabase에 로그인합니다.
2. New project를 만듭니다.
3. Database password를 설정합니다.
4. 프로젝트 생성이 끝날 때까지 기다립니다.

### 2. Connection string 복사
Supabase Project 화면에서 아래 경로로 이동합니다.

```text
Project Settings > Database > Connection string
```

Streamlit Cloud 같은 환경은 연결이 짧게 반복될 수 있으므로, Supabase의 pooler connection string 사용을 권장합니다.

예시 형식:

```text
postgresql://postgres.xxxxxx:YOUR_PASSWORD@aws-0-ap-northeast-2.pooler.supabase.com:6543/postgres
```

`YOUR_PASSWORD`를 실제 DB password로 바꿉니다.

### 3. Streamlit Secrets 입력
Streamlit Cloud 앱 생성 또는 앱 Settings에서 Secrets에 아래처럼 입력합니다.

```toml
DATABASE_URL = "postgresql://postgres.xxxxxx:YOUR_PASSWORD@aws-0-ap-northeast-2.pooler.supabase.com:6543/postgres"
AUTO_COLLECT_ON_LOAD = false
```

처음에는 `AUTO_COLLECT_ON_LOAD = false`를 권장합니다.
이렇게 하면 접속할 때 오래 수집하느라 멈추지 않고, DB에 저장된 자료부터 바로 보여줍니다.

### 4. 최초 데이터 수집
앱이 배포되면 화면에서 기간을 선택하고 [수동 재수집]을 누릅니다.

추천 최초 수집:
```text
직접 선택: 2026-01-01 ~ 오늘
수동 재수집 클릭
```

수집된 데이터는 Supabase DB에 저장됩니다.
이후 접속자는 DB 데이터를 바로 조회합니다.

## 자동 수집을 켜고 싶을 때
Secrets에서 아래처럼 바꿉니다.

```toml
AUTO_COLLECT_ON_LOAD = true
```

이 경우 하루에 한 번, 첫 접속자가 최근 14일 기준 수집을 수행합니다.
다만 그 접속자는 수집이 끝날 때까지 로딩을 기다릴 수 있습니다.

## 주의
실제 DB 비밀번호는 GitHub에 올리지 마세요.
Secrets는 Streamlit Cloud Settings에서만 입력합니다.
