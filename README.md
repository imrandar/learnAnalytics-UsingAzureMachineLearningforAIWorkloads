# Welcome

Welcome to the two day workshop on how to effectively use Azure Machine Learning. In these two days, we will focus on hands-on activities that develop proficiency in AI-oriented workflows leveraging Azure Machine Learning, the Team Data Science Process, Visual Studio Team Services, and Azure Container Services. These labs assume a introductory to intermediate knowledge of these services, and if this is not the case, then you should spend the time working through the pre-requisites.

# Pre-requisites

Pre-requisites can be found [here][prereq3.0]. Briefly, pre-requisites include the following:

## Resources

- The ability to create resources within an Azure subscription
- Familiarity with how to create resources in said subscription

## Azure Machine Learning Services

- Python Proficiency and familiarity with data science workloads and techniques.

## Version Control

- Familiarity with [Git](https://git-scm.com/) and a [Visual Studio Team Services Account](https://azure.microsoft.com/en-us/services/visual-studio-team-services/) to leverage collaborative features of Azure Machine Learning.

## Languages

- Python

# Goals

- Understand and use the Team Data Science Process (TDSP) to clearly define business goals and success criteria
- Use a code-repository system with the Azure Machine Learning Workbench using the TDSP structure
- Create an example environment
- Use the TDSP and AML for data acquisition and understanding
- Use the TDSP and AML for creating an experiment with a model and evaluation of models
- Use the TDSP and AML for deployment
- Use the TDSP and AML for project close-out and customer acceptance
- Execute Data preparation workflows and train your models on remote Data Science Virtual Machines (with or without GPUs) and HDInsight Clusters running Spark
- Manage and compare models with Azure Machine Learning
- Explore hyper-parameters on Spark using Azure Machine Learning
- Deploy and Consume a scoring service on Azure Container Service
- Collect and Analyze data from a scoring service in production to progress the data science lifecycle.

# Agenda

Please note: This is a rough agenda, and the schedule is subject to change pending class activities and interaction.

- Day 1
  - 9-11: Introduction and Context
  - 10-11: [Lab 1: Introduction to Team Data Science Process with Azure Machine Learning][lab01]
  - 11-12: [Lab 2: Comparing and Managing Models with Azure Machine Learning][lab02]
  - 12-1: Lunch
  - 1-2:20 [Lab 3: Behind the scenes: Docker images and Conda environments][lab03]
  - 2:30-3:50 [Lab 4: Executing a data engineering or model training workflow in a remote execution environment][lab04]
  - 4-5: Summary and White-board Discussion
- Day 2
  - 9-9:20: Review and Next Steps
  - 9:20-10:20: [Lab 5: Executing a neural network workflow remotely using GPUs][lab05]
  - 10:30-10:50: Introduction to Deployment and Context
  - 11-12:00: [Lab 6: Managing Models using Azure Machine Learning][lab06]
  - 12:00-1: Lunch
  - 1:00-1:50: [Lab 7: Deploying a scoring service to Azure Container Service (AKS)][lab07]
  - 2:00-2:50: [Lab 8: Consuming the final service][lab08]
  - 3:00-3:50: [Lab 9: Collect data from a scoring service][lab09]
  - 4:00-5:00: Q&A and Feedback for Pro AI Bootcamp


# Discussion Forum

We will also use a gitter forum for discussion. Please post comments and questions [here][gitter].

# Software Dependencies

These materials have been tested on Windows with:

- Docker Community Edition v`17.12.0-ce-win47 (15139)`
- Azure Machine Learning Workbench v`0.1.1712.18263`

[prereq3.0]: lab00-bootcamp-pre-requisites/0_README.md
[lab01]: lab01-tdsp_and_aml/0_README.md
[lab02]: lab02-compare_and_choose_models/0_README.md
[lab03]: lab03_manage_conda_envs_in_aml/0_README.md
[lab04]: lab04-execute_in_remote_environment/0_README.md
[lab05]: lab05-execute_remote_gpu/0_README.md
[lab06]: lab06-managing_models_with_aml/0_README.md
[lab07]: lab07-deploying_a_scoring_service_to_aks/0_README.md
[lab08]: lab08-consuming_a_scoring_service/0_README.md
[lab09]: lab09-collect_and_analyze_data_from_a_scoring_service/0_README.md
[lab10]: lab10-parallel_hyperparameter_sweeps_on_spark/0_README.md
[gitter]: https://gitter.im/LearnAI-Bootcamps

# Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.microsoft.com.

When you submit a pull request, a CLA-bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., label, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.
