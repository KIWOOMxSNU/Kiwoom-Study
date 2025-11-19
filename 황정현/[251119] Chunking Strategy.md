# 📘 RAG Chunking Techniques 

> Retrieval-Augmented Generation(RAG)의 성능은 **chunking 품질**에 의해 결정됩니다.  
> 아래는 15가지 대표적인 chunking 기법을 실무 기준으로 정리한 내용입니다.

---

## 1️⃣ Line-by-Line Chunking (한 줄씩 분리)

### 개념
줄바꿈 단위로 chunk를 만든다.

### 언제 사용?
- 메시지 로그(chat)
- 인터뷰 대화
- 스크립트
- Slack, Messenger, Discord 같은 대화 데이터

### 장점
- 한 메시지가 독립 의미 → 검색 정확도 높음  
- Q&A 구조의 대화형 데이터에 최적

### 단점
- 문맥 단절로 LLM이 혼란스러울 수 있음  
- 짧은 문장이 많으면 할루시네이션 위험 증가

---

## 2️⃣ Fixed-Size Chunking (고정 크기)

### 개념
의미와 상관없이 일정 단어/문자 수로 자른다.

### 언제 사용?
- 구조가 없는 텍스트
- OCR 결과물
- 웹 크롤링 raw text

### 장점
- 구현 매우 쉬움  
- 균일한 크기 → 임베딩 품질 안정

### 단점
- 문장이 중간에서 끊김  
- 의미 단절 → 검색 품질 저하

> 실무에서는 거의 사용하지 않지만 불가피할 때 사용.

---

## 3️⃣ Sliding Window Chunking (슬라이딩 윈도우)

### 개념
Chunk끼리 **겹침(overlap)**을 주어 문맥을 유지한다.

예시:
- chunk size: 200 tokens  
- overlap: 50 tokens

### 언제 사용?
- 논문, 기술 문서 등 문맥이 길게 이어지는 경우  
- 의미 흐름이 끊어지면 안 될 때

### 장점
- 문맥 보존 최고  
- Retrieval 시 자연스러운 연결 제공

### 단점
- 중복 저장 증가  
- 비용 증가

---

## 4️⃣ Sentence-Based Chunking (문장 단위)

### 개념
문장 단위로 분리한다.

### 언제 사용?
- 정제된 글, 뉴스, 논문, 설명서

### 장점
- 문장 = 의미 단위 → 검색 효율 높음

### 단점
- 문장 짧을 경우 정보 부족  
(여러 문장을 묶어 chunk 만들기도 함)

---

## 5️⃣ Paragraph Chunking (문단 단위)

### 개념
문단(paragraph)을 기준으로 split한다.

### 언제 사용?
- 보고서, 블로그, 논문

### 장점
- 문단은 완성된 주제를 담음  
- Retrieval 품질 매우 좋음

### 단점
- 문단이 너무 긴 경우 → 토큰 초과 가능

---

## 6️⃣ Page-Based Chunking (페이지 단위)

### 개념
PDF 페이지 단위로 자른다.

### 언제 사용?
- 법률 문서  
- 계약서  
- 보고서  
- 페이지 reference가 필요한 문서

### 장점
- 페이지 구조 유지  
- Reference 관리 용이

### 단점
- 페이지가 너무 길면 토큰 초과 가능

---

## 7️⃣ Section / Heading-Based Chunking (섹션 기준)

### 개념
H1/H2/H3 또는 제목(Introduction 등)을 기준으로 분리한다.

### 언제 사용?
- 기술 문서  
- API 문서  
- Whitepaper  
- 책

### 장점
- 주제 단위로 잘 분리됨  
- 검색 정확도 매우 높음

---

## 8️⃣ Keyword-Based Chunking (키워드 기준)

### 개념
특정 키워드 등장 시 chunk를 분리한다.

예시: `Note:`, `Diagnosis:`, `Step 1`, `Important`

### 언제 사용?
- 의료 기록  
- 로그 파일  
- 단계별 절차 문서

### 장점
- 의미적 구조를 보존  
- 관련 정보가 자연스럽게 묶임

---

## 9️⃣ Entity-Based Chunking (개체 기반)

### 개념
NER(Named Entity Recognition)으로 인물/기관/제품 단위로 그룹화한다.

### 언제 사용?
- 뉴스  
- 법률 문서  
- 리뷰 분석

### 장점
- 특정 entity 기반 검색이 매우 정확  
- “Apple 관련 내용만” 등 질의에 최적

---

## 🔟 Token-Based Chunking (토큰 기준)

### 개념
LLM의 토큰 단위로 자르는 방식 (예: 200 tokens).

### 언제 사용?
- LLM의 context 길이 제한 관리  
- OpenAI API 기반 RAG에서 매우 중요

### 장점
- 토큰 제한 내에서 안정적 처리  
- 자동 관리 가능

### 단점
- 문장 중간에서 끊기면 의미 손상 가능

---

## 1️⃣1️⃣ Table Chunking (테이블 기반)

### 개념
테이블을 독립된 chunk로 분리.

### 언제 사용?
- 재무 보고서  
- 논문 데이터  
- 표가 많은 PDF

### 장점
- 표 기반 질의 가능  
- 숫자 질문에 정확히 대응 (예: “Q2 Revenue?”)

---

## 1️⃣2️⃣ Recursive Chunking (재귀적 척킹)

### 개념
큰 chunk를 → 문단 → 문장 → 단어 순으로 재귀적으로 분리.

### 언제 사용?
- 문단 길이가 불균형할 때  
- 토큰 초과를 방지해야 할 때

### 장점
- chunk 크기가 균일  
- 안정적 Retrieval

---

## 1️⃣3️⃣ Semantic Chunking (의미 기반 척킹)

### 개념
AI/임베딩 모델이 문장의 **의미**를 기준으로 묶는다.

### 언제 사용?
- 다양한 주제의 문서  
- FAQ  
- 고객센터 로그  
- 기술지원 Q&A

### 장점
- 의미 기반 검색  
- 사용자 질의 의도와 정확히 일치하는 chunk 반환

---

## 1️⃣4️⃣ Hierarchical Chunking (계층적 척킹)

### 개념
Chapter → Section → Paragraph 같은 계층 구조로 분리.

### 언제 사용?
- 구조가 큰 문서  
- 책, 법률 문서, 연구 논문

### 장점
- “개괄적 정보”와 “세부 정보” 검색 모두 가능

---

## 1️⃣5️⃣ Content-Type Aware Chunking (콘텐츠 유형별 척킹)

### 개념
텍스트, 목록, 표, 이미지 등 **유형별로 각각 chunk**로 분리.

### 언제 사용?
- PDF 보고서  
- 연구 논문  
- 이미지·테이블이 혼합된 문서

### 장점
- 검색 정확도가 매우 높음  
- “표만”, “그림 설명만” 등 요청 처리 가능

---

# ✔️ 핵심 요약

### ✔ RAG 성능 = Chunking 품질  
Retrieval의 80%는 chunk quality가 결정한다.

### ✔ 문서 유형과 질문 유형에 따라 최적 기법이 다르다
- 뉴스 → Entity Chunking  
- 논문 → Section 기반  
- 재무 보고서 → Table Chunking  
- FAQ → Semantic Chunking  
- 법률 문서 → Page / Section Chunking

### ✔ 반드시 테스트해야 한다!
- chunk가 너무 작으면 → 할루시네이션 발생  
- chunk가 너무 크면 → 검색 precision 감소  
- overlap 없음 → 문맥 단절  
- overlap 너무 큼 → 비용 증가  

---