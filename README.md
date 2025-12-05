# Network traffic anomaly detection

5 моделей для детекта аномалий по flow-признакам: Random Forest, CatBoost, MLP, Isolation Forest, Autoencoder.

## Что за данные
`N=10005` синтетических flow-записей для SDN. Одна строка = агрегированный поток: `time`, IP/порты, `protocol`, `duration`, `packet_count`, `bytes_sent`, `bytes_received`, метка `label` (0 норма, 1 аномалия).

Аномалии: DDoS, port scan, exfiltration, brute-force, protocol misuse (GRE/ESP/AH и нетипичные порты).  
Норма: в основном TCP, стандартные сервисные порты (80/443/22/53), умеренные длительности/счетчики.

## Структура
- `data/` — датасет
- `notebooks/`
  - `DetectAnomaly_RF.ipynb`: random forest
  - `DetectAnomaly_CB.ipynb`: catboost (gradient boosting)
  - `DetectAnomaly_MLP.ipynb`: torch MLP
  - `DetectAnomaly_IF.ipynb`: isolation forest (semi-supervised)
  - `DetectAnomaly_AE.ipynb`: autoencoder (semi-supervised)

## Как запустить
```bash
python -m venv .venv
source .venv/bin/activate
pip install numpy pandas scikit-learn catboost torch matplotlib jupyter ipykernel
python -m ipykernel install --user --name <kernel-name>
juputer lab
```
Далее уже можно открывать и запускать ноутбуки с модельками (путь до данных надо будет поменять)

## Метрики

| model | ROC-AUC | accuracy | F1(1) | P(1) | ROC(1) |
|---|---:|---:|---:|---:|---:|
| Random Forest | 1.0000 | 0.9990 | 0.9975 | 0.9950 | 1.0000 |
| CatBoost | 1.0000 | 0.9985 | 0.9963 | 0.9926 | 1.0000 |
| MLP | 0.9198 | 0.8861 | 0.6028 | 1.0000 | 0.4314 |
| Isolation Forest | 0.9684 | — | — | — | — |
| Autoencoder (q=0.95) | 0.92 | 0.9060 | 0.7602 | 0.7781 | 0.7431 |
