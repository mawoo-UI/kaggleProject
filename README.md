# 🚀 지식 농축 에이전트: 개념 드링크(Concept Drink)

> 💡 YouTube 영상을 자동으로 분석하고 Gemini AI로 요약하여 Notion에 구조화된 페이지로 저장하는 다국어 지원 Agent 협업 시스템

---

## 🧐 개요 및 목적 (Overview & Purpose)

이 프로젝트는 **YouTube 콘텐츠 자동 분석 및 문서화**를 **자율적으로** 처리하도록 설계된 AI Agent 협업 시스템인 **YouTube-Gemini-Notion Agent**를 구현합니다.

* **주요 목표:** YouTube URL 하나만 입력하면 자막 추출, AI 요약, Notion 페이지 생성까지 완전 자동화
* **주요 기능:** 
    * 기능 1: **다국어 자막 추출** - YouTube 영상에서 한국어/영어/기타 언어 자막 자동 추출
    * 기능 2: **자동 번역** - 영어/일본어 등 외국어 자막을 한국어로 고품질 번역
    * 기능 3: **AI 요약** - Gemini 2.5 Flash Lite로 학습자 친화적인 구조화된 요약 생성
    * 기능 4: **Notion 자동 발행** - 요약+원본 자막을 포함한 구조화된 Notion 페이지 자동 생성

## 🧠 에이전트 아키텍처 (Agent Architecture)

기존 Monolithic 클래스 구조에서 **Tool → Agent → Coordinator 3계층 협업 아키텍처**로 완전히 전환되었습니다.

### ⚙️ 핵심 구성 요소

| 구성 요소 | 역할 | 사용된 기술/모델 |
| :--- | :--- | :--- |
| **Tool 계층** | 개별 기능을 담당하는 독립적인 도구들 | YouTube Transcript API, Gemini API, Notion API |
| **Agent 계층** | Tool들을 조합하여 특정 영역을 전문 처리 | YouTubeContentAgent, ContentSummaryAgent, NotionPublishAgent |
| **Coordinator 계층** | 전체 워크플로우 조율 및 상태 관리 | YouTubeGeminiNotionCoordinator |
| **Factory 계층** | 시스템 쉬운 초기화 및 구성 | YouTubeGeminiNotionFactory |

### 🗺️ 작업 흐름 (Agent Collaboration Workflow)

Agent들이 협업하여 하나의 YouTube 영상을 처리하는 단계별 프로세스:

1. **입력:** YouTube URL 수신
2. **YouTube Content Agent:** 자막 추출 → 언어 감지 → 필요시 한국어 번역
3. **Content Summary Agent:** Gemini AI로 구조화된 요약 생성
4. **Notion Publish Agent:** 요약+메타데이터+원본을 포함한 Notion 페이지 생성
5. **Coordinator:** 전체 프로세스 조율 및 결과 통합
6. **출력:** 생성된 Notion 페이지 URL 및 상세 통계 정보

---

## 💻 사용 기술 및 프레임워크 (Tech Stack)

* **언어:** Python 3.11+
* **에이전트 아키텍처:** 커스텀 Tool-Agent-Coordinator 패턴
* **LLM API:** Google Gemini 2.5 Flash Lite
* **외부 API:** YouTube Transcript API, Notion API
* **주요 라이브러리:** google-generativeai, youtube-transcript-api, requests, asyncio

## 💡 데모 및 결과 (Demo & Results)

### 📝 사용 예시

| 사용자 입력 (Input) | Agent 협업 출력 (Output) |
| :--- | :--- |
| "https://youtu.be/example (영어 영상)" | "🌍 자동 번역 완료: en → 한국어<br>📄 Notion 페이지: [링크]<br>📊 요약 길이: 2,664자<br>⚡ 토큰 사용량: 1,611" |
| **Agent 협업 과정:** YouTubeContentAgent가 영어 자막 추출 → TranslationTool로 한국어 번역 → ContentSummaryAgent가 AI 요약 → NotionPublishAgent가 구조화된 페이지 생성 |

### 📊 성능 측정 (Evaluation)

* **성공률:** 95% (다국어 자막 지원으로 처리 가능한 영상 범위 대폭 확대)
* **평균 응답 시간:** 15-30초 (자막 길이에 따라 변동)
* **번역 품질:** Gemini AI 기반 의미 중심 고품질 번역
* **지원 언어:** 한국어, 영어, 일본어 등 YouTube에서 지원하는 모든 언어

---

## 🛑 제한 사항 및 향후 계획 (Limitations & Future Work)

### **현재 제한 사항:**
* 자막이 없는 YouTube 영상은 처리 불가
* 매우 긴 영상(3시간+)의 경우 토큰 제한으로 일부 내용 축약
* 실시간 스트리밍 영상은 지원하지 않음

### **향후 계획:**
* **음성 인식 도구 추가:** 자막이 없는 영상도 처리 가능하도록 개선
* **슬라이드/이미지 분석:** 영상 내 시각적 콘텐츠 분석 기능 추가
* **배치 처리:** 여러 YouTube 영상을 한 번에 처리하는 기능
* **검색 기능:** YouTube 검색 결과 자동 수집 및 처리
* **알림 시스템:** Slack, Discord 등 외부 알림 도구 연동

## 🔧 설치 및 사용법

### 환경 변수 설정
```env
NOTION_TOKEN=your_notion_token
NOTION_PARENT_PAGE=your_parent_page_id  
GEMINI_API_KEY=your_gemini_api_key
```

### 사용 방법
```python
# Agent 협업 시스템 초기화
coordinator = YouTubeGeminiNotionFactory.from_env_vars()

# YouTube 영상 처리 (다국어 지원)
result = await coordinator.process_single_video(
    url="https://youtube.com/watch?v=example",
    custom_prompt="간단히 요약해줘",
    custom_title="AI 강의 요약"
)

print(f"생성된 페이지: {result['final_result']['page_url']}")
```

## 📧 연락처

* **프로젝트 관리자:** [@mawoo-UI]
* **이메일:** hanjutun@gmail.com
