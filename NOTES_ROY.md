# SEAL 분석 노트 (ROYBANNER-rail fork)

원본: https://github.com/Continual-Intelligence/SEAL (MIT License, 저작권은 원저자에게 있음)

## 한 줄 요약
모델이 새 정보를 받으면 스스로 학습 노트(self-edit)를 쓰고, 그 노트로 자신을 잠깐 학습한 뒤
정답률이 오른 노트만 골라 「노트 쓰는 능력」을 강화한다.

## 코드 지도
| 단계 | 파일 |
|---|---|
| 노트 생성 (implications) | general-knowledge/src/data_generation/make_squad_data.py |
| 임시 LoRA 학습 + 전/후 정답률 | general-knowledge/src/inner/TTT_server.py |
| 상위 노트 선별 (top-k by adapter_mean) | general-knowledge/src/EM/build_SFT_dataset.py |
| 노트 생성 능력 학습 (ReST-EM) | general-knowledge/src/EM/train_SFT.py |
| ARC 퍼즐 버전 | few-shot/self-edit.py, few-shot/BC-self-edit.py |

## 실행 요건
A100/H100 GPU 2장, SLURM, Python 3.12, OpenAI API 키. 개인 맥에서는 실행 불가.

## 재해석 방향
가중치를 학습하는 대신, 에이전트의 매뉴얼 문서를 같은 루프로 개선한다.
수정안 여러 개 생성 → 같은 작업에 적용 → 검증 항목 통과 수로 채점 → 최고점만 채택.
