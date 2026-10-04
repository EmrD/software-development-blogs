# Hash Table

A **Hash Table** is a data structure that maps key-value pairs. Hash tables are frequently used due to their high speed, especially in search queries. By using a **primary hash function**, keys are converted into indices within an array, and values are stored at these indices.

## Working Principle of a Hash Table
1. A key is sent to a defined **primary hash function**.
2. The hash function converts the key into an index within an array.
3. The value is saved at the calculated index.
4. When multiple keys generate the same index (collision), techniques such as **separate chaining** or **linear probing** are used.

## What to Do in Case of Index Collision on a Hash Table
1. Separate Chaining
2. Linear Probing
3. Quadratic Probing
4. Double Hashing
5. Plus 3<br>
...

## Separate Chaining
In this collision resolution method, the main goal is to create a linked-list at the colliding index to hold two or more items in a `value => next` structure. For example, the number 4 wants to be placed at index 4. However, later, the number 14 will also get index 4 in a array of size 10. In this case, the following structure is formed:

```4 - 14 => 14 - NULL```

## Linear Probing
In this method, the colliding index value is increased by a certain rate, and the resulting value becomes the new index. For example, in an array of size 10:

```h(x) = x % 10```

be the primary function. In this case, the numbers **4** and **14** will yield the same index value. For calculating the index of the newly added number:

```f(index) = index + 1```

a function like this is used. The increment amount here varies depending on the data structures and algorithms used in the project. For example, for the number 14:

```f(h(14)) = 5```

yields the result. Therefore, the number 14 is placed at index 5. This process continues to increment until an empty slot is found.

# Disadvantages of Linear Probing
One of the biggest disadvantages of the Linear Probing structure is that it creates clustering at certain index values on the hash table, failing to write any data to previous or intermediate indices. This leads to inefficient placement of data.

## Quadratic Probing
In this structure, the goal is to somewhat disperse the clustering at certain indices caused by Linear Probing. This is done by linearly incrementing a variable such as i. For example, the number 4 is at index 4. The number 14 also attempts to land on index 4 in an array of size 10. In this case, a function like this emerges:

```f(pre_index) = (pre_index + i^2) mod 10```

therefore;

```f(4) = (4 + 1*1) mod 10 = 5```

(The value of i, in most cases, increases linearly starting from 1. It is updated to 2 in the next iteration.)

## Double Hashing
The goal in this structure, like Quadratic Probing, is to distribute the data evenly. However, it does this by passing it through an extra user-defined function. Let the primary function for 4 and 14 be:

```h(x) = x mod size```

For size 10;

```h(4) = 4 mod 10 = 4```. placed at index 4.

For 14;

```h(14) = 14 mod 10 = 4```. ***wants*** to be placed at index 4.

In this case, a new index can be obtained by passing it through a user-defined function like this:

```f(x) = ((x + x) - (x/2)) mod size```

New index for 14;

```f(14) = (14 + 14) - (14/2) mod 10 = 21 mod 10 = 1```. index 1 becomes the new index value for the number 14.

## Plus 3
In this structure, if a collision occurs on an index, 3 is added to the index value of the new element to be added, and it is placed at that index. For 4 and 14:

The number 4 is placed at index 4.
The number 14 **under normal conditions** would be placed at index 4. However, when it is full, it is placed at index `4 + 3 = 7`. The number 3 here is given as an average value to somewhat prevent index clustering. **However, it is not part of standard terminology.**
