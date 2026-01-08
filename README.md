# NSCLC Agentic AI Platform

## 프로젝트 개요

**폐암(NSCLC)** 신약 개발을 위한 **Agentic AI 시스템**의 통합 플랫폼입니다. Palantir 스타일의 사이드바 네비게이션을 통해 전체 워크플로우를 관리할 수 있습니다.

## 🎯 핵심 목표

- ✅ **Evidence-Based Medicine**: 증거 기반 의약 개발 지원
- ✅ **Ontology-Driven**: RDF/OWL 온톨로지 기반 지식 관리
- ✅ **AI-Powered Reasoning**: SWRL 기반 추론 엔진
- ✅ **Quality Assurance**: SHACL 검증을 통한 데이터 품질 보증
- ✅ **Clinical Decision Support**: 환자별 맞춤 치료 추천

## 🌐 접속 정보

### **공개 URL (GitHub Pages)**
```
https://myungdae.github.io/medicine/
```

### **GitHub 저장소**
```
https://github.com/myungdae/medicine
```

## 📊 시스템 구성

### Core Modules

#### 1. 📊 Dashboard
- 시스템 전체 현황 대시보드
- 실시간 통계 (Evidence Items, Patient Cases, Active Drugs)
- 최근 활동 타임라인
- 시스템 상태 모니터링

#### 2. 💧 Hydrate Ontology
**증거 수집 및 구조화 모듈** (기존 입력창)

**목적**: 판단(추천)이 아닌, 증거(Article)와 관측값(Observation/Result)의 맥락 보존

**기능**:
- 5개 탭 인터페이스
  - 📋 메타데이터 (Evidence Type, Publication Date, DOI 등)
  - 📝 원문/요약 입력 (Abstract, Trial Summary)
  - 🧬 엔티티/코호트 (Gene, Alteration, Drug)
  - 📊 결과 수치 (ORR, PFS, OS, IC50)
  - 🔒 신뢰도/거버넌스 (Peer Review, Confidence Level)
- 구조화된 JSON 생성
- 데이터 파이프라인 연동 준비

**지원 데이터**:
- Evidence Types: Paper, TrialSummary, Review, Preprint
- Cancer Focus: NSCLC
- Genes: EGFR, ALK
- Alterations: EGFR exon19 del, EGFR L858R, ALK fusion

#### 3. 📋 Patient Cases
**임상 사례 관리 모듈**

환자별 프로필, 치료 추천, 근거 추적을 관리합니다.

**예시 케이스**:
- Case #037: NSCLC, EGFR exon19 del, 1L → Drug_A 추천
- Case #036: NSCLC, ALK fusion, 2L → Alectinib 추천

#### 4. 🧠 Reasoning Engine
**AI 기반 추론 엔진** (개발 예정)

SWRL (Semantic Web Rule Language) 기반으로:
- 온톨로지 데이터 + 환자 프로필 → 치료 추천
- 규칙 편집, 추론 로그, 신뢰도 스코어링

### Knowledge Base

#### 5. 📚 Evidence Library
**증거 아이템 저장소** (개발 예정)

Hydrate Ontology를 통해 수집된 모든 증거를 검색/관리:
- 고급 검색 및 필터링
- RAG (Retrieval-Augmented Generation) 통합
- 증거 품질 메트릭

#### 6. 🔬 Ontology Browser
**온톨로지 탐색기** (개발 예정)

RDF/OWL 기반 온톨로지 시각화:
- 그래프 기반 관계 시각화
- SPARQL 쿼리 인터페이스
- 엔티티 간 관계 탐색

#### 7. ✓ SHACL Validation
**데이터 품질 검증** (개발 예정)

SHACL (Shapes Constraint Language) 기반:
- 필수 필드 검증
- 데이터 타입 체크
- 관계 무결성 검증
- 자동 수정 제안

### Analysis

#### 8. 💊 Drug Recommendations
**치료 추천 시스템** (개발 예정)

환자별 맞춤 치료 옵션:
- 비교 분석 (Comparative Analysis)
- 위험-이익 평가 (Risk-Benefit Assessment)
- 대안 옵션 제시

#### 9. ⚙️ Pipeline Monitor
**파이프라인 모니터링** (개발 예정)

실시간 데이터 파이프라인 추적:
- Hydrate → SHACL Validation → Activate → Recommend
- 실시간 로그 및 성능 메트릭
- 오류 추적 및 알림

## 🚀 데이터 파이프라인

```
┌─────────────────┐
│  Hydrate        │  ← 증거 수집 (Article Ingestion)
│  Ontology       │
└────────┬────────┘
         ↓
┌─────────────────┐
│  SHACL          │  ← 데이터 검증 (Quality Gate)
│  Validation     │
└────────┬────────┘
         ↓
┌─────────────────┐
│  Evidence       │  ← 지식 저장 (Knowledge Repository)
│  Library        │
└────────┬────────┘
         ↓
┌─────────────────┐
│  Reasoning      │  ← AI 추론 (SWRL-based Inference)
│  Engine         │
└────────┬────────┘
         ↓
┌─────────────────┐
│  Drug           │  ← 치료 추천 (Clinical Decision Support)
│  Recommendations│
└─────────────────┘
```

## 💡 사용 방법

### 1. Dashboard 접속
- 전체 시스템 현황 확인
- 주요 메트릭 모니터링
- Quick Actions 사용

### 2. Hydrate Ontology로 증거 입력
1. 사이드바에서 "Hydrate Ontology" 클릭
2. "예시 채우기" 버튼으로 샘플 데이터 확인
3. 5개 탭을 이동하며 데이터 입력
   - 메타데이터, 원문, 엔티티, 결과 수치, 거버넌스
4. "Hydrate JSON 생성" 클릭
5. 우측 패널에서 JSON 확인 및 복사

### 3. Patient Cases 관리
1. "Patient Cases" 페이지에서 환자 사례 확인
2. 각 케이스의 프로필, 추천, 근거 검토
3. 새로운 케이스 생성 또는 기존 케이스 업데이트

### 4. 사이드바 접기/펼치기
- 좌측 하단의 ☰ 버튼으로 사이드바 토글
- 모바일/태블릿에서는 자동으로 접힘

## 🏗️ 기술 스택

### Frontend
- **HTML/CSS/JavaScript** (No dependencies, Pure vanilla)
- **Palantir-inspired Design**: 다크 테마, 계층적 네비게이션
- **Responsive Design**: 모바일/태블릿/데스크톱 지원

### Data Format
- **JSON**: 구조화된 데이터 교환 포맷
- **RDF/OWL** (예정): 온톨로지 표현
- **SHACL** (예정): 데이터 제약 조건

### AI/ML Components (예정)
- **SWRL**: 규칙 기반 추론
- **RAG**: 증거 검색 및 생성
- **Semantic Web**: 지식 그래프 구축

## 📁 프로젝트 구조

```
medicine/
├── index.html          # 메인 애플리케이션 (SPA)
├── README.md           # 프로젝트 문서
└── (향후 추가 예정)
    ├── /api            # 백엔드 API
    ├── /ontology       # RDF/OWL 온톨로지 파일
    ├── /shacl          # SHACL 검증 규칙
    └── /data           # 샘플 데이터
```

## 🎨 UI/UX 특징

### Palantir-Style Design
- **다크 테마**: 의료 전문가용 집중 환경
- **계층적 네비게이션**: 명확한 정보 구조
- **실시간 피드백**: 상태 표시, 검증 메시지
- **접근성**: 키보드 네비게이션, 고대비 색상

### 주요 컴포넌트
- **Collapsible Sidebar**: 공간 효율적인 네비게이션
- **Multi-tab Forms**: 복잡한 데이터 입력 단순화
- **Real-time Validation**: 즉각적인 오류 피드백
- **JSON Preview**: 실시간 데이터 구조 확인
- **Timeline**: 활동 이력 시각화
- **Stat Cards**: 핵심 메트릭 강조

## 🔄 다음 단계 (Roadmap)

### Phase 1: Foundation (현재)
- ✅ UI/UX 레이아웃 구축
- ✅ Hydrate Ontology 입력 폼
- ✅ 네비게이션 시스템
- ✅ Dashboard 기본 구조

### Phase 2: Backend Integration (예정)
- 🔲 REST API 서버 구축
- 🔲 PostgreSQL 데이터베이스
- 🔲 RDF/OWL 온톨로지 저장소 (Jena Fuseki / GraphDB)
- 🔲 SHACL 검증 엔진

### Phase 3: AI/ML Features (예정)
- 🔲 SWRL 추론 엔진 구현
- 🔲 RAG 시스템 (벡터 DB + LLM)
- 🔲 자동 증거 추출 (논문 PDF 파싱)
- 🔲 치료 추천 알고리즘

### Phase 4: Production (예정)
- 🔲 사용자 인증/권한 관리
- 🔲 감사 로그 (Audit Trail)
- 🔲 규제 준수 (HIPAA, GDPR)
- 🔲 성능 최적화 및 스케일링

## 🧪 핵심 원칙

### 1. Hydrate 원칙
> "결론"을 넣지 말고, **출처/날짜/맥락을 먼저 확보하세요.**

### 2. 동의어 정규화
> EGFR/ALK, 변이 표기(ex19del vs del19)를 **"한 인스턴스"로 고정**

### 3. SHACL 게이트
> metric/value/unit + reportedIn(EvidenceItem)이 없으면 **"보류/반려" 처리**

### 4. 증거 기반 추천
> 모든 추천은 **명시적인 증거**와 **신뢰도 점수**를 포함

## 📚 참고 자료

### 관련 기술
- [RDF 1.1](https://www.w3.org/TR/rdf11-primer/)
- [OWL 2](https://www.w3.org/TR/owl2-overview/)
- [SHACL](https://www.w3.org/TR/shacl/)
- [SWRL](https://www.w3.org/Submission/SWRL/)
- [SPARQL 1.1](https://www.w3.org/TR/sparql11-overview/)

### 임상 데이터베이스
- [ClinicalTrials.gov](https://clinicaltrials.gov/)
- [PubMed](https://pubmed.ncbi.nlm.nih.gov/)
- [OncoKB](https://www.oncokb.org/)
- [cBioPortal](https://www.cbioportal.org/)

## 🤝 기여

이 프로젝트는 폐암 치료 연구를 위한 오픈 소스 프로젝트입니다.

### 기여 방법
1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 라이선스

MIT License

## 📞 문의

프로젝트 관련 문의는 GitHub Issues를 통해 남겨주세요.

---

**Version:** 2.0  
**Last Updated:** 2026-01-08  
**Status:** Active Development  
**Architecture:** Palantir-inspired Agentic AI Platform
