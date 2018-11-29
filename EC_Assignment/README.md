# Extra Credit Assignment: K-source shortest paths

# Due December 1, 2018 @ 11:59pm

TODO Please edit the following information in your assignment

- Name:
- How many hours did it take you to complete this assignment?
- Did you collaborate with any other students/TAs/Professors?
- Did you use any external resources? (Cite them below)
  - tbd
  - tbd
- (Optional) What was your favorite part of the assignment?
- (Optional) How would you improve the assignment?

# Logistics

For this assignment, you can use either your Linux VM or the CCS machines.  You will likely have much better performance on the CCS machines. 

# Introduction
One of the ways to traverse a graph is using Breadth First Search (BFS).  BFS has several nice qualities:  
- Computes if the graph is connected
- Visits all the vertices of the graph
- Can find a path with the minimum number of edges between any two vertices
- Can find a simple cycle


# The Breadth First Search Algorithm
BFS is an algorithm where you start traversing from a selected node (the source or start node) and traverse the graph layerwise, exploring the neighbor nodes (nodes which are directly connected to source node). One done, you move towards the next-level neighbor nodes.

A graph can contain cycles, which may bring you to the same node again (and again, and again, ...) while traversing the graph. To avoid processing the same node over and over again, you need to mark the node after it is processed. 

The algorithm puts each node of the graph into one of two categories:
1. Visited
1. Not Visited

The goal of the algorithm is to mark each node as visited while avoiding cycles.

The algorithm works as follows:

Start by putting the source or start node at the back of a queue .
Take the front item of the queue and add it to the visited list.
Create a list of that node’s adjacent nodes. Add the ones which aren't in the visited list to the back of the queue.
Keep repeating steps 2 and 3 until the queue is empty.

The queue follows a First In First Out (FIFO) queuing method, and therefore, the neighbors of the node will be visited in the order in which they were inserted in the node i.e. the node that was inserted first will be visited first, and so on. 

If the graph is thought to not be connected, run BFS starting at every node to cover all the nodes. 

# Example:
Let’s look at an example with five nodes (from https://www.programiz.com/dsa/graph-bfs).

<img align="center" src="https://cdn.programiz.com/sites/tutorial2program/files/graph-bfs-step-0.jpg" width="600px" alt="picture">

Let’s start with node 0, the BFS algorithm starts by putting it in the Visited list and putting all its adjacent nodes in the queue.

<img align="center" src="https://cdn.programiz.com/sites/tutorial2program/files/graph-bfs-step-1.jpg" width="600px" alt="picture">

Next, we visit the element at the front of queue i.e. 1 and go to its adjacent nodes. Since 0 has already been visited, we visit 2 instead (which is at the front of the queue).

<img align="center" src="https://cdn.programiz.com/sites/tutorial2program/files/graph-bfs-step-2.jpg" width="600px" alt="picture">

Node 2 has an unvisited adjacent node in 4, so we add that to the back of the queue and visit 3, which is at the front of the queue.

<img align="center" src="https://cdn.programiz.com/sites/tutorial2program/files/graph-bfs-step-3.jpg" width="600px" alt="picture">

<img align="center" src="https://cdn.programiz.com/sites/tutorial2program/files/graph-bfs-step-4.jpg" width="600px" alt="picture">

Node 3 doesn’t have any neighbors that have not already been marked as Visited.  So we mark it as Visited and visit node 4, which is at the front of the queue. 

<img align="center" src="https://cdn.programiz.com/sites/tutorial2program/files/graph-bfs-step-5.jpg" width="600px" alt="picture">


Since the queue is empty, we have completed the Depth First Traversal of the graph

# Task 1: Single Source Shortest Path
You are given a source node, find the shortest path from the source node 5 to rest of the nodes. Print out the path length from the source node to all the nodes (assume the edge cost is 1 for all edges).  Use the following print format to print the path length “%d -> %d: %d”
BFS gives you the shortest path from one node to all the other connected nodes.  You will need to augment the algorithm above to record the path length ( # of levels) to each node you touch.  


# Task 2: K-source Shortest Path
Now you are given K source nodes. Find the shortest path from K nodes to all other nodes. For each source node, print out the path in an distance ordered list of “(node, distance from source)” pairs separated by commas.  For the example above, the list for source 0 would be: (0, 0), (1, 1), (2, 1), (3, 1), (4, 2).

## Part 1:
Write a single thread/process code to iteratively calculate the shortest path from K given nodes.
Write a multi-process / multi-threaded version (K threads?) to calculate shortest path to all the nodes.  You can use any of the techniques we’re covered, from forking sub-processes, to generating multiple threads explicitly, to using OpenMP.
Compare the running times for finding K-source shortest paths on your single process implementation versus your multi-thread/process implementation.

## Part 2:
Compare the running time (mean and median running time) of both implementations over 101 runs for different size of graphs in the repo.  Select the K source nodes randomly (uniform) for each run.  The node IDs range from 0 to N-1, where N is the number of nodes in the network. Add a table to the README for the mean and median running times for each size network that is in the repo. 
 




# Part 1 - Exploration Questions

In this part you will answer some questions about the xv6 operating system. Modify this README.md file directly with your answers.
Processes:

### Processes

1. In which file does the the process table exist? *your answer here*
2. What is the struct name of the process table? *your answer here*
3. When there is a context switch from one process to another, where are the values of the registers of the old process saved? *your answer here*
4. What are the 6 possible states of a process?  Also, give a brief phrase describing the purpose of each state.
	1. *your answer here*
	2. *your answer here*
	3. *your answer here*
	4. *your answer here*
	5. *your answer here*
	6. *your answer here*
5. What is the function that does a context switch between two processes? *your answer here*
6. Explain how the context switch function works (Note, this "function" *may* be in an assembly file). *your answer here*

### Process Startup and running

1. What function from 'main.c' creates the first user process? *your answer here*
2. Where do we actually start running processes in our code? That is, what is the actual function that has an infinite loop for running processes? *your answer here*

### Files and File Descriptors:

In class, we have briefly used and talked about files and file descriptors.  We have not yet discussed i-nodes.  For these questions, you can think of an i-node as a location on disk that has the "table of contents" for all information about a file.

In these questions, we will follow the chain of control from open() to a file descriptor, to a "file handle" (including the
offset into the file), to the i-node. Note: What I call a "file handle", UNIX xv6 calls a "struct file".

1.  The function 'sys_open()' returns a file descriptor 'fd'. To do this, it opens a new file handle 'filealloc()', and it allocates a new file descriptor with 'fdalloc()'. Where is the file descriptor allocated?  
	- Hint: You will see that the file descriptor is one entry in an array in an important struture you have already looked at.
	- *your answer here*
2. What is the algorithm used to choose which entry in the array to use for the new file descriptor?
    	- Note: The name 'NOFILE' means "file number".  "No." is sometimes used as an abbreviation for the word "number".
	- *your answer here*
3.  As you saw above, the file descriptor turned out to be an index in an array.  What is the name of the array for which the file descriptor is an index?  Also, what is the type of one entry in that array.
	- *your answer here*
4.  The type that you saw in the above question is what I was calling a "file handle" (with an offset into the file, etc.). What is the name of the field that holds the offset into the file? We saw it in the function 'sys_open()' (*hint, it's towards the bottom of the function*).
	- *your answer here*
5.  The file handle type was initialized to 'FD_INODE' in the sys_open function.  What are the other types that it could have been initialized to (*hint, look for the file struct*)?
	- *your answer here*
    
# Part 2 - Proc Priority modification

We have spent time in class talking about how we do not have control over how processes threads are scheduled, and the order in which they execute. Sometimes the operating system needs to interrupt for example to perform some task (e.g. a new device is plugged in, some data is ready to be read from disk, etc.). However, a question remains of how are processes and threads actually scheduled. The answer is based on a 'scheduling' algorithm, and we can begin exploring this idea by modifying a structure you saw a few times in Part 1.

## Task 1 - Implementing priorities

Below is the 'proc' data structure found in the proc.h file. The `struct proc` as we know, contains information about processes running in our operating system.

```c
// Per-process state
struct proc {
  uint sz;                     // Size of process memory (bytes)
  pde_t* pgdir;                // Page table
  char *kstack;                // Bottom of kernel stack for this process
  enum procstate state;        // Process state
  int pid;                     // Process ID
  struct proc *parent;         // Parent process
  struct trapframe *tf;        // Trap frame for current syscall
  struct context *context;     // swtch() here to run process
  void *chan;                  // If non-zero, sleeping on chan
  int killed;                  // If non-zero, have been killed
  struct file *ofile[NOFILE];  // Open files
  struct inode *cwd;           // Current directory
  char name[16];               // Process name (debugging)
};
```

### Modifying Proc with priorities

1. Add an `int priority;` to the struct above found in proc.h.
	- You might also add a comment along the lines of `// stores the processes priority (i.e. importance) for running`
	- Note: The higher the value, the more important a process is.
2. Now since we have added an additional field to our proc structure, we also need to modify what happens when we iniitalize a new process.
	- Find the function 'allocproc' - which stands for allocate process.
	- Under the 'found:' label, and after `p->pid=nextpid++;` add `p->priority=10; // This is around line 90`

### Testing our priorities

1. Now, it is up to you to create a system call, called `cps`. cps stands for 'current process state'
	- The `int cps()` command should print out the following information about a process.
		- I implemented 'cps' in proc.c 
	- It will return the value '22' matching its system call.
		- e.g. 
		```c
			struct proc *p;
			// Enable interrupts
			sti();
			
			// Maybe acquire some lock?
			// Maybe a loop here?
				if(p->state == SLEEPING)
					cprintf("%s \t %d \t SLEEPING \t %d\n", p->name, p->pid, p->priority);
				else if(p->state == RUNNING)
					cprintf("%s \t %d \t RUNNING \t %d\n", p->name, p->pid, p->priority);
				else if(p->state == RUNNABLE)
					cprintf("%s \t %d \t RUNNABLE \t %d\n", p->name, p->pid, p->priority);
					
			return ??
		```
	- Your 'sys_cps()' command will merely calls cps. You can do this in sysproc.c
	- Don't forget to modify syscall.c
	- Don't forget to modify usys.S
	- Note: I used the 'scheduler' as inspiration for figuring out how to loop through all of the processes.
	- Note: You can use your previous lab as an additional resource to make sure all of the steps are implemented.
2. Next, write a program called 'mycommand' that will make a call to 'cps()'
	- This can be a very simple program.
3. Go ahead and run 'make' again, and then run your xv6.
	- Now run the 'cps' command on the terminal to see the processes running and their priority.
	- Congratulations on your first modification to the xv6 operating system!
	- Note: You can use `ps -c` on your regular unix terminal to get a reference, where 'PRI' is a priority.
    
# Deliverable

- Run 'make clean' on your xv6 directory.
	- Then upload your xv6 repository to github in a directory named 'xv6'.

# Rubric

- 50% - Part 1
	- Based on correctness of written responses
- 50% - Part 2
	- A working 'mycommand' program that prints out processes with a default priority value of 10.
     
### Glossary and Tips
1. Commit your code changes to the repository often, this is best practice.
2. Do not commit your .o file or your executable file to the repository, this is considered bad practice!
3. On Functionality/Style
	1. You should have comments in your code to explain what is going on, and use good style (80 characters per line maximum for example, functions that are less than 50 lines, well named variables, well named functions etc.).

# Going Further

- Implement a priority queue that runs processes based on their priority number. Every timeslice a process gets, it should then decrement its priority, and then reset its priority to its 'initial' priority for when it was created.
	- (You can of course modify this heuristic as you like, but think about how to create a 'fair' scheduler).

# Feedback Loop

(An optional task that will reinforce your learning throughout the semester)

- xv6 is based on the original UNIX, Sixth Edition (also called [Version 6 UNIX](https://en.wikipedia.org/wiki/Version_6_Unix))
	- There is a commentary for the original v6 here: https://en.wikipedia.org/wiki/Lions%27_Commentary_on_UNIX_6th_Edition,_with_Source_Code
