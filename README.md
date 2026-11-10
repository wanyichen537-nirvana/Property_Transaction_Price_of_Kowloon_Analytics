# Background Information

This project contains a file hw.csv which contains the Property Transaction price of Kowloon between Nov 2014 to Nov 2020. The data was extracted from the "District
Latest Deals" from https://data.28hse.com/en/

# Project Workflow
1. **Import and cleaning data:** Import the data to a pandas dataframe from the data file, clean up the data, check and
handle missing values, rename column to a more meaningful name, etc.
2. **Merge Data:** Get another dataset and merge them with this data set. 
3. **Exploratory Data Analysis:** Do exploratory data analysis and do visualization.
4. **Learning model:**Apply multiple statistical learning / machine learning to determine what kind of factors impact the property price (distirct / area / floor),which included training vs testing data split, training model. Evaluate and compare the model.
5. **Findings and Conclusion:** Show the summary of interesting findings and conclusion of analysis

# Kowloon Residential Property Transaction Price Analysis / 九龙住宅物业成交价格分析

This project explores residential property transactions in Kowloon using Python and pandas. It cleans transaction records, combines them with annual Hong Kong economic indicators, summarizes price and transaction patterns, and fits an exploratory ordinary least squares (OLS) model.

本项目使用 Python 和 pandas 探索九龙住宅物业成交记录，包括清洗成交数据、合并香港年度经济指标、汇总价格与成交量特征，并拟合探索性的普通最小二乘（OLS）模型。

The transaction dataset contains **80,000 records**, covering **27 October 2014 to 27 November 2020**. It includes 32 district names and 2,009 estate names. The project is a historical analysis of this dataset; it does not describe current market conditions.

成交数据集共 **80,000 条记录**，时间范围为 **2014 年 10 月 27 日至 2020 年 11 月 27 日**，包含 32 个地区名称和 2,009 个屋苑名称。本项目分析的是这段历史数据，不代表当前市场情况。

## Questions explored / 探索的问题

- How did transaction prices and transaction counts vary by year, quarter, and calendar month?

  成交价格和成交笔数在不同年份、季度及日历月份之间有何差异？
- How did transaction volume, floor area, sale price, and price per square foot differ across districts and estates?

  不同地区和屋苑的成交量、面积、成交价及每平方呎价格有何差异？
- How were transaction characteristics associated with Hong Kong's annual economic indicators?

  成交特征与香港年度经济指标之间呈现怎样的关联？
- Could selected property and time variables explain variation in sale price in a simple linear model?

  在简单线性模型中，所选物业特征和时间变量能否解释成交价格的差异？

## Data sources / 数据来源

- `hw.csv`: Kowloon transaction records collected from the “District Latest Deals” pages on [28Hse](https://data.28hse.com/en/).

  `hw.csv`：来自 [28Hse](https://data.28hse.com/en/) “District Latest Deals”（地区最新成交）页面的九龙成交记录。
- `merge_data_hk.csv`: annual Hong Kong economic indicators. The notebook identifies the Census and Statistics Department as the source.

  `merge_data_hk.csv`：香港年度经济指标数据。Notebook 中注明数据来源为香港政府统计处。

The transaction rows include the sale date and price, estate and district names, saleable floor area (SFA), gross floor area (GFA), floor, and prior transaction information. The economic file contains annual measures such as GDP, GDP per capita and growth, unemployment, inflation, population, population growth, and birth rate.

成交记录包含成交日期和价格、屋苑及地区名称、实用面积（SFA）、建筑面积（GFA）、楼层以及过往成交信息。经济数据文件包含年度 GDP、人均 GDP 及其增长率、失业率、通胀率、人口、人口增长率和出生率等指标。

## Project files / 项目文件

| File / 文件 | Description / 说明 |
| --- | --- |
| `my_analysis.ipynb` | Main analysis notebook. Run this notebook for the current end-to-end workflow.<br>主分析 Notebook。项目当前的端到端分析流程以此文件为准。 |
| `hw.csv` | Source property transaction data.<br>原始物业成交数据。 |
| `merge_data_hk.csv` | Annual economic indicator data used by the main notebook.<br>主 Notebook 使用的年度经济指标数据。 |
| `Check_point_1.csv` – `Check_point_4.csv` | Intermediate data exports created at stages of cleaning, feature preparation, and the economic-data merge.<br>数据清洗、特征处理和经济数据合并等阶段导出的中间文件。 |
| `month_avg_price_per_district.csv` | District price summaries grouped by calendar month.<br>按日历月份汇总的地区价格数据。 |
| `month_avg_price_per_estate.csv` | Estate price summaries grouped by calendar month and district.<br>按日历月份和地区汇总的屋苑价格数据。 |

## Workflow / 分析流程

1. **Load and clean transactions.** Parse transaction dates, remove unused identifiers and descriptive fields, rename columns, remove formatting from area and price fields, convert holding periods to days, and extract numeric floor values.

   **载入并清洗成交数据。** 解析成交日期，删除无关标识和描述字段，统一列名，清理面积和价格字段的格式，将持有时间转换为天数，并提取数值楼层。
2. **Handle missing values and derive measures.** Fill missing prior-transaction flags, floor values, and percentage changes with zero. Fill missing SFA and GFA values with district averages; if an area remains unavailable, use the other area field. Calculate SFA and GFA price per square foot from sale price and area, then add holding time in years.

   **处理缺失值并构造指标。** 将缺失的过往成交标记、楼层和涨跌幅填为零。SFA 和 GFA 的缺失值先用地区均值填补；若仍缺失，则使用另一种面积字段补齐。再用成交价除以面积计算实用面积和建筑面积的每平方呎价格，并新增以年为单位的持有时间。
3. **Prepare time fields and merge economic data.** Derive year, month, and quarter from the transaction date, then join annual economic indicators by year.

   **构造时间字段并合并经济数据。** 从成交日期提取年份、月份和季度，再按年份合并年度经济指标。
4. **Aggregate and visualize.** Create district and estate price summaries; plot overall patterns and compare five selected districts and ten selected estates. The notebook also uses histograms, bar charts, box plots, scatter plots, and correlation heatmaps.

   **汇总并可视化。** 生成地区和屋苑价格汇总，绘制整体走势，并比较选定的 5 个地区和 10 个屋苑。Notebook 还使用直方图、柱状图、箱线图、散点图和相关性热图。
5. **Fit an exploratory OLS model.** The selected inputs are district ID, estate ID, SFA, SFA price per square foot, holding time, floor, year, and quarter. The notebook creates an 80/20 train/test split and fits OLS on the training portion.

   **拟合探索性 OLS 模型。** 模型选用地区 ID、屋苑 ID、SFA、SFA 每平方呎价格、持有时间、楼层、年份和季度作为输入。Notebook 按 80/20 划分训练集和测试集，并在训练集上拟合 OLS。

## Findings recorded in the notebook / Notebook 记录的发现

- The highest average sale price appears in **2018** when grouped by year, **Q2** when grouped by quarter, and **June** when grouped by calendar month. Recalculating from `Check_point_3.csv` gives **Q1** as the lowest-price quarter and **February** as the lowest-price month; the notebook caption says Q3, so that caption appears to be incorrect. These are separate marginal summaries; they do not identify a single year-quarter-month combination.

  按年份、季度和日历月份分别分组时，平均成交价最高的组别是 **2018 年**、**第二季度**和 **6 月**。根据 `Check_point_3.csv` 重新计算，平均成交价最低的季度是 **第一季度**、最低月份是 **2 月**；Notebook 图注写的是第三季度，因此图注可能有误。这些是分开统计的边际结果，不能据此确定某个具体的“年份—季度—月份”组合为最高点。
- Transaction counts are highest in **2015** by year, **Q2** by quarter, and **November** by month. The 2014 count is lower because the dataset begins late in that year.

  按年份、季度和月份分别统计时，成交笔数最多的组别是 **2015 年**、**第二季度**和 **11 月**。2014 年的记录较少，是因为数据从当年较晚时候开始。
- **Hung Hom** has the most transactions among districts (7,495); **Mei Foo Sun Chuen** has the most among estates (2,375).

  地区中成交笔数最多的是 **红磡**（7,495 笔）；屋苑中最多的是 **美孚新邨**（2,375 笔）。
- Average SFA is largest in **Shek Kip Mei** and smallest in **Wong Tai Sin**. Average GFA is largest in **Kowloon Station** and smallest in **Sham Shui Po**. These averages include imputed area values.

  平均 SFA 最大的地区是 **石硖尾**，最小的是 **黄大仙**；平均 GFA 最大的是 **九龙站**，最小的是 **深水埗**。这些平均值包含经过填补的面积数据。
- The OLS summary reports a training-set R² of **0.856**. The notebook reports positive coefficients for SFA, SFA price per square foot, and floor, and a negative coefficient for holding time; the quarter coefficient is not statistically significant in that fit.

  OLS 摘要中的训练集 R² 为 **0.856**。Notebook 报告 SFA、SFA 每平方呎价格和楼层的系数为正，持有时间的系数为负；在该模型中，季度项的系数没有达到统计显著。

These findings describe the supplied historical records and the calculations in the notebook. They should be read alongside the limitations below.

这些发现仅描述现有历史记录及 Notebook 中的计算结果，解读时应同时考虑下文列出的限制。

## Run the main notebook / 运行主 Notebook

Use Python with Jupyter and the libraries imported by the notebook:

使用 Python 和 Jupyter，并安装 Notebook 中导入的依赖库：

```bash
python -m pip install jupyter numpy pandas matplotlib seaborn plotly scikit-learn statsmodels
jupyter lab my_analysis.ipynb
```

Keep `hw.csv` and `merge_data_hk.csv` in the same directory as the notebook. The notebook writes its checkpoint and summary CSV files to the current working directory. The repository does not currently include a pinned dependency file or a tested environment specification.

请将 `hw.csv` 和 `merge_data_hk.csv` 放在 Notebook 所在目录。Notebook 会把检查点和汇总 CSV 文件写入当前工作目录。本项目目前没有固定版本的依赖清单，也没有经过验证的环境配置文件。

## Interpretation and data limitations / 结果解读与数据限制

- The district and estate summary files group by `date.dt.month` without year. For example, the January group pools January transactions from multiple years; it is not a month-by-month time series.

  地区和屋苑汇总文件按 `date.dt.month` 分组，但没有保留年份。例如，“1 月”组会合并多个年份的 1 月成交，因此它不是逐月时间序列。
- Missing SFA and GFA values are imputed. When both district-level estimates are unavailable, the remaining area is copied from the other field. This can affect area averages and derived prices per square foot.

  缺失的 SFA 和 GFA 值经过了填补；若地区均值仍不可用，则将另一种面积字段复制过来。这可能影响面积均值和由此计算的每平方呎价格。
- The notebook converts unparsed floor values to zero. As a result, floor zero can combine ground-floor records with missing or unrecognized floor values.

  Notebook 将无法解析的楼层填为零。因此，楼层为零的记录可能同时包含地面层、缺失楼层和无法识别的楼层。
- The OLS target is `Sell Price`, while `SFA New Price` is calculated as `Sell Price / SFA` and is also used as a predictor. This introduces target leakage, so the reported R² does not establish reliable out-of-sample prediction.

  OLS 的目标变量是 `Sell Price`，而 `SFA New Price` 是用 `Sell Price / SFA` 计算出来的，却也被用作预测变量。这造成了目标泄漏，因此报告的 R² 不能证明模型具有可靠的样本外预测能力。
- Although the notebook creates a test split, it does not report metrics on the held-out test rows. District and estate IDs are also treated as numeric values rather than categorical variables.

  虽然 Notebook 划分了测试集，但没有报告留出测试数据上的评估指标。此外，地区和屋苑 ID 被当作数值而不是类别变量处理。
- The notebook's “Top 20 Estates by Average SFA/GFA” code sorts ascending and selects the first 20 rows, which selects the smallest averages rather than the largest. Those charts and captions should be checked before reusing them.

  Notebook 中“平均 SFA/GFA 最高的 20 个屋苑”相关代码按升序排序后取前 20 行，实际选出的是均值较小的屋苑。再次使用这些图表和图注前应先核对。
- The economic indicators vary by year, while there are many property transactions per year. The merged scatter plots and correlations are exploratory and do not establish that economic indicators cause changes in property prices.

  经济指标按年变化，而每年包含许多笔物业成交。合并后的散点图和相关性分析仅用于探索，不能据此认定经济指标导致了房价变化。

This project is suited to historical market exploration and demonstrating a Python data-analysis workflow. Its historical coverage and modeling limitations define the scope of its use.

本项目适用于历史市场探索和展示 Python 数据分析流程；数据覆盖时段和模型局限决定了它的适用范围。
