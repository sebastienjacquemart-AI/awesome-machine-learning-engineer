# Experiment with Azure Machine Learning

In Azure Machine Learning, you can streamline model development using **automated machine learning (AutoML)**. Instead of manually trying different configurations, AutoML automates model selection, preprocessing, and tuning.

Before running an experiment, you must upload your data and register it as a **MLTable** data asset in your **Azure Machine Learning** workspace. This defines the schema so AutoML can read it correctly.

AutoML automatically applies **scaling**, **normalization**, and **featurization**. You can customize featurization (e.g. imputation methods), or turn it off entirely. After training, AutoML provides a summary of what transformations were applied and flags any data issues, such as missing values or class imbalance.

By default, AutoML will randomly select from the full range of algorithms for the specified task. You can choose to block individual algorithms from being selected. AutoML will try various combinations of featurization and algorithms to train a machine learning model.

You need to define a **primary metric** (e.g. accuracy, AUC) that AutoML uses to identify the best model. You can also include or exclude specific algorithms, which is useful if your data or policies restrict their use.

To control resources, use `set_limits()` to configure timeouts, trial count, and **early termination**. You can run multiple trials in parallel using a **compute cluster**, adjusting the `max_concurrent_trials` value as needed.

When training is complete, results are shown in the **Azure Machine Learning studio**, sorted by the primary metric. If featurization is enabled, **data guardrails** are applied automatically—such as detection of class imbalance, missing values, and high-cardinality features.

To track experiments and ensure reproducibility, use **MLflow** in your notebooks. After configuring **Azure Machine Learning** as the tracking backend, you can log parameters, metrics, and artifacts. MLflow supports **autologging** for common libraries like XGBoost, or you can log data manually if needed.

Each MLflow run is grouped into an experiment. If no experiment is set, logs go to the default one named `Default`.



