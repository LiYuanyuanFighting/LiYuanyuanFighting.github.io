---
layout: post
title: Algorithm---8 Puzzle(Priority Queues)
categories: [blog ]
tags: [study,java,sort,priorityQueue ]
description: Good algorithm is much more efficienet than spending more money and time
---  

In the assignment, it introduces an algorithm----A*, after checking it, I just realized
how widely use it is.
**A*(A star)** is a computer algorithm widely used in pathfinding and graph traversal.   
The simplest (and least efficient) method of traversing a graph is the **Depth First Search**   
(an algorithm starts at the root and explores as far as possible along each branch before    
backtracking. In some area, the graph to be traversed is often either too large to visit  
in its entirety or infinite, then DFS will suffer non-termination).  

Pathfinders enable us to plan ahead rather than waiting until the last moment to discover   
there is a problem.

**backtrack**   
In backtracking algorithms, try to build one step at a time. If at some step it become clear
that the current path that you are on cannot lead to a solution you go back to. Common example   
is Eight qqueens puzzle:
    queens = [0,0...,0]
    for i=1 to maxRows:
        for j=1 to maxRows:
            queens[i] = queens[i]++
            if queensNotUnderAttack(i) then:
                break
            else :
                if j == maxRows then: i--

[![situation without using proper algorithm](http://imgur.com/VWcbNF9 "situation without using proper algorithm")](http://imgur.com/VWcbNF9 "situation without using proper algorithm")
[![extend a movement algorithm](http://imgur.com/fWX98sa "extend a movement algorithm")](http://imgur.com/fWX98sa "extend a movement algorithm")

Dijikstra's algorithm works well to find the shorted path, but it wastes time in exploring directions  
that aren't promising. Greedy Best First Search algorithm selects the vertex closest to the goal, exploring  
in promising directions but it may not find the shortest path. The A* algorithm uses both the actual    
distance from the start and the estimated distance to the goal.  

Compare the algorithms: Dijkstra’s Algorithm calculates the distance from the start point. Greedy   
Best-First Search estimates the distance to the goal point. A* is using the sum of those two distances.  

[reference1](http://theory.stanford.edu/~amitp/GameProgramming/AStarComparison.html)
#### Notes  
(1)To calculate Manhattan priority is to calculate the sum of the distance in vertical and horizontal.
(2)Unsolvable case: if the initial board can become into global board by swapping any pair of blocks.(prove!)
(3)The length of 2 dimension array:   
     public static void main(String[] args) {
    
        int[][] foo = new int[][] {
            new int[] { 1, 2, 3 },
            new int[] { 1, 2, 3, 4},
        };
    
        System.out.println(foo.length); //2
        System.out.println(foo[0].length); //3
        System.out.println(foo[1].length); //4
    }

#### Problems    
1. how to create priority queue and game tree, in this case, one board cann have more than 2 neighbors,   
how to express this or should use not binary tree? To construct priority queue, should have the key, should
the priority be the key? If this, how to find the right path under the condition that parent is not real parant?

Analysis:  
Follow the steps: 1st create an inner class called gameNode to which includes current board, previous board,  
isTwin, move num which is enough to trace the solution  
2nd create 1 minPQ which is a binary tree implementation using class methods provided, add the   
initial and the twin of the initial  
3rd using minPQ.delMin() to find the node with lowest priority(sum of manhatton number and moves is smallest)  
which can guarantee smallest move, then check each of its neighbors, add them into the queue then repeat this   
step till find the node A which has board equals to the goal.  
4th check if the attribute isTwin of node A is true, then it meets the unsolvable condition, so no solution.

But still don't understand why that case(after switching any pair of initial can become into goal) is unsolvable  
and why add the twin of initial into queue????

2. decide unsolvable puzzles, how to solve the "any"?

We can only reach the goal state from a state which has even inversion count. The idea is based  
on the fact that the parity of inversions remains same after a set of moves, i.e. if the inversion  
count is even, then it remains even after any sequence of moves.   
So we just need to invert any pair in the same row(1 inversion), if the inverted one can reach the  
goal, it will mean the puzzle is unsolvable

#### Mistakes
1.  The operator == is undefined for the argument type(s) int, null:  

Primitive types(int here) in Java cannot be null. If you want to check for 0, do a != 0.  
int cannot be compared to null, which is a null reference to an object  
Similarly, cannot invoke equals(int) on the primitive type int

2. To exit 2 nest loops  
in java, can use label to specify which loop to break
    mainLoop:
    while (goal <= 100) {
       for (int i = 0; i < goal; i++) {
          if (points > 50) {
             break mainLoop;
          }
          points += i;
       }
    }

3. To create neighbors, I used only one array for 4 cases(left, right, up, down), even though
I added them inside the stack in order, but the content change of array in any neighbor will
influence others, because of the same array

4. My swap method as following is wrong:
        private void swap(int a, int b) {
            int temp = 0;
            temp = a;
            a = b;
            b = temp;
        }
it won't change anything because java is passing-by-value. Instead can do like this:   
    void swap(int[][] array, int row1, int col1, int row2, int col2) {
        int temp = array[row1][col1];
        array[row1][col1] = array[row2][col2];
        array[row2][col2] = temp;
    }

5.Stores a reference to an externally mutable object in the instance variable   
[reference1](http://stackoverflow.com/questions/34109363/how-can-we-maintain-immutability-of-a-class-with-a-mutable-referrence)
my code is:  
     private final int n;
        private int[][] blocks; // should make a new array to keep original immutable or not?
        public Board(int[][] blocks)           // construct a board from an n-by-n array of blocks
        {
            if (blocks.length == 0)
                throw new java.lang.NullPointerException();
            this.n = blocks.length;
            this.blocks = blocks;
        }
can change like this:   
    private int[][] matrix; // blocks
        private int N; // deimension
        private int posX, posY; // 0' position 
        
        // construct a board from an N-by-N array of blocks
        // (where blocks[i][j] = block in row i, column j)
        public Board(int[][] blocks) {
            N = blocks.length;
            matrix = new int[N][N];
            for (int i = 0; i < N; i++) {
                for (int j = 0; j < N; j++) {
                    matrix[i][j] = blocks[i][j];
                    if (matrix[i][j] == 0) {
                        posX = i;
                        posY = j;
                    }
                }
            }
        }

6.OperationCountLimitExceededException. Number of calls to methods in Board exceeds limit: 100000000   
Analysis:   
Originally, I used 4 double for loops to get the 4 possible neighbors which is really time consuming...   
Solution:   
(1) just use the index directly to get the neighbors
(2) can use a trick:  
    int[] dx = {0, 0, -1, 1};
            int[] dy = {1, -1, 0, 0};
            for (int i = 0; i < 4; i++) {
                int x = posX + dx[i];
                int y = posY + dy[i];
which stores possible neighbor index

In fact the main reason is my mistake here: 
public int compareTo(GameNode that) {   
            int priority1 = this.board.manhattan() + this.moves;   
            int priority2 = this.board.manhattan() + that.moves;  
for priority2, it should be that.board.manhattan(), so before it was difficult to find a real GameNode  
object in pq.delMin(), then easily operate on the wrong one.   
This error took me one week.....

Share one trick to delete the line number when copy codes from websites:  
use word software: alt + mouse left click to select the line number area, then can cut it.


