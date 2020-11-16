# Lab3OS MULTILEVEL QUEUE SCHEDULING - MQS
## DESCRIPTION
## About MQS

**Multiple-level queues** are not an independent scheduling algorithm. They make use of other existing algorithms to group and schedule jobs with common characteristics.

1) Multiple queues are maintained for processes with common characteristics.

2) Each queue can have its own scheduling algorithms.

3) Priorities are assigned to each queue.

**First Come First Serve (FCFS)**

1) Jobs are executed on first come, first serve basis.

2) It is a non-preemptive, pre-emptive scheduling algorithm.

3) Easy to understand and implement.

4) Its implementation is based on FIFO queue.

5) Poor in performance as average wait time is high.

**Priority Based Scheduling**

1) Priority scheduling is a non-preemptive algorithm and one of the most common scheduling algorithms in batch systems.

2) Each process is assigned a priority. Process with highest priority is to be executed first and so on.

3) Processes with same priority are executed on first come first served basis.

4) Priority can be decided based on memory requirements, time requirements or any other resource requirement.

**Round Robin Scheduling**

1) Round Robin is the preemptive process scheduling algorithm.

2) Each process is provided a fix time to execute, it is called a quantum.

3) Once a process is executed for a given time period, it is preempted and other process executes for a given time period.

4) Context switching is used to save states of preempted processes.

It may happen that processes in the ready queue can be divided into different classes where each class has its own scheduling needs. For example, a common division is a **foreground (interactive)** process and **background (batch)** processes.These two classes have different scheduling needs. For this kind of situation Multilevel Queue Scheduling is used.Now, let us see how it works. 

**Ready Queue** is divided into separate queues for each class of processes. For example, let us take three different types of process System processes, Interactive processes and Batch Processes. All three process have there own queue. Now,look at the below figure.

![Image alt](https://github.com/VovaMaybeNextTime/Lab3OS/blob/main/res/1.jpg)

All three different type of processes have there own queue. Each queue have its own Scheduling algorithm. For example, queue 1 and queue 2 uses **Round Robin** while queue 3 can use **FCFS** to schedule there processes.



**Scheduling among the queues** : What will happen if all the queues have some processes? Which process should get the cpu? To determine this Scheduling among the queues is necessary. There are two ways to do so – 

 

**1) Fixed priority preemptive scheduling method** – Each queue has absolute priority over lower priority queue. Let us consider following priority order queue 1 > queue 2 > queue 3.According to this algorithm no process in the batch queue(queue 3) can run unless queue 1 and 2 are empty. If any batch process (queue 3) is running and any system (queue 1) or Interactive process(queue 2) entered the ready queue the batch process is preempted.

**2) Time slicing** – In this method each queue gets certain portion of CPU time and can use it to schedule its own processes.For instance, queue 1 takes 50 percent of CPU time queue 2 takes 30 percent and queue 3 gets 20 percent of CPU time.

**Example Problem** : 
Consider below table of four processes under Multilevel queue scheduling.Queue number denotes the queue of the process.

![Image alt](https://github.com/VovaMaybeNextTime/Lab3OS/blob/main/res/2.jpg)

Priority of queue 1 is greater than queue 2. queue 1 uses Round Robin (Time Quantum = 2) and queue 2 uses FCFS. 

Below is the **gantt chart** of the problem :


![Image alt](https://github.com/VovaMaybeNextTime/Lab3OS/blob/main/res/3.jpg)

At starting both queues have process so process in queue 1 (P1, P2) runs first (because of higher priority) in the round robin fashion and completes after 7 units then process in queue 2 (P3) starts running (as there is no process in queue 1) but while it is running P4 comes in queue 1 and interrupts P3 and start running for 5 second and after its completion P3 takes the CPU and completes its execution. 

**Advantages**:

1)The processes are permanently assigned to the queue, so it has advantage of low scheduling overhead.

**Disadvantages**:

1)Some processes may starve for CPU if some higher priority queues are never becoming empty.
2)It is inflexible in nature.

## Process structure

**struct process** {

    int priority;
    
    int burst_time;
    
    int tt_time;
    
    int total_time = 0;

};

## Queues structure

**struct queues** {

    int priority_start;
    
    int priority_end;
    
    int total_time = 0;
    
    int length = 0;
    
    process* p;
    
    bool executed = false;
    
};

## Functions

**bool notComplete(queues q[])** {

    bool a = false;
    
    int countInc = 0;
    
    for (int i = 0; i < 3; i++) {
    
        countInc = 0;
        
        for (int j = 0; j < q[i].length; j++) {
        
            if (q[i].p[j].burst_time != 0) {
            
                a = true;
                
            }
            
            else {
            
                countInc += 1;
                
            }
            
        }
        
        if (countInc == q[i].length) {
        

            q[i].executed = true;
            
        }
        
    }
    
    return a;
    
}

**void sort_ps(queues q)** {

    for (int i = 1; i < q.length; i++) {
    
        for (int j = 0; j < q.length - 1; j++) {
        
            if (q.p[j].priority < q.p[j + 1].priority) {
            
                process temp = q.p[j + 1];
                
                q.p[j + 1] = q.p[j];
                
                q.p[j] = temp;
                
            }
            
        }
        
    }
    
}

**void checkCompleteTimer(queues q[])** {

    bool a = notComplete(q);
    
    for (int i = 0; i < 3; i++) {
    
        if (q[i].executed == false) {
        
            for (int j = 0; j < q[i].length; j++) {
            
                if (q[i].p[j].burst_time != 0) {
                
                    q[i].p[j].total_time += 1;
                    
                }
                
            }
            
            q[i].total_time += 1;
            
        }
        
    }
    
}


## Сonclusion

It is queue scheduling algorithm in which ready queue is partitioned into several smaller queues and processes are assigned permanently into these queues. The processes are divided on basis of their intrinsic characterstics such as memory size, priority etc.

In this algorithm queue are classified into two groups, first containing background processes and second containing foreground processes.
80% CPU time is given to foreground queue using Round Robin Algorithm and 20% time is given to background processes using First Come First Serve Algorithm.

The priority is fixed in this algorithm. When all processes in one queue get executed completely then only processes in other queue are executed.
Thus, starvation can occur.

Since, processes do not move between queues, it has low scheduling overhead and is inflexible.
