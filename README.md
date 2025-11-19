# Ionic Conductivity Data Extractor

Automated Data Extraction & Harmonization Tool using GPT / Claude

이 프로젝트는 과학 논문 텍스트(.txt)에서 이온전도도(Ionic Conductivity) 관련 데이터를 자동으로 추출하여,
Deep Learning 회귀 모델 학습에 바로 사용할 수 있는 통합 CSV 데이터셋을 생성하는 Python 기반 도구입니다.

Claude와 GPT 두 모델의 결과를 비교하여 정확성, 신뢰도, 일치도를 자동 평가하고,
가장 적합한 최종 데이터를 선택하여 하나의 표준화된 형태로 저장합니다.

## 주요 기능 (Features)
### 1. 논문 텍스트 분석 및 데이터 추출

Claude / GPT 모두로 동일한 텍스트를 분석하여

구조 타입

화학조성

온도

단위

이온전도도

활성화 에너지

화학 패밀리

이동 이온 종류
등 실험 데이터 추출 및 정리

### 2. 두 모델 결과 비교 및 자동 정합(Matching)

Exact / Semantic / Partial / Mismatch / Missing

모델 간 비교를 통해 가장 정확한 Final Value 선택

신뢰도(High/Medium/Low) 자동 표시

### 3. 딥러닝 입력용 표준 포맷 생성

최종 CSV 컬럼:

section, chemical composition, source, exp. calc, temperature, temp_unit,
conductivity, unit, activation e, structure type, chemical family,
mobile ion, Match Level, Reliability, Notes

### 4. 폴더 내 모든 .txt 파일 자동 처리

여러 논문 section 파일을 한 번에 처리

통합 CSV(추출된 파일.csv)로 저장

## 설치 및 환경 설정 (Installation)
1. 저장소 클론
git clone https://github.com/yourname/repo-name.git
cd repo-name

2. 필요한 라이브러리 설치
pip install pandas openai anthropic

3. API 키 설정

코드 내 다음 부분을 실제 키로 교체:

anthropic = Anthropic(api_key='YOUR_CLAUDE_API_KEY')
openai.api_key = 'YOUR_OPENAI_API_KEY'


보안을 위해 환경변수 사용을 권장합니다:

export ANTHROPIC_API_KEY="..."
export OPENAI_API_KEY="..."

## 실행 방법 (Usage)
1. 논문 section 텍스트(.txt) 파일 준비

폴더 구조 예시:

/paper_sections
    /2025_01/
        section1.txt
        section2.txt
    /2025_02/
        ...

2. 코드에서 폴더 경로 지정
folder_path = "path/to/paper_sections"

3. 전체 파일 자동 처리 실행
python main.py

4. 출력 파일

프로젝트 폴더 내에 다음 CSV 생성:

추출된 파일.csv

## 데이터 처리 파이프라인
flowchart TD
A[Input: Paper Section (.txt)] --> B[Claude Extraction]
A --> C[GPT Extraction]

B --> D[Result Comparison]
C --> D

D --> E[Final Standardized Table Row]
E --> F[Append to Global DataFrame]
F --> G[Output CSV File]

## 출력 형식 예시
section	chemical composition	source	exp. calc	temperature	temp_unit	conductivity	unit	activation e	structure type	chemical family	mobile ion	Match Level	Reliability	Notes
Results (text)	Li₇P₃S₁₁	10.1234/abcd.2025	exp	25	°C	1.2e-3	S/cm	0.32	Glass-Ceramic	Thiophosphate	Li⁺	Exact Match	High	-
## 주의사항

GPT/Claude 응답 토큰 수에 따라 분석 범위가 제한될 수 있습니다.

너무 긴 section은 잘라서 넣는 것이 좋습니다.

논문 PDF를 직접 업로드하는 기능은 포함되어 있지 않습니다.

"추출된 파일.csv" 내에서 conductivity가 "No data" 또는 "N/A"인 항목은 자동 제외됩니다.
