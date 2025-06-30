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

### 📄 Expriments Notes

**[ First Test ]**   
- Feature: Morgan Fingerprint + MACCS (500 top)
- top_indices: 500
- KFold + LightGBM + XGBoost (learning_rate = 0.01)

= Local Score: 0.6125 / NRMSE: 0.2358 / Pearson: 0.4607 

= **Public Score: 0.69314**

---

**[ Second Test ]**   
- Feature: Morgan Fingerprint + MACCS (500 top)
- top_indices: 1500
- KFold + LightGBM + XGBoost (learning_rate = 0.02)

= Local Score: 0.6032 / NRMSE: 0.2380 / Pearson: 0.4445

---

**[ Third Test ]**   
- Feature: Morgan Fingerprint + MACCS (500 top)
- top_indices: 500
- KFold + LightGBM + XGBoost + CatBoost (learning_rate = 0.01)

= Local Score: 0.6147 / NRMSE: 0.2352 / Pearson: 0.4646

---

**[ Fourth Test ]**
- Feature: Morgan Fingerprint + RDKit 2D Description (500 top)
- top_indices: 500
- KFold + LightGBM + XGBoost + CatBoost (learning_rate = 0.01)

= Local Score: 0.6154 / NRMSE: 0.2351 / Pearson: 0.4659

---

**[ Fifth Test ]**
- Feature: Morgan Fingerprint + RDKit 2D Description + Mol2Vec Embedding (500 top)
- top_indices: 500
- KFold + LightGBM + XGBoost + CatBoost (learning_rate = 0.01)

= Local Score: 0.6157 / NRMSE: 0.2350 / Pearson: 0.4663   

= **Public Score: 0.74983**

---

**[ Sixth Test ]**
- Feature: Morgan Fingerprint + RDKit 2D Description + Mol2Vec Embedding (500 top)
- top_indices: 1000
- KFold + LightGBM + XGBoost + CatBoost (Ensemble Weight 0.5:0.2:0.3)

= Local Score: 0.6138 / NRMSE: 0.2355 / Pearson: 0.4632   

---

**[ Seventh Test ]**
- Feature: Morgan Fingerprint + RDKit 2D Description + Mol2Vec Embedding (500 top)
- top_indices: 500
- KFold + LightGBM + XGBoost + CatBoost (Ensemble Weight 0.5:0.2:0.3)

= Local Score: 0.6157 / NRMSE: 0.2350 / Pearson: 0.4663   

---

**[ Eighth Test ]**
- Feature: Morgan Fingerprint + RDKit 2D Description + Mol2Vec Embedding (500 top)
- top_indices: 500
- KFold + LightGBM + XGBoost + CatBoost (Ensemble Weight 0.5:0.2:0.3)
- Hyperparameter (LightGBM, XGBoost): Lr [0.01 > 0.015], max_depth [7 > 9], subsample, colsample_bytree [0.8 > 0.9]
- Hyperparameter (CatBoost): Lr [0.01 > 0.015], depth [7 > 9]

= Local Score: 0.6119 / NRMSE: 0.2359 / Pearson: 0.4597   

---

**[ Ninth Test ]**
- Feature: Morgan Fingerprint + RDKit 2D Description + Mol2Vec Embedding (500 top)
- top_indices: 500
- KFold + LightGBM + XGBoost + CatBoost (Ensemble Weight 0.5:0.2:0.3)
- Hyperparameter (LightGBM): Lr [0.015 > 0.01], max_depth [9 > 7], subsample, colsample_bytree [0.9 > 0.8], num_leaves [64 > 128]
- Hyperparameter (XGBoost): Lr [0.015 > 0.01], max_depth [9 > 7], subsample, colsample_bytree [0.9 > 0.8]

= Local Score: 0.6147 / NRMSE: 0.2353 / Pearson: 0.4646 

---

**[ Tenth Test ]**
- Feature: Morgan Fingerprint + RDKit 2D Description + Mol2Vec Embedding (500 top)
- top_indices: 300
- KFold + LightGBM + XGBoost + CatBoost (Ensemble Weight 0.4:0.3:0.3)
- Hyperparameter (LightGBM): num_leaves [128 > 64]

= Local Score: 0.6192 / NRMSE: 0.2342 / Pearson: 0.4725

= **Public Score: 0.66617**


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
