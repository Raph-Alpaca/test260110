📝 Streamlit 서술형 평가 앱 (with GPT-4o Feedback)<div align="center">교사 연수용 단일 파일(app.py) 서술형 평가 및 AI 피드백 시스템 예시입니다.학생이 답안을 제출하면 AI가 즉시 채점하고 피드백을 제공합니다.</div>🧐 프로젝트 개요이 프로젝트는 기존의 수동 채점 방식에서 벗어나, OpenAI GPT API를 활용해 실시간으로 학생들에게 피드백을 제공하는 웹 애플리케이션입니다.1. To-Be: AI 기반 자동화 모델 (확장 아키텍처)아래 다이어그램은 Supabase DB 및 교사 대시보드까지 확장했을 때의 이상적인 전체 아키텍처를 보여줍니다.graph TD
    %% 노드 정의
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
2. As-Is: 기존 수동 평가 모델교사가 채점, 기록, 피드백 작성의 모든 과정을 수동으로 처리하여 **업무 병목(Bottleneck)**이 발생하는 구조입니다.graph TD
    %% 노드 정의
    ST_OLD[👨‍🎓 학생]
    PAPER[📄 답안지/LMS<br/>(종이 또는 파일 제출)]
    TE_OLD[👩‍🏫 교사]
    WORK(✍️ 수동 채점 및<br/>피드백 작성)

    %% 흐름 정의
    ST_OLD -->|"1. 답안 작성 및 제출"| PAPER
    PAPER -->|"2. 답안 수거"| TE_OLD
    
    %% 교사의 반복 업무 강조
    TE_OLD -->|"3. 일일이 읽고 O/X 판정"| WORK
    WORK -.->|"4. 학생별 피드백 수기 작성"| TE_OLD
    
    %% 피드백 전달 (지연됨을 암시)
    TE_OLD -->|"5. 결과 통보 (시간 소요)"| ST_OLD

    %% 스타일 정의 (회색조 사용으로 대비 효과)
    style ST_OLD fill:#eee,stroke:#333,stroke-width:2px
    style PAPER fill:#fff,stroke:#333,stroke-dasharray: 5 5
    style TE_OLD fill:#FF9F1C,stroke:#333,stroke-width:4px
    style WORK fill:#ffcccc,stroke:#333,stroke-width:2px,color:red
🚀 시작하기 (Getting Started)1. 환경 설정 (Prerequisites)이 프로젝트를 실행하기 위해서는 Python 3.10 이상과 OpenAI API Key가 필요합니다.# 가상환경 생성 (선택 사항)
python -m venv venv
source venv/bin/activate  # Mac/Linux
venv\Scripts\activate     # Windows
2. 라이브러리 설치pip install streamlit openai
# Supabase 연동 시 추가 설치: pip install supabase
3. API Key 설정프로젝트 루트 경로에 .streamlit/secrets.toml 파일을 생성하거나 환경 변수를 설정하세요..streamlit/secrets.toml 예시:[openai]
api_key = "sk-proj-..."
4. 앱 실행streamlit run app.py
📂 파일 구조📦 streamlit-essay-feedback
 ┣ 📂 .streamlit
 ┃ ┗ 📜 secrets.toml    # API Key 저장 (Git 업로드 금지)
 ┣ 📜 app.py            # 메인 애플리케이션 코드
 ┣ 📜 requirements.txt  # 의존성 패키지 목록
 ┗ 📜 README.md         # 프로젝트 문서
