# 📝 Streamlit 서술형 평가 앱 (with GPT-5 Feedback)

<div align="center">

![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?style=for-the-badge&logo=Streamlit&logoColor=white)
![OpenAI](https://img.shields.io/badge/OpenAI-412991?style=for-the-badge&logo=OpenAI&logoColor=white)
![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)

**교사 연수용 단일 파일(`app.py`) 서술형 평가 및 AI 피드백 시스템 예시입니다.**


</div>

---

## 🧐 프로젝트 개요

이 프로젝트는 학생들이 서술형 답안을 제출하면, OpenAI GPT API가 즉시 채점(O/X)하고 피드백을 제공하는 웹 애플리케이션입니다.

### 왜 이 도구가 필요한가요? (업무 흐름 비교)

AI와 데이터베이스를 도입했을 때 교사의 업무가 어떻게 변화하는지 시각적으로 비교해 보세요.

#### 1. To-Be: AI 도입 모델 (제안 방식)
교사는 평가 업무에서 해방되어 **대시보드 모니터링**에 집중할 수 있으며, 데이터는 자동으로 축적됩니다.

```mermaid
graph TD
    %% 노드 정의 (특수문자 포함 시 따옴표 권장)
    ST[👨‍🎓 학생]
    APP("📝 Streamlit Web App<br/>학생 평가 화면")
    GPT{🤖 OpenAI GPT API}
    DB[("🗄️ Supabase DB<br/>결과 데이터 저장")]
    TE[👩‍🏫 교사]
    DASH("📊 교사 대시보드<br/>데이터 확인 화면")

    %% 학생 흐름 (제출 및 피드백)
    ST -->|"1. 학번/답안 입력 및 제출"| APP
    APP -->|"2. 답안 전송 및 채점 요청"| GPT
    GPT -->|"3. O/X 및 피드백 반환"| APP
    APP -->|"4. 전체 데이터(Payload) 저장"| DB
    APP -.->|"5. 결과 화면 확인"| ST

    %% 교사 흐름 (조회)
    TE -->|"6. 대시보드 접속"| DASH
    DASH -->|"7. 데이터 조회 요청"| DB
    DB -.->|"8. 학생별 결과 데이터 반환"| DASH

    %% 스타일 정의
    style APP fill:#FF4B4B,stroke:#333,stroke-width:2px,color:white
    style GPT fill:#412991,stroke:#333,stroke-width:2px,color:white
    style DB fill:#3ECF8E,stroke:#333,stroke-width:2px,color:white
    style DASH fill:#FF9F1C,stroke:#333,stroke-width:2px,color:white
```
```mermaid
graph TD
    %% 노드 정의 (특수문자 포함 시 반드시 따옴표 사용)
    ST_OLD[👨‍🎓 학생]
    PAPER["📄 답안지/LMS<br/>(종이 또는 파일 제출)"]
    TE_OLD[👩‍🏫 교사]
    WORK("✍️ 수동 채점 및<br/>피드백 작성")

    %% 흐름 정의
    ST_OLD -->|"1. 답안 작성 및 제출"| PAPER
    PAPER -->|"2. 답안 수거"| TE_OLD
    
    %% 교사의 반복 업무 강조
    TE_OLD -->|"3. 일일이 읽고 O/X 판정"| WORK
    WORK -.->|"4. 학생별 피드백 수기 작성"| TE_OLD
    
    %% 피드백 전달 (지연됨을 암시)
    TE_OLD -->|"5. 결과 통보 (시간 소요)"| ST_OLD

    %% 스타일 정의
    style ST_OLD fill:#eee,stroke:#333,stroke-width:2px
    style PAPER fill:#fff,stroke:#333,stroke-dasharray: 5 5
    style TE_OLD fill:#FF9F1C,stroke:#333,stroke-width:4px
    style WORK fill:#ffcccc,stroke:#333,stroke-width:2px,color:red
```
