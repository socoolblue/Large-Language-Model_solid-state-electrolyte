# Ionic Conductivity Data Extraction Pipeline
고체 전해질(solid‑state electrolyte) 논문 텍스트(.txt)에서 이온전도도(Ionic Conductivity) 관련 데이터를 자동 추출하고,
두 개의 대형 언어모델(LLM)을 사용해 결과를 비교‧정제 후 머신러닝 학습용 CSV로 저장하는 파이프라인입니다.

## 기능 개요
- 논문 텍스트에서 물질 조성, 온도, 전도도 등 핵심 정보를 자동으로 추출

- GPT(OpenAI)와 Claude(Anthropic)의 결과를 병합

- 수치 정규화 및 누락값 처리로 머신러닝 준비 완료된 데이터셋 생성

- 지정된 폴더 내다 .txt 파일을 한꺼번에 처리하는 배치 모드 지원

## 추출 항목
- section

- chemical composition

- source (DOI)

- exp / calc

- temperature / temp_unit

- conductivity / unit

- activation e

- structure type

- chemical family

- mobile ion

- Match Level

- Reliability

- Notes

## 요구 사항
pandas  
anthropic  
openai  
pip install -r requirements.txt

## 프로젝트 구조
    ├─ main.py                   # 핵심 스크립트
    ├─ README.md
    ├─ requirements.txt
    ├─ data/                    # 입력 .txt 파일 모음
    └─ output/
        └─ extracted_data.csv     # 최종 통합된 데이터셋

## 주요 구성요소
extract_data(model, text, property_name, definition, doi)
주어진 텍스트와 LLM을 사용해 마크다운 형식의 표로 데이터 추출

compare_results(...)
두 모델 결과를 비교 → Match Level 및 Reliability 산출 → 최종 데이터 선택

parse_table(table_string, source)
표 문자열을 구조화된 데이터 레코드로 파싱

update_dataframe(new_data)
데이터프레임 갱신 및 CSV 저장

process_all_files_in_folder(folder_path, property_name, definition, preferred_model='gpt')
폴더 내 .txt 파일을 재귀 탐색하여 전체 자동 처리

## 사용 방법
1. API 키 설정

from anthropic import Anthropic  
import openai  
anthropic = Anthropic(api_key="YOUR_ANTHROPIC_API_KEY")  
openai.api_key = "YOUR_OPENAI_API_KEY"
2. 텍스트 파일 준비
‑ 논문 본문 또는 섹션을 .txt로 저장
‑ DOI 등을 별도로 관리

3. 실행

folder_path = "data"  
process_all_files_in_folder(folder_path, property_name, definition)
4. 결과 확인

- output/extracted_data.csv 파일 생성

- 머신러닝 회귀 모델의 입력 데이터로 즉시 활용 가능

## 주의 및 확장사항
- PDF → 텍스트 변환 기능은 포함되어 있지 않습니다

- LLM 출력 포맷 변동 시 파싱 오류가 발생할 수 있으므로 예외처리 권장

- 추후 구조 정보(격자 상수, 공간군 등)를 추가 피처로 적용 가능

## 라이선스
필요시 MIT License 등으로 오픈소스화 가능합니다.
