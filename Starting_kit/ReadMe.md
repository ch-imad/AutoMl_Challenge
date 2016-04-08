
# ReadMe

### We call <a href="https://www.codalab.org/competitions/1381" >AutoML</a> the website URL of the challenge.                     
<i> All python exercises require the<a href="http://continuum.io/downloads" > Anaconda </a>distribution of Python 2.7 downloadable from 
This version was tested with: Python 2.7.8 | Anaconda 2.1.0 (x86_64).</i>

# Quick start

### Running the sample code on your local machine
- Copy the dataset in the directory: ../sample_input
- Update the root_directory link in run.py with your local directory
- Execute the code from your terminal : $python run.py

### Running the sample code with Jupyter Notebook
- Launch a the notebook from your terminal: $jupyter notebook starting_kit.ipynb
- Update the root_dir to link with your local directory: root_dir = "[...]/Starting_kit/"

Submit to the <a href="https://competitions.codalab.org/competitions/2321">Codalab platform </a> :

- sample_result_submission[n].zip
    - Or
- sample_code_submission[n].zip

where [n] is the round number.
### <span style="color : blue ">Consult Starting-kit.md to see the details of run.py execution</span> 
# Slower start

### Directories

Starting_kit/                                                   
├── README.txt                                    
├── sample_code_submission                                    
│   ├── lib                                    
│   │ ├── data_converter.py                                    
│   │ ├── data_io.py                                    
│   │ ├── data_manager.py                                    
│   │ └── models.py                                    
│   ├── metadata                                    
│   ├── performance.csv                                    
│   ├── README.md                                    
│   ├── run.py                                    
│   ├── starting_kit.html                                    
│   └── starting_kit.ipynb                                    
├── sample_input                                    
│   ├── ada                                    
│   │   ├── ada_public.info                                    
│   │   ├── ada_test.data                                    
│   │   ├── ada_train.data                                    
│   │   ├── ada_train.solution                                    
│   │   ├── ada_valid.data                                    
│   │   └── README.txt                                    
│   └── arcene                                    
│       ├── arcene_public.info                                    
│       ├── arcene_test.data                                    
│       ├── arcene_train.data                                    
│       ├── arcene_train.solution                                    
│       ├── arcene_valid.data                                    
│       └── README.txt                                    
├── sample_result_submission_0.zip                                    
├── sample_result_submission_1.zip                                    
├── sample_result_submission_2.zip                                    
├── sample_result_submission_3.zip                                    
├── scoring_input                                                                        
│   ├── README.txt                                    
│   ├── ref                                    
│   │   ├── ada_public.info                                    
│   │   ├── ada_valid.solution                                    
│   │   ├── arcene_public.info                                    
│   │   └── arcene_valid.solution                                    
│   └── res                                    
│       ├── ada_test.predict                                    
│       ├── ada_valid.predict                                    
│       ├── arcene_test.predict                                    
│       └── arcene_valid.predict                                    
├── scoring_output                                    
│   ├── scores.html                                    
│   └── scores.txt                                    
├── scoring_program                                    
│   ├── libscores.py                                    
│   ├── metadata                                    
│   ├── score copy.py                                    
│   └── score.py                                    
├── starting_kit.html                                    
└── starting_kit.md                                    


<i>Examples of datasets (they have been stripped out of their contents and replaced by a data matrix containing a single column; replace by the real datasets)</i>                  
sample_input/      
├── ada   
└── arcene


<i>Example of what the scoring program sees</i>                       
scoring_input/           
├── ref               
└── res


<i>Example of what the scoring program produces</i>                 
scoring_output/                 
├── scores.html         
└── scores.txt         


<i>The code of the scoring program</i>               
scoring_program/            
├── libscores.py       
├── score copy.py           
└── score.py

Zipped archives of sample_submission (keep a copy as is for reference):          
- sample_code_submission.zip (flag result_submission_only = False in run.py)         
- sample_result_submission.zip (flag result_submission_only = True in run.py)

# Step-by-step instructions

## 1) INSTRUCTIONS FOR SUBMITTING RESULTS ONLY (no code submission required)

#### Drill 1.1
<i>Submit the sample submission</i>
- Go to [AutoML]#participate-submit_results.
- Optionally enter a description of your submission, e.g. “test the sample result submission”.
- Click the “Submit” button.
- Locate file sample_result_submission.zip and upload it.
- Refresh until finished.
- Check the various file (log files, result file, etc.).
- Check the leaderboard in [AutoML]/#results.

#### Drill 1.2
<i>Make a new submission of RESULTS</i>
- Assume that you are not a python user and want to submit results without using Python and without submitting your code
- Unzip sample_result_submission.zip [make sure to keep a copy for further reference].
- Keep all the code and directory structure “AS IS”.
- Locate subdirectory “res”: this is where the results you will be submitting should be.
JUST REPLACE ITS CONTENTS BY YOUR OWN PREDICTIONS. 
- re-zip the contents of the directory (the contents only, not the directory itself) and re-submit (see drill 1.1).

#### Drill 1.3
<i>Run the scoring program</i>
- We provide the scoring program so you can examine it and eventually use it to self-evaluate your results. This is the exact same code we run on the server to evaluate your results.
- Open a terminal and change directory to [rootdir]/scoring_program/.
- At the prompt type:
	python score.py [rootdir]/scoring_input/ [rootdir]/scoring_output/
- Note that the contents of [rootdir]/scoring_output/ changed (by checking the time stamp of the file) and examine its contents.
- The example is designed to run with the sample data. For the challenge data, you do not have the truth values (solution) for the validation set and the test set. On the server, when you make result submissions, the contents of [rootdir]/sample_result_submission/program/res gets copied to [rootdir]/scoring_input/res and [rootdir]/scoring_input/ref contains the solutions (unknown to you).

## 2) INSTRUCTIONS FOR SUBMITTING CODE

#### Drill 2.1
<i>Submit the sample submission</i>
- Go to [AutoML]#participate-submit_results.
- Optionally enter a description of your submission, e.g. “test the sample code submission”.
- Click the “Submit” button.
- Locate file sample_code_submission.zip and upload it.
- Refresh until finished.
- Check the various file (log files, result file, etc.).
- Check the leaderboard in [AutoML]/#results.

#### Drill 2.2 
<i>Run the sample code to produce results</i>
- Open a terminal and change directory to sample_submission
- At the prompt type:
	python run.py [rootdir]/sample_input/ [rootdir]/sample_submission/program/res/
- Examine the time stamps of the files in output in program/res/ and see what changed
- Troubleshooting: if you encounter problems, change in run.py the debug flag:
debug = True
and run the program again. You will just get a listing of the directories and the library versions.
- To make changes to the code, keep the overall structure and modify only the files data_manager.py and models.py found in the [rootdir]/program/ directory.
- To make a new code submission, re-zip the contents of the directory (the contents only, not the directory itself) and re-submit (see drill #2.1).
The code is designed such that you can submit both results and code:
	* to submit results only, use the flag result_submission_only = True in run.py
	* to submit code only or both code and results, use the flag result_submission_only = False in run.py
There is no disadvantage to submitting both results and code. 

#### Drill 2.3            
<i>Run the scoring program</i>
- Run first drill 1.3.
- Copy the contents of [rootdir]/sample_result_submission/program/res to [rootdir]/scoring_input/res. 
- run again:
	python score.py [rootdir]/scoring_input/ [rootdir]/scoring_output/
Whatever new results you produced will now be scored in [rootdir]/scoring_output/, provided that you have the solutions for them in [rootdir]/scoring_input/ref. The example is designed to run with the sample data. For the challenge data, you do not have the truth values (solution) for the validation set and the test set. But you can use other datasets for which you have the solution or use the scoring functions for cross-validation by splitting the training data.


### <span style="color : red">*** *** *** *** *** *** *** IMPORTANT *** *** *** *** *** *** *** 
</span>

### PREREQUISITES to participate: 
	1) you do NOT need to use Python or know anything about it.
	2) you do not need to submit code.
However, to win prizes in the AutoML phases, you need to submit code and for the moment this is limited to Python code.
-  There is the same code in sample_code_submission.zip and sample_result_submission.zip, which allows you to conveniently submit both code and results or switch from one to the other:
you can use this template to submit your own Python code if you respect the interface or to just submit results (if you change nothing to the code).
-  Submissions can be made ONLY during TWEAKATON phases: your last submission of a given Tweakaton phase is automatically forwarded to the next Final and AutoML phases, and even to the next Twaekaton phase.
-  If you submit results only, you must use the sample code to do so, with the appropriate flag (result_submission_only = True in run.py). In the AutoML phase, it will be run, i.e. your AutoML results will be the baseline results. You must submit results BOTH on the validation set (‘valid’ data) and test set (‘test’ data).
-  If you submit code, you may also submit results. Use the appropriate flag (result_submission_only = False in run.py). In the Tweakaton and Final phases, your submitted results will be used for scoring, or, if you do not submit results, your code will be run to produce them. In the AutoML phase, your code is always run (because your eventual submitted results do not match the new data).
-  The code must produce results on BOTH on the validation set (‘valid’ data) and test set (‘test’ data).
-  Results submitted with code do not need to have been produced by the code submitted. In this way you can optimize your Final submissions by tweaking your methods to the datasets used during the previous Tweakaton. The code submitted however will be run during the next AutoML phase on new data, without human supervision.

<i>ALL INFORMATION, SOFTWARE, DOCUMENTATION, AND DATA ARE PROVIDED "AS-IS". 
ISABELLE GUYON, CHALEARN, AND/OR OTHER ORGANIZERS OR CODE AUTHORS DISCLAIM
ANY EXPRESSED OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED
WARRANTIES OF MERCHANTABILITY AND FITNESS FOR ANY PARTICULAR PURPOSE, AND THE
WARRANTY OF NON-INFRINGEMENT OF ANY THIRD PARTY'S INTELLECTUAL PROPERTY RIGHTS. 
IN NO EVENT SHALL ISABELLE GUYON AND/OR OTHER ORGANIZERS BE LIABLE FOR ANY SPECIAL, 
INDIRECT OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES WHATSOEVER ARISING OUT OF OR IN
CONNECTION WITH THE USE OR PERFORMANCE OF SOFTWARE, DOCUMENTS, MATERIALS, 
PUBLICATIONS, OR INFORMATION MADE AVAILABLE FOR THE CHALLENGE.</i>
