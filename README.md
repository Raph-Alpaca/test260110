# Streamlit 서술형 평가 앱 (Step 1-2 + Step 2 GPT 피드백)

이 프로젝트는 **Streamlit 단일 파일(app.py)** 로 동작하는 연수용 예시입니다.

- **Step 1-2**: 학번 입력 + 서술형 3문항 답안 제출(검증 포함, `st.form` 사용)
- **Step 2**: OpenAI GPT API를 이용해 문항별 **O/X 판정 + 200자 이내 피드백** 생성
- (추후) **Supabase 저장**을 위해 `st.session_state.gpt_payload` 형태로 데이터 묶음을 준비해 둠

---

## 1) 준비물

- Python 3.10 이상 권장
- VS Code (권장)
- 인터넷 연결(OpenAI API 호출 시)
- 필수 패키지
  - `streamlit`
  - `openai`

---

## 2) 폴더/파일 구조(권장)

프로젝트 폴더(예: `1/`)에 아래처럼 둡니다.

