# Optimize Model Training with Azure Machine Learning

## Running Training Scripts as Command Jobs

To move from experimentation to production in **Azure Machine Learning**, you should convert your notebook-based code into clean, modular Python scripts. This means removing nonessential lines (such as print statements) and organizing your code into functions for maintainability and testability.

Before integrating scripts into production pipelines, you should test them locally—for example, by running them in the terminal of a compute instance in your **Azure Machine Learning workspace**.

To run a script as a **command job**, use the `command()` function from the **Azure Machine Learning Python SDK (v2)**. You'll configure the job by specifying:
- `code`: the folder containing your script,
- `command`: the script entry point,
- `environment`: a definition of the Python environment with required packages,
- `compute`: the compute target to run the job,
- `display_name`: the job's name,
- `experiment_name`: the logical group the job belongs to.

Once submitted, the job runs on the specified compute and is tracked in **Azure Machine Learning studio**, where jobs under the same `experiment_name` are grouped together.

You can make scripts more flexible by adding parameters using a library like `argparse`. These allow you to pass values (like dataset paths or hyperparameters) when submitting jobs using command.

## Tracking Training Jobs with MLflow

To track training runs, integrate **MLflow** into your training scripts. This allows you to log and review model parameters, metrics, and artifacts. There are two main approaches:
- Enable **autologging** with `mlflow.autolog()` for automatic tracking, or
- Use `mlflow.log_param`, `mlflow.log_metric`, and `mlflow.log_artifact` for custom logging.

To use MLflow during training, your job’s **environment** must include both `mlflow` and `azureml-mlflow` packages.

All metrics and logs from the script are stored automatically when the script is submitted as a job in **Azure Machine Learning**. These can be explored in the **Azure Machine Learning studio**, where metrics are attached to individual runs inside experiments.

You can also analyze MLflow runs programmatically from notebooks. By referencing an experiment name or ID, you can query and compare runs, even across multiple experiments. This is especially useful for comparing models trained by different users or in different project iterations.

## Perform hyperparameter tuning with Azure Machine Learning

The set of hyperparameter values tried during hyperparameter tuning is known as the search space. The definition of the range of possible values that can be chosen depends on the type of hyperparameter. 

Some hyperparameters require discrete values - in other words, you must select the value from a particular finite set of possibilities. You can define a search space for a discrete parameter using a Choice from a list. You can also select discrete values from any of the following discrete distributions: QUniform; QLogUniform; QNormal; QLogNormal. Some hyperparameters are continuous - in other words you can use any value along a scale, resulting in an infinite number of possibilities. To define a search space for these kinds of value, you can use any of the following distribution types: Uniform; LogUniform; Normal; LogNormal. To define a search space for hyperparameter tuning, create a dictionary with the appropriate parameter expression for each named hyperparameter.

The specific values used in a hyperparameter tuning run, or sweep job, depend on the type of sampling used. There are three main sampling methods available in Azure Machine Learning. Grid sampling can only be applied when all hyperparameters are discrete, and is used to try every possible combination of parameters in the search space. Random sampling is used to randomly select a value for each hyperparameter, which can be a mix of discrete and continuous values. Sobol is a type of random sampling that allows you to use a seed. When you add a seed, the sweep job can be reproduced, and the search space distribution is spread more evenly. Bayesian sampling chooses hyperparameter values based on the Bayesian optimization algorithm, which tries to select parameter combinations that will result in improved performance from the previous selection.

For you to find the best model, however, can be a never-ending conquest. You always have to consider whether it's worth the time and expense of testing new hyperparameter values to find a model that may perform better. When you configure a sweep job in Azure Machine Learning, you can also set a maximum number of trials. A more sophisticated approach may be to stop a sweep job when newer models don't produce significantly better results. To stop a sweep job based on the performance of the models, you can use an early termination policy. Whether you want to use an early termination policy may depend on the search space and sampling method you're working with. An early termination policy can be especially beneficial when working with continuous hyperparameters in your search space. Continuous hyperparameters present an unlimited number of possible values to choose from. There are two main parameters when you choose to use an early termination policy: evaluation_interval, delay_evaluation.

New models may continue to perform only slightly better than previous models. To determine the extent to which a model should perform better than previous trials, there are three options for early termination: You can use a bandit policy to stop a trial if the target performance metric underperforms the best trial so far by a specified margin. A median stopping policy abandons trials where the target performance metric is worse than the median of the running averages for all trials. A truncation selection policy cancels the lowest performing X% of trials at each evaluation interval based on the truncation_percentage value you specify for X.

To run a sweep job, you need to create a training script just the way you would do for any other training job, except that your script must: Include an argument for each hyperparameter you want to vary; Log the target performance metric with MLflow. You can monitor sweep jobs in Azure Machine Learning studio. The sweep job will initiate trials for each hyperparameter combination to be tried. For each trial, you can review all logged metrics.









