# Azure AI Foundry Evaluation Workshop

[![Azure AI](https://img.shields.io/badge/Azure%20AI-Foundry-0078D4?logo=microsoft-azure)](https://learn.microsoft.com/azure/ai-studio/)
[![Python](https://img.shields.io/badge/Python-3.9+-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

Azure AI Foundry의 Evaluation SDK를 활용하여 LLM 기반 애플리케이션의 품질을 평가하는 방법을 학습하는 실습 워크숍입니다.

---

## 📚 공식 문서 및 참조

| 리소스 | 링크 |
|--------|------|
| **Azure AI Evaluation SDK** | https://learn.microsoft.com/azure/ai-studio/how-to/develop/evaluate-sdk |
| **Evaluation 개념** | https://learn.microsoft.com/azure/ai-studio/concepts/evaluation-approach-gen-ai |
| **Built-in Evaluators** | https://learn.microsoft.com/azure/ai-studio/how-to/develop/evaluate-sdk#built-in-evaluators |
| **Custom Evaluators** | https://learn.microsoft.com/azure/ai-studio/how-to/develop/evaluate-sdk#custom-evaluators |
| **Python API Reference** | https://learn.microsoft.com/python/api/azure-ai-evaluation/azure.ai.evaluation |
| **Azure AI Foundry Portal** | https://ai.azure.com/ |
| **GitHub - azure-ai-evaluation** | https://github.com/Azure/azure-sdk-for-python/tree/main/sdk/evaluation/azure-ai-evaluation |
| **PyPI Package** | https://pypi.org/project/azure-ai-evaluation/ |
| **AI RAG Chat Evaluator** | https://github.com/Azure-Samples/ai-rag-chat-evaluator |

> 위 항목 중 `AI RAG Chat Evaluator`는 평가를 위해 꼭 한번 참고할만한 레포입니다.

---

## 🎯 워크숍 목표

1. Azure AI Foundry의 다양한 Evaluator 이해 및 활용
2. RAG (Retrieval-Augmented Generation) 시스템 품질 평가 방법 학습
3. 비즈니스 요구사항에 맞는 Custom Evaluator 구현
4. 할루시네이션(Hallucination) 탐지 및 품질 개선

---

## 📁 프로젝트 구조

```
Foundry Evaluation/
├── 📓 01_QA_Evaluator.ipynb           # QA 평가자 (종합 품질)
├── 📓 02_Retrieval_Evaluator.ipynb    # 검색 품질 평가자
├── 📓 03_Groundedness_Evaluator.ipynb # 근거성 평가자 (LLM 기반)
├── 📓 04_Groundedness_Pro_Evaluator.ipynb # 근거성 Pro 평가자 (서비스 기반)
├── 📓 05_Custom_Evaluator.ipynb       # 사용자 정의 평가자
├── 📂 data/
│   └── sample_qa.json                 # 샘플 Q&A 데이터셋
├── 📂 prompt/
│   └── response_quality.prompty       # 응답 품질 평가 프롬프트
├── requirements.txt                   # 필수 패키지
└── README.md                          # 이 문서
```

---

## 🔧 Evaluator 상세 설명

> **⚠️ 중요 사항**   
> 내장된 측정 지표는 한글을 파라미터로 입력해도 동작합니다. 다만, 내부적으로 영어 프롬프트를 사용하기 때문에 영어 답변 평가에 우수할 수 있어, 결과 값을 비교 후 **Custom Evaluator**를 사용할 수도 있음을 고려해야 합니다.

### 1️⃣ QAEvaluator (01_QA_Evaluator.ipynb)

**개요**: QAEvaluator는 Azure AI Evaluation SDK의 여러 Built-in Evaluator를
조합하여 종합 품질을 평가하는 개념적 워크숍 단위 평가 묶음이다.

| 메트릭 | 설명 |
|--------|------|
| `groundedness` | 응답이 컨텍스트에 기반하는 정도 |
| `relevance` | 응답이 질문에 관련된 정도 |
| `coherence` | 응답의 논리적 일관성 |
| `fluency` | 응답의 언어적 자연스러움 |

점수 범위는 기본 예제 프롬프트 기준이며,
Custom 또는 Prompty 기반 Evaluator에 따라 달라질 수 있음

**필요 입력**:
- `query`: 사용자 질문
- `response`: AI 응답
- `context`: 참조 컨텍스트

#### 💼 비즈니스 시나리오

| 시나리오 | 활용 방법 |
|----------|-----------|
| **고객 서비스 챗봇 품질 관리** | 고객 문의 응답의 전반적인 품질을 정량화하여 서비스 수준 모니터링 |
| **콘텐츠 생성 시스템 평가** | 마케팅 콘텐츠, 제품 설명 자동 생성의 품질 검증 |
| **AI 튜터링 시스템** | 학습자 질문에 대한 교육 콘텐츠 응답 품질 평가 |
| **기술 문서 자동화** | API 문서, 매뉴얼 자동 생성 품질 검증 |

---

### 2️⃣ RetrievalEvaluator (02_Retrieval_Evaluator.ipynb)

**개요**: RAG 시스템에서 검색된 컨텍스트의 관련성을 평가

| 메트릭 | 설명 | 점수 범위 |
|--------|------|-----------|
| `retrieval` | 검색 관련성 점수 | 1-5 |
| `retrieval_result` | Pass/Fail 결과 | pass/fail |
| `retrieval_reason` | 점수 근거 설명 | 텍스트 |

**필요 입력**:
- `query`: 사용자 질문
- `context`: 검색된 컨텍스트

#### 💼 비즈니스 시나리오

| 시나리오 | 활용 방법 |
|----------|-----------|
| **기업 지식 베이스 검색** | 사내 문서 검색 시스템의 검색 품질 측정 및 개선점 파악 |
| **법률/의료 문서 검색** | 규정 준수가 중요한 도메인에서 정확한 문서 검색 보장 |
| **제품 카탈로그 검색** | E-commerce 제품 검색의 관련성 평가 |
| **RAG 파이프라인 최적화** | 청크 크기, 임베딩 모델, 검색 알고리즘 튜닝 |

**⚠️ 주요 탐지 케이스**:
- `rag-fail-agent`: 질문과 무관한 컨텍스트가 검색되는 경우 (검색 실패)
- 검색 결과가 질문의 의도와 맞지 않는 경우

---

### 3️⃣ GroundednessEvaluator (03_Groundedness_Evaluator.ipynb)

**개요**: LLM을 사용하여 응답이 제공된 컨텍스트에 얼마나 근거하는지 평가

| 메트릭 | 설명 | 점수 범위 |
|--------|------|-----------|
| `groundedness` | 근거성 점수 | 1-5 |
| `groundedness_reason` | 평가 근거 | 텍스트 |

**점수 해석**:
- **5점**: 응답이 완전히 컨텍스트에 기반
- **3점**: 부분적으로 컨텍스트에 기반
- **1점**: 근거 없음 / 할루시네이션

**필요 입력**:
- `query`: 사용자 질문 (선택)
- `response`: 평가할 응답
- `context`: 참조 컨텍스트

#### 💼 비즈니스 시나리오

| 시나리오 | 활용 방법 |
|----------|-----------|
| **할루시네이션 탐지** | AI가 만들어낸 거짓 정보 자동 식별 |
| **의료/법률 AI 검증** | 생명과 법적 책임이 관련된 응답의 사실 검증 |
| **금융 보고서 생성** | 재무 데이터 기반 보고서의 정확성 검증 |
| **뉴스/미디어 팩트체크** | 생성된 콘텐츠의 사실 여부 확인 |

**⚠️ 주요 탐지 케이스**:
- `hallucination-agent`: 컨텍스트에 없는 정보를 생성하는 경우

---

### 4️⃣ GroundednessProEvaluator (04_Groundedness_Pro_Evaluator.ipynb)

**개요**: Azure AI 평가 서비스를 사용하는 고급 근거성 평가자   
사용자 Azure OpenAI 리소스를 직접 호출하지 않으며, Azure AI Evaluation 서비스에 포함된 관리형 모델을 사용

| 특징 | 설명 |
|------|------|
| **평가 방식** | Azure AI 클라우드 서비스 기반 |
| **LLM 비용** | 없음 (서비스 요금만 적용) |
| **속도** | 더 빠름 |
| **인증** | Azure AI Project + Credential 필요 |

**필요 설정**:
```python
azure_ai_project = {
    "subscription_id": "your-subscription-id",
    "resource_group_name": "your-resource-group",
    "project_name": "your-project-name"
}
```

#### 💼 비즈니스 시나리오

| 시나리오 | 활용 방법 |
|----------|-----------|
| **대규모 평가 파이프라인** | 수천 건의 응답을 비용 효율적으로 평가 |
| **실시간 모니터링** | 프로덕션 환경에서 응답 품질 실시간 추적 |
| **CI/CD 통합** | 모델 배포 전 자동화된 품질 게이트 |
| **A/B 테스트** | 모델 버전별 품질 비교 분석 |

---

### 5️⃣ Custom Evaluator (05_Custom_Evaluator.ipynb)

**개요**: 비즈니스 요구사항에 맞춘 사용자 정의 평가자 구현

#### 5-1. Code-based Evaluator

규칙 기반으로 빠르게 평가하는 Python 클래스

```python
class AnswerLengthEvaluator:
    def __call__(self, *, response: str, **kwargs) -> Dict:
        return {"answer_length": len(response)}
```

**장점**: 빠른 실행, 비용 없음, 일관된 평가  
**단점**: 의미 기반 평가 불가

#### 5-2. Prompty-based Evaluator

`.prompty` 파일을 사용한 LLM 기반 평가

```yaml
# evaluators/response_quality.prompty
inputs:
  question: string
  response: string
  context: string
outputs:
  accuracy_score: number
  completeness_score: number
  relevance_score: number
  clarity_score: number
```

**장점**: 의미 기반 정교한 평가, 다차원 분석  
**단점**: LLM 호출 비용, 느린 속도

#### 💼 비즈니스 시나리오

| 시나리오 | 활용 방법 |
|----------|-----------|
| **브랜드 톤앤매너 검증** | 기업 브랜드 가이드라인 준수 여부 평가 |
| **다국어 응답 품질** | 번역/다국어 응답의 문화적 적절성 평가 |
| **산업별 규정 준수** | HIPAA, GDPR 등 규정 준수 체크리스트 |
| **고객 감성 분석** | 응답의 친절도, 공감도 등 감성 메트릭 |

---

## 📊 샘플 데이터셋 (sample_qa.json)

5가지 에이전트 유형으로 구성된 33개 Q&A 샘플:

| 에이전트 | 설명 | 용도 |
|----------|------|------|
| `product-agent` | 제품 정보 응답 | 정상 RAG 응답 평가 |
| `review-agent` | 리뷰 요약 응답 | 요약 품질 평가 |
| `policy-agent` | 정책 안내 응답 | 규정 기반 응답 평가 |
| `hallucination-agent` | 할루시네이션 포함 | 할루시네이션 탐지 테스트 |
| `rag-fail-agent` | 검색 실패 케이스 | 검색 품질 평가 테스트 |

---

## � 시작하기 전 - 데이터셋 생성 가이드

### Ground Truth 데이터셋의 중요성

평가를 위해서는 새로운 응답을 비교할 수 있는 **"Ground Truth"(정답) 데이터**가 필요합니다. Ground Truth는 특정 질문에 대한 이상적인 답변을 의미하며, 평가의 기준점이 됩니다.

프로젝트의 `data/sample_qa.json` 파일이 이러한 형식의 예시입니다. **가능하면 최소 200개 이상의 QA 쌍**을 준비하는 것을 권장합니다.

### 데이터셋 생성 방법

다음 3가지 접근 방법 중 하나를 선택할 수 있습니다:

#### 1️⃣ 수동 큐레이션 (Manual Curation)

**방법**: 이상적이라고 판단되는 질문-답변 쌍을 직접 작성합니다.

**장점**:
- 가장 정확하고 신뢰할 수 있는 데이터
- 도메인 전문가의 지식과 기준이 반영됨

**단점**:
- 가장 많은 시간과 노력이 필요
- 도메인 전문 지식 필수

**주의사항**:
- 응답에 인용(citations)이 필요한 경우, 예상되는 형식으로 포함시켜야 함
- 일관된 품질 기준을 유지해야 함

#### 2️⃣ 자동 생성 후 직접 사용 (Direct Generation)

**방법**: 생성 스크립트를 사용하여 질문-답변 쌍을 생성하고, 바로 사용합니다.

**장점**:
- 가장 빠른 방법
- 대량의 데이터를 신속하게 확보 가능

**단점**:
- 정확도가 가장 낮을 수 있음
- 생성된 답변의 품질 보장 어려움

**적용 시나리오**:
- 초기 프로토타입 단계
- 평가 파이프라인 구축 테스트용

#### 3️⃣ 생성 + 큐레이션 조합 (Hybrid Approach) ⭐ **권장**

**방법**: 생성 스크립트로 초안을 만든 후, 수동으로 검토하고 개선합니다.

**절차**:
1. 스크립트로 질문-답변 쌍 생성
2. 품질이 낮은 답변을 재작성
3. 누락된 인용(citations) 추가
4. 중복되거나 유사한 질문 제거

**장점**:
- 속도와 정확성의 균형
- 실용적이고 효율적
- 품질 관리 가능

**이것이 우리가 권장하는 방법입니다.**

<details>
<summary><b>📌 데이터셋 생성 추가 팁 (펼쳐보기)</b></summary>

<br>

#### ✅ 품질 관리

- **필요량보다 더 많이 생성**: 처음에는 더 많은 QA 쌍을 생성한 후, 품질과 중복도를 기준으로 정제합니다.
- **저품질 답변 제거**: 불완전하거나 부정확한 답변은 제외합니다.
- **중복 질문 제거**: 너무 유사한 질문들은 통합하거나 제거합니다.

#### ✅ 지식 분포 고려

- **문서 전반에 걸친 샘플링**: 지식 베이스의 특정 영역에 편중되지 않도록 다양한 주제를 다룹니다.
- **난이도 분포**: 쉬운 질문부터 복잡한 질문까지 고르게 포함합니다.
- **질문 유형 다양화**: 사실 확인, 비교, 절차 설명 등 다양한 유형을 포함합니다.

#### ✅ 실제 사용자 질문 반영

- **프로덕션 환경에서 수집**: 챗봇이 배포된 후에는 실제 사용자 질문을 지속적으로 샘플링합니다.
- **개인정보 보호 정책 준수**: 사용자 데이터 수집 시 개인정보 보호 정책을 준수해야 합니다.
- **트렌드 반영**: 사용자가 실제로 묻는 질문의 유형과 패턴을 데이터셋에 반영합니다.

</details>

### 데이터셋 형식 예시

```json
{
  "query": "사용자 질문",
  "response": "이상적인 응답 (필요시 인용 포함)",
  "context": "참조 컨텍스트 (RAG 시스템의 경우)",
  "ground_truth": "정답 (선택사항)"
}
```

자세한 형식은 [data/sample_qa.json](data/sample_qa.json) 파일을 참조하세요.


---

## �🚀 시작하기

### 1. 환경 설정

```bash
# 가상환경 생성
python -m venv .venv

# 가상환경 활성화 (Windows)
.venv\Scripts\activate

# 패키지 설치
pip install -r requirements.txt
```

### 2. Azure 설정

각 노트북에서 `model_config` 설정:

```python
model_config = {
    "azure_endpoint": "https://your-endpoint.openai.azure.com/",
    "api_key": "your-api-key",
    "azure_deployment": "gpt-4",
    "api_version": "2024-12-01-preview"
}
```

※ 모델명은 Foundry 프로젝트에 배포된 Deployment Name 기준이며
예시는 환경에 따라 반드시 수정 필요

### 3. 노트북 실행 순서

| 순서 | 노트북 | 설명 |
|------|--------|------|
| 1 | `01_QA_Evaluator.ipynb` | 종합 품질 평가 (기본) |
| 2 | `02_Retrieval_Evaluator.ipynb` | 검색 품질 평가 |
| 3 | `03_Groundedness_Evaluator.ipynb` | 근거성 평가 (LLM) |
| 4 | `04_Groundedness_Pro_Evaluator.ipynb` | 근거성 평가 (서비스) |
| 5 | `05_Custom_Evaluator.ipynb` | 사용자 정의 평가 |

---

## 📈 평가 결과 해석 가이드

### 점수별 품질 등급

| 점수 범위 | 등급 | 권장 조치 |
|-----------|------|-----------|
| 4.5 - 5.0 | 🟢 우수 | 배포 가능 |
| 3.5 - 4.4 | 🟡 양호 | 모니터링 필요 |
| 2.5 - 3.4 | 🟠 보통 | 개선 필요 |
| 1.0 - 2.4 | 🔴 미흡 | 긴급 수정 필요 |

※ 본 등급은 워크숍 실습용 가이드이며
실제 배포 결정 기준을 대체하지 않음

### 에이전트별 예상 결과

| 에이전트 | 예상 Retrieval | 예상 Groundedness | 설명 |
|----------|---------------|-------------------|------|
| product-agent | 높음 (4-5) | 높음 (4-5) | 정상 RAG 응답 |
| review-agent | 높음 (4-5) | 중간-높음 (3-5) | 요약 시 일부 누락 가능 |
| policy-agent | 높음 (4-5) | 높음 (4-5) | 규정 기반 정확한 응답 |
| hallucination-agent | 높음 (4-5) | 낮음 (1-2) | 컨텍스트와 불일치 |
| rag-fail-agent | 낮음 (1-2) | N/A | 검색 자체 실패 |

---

## ⚠️ 중요 사항 및 한계

### 이 프레임워크는 무엇인가
- Microsoft Foundry Evaluation SDK를 활용한 **평가 파이프라인**
- **LLM을 평가자로 활용하는(LLM-assisted)** 평가 방식
- 배치 평가, 비교 분석, 추세 모니터링 지원

### 이 프레임워크가 아닌 것
- 인간 판단을 **대체**하는 시스템이 아님
- 결정론적(deterministic) 평가 도구가 아님
- 법적·규정 준수 판단용 최종 도구가 아님

### 권장 사용 방식
- ✅ 동일 조건 하 상대 비교
- ✅ 시간에 따른 변화 추적
- ✅ Human-in-the-loop 연계
- ❌ 점수를 절대적 정답으로 사용
- ❌ 자동 승인 기준으로만 사용

---

## 🛠️ 트러블슈팅

| 오류 | 원인 | 해결 방법 |
|------|------|-----------|
| `ModuleNotFoundError` | 패키지 미설치 | `pip install azure-ai-evaluation` |
| `404 Not Found` | 잘못된 엔드포인트/배포명 | Azure Portal에서 확인 후 수정 |
| `401 Unauthorized` | API 키 오류 | API 키 재확인 |
| `RequiredEnvironmentVariablesNotSetError` | 환경 변수 미설정 | `os.environ` 설정 추가 |

---

## 📜 라이선스

MIT License

---

## 🤝 기여하기

이슈 제출 및 PR 환영합니다!

---

## 📞 추가 참조

- [Azure AI Foundry Documentation](https://learn.microsoft.com/azure/ai-studio/)
- [Azure AI Evaluation API Reference](https://learn.microsoft.com/python/api/azure-ai-evaluation/azure.ai.evaluation)
- [Prompty Specification](https://prompty.ai/)

---

## 📋 Azure AI Evaluation SDK의 추가 Evaluator

이 워크숍에서는 일부 Evaluator만 다루었습니다. Azure AI Evaluation SDK는 다양한 Built-in Evaluator를 제공하며, 전체 목록은 [공식 문서](https://learn.microsoft.com/python/api/azure-ai-evaluation/azure.ai.evaluation?view=azure-python)에서 확인할 수 있습니다.

### 콘텐츠 품질 평가 Evaluators

| Evaluator | 설명 | 점수 범위 | 워크숍 포함 여부 |
|-----------|------|-----------|------------------|
| **CoherenceEvaluator** | 응답의 논리적 일관성과 자연스러운 흐름 평가 | 1-5 | ✅ (01 포함) |
| **FluencyEvaluator** | 문법, 구문, 어휘 사용의 언어적 정확성 평가 | 1-5 | ✅ (01 포함) |
| **RelevanceEvaluator** | 응답이 질문에 얼마나 관련있는지 평가 | 1-5 | ✅ (01 포함) |
| **SimilarityEvaluator** | 생성된 응답과 참조 답변 간의 의미적 유사도 평가 | 1-5 | ❌ |
| **IntentResolutionEvaluator** | 사용자 의도가 올바르게 식별되고 해결되었는지 평가 | 1-5 | ❌ |
| **ResponseCompletenessEvaluator** | 응답이 필요한 모든 정보를 포함하는지 평가 | 1-5 | ❌ |

### 근거성 및 검색 평가 Evaluators

| Evaluator | 설명 | 점수 범위 | 워크숍 포함 여부 |
|-----------|------|-----------|------------------|
| **GroundednessEvaluator** | LLM 기반으로 응답의 컨텍스트 근거성 평가 | 1-5 | ✅ (03 포함) |
| **GroundednessProEvaluator** | 서비스 기반 고급 근거성 평가 (비용 효율적) | Boolean | ✅ (04 포함) |
| **RetrievalEvaluator** | RAG 시스템의 검색 품질 평가 | 1-5 | ✅ (02 포함) |

### 텍스트 유사도 메트릭 Evaluators

| Evaluator | 설명 | 점수 범위 | 워크숍 포함 여부 |
|-----------|------|-----------|------------------|
| **BleuScoreEvaluator** | BLEU 점수 - 기계 번역 및 텍스트 생성 평가 (n-gram 기반) | 0-1 | ❌ |
| **GleuScoreEvaluator** | GLEU 점수 - 문장 수준의 번역 품질 평가 (균형잡힌 n-gram) | 0-1 | ❌ |
| **MeteorScoreEvaluator** | METEOR 점수 - 동의어, 어간, 어순을 고려한 번역 평가 | 0-1 | ❌ |
| **RougeScoreEvaluator** | ROUGE 점수 - 텍스트 요약 평가 (재현율 중심) | 0-1 | ❌ |
| **F1ScoreEvaluator** | F1 점수 - 정밀도와 재현율의 조화 평균 | 0-1 | ❌ |

### 콘텐츠 안전성 평가 Evaluators (실험적 기능)

| Evaluator | 설명 | 점수 범위 | 워크숍 포함 여부 |
|-----------|------|-----------|------------------|
| **ContentSafetyEvaluator** | QA 시나리오의 전반적인 콘텐츠 안전성 평가 | 0-7 | ❌ |
| **HateUnfairnessEvaluator** | 혐오 및 불공정 콘텐츠 탐지 및 평가 | 0-7 | ❌ |
| **ViolenceEvaluator** | 폭력적 콘텐츠 탐지 및 평가 | 0-7 | ❌ |
| **SexualEvaluator** | 성적 콘텐츠 탐지 및 평가 | 0-7 | ❌ |
| **SelfHarmEvaluator** | 자해 관련 콘텐츠 탐지 및 평가 | 0-7 | ❌ |

### 보안 및 위험 평가 Evaluators (실험적 기능)

| Evaluator | 설명 | 점수 범위 | 워크숍 포함 여부 |
|-----------|------|-----------|------------------|
| **IndirectAttackEvaluator** | 간접 공격(XPIA) 탐지 - 프롬프트 인젝션 등 | Boolean | ❌ |
| **ProtectedMaterialEvaluator** | 저작권 보호 자료 탐지 - 가사, 레시피, 기사 등 | Boolean | ❌ |
| **CodeVulnerabilityEvaluator** | 코드 취약점 탐지 - SQL 인젝션, XSS 등 | Boolean | ❌ |
| **UngroundedAttributesEvaluator** | 근거 없는 인적 속성 추론 탐지 | Boolean | ❌ |

### AI 작업 품질 평가 Evaluators (실험적 기능)

| Evaluator | 설명 | 점수 범위 | 워크숍 포함 여부 |
|-----------|------|-----------|------------------|
| **TaskAdherenceEvaluator** | AI 어시스턴트의 작업 준수도 평가 (목표/규칙/절차) | Boolean | ❌ |
| **ToolCallAccuracyEvaluator** | 도구 호출의 정확성 평가 (관련성, 매개변수 정확성) | 1-5 | ❌ |

### 종합 평가 Evaluators

| Evaluator | 설명 | 점수 범위 | 워크숍 포함 여부 |
|-----------|------|-----------|------------------|
| **QAEvaluator** | 다중 메트릭 종합 평가 (Groundedness, Relevance, Coherence, Fluency) | 복합 | ✅ (01 포함) |

### 고급 Grader 클래스 (실험적 기능)

| Grader | 설명 | 워크숍 포함 여부 |
|--------|------|------------------|
| **AzureOpenAIGrader** | Azure OpenAI 기반 커스텀 평가자 베이스 클래스 | ❌ |
| **AzureOpenAILabelGrader** | 레이블 분류 기반 평가 | ❌ |
| **AzureOpenAIPythonGrader** | Python 코드 실행 기반 평가 | ❌ |
| **AzureOpenAIScoreModelGrader** | 연속 점수 평가 (사용자 정의 프롬프트) | ❌ |
| **AzureOpenAIStringCheckGrader** | 문자열 체크 기반 평가 | ❌ |
| **AzureOpenAITextSimilarityGrader** | 텍스트 유사도 기반 평가 | ❌ |

---

### 💡 Evaluator 선택 가이드

| 사용 사례 | 권장 Evaluator |
|-----------|----------------|
| **RAG 시스템 평가** | RetrievalEvaluator, GroundednessEvaluator, RelevanceEvaluator |
| **챗봇 품질 평가** | QAEvaluator (종합), CoherenceEvaluator, FluencyEvaluator |
| **번역 품질 평가** | BleuScoreEvaluator, MeteorScoreEvaluator |
| **요약 품질 평가** | RougeScoreEvaluator, F1ScoreEvaluator |
| **콘텐츠 안전성 검증** | ContentSafetyEvaluator, HateUnfairnessEvaluator, ViolenceEvaluator |
| **할루시네이션 탐지** | GroundednessEvaluator, GroundednessProEvaluator |
| **보안 위협 탐지** | IndirectAttackEvaluator, CodeVulnerabilityEvaluator |
| **AI 에이전트 평가** | TaskAdherenceEvaluator, ToolCallAccuracyEvaluator |

---

### ⚠️ 실험적 기능(Experimental) 주의사항

`실험적 기능`으로 표시된 Evaluator들은 향후 변경될 수 있습니다:
- API 인터페이스 변경 가능
- 평가 기준 및 점수 체계 변경 가능
- 프로덕션 환경에서 사용 시 주의 필요
- 자세한 내용: https://aka.ms/azuremlexperimental

---

### 📚 더 알아보기

전체 Evaluator 목록 및 최신 업데이트는 다음 링크를 참조하세요:
- [Azure AI Evaluation API Reference](https://learn.microsoft.com/python/api/azure-ai-evaluation/azure.ai.evaluation?view=azure-python)
- [Built-in Evaluators 가이드](https://learn.microsoft.com/azure/ai-studio/how-to/develop/evaluate-sdk#built-in-evaluators)
