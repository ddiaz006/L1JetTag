# L1LLPTag
Scripts and code used for the ongoing Level 1 LLP tagging project for CMS.


# Setting up Conda
Conda will need to be set up in order to run any of the scripts, including launching jupyter notebooks. On the directory of your choice, run the following commands:
<pre>
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh

export PATH="$HOME/miniconda3/bin:$PATH"
conda config --set auto_activate_base false

</pre>

After installing conda, we need to set up a virtual environment containing the modules used in the scripts. To create, and subsequently, activate the virtual environment, run:

<pre>
conda env create -f environment.yml
conda activate L1JetTag
</pre>

Note inside the .yml file that the environment name will be L1JetTag.

# Reconstructing Jets:
The Ntuples containing particle data, i.e. events with particles, can be accessed using the grid through LXPLUS or LPC for instance. Once we can access the Ntuples, we need to extract the particle information and cluster them into jets with the `dataForgeScripts/dataForge.py` script. Such a file can be run using the following command with the corresponding arguments: 

`$ Python3 DataF.py </path/to/file> (using xrootd or another access mode> QCDpt30 30 50 0`

Order of the arguments is as follows:

path to file (using xrootd: `root://cmsxrootd.fnal.gov///store/...`)

tag = "QCDpt30" or "Stpt30" in this case. This is just a tag to be added to the name of the file.

ptCut = 30 (so, jets with Pt>30 GeV in this case).

trainPercent = 50 (50 % training data).

usePuppi = 0 (0 for pf, 1 for PUPPI).

# Removing Background Jets from Signal Sample: 
We have two main sources of background our data: all jets reconstructed from the QCD Ntuples and some jets reconstructed from our signal Ntuples. The latter ones represent jets where the LLP is not matched to a jet in the event as determined by the DeltaR value. The script `removeBackground.py` can help us remove such jets to create a true-signal dataset. For that, we need to input the .h5 files (test and train) generated by dataForge.py from signal Ntuples. We can do that by running:

`python3 removeBackground.py <path/train/filename.h5> <path/test/file.h5>`

# Training:
Add paths of the training files resulting from DataForge.py.

# ROC Curves:
Inside ROC.py, add paths of the testing data resulting from the DataForge.py.
