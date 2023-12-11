# Disaster Response Pipeline Project
My second Udacity Project creates a machine learning pipeline to categorize disaster events so that the messages can be viewed by emergency workers and sent to the appropriate disaster relief agencies.

# Table of Contents

* [Installation](#Installation)
* [Project Motivation](#Project-Motivation)
* [File Descriptions and Analyses](#File-Descriptions-and-Analyses)
* [Results](#Results)
* [Licensing, Authors, and Acknowledgements](#Licensing,-Authors,-and-Acknowledgements)


## Installation <a name="Installation"></a>
The code should run with no issues using Python version 3.6.3 using Jupyter Notebook server version 5.7.0 in the Udacity Project Workspace.  Packages that were imported include Pandas,sqlite3, sqlalchemy create_engine, NumPy,  matplotlib.pyplot.  

## Project Motivation <a name="Project-Motivation"></a>
For this project, I used a data set from [Appen](https://appen.com/) (formally Figure 8) containing real messages that were sent during previous disaster events. 

A machine learning pipeline was created to categorize these events so that the messages could be sent to an appropriate disaster relief agency. This project included a web app where an emergency worker can input a new message and get classification results in several categories. The web app will also display three visualizations of the data. 

**_Project Components_**
 
**_1. ETL Pipeline_** *The Extract, Transform, and Load (ETL) process read the dataset, cleaned the data, and then stored it in a SQLite database.*  
**_2. Machine Learning Pipeline_** *The data was loaded from the SQLite database.  The data was then split  into a training set and a test set. A text processing and machine learning pipeline was built.  GridSearchCV was used to train and tune the model.  The results on the test set were outputted, and the model was exported to a pickle file*  
**_3. Flask Web App Deployment_** *The flask web app design was customised with html and css.  Data visualizations were created using Plotly in the web app*  



## File Descriptions and Analyses <a name="File-Descriptions-and-Analyses"></a>
**_ETL Pipeline Preparation_**
Two datasets ```messages.csv``` and ```categories.csv``` containing real messages that were sent during disaster events were merged using the common ```'id'```.  This combined dataset was assigned to ```df```which was then cleaned. The cateories were split into separate category columns on ```;``` with ```expand=True``` on split function which uses strings.  A lambda function was then used to take everything 
up to the second to last character of each string with slicing to get the 36 category columns titles.  Then to convert category values to just numbers 0 or 1, ```astype(str)``` then ```astype(int)```were applied to the last character of the string.  The columns were then the checked for binary and any'2' was replaced by '1'.  The categories column was replaced in df with new category columns by using ```drop```and ```concat```.  Duplicates were removed using ```drop_duplicates```.  Finally it was stored in a SQLite database ```DisasterResponse.db``` using the pandas dataframe ```.to_sql()``` method.



**_ML Data Preparation_**  
The SQLite database was loaded. A machine learning pipeline (MLP) was created that used NLTK, Pipeline and GridSearchCV to output a final model that uses the message column to predict classifications for 36 categories (multi-output classification). The data was split into a training set and a test set, trained and tested.  The model was exported to a pickle file ```classifier,pkl```. The final machine learning code was in the script ```train_classifier.py```. The script used a tokenize function using NLTK to case normalize, lemmatize, and tokenize text.  The function wass used in the MLP to vectorize and then apply TF-IDF to the text. ```MultiOutputClassifier(RandomForestClassifier())``` was also used for predicting multiple target variables.


## Results <a name="Results"></a>

**ETL Pipeline** *Python ETL script ```process_data.py``` which contained the data cleaning code ran in the project workspace and the terminal without errors.
To run ETL pipeline that cleans data and python models/train_classifier.py data/DisasterResponse.db models/classifier.pkl stores in database*
        ```python data/process_data.py data/disaster_messages.csv data/disaster_categories.csv data/DisasterResponse.db```
![image](https://github.com/nirvannar/DisasterResponsePipeline/assets/52913504/a5e47eec-3b28-4641-882f-9ff3b241b5b3)



**Machine Learning Pipeline** *The machine learning script, ```train_classifier.py``` ran in the project workspace and terminal without errors. The precision, recall and f1-score for each output category of the dataset was reported using ```classification_report```. The higher the scores the better the model.  Scores were 0.74, 0.44 and 0.50 respectively.  

After using GridSearchCV to find better parameters, scores were 0.74, 0.49 and 0.53 respectively in the project workspace.  The IDE scores were 0.76, 0.49 and 0.53 respectively for the same exercise. 

This dataset is imbalanced as shown on the web app visualisations below, as some labels like water, child_alone, fire and cold have few examples with f1-scores of zero, while related has many examples and fi-score of 0.88. The top 5 categories have high f1-scores.  However in the case where there are few examples of labels, the ML model will not be reflective of all labels and not as accurate as required.  To evaluate how well the model deals with identifying and predicting True Positives, we should measure precision and recall. The F-score is the harmonic mean of a system's precision and recall values.  A low F1 score is an indication of both poor precision and poor recall as can be illustrated by the scores of category labels with few examples such as offer, missing_people and fire which all had scores of zero for precision, recall and f1-score.  Practically the dataset needs to be bigger and encompass more examples of each category. In a real life situation this may lead to the aid being dispatched to where it is not needed thereby wasting resources (False Positive) reflected in precision, or even worse it may not get to the intended people requiring aid (False Negative) reflected in recall because the model was not trained in those examples adequately.*  

*To run ML pipeline that trains classifier and saves*
        ```python models/train_classifier.py data/DisasterResponse.db models/classifier.pkl``` 

![image](https://github.com/nirvannar/DisasterResponsePipeline/assets/52913504/2367181f-cbaa-417e-832b-a0bf040b7381)
![image](https://github.com/nirvannar/DisasterResponsePipeline/assets/52913504/076d47f3-cdbb-49da-9f8f-02a572c70b7a)
![image](https://github.com/nirvannar/DisasterResponsePipeline/assets/52913504/f903dfcf-514d-4496-a2bb-e74e7a29c15a)


**Flask Web App Deployment** *The web app ran without errors. When a user inputs a message into the app, the app returns classification results for all 36 categories as shown in the screenshots below.*  




Type in the command line Project Workspace IDE as below to run the Flask app.

```python
python run.py
```



![image](https://github.com/nirvannar/DisasterResponsePipeline/assets/52913504/1f5b13a0-9d57-44cc-8b6f-c0db43e7be5e)
![image](https://github.com/nirvannar/DisasterResponsePipeline/assets/52913504/36987932-4d63-42fc-a5f9-7209ed944b02)

![image](https://github.com/nirvannar/DisasterResponsePipeline/assets/52913504/1dff5d30-c10e-4b01-9582-2061d859d642)

The MLP visualisations and analyses of data suggested that messages could go to disaster relief organizations such as [The Red Cross](https://www.redcross.org/about-us/our-work/international-services/international-disasters-and-crises.html), [The International Rescue Committee](https://www.rescue.org/) and [the United Nations Office for the Coordination of Humanitarian Affairs](https://www.unocha.org/).




## Licensing, Authors, and Acknowledgements <a name="Licensing,-Authors,-and-Acknowledgements"></a>
The data was provided by [Appen](https://appen.com/).  [Scikit-learn.org](https://scikit-learn.org/), [Stack Overflow](https://stackoverflow.com/), [Plotly](https://plotly.com/graphing-libraries/), [Udacity](https://knowledge.udacity.com/?nanodegree=nd025&page=1&project=516&rubric=1565), [freeCodeCamp](https://www.freecodecamp.org/news/how-to-sort-values-in-pandas/), [Geeks for Geeks](https://www.geeksforgeeks.org/) and [Github](https://github.com/) were consulted for this project.  
