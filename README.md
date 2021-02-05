
# Operationalizing Machine Learning 

In this project, I worked on the Bank Marketing dataset using Azure ML studio, where I  configured a cloud-based machine learning production model, deploy it as a REST endpoint, and consumed it using HTTP, then created, published and consumed the pipeline.


## Architectural Diagram
![arch-diagram](images/Architectural-Diagram.png)

## Key Steps

1- Create a new Automated ML run, then select and upload the **Bank Marketing dataset** from [here](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv) or create it from web files using this link [https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv), I used the second method.


![register-dataset](/images/registered-dataset.PNG)


2- Configured a compute cluster(VM Size: Standard_DS2_V2, number of minimum nodes = 1, number of maximum nodes = 4, Exit criterion = 1, Concurrency = 5) then ran the experiment using *classification*.
The experiment took almost 30 minutes, then the status changed to **Completed**.

In Experiment section

![exp-copm](completed-in-exp-section.PNG)

In AutoML section

![run-completed](images/automl-exp-completed.PNG)


3- The AutoML run had different models as shown below, the models were ranked based on the **Accuracy** metric, and the best model is **VotingEnsemble** with accuracy of **0.91866**


![best-model](images/best-model.PNG)

![best-metrics](images/run-metrics-best-model.PNG)


4- Deploy the best model and enable the **Authentication**, you will see the deployment status as **succeeded** 


![best-model](images/best-moel-deployed.PNG)


Then I waited and had the deployment state as **Healthy** and got the **Swagger URI** and **REST endpoint**,


![healthy](images/healthy-deploy.PNG)


You can also notice that the **Application Insights** is false at this moment because I didn't enable them yet.


![app-insights-false](images/app-insights-false.PNG)


5- Here I want to enable the logging (enabling Application Insights), and this was done using `logs.py` script, and setting the value to `true` using this line of code `service.update(enable_app_insights = True)`.


![logs.py-script](images/logs-update.PNG)

![enable-logging-terminal](images/logs-1.PNG)
![enable-logging-terminal](images/logs-2.PNG)


Now we will have the application insights enabled as you can see, and also the **Application Insights URI** was generated.


![app-insights](images/app-insights.PNG)


6- In this step **Swagger** is used, I used `swagger.sh` to run a swagger docker image as it contains this command `docker run -p 80:8080 swaggerapi/swagger-ui` Swagger is now running on port 80.


7- `serve.py` script is used to start a python server on port 8000 as it serves the `swagger.json` on an HTTP server. 


![swagger-ui](images/swagger-ui.PNG)

![swagger-ui](images/post-swagger.PNG)

![swagger-ui](images/post-swagger-2.PNG)

![swagger-ui](images/post-swagger-3.PNG)


8- `endpoint.py` is used to consume/ interact with the deployed model, we update `endpoint.py` by modifying the `scoring_uri` with the **REST endpoint** and `key` with the **Primary key** that were generated after deployment.
Then the result will be returned as a JSON file.


![consume](images/consume-deploy.PNG)

![endpoint-file](/images/endpoint-update.PNG)

![endpoint-terminal](/images/endpoint.PNG)


9- Apache Benchmarking is used to benchmark the deployed model.


![bench-update](images/bench-update.PNG)

![bench-1](images/bench-1.PNG)
![bench-2](images/bench-2.PNG)
![bench-3](images/bench-3.PNG)
![bench-4](images/bench-4.PNG)


Then data will be saved as JSON file

![json-data](images/data-json.PNG)


10- Now this part of the project was done using Azure Python SDK to create, consume and publish a pipeline using the Jupyter notebook [aml-pipelines-with-automated-machine-learning-step](https://github.com/fati-ma/nd00333_AZMLND_C2/blob/master/aml-pipelines-with-automated-machine-learning-step%20(2).ipynb), and updated the key, URI's, cluster name, and experiemnt name.
**First** created a pipeline 


![create-pipeline](images/pipeline-created.PNG)

![create-pipeline](images/pipeline-created-2.PNG)


You can see the pipeline runs 


![pipelines-run](/images/pipleines-runs-last.PNG)


11- Here when the pipeline endpoint was created.


![pipeline-endpoint](images/pipeline-endpoint.PNG)


12- In the image below you can see the pipeline graph that shows the Bank Marketing dataset


13- After the pipeline is **published** we can see the **Published Pipeline Overview** where the status is *Active* and we have the *REST endpoints*


![pipeline-graph](images/pipeline-endpoint-graph.PNG)


14- This is the run details of pipeline endpoint run in the notebook using `RunDetails` widget


![run-details](images/runDetails.PNG)


## Screen Recording

This is the [screen record](https://drive.google.com/file/d/1HtMxSrD0viGXShNBoAexjr8OOINj7iR7/view?usp=sharing) of the project

## Standout Suggestions

I would try to increase the AutoML run time to test many more models and compare the results or usind different metrics to choose the best model, also the accuracy of the model can be improved by removing the class imbalance in the dataset.

