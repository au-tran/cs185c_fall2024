How does tsunamis behave as they traverse different seafloor topographies?

In this project, I will investigate the behavior of tsunamis as they traverses across different seafloor topographies. In particular, I will investigate the changes in the tsunami's speed and amplitude as it approaches the coast of Japan versus when it heads off into the deep Pacific Ocean. I will construct a model that encloses the coast of Japan, including the Sea of Japan, and a portion of the Pacific Ocean. I expect the tsunami to travels significantly slower but appears larger in amplitude as it approaches shores and the opposite for a tsunami that heads into deeper waters in the direction of the Pacific Ocean. I expect these results because the continental shelf and slope should exerts a frictional force on the incoming tsunami that forces it to slow down. 

For initial conditions, I will use the ECCO Version 5 Model in 2015.  I intend to simulate tsunamis in different parts of the oceans by raising the sea surface level and observing the effects. Similarly, I will construct boundary and external forcing conditions for this model from the ECCO Version 5 model output. To analyze the results, I will create a time series of the tsunami's velocity and amplitude in different parts of the ocean. For visualization, I will create a movie that displays the velocity and amplitude of the tsunami over time.

# Reproducing Model Results
The following steps outline how to construct the model files, configure and run the model, and assess the model results.

## Step 1: Create the Model Files
Several input files need to be created to run the model. Generate the following list of files using the notebooks indicated in paratheses:
  
  1. Model Grid (notebooks/Grid.ipynb)
  2. Bathymetry (notebooks/Bathymetry.ipynb)
  3. Initial Conditions (notebooks/Creating Initial Conditions.ipynb)
     
The model files should all be placed into the input directory.

### Note

I did attempt to make the boundary conditions and external forcing conditions work but introducing those into my model results in numerical instabilities with CALC_R_STAR that crashed my model. As a result, my successful model runs are from disabling those two packages. 

The Boundary Conditions were created and visualized in (notebooks/Creating Boundary Conditions.ipynb). The boundary condition files in particular were placed inside the input/obcs directory.
The External Forcing Conditions files are global data files downloaded from NASA ECCO drive Version 5/Alpha/input_forcing for the year 2015 and were placed in the input/exf directory. The visualized data can be seen in the (notebooks/Creating External Forcing Conditions.ipynb).

The data can be viewed in the notebooks located in notebooks/Creating External Forcing Conditions.ipynb.

## Step 2: Add files to the computing cluster
Once the input files have been created, the model files can be transferred to the computing cluster. Begin by cloning a copy of MITgcm into your scratch directory and make a folder for the configuration, .e.g.

mkdir MITgcm/configurations/tsunami

After, use the scp command to move the code, input, and namelist folder to the /scratch/[username]/MITgcm/configurations/tsunami directory.

## Step 3: Compile the model

When all the files have been correctly placed, compile the model by making a build directory inside of /scratch/[username]/MITgcm/configurations/tsunami directory. 
Move inside the build directory and runs the following lines:

export MPI_HOME=/home/[username]/.conda/envs/cs185c
../../../tools/genmake2 -of ../../../tools/build_options/linux_amd64_gfortran
-mods ../code -mpi
make depend
make

After the compilation model is complete and successful, move back into the tsunami directory and create three run directories.
run_no_tsunami, run_Japan_tsunami, and run_Pacific_tsunami.


## Step 4: Making the run directories
After the compilation model is complete and successful, move back into the tsunami directory and create three run directories.
run_no_tsunami, run_Japan_tsunami, and run_Pacific_tsunami.

Move into each of these 3 run directories. For each of them, make a diags folder. Move inside the diags folder and create 4 folder. They are
EtaN_5min_mean, EtaN_5min_snap, vel_5min_mean, vel_5min_snap.

## Step 5.1: Run model without tsunami
After creating the run directories and their diags folder, move into the run_no_tsunami folder.
Run the following lines to link the relevant files:

ln -s ../build/mitgcmuv .
ln -s ../namelist/* .
ln -s ../code/* .
ln -s ../input/* .

Afterwards, edit the data file and make sure the line pSurfInitFile line at &PARM05 is 

pSurfInitFile = 'ETAN_IC.bin'

Finally, submit the job script to run the model:
sbatch cs185c15.slm

## Step 5.2: Run model with Japan tsunami
Similar to the previous step, move into the run_Japan_tsunami folder and link the relevant files.
Edit the data file this time and make sure the pSurfInitFile line at &PARM05 is

pSurfInitFile = 'ETAN_Pacific_tsunami_IC.bin'

Finally, submit the job script to run the model:
sbatch cs185c15.slm

## Step 5.3: Run model with Pacific tsunami
Similar to the previous step, move into the run_Japan_tsunami folder and link the relevant files.
Edit the data file this time and make sure the pSurfInitFile line at &PARM05 is

pSurfInitFile = 'ETAN_Japan_tsunami_IC.bin'

Finally, submit the job script to run the model:
sbatch cs185c15.slm

## Step 6: Analyze the Model Results

In the notebooks/Analyzing the Model Results.ipynb file you will see my conclusion for my experiments. Although the scenarios I created were successfully simulated and my model ran, the results were inconclusive and unsatisfactory. 

