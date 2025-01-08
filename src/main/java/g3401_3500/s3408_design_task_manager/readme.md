[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3408\. Design Task Manager

Medium

There is a task management system that allows users to manage their tasks, each associated with a priority. The system should efficiently handle adding, modifying, executing, and removing tasks.

Implement the `TaskManager` class:

*   `TaskManager(vector<vector<int>>& tasks)` initializes the task manager with a list of user-task-priority triples. Each element in the input list is of the form `[userId, taskId, priority]`, which adds a task to the specified user with the given priority.
    
*   `void add(int userId, int taskId, int priority)` adds a task with the specified `taskId` and `priority` to the user with `userId`. It is **guaranteed** that `taskId` does not _exist_ in the system.
    
*   `void edit(int taskId, int newPriority)` updates the priority of the existing `taskId` to `newPriority`. It is **guaranteed** that `taskId` _exists_ in the system.
    
*   `void rmv(int taskId)` removes the task identified by `taskId` from the system. It is **guaranteed** that `taskId` _exists_ in the system.
    
*   `int execTop()` executes the task with the **highest** priority across all users. If there are multiple tasks with the same **highest** priority, execute the one with the highest `taskId`. After executing, the `taskId` is **removed** from the system. Return the `userId` associated with the executed task. If no tasks are available, return -1.
    

**Note** that a user may be assigned multiple tasks.

**Example 1:**

**Input:**   
 ["TaskManager", "add", "edit", "execTop", "rmv", "add", "execTop"]   
 [[[[1, 101, 10], [2, 102, 20], [3, 103, 15]]], [4, 104, 5], [102, 8], [], [101], [5, 105, 15], []]

**Output:**   
 [null, null, null, 3, null, null, 5]

**Explanation**

TaskManager taskManager = new TaskManager([[1, 101, 10], [2, 102, 20], [3, 103, 15]]); // Initializes with three tasks for Users 1, 2, and 3.   
 taskManager.add(4, 104, 5); // Adds task 104 with priority 5 for User 4.   
 taskManager.edit(102, 8); // Updates priority of task 102 to 8.   
 taskManager.execTop(); // return 3. Executes task 103 for User 3.   
 taskManager.rmv(101); // Removes task 101 from the system.   
 taskManager.add(5, 105, 15); // Adds task 105 with priority 15 for User 5.   
 taskManager.execTop(); // return 5. Executes task 105 for User 5.

**Constraints:**

*   <code>1 <= tasks.length <= 10<sup>5</sup></code>
*   <code>0 <= userId <= 10<sup>5</sup></code>
*   <code>0 <= taskId <= 10<sup>5</sup></code>
*   <code>0 <= priority <= 10<sup>9</sup></code>
*   <code>0 <= newPriority <= 10<sup>9</sup></code>
*   At most <code>2 * 10<sup>5</sup></code> calls will be made in **total** to `add`, `edit`, `rmv`, and `execTop` methods.

## Solution

```java
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.TreeSet;

public class TaskManager {

    private TreeSet<int[]> tasks;
    private Map<Integer, int[]> taskMap;

    public TaskManager(List<List<Integer>> tasks) {
        this.tasks = new TreeSet<>((a, b) -> b[2] == a[2] ? b[1] - a[1] : b[2] - a[2]);
        this.taskMap = new HashMap<>();
        for (List<Integer> task : tasks) {
            int[] t = new int[] {task.get(0), task.get(1), task.get(2)};
            this.tasks.add(t);
            this.taskMap.put(task.get(1), t);
        }
    }

    public void add(int userId, int taskId, int priority) {
        int[] task = new int[] {userId, taskId, priority};
        this.tasks.add(task);
        this.taskMap.put(taskId, task);
    }

    public void edit(int taskId, int newPriority) {
        int[] task = taskMap.get(taskId);
        tasks.remove(task);
        taskMap.remove(taskId);
        int[] newTask = new int[] {task[0], task[1], newPriority};
        tasks.add(newTask);
        taskMap.put(taskId, newTask);
    }

    public void rmv(int taskId) {
        this.tasks.remove(this.taskMap.get(taskId));
        this.taskMap.remove(taskId);
    }

    public int execTop() {
        if (this.tasks.isEmpty()) {
            return -1;
        }
        int[] task = this.tasks.pollFirst();
        this.taskMap.remove(task[1]);
        return task[0];
    }
}

/*
 * Your TaskManager object will be instantiated and called as such:
 * TaskManager obj = new TaskManager(tasks);
 * obj.add(userId,taskId,priority);
 * obj.edit(taskId,newPriority);
 * obj.rmv(taskId);
 * int param_4 = obj.execTop();
 */
```