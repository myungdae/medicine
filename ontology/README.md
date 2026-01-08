# NSCLC Ontology Files

## 개요

이 디렉토리에는 NSCLC (Non-Small Cell Lung Cancer) Agentic AI 시스템의 온톨로지 파일들이 포함되어 있습니다.

## 파일 구성

### 1. `01_nsclc_seed.ttl` - 기본 온톨로지
**목적**: 핵심 클래스, 프로퍼티, 시드 인스턴스 정의

**주요 내용**:
- **Classes**: CancerType, PatientSubgroup, Gene, Alteration, Compound, EvidenceItem, Result 등
- **Object Properties**: hasCancerType, hasAlteration, targets, reportedIn 등
- **Datatype Properties**: metric, value, unit, evidenceType, doi, abstractText 등
- **Seed Instances**: 
  - NSCLC (Cancer Type)
  - EGFR, ALK (Genes)
  - EGFR_ex19del, EGFR_L858R, ALK_fusion (Alterations)
  - NSCLC_EGFR_ex19del, NSCLC_ALK_fusion 등 (Patient Subgroups)

**개선 사항 (v2.0)**:
- ✅ Hydrate Ontology UI와 완벽히 매칭되도록 프로퍼티 추가
- ✅ OWL 2 표준 준수 강화
- ✅ 모든 클래스/프로퍼티에 rdfs:comment 주석 추가
- ✅ Evidence governance 프로퍼티 추가 (peerReviewed, confidence, limitations, funding, conflictOfInterest)
- ✅ Compound 메타데이터 보강 (compoundName, lineOfTherapy)

### 2. `02_nsclc_shapes.ttl` - SHACL 검증 규칙
**목적**: 데이터 품질 검증 ("SHACL Gate")

**주요 Shape**:
1. **ResultShape**: Result 인스턴스 검증
   - aboutCancerType, aboutDrug, reportedIn 필수
   - metric: ORR/PFS/OS/IC50/DOR/DCR 중 하나
   - value: decimal, >= 0
   - unit: 문자열 필수

2. **EvidenceItemShape**: Evidence 메타데이터 검증
   - evidenceType: Paper/TrialSummary/Review/Preprint
   - sourceDate: 필수
   - abstractText: 최소 10자 이상
   - confidence: High/Medium/Low

3. **ClinicalTrialShape**: Trial ID 형식 검증
   - trialId 패턴: NCT/EUCTR/JPRN/ChiCTR/CTRI + 숫자

4. **CompoundShape**: Drug 메타데이터 검증
   - compoundName 필수 (최소 2자)
   - 최소 1개 이상의 Gene target

5. **PatientSubgroupShape**: 환자 코호트 정의 검증
   - hasCancerType, hasAlteration 필수

6. **AlterationShape**: 변이-유전자 링크 검증
   - inGene 필수

7. **ResultUnitConsistencyShape**: Metric-Unit 일치성 검증 (SPARQL)
   - ORR → %
   - PFS/OS → months
   - IC50 → nM/uM

**개선 사항 (v2.0)**:
- ✅ 상세한 오류 메시지 추가
- ✅ SPARQL-based advanced validation 추가
- ✅ Clinical trial ID 정규식 패턴 검증
- ✅ 모든 shape에 rdfs:label, rdfs:comment 추가

### 3. `03_nsclc_rules_swrl.ttl` - SWRL 추론 규칙 (RDF)
**목적**: 자동 추론 및 추천 생성

**12개 규칙**:
1. **Rule1_EGFR_Recommendation**: EGFR 환자 → EGFR 약물 추천
2. **Rule2_ALK_Recommendation**: ALK 환자 → ALK 약물 추천
3. **Rule3_ORR_HighPriority**: ORR > 50% → highPriority 플래그
4. **Rule4_PFS_HighPriority**: PFS > 12 months → highPriority
5. **Rule5_OS_HighPriority**: OS > 24 months → highPriority
6. **Rule6_Trial_Followup**: TrialSummary → trial follow-up 제안
7. **Rule7_ORR_Unit_Check**: ORR unit != % → needsReview
8. **Rule8_PFS_Unit_Check**: PFS unit != months → needsReview
9. **Rule9_OS_Unit_Check**: OS unit != months → needsReview
10. **Rule10_EGFR_Subgroup_DrugMismatch**: EGFR 환자 + non-EGFR 약물 → needsReview
11. **Rule11_ALK_Subgroup_DrugMismatch**: ALK 환자 + non-ALK 약물 → needsReview
12. **Rule12_TopCandidate**: 2+ Paper 증거 + highPriority → topCandidate

**개선 사항 (v2.0)**:
- ✅ OS (Overall Survival) 관련 규칙 추가
- ✅ 모든 규칙에 rdfs:label, rdfs:comment 추가
- ✅ 더 명확한 변수명 사용

### 4. `03_nsclc_rules_text.txt` - SWRL 규칙 (텍스트)
**목적**: TopBraid Composer SWRL 탭에 복사 가능한 형식

위의 12개 규칙을 human-readable 텍스트 형식으로 제공합니다.

**사용법**:
```
PatientSubgroup(?s) ^ hasAlteration(?s, ?a) ^ inGene(?a, EGFR) ^ Compound(?d) ^ targets(?d, EGFR)
-> recommendedForSubgroup(?d, ?s)
```

### 5. `04_agentic_decision_flow.ttl` - 의사결정 워크플로우
**목적**: Proposal → Request → Decision → Execution 프로세스 정의

**주요 내용**:
- **ActionProposal**: AI 또는 큐레이터가 생성한 제안
  - proposesAction, proposalStatus, proposalRationale
  - triggeredByRule: 어떤 SWRL 규칙이 트리거했는지
  
- **ApprovalRequest**: 승인 요청
  - requestedTo, requestPriority
  
- **ApprovalDecision**: 승인/거부 결정
  - decision: Approve/Reject/Hold
  - decisionReason, decidedBy
  
- **Execution**: 실행 기록
  - executionStatus, producedResult

**워크플로우 상태**:
```
Draft → Requested → Approved/Rejected/Hold → Executed
```

**MoE (Mixture of Experts) 통합**:
- expertType: Human Curator / AI Reasoner / Hybrid
- confidenceScore: 0.0-1.0
- requiresHumanReview: boolean

**개선 사항 (v2.0)**:
- ✅ Audit trail 프로퍼티 추가
- ✅ Evidence quality gating 메커니즘
- ✅ Notification/alerting 프로퍼티
- ✅ 예제 템플릿 인스턴스 추가

## 사용 가이드

### TopBraid Composer에서 사용하기

1. **Import 순서** (중요!):
   ```
   1. 01_nsclc_seed.ttl         (기본 온톨로지)
   2. 02_nsclc_shapes.ttl       (SHACL 검증)
   3. 03_nsclc_rules_swrl.ttl   (SWRL 규칙)
   4. 04_agentic_decision_flow.ttl (워크플로우)
   ```

2. **SHACL 검증 실행**:
   - Import 후 "SHACL Validate" 실행
   - 데이터 입력 전에 검증하여 품질 확보

3. **SWRL 규칙 적용**:
   - SWRL 탭에서 규칙 확인
   - 또는 `03_nsclc_rules_text.txt` 내용을 복사하여 추가
   - Reasoner 실행 → highPriority/topCandidate 플래그 생성

4. **테스트 데이터 추가**:
   - EvidenceItem 인스턴스 생성
   - Result 인스턴스 생성 (reportedIn으로 연결)
   - 규칙 실행 후 추론 결과 확인

### Apache Jena Fuseki에서 사용하기

```bash
# Fuseki 서버 시작
fuseki-server --mem /nsclc

# 온톨로지 로드 (REST API)
curl -X POST http://localhost:3030/nsclc/data \
  -H "Content-Type: text/turtle" \
  --data-binary @01_nsclc_seed.ttl

curl -X POST http://localhost:3030/nsclc/data \
  -H "Content-Type: text/turtle" \
  --data-binary @02_nsclc_shapes.ttl
```

### Python (rdflib)에서 사용하기

```python
from rdflib import Graph, Namespace

# 온톨로지 로드
g = Graph()
g.parse("01_nsclc_seed.ttl", format="turtle")
g.parse("02_nsclc_shapes.ttl", format="turtle")

# SPARQL 쿼리 예시
nsclc = Namespace("http://example.org/nsclc#")
query = """
PREFIX : <http://example.org/nsclc#>
SELECT ?drug ?subgroup
WHERE {
  ?drug :recommendedForSubgroup ?subgroup .
}
"""
results = g.query(query)
for row in results:
    print(f"Drug: {row.drug}, Subgroup: {row.subgroup}")
```

### SHACL 검증 (Python)

```python
from pyshacl import validate

data_graph = Graph()
data_graph.parse("my_data.ttl", format="turtle")

shapes_graph = Graph()
shapes_graph.parse("02_nsclc_shapes.ttl", format="turtle")

conforms, results_graph, results_text = validate(
    data_graph,
    shacl_graph=shapes_graph,
    inference="rdfs",
    abort_on_first=False,
)

print(f"Validation Result: {conforms}")
print(results_text)
```

## 데이터 모델 개념도

```
┌─────────────────┐
│  CancerType     │──┐
│  (NSCLC)        │  │
└─────────────────┘  │
                     │ hasCancerType
┌─────────────────┐  │  ┌──────────────────┐
│  Gene           │──┼─→│ PatientSubgroup  │
│  (EGFR, ALK)    │  │  │ (NSCLC+EGFR...)  │
└─────────────────┘  │  └──────────────────┘
       ↑ inGene      │         ↓ aboutSubgroup
┌─────────────────┐  │  ┌──────────────────┐
│  Alteration     │──┘  │     Result       │
│  (ex19del...)   │     │  (ORR: 62%)      │
└─────────────────┘     └──────────────────┘
                               ↑ reportedIn
       ┌──────────────────────┘
┌──────────────────┐
│  EvidenceItem    │
│  (Paper/Trial)   │
└──────────────────┘
       ↓ hasTrial
┌──────────────────┐
│  ClinicalTrial   │
│  (NCT01234567)   │
└──────────────────┘
```

## 확장 가능성

### 추가 가능한 암 종류
- SCLC (Small Cell Lung Cancer)
- Breast Cancer (HER2, BRCA1/2)
- Colorectal Cancer (KRAS, BRAF)

### 추가 가능한 변이
- EGFR T790M (resistance mutation)
- KRAS G12C
- MET amplification

### 추가 가능한 메트릭
- DOR (Duration of Response)
- DCR (Disease Control Rate)
- AE (Adverse Events)
- QoL (Quality of Life)

## 버전 히스토리

- **v2.0** (2026-01-08): 
  - Hydrate Ontology UI 완벽 매칭
  - SHACL 검증 강화
  - SWRL 규칙 확장 (12개)
  - Agentic workflow 추가
  - 주석 및 문서화 개선

- **v1.0** (Initial): 
  - 기본 온톨로지 구조
  - 10개 SWRL 규칙
  - 기본 SHACL shapes

## 라이선스

MIT License

## 참고 문헌

- [RDF 1.1 Primer](https://www.w3.org/TR/rdf11-primer/)
- [OWL 2 Web Ontology Language](https://www.w3.org/TR/owl2-overview/)
- [SHACL - Shapes Constraint Language](https://www.w3.org/TR/shacl/)
- [SWRL - Semantic Web Rule Language](https://www.w3.org/Submission/SWRL/)
- [TopBraid Composer](https://www.topquadrant.com/products/topbraid-composer/)
- [Apache Jena](https://jena.apache.org/)
