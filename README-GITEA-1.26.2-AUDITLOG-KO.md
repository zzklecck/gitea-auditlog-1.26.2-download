# Gitea 1.26.2 감사로그 Windows AMD64

두 운영 서버에서 확인된 `Gitea 1.26.2 / Go 1.26.3 / Windows AMD64` 환경용 실행 파일입니다.

- 실행 파일: `gitea-1.26.2-auditlog-windows-amd64.exe`
- 빌드 태그: `bindata sqlite sqlite_unlock_notify`
- SHA-256: `C82A27CB19FC247C4A8A8D92062B5DDE1E37184A1E1411A0D90FD3F77F3C5AB4`

## 주요 기능

- 관리자 메뉴 `감사로그` 및 `/-/admin/auditlog` 화면
- `gitea.log`, `gitea.log.*`, `access.log`, `access.log.*` 읽기
- 로그인 성공·실패 사용자 및 IP 병합 표시
- 시작일·종료일·사용자·구분 한 줄 필터
- 체크박스 복수 선택과 전체선택
- 기본 조회에서 `Git.Fetch/Pull/Clone` 제외, 필요할 때 선택 조회
- CSV 내보내기와 JSON API
- 긴 상세 내용 줄바꿈

## 권장 app.ini

```ini
[log]
ROOT_PATH = C:/Program Files/gitea/custom/log
MODE = file
LEVEL = Info
logger.access.MODE = access-file

[log.file]
FILE_NAME = gitea.log
LOG_ROTATE = true
DAILY_ROTATE = true
MAX_DAYS = 30
COMPRESS = false

[log.access-file]
MODE = file
FILE_NAME = access.log
LOG_ROTATE = true
DAILY_ROTATE = true
MAX_DAYS = 30
COMPRESS = false
```

회전 로그를 텍스트로 읽으므로 `COMPRESS = false`를 사용합니다.

## 배포

1. Gitea 서비스를 중지합니다.
2. 기존 `gitea.exe`와 `app.ini`를 백업합니다.
3. 제공된 EXE의 이름을 운영 파일명인 `gitea.exe`로 바꾸어 교체합니다.
4. `gitea.exe --version` 결과가 `1.26.2 built with go1.26.3 : bindata, sqlite, sqlite_unlock_notify`인지 확인합니다.
5. 서비스를 시작하고 관리자 메뉴의 `감사로그`를 확인합니다.

이 실행 파일은 템플릿과 프런트엔드 자산을 내부에 포함하므로 별도 템플릿 파일을 배포할 필요가 없습니다.

## 검증 결과

- 감사로그 파서 및 다중 필터 테스트 통과
- 관리자 라우터 테스트 통과
- 템플릿·프런트 자산 테스트 통과
- ESLint 통과
- Vite 운영 자산 빌드 통과
- 내장 자산에서 `templates/admin/auditlog.tmpl`, 감사로그 UI 및 `Git.Fetch/Pull/Clone` 항목 확인
- 실행 파일 버전 및 빌드 태그 확인
