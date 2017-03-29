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
bsub myjob.sh
bsub rsync -av ~/project/plan_B /export/projects/plan_B

bsub -J bunch_of_jobs myjob1
bsub -J bunch_of_jobs myjob2
bsub -J bunch_of_jobs myjob3

Use sbatch parameters that are inside the script file
bsub < myjob.sh


Job Control
Killing a Job
bkill JOB_ID
# all jobs in a given queue
bkill -q QUEUE_NAME
# all jobs with a given name
bkill -J JOB_NAME
# all jobs (for myself). period.
bkill 0

Suspending/Resuming a Job
bsusp JOB_ID
bsusp -q QUEUE_NAME
bsusp -J JOB_NAME
bsusp 0

bresume JOB_ID
bresume -q QUEUE_NAME
bresume -J JOB_NAME
bresume -

Information on Jobs (unfinished and < 1-hr finished)
bjobs JOB_ID
bjobs -q QUEUE_NAME
bjobs -J JOB_NAME
bjobs -g JOB_GROUP

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

Information on Jobs, finished and unfinished 
bhist JOB_ID
# finished and unfinished
bhist -a 
```
