# Abaqus Licence Checker

Simple bash script to run an abaqus job via a SLURM scheduler when enough licences are available. 

Useful for scheduling SLURM HPC jobs that require abaqus licence tokens. Only tested on the Monash M3 HPC cluster configuration currently. 

# Installation

## Linux

1. Download the abq_licence_checker file and place it in a convenient location. I use: `~/abq_licence_checker\abq_licence_checker`
2. Create a file named `abaquslm.lic` in the home directory
3. Add the following to the `abaquslm.lic` file:

        SERVER <NAME/IP ADDRESS OF LICENCE SERVER> ANY <PORT>
        USE_SERVER
    If you do not know what these are, run the command `abaqus licensing lmstat`, which should create a printout of the software licenses currently connected.\
    \
    The printout should have an entry that looks something like:

        License server status: <PORT>@<NAME/IP ADDRESS>
            License file(s) on <NAME>
            ...
            ABAQUSLM: UP <VERSION>

    The `<PORT>@<NAME/IP_ADDRESS>` values should be placed in the file.

4. Add the following environment variables to `~/.bashrc`

    - `ABAQUSLM_LICENSE_FILE="~/abaquslm.lic"`
    - `ABQ_LICENSE_CHECKER_PATH="<PATH_TO_FILE>/<FILE_NAME>"`
    - `PATH="$PATH:<PATH_TO_FILE>"`

    
# Usage

In the directory with the SLURM submission script/abaqus job that you would like to run, enter the command:
    
    abq_licence_checker -s <SCRIPT_TO_RUN> -n <NUMBER_OF_CPUS>

If the abaqus analysis is coupled (In a FSI simulation for example) then the additional command line option `-c` can be appended to account for this in the number of licenses calculated. 

The script will continue to ping the licence server every 15 minutes, checking if there are enough abaqus licenses available to run the job. When enough are available, the SLURM job will be submitted. If the job has not been submitted after 20 tries, the script will close. 



