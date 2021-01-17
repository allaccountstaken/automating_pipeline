# Operationalizing Pipeline with AutoML

*NOTE: This project is part of the Udacity Azure ML Nanodegree. In this project, I create Microsoft Azure AutoML pipeline using two alternative approaches, a web-based UI and a Jyputer Notebook. Both paths allow for some degree of automation, but provide different user experience and level of customization.*

## Architectural Diagram
Generally speaking, AutoML process can be broken down into several fundamental steps, shown with the flowchart in the middle of architectural diagram below. Specific steps of the two paths are mapped to the logical steps.

![]()

The process starts with data ingestion, complexity of which eventually determines model configuration. Compute target configuration and setup are next. Experiment run, the third step, will produce numerous models. The best model needs to be selected according to a predefined selection criteria, for example testing accuracy score. The best model is registered and deployed. Once the model is deployed, endpoints become available for consumption. At the final step, consumed model can be used for inference remotely. 

## Key Steps - web-based UI
The top of the diagram UI path starts with uploading and validating the dataset. Once the dataset is registered on the platform, it becomes available for New AutoML Run. 
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

## Key Steps - Jyputer Notebook
Alternative path employs Jyputer Notebook for pipeline definition and control. First, python dependencies, workspace configuration and experiment variable are loaded. Second, AmlCompute cluster is created and the dataset is loaded. AutoMlCongig is defined to train the best classification model on target “y” with auto-fetuarization and early stopping. 
![](https://github.com/allaccountstaken/automating_pipeline/blob/main/imgs/7.1.png)
Pipeline with AutoMLStep is used to control the flow of iterative experiment runs with RunDetails.Once the PipelineRun is complete, results become available for examination. The best model can also be retrieved, examined and tested. If performance is satisfactory, the pipeline can be published, what is roughly equivalent to model registration and deployment. 
![](https://github.com/allaccountstaken/automating_pipeline/blob/main/imgs/7.4.png)
![](https://github.com/allaccountstaken/automating_pipeline/blob/main/imgs/7.5.1.png)
At the end of this process, REST endpoints become available. 
![](https://github.com/allaccountstaken/automating_pipeline/blob/main/imgs/7.2.png)
![](https://github.com/allaccountstaken/automating_pipeline/blob/main/imgs/7.3.png)
Endpoint can be used with requests for JSON payload transmission. 
![](https://github.com/allaccountstaken/automating_pipeline/blob/main/imgs/7.6.png)
Published pipeline run is presented below:
![](https://github.com/allaccountstaken/automating_pipeline/blob/main/imgs/7.5.2.png)

## Screen Recording
*TODO Provide a link to a screen recording of the project in action. Remember that the screencast should demonstrate:*

## Standout Suggestions
*TODO:
Fix authentication by using enterprise edition
Use Different model
Run everything from the python scripts using SDK only
TODO (Optional): This is where you can provide information about any standout suggestions that you have attempted.*
