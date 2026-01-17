# 📝 Streamlit 서술형 평가 앱 (with GPT-5 Feedback)

<div align="center">

![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?style=for-the-badge&logo=Streamlit&logoColor=white)
![OpenAI](https://img.shields.io/badge/OpenAI-412991?style=for-the-badge&logo=OpenAI&logoColor=white)
![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)

**교사 연수용 단일 파일(`app.py`) 서술형 평가 및 AI 피드백 시스템 예시입니다.**

[🚀 주요 기능] | [⚙️ 설치 가이드] | [📖 사용 흐름] | [🛠️ 문제 해결]
:---:|:---:|:---:|:---:

</div>

---

## 🧐 프로젝트 개요

이 프로젝트는 학생들이 서술형 답안을 제출하면, OpenAI GPT API가 즉시 채점(O/X)하고 피드백을 제공하는 간단한 웹 애플리케이션입니다. 추후 DB 연동을 고려하여 설계되었습니다.

```mermaid
graph LR
    A[👨‍🎓 학생] -->|1. 학번/답안 입력| B(📝 Streamlit Web App);
    B -->|2. 제출 버튼 클릭| B;
    B -->|3. 답안 데이터 전송| C{🤖 OpenAI GPT API};
    C -->|4. O/X 및 피드백 생성| B;
    B -->|5. 결과 화면 표시| A;

    style B fill:#FF4B4B,stroke:#333,stroke-width:2px,color:white
    style C fill:#412991,stroke:#333,stroke-width:2px,color:white
