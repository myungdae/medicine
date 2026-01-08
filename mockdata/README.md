# Mock Data for NSCLC Agentic AI Platform

## 개요

이 디렉토리에는 프론트엔드 개발 및 테스트를 위한 Mock 데이터가 포함되어 있습니다. 실제 서버가 구축되기 전에 UI를 완전히 작동시키고 데이터 플로우를 시뮬레이션할 수 있습니다.

## 파일 구성

### 1. `evidence-items.json` (8개)
**목적**: Hydrate Ontology에서 수집된 증거 아이템

**내용**:
- 실제 임상 시험 및 논문 데이터 기반
- FLAURA, AURA3, ALEX, ALTA-1L 등 주요 trials 포함
- Evidence types: Paper, TrialSummary, Review, Preprint

**주요 필드**:
```json
{
  "id": "EVI001",
  "type": "TrialSummary",
  "title": "...",
  "publicationDate": "2017-01-19",
  "doi": "10.1056/NEJMoa1612674",
  "trialId": "NCT02151981",
  "abstractText": "...",
  "peerReviewed": "true",
  "confidence": "High",
  "limitations": "...",
  "funding": "AstraZeneca",
  "conflictOfInterest": "true"
}
```

**샘플**:
1. **EVI001**: Osimertinib in T790M+ NSCLC (AURA3)
2. **EVI002**: First-line Osimertinib (FLAURA) ⭐
3. **EVI003**: Alectinib vs Crizotinib (ALEX) ⭐
4. **EVI004**: Lorlatinib in brain metastases
5. **EVI005**: Afatinib vs Gefitinib (LUX-Lung 7)
6. **EVI006**: EGFR targeting review
7. **EVI007**: Investigational drug (preprint) - Low confidence
8. **EVI008**: Brigatinib vs Crizotinib (ALTA-1L)

### 2. `results.json` (15개)
**목적**: Evidence items에서 보고된 임상 결과

**내용**:
- ORR, PFS, OS, IC50 등 다양한 metrics
- EGFR (ex19del, L858R) 및 ALK fusion 변이
- 1L, 2L, 3L+ 치료 라인

**주요 필드**:
```json
{
  "id": "RES001",
  "evidenceId": "EVI001",
  "cancerType": "NSCLC",
  "gene": "EGFR",
  "alteration": "EGFR_ex19del",
  "subgroupLabel": "NSCLC + EGFR exon19 del",
  "lineOfTherapy": "1L",
  "drugName": "Osimertinib",
  "moa": "TKI",
  "metric": "PFS",
  "value": 18.9,
  "unit": "months",
  "resultNote": "..."
}
```

**Metric 분포**:
- ORR: 8건
- PFS: 9건
- OS: 0건 (향후 추가 가능)
- IC50: 0건

**Drug-Metric 하이라이트**:
| Drug | 1L ORR | 1L PFS | 2L ORR | 2L PFS |
|------|--------|--------|--------|--------|
| Osimertinib (EGFR) | 80% | 18.9m | 71% | 10.1m |
| Alectinib (ALK) | 82.9% | 34.8m | - | - |
| Brigatinib (ALK) | 71% | 24.0m | - | - |
| Afatinib (EGFR) | 70% | 11.0m | - | - |

### 3. `patient-cases.json` (5개)
**목적**: 환자 사례 및 치료 추천

**내용**:
- Active, Draft, Rejected, Executed 상태
- 각 케이스에 profile, recommendation, evidence 포함
- 실제 임상 의사결정 프로세스 시뮬레이션

**주요 필드**:
```json
{
  "id": "CASE037",
  "status": "Active",
  "profile": {
    "cancerType": "NSCLC",
    "gene": "EGFR",
    "alteration": "EGFR_ex19del",
    "lineOfTherapy": "1L",
    "treatmentNaive": true,
    "age": 62,
    "sex": "Female",
    "performanceStatus": "ECOG 1"
  },
  "recommendation": {
    "drug": "Osimertinib",
    "rationale": "...",
    "confidence": 0.94,
    "evidence": [...],
    "alternatives": [...]
  },
  "proposalStatus": "Approved",
  "decidedBy": "Dr. Kim (Oncologist)"
}
```

**케이스 시나리오**:
1. **CASE037** (Active): EGFR ex19del, 1L, Osimertinib 추천 → Approved
2. **CASE036** (Active): ALK fusion, 2L (post-crizotinib), Alectinib 추천 → Approved
3. **CASE035** (Draft): EGFR L858R, 1L, Osimertinib 고려 → Pending review
4. **CASE034** (Active): ALK fusion, 3L+, Lorlatinib 추천 → Executed
5. **CASE033** (Rejected): EGFR ex19del, 2L, ECOG 3, BSC 권장 → Rejected

### 4. `drugs.json` (9개)
**목적**: 약물 데이터베이스

**내용**:
- FDA/EMA 승인 정보
- 임상 시험 연결
- Efficacy metrics 요약
- Confidence score, priority flags

**주요 필드**:
```json
{
  "id": "DRUG001",
  "name": "Osimertinib",
  "brandName": "Tagrisso",
  "moa": "TKI",
  "targets": ["EGFR"],
  "approvals": [...],
  "clinicalTrials": [...],
  "efficacyMetrics": {
    "firstLine": {
      "ORR": 80,
      "PFS": 18.9,
      "OS": 38.6
    }
  },
  "recommendedFor": ["NSCLC_EGFR_ex19del", "NSCLC_EGFR_L858R"],
  "highPriority": true,
  "topCandidate": true,
  "confidenceScore": 0.95
}
```

**약물 리스트**:
1. **Osimertinib** (3rd-gen EGFR) - Top candidate ⭐
2. **Alectinib** (2nd-gen ALK) - Top candidate ⭐
3. **Lorlatinib** (3rd-gen ALK) - High priority
4. **Brigatinib** (2nd-gen ALK) - Top candidate ⭐
5. **Afatinib** (Pan-ErbB) - Standard option
6. **Gefitinib** (1st-gen EGFR) - Standard option
7. **Erlotinib** (1st-gen EGFR) - Standard option
8. **Crizotinib** (1st-gen ALK) - Standard option
9. **Investigational_Drug_X** - Low confidence, needs review

## 데이터 관계도

```
EvidenceItems (8)
    ↓ reportedIn
Results (15)
    ↓ aboutDrug
Drugs (9)
    ↓ recommendedFor
PatientCases (5)
```

## 사용 방법

### Frontend에서 Mock Data 로드

```javascript
// 1. Evidence Library 페이지
async function loadEvidenceItems() {
  const response = await fetch('/mockdata/evidence-items.json');
  const evidence = await response.json();
  displayEvidenceList(evidence);
}

// 2. Patient Cases 페이지
async function loadPatientCases() {
  const response = await fetch('/mockdata/patient-cases.json');
  const cases = await response.json();
  displayCasesList(cases);
}

// 3. Drug Recommendations 페이지
async function loadDrugs() {
  const response = await fetch('/mockdata/drugs.json');
  const drugs = await response.json();
  
  // Filter top candidates
  const topCandidates = drugs.filter(d => d.topCandidate);
  displayDrugList(topCandidates);
}

// 4. Results 데이터로 차트 생성
async function loadResults() {
  const response = await fetch('/mockdata/results.json');
  const results = await response.json();
  
  // Group by drug and metric
  const grouped = results.reduce((acc, r) => {
    if (!acc[r.drugName]) acc[r.drugName] = [];
    acc[r.drugName].push(r);
    return acc;
  }, {});
  
  renderChart(grouped);
}
```

### Dashboard Statistics 계산

```javascript
async function calculateDashboardStats() {
  const [evidence, cases, drugs] = await Promise.all([
    fetch('/mockdata/evidence-items.json').then(r => r.json()),
    fetch('/mockdata/patient-cases.json').then(r => r.json()),
    fetch('/mockdata/drugs.json').then(r => r.json())
  ]);
  
  return {
    evidenceCount: evidence.length,  // 8 (실제로는 127로 변경 가능)
    casesCount: cases.filter(c => c.status === 'Active').length,
    drugsCount: drugs.length,
    validationRate: (evidence.filter(e => e.peerReviewed === 'true').length / evidence.length * 100).toFixed(1)
  };
}
```

### Evidence-Result Join

```javascript
async function getEvidenceWithResults() {
  const evidence = await fetch('/mockdata/evidence-items.json').then(r => r.json());
  const results = await fetch('/mockdata/results.json').then(r => r.json());
  
  return evidence.map(evi => ({
    ...evi,
    results: results.filter(r => r.evidenceId === evi.id)
  }));
}
```

## 데이터 확장 가능성

### 추가 가능한 필드

**EvidenceItem**:
- `keywords`: ["EGFR", "TKI", "NSCLC", ...]
- `citationCount`: 논문 인용 수
- `lastUpdated`: 마지막 업데이트 날짜
- `relatedEvidence`: 관련 증거 ID 리스트

**Result**:
- `confidenceInterval`: [lower, upper]
- `pValue`: 통계적 유의성
- `comparisonArm`: 비교군 결과
- `subgroupAnalysis`: 하위 그룹 분석

**PatientCase**:
- `biomarkers`: 추가 바이오마커 (PDL1, TMB, etc.)
- `imagingData`: 영상 소견
- `adverseEvents`: 부작용 기록
- `qualityOfLife`: QoL 점수

**Drug**:
- `cost`: 약가 정보
- `availability`: 국가별 허가 현황
- `combinationTherapies`: 병용 요법
- `resistanceMechanisms`: 내성 기전

## 실제 서버 연동 준비

Mock data 구조는 실제 API 스키마와 호환되도록 설계되었습니다:

```javascript
// Development (Mock)
const API_BASE = '/mockdata';

// Production (Real Server)
const API_BASE = 'https://api.nsclc-ai.com/v1';

async function fetchEvidence() {
  const response = await fetch(`${API_BASE}/evidence-items.json`);  // Dev
  // const response = await fetch(`${API_BASE}/evidence`);  // Prod
  return response.json();
}
```

## 테스트 시나리오

### 1. Evidence Library Search
- "Osimertinib" 검색 → EVI001, EVI002 반환
- "ALK" 필터 → EVI003, EVI004, EVI008 반환
- "High confidence" 필터 → 6건 반환

### 2. Patient Case Workflow
- CASE035 (Draft) → Tumor board review 시뮬레이션
- CASE036 (Active) → Evidence 연결 확인
- CASE037 (Active) → Alternative 약물 비교

### 3. Drug Recommendations
- EGFR ex19del 환자 → Osimertinib (0.95), Afatinib (0.78)
- ALK fusion 환자 → Alectinib (0.93), Brigatinib (0.90)

### 4. SWRL Rule Simulation
- RES003 (PFS 18.9) → highPriority flag
- RES004 (ORR 80%) → highPriority flag
- DRUG001 + multiple Paper evidence → topCandidate flag

## 버전

- **v1.0** (2026-01-08): Initial mock data release
  - 8 Evidence Items
  - 15 Results
  - 5 Patient Cases
  - 9 Drugs

## 라이선스

MIT License

## 참고

Mock data는 실제 임상 데이터를 기반으로 하되, 테스트 목적으로 단순화 및 익명화되었습니다.
