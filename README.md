

```markdown
# Streamlit 서술형 평가 앱 (Step 1-2 + GPT 피드백)

이 프로젝트는 **Streamlit 단일 파일(app.py)** 로 실행되는 연수용 예시입니다.

- **Step 1-2**: 학번 입력 + 서술형 3문항 답안 제출 (`st.form` 사용)
- **Step 2**: OpenAI GPT API로 문항별 **O/X 판정 + 200자 이내 피드백** 생성
- (추후 확장) **Supabase DB 저장**을 염두에 두고 payload 구조를 미리 준비

---

## 1. 실행 환경

- Python **3.10 이상 권장**
- VS Code (권장)
- 필수 라이브러리
  - `streamlit`
  - `openai`

---

## 2. 폴더 구조 (권장)

```

project/
├─ app.py
└─ .streamlit/
└─ secrets.toml

````

> `secrets.toml`은 반드시 `.streamlit` 폴더 안에 있어야 합니다.

---

## 3. 가상환경(venv) 생성 및 설치

### Windows (PowerShell)

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install streamlit openai
````

※ 실행 정책 오류가 나면:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
.\.venv\Scripts\Activate.ps1
```

### Windows (CMD)

```bat
python -m venv .venv
.\.venv\Scripts\activate.bat
pip install streamlit openai
```

### macOS / Linux

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install streamlit openai
```

---

## 4. OpenAI API 키 설정

파일 생성:

```
.streamlit/secrets.toml
```

내용 예시:

```toml
OPENAI_API_KEY = "본인의_API_키"
```

* API 키가 없어도 **Step 1 화면은 실행됨**
* GPT 버튼을 누를 때만 키가 필요함

---

## 5. 실행 방법

```bash
streamlit run app.py
```

브라우저에서 자동 실행되며, 보통 아래 주소로 접속됩니다.

```
http://localhost:8501
```

---

## 6. 사용 흐름 (연수 시연 기준)

1. 학번 입력
2. 서술형 3문항 답안 작성
3. **[제출] 버튼 클릭**

   * 학번/빈 답안 검증 통과 시 → “제출 완료”
   * `st.session_state.submitted_ok = True` 설정
4. **[GPT 피드백 확인] 버튼 활성화**
5. 문항별 피드백 출력

   * 형식: `O: ...` 또는 `X: ...`
   * 최대 200자

---

## 7. GPT 모델 관련 설정

### 권장 기본값 (연수용)

* 모델: `gpt-5-mini`
* 파라미터:

  * `max_completion_tokens`: **800~1000**
  * `temperature`: 사용하지 않음
  * `reasoning_effort`: `"minimal"` 권장

> GPT-5 계열은 `max_tokens` / `temperature=0`을 지원하지 않음

---

## 8. 생성되는 데이터 구조 (Supabase 연동 대비)

GPT 실행 후 아래 데이터가 세션에 저장됩니다.

```python
st.session_state.gpt_payload = {
  "student_id": "10123",
  "answers": {
    "Q1": "...",
    "Q2": "...",
    "Q3": "..."
  },
  "feedbacks": {
    "Q1": "O: ...",
    "Q2": "X: ...",
    "Q3": "O: ..."
  },
  "guidelines": {...},
  "model": "gpt-5-mini",
  "created_at": "2026-01-01T12:00:00Z"
}
```

→ Supabase에서는 이 payload를 **jsonb 컬럼으로 그대로 저장**하는 방식이 가장 단순합니다.

---

## 9. 자주 발생하는 문제 (중요)

### ① GPT 버튼이 비활성화됨

* 원인: 제출 성공 시 `submitted_ok = True`가 설정되지 않음
* 해결:

  * 학번/모든 답안 작성 후 **제출 버튼 먼저 클릭**

### ② `max_tokens` 관련 400 에러

* 원인: GPT-5 계열은 `max_tokens` 미지원
* 해결:

  * `max_completion_tokens` 사용

### ③ `temperature=0` 에러

* 원인: GPT-5 계열은 기본 temperature(1)만 허용
* 해결:

  * `temperature` 파라미터 제거

### ④ 결과가 `X: 피드백 생성 실패`로만 출력

* 원인:

  * 출력 토큰 부족
  * reasoning 토큰 과다 사용
* 해결:

  * `max_completion_tokens` → **800 이상**
  * `reasoning_effort="minimal"` 설정

---

## 10. 연수 운영 팁

* 처음 시연:

  * **Step 1만 실행 → UI 구조 설명**
* 그 다음:

  * API 키 설정 후 Step 2 실행
* 비용 절감:

  * 필요 시 `gpt-5-nano`로 변경 후 품질 비교
* 안정성:

  * 출력 포맷은 코드에서 후처리(normalize)로 보정

---

## 11. 주의사항

* 본 예시는 **연수/수업 데모용**입니다.
* 실제 학생 데이터 저장 시 학교·기관의 **개인정보 처리 지침**을 반드시 준수하세요.

```

---

원하시면 다음도 바로 만들어드릴 수 있습니다.

**Q1**  

이 README를 “연수 참가자 배포용(1페이지 요약)” 버전으로 더 줄여볼까?  


  
**Q2**  

Supabase 테이블 생성용 SQL 예시(`jsonb` 기반)도 README 하단에 추가할까?  


  
**Q3**  

교사용/학생용 버튼을 분리하는 UI 버전도 연수 확장 예제로 만들어볼까?
```
