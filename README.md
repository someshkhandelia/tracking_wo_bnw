
## Work taken from
 This repository is obtained by modifying code to work with CTMC dataset. The original respository is the code for the following publication:

```
  @InProceedings{tracktor_2019_ICCV,
  author = {Bergmann, Philipp and Meinhardt, Tim and Leal{-}Taix{\'{e}}, Laura},
  title = {Tracking Without Bells and Whistles},
  booktitle = {The IEEE International Conference on Computer Vision (ICCV)},
  month = {October},
  year = {2019}}
```

## Steps to make it work

### Clone this repository
Ensure that your current working directory right now is 
<pre>
tracking_wo_bnw/
</pre>

### Start a conda virtual environment
<pre>
conda create -n adl4cv python=3.8

conda activate adl4cv
</pre>

### Install pytorch related libraries
<pre>
pip3 install https://download.pytorch.org/whl/cu110/torch-1.7.1%2Bcu110-cp38-cp38-linux_x86_64.whl
pip3 install https://download.pytorch.org/whl/cu110/torchvision-0.8.2%2Bcu110-cp38-cp38-linux_x86_64.whl
pip3 install https://download.pytorch.org/whl/torchaudio-0.7.2-cp38-cp38-linux_x86_64.whl
</pre>

### Install requirements
<pre>
pip3 install -r requirements.txt
</pre>

### Install Tracktor
<pre>
pip3 install -e .
</pre>

### Download CTMC training data 
Download the data from [Link](https://ctmc-2022.s3.us-east-2.amazonaws.com/CTMC-train.zip)

Extract the zip file and keep all the sequence folders inside
<pre>
data/CTMC/train
</pre>

### Generate re-id data
The following command will create a 'reid' directory inside 'data/CTMC/train/' and several .json annotation files as well.
<pre>
python experiments/evaluation_tools/generate_reid_data_ctmc.py --dataset CTMC --data_root data
</pre>

### Install torchreid
To train a model using the data that we just generated, we use torchreid. 
Get out of current directory to one step back
<pre>
cd ..
</pre>
Clone the torchreid repo.
<pre>
git clone https://github.com/KaiyangZhou/deep-person-reid.git
</pre>
Ensure that when you run the 'ls' command in the present directory, you see 'tracking_wo_bnw/' and 'deep-person-reid/'.<br>
Get inside the cloned repo.
<pre>
cd deep-person-reid/
</pre>
Install requirements
<pre>
pip3 install -r requirements.txt
</pre>
Install torchreid
<pre>
python setup.py develop
</pre>

### Train reid network
First change directory back to our repo.
<pre>
cd ../tracking_wo_bnw/
</pre>

Train the network
<pre>
python experiments/scripts/train_reid.py --config-file "<PATH_TO_ROOT_DIR>/tracking_wo_bnw/experiments/cfgs/reid_ctmc_training.yaml"
</pre>

This will create a directory inside 'tensorboard' directory, namely 'tracktor/reid/CTMC_training/' where you shall find the tensorboard event files as well as the saved model checkpoints (model is saved after every epoch, as well as best model so far if in the current epoch) during the course of the training.
