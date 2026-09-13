# Continuous Data Preprocessing Pipeline
flowchart TD
    A([📁 Raw Continuous CSV Dataset]) --> B[📦 1. Import Essential Libraries]
    B --> C[📥 2. Ingest Dataset & Separate Features / Target]
    C --> D{❓ 3. Check Missing Data}
    D -- Missing Values Found --> E[🛠️ Imputation / Removal Strategy]
    D -- Clean --> F[📊 4. Detect Outliers via IQR & Boxplots]
    E --> F
    F --> G[✂️ 5. Outlier Filtering / Trimming]
    G --> H[⚖️ 6. Split into Training & Testing Sets]
    H --> I([🚀 Ready for Model Training])

    classDef stage fill:#1e293b,stroke:#3b82f6,stroke-width:2px,color:#fff;
    classDef step fill:#0f172a,stroke:#64748b,stroke-width:1px,color:#e2e8f0;
    class A,I stage;
    class B,C,D,E,F,G,H step;


🧭 Step-by-Step Overview[1. Dataset Acquisition] ───► Place 'dataset.csv' with numeric features in root
         │
[2. Environment Setup]   ───► Load pandas, numpy, scikit-learn, and seaborn
         │
[3. Data Ingestion]      ───► Parse rows/columns; map X (predictors) & y (target)
         │
[4. Missing Data Scan]   ───► Count NaNs (df.isnull()) ──► Apply mean/median impute
         │
[5. Outlier Detection]   ───► Compute IQR (Q3 - Q1)    ──► Flag values beyond ±1.5×IQR
         │
[6. Data Partitioning]   ───► Split into 80% Train and 20% Test sets
📊 Summary of Stages & ObjectivesStepStageCore TechniqueObjective01Dataset GatheringLocal File StorageEnsure tabular numeric continuous data is accessible.02Library Setuppandas, numpy, sklearnLoad foundational numerical and modeling toolkits.03Data Loadingpd.read_csv()Load tabular records into memory and separate $X$ and $y$.04Missing ValuesSimpleImputer(strategy='mean')Prevent NaN errors without reducing sample size.05Outlier HandlingBoxplots / IQR ThresholdingRemove or cap extreme values distorting data variance.06Train/Test Split
