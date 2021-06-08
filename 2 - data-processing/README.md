#### Data Processing

These scripts are used to extract and transform paid parking occupancy data for the year 2012 to present and blocface to create paid parking fact, date and blockface dimension.

All of the above are done in PySpark. 

## blockface_processing

#### `executeBlockface.py`
Pyspark Script to process the Seattle Blockface dataset

## occupancy_processing

#### `executeOccupancyProcess.py`
PySpark script for transforming historic and delta Paid Parking data from '2012 to present'.

## Data

#### `occupancy.json`
Metadata for the paid parking occupancy dataset in json file

#### `blockface.json`
Metadata for the blockface dataset in json file

## job_tracker

#### `job_tracker.py`
Python functions to connect to postgres table, insert job tracking records and get the status of historic and delta loads runs for Occupancy dataset

## utilities

#### `miscProcess.py`
Custom Helper functions for logging the etl execution to text files using spark dataframe

#### `processDataframeConfig.py`
Custom Helper functions for reading the configuration json file comprising the dataset details like columns, schema etc

#### `readEnvironmentParameters.py` 
Custom Helper functions for reading the configuration file.


## Driver Script

#### `occupancy_etl.py` 
Driver etl script to process the historical and delta load for Seattle Paid Parking Project. A Lambda architecture is applied, which processes the historical files once based on the status in the job tracking table and delta load on daily basis based on last processed date.

* Loads the historical and delta datasets from data lake to DBFS. Key and secrets are stored and accessed via databricks. Detailed steps to setup secret scope in databricks are in the document 'AzureADSetupforDatabricksSecretScope.docx'

[Reference](https://docs.microsoft.com/en-us/azure/databricks/security/secrets/secret-scopes

* Checks for the historical load status for the years 2012-2020 in Azure Postgres table and sets Flag.
* Loads Blockface into a dataframe and perform transformations.
* Executes the transformations for the year 2012-2020 based on the Flag status captured in step 2.
* Executes the delta load transformations for the records based on the last processed date.


#### Execute the ETL script and trigger the transformation on the datasets via command line

```
python occupancy_etl.py <caller_jobname> <log_filename> <spark_submit_mode>

```

### Create and install the .whl file to the cluster and call the below code in a notebook

```
from datetime import datetime
from dataset_processing.occupancy_etl import main
from dataset_processing import *

to= datetime.today()
caller_jobname='setup'
log_filename='testlog_'+str(to.day)+'0'+str(to.month)+''+str(to.year)+'.log'
spark_client_mode='N'

main(caller_jobname,
     log_filename,
     spark_client_mode)

```

# Screenshot

## Cluster 

![Alt text](../Documentation/ClusterScreenshot.PNG?raw=true "Cluster")

## Whl screenshot

![Alt text](../Documentation/whlscreenshot.PNG?raw=true "Whl screenshot")

## Whl execution

![Alt text](../Documentation/whlexecution.PNG?raw=true "whl execution")
