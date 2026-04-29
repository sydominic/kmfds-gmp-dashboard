# MFDS Regulatory Dashboard

식약처 게시물을 자동 수집하고, 외부 PostgreSQL DB에 누적 저장하는 Streamlit 온라인 대시보드입니다.

## 주요 기능
- 식약처 게시판 자동 수집
- 식약처 정보 / 구분별 정보 탭
- 기간 조회: 오늘 / 최근 7일 / 최근 14일 / 직접 선택
- SQLite fallback + 외부 PostgreSQL 지원
- Supabase/PostgreSQL 연결 시 온라인에서도 누적 DB 유지

## GitHub 업로드 구조

아래 파일과 폴더를 repository root에 업로드합니다.

```text
app.py
requirements.txt
.gitignore
README.md
.streamlit/config.toml
.streamlit/secrets.toml.example
data/.gitkeep
logs/.gitkeep
output/.gitkeep
```

## 외부 DB 연결

이 앱은 `DATABASE_URL`이 있으면 외부 PostgreSQL을 사용하고, 없으면 Local SQLite로 fallback합니다.

### Streamlit Cloud Secrets 예시

Streamlit Cloud 앱 화면에서 `Settings > Secrets`에 아래 형식으로 입력합니다.

```toml
DATABASE_URL = "postgresql://postgres.xxxxxx:YOUR_PASSWORD@aws-0-ap-northeast-2.pooler.supabase.com:6543/postgres"
```

주의:
- 실제 비밀번호가 들어간 `.streamlit/secrets.toml`은 GitHub에 올리지 마세요.
- GitHub에는 `.streamlit/secrets.toml.example`만 올립니다.

## Streamlit Cloud 배포 설정
- Repository: GitHub repository
- Branch: `main`
- Main file path: `app.py`

## 최초 실행
앱이 열리면 최근 14일 기준으로 식약처 게시물을 자동 수집합니다.
같은 날에는 자동 수집을 1회만 수행합니다.
필요하면 `수동 재수집` 버튼으로 선택 기간을 다시 수집할 수 있습니다.


## 드래그 업로드용 단순 파일 구성

GitHub 웹 업로드에서 숨김 폴더가 누락되는 문제를 피하기 위해 이 세트는 보이는 파일만 포함합니다.

업로드할 파일:
```text
app.py
requirements.txt
README.md
SECRETS_EXAMPLE.txt
```

`data`, `logs`, `output` 폴더는 앱 실행 시 자동 생성됩니다.
`.streamlit/config.toml`은 없어도 앱 실행에는 문제 없습니다.
디자인 CSS는 `app.py` 내부에 포함되어 있습니다.

## GitHub 업로드 방법
1. 이 ZIP을 압축 해제합니다.
2. 위 4개 파일을 GitHub 업로드 화면에 드래그합니다.
3. `Commit changes`를 누릅니다.
4. Streamlit Cloud에서 repository를 연결하고 Main file path를 `app.py`로 지정합니다.
5. Streamlit Cloud `Settings > Secrets`에 `DATABASE_URL`을 입력합니다.
