<h1>XV6-Linux</h1>


<h2>Description</h2>
This project was done to gain better understanding of Linux architecture using xv6. xv6 is a simple UNIX like operating system which is often used to teach linux architecture. This project has multiple stages as listed below. These stages includes making changes or adding new components to existing base to improve functionality and implement different ideas to gain better understanding if linux architecture.
<br />


<h2>Languages and Utilities Used</h2>

- <b>C</b>
- <b>vim</b>
- <b>xv6</b>

<h2>Trace System Call</h2>
It takes one argument, an integer "mask", whose bits specify which system calls to trace. For example, to trace the fork 
system call, a program calls trace "1 << SYS_fork"
The Snippet below shows how it works when trace is called on 2147483647.
<pre>
    <code>
      $ trace 2147483647 grep hello README
      4: syscall trace -> 0
      4: syscall exec -> 3
      4: syscall open -> 3
      4: syscall read -> 1023
      4: syscall read -> 966
      4: syscall read -> 70
      4: syscall read -> 0
      4: syscall close -> 0
    </code>
</pre>


<h2>Mutex</h2>
Implementing Mutex to allow user threads to coordinate critical sections and safely share data.
<pre>
    <code>
typedef struct Mutex{
	int* lock;
	lock_id id;

}mutex;
mutex list[MAXMUTEXS];
int num_mutex;

lock_id mtx_create(int locked){
	/*Check */
	if(num_mutex < MAXMUTEXS){
		locked = 1;
		list[num_mutex].lock = &locked;
		list[num_mutex].id = num_mutex;
		num_mutex++;
		return list[num_mutex].id;
	}
	else{
		return -1;
	}
	
}

int mtx_lock(lock_id id){
	/*busy wait on mutex*/
	if(id > MAXMUTEXS){
		return 1;
	}
	else{
		while(*list[id].lock){
			/*if mutex locked , yield cpu to next thread*/
			printf("Mutex Yileding CPU Cause Lock not available \n");
			thread_yield();
		}
		*list[id].lock = 1;
		return 0;
	}
}

int mtx_unlock(lock_id id){
	if(id > MAXMUTEXS){
		return 1;
	}
}

    </code>
</pre>

<h2>Implementing Fair Share Scheduling algorithm</h2>
Implemented an advanced scheduler that schedules processes based on fair shares of the CPU. Each process has a share, and the scheduler must ensure that the number of quanta a process has been allocated stays near its predefined share.
<h2>Program walk-through:</h2>

<p align="center">
Creating ships using shift + left click <br/>
<img src="https://i.imgur.com/jNmwpIb.png" height="80%" width="80%" alt="Feature Pamnel"/>
<br />
<br />
Selecting Mutiple ships using mouse drag:  <br/>
<img src="https://i.imgur.com/5nlRhUz.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Using ctrl + c to copy and crtl +p to paste <br/>
<img src="https://i.imgur.com/5fsunDv.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Grouping items together(yellow ships are a group and moving one ship will move the whole ship)<br/>
<img src="https://i.imgur.com/5fsunDv.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />

<!--
 ```diff
- text in red
+ text in green
! text in orange
