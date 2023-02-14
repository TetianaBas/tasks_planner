# task_sceduler-
A. Why is a priority queue a particularly well-suited data structure to prioritize
tasks?

The major reason for this is the difference in time complexities. To get the highest element from
the list I have to use max function that had O(n) time complexity since it will have to traverse all
the elements and compare them with each other unlit it finds the highest value. Meanwhile I will
do heapify with time complexity of O(log(n)) and get the top value with a constraint time
complexity of O(1) which on the target scale will not be important and the added time
complexities will be still O (log(n)). It means that as the number of tasks increases, the run time
of the list based time scheduler will increase linearly at the same rate as the the number of tasks.
For the heap based taks scheduler there will be a different dynamic, for increase in 100 task the
run time will increase for only 6.6 comparing to the increase for a 100 that will demonstrate the
list based time scheduler.
Therefore heap gives much better efficiency and I will be able to get my tasks executed faster. It
is more efficient due to its structure. It is represented as a binary tree which makes it unnecessary
to traverse all the elements and allows it to focus on the level based traversal. Since the
logarithmic base in programming is 2 we get the tie complexity that is equal to the height of the
tree.
I will need only one priority queue. I have 3 tasks that have specific deadlines and time frames
in which they can be executed (running before 10 am, lunch in the lunch time and the temple
visit during its working hours).
I will not separate this task into a different priority queue. I will accommodate this task in my
schedule by making sure its priority value gets larger as I approach the deadline time. I will
create a linear equation for each task with a deadline that will set the value of the priority with
respect to the current time. In this way, I will make sure that running is executed in the morning
since it cannot be executed at a different time of the day. It is highly unlikely that the time fixed
task will have lower priority than another task at the deadline of execution. The initial priority is
0 and tasks without time constraint get increase of priority of either duration /10 (it is unlikely to
get a task that lasts 1000 min since it is more than 16 hours) or a task should have a lot of
dependencies and block all the other tasks (which will be a reasonable cause for skipping the
fixed task: I have a lot of blocked tasks by a different task).
If I had a fixed time task I would have used the same approach but with a more narrow time
frame. I explained this in Detail in Q5.

B. Describe how your scheduler will work at a very high level

I will create 3 classes to assist with scheduling tasks
The first class is Maxheap which will be the algorithm at the basis of the priority queue.
Heap is the binary tree with the top element that has the highest priority value. One node has
only 2 children and both of them should have a lower value than the parent. This ensures that the
node with the highest value will always be at the top. The highest value of the heap is the
element with the highest priority thus it will be executed next.
If we want to add a new element to the tree we will by default add it to the free space on the
lower level (moving from right to left). If a new element is lower than its parent it just stays
under the parent but if it is higher it starts traversing the levels up to ensure that the properties of
the max heap are not violated. As the task is executed I will have to delete it from the tree. Since
I will delete the top element the tree will have to rebalance itself.
The second class is responsible for creating and maintaining tasks.
It manages all the attributes of the task, sets the priority to default 0, and status to default not
started. I also introduced the value of the total number of tasks that will help me to assign the
unique identification number to the tasks. So when the task is added the total number of tasks is
increased by 1 and the number is assigned to the task as an id. Even if the task is deleted, its id
will not be given to a different task since when it enters the class it is recorded. It also ensures
that there would be no repetitions.
I defined a function that will be updating the priorities of the tasks. I update the initial priorities
of the tasks based on time (relevant for the tasks that have specific deadlines) and/or based on the
dependencies (by increasing the priority of the tasks the current task is blocked by to prioritize it
compared to the tasks that do not block other tasks) or based on the duration.
I also defined 2 built-in functions to “teach” my code to compare 2 objects of the Task class by
comparing values of their priorities.
Other functions in this class are related to priority calculation. They will help to calculate the
priority of each task and update it each time one of the tasks gets executed. The priorities will be
calculated based on the number of dependencies and the priorities of the dependencies. Also, it
will consider the time for the functions that have time constraints.
The detailed work of these functions is explained in part C.
The third class is the execution of the tasks based on their priority.
Within this class, I will add the tasks to my priority queue that is represented by the heap data
structure. I will add only the tasks that are not blocked (do not have dependencies). As a task is
added to the priority queue its status is changed from the default “not started” to “in priority
queue”. I will execute one task at a time, it will be a task at the top of the tree since it has the
highest priority value. As a task is executed I delete it from the tree and add it to the array of
completed tasks. After that the tree rebalances itself based on new priorities (the initial priority
will be recalculated since we have 1 task less), new tasks are added to the tree (if the one
executed was blocking other tasks) and a new task with the highest priority will become a new
root. I will repeat this process until I do not have any tasks that are unfinished (have any status
that is not completed). The program will print tasks as they are executed including the time and
their priority. In the end, it will also return the array of the task descriptions in the order in which
the tasks were executed.

C.Explain how you have defined and computed the priority value of each task.

I am choosing max heap since the tasks with the highest priority value should be executed first.
The starting priority of the tasks is 0. The priorities of the tasks are calculated in different ways.
I have tasks with/without deadlines and with/without dependencies so the process for them is
different
For the tasks with deadlines
I define the linear equations that will increase their priority to 100 when they hit the last point to
be executed to make it before the deadline. For example, the earliest I can start running is 7 am
(1 hour after waking up). Before 7 am the priority for the task is 0. Since the deadline for the task
is 10 am and the duration is 60 min the latest time I can start doing it is 9 am. So at 9 am, the
priority for running is 100. I included the slope and intercept calculation formula that will
calculate the priority of tasks with fixed deadlines before the execution of each next task.
If the time is outside of the window when the task can be executed the priority of the tasks with
deadlines is 0.
For the tasks with dependencies:
Since a task that is blocked by other tasks cannot be pushed in the priority queue it does not
make sense to manipulate its priority. However, I should increase the priority of the task that the
current task is blocked by. In this way it will be executed faster and my blocked task will be
pushed in the priority queue. To accomplish this I add to the initial priority of the blocking task 2
raised to the power the number of total tasks that block my current task, so I will give priority to
the tasks that are heavily blocked by other tasks by increasing the priority of those blocking
tasks.
For the tasks without deadlines and without dependencies:
For these tasks the updated priority will be their duration divided by 6 (to avoid making those
tasks too heavy and higher than the tasks with time constraints when they approach the deadline).
So the priority is proportional to the duration which means that the tasks that take more time will
be executed faster. Usually the tasks that take more time are more difficult and have higher value
and I want to finish them to get higher return on the invested time. Also, it is more effective to do
more difficult tasks first according to Tracy, B. (2017).
I treat priorities as utilities and use the law of diminishing marginal utility when evaluating the
priorities of the tasks with dependencies. I add the priority values to the tasks that block the
current task that is evaluated. I add 2 raised to the power of the number of tasks that block the
evaluated one. So depending on the length of the list tasks will get different priorities and the
difference will change not linearly. If the evaluated task has only 1 priority, 2^1 = 1 will be added
to the initial priority of blocking task, if the task is blocked by 2 other tasks those tasks will get
additional priority of 2^2 = 4, so although there is a difference in 1 task, the difference in added
priority is 3 and the gap will increase as the number of blocking tasks will increase to maintain
the balance.
