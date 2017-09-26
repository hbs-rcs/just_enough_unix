## LSF Commands

We provide a brief set of the most important commands one can use for working with 
the LSF (Load Sharing Facility) scheduler, the software that manages all work, both 
interactive and batch, that is done on the compute grid.


## TOC
* [Wrappers vs Custom Job Submission](#intro)
* [Submitting a Job With Custom Scripts](#custom_scripts)
* [Job Control](#job_control)
* [Information on Jobs](#job_info)
* [Useful LSF One Liners](#useful_commands)


Note: In the following sections, please know that where relevant, one can 
select / filter jobs by the following:

  `-q`: QUEUE_NAME<br>
  `-s`: STATE<br>
  `-J`: JOB_NAME<br>
  `-g`: JOB_GROUP<br>
  `-m`: HOST_NAME<br>
`-app`: APP_PROFILE<br>


<a name="job_submission"></a>
### Wrappers vs Custom Job Submission

HBS compute grid uses scripts to facilitate easy job submission and compute grid utilization.
These bash scripts take care of not only running the program you want, but also requesting the
resources -- the amount of RAM and number of CPUs -- that your program needs to run.

While easy & convenient, this is inflexible. For maximum flexibility, you can write your own
submission scripts to run your jobs. The next section presents some examples.

Default submission scripts are covered at http://grid.rcs.hbs.org/default-submission-scripts.


<a name="custom_scripts"></a>
### Submitting a Job With Custom Scripts

The `bsub` command allows you to submit jobs to the cluster scheduler, both background and 
interactive, and offers many options.

```bash
# submit a job script or a command with parameters
#  this uses defaults for time, RAM (100 MB), and CPUs (1 CPU)
bsub myjob.sh
bsub rsync -av ~/project/plan_B /export/projects/plan_B

# run an interactive shell (bash, 1 CPU, 500 MB RAM, 6 hour limit)
bsub -q interactive -Is -W 6:00 -R "rusage[mem=500]" /bin/bash
# run MATLAB interactively (1 CPU, 4 GB RAM, 6 hour limit)
bsub -q interactive -Is -W 6:00 -R "rusage[mem=4000]" matlab

# run a background copy job
bsub -q normal -W 6:00 -R "rusage[mem=4000]" cp myfile /to/some/path
# run MATLAB script in the background
bsub -q interactive -Is -W 6:00 -R "rusage[mem=4000]" matlab -r myscript.m

# a bunch of jobs with a single job name
bsub -J bunch_of_jobs myjob1
bsub -J bunch_of_jobs myjob2
bsub -J bunch_of_jobs myjob3

# Use bsub parameters that are inside the script file
#   These are #BSUB options at the top of the file
bsub < myjob.sh

# Let the command line options override things inside the job script
bsub -q normal < myjob.sh

```

Custom submission scripts are covered in more detail at http://grid.rcs.hbs.org/custom-submission-scripts.


<a name="job_control"></a>
### Job Control

Managing jobs often requires either the job ID, which is reported to you when you submit
the job with `bsub`, or using by job names or groups, though the latter is less common.

#### `bkill`: Killing Jobs
```bash
# killing a particular job
bkill JOB_ID

# kill all jobs in a given queue or all jobs with a given name
bkill -q QUEUE_NAME
bkill -J JOB_NAME

# kill all jobs (for myself). period.
bkill 0
```

#### `bsusp` and `bresume`: Suspending and Resuming Jobs

```bash
# suspend a particular job, my jobs in a given queue, with a given job name, or ALL jobs
bsusp JOB_ID
bsusp -q QUEUE_NAME
bsusp -J JOB_NAME
bsusp 0

# resuming a particular job, for a given queue, or with a job name
bresume JOB_ID
bresume -q QUEUE_NAME
bresume -J JOB_NAME
```


<a name="job_info"></a>
### Information on Jobs

Use the bjobs, bhist, and bacct commands to get current and historical information. This is
especially helpful to understand memory and CPU usage patterns, so that appropriate 
values can be given when submitting future jobs.

#### `bjobs`: Currently unfinished or < 1-hr finished

```bash
# list my jobs, one job, for a queue, a job name, a job group, or everyone's jobs
bjobs JOB_ID
bjobs -q QUEUE_NAME
bjobs -J JOB_NAME
bjobs -g JOB_GROUP
bjobs -u all -q QUEUE_NAME

# show data in wide columnar format
bjobs -w

# use a detailed, long format or unformatted job detail
bjobs -l 
bjobs -UF

# recently finished (1 hr), pending jobs & reasons, running
bjobs -d
bjobs -p
bjobs -r

# summary info about unfinished jobs
bjobs -sum
# resource use info
bjobs -W
```

#### `bhist`: Currently unfinished or past (anytime) finished/exited

```bash
# simple & long format
bhist JOB_ID
bhist -l JOB_ID

# all or submitted from a specific date range, or to now
bhist -a 
bhist -a -S 2017/02/06      # only that day
bhist -a -S 2017/02         # only that month
bhist -a -S 2017/02/06,2017/03/06   # the range
bhist -a -S 2017/02/06,     # then to now
bhist -a -S 2017/02/06, -l  # then to now, long format
```

#### `bacct`: Accounting (resource use) information on one or more jobs

```bash 
# simple or will long (full) details
bacct JOB_ID
bacct -l 143971

# using multiple Job_IDs means that the data will be summed
bacct 143971 145629
bacct -l 143971 145629
```

<a name="useful_commands"></a>
### Useful LSF One-Liners

```bash
# who has how many jobs running?
#
#   problem is that wide returns extra spaces, & these need to be counted as one delimiter
bjobs -w -u all | grep RUN | awk -F '[[:space:]]+' '{print $2}' | sort | uniq -c

# who has what going on?
#
bjobs -w -u all | awk -F '[[:space:]]+' '{print $2}' | sort | uniq -c

# how many people on the grid right now?
#
bjobs -w -u all | awk -F '[[:space:]]+' '{print $2}' | sort | uniq -c | wc -l

# what does their job data look like?
#   one job
#
bjobs -l 143971 | grep -E "User|g>|IDLE|MAX"

# what does their job data look like?
#   one user
#
bjobs -l -u jharvard | grep -E "g>|IDLE|MAX"

# what does their job data look like?
#   all users
#
bjobs -l -u all | grep -E "User|g>|IDLE|MAX"

# what does historic job info look like?
#   for myself
#
bhist -a -S 2017/09/01, -l | grep -E "User|g>|MAX"

## can format this more nicely using awk to piece out data from each line

```

