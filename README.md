# Hydrate Ontology — NSCLC Article Ingestion System

## 프로젝트 개요

이 프로젝트는 **폐암(NSCLC)** 신약 개발을 위한 **Agentic AI** 시스템의 기초 단계인 **Hydrate Ontology 레벨**에서 작동하는 데이터 파이프라인의 입력 인터페이스입니다.

## 목적

이 시스템의 핵심 목적은:
- ✅ **판단(추천)이 아닌, 증거(Article)와 관측값(Observation/Result)의 맥락 보존**
- ✅ 논문, 임상 시험 요약 등 다양한 유명 article을 구조화된 형태로 입력
- ✅ EGFR/ALK 변이를 가진 NSCLC 환자군에 대한 치료 증거 수집
- ✅ 동의어 정규화 및 온톨로지 기반 데이터 구조화

## 주요 기능

### 1. Article 입력 (Hydrate)
다음 5가지 탭으로 구성된 입력 폼:

#### 📋 메타데이터
- Evidence Type (Paper, TrialSummary, Review, Preprint)
- Publication Date
- Title, DOI, Source URL
- Authors/Organization
- Journal/Venue
- Trial ID

#### 📝 원문/요약 입력
- Abstract 또는 Trial Summary 텍스트
- Methods/Notes (환자군, endpoint 정의 등)

#### 🧬 엔티티/코호트
- Cancer Type (NSCLC)
- Gene Focus (EGFR, ALK)
- Alteration (ex19del, L858R, ALK fusion 등)
- Patient Subgroup Label (자동 생성)
- Line of Therapy (1L, 2L, 3L+)
- Drug/Compound 정보
- Mechanism of Action (MoA)

#### 📊 결과 수치
- Metric (ORR, PFS, OS, IC50)
- Value & Unit
- Result Notes

#### 🔒 신뢰도/거버넌스
- Peer Reviewed 여부
- Confidence Level (High/Medium/Low)
- Limitations/Caveats
- Funding/Sponsor
- Conflict of Interest

### 2. JSON 생성
입력된 데이터를 구조화된 JSON 형식으로 자동 변환:
```json
{
  "hydrate_version": "1.0",
  "scope": { ... },
  "evidenceItem": { ... },
  "entities": { ... },
  "observation": { ... },
  "proposal": { "status": "Draft", "text": null }
}
```

## 사용 방법

### 온라인 접속
🌐 **GitHub Pages:** [https://myungdae.github.io/medicine/](https://myungdae.github.io/medicine/)

### 로컬 실행
```bash
# 저장소 클론
git clone https://github.com/myungdae/medicine.git
cd medicine

# 브라우저에서 index.html 열기
open index.html  # macOS
# 또는
start index.html  # Windows
# 또는
xdg-open index.html  # Linux
```

## 예시 사용 흐름

1. **"예시 채우기"** 버튼을 클릭하여 샘플 데이터 확인
2. 각 탭을 이동하며 필수 항목 입력
3. **"Hydrate JSON 생성"** 버튼 클릭
4. 우측 패널에서 생성된 JSON 확인
5. JSON을 복사하여 서버 API로 전송

## 데이터 파이프라인 통합

생성된 JSON은 다음 단계로 전달됩니다:
1. **(a) EvidenceItem 저장** - 원문 및 메타데이터 저장
2. **(b) Result 저장** - 관측값 및 수치 저장
3. **(c) SHACL 검증** - 데이터 무결성 검증
4. **(d) Activate 단계** - SWRL/스코어링을 통한 추론 활성화

## 핵심 원칙

### Hydrate 원칙
> "결론"을 넣지 말고, **출처/날짜/맥락을 먼저 확보하세요.**

### 동의어 정규화
EGFR/ALK, 변이 표기(ex19del vs del19)를 **"한 인스턴스"로 고정**하는 것이 hydrate의 핵심입니다.

### SHACL 게이트 관점
metric/value/unit + reportedIn(EvidenceItem)이 없으면 Result는 **"보류/반려" 처리**되어야 합니다.

## 기술 스택

- **Frontend:** Pure HTML/CSS/JavaScript (No dependencies)
- **Styling:** Custom CSS with dark theme
- **Validation:** Client-side form validation
- **Output:** JSON format

## 다음 단계

이 입력창은 **Hydrate Ontology 레벨**의 시작점입니다. 다음 단계에서는:

1. **백엔드 서버 구축** - JSON을 받아 데이터베이스에 저장
2. **온톨로지 통합** - RDF/OWL 기반 온톨로지 구조 구현
3. **SHACL 검증 엔진** - 데이터 품질 관리
4. **RAG 시스템** - 저장된 증거에서 컨텍스트 추출
5. **Activate 단계** - SWRL 기반 추론 및 추천 시스템

## 라이선스

MIT License

## 기여

이 프로젝트는 폐암 치료 연구를 위한 오픈 소스 프로젝트입니다. 기여를 환영합니다!

## 문의

프로젝트 관련 문의는 GitHub Issues를 통해 남겨주세요.

---

**Version:** 1.0  
**Last Updated:** 2026-01-08  
**MoE Expert:** Human Curator
