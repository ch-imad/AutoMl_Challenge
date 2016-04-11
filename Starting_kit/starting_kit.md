
# <a href="https://competitions.codalab.org/competitions/2321">Automatic Machine Learning Challenge (AutoML) </a>Design the perfect machine learning .
### <a href=http://automl.chalearn.org/>ChaLearn</a>Automatic Machine Learning: Until Jan. 2016, $30,000 in prizes donated by Microsoft. 

## Introduction

AutoMl : 
![alt text](http://automl.chalearn.org/_/rsrc/1416778116983/home/NewSpiral.png?height=224&width=400 "Logo Title Text 1")

Be part of the exciting ChaLearn AutoML challenge (5 rounds until March, 2016, 30 datasets, $30,000 in prizes): design the perfect machine learning “black box” capable of performing all model selection and hyper-parameter tuning without any human intervention:
- Round 0: Preparation. 
- Round 1: Binary classification (Novice phase). 
- Round 2: Multiclass classification (Intermediate phase).
- Round 3: Multilabel classification (Advanced phase). 
- Round 4: Regression (Expert phase).
- Round 5: Everything (Master phase).

## General purpose functions


```python
import time
overall_start = time.time()         # <== Mark starting time
from sys import argv, path
import numpy as np
import gc
import os
import psutil
```

### Verbose mode 
<i> Recommended to keep verbose = True: shows various progression messages</i>



```python
verbose = True
```

### Debug level
- 0: run the code normally, using the time budget of the tasks
- 1: run the code normally, but limits the time to max_time
- 2: run everything, but do not train, generate random outputs in max_time
- 3: stop before the loop on datasets
- 4: just list the directories and program version


```python
debug_mode = 0
```

### Time budget
- Maximum time of training in seconds PER DATASET (there are 5 datasets). 
- The code should keep track of time spent and NOT exceed the time limit 
+ in the dataset "info" file, stored in D.info['time_budget'], see code below.
+ If debug >=1, you can decrease the maximum time (in sec) with this variable:


```python
max_time = 30 
```

### Maximum number of cycles, number of samples, and estimators
<i>Your training algorithm may be fast, so you may want to limit anyways the 
number of points on your learning curve (this is on a log scale, so each 
point uses twice as much time than the previous one.)
The original code was modified to do only a small "time trial" followed
by one single cycle. We can now also give a maximum number of estimators 
(base learners).</i>


```python
max_cycle = 1 
max_estimators = float('Inf')
max_samples = 50000

```

<i> Use this flag to enable zipping of your code submission</i>


```python
zipme = True 
```


```python
import datetime
the_date = datetime.datetime.now().strftime("%y-%m-%d-%H-%M")
submission_filename = '../automl_sample_submission_' + the_date
```

### I/O defaults
- If true, the previous res/ directory is not overwritten, it changes name


```python
save_previous_results = False
overwrite_output = True # save space
```

- Use default location for the input and output data:
- If no arguments to run.py are provided, this is where the data will be found
and the results written to. Change the root_dir to your local directory.


```python
root_dir = "/home/imad/Documents/Final/Starting_kit/"
default_input_dir = root_dir + "sample_input/" 
default_output_dir = root_dir + "scoring_input/res"
```

In  this version :
Solve the problems of consumption memory and speed up loading of data 


```python
version = 3.4
```

<i>Our directories
Note: On cadalab, there is an extra sub-directory called "program"
Keave this stuff "as is"</i>


```python
running_on_codalab = False
run_dir = os.path.abspath(".")
codalab_run_dir = os.path.join(run_dir, "program")
if os.path.isdir(codalab_run_dir): 
    run_dir=codalab_run_dir
    running_on_codalab = True
    print "Running on Codalab!"
lib_dir = os.path.join(run_dir, "lib")
res_dir = os.path.join(run_dir, "res")
path.append (run_dir)
path.append (lib_dir)
```

### Our libraries 


```python
from data_manager import DataManager # load/save data and get info about them
from models import MyAutoML          # example models from scikit learn
from data_manager import DataManager #load/save data and get info about them
from models import MyAutoML #example models from scikit learn
import data_io                       # general purpose input/output functions
from data_io import platform_score # save score  and platform information in csv file
from data_io import vprint #print only in verbose mode
from data_io import vprint           # print only in verbose mode


```

<i> Show library version and directory structure </i>


```python
 if debug_mode >= 4 or running_on_codalab: 
    data_io.show_version()
    data_io.show_dir(run_dir)
```

<i>Check whether everything went well (no time exceeded)</i>


```python
execution_success = True
```

<i> Get input and output directory names</i>


```python
input_dir = default_input_dir
output_dir = default_output_dir
```

<i> Move old results and create a new output directory </i> 


```python
if not(running_on_codalab) and save_previous_results:
    data_io.mvdir(output_dir, output_dir+'_'+the_date) 
data_io.mkdir(output_dir) 
```

<i> INVENTORY DATA (and sort dataset names alphabetically)</i>


```python
datanames = data_io.inventory_data(input_dir)
```

<i>DEBUG MODE: Show dataset list and STOP</i>


```python
if debug_mode>=3:
    data_io.show_io(input_dir, output_dir)
    print('\n****** Sample code version ' + str(version) + ' ******\n\n' + '========== DATASETS ==========\n')        	
    data_io.write_list(datanames)      
    datanames = [] # Do not proceed with learning and testing
```

### RESULT SUBMISSION (<span style="color:red">KEEP THIS</span>)
<i> Always keep this code to enable result submission of pre-calculated results
     deposited in the res/ subdirectory.</i>


```python
if len(datanames)>0:
    vprint( verbose,  "************************************************************************")
    vprint( verbose,  "****** Attempting to copy files (from res/) for RESULT submission ******")
    vprint( verbose,  "************************************************************************")
    datanames = data_io.copy_results(datanames, res_dir, output_dir, verbose) # DO NOT REMOVE!
    if not datanames: 
         vprint( verbose,  "[+] Success")
    else:
        vprint( verbose, "======== Some missing results on current datasets!")
        vprint( verbose, "======== Proceeding to train/test:\n")
```

    ************************************************************************
    ****** Attempting to copy files (from res/) for RESULT submission ******
    ************************************************************************
    [-] Missing 'test' result files for ada
    [-] Missing 'valid' result files for ada
    [-] Missing 'test' result files for arcene
    [-] Missing 'valid' result files for arcene
    ======== Some missing results on current datasets!
    ======== Proceeding to train/test:
    


<b>Initialize time</b>


```python
overall_time_budget = 0
time_left_over = 0
```

# Select the data


```python

```


```python
print sorted(datanames)
print "Length of Datanames",len(datanames)
basename = datanames[0] 
print ("************************Processing dataset " + basename.capitalize() + "************************")
```

    ['ada', 'arcene']
    Length of Datanames 2
    ************************Processing dataset Ada************************


###  Learning on a time budget:
<i>Keep track of time not to exceed your time budget. Time spent to inventory data neglected.
</i>


```python
start = time.time()
```

## Creating a data object with data, informations about it


```python
D = DataManager(basename, input_dir, replace_missing=True, filter_features=True, max_samples=max_samples, verbose=verbose)
vprint( verbose,  "[+] Size of uploaded data  %5.2f bytes" % data_io.total_size(D))
        
```

    Info file found : /home/imad/Documents/Final/Starting_kit/sample_input/ada/ada_public.info
    ========= Reading /home/imad/Documents/Final/Starting_kit/sample_input/ada/ada_feat.type
    [+] Success in  0.01 sec
    ========= Reading /home/imad/Documents/Final/Starting_kit/sample_input/ada/ada_train.data
    [+] Success in  0.02 sec
    ========= Reading /home/imad/Documents/Final/Starting_kit/sample_input/ada/ada_train.solution
    [+] Success in  0.01 sec
    ========= Reading /home/imad/Documents/Final/Starting_kit/sample_input/ada/ada_valid.data
    [+] Success in  0.00 sec
    ========= Reading /home/imad/Documents/Final/Starting_kit/sample_input/ada/ada_test.data
    [+] Success in  0.23 sec
    [+] Size of uploaded data  72.00 bytes


### Keeping track of time


```python
if debug_mode<1:    
    time_budget = D.info['time_budget']        # <== HERE IS THE TIME BUDGET!
else:
    time_budget = max_time
overall_time_budget = overall_time_budget + time_budget
vprint( verbose,  "[+] Cumulated time budget (all tasks so far)  %5.2f sec" % (overall_time_budget))
vprint( verbose,  "[+] Time budget for this task %5.2f sec" % time_budget)
time_spent = time.time() - start
vprint( verbose,  "[+] Remaining time after reading data %5.2f sec" % (time_budget-time_spent))

        
```

    [+] Cumulated time budget (all tasks so far)  200.00 sec
    [+] Time budget for this task 100.00 sec
    [+] Remaining time after reading data 99.62 sec


### <span style = "color : orange">Time budget exceeded


```python
if time_spent >= time_budget:
    vprint( verbose,  "[-] Sorry, time budget exceeded, skipping this task")
    execution_success = False
```

## Creating model 


```python
M = MyAutoML(D.info, verbose=False, debug_mode=debug_mode)
print M
```

    MyAutoML : 
    GradientBoostingClassifier(init=None, learning_rate=0.1, loss='deviance',
                  max_depth=3, max_features=None, max_leaf_nodes=None,
                  min_samples_leaf=1, min_samples_split=10,
                  min_weight_fraction_leaf=0.0, n_estimators=1, presort='auto',
                  random_state=1, subsample=1.0, verbose=False,
                  warm_start=False)


### Iterating over learning cycles and keeping track of time


```python
time_spent = time.time() - start
vprint( verbose,  "[+] Remaining time after building model %5.2f sec" % (time_budget-time_spent))        
```

    [+] Remaining time after building model 99.10 sec


### <span style = "color : orange">Time budget exceeded


```python
if time_spent >= time_budget:
    vprint( verbose,  "[-] Sorry, time budget exceeded, skipping this task")
    execution_success = False
    
```

- Remove time spent so far
- Reset the counter
- Initialize time spent learning


```python
print time_budget
print time_spent
```

    100
    0.895457983017



```python
time_budget = time_budget - time_spent  
start = time.time()                     
time_spent = 0                         
cycle = 0
```

## Preprocessing for prediction
<i>
- The model can also select its hyper-parameters based on other elements of info.

</i>


```python
while time_spent <= time_budget/2 and cycle <= max_cycle and M.model.n_estimators<max_estimators:
    vprint( verbose,  "=========== " + basename.capitalize() +" Training cycle " + str(cycle) +" ================") 
    # Estimate the number of base estimators
    # --------------------------------------
    if cycle==1 and max_cycle==1:
        # Directly use up all time left in one iteration
        n_estimators = M.model.n_estimators
        new_n_estimators = int((np.floor(time_left_over / time_spent) - 1 ) * n_estimators)
        if new_n_estimators<=n_estimators: break
        M.model.n_estimators = new_n_estimators
    else:
        # Make a learning curve by exponentially increasing the number of estimators
        M.model.n_estimators = int(np.exp2(cycle))
                
    M.model.n_estimators = min(max_estimators, M.model.n_estimators)
    vprint( verbose,  "[+] Number of estimators: %d" % (M.model.n_estimators))  
    last_n_estimators =  M.model.n_estimators 
    # Fit base estimators
    # -------------------
    M.fit(D.data['X_train'], D.data['Y_train']) 

    vprint( verbose,  "[+] Fitting success, time spent so far %5.2f sec" % (time.time() - start))
    vprint( verbose,  "[+] Size of trained model  %5.2f bytes" % data_io.total_size(M))
    # Make predictions
    # -----------------
    Y_valid = M.predict(D.data['X_valid'])
    Y_test = M.predict(D.data['X_test'])                         
    vprint( verbose,  "[+] Prediction success, time spent so far %5.2f sec" % (time.time() - start))
    # Write results
    # -------------
    if overwrite_output:
        filename_valid = basename + '_valid.predict'                
        filename_test = basename + '_test.predict'
    else:
        filename_valid = basename + '_valid_' + str(cycle).zfill(3) + '.predict'                
        filename_test = basename + '_test_' + str(cycle).zfill(3) + '.predict'                
    data_io.write(os.path.join(output_dir,filename_valid), Y_valid)
    data_io.write(os.path.join(output_dir,filename_test), Y_test)

    vprint( verbose,  "[+] Results saved, time spent so far %5.2f sec" % (time.time() - start))
    time_spent = time.time() - start 
    time_left_over = time_budget - time_spent
    vprint( verbose,  "[+] End cycle, time left %5.2f sec" % time_left_over)
    if time_left_over<=0: break
    cycle += 1

```

    =========== Ada Training cycle 0 ================
    [+] Number of estimators: 1
    [+] Fitting success, time spent so far  0.11 sec
    [+] Size of trained model  72.00 bytes
    [+] Prediction success, time spent so far  0.11 sec
    [+] Results saved, time spent so far  0.21 sec
    [+] End cycle, time left 98.89 sec
    =========== Ada Training cycle 1 ================
    [+] Number of estimators: 464
    [+] Fitting success, time spent so far  0.58 sec
    [+] Size of trained model  72.00 bytes
    [+] Prediction success, time spent so far  0.64 sec
    [+] Results saved, time spent so far  0.71 sec
    [+] End cycle, time left 98.39 sec


Clean up


```python
del D
del M
gc.collect()
```




    10



Save score and Platform information in csv file 


```python
process = psutil.Process(os.getpid())
mem_used = process.memory_info().rss
data_io.platform_score( basename ,mem_used,last_n_estimators , time_spent, overall_time_budget)
```

All result files should be formatted as text files ending with a ".predict" extension, with one result per sample per line, in the order of the samples:
- Regression problems: one numeric value per line.
- Binary classification problems: one numeric value between 0 and 1 to per line, indicating a score of class 1 membership (1 is certainty of class 1, 0.5 is a random guess, 0 is certainty of class 0).
- Multiclass or multilabel problems: for C classes, C numeric values between 0 and 1 per line, indicating the scores of membership of the C classes. The scores add up to 1 for multiclass problems only.

We ask the participants to test their models regularly and produce intermediate prediction results, numbered from num=0 to n. The following naming convention of the files should be respected:
    [basename]_[setname]_[num].predict
where "basename" is the dataset name (e.g. adult, cadata, digits, dorothea, or newsgroups, in the first round), "setname" is either "valid" (validation set) or "test" (test set) and "num" is the order number of prediction results submitted. Please use the format 03d to number your submissions because we sort the file names in alphabetical order to determine the result order.

### Overall time spent


```python
overall_time_spent = time.time() - overall_start
if execution_success:
    vprint( verbose,  "[+] Done")
    vprint( verbose,  "[+] Overall time spent %5.2f sec " % overall_time_spent + "::  Overall time budget %5.2f sec" % overall_time_budget)
else:
    vprint( verbose,  "[-] Done, but some tasks aborted because time limit exceeded")
    vprint( verbose,  "[-] Overall time spent %5.2f sec " % overall_time_spent + " > Overall time budget %5.2f sec" % overall_time_budget)
              
```

    [+] Done
    [+] Overall time spent 56.36 sec ::  Overall time budget 200.00 sec


# Submission
<i> This is a challenge with code submission: your code will be executed automatically on our servers to train and test your learning machines with unknown datasets. 
However, there is NO OBLIGATION TO SUBMIT CODE. Half of the prizes can be won by just submitting prediction results. There are six rounds (Prep, Novice, Intermediate, Advanced, Expert, and Master) in which datasets of progressive difficulty are introduced (5 per round). There is NO PREREQUISITE TO PARTICIPATE IN PREVIOUS ROUNDS to enter a new round. The rounds alternate AutoML phases in which submitted code is "blind tested" in limited time on our platform, using datasets you have never seen before, and Tweakathon phases giving you time to improve your methods by tweaking them on those datasets and running them on your own systems (without computational resource limitation).</i>


### ZIP your results and code
<i>You can create a code submission archive, ready to submit, with zipme = True.
This is meant to be used on your LOCAL server.</i>


```python
if zipme:
        vprint( verbose,  "========= Zipping this directory to prepare for submit ==============")
        data_io.zipdir(submission_filename + '.zip', ".")
```

    ========= Zipping this directory to prepare for submit ==============


## Fetch the results and load it in pandas


```python
#!pip install pandas # if you don't have it, or pip3 for python3
```


```python
import pandas as pd
```


```python
data = pd.read_csv("performance.csv")
```


```python
data
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Data name</th>
      <th>Nb estimators</th>
      <th>System</th>
      <th>Machine</th>
      <th>Platform</th>
      <th>memory used (Mb)</th>
      <th>number of CPU</th>
      <th>Time spent (sec)</th>
      <th>Overall time budget (sec)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>arcene</td>
      <td>895</td>
      <td>Linux</td>
      <td>x86_64</td>
      <td>Linux-4.2.0-27-generic-x86_64-with-debian-jess...</td>
      <td>94371840</td>
      <td>4</td>
      <td>0.278357</td>
      <td>100</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ada</td>
      <td>464</td>
      <td>Linux</td>
      <td>x86_64</td>
      <td>Linux-4.2.0-27-generic-x86_64-with-debian-jess...</td>
      <td>116383744</td>
      <td>4</td>
      <td>0.714260</td>
      <td>200</td>
    </tr>
  </tbody>
</table>
</div>


