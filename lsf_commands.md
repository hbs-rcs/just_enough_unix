## LSF Commands

Know that where relevant, one can select / filter jobs by the following:

-q QUEUE_NAME<br>
-s STATE<br>
-J JOB_NAME<br>
-g JOB_GROUP<br>
-m HOST_NAME<br>
-app APP_PROFILE<br>

### Wrappers vs Custom Job Submission
HBS compute grid uses bash scripts to facilitate easy job submission and compute grid utilization.

### Submitting a Job
```bash
# submit a job script or a command with parameters
bsub myjob.sh
bsub rsync -av ~/project/plan_B /export/projects/plan_B

# a bunch of jobs with a single job name
bsub -J bunch_of_jobs myjob1
bsub -J bunch_of_jobs myjob2
bsub -J bunch_of_jobs myjob3

# Use sbatch parameters that are inside the script file
bsub < myjob.sh

# Let the command line options override things inside the job script
bsub -q normal < myjob.sh
```

### Job Control

```bash
# Killing a Job
bkill JOB_ID

# all jobs in a given queue or all jobs with a given name
bkill -q QUEUE_NAME
bkill -J JOB_NAME

# all jobs (for myself). period.
bkill 0

# Suspending/Resuming a Job
bsusp JOB_ID
bsusp -q QUEUE_NAME
bsusp -J JOB_NAME
bsusp 0

# Resuming a job
bresume JOB_ID
bresume -q QUEUE_NAME
bresume -J JOB_NAME
```

### Information on Jobs

# bjobs: Currently unfinished or < 1-hr finished
# 
# simple
bjobs JOB_ID
bjobs -q QUEUE_NAME
bjobs -J JOB_NAME
bjobs -g JOB_GROUP

# wide columnar format
bjobs -w

# long format or unformatted job detail
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

# bhist: Currently unfinished or past (anytime) finished/exited
#
# simple & long format
bhist JOB_ID
bhist -l JOB_ID

# all or from a specific date range, or to now
bhist -a 
bhist -a -D 2017/02/06      # only that day
bhist -a -D 2017/02         # only that month
bhist -a -D 2017/02/06,2017/03/06   # the range
bhist -a -D 2017/02/06,     # then to now


# bacct: accounting (resource use) information on one or more jobs
# 
# simple or will long (full) details
bacct JOB_ID
bacct -l 143971

# using multiple Job_IDs means that the data will be summed
bacct 143971 145629
bacct -l 143971 145629
```

### Putting It Together

```bash
# who has how many jobs running?
#   problem is that wide returns extra spaces, & these need to be counted as one delimiter
bjobs -w -u all | grep RUN | awk -F '[[:space:]]+' '{print $2}' | sort | uniq -c

# who has what going on?
bjobs -w -u all | awk -F '[[:space:]]+' '{print $2}' | sort | uniq -c

# how many people on the grid right now?
bjobs -w -u all | awk -F '[[:space:]]+' '{print $2}' | sort | uniq -c | wc -l

# what does their job data look like?
#   one job
bjobs -l 143971 | grep -E "User|g>|IDLE|MAX"

#   one user
bjobs -l -u cpoliquin | grep -E "g>|IDLE|MAX"

#   all users
bjobs -l -u all | grep -E "User|g>|IDLE|MAX"

## can format this more nicely using awk to piece out data from each line
```
```
