[![Build Status](https://travis-ci.org/Gaius-Augustus/Augustus.svg?branch=master)](https://travis-ci.org/Gaius-Augustus/Augustus)

# Gene Prediction with AUGUSTUS

[INTRODUCTION](#introduction)  
[DATASET](#dataset)
[BASIC](#basicuse)  
[ADVANCED](#advanceduse)  
[TODO LIST](#todolist)  
[LICENSES](#licenses)  

# INTRODUCTION

The present version of Augustus allows to run prediction over a minimal dataset to assess how changes in the code may have affected accuracy. Set TESTING = false in [common.mk](common.mk) if this feature is not required or the required libraries are not available. By default TESTING = true.

# DATASET

After extraction of only those data which are strictly need from original full length genomes, the size of FASTAs is dramatically reduced from 1-3 Gigas to a few Megas. Likewise, serialization of alignments retains only blocks which are strictly need from original MAFs. Reasonably, the overall size of a dataset containing around 300 genes from hg38.chr1 can be expected to fall in a range between 80 and 100 Mb. The dataset includes minimal FASTAs and serialized alignments. The dataset currently comes uncompressed. In addition to hg38, eleven more vertebrates belong to the clade. 

# BASIC

## Dependencies
Please make sure boost-serialization-dev is installed. If it is not the case, try and run:

```
sudo apt-get update
sudo apt-get install libboost-serialization-dev
```
In order to have accuracy returned, [EVAL](https://mblab.wustl.edu/software/download/eval-2.2.8.tar.gz) should be installed on your machine. Make sure it is the case and set eval_dir within script/executeTestCGP.py to eval path as follows:

```
eval_dir = /my_path_to_EVAL/
```

Running the python3 script executeTestCGP.py has the following software dependency:
  - Python3

## First use
Run the following commands from within scripts Augustus subdirectory:
```
python3 executeTestCGP.py --chunk 1 2 3 --run
```
in --run mode, the script runs prediction using minimal FASTAs (estimated running time 90 min c.ca when running in parallel, more details to come). Results are stored in example/cgp12way/outCHUNKrun, where CHUNK is a positive integer corresponding to the chunk under analysis.
```
python3 executeTestCGP.py --chunks 1 2 3 --eval 
```
in --eval mode, the script computes accuracy for some prediction obtained using minimal FASTAs during the execution of --run mode. Results are stored in example/cgp12way/ACCURACY. Chunks parameter can be set to be any list of chunks, provided data are available for them (e.g. serialization of alignments, SQlite db). So far I made available such data for chunks 1, 2 and 3.

## Optional test
The following command allows to run a basic test over the correctness of the code. The prediction obtained by the user in --run mode over minimal FASTAs should be identical to the original prediction carried out on full size FASTAs and included in directories named outCHUNKprediction. Obviously, the test is to be run before any change is made to the original code to ensure the same accuracy.
```
python3 executeTestCGP.py --chunk 1 2 3 --test
```

# ADVANCED
Todo add description of advanced use here

# TODO LIST
Here it follows a list of issues and corresponding progress:
  - parallelization (done)
  - code clean-up (pending)
  - make dataset unbiased (pending)
  - compress dataset (pending)

# LICENSES

All source code, i.e.
  - the AUGUSTUS source code (src/*.cc, include/*.hh)
  - the scripts (scripts/*.pl)
  - the auxiliary programs (auxprogs/)
  - the tree-parser (src/scanner,src/parser)
  
is under the [Artistic License](src/LICENSE.TXT).