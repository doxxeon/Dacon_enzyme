# Dacon_enzyme
🧬 Dacon | Boost up AI 2025: 신약 개발 경진대회🧬

## 📌 프로젝트 개요

* **목표:** 화합물의 분자 구조(Canonical SMILES)로부터 CYP3A4 저해율(%)을 예측
* **평가 방식:**

  * **NRMSE** (Normalized RMSE): 예측 정확도
  * **Pearson Correlation**: 예측값과 실제값 간의 상관관계
  * **최종 Score:** `0.5 x (1 - min(NRMSE,1)) + 0.5 x Pearson`

## 🛠️ 주요 기법

### Molecular Feature Engineering

* **Morgan Fingerprint (ECFP)**: 분자 구조를 circular fingerprint로 이진 벡터화
* **MACCS Keys**: 166개의 주요 서브구조 존재 여부
* **RDKit 2D Descriptors**: 물리화학적 특성 (분자량, TPSA, LogP 등)
* **Mol2Vec Embedding**: Word2Vec 방식으로 분자 부분구조를 벡터화

### Ensemble Learning

* LightGBM
* XGBoost
* CatBoost
* K-Fold Cross Validation을 통해 OOF 예측 및 모델 안정성 평가

## ⚙️ 실행 환경 및 주요 라이브러리

* Python >= 3.8
* RDKit
* lightgbm
* xgboost
* catboost
* gensim (Mol2Vec)
* scikit-learn

## 🧪 실험 기록 요약

| Test | Features                 | Top Indices | Ensemble Weights | Local Score | Public Score |
| ---- | ------------------------ | ----------- | ---------------- | ----------- | ------------ |
| 1    | Morgan + MACCS           | 500         | (0.5,0.5,0)      | 0.6125      | 0.6931       |
| 3    | Morgan + MACCS           | 500         | (0.33,0.33,0.33) | 0.6147      | -            |
| 5    | Morgan + RDKit + Mol2Vec | 500         | (0.33,0.33,0.33) | 0.6157      | 0.7498       |
| 10   | Morgan + RDKit + Mol2Vec | 300         | (0.4,0.3,0.3)    | **0.6192**  | 0.66         |

✅ **베스트 스코어 모델:** Feature 3종 + Top 300 Feature + Ensemble Weight 조정

## 💡 사용 방법

```bash
# 가상환경 생성 권장
pip install -r requirements.txt
```


**참고:**

* `model_300dim.pkl` Mol2Vec 사전학습 가중치 필요 (jupyter file 내 다운로드 링크 첨부)
* `train.csv` 및 `test.csv` 데이터 필요

## 📂 디렉토리 구조

```
.
├── train.csv
├── test.csv
├── sample_submission.csv
├── model_300dim.pkl (다운로드 필요)
├── 0630.ipynb
├── README.md
└── requirements.txt
```

## 📝 참고 자료

* [Mol2Vec: Unsupervised Machine Learning Approach with Chemical Intuition](https://pubs.acs.org/doi/10.1021/acscentsci.7b00512)
* [RDKit Documentation](https://www.rdkit.org/docs/)
* [XGBoost Documentation](https://xgboost.readthedocs.io/)
* [LightGBM Documentation](https://lightgbm.readthedocs.io/)

---
