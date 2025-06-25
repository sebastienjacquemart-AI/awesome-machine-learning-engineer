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










