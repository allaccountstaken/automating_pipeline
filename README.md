# Operationalizing Pipeline with AutoML

*NOTE: This project is part of the Udacity Azure ML Nanodegree. In this project, I create Microsoft Azure AutoML pipeline using two alternative approaches, a web-based UI and a Jyputer Notebook. Both paths allow for some degree of automation, but provide different user experience and level of customization.*

## Architectural Diagram
Generally speaking, AutoML process can be broken down into several fundamental steps, shown with the flowchart in the middle of architectural diagram below. Specific steps of the two paths can be mapped to the logical steps.
![](https://github.com/allaccountstaken/automating_pipeline/blob/main/imgs/design.png)
The process starts with data ingestion, complexity of which eventually determines model configuration. Compute target configuration and setup are next. Experiment run, the third step, will produce numerous models. The best model needs to be selected according to a predefined selection criteria, for example testing accuracy score. The best model is registered and deployed. Once the model is deployed, endpoints become available for consumption. At the final step, consumed model can be used for inference remotely. 

## Key Steps for Visual AutoML Path
The top of the diagram path starts with uploading and validating the dataset. Once the dataset is registered on the platform, it becomes available for New AutoML Run. 
![](https://github.com/allaccountstaken/automating_pipeline/blob/main/imgs/2.1.png)
When configuring the run, new Experiment is created with Target column set to “y” and compute cluster set to Standard_DS12_V2. 
![](https://github.com/allaccountstaken/automating_pipeline/blob/main/imgs/2.2.png)
In my case, the experiment run took approximately 33 minutes and produced various models of different accuracy scores. 
![](https://github.com/allaccountstaken/automating_pipeline/blob/main/imgs/2.3.1.png)
The best model was VotingEnsemble with testing accuracy of 0.9214. This model was also registered and deployed using UI tools.
![](https://github.com/allaccountstaken/automating_pipeline/blob/main/imgs/2.3.2.png)
![](https://github.com/allaccountstaken/automating_pipeline/blob/main/imgs/2.3.3.png)
Once deployed, the model was assigned REST endpoint and Swagger URI. 
![](https://github.com/allaccountstaken/automating_pipeline/blob/main/imgs/3.1.png)
Application insights were enabled programmatically by running `logs.py` python script in the terminal. 
![](https://github.com/allaccountstaken/automating_pipeline/blob/main/imgs/4.1.png)
![](https://github.com/allaccountstaken/automating_pipeline/blob/main/imgs/4.2.png)
Swagger documentation was used to send POST and GET requests to the model for remote inference.  
![](https://github.com/allaccountstaken/automating_pipeline/blob/main/imgs/5.1.png)
![](https://github.com/allaccountstaken/automating_pipeline/blob/main/imgs/5.2.png)
Communication with the deployed model for inference is done in `endpoint.py` and takes the following form: {“result”:[“yes”, “no”]}
![](https://github.com/allaccountstaken/automating_pipeline/blob/main/imgs/6.1.png)

## Key Steps for Notebook AutoML Path
As briefly discussed above, high level machine learning workflow goes through data preparation, training, and deployment. These phases may have an arbitrary number of steps depending on the requirements and all steps could, at least in theory, be automated using pipelines. Data preparation, in this cases, simply requires dataset registration. Training includes AutoML object configuration and experiment run, as well as selecting the best model. Deployment includes registering selected model, scoring and monitoring. Endpoints allow continuing the process from a desired step on, replacing or modifying forthcoming steps.

First, python dependencies, workspace configuration and experiment variable are loaded. Second, AmlCompute cluster is created and the dataset is loaded. AutoMlCongig is defined to train the best classification model on target “y” with auto-fetuarization and early stopping. 
![](https://github.com/allaccountstaken/automating_pipeline/blob/main/imgs/create_pipeline.png)
![](https://github.com/allaccountstaken/automating_pipeline/blob/main/imgs/7.1.png)
Pipeline with AutoMLStep is used to control the flow of iterative experiment runs with RunDetails. 
![](https://github.com/allaccountstaken/automating_pipeline/blob/main/imgs/automlstep_results.png)
![](https://github.com/allaccountstaken/automating_pipeline/blob/main/imgs/pipeline_run.png)
Once the PipelineRun is complete, results become available for examination. The best model can also be retrieved, examined and tested. If performance is satisfactory, the pipeline can be published, i.e. equivalent to model registration and deployment. 
![](https://github.com/allaccountstaken/automating_pipeline/blob/main/imgs/7.4.png)
![](https://github.com/allaccountstaken/automating_pipeline/blob/main/imgs/7.5.1.png)
At the end of this process, REST endpoints become available. 
![](https://github.com/allaccountstaken/automating_pipeline/blob/main/imgs/7.2.png)
![](https://github.com/allaccountstaken/automating_pipeline/blob/main/imgs/7.3.png)
Endpoint can be used with requests for JSON payload transmission. 
![](https://github.com/allaccountstaken/automating_pipeline/blob/main/imgs/7.6.png)
Published pipeline run is presented below:
![](https://github.com/allaccountstaken/automating_pipeline/blob/main/imgs/7.5.2.png)

## Feedback Received
In order to address feedback on this project, Notebook was executed with the new compute target. This resulted in a new set of pipeline endpoints:

![](https://github.com/allaccountstaken/automating_pipeline/blob/main/imgs/new_endpoints.png)

Sending JSON payload was redone once again for a different pipeline, new run ID. 

![](https://github.com/allaccountstaken/automating_pipeline/blob/main/imgs/new_runid.png)

First, `requests` library is imported and POST message is sent with response status 200.

![](https://github.com/allaccountstaken/automating_pipeline/blob/main/imgs/new_JSON.png)

Calling `response.json().get(‘Id')` produces expected run ID as shown below.

![](https://github.com/allaccountstaken/automating_pipeline/blob/main/imgs/new_response.png)

Important, `RunDetails` can be used to monitor the status of the new run.

![](https://github.com/allaccountstaken/automating_pipeline/blob/main/imgs/new_rundetails.png)

## Improving the project
Performance of the model could be improved by training on a larger dataset or selecting a different primary performance metric. Additionally, oversampling techniques can help mitigate class imbalances.

Pipeline optimization may include more granular separation of steps and running them in parallel on different notes. Similarly, several models may be selected for different tasks, for example optimizing precision or recall, and consumed for inference via different endpoints.  

It was very difficult to complete the project in the slow Lab environment, so I used my personal Azure account. Authentication did not work properly, possibly because I was not able to upgrade my account to the enterprise level.
