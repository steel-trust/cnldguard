# ColdGuard AI

냉간압연(TCM) 작업롤 불량 조기 판정 — POSCO K-뉴딜 아카데미 팀 프로젝트

> 데이터는 공개 시뮬레이션 데이터셋(Zenodo 11469702, TCM)이다. 실제 공장 데이터가 아니며, 결과는 방법 검증 범위로 한정한다.

## 폴더

| 폴더 | 내용 | 담당 팀 |
|---|---|---|
| `db/` | DB 스키마(`.sql`). DB가 처음 만들어질 때 이름 순서대로 자동 실행 | backend |
| `api/` | FastAPI | backend |
| `web/` | 대시보드 | frontend |
| `model/` | 전처리·모델 코드 (`.py`) | data |
| `notebooks/` | EDA·실험 노트북 (1인 1파일) | data |
| `docs/` | 기획·명세 문서 | PM |
| `data/` | **git에 올리지 않음.** 공유 드라이브에서 받아 넣는다 | — |

## 실행

1. `cp .env.example .env` 후 비밀번호를 채운다 (영문·숫자만).
2. `docker compose up -d`
3. 상태 확인: `docker compose ps` 에서 db가 `healthy` 이면 준비 완료.
4. DB 접속: `docker compose exec db psql -U coldguard -d coldguard`

끄기: `docker compose down` (데이터 유지) / `docker compose down -v` (DB 초기화)

> 스키마는 데이터팀의 컬럼명 확정 후 `db/01_schema.sql` 로 추가한다. `db/` 의 SQL은 **DB를 처음 만들 때만** 실행되므로, 스키마를 바꾼 뒤에는 `docker compose down -v` 로 초기화하고 다시 올린다.

## 협업 규칙

- `main` 에 직접 push 하지 않는다. 작업 브랜치 → PR → 리뷰 1명 승인 → merge.
- 브랜치 이름: `feat/…` 기능, `fix/…` 수정, `docs/…` 문서
- 커밋 메시지: `feat(api): 경보 목록 조회 추가` 처럼 `종류(범위): 내용`
- 이슈 제목에 WBS 번호를 붙인다. 예: `[2.3.1] EDA① 분포·데이터 이동`
- PR 설명에 `Closes #이슈번호` 를 쓰면 머지 때 이슈가 닫힌다.
- 노트북은 1인 1파일. 공통 코드는 `model/` 의 `.py` 로 뺀다.

## 지켜야 할 원칙

- 1·2번 파일은 탐지 학습·평가에 쓰지 않는다.
- 규칙은 5번 파일로만 고른다. 6번(시험) 정답은 규칙 확정 뒤 QA 담당만 연다. 저장소·공유 드라이브에 6번 정답을 두지 않는다.
- 정답 데이터(eval)는 API로 노출하지 않는다.
- DB 포트는 `127.0.0.1` 에만 연다. 비밀번호는 `.env` 에만 둔다.
