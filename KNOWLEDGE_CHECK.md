# Lab 08: Knowledge check

**Answer each question below it, in your own words, in one to three sentences.** Replace the `...` below **Your answer:** with your text. Questions 1 and 3 also ask for saved output, which goes into the directory `results/`.

## 1. Slurm: what jobs have I submitted?

**Your task:** print a history of all your Slurm job submissions *since November 4, 2026*. Format the output to include the job ID, job name, node list, partition, account, user, state, start time, and elapsed time. See the job accounting fields in the [sacct documentation](https://slurm.schedmd.com/sacct.html). You may also ask Claude Code or another LLM for the exact command. 

Redirect the command's output to `results/job-history.txt` and include the file in your submission. Do not worry if some earlier jobs failed or were canceled. The complete history should show your process, including trial and error.

*Hint:* See [Part 1, Step 6](README.md#step-6-check-how-the-tasks-ended) in the README.

**Your answer:** Provide the entire command you used, including all CLI options.

sacct -j 22833206 -X --format=JobID,JobName,NodeList,State,ExitCode,Start,Elapsed > results/job-history.txt
THIS RETURNS ALL THE JOBS DONE TODAY INTO ONE FILE, JOB-HISTORY.TXT.THIS IS ALL THE PROCESSED INFORMAITON.

## 2. Slurm: did every array task run at the same time?

You submitted your entire job array with one `sbatch` command, but Slurm scheduled each task separately.

**Your task:** in the job history from Question 1, find the tasks from your most recent completed array run. Compare their start times and elapsed times.

*Hint:* See [Part 1, Step 5](README.md#step-5-watch-the-job-array) in the README.

**Your answer:** Did all tasks in the array start at the same time? Was the elapsed time identical for every task? Explain why or why not.
sort -k6 results/job-history.txt | head -n 5
sort -k6 results/job-history.txt | tail -n 5
NO THE ARRAY TASK RUN DID NOT RUN AT THE SAME TIME, THERE WERE VARYING TIMES, GOING FROM :17, :15, :13, ETC. THE TIMES VARIED DEPENDING ON HOW MUCH COMPUTATION WAS NEEDED.


## 3. Slurm: did my job use what it asked for?

Suppose your next job array has 1,000 books instead of 101. Before you submit it, you want to know whether your script asks for too much or too little memory and time.

**Your task:** run `seff` for **one** task of your finished job array, and save its output as `results/seff.txt`.

*Hint:* `seff` takes the ID of one task, such as `4211873_1`. Compare its **Memory Utilized** and **Job Wall-clock time** with `--mem` and `--time` in your `summarize.slurm`.

**Your answer:** How much memory and time did the task use, compared with what your script asked for? What would you change in your script, and why?

seff 22833206_1 > results/seff.txt 
cat results/seff.txt

Job ID: 22847112
Array Job ID: 22833206_1
Cluster: shen
User/Group: gew7vc/users
State: COMPLETED (exit code 0)
Nodes: 1
Cores per node: 2
CPU Utilized: 00:00:09
CPU Efficiency: 26.47% of 00:00:34 core-walltime
Job Wall-clock time: 00:00:17
Memory Utilized: 1.41 GB
Memory Efficiency: 8.84% of 16.00 GB (16.00 GB/node)

MEMORY USED WAS 8.84% OF 16GB AND CLOCK TIME WAS 00:00:17. REQUESTED 16 GB OF MEMORY AND TIME PER TASK WAS LIMIT 5 MINUTES.
