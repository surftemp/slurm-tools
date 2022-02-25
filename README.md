# slurm-tools

Various helper scripts for using Slurm at Jasmin.

These scripts are generally wrappers around [Slurm commands](https://slurm.schedmd.com/man_index.html)
which use [pandas](https://pandas.pydata.org/) to produce readable summaries.

## squeues

Summarises [squeue](https://slurm.schedmd.com/squeue.html) output to produce a
human readable summary of jobs per user, per queue. Usage:
```
squeues [-h] [-p PARTITION] [-s]
```

### optional arguments:

#### -h, --help
Show this help message and exit

#### -p PARTITION, --partition PARTITION
Partitions to view

#### -s
Sort users by number of jobs (default = priority)

### Examples
Show the current queue status:
```
$ squeues 
QUEUE_NAME         NJOBS   PEND   RUN  COMP  CPU   Time Pending     Priority
TOTAL             193283 186034  7052   197  8837  0 days 07:58     0-867992
short-serial      184765 178867  5875    23  7414  0 days 09:11     0-10021
  user1           174835 173852   972    11  1966  8 days 03:42     4286-10021
  user2              812    807     5           5  0 days 00:54     39-2531
  user3             1521         1521        1521  1 days 14:15     304-2491

```

## srusage

Summarises [sacct](https://slurm.schedmd.com/sacct.html) output to produce a
human readable summary of resource usage for recent jobs.
```
srusage [-h] [-n NAME] [-p PARTITION] [-u USER] [-t STATE] [-S STARTTIME] [-E ENDTIME] [--plot]
```

### optional arguments:

#### -h, --help
Show this help message and exit

#### -n NAME, --name NAME
Show jobs matching these names. Default is to display all jobs

#### -p PARTITION, --partition PARTITION
Partitions to view

#### -u USER, --user USER
Display jobs from specific users. Default is to display jobs of current user.

#### -t STATE, --state STATE
Comma separated list of jobs states to view. Default is to display any completed
jobs.

#### -S STARTTIME, --starttime STARTTIME
Show jobs after specified time. Default is start of current day. Valid time
formats are:
* YYYY-MM-DD[THH:MM]
* now, today (start of current day)
* {now|today}-count{minutes|hours|days|weeks}

#### -E ENDTIME, --endtime ENDTIME
Show jobs before specified time (default is now). Valid time formats are the
same as `--starttime`

### --plot
Plot results

### Examples
Show resources used by jobs which completed today:
```
$ srusage
Gathering Slurm job statistics:
Found 2615 total jobs
State     COMPLETED  FAILED  TIMEOUT
JobName                             
job_a           154       1     1532
job_b           916       3        9
         JobName Pending              Timelimit Elapsed                   ReqMem   MaxRSS                 
           count     min    max          unique  median    min    max     unique   median     min      max
JobName                                                                                                   
job_a       1687     0.5  166.4  [240.0, 480.0]   240.3  102.4  240.5  [16000.0]   9894.8     0.9  15854.6
job_b        928   158.9  301.4         [480.0]   308.7   66.5  480.4  [16000.0]  10097.5  9097.0  15919.9

 Disk usage
         MaxDiskRead                                                                    MaxDiskWrite                                                               
               count      mean      std      min      25%       50%       75%       max        count     mean     std      min      25%      50%      75%       max
JobName                                                                                                                                                            
job_a         1699.0   79630.8  13846.3      0.0  69022.9   83604.3   89034.3  114500.1       1699.0  53087.1  9293.7      0.0  45950.7  55780.6  59519.9   76262.2
job_b          928.0  102708.6  13261.5  25079.6  95305.4  103148.6  110738.2  158496.4        928.0  68527.5  9133.5  15666.4  63600.6  68910.4  73982.4  107030.6
```

Show resources used by jobs which completed in the last week and produce plots:
```
$ srusage -S today-1weeks --plot
```
