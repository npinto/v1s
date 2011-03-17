# create/activate virtualenv
virtualenv plos08_reprod
cd plos08_reprod
export PJT=$(pwd)
source bin/activate
mkdir src data

# -- phase 1: get caltech101 image set

cd $PJT/data
wget http://www.vision.caltech.edu/Image_Datasets/Caltech101/101_ObjectCategories.tar.gz
tar xzvf 101_ObjectCategories.tar.gz 

# -- phase 2: run 'v1s' (tarball)

# get v1s plos08 original source code
cd $PJT/src
wget http://pinto.scripts.mit.edu/uploads/Research/v1s-0.0.5.tar.gz
tar xzvf v1s-0.0.5.tar.gz

# assumption: gcc-4.2, PIL, numpy and scipy are installed (with atlas/blas support)

# get old pyml (compatible with v1s-0.0.5)
cd $PJT/src
wget http://downloads.sourceforge.net/project/pyml/pyml/0.7.0/PyML-0.7.0.tar.gz
tar xzvf PyML-0.7.0.tar.gz
cd PyML-0.7.0
CXX=g++-4.2 CC=gcc-4.2 python setup.py build
python setup.py install

# run v1s code
cd $PJT/src/v1s-0.0.5
python ./v1s_run.py params_simple.py $PJT/data/101_ObjectCategories

#All scores:
# (1)  57.42
# (2)  58.60
# (3)  58.67
# (4)  57.09
# (5)  57.15
# (6)  57.46
# (7)  57.97
# (8)  58.38
# (9)  58.29
# (10)  57.86
#--------------------------------------------------------------------------------
#10 Average: 57.89 (std=0.56, stderr=0.18)
#================================================================================


