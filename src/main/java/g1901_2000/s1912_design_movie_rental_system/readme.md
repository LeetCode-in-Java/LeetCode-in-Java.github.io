[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1912\. Design Movie Rental System

Hard

You have a movie renting company consisting of `n` shops. You want to implement a renting system that supports searching for, booking, and returning movies. The system should also support generating a report of the currently rented movies.

Each movie is given as a 2D integer array `entries` where <code>entries[i] = [shop<sub>i</sub>, movie<sub>i</sub>, price<sub>i</sub>]</code> indicates that there is a copy of movie <code>movie<sub>i</sub></code> at shop <code>shop<sub>i</sub></code> with a rental price of <code>price<sub>i</sub></code>. Each shop carries **at most one** copy of a movie <code>movie<sub>i</sub></code>.

The system should support the following functions:

*   **Search**: Finds the **cheapest 5 shops** that have an **unrented copy** of a given movie. The shops should be sorted by **price** in ascending order, and in case of a tie, the one with the **smaller** <code>shop<sub>i</sub></code> should appear first. If there are less than 5 matching shops, then all of them should be returned. If no shop has an unrented copy, then an empty list should be returned.
*   **Rent**: Rents an **unrented copy** of a given movie from a given shop.
*   **Drop**: Drops off a **previously rented copy** of a given movie at a given shop.
*   **Report**: Returns the **cheapest 5 rented movies** (possibly of the same movie ID) as a 2D list `res` where <code>res[j] = [shop<sub>j</sub>, movie<sub>j</sub>]</code> describes that the <code>j<sup>th</sup></code> cheapest rented movie <code>movie<sub>j</sub></code> was rented from the shop <code>shop<sub>j</sub></code>. The movies in `res` should be sorted by **price** in ascending order, and in case of a tie, the one with the **smaller** <code>shop<sub>j</sub></code> should appear first, and if there is still tie, the one with the **smaller** <code>movie<sub>j</sub></code> should appear first. If there are fewer than 5 rented movies, then all of them should be returned. If no movies are currently being rented, then an empty list should be returned.

Implement the `MovieRentingSystem` class:

*   `MovieRentingSystem(int n, int[][] entries)` Initializes the `MovieRentingSystem` object with `n` shops and the movies in `entries`.
*   `List<Integer> search(int movie)` Returns a list of shops that have an **unrented copy** of the given `movie` as described above.
*   `void rent(int shop, int movie)` Rents the given `movie` from the given `shop`.
*   `void drop(int shop, int movie)` Drops off a previously rented `movie` at the given `shop`.
*   `List<List<Integer>> report()` Returns a list of cheapest **rented** movies as described above.

**Note:** The test cases will be generated such that `rent` will only be called if the shop has an **unrented** copy of the movie, and `drop` will only be called if the shop had **previously rented** out the movie.

**Example 1:**

**Input** ["MovieRentingSystem", "search", "rent", "rent", "report", "drop", "search"] [[3, [[0, 1, 5], [0, 2, 6], [0, 3, 7], [1, 1, 4], [1, 2, 7], [2, 1, 5]]], [1], [0, 1], [1, 2], [], [1, 2], [2]]

**Output:** [null, [1, 0, 2], null, null, [[0, 1], [1, 2]], null, [0, 1]]

**Explanation:** MovieRentingSystem movieRentingSystem = new MovieRentingSystem(3, [[0, 1, 5], [0, 2, 6], [0, 3, 7], [1, 1, 4], [1, 2, 7], [2, 1, 5]]); movieRentingSystem.search(1); // return [1, 0, 2], Movies of ID 1 are unrented at shops 1, 0, and 2. Shop 1 is cheapest; shop 0 and 2 are the same price, so order by shop number. movieRentingSystem.rent(0, 1); // Rent movie 1 from shop 0. Unrented movies at shop 0 are now [2,3]. movieRentingSystem.rent(1, 2); // Rent movie 2 from shop 1. Unrented movies at shop 1 are now [1]. movieRentingSystem.report(); // return [[0, 1], [1, 2]]. Movie 1 from shop 0 is cheapest, followed by movie 2 from shop 1. movieRentingSystem.drop(1, 2); // Drop off movie 2 at shop 1. Unrented movies at shop 1 are now [1,2]. movieRentingSystem.search(2); // return [0, 1]. Movies of ID 2 are unrented at shops 0 and 1. Shop 0 is cheapest, followed by shop 1.

**Constraints:**

*   <code>1 <= n <= 3 * 10<sup>5</sup></code>
*   <code>1 <= entries.length <= 10<sup>5</sup></code>
*   <code>0 <= shop<sub>i</sub> < n</code>
*   <code>1 <= movie<sub>i</sub>, price<sub>i</sub> <= 10<sup>4</sup></code>
*   Each shop carries **at most one** copy of a movie <code>movie<sub>i</sub></code>.
*   At most <code>10<sup>5</sup></code> calls **in total** will be made to `search`, `rent`, `drop` and `report`.

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Comparator;
import java.util.HashMap;
import java.util.Iterator;
import java.util.List;
import java.util.TreeSet;

@SuppressWarnings("java:S1172")
public class MovieRentingSystem {
    private static class Point {
        int movie;
        int shop;
        int price;

        public Point(int movie, int shop, int price) {
            this.movie = movie;
            this.shop = shop;
            this.price = price;
        }
    }

    private HashMap<Integer, TreeSet<Point>> unrentedMovies = new HashMap<>();
    private HashMap<String, Integer> shopMovieToPrice = new HashMap<>();
    private Comparator<Point> comparator =
            (o1, o2) -> {
                if (o1.price != o2.price) {
                    return Integer.compare(o1.price, o2.price);
                } else if (o1.shop != o2.shop) {
                    return Integer.compare(o1.shop, o2.shop);
                } else {
                    return Integer.compare(o1.movie, o2.movie);
                }
            };
    private TreeSet<Point> rented = new TreeSet<>(comparator);

    public MovieRentingSystem(int n, int[][] entries) {
        for (int[] entry : entries) {
            int shop = entry[0];
            int movie = entry[1];
            int price = entry[2];
            unrentedMovies.putIfAbsent(movie, new TreeSet<>(comparator));
            unrentedMovies.get(movie).add(new Point(movie, shop, price));
            shopMovieToPrice.put(shop + "+" + movie, price);
        }
    }

    public List<Integer> search(int movie) {
        if (!unrentedMovies.containsKey(movie)) {
            return new ArrayList<>();
        }
        Iterator<Point> iterator = unrentedMovies.get(movie).iterator();
        List<Integer> listOfShops = new ArrayList<>();
        while (iterator.hasNext() && listOfShops.size() < 5) {
            listOfShops.add(iterator.next().shop);
        }
        return listOfShops;
    }

    public void rent(int shop, int movie) {
        int price = shopMovieToPrice.get(shop + "+" + movie);
        rented.add(new Point(movie, shop, price));
        unrentedMovies.get(movie).remove(new Point(movie, shop, price));
    }

    public void drop(int shop, int movie) {
        int price = shopMovieToPrice.get(shop + "+" + movie);
        rented.remove(new Point(movie, shop, price));
        unrentedMovies.get(movie).add(new Point(movie, shop, price));
    }

    public List<List<Integer>> report() {
        List<List<Integer>> ans = new ArrayList<>();
        Iterator<Point> iterator = rented.iterator();
        while (iterator.hasNext() && ans.size() < 5) {
            Point point = iterator.next();
            ans.add(Arrays.asList(point.shop, point.movie));
        }
        return ans;
    }
}
```