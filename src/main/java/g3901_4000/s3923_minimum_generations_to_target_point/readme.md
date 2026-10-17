[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3923\. Minimum Generations to Target Point

Medium

You are given a 2D integer array `points` where <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>, z<sub>i</sub>]</code> represents a point in 3D space, and an integer array `target` representing a target point.

Define **generation** 0 as the initial list of points. For each integer `k >= 1`, form generation `k` as follows:

*   Consider every pair of two **distinct** points <code>a = [x<sub>1</sub>, y<sub>1</sub>, z<sub>1</sub>]</code> and <code>b = [x<sub>2</sub>, y<sub>2</sub>, z<sub>2</sub>]</code> taken from all points produced in generations 0 through `k - 1`.
*   For each such pair, compute <code>c = [floor((x<sub>1</sub> + x<sub>2</sub>) / 2), floor((y<sub>1</sub> + y<sub>2</sub>) / 2), floor((z<sub>1</sub> + z<sub>2</sub>) / 2)]</code> and collect every such `c` into a generation `k`.
*   All points in the generation `k` are produced **simultaneously** from points in generations 0 through `k - 1`.
*   After generation `k` is formed, the points in the generation `k` are considered available for forming later generations.

Return the **smallest** integer `k` such that the `target` appears in one of the generations 0 through `k`. If the `target` is already in the initial points, return 0. If it is impossible to obtain the `target`, return -1.

Notes:

*   **floor** denotes rounding **down** to the nearest integer.
*   "Two **distinct** points" means the two chosen points must have **different** `(x, y, z)` coordinates. A point cannot be paired with itself, and pairing two points with **identical** coordinates is not possible.

**Example 1:**

**Input:** points = \[\[0,0,0],[6,6,6]], target = [3,3,3]

**Output:** 1

**Explanation:**

*   **Generation 0:** The initial `points = \[\[0, 0, 0], [6, 6, 6]]`.
*   The `target = [3, 3, 3]` does not exist in generation 0.
*   **Generation 1:** For each pair of points in generation 0, we create new points.
    *   Using `[0, 0, 0]` and `[6, 6, 6]`, we generate `[3, 3, 3]`.
*   After generation 1, `points = \[\[0, 0, 0], [6, 6, 6], [3, 3, 3]]`.
*   The `target = [3, 3, 3]` is found in generation 1, so the smallest `k` is 1.

**Example 2:**

**Input:** points = \[\[0,0,0],[5,5,5]], target = [1,1,1]

**Output:** 2

**Explanation:**

*   **Generation 0:** The initial `points = \[\[0, 0, 0], [5, 5, 5]]`.
*   The `target = [1, 1, 1]` does not exist in generation 0.
*   **Generation 1:** For each pair of points in generation 0, we create new points.
    *   Using `[0, 0, 0]` and `[5, 5, 5]`, we generate `[2, 2, 2]`.
*   After generation 1, `points = \[\[0, 0, 0], [5, 5, 5], [2, 2, 2]]`.
*   **Generation 2:** For each pair of points available after generation 1, we create new points.
    *   Using `[0, 0, 0]` and `[5, 5, 5]`, we generate `[2, 2, 2]`.
    *   Using `[0, 0, 0]` and `[2, 2, 2]`, we generate `[1, 1, 1]`.
    *   Using `[5, 5, 5]` and `[2, 2, 2]`, we generate `[3, 3, 3]`.
*   After generation 2, `points = \[\[0, 0, 0], [5, 5, 5], [2, 2, 2], [1, 1, 1], [3, 3, 3]]`.
*   The `target = [1, 1, 1]` is found in generation 2, so the smallest `k` is 2.

**Example 3:**

**Input:** points = \[\[0,0,0],[2,2,2],[3,3,3]], target = [2,2,2]

**Output:** 0

**Explanation:**

*   **Generation 0:** The initial `points = \[\[0, 0, 0], [2, 2, 2], [3, 3, 3]]`.
*   The `target = [2, 2, 2]` already exists in generation 0, so the smallest `k` is 0.

**Example 4:**

**Input:** points = \[\[1,2,3]], target = [5,5,5]

**Output:** \-1

**Explanation:**

*   Only one initial point is available, so no new points can be generated.
*   Therefore, the target cannot be obtained, and the answer is -1.

**Constraints:**

*   `1 <= points.length <= 20`
*   <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>, z<sub>i</sub>]</code>
*   <code>0 <= x<sub>i</sub>, y<sub>i</sub>, z<sub>i</sub> <= 6</code>
*   `target.length == 3`
*   `0 <= target[i] <= 6`
*   The initial set of points contains no duplicates.

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    private static final int SIZE = 7;
    private static final int TOTAL_POINTS = SIZE * SIZE * SIZE;

    public int minGenerations(int[][] points, int[] target) {
        boolean[] visited = new boolean[TOTAL_POINTS];
        List<Integer> allPoints = new ArrayList<>();
        List<Integer> frontier = new ArrayList<>();
        int targetCode = encode(target[0], target[1], target[2]);
        initializePoints(points, visited, allPoints, frontier);
        if (frontier.contains(targetCode)) {
            return 0;
        }
        int generation = 0;
        while (!frontier.isEmpty()) {
            generation++;
            List<Integer> nextFrontier = generateNextFrontier(frontier, allPoints, visited);
            if (nextFrontier.contains(targetCode)) {
                return generation;
            }
            addPoints(nextFrontier, visited, allPoints);
            frontier = nextFrontier;
        }
        return -1;
    }

    private void initializePoints(
            int[][] points, boolean[] visited, List<Integer> allPoints, List<Integer> frontier) {
        for (int[] point : points) {
            int code = encode(point[0], point[1], point[2]);
            visited[code] = true;
            allPoints.add(code);
            frontier.add(code);
        }
    }

    private List<Integer> generateNextFrontier(
            List<Integer> frontier, List<Integer> allPoints, boolean[] visited) {
        List<Integer> nextFrontier = new ArrayList<>();
        boolean[] addedThisGeneration = new boolean[TOTAL_POINTS];
        for (int pointA : frontier) {
            addGeneratedPoints(pointA, allPoints, visited, addedThisGeneration, nextFrontier);
        }
        return nextFrontier;
    }

    private void addGeneratedPoints(
            int pointA,
            List<Integer> allPoints,
            boolean[] visited,
            boolean[] addedThisGeneration,
            List<Integer> nextFrontier) {
        int[] a = decode(pointA);
        for (int pointB : allPoints) {
            if (pointA != pointB) {
                addGeneratedPoint(a, pointB, visited, addedThisGeneration, nextFrontier);
            }
        }
    }

    private void addGeneratedPoint(
            int[] a,
            int pointB,
            boolean[] visited,
            boolean[] addedThisGeneration,
            List<Integer> nextFrontier) {
        int[] b = decode(pointB);
        int x = (a[0] + b[0]) / 2;
        int y = (a[1] + b[1]) / 2;
        int z = (a[2] + b[2]) / 2;
        int newPoint = encode(x, y, z);
        if (!visited[newPoint] && !addedThisGeneration[newPoint]) {
            addedThisGeneration[newPoint] = true;
            nextFrontier.add(newPoint);
        }
    }

    private void addPoints(List<Integer> points, boolean[] visited, List<Integer> allPoints) {
        for (int point : points) {
            visited[point] = true;
            allPoints.add(point);
        }
    }

    private int encode(int x, int y, int z) {
        return x * SIZE * SIZE + y * SIZE + z;
    }

    private int[] decode(int code) {
        int x = code / (SIZE * SIZE);
        code %= SIZE * SIZE;
        int y = code / SIZE;
        int z = code % SIZE;
        return new int[] {x, y, z};
    }
}
```