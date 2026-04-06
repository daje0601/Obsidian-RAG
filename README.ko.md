# Obsidian RAG

벡터 데이터베이스 없이. 임베딩 없이. 코드 없이.  
Obsidian + Claude Code만으로 만드는 AI 지식 베이스.

> [Andrej Karpathy의 아이디어](https://x.com/karpathy)에서 영감을 받았습니다 — 기존 RAG 인프라 대신 구조화된 마크다운과 LLM을 사서로 활용합니다.

---

## 어떻게 동작하나요?

```
기사/자료를 클리핑 → raw/ 폴더에 저장
        ↓
Claude Code가 읽고 정리 → wiki/ 폴더에 구조화
        ↓
질문하면 → Claude가 인덱스를 탐색 → 답변
```

기존 RAG: `문서 → 임베딩 → 벡터DB → 유사도 검색 → LLM`  
Obsidian RAG: `문서 → 마크다운 → Claude Code가 직접 파일을 읽음`

### 검색 원리 (벡터 DB가 필요 없는 이유)

질문을 하면 Claude Code가 3~4번의 파일 읽기만으로 답을 찾습니다:

1. `_master-index.md` 읽기 — "어떤 토픽들이 있지?"
2. 해당 토픽 폴더의 `_index.md` 읽기 — "관련 문서가 뭐가 있지?"
3. 실제 문서 1~3개 읽기
4. 답변 합성

수백~수천 개의 문서까지 확장 가능합니다.

---

## 빠른 시작

### 사전 준비

- [Obsidian](https://obsidian.md) (무료)
- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) (`npm install -g @anthropic-ai/claude-code`)

### Step 1 — 이 저장소를 Clone

Obsidian이 접근할 수 있는 위치에 clone합니다:

```bash
# 예시: Documents 폴더에 clone
git clone https://github.com/YOUR_USERNAME/obsidian-RAG.git

# iCloud에 연결된 Obsidian을 사용하는 경우:
cd ~/Library/Mobile\ Documents/iCloud~md~obsidian/Documents/
git clone https://github.com/YOUR_USERNAME/obsidian-RAG.git
```

### Step 2 — Obsidian에서 Vault로 열기

1. Obsidian 실행
2. **"폴더를 보관소로 열기"** 클릭
3. clone한 `obsidian-RAG` 폴더 선택
4. 끝 — 사이드바에 폴더 구조가 보입니다

### Step 3 — Claude Code 실행

```bash
cd /path/to/obsidian-RAG
claude
```

Claude Code가 `CLAUDE.md`를 자동으로 읽고 지식 베이스 사서 역할을 시작합니다.

### Step 4 — 직접 해보기

`raw/` 폴더에 예제 파일이 있습니다. Claude Code에게 이렇게 말해보세요:

```
compile
```

raw 파일들이 구조화된 위키 문서로 정리되는 과정을 확인할 수 있습니다.

---

## 4가지 핵심 명령

| 명령 | 동작 |
|------|------|
| **Clip** | 기사/자료를 `raw/`에 저장 (웹 클리퍼 또는 수동) |
| **Compile** | Claude Code에게 `compile` — `raw/`를 `wiki/`로 정리 |
| **Query** | 질문하면 Claude가 위키를 탐색해서 답변 |
| **Audit** | Claude Code에게 `audit` — 위키의 문제점과 개선점 검토 |

### Compile (컴파일)

```
compile
```

Claude Code가 수행하는 작업:
1. `raw/`의 각 파일 읽기
2. 적절한 토픽 폴더를 `wiki/`에 생성하거나 찾기
3. 핵심 요약과 [[위키 링크]]가 포함된 구조화된 문서 작성
4. 모든 인덱스 업데이트

### Query (질문)

```
기존 RAG와 Obsidian RAG의 핵심 차이점이 뭐야?
```

```
위키 내용을 기반으로 [개념 A]와 [개념 B]의 관계를 설명해줘
```

```
위키의 모든 내용을 기반으로 [주제]에 대한 주요 접근 방식을 정리해줘.
답변을 새 위키 문서로 저장해.
```

마지막 질문이 핵심입니다 — 답변이 지식 베이스의 일부가 됩니다. 질문할수록 시스템이 똑똑해집니다.

### Audit (감사)

```
audit
```

Claude Code가 전체 위키를 검토하고 보고합니다:
- 문서 간 모순되는 정보
- 누락된 교차 링크
- 다루지 않은 주제
- 추가하면 좋을 문서 제안

---

## 폴더 구조

```
obsidian-RAG/
├── CLAUDE.md                        # Claude Code 규칙 (매 세션 자동 읽힘)
├── README.md                        # 영문 튜토리얼
├── README.ko.md                     # 한국어 튜토리얼 (이 파일)
├── raw/                             # 입력 폴더 — 자료를 여기에 넣으세요
│   ├── example-*.md                 # 예제 파일 (바로 compile 해보세요)
│   └── (클리핑한 기사들)
├── wiki/                            # Claude Code 관리 영역
│   ├── _master-index.md             # 전체 토픽 목록 (진입점)
│   └── {topic}/                     # 토픽별 서브폴더 (compile 시 생성)
│       ├── _index.md                # 해당 토픽의 문서 목록
│       └── *.md                     # 개별 문서
└── output/                          # 질의 결과 및 리포트
```

---

## 선택 사항: Obsidian 웹 클리퍼

웹 기사를 원클릭으로 `raw/`에 저장하려면:

1. 브라우저 확장 프로그램 스토어에서 **Obsidian Web Clipper** 설치
2. 설정 → General → vault 추가
3. Templates → Default 템플릿 편집:
   - **Vault**: 본인의 vault 선택
   - **Note location**: `raw`
   - **Note name**: `{{date|date:"YYYY-MM-DD"}}-{{title|safe_name}}`

이제 웹 기사를 한 번 클릭으로 `raw/`에 마크다운으로 저장할 수 있습니다.

### 선택 사항: Local Images Plus

클리핑한 기사의 이미지를 자동 다운로드하려면:

1. Obsidian → 설정 → 커뮤니티 플러그인 → 탐색
2. **"Local Images Plus"** 검색 → 설치 → 활성화

---

## Obsidian RAG vs 기존 RAG 비교

| | Obsidian RAG | 기존 RAG |
|---|---|---|
| **인프라** | 없음 (파일만 있으면 됨) | 벡터DB + 임베딩 모델 + API |
| **설치 시간** | 5분 | 수 시간 ~ 수 일 |
| **사람이 읽을 수 있나** | Yes — 직접 위키를 열람 가능 | No — 벡터 저장소는 블랙박스 |
| **자기 개선** | audit로 품질 개선, 질문이 지식 추가 | 재인덱싱 전까지 정적 |
| **적합한 용도** | 개인, 연구자, 소규모 팀 | 프로덕션 앱, 5000+ 문서, 다중 사용자 |
| **비용** | Claude Code 구독만 | DB 호스팅 + 임베딩 API + LLM API |

### 기존 RAG로 전환이 필요한 시점
- 5,000개 이상 문서로 인덱스 탐색이 어려워질 때
- 시맨틱 검색 폴백이 필요할 때
- 다중 사용자 또는 프로덕션 배포가 필요할 때
- 수백 개 문서를 한 번에 대량 처리해야 할 때

---

## 팁

- **일상 워크플로우**: Clip → Compile → Query → Audit
- **교차 링크**: 문서 간 [[위키 링크]]가 많을수록 Claude가 더 잘 탐색합니다
- **지식 복리**: 합성 질문을 하고 답변을 위키에 저장하세요 — 질문할수록 시스템이 똑똑해집니다
- **정기 감사**: 주 1회 `audit`을 실행해서 품질을 유지하세요

---

## 라이선스

Apache-2.0
