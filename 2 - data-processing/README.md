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

* Loads the historical and delta datasets from data lake to DBFS. Key and secrets are stored and accessed via databricks
* Checks for the historical load status for the years 2012-2020 and sets Flag
* Loads Blockface into a dataframe and perform transformations
* Executes the transformations for the year 2012-2020 based on the Flag status captured in step 2.
* Executes the delta load transformations for the records based on the last processed date.




Car parking has been a major issue in urban areas worldwide. Most countries are facing issues related to the lack of parking places. With the increasing economic development and urbanisation, car ownerships are growing rapidly, which exacerbates the imbalance between parking supply and demand [1]. The Ministry of Public Security of China released data of car ownership nationwide in 2018, showing that the number of cars reached 240 million with an annual growth rate of 10.51%, but the total number of parking spaces was only 102.5 million including private and public parking spaces, which is lower than half of the total number of cars. Moreover, around 30% of the traffic congestion in Chongqing and Shanghai, major cities of China, is due to lack of car parking spaces [2]. This issue is mainly caused by ineffective parking management. According to the latest research report [3], the parking space utilisation rate of more than 90% of cities in China is <50%. With the limited areas in the cities, increasing parking area would not be a sustainable solution, but the implementation of efficient parking management would be a practical solution. The intelligent parking system is an essential part of efficient parking management. In intelligent parking system, the time-sensitive parking occupancy prediction will be of great significance for decision makers and city planners regarding parking.

The number of available parking spaces plays an important role in drivers’ decision-making processes regarding parking [4, 5]. According to Caicedo et al. [6], drivers that possess information on parking availability are 45% more successful in availing parking spaces than those without knowledge. Moreover, the parking occupancy prediction is helpful in transportation management and planning [7]. For instance, public agencies such as city traffic and planning departments use the predicted parking occupancy information to manage transportation demand and traffic congestion [8]. Parking facility managers and operators may foresee the parking system performance and carry out short- and long-term preventive strategic decisions to avoid system breakdowns [9]. On the other side, the parking occupancy prediction can help reduce traffic congestion and energy consumption [10]. According to a report [11], on an average, US drivers spend 17 h per year searching for parking spaces at a cost of $345 per driver incurred due to time consumption, fuel, and emissions. If an accurate prediction of parking availability


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

