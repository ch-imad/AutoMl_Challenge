
# ReadMe   
   <b>                            April 2016 version 4</b>

This directory contains a sample submission for the <a href="http://automl.chalearn.org/">ChaLearn AutoML challenge<a>

## Instructions

- The metadata file must be included and remain unchanged. 
- The directory structure and naming should remain unchanged.
- A subdirectory res/ should contain results (for result submission only)

- To create a valid submission, zip all the file with:
		zip -r zipfilename *
starting from this directory, i.e. do not zip the directory itself, just its contents.
The directory may also be zipped by turning on the flag 
zipme = True
in run.py

- For zipfilename use something explicit to remember the nature of its contents (algorithm name, code version, etc.)

### Note

This directory contains everything you need for either code or result submission. 
* To submit the code, you do not need to generate prediction results on your local machine, just submit the contents of this directory as a zip file.
* To submit results, first run the code on your local computer to generate a res/ subdirectory; then bundle everything (including run.py, metadata, lib/, and res/) and submit the zip file. This is what we want you to do the the hackathon.




<span style =" color:red">IMPORTANT: you do NOT need to bundle the data with your submission, just the code and the results.<span >

### Disclaimer:
<i>The authors decline responsibility for mistakes, incompleteness or lack of quality of the information provided. The authors intended not to use any copyrighted material, if not possible, to indicate the copyright of the respective object. The copyright for any material created by the authors is reserved. The authors intended not to violate any patent rights or, if not possible, to indicate the patents of the respective objects. The payment of royalties or other fees for use of methods, which may be protected by copyright patents, remains the responsibility of the users. 
ALL INFORMATION, SOFTWARE, DOCUMENTATION, AND DATA ARE PROVIDED "AS-IS" THE ORGANIZERS DISCLAIM ANY EXPRESSED OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE. IN NO EVENT SHALL ISABELLE GUYON AND/OR OTHER ORGANIZERS BE LIABLE FOR ANY SPECIAL, INDIRECT OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES WHATSOEVER ARISING OUT OF OR IN CONNECTION WITH THE USE OR PERFORMANCE OF SOFTWARE, DOCUMENTS, MATERIALS, PUBLICATIONS, OR INFORMATION MADE AVAILABLE FOR THE AUTOML CHALLENGE.</i>

