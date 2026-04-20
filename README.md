# consumer_product_safety_analysis

<a target="_blank" href="https://cookiecutter-data-science.drivendata.org/">
    <img src="https://img.shields.io/badge/CCDS-Project%20template-328F97?logo=cookiecutter" />
</a>

1. Summary <br>
The Consumer Safety Research Institute (CSRI) is a specialized fictional nonprofit organization established in 2012 to perform independent research on consumer product safety and publish reports and insights that defines public disclosure, and aids in policy decisions. The domain of consumer product safety is currently characterized by a significant disconnect between available data and public understanding. the prevailing public discourse frequently relies on anecdotal evidence, high-profile media incidents, or biased manufacturer marketing rather than systematic, empirical analysis. This reliance on incomplete information leaves consumers, policymakers, and safety advocates without the evidence-based insights necessary for informed decision-making. This information gap creates a critical challenge in an environment where stakeholders increasingly demand rigorous, independent verification to distinguish between isolated accidents and genuine systemic risks. There is a pressing market need for an authoritative intermediary capable of translating dense surveillance data into clear, actionable intelligence. <br>
The primary objective of the project is to develop predictive models to identify which consumer product injuries are most likely to result in hospitalization and determine what factors—product categories, demographics, injury characteristics—predict severe medical outcomes among emergency department cases, producing evidence-based findings that inform CSRI's 2025 Product Safety Spotlight report and contribute to public understanding of consumer product safety priorities. The secondary objective of the project is to Identify temporal and demographic trends in consumer product injury severity patterns, including seasonal variations, changes over time, and emerging patterns that may indicate shifting safety concerns, generate insights about demographic patterns in severe product injuries that inform targeted safety messaging, and create analytical approaches that enhance CSRI's reputation as a leading source of independent consumer safety research.<br>
Primary data science goal is to establish statistically significant correlations between factors contributing to physical injuries and hospitalization due to the use of consumer products with a high degree of confidence; secondary goal is to create interpretable models that quantify effect sizes for public communication. This dataset description document will explain in detail the datasets used for the modelling and any major analysis work of the project.<br>


2. Independent Variables and Dependent Variables<br>
Predictor Variables:<br>
Used Cramers' V rule to identify the strength of relationship of Independent variables Vs the target variable. Below were the independent variables used<br>

Target Variable:<br>
Through feature engineering, created a new feature "Hospitalization" from the disposition feature and used it as the traget variable.<br>

3. Challenges Faced:<br>
High Class Imbalance:<br>
The data downloaded from the  NEISS portal was inherently highly imbalanced in the ratio 9:1 (minority class to majority class) . A 9:1 class imbalance introduces fundamental challenges in predictive modeling, primarily because most standard machine learning algorithms are optimized to minimize overall global error. When 90% of the data belongs to a single class, the model is mathematically incentivized to prioritize the majority, often at the complete expense of the minority class.<br>
Data Volume:<br>
Processing a dataset of 7 million records (approximately 1GB) fundamentally shifts the operational focus of the Data Preparation and Modeling phases from basic functionality to strict computational efficiency. During Data Preparation, this volume requires careful memory management and the exclusive use of vectorized operations, as inefficient code can quickly balloon memory usage and create debilitating Input/Output bottlenecks that overwhelm standard hardware.<br>
Moving into the Modeling phase, the sheer scale of the data renders time-complex algorithms—like standard SVMs or K-Nearest Neighbors—virtually unusable and makes extensive hyperparameter tuning prohibitively expensive, forcing a reliance on highly scalable tree-based or linear models.<br>
High Feature Cardinality:<br>
The presence of high-cardinality categorical variables—such as a feature containing 750 distinct product codes—introduces critical complexities that heavily impact both the Data Preparation and Modeling phases. During Data Preparation, naive transformations like one-hot encoding needs be strictly avoided, as they trigger a massive expansion of the feature space (the "curse of dimensionality"), which drastically increases memory consumption and computational overhead. This forced  me to implement more sophisticated Target encoding to condense the categorical information without losing its signal.<br>
Harmonizing Product Taxonomy Changes:<br>
During the Data Preparation phase, the harmonization of shifting product taxonomies is a critical dependency for ensuring structural and historical consistency across the dataset. Because longitudinal data often contains evolving classification codes, deprecated categories, or fragmented sub-tiers, failing to map these disparate entries into a unified standard artificially fractures the underlying data signal and creates unnecessary sparsity.<br>

4. Data Split:<br>
A 70:15:15 stratified split was done on the dataset. The stratified split used a strtified key 
Year + Target Varibale to make sure that the class imbalance gets reflected in the training/test/holdout datasets.<br>

5. Training:<br>
We tried the below 4 estimators on the data.<br>
Logistic Regression <br>
Decision Tree<br>
Random Forest<br>
XGBoost <br>

6. Performance Evaluation:<br>
Model performance was evaluated utilizing PR-AUC and MCC metrics to ensure robustness on highly imbalanced data<br>

Primary Estimator: Decision Tree<br>
Secondary Estimator: XGBoost<br>

The Decision Tree was selected as the Primary Model because it achieved the highest MCC score, demonstrating superior balanced classification at the standard 0.50 operational threshold. Crucially, it provides the high interpretability required to translate mathematical findings into actionable safety recommendations. XGBoost was designated as the Secondary Model. While XGBoost delivered the highest overall PR-AUC—indicating strong general capability in ranking high-risk versus low-risk cases across all thresholds—its 'black box' architecture limits transparency. Furthermore, at the specific 0.50 operational cutoff, XGBoost generated more 'near miss ‘ classification errors (false positives and false negatives), which penalized its MCC score relative to the Decision Tree.<br>

Achieving an MCC score of 0.432, the predictive model demonstrates a reliable capability to identify the primary factors contributing to severe product-related injuries and hospitalizations. The model successfully isolates high-risk product categories and highlights critical demographic vulnerabilities. Armed with these statistically robust findings, CSRI can publish evidence-based insights—such as the 2025 Product Safety Spotlight report—that elevate public discourse. Ultimately, this output will help shape local policy discussions, inform manufacturer safety standards, and empower consumers to make data-backed decisions regarding consumer  product safety. The MCC of 0.43 that we got for the Decision Tree estimator reflects a deliberate, optimized trade-off between Precision and Recall. Given the business objective of identifying high-severity injuries, the threshold was calibrated to prioritize capturing True Positives (actual severe injuries), accepting a corresponding increase in False Positives. This trade-off prevents the metric from reaching the upper bounds but strictly aligns with our risk-mitigation goals. Because the model exhibits a moderate predictive correlation,  deployment strategies must account for a known margin of predictive error. The algorithm should not be utilized as an autonomous, definitive classifier for product recalls. Instead, it must be deployed as a 'prioritization engine' to flag high-risk product categories and vulnerable demographics for further human-in-the-loop review by safety analysts.<br>

Publishing this consumer product safety spotlight report would cost approximately 60 hours of editorial time but could establish the publication's credibility in data-driven reporting, potentially attracting new readers interested in evidence-based local journalism and demonstrating the organization's commitment to public service reporting.<br>

7. Feature Importance: <br>
The XGBoost model identified diagnosis type and body part affected as the strongest predictors of hospitalization. These features should guide clinical assessment and intervention strategies.<br>

8. Results Summary:<br>
8.1 Focus safety messaging: <br>
To translate our data insights into an actionable deployment strategy, I recommend initiating targeted safety messaging campaigns. First, interventions must prioritize the 65-and-older demographic to increase risk awareness, as this group exhibits the highest hospitalization frequency. Secondly, deployment should include public education initiatives highlighting the dangers associated with high-risk consumer products identified in the model, notably stairs, toilets, rugs, ladders, coins, and blankets.<br>

8.2 Execute safety campaigns:<br>
To translate the model's insights into an actionable Deployment strategy, interventions must be timed to align with seasonal risk trends. Because the data indicates that injury volumes consistently peak at 1.2 to 1.3 million monthly incidents from May through September, deployment efforts must prioritize this timeframe. I recommend scheduling targeted safety campaigns to coincide directly with this identified high-risk season to maximize preventative impact.<br>

8.3 Recommend product design improvements:<br>
To address our core objectives regarding risk mitigation, the analytical findings must directly influence future product development. Because the evaluation identified blankets, toilets, rugs, ladders, and stairs as high-severity variables, I  recommend prioritizing these categories for engineering redesigns. Integrating enhanced safety features into future builds will translate our data insights into tangible consumer protection.<br>

8.4 Develop Age specific guidelines:<br>
To effectively translate model insights into public safety improvements, our Deployment strategy mandates the creation of demographic-specific safety campaigns. By designing specialized guidance for the vulnerable populations identified in the data (specifically seniors and young children), we can optimize public awareness and foster safer consumer handling of targeted products.<br>


9. Limitations and caveats:<br>
NEISS exclusively captures injuries treated in US hospital emergency departments. The dataset inherently suffers from severity bias. It completely misses minor injuries treated at home, at primary care physicians, or in urgent care clinics, as well as fatal injuries where the patient died before reaching the hospital. NEISS records indicate that a consumer product was involved in an incident, not necessarily that the product caused the injury. NEISS is a stratified probability sample of roughly 100 hospitals, not a universal census. While national estimates (like the 1.2 to 1.3 million peak summer incidents) are generated using statistical weights, they carry an inherent margin of error.<br>

10. Model Risks:<br>
The Evaluation of the final model highlights two critical caveats regarding its real-world application. The algorithm may underperform when encountering out-of-vocabulary data, such as new products requiring novel classifications (e.g., electric scooters). Furthermore, because the model identifies statistical correlations rather than definitive causality, it may erroneously associate hospitalizations with completely safe products that were involved in an event but did not act as the root cause of the injury.<br>

11. Data Risks:<br>
If the data collection process changes and if certain fields start having more missing values – like narrative feature or diagnosis or product the model’s accuracy could drop below the acceptable thresholds.<br>

11. Ethical Risks:<br>
A critical ethical consideration governing this project is the socioeconomic bias embedded within the source data. By relying exclusively on Emergency Department visits, the dataset inherently underrepresents uninsured and lower-income populations who often avoid hospital-based care due to associated costs. To ensure equitable public safety reporting, our conclusions must transparently account for this disparity, acknowledging that the model is statistically predisposed to underestimate the true hazard levels of consumer products predominantly used by underinsured communities.<br>

12. Mitigation Approaches:<br>
To ensure the integrity of the Deployment phase, all model outputs and subsequent safety messaging must be governed by a strict operational framework. This requires mandating rigorous peer review for all published conclusions and embedding explicit uncertainty quantification (such as 95% confidence intervals) directly into the public reporting. Furthermore, the deployment strategy must include tailored community engagement protocols to accurately communicate these risk boundaries to the public, supported by formalized correction and retraction procedures to address any post-publication anomalies.<br>

## Project Organization

```
├── LICENSE            <- Open-source license if one is chosen
├── Makefile           <- Makefile with convenience commands like `make data` or `make train`
├── README.md          <- The top-level README for developers using this project.
├── data
│   ├── external       <- Data from third party sources.
│   ├── interim        <- Intermediate data that has been transformed.
│   ├── processed      <- The final, canonical data sets for modeling.
│   └── raw            <- The original, immutable data dump.
│
├── docs               <- A default mkdocs project; see www.mkdocs.org for details
│
├── models             <- Trained and serialized models, model predictions, or model summaries
│
├── notebooks          <- Jupyter notebooks. Naming convention is a number (for ordering),
│                         the creator's initials, and a short `-` delimited description, e.g.
│                         `1.0-jqp-initial-data-exploration`.
│
├── pyproject.toml     <- Project configuration file with package metadata for 
│                         cpsa and configuration for tools like black
│
├── references         <- Data dictionaries, manuals, and all other explanatory materials.
│
├── reports            <- Generated analysis as HTML, PDF, LaTeX, etc.
│   └── figures        <- Generated graphics and figures to be used in reporting
│
├── requirements.txt   <- The requirements file for reproducing the analysis environment, e.g.
│                         generated with `pip freeze > requirements.txt`
│
├── setup.cfg          <- Configuration file for flake8
│
└── cpsa   <- Source code for use in this project.
    │
    ├── __init__.py             <- Makes cpsa a Python module
    │
    ├── config.py               <- Store useful variables and configuration
    │
    ├── dataset.py              <- Scripts to download or generate data
    │
    ├── features.py             <- Code to create features for modeling
    │
    ├── modeling                
    │   ├── __init__.py 
    │   ├── predict.py          <- Code to run model inference with trained models          
    │   └── train.py            <- Code to train models
    │
    └── plots.py                <- Code to create visualizations
```

--------

