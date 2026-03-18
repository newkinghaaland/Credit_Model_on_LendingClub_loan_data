# LendingClub Credit Grade Modeling (Random Forest)

本项目使用 LendingClub 贷款数据进行信用等级（`grade`）预测，当前主流程精简为 3 个 notebook：

- [01_data_cleaning.ipynb](01_data_cleaning.ipynb): 数据清洗与基础筛选
- [02_feature_encoding_and_dimensionality_reduction.ipynb](02_feature_encoding_and_dimensionality_reduction.ipynb): 编码与特征处理
- [03_random_forest_training.ipynb](03_random_forest_training.ipynb): 随机森林训练、特征删减、轻量调优

## Project Structure

- [01_data_cleaning.ipynb](01_data_cleaning.ipynb)
- [02_feature_encoding_and_dimensionality_reduction.ipynb](02_feature_encoding_and_dimensionality_reduction.ipynb)
- [03_random_forest_training.ipynb](03_random_forest_training.ipynb)
- [loan.csv](loan.csv): 原始数据
- `loan.csv` 可从该网址获取：https://blog.csdn.net/ni_ming_zhe/article/details/109578015
- [loan_filtered.csv](loan_filtered.csv): 清洗后数据
- [loan_encoded.csv](loan_encoded.csv): 编码后训练数据（03 默认输入）
- [requirements.txt](requirements.txt)

## Environment Setup

推荐 Python 3.10+。

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

## Recommended Run Order

按以下顺序运行：

1. [01_data_cleaning.ipynb](01_data_cleaning.ipynb)
2. [02_feature_encoding_and_dimensionality_reduction.ipynb](02_feature_encoding_and_dimensionality_reduction.ipynb)
3. [03_random_forest_training.ipynb](03_random_forest_training.ipynb)

说明：

- 第 03 个 notebook 内已包含基线模型、特征删减（ablation）。
- 如果只复现最终建模，可直接运行 [03_random_forest_training.ipynb](03_random_forest_training.ipynb)（前提是 [loan_encoded.csv](loan_encoded.csv) 已存在）。

## 03 Notebook Workflow

[03_random_forest_training.ipynb](03_random_forest_training.ipynb) 当前包含以下阶段：

1. 基线随机森林（分层切分 + 5 折交叉验证）
2. 自动特征删减（先 20 万分层样本做 ablation，再全量复核）
3. 改进模型评估（测试集指标、混淆矩阵、分类报告）
4. 学习曲线与参数范围收窄建议

## Current Best Result (Example)

近期实验中，改进模型相对基线有明显提升：

- `test_accuracy`: 0.7446 -> 0.8155
- `test_f1_weighted`: 0.7411 -> 0.8138
- `cv_accuracy_mean`: 0.7373 -> 0.8071
- `cv_f1_mean`: 0.7335 -> 0.8052

注：具体数值会随随机种子、运行顺序、环境差异有小幅波动。

## Notes

- 随机森林对标准化通常不敏感，本项目主线不依赖 normalized 数据。
- 在 4 核 16GB 环境下，建议保持外层交叉验证串行、模型内部并行为 1-2 线程。

## Reproducibility

为了更稳定复现结果，建议：

1. 固定 `random_state`
2. 保持相同训练/测试切分方式（分层切分）
3. 记录每轮实验的参数与指标对比表