# What is a Linked List?

Linked lists can be analyzed in 4 main groups:

- Singly Linked List
- Doubly Linked List
- Circular Linked List (means circular linked list, which can be singly or doubly linked)

Fundamentally, it keeps multiple data elements together in memory by taking references of the necessary elements in required places to perform operations such as searching, sorting, inserting, and deleting data.

## Singly Linked List

A singly linked list is the most frequently used type among linked lists and features a more foundational structure compared to the others. Basically, 2 nodes are held within 1 index: one stores the actual value of the data at that index, while the other node holds the reference to the value located at the next index. If the corresponding index is the last index, the next value is assigned as NULL. You can find its visual representation below:

![image](https://github.com/user-attachments/assets/99cee3d8-a059-4afd-9648-0c22866f9098)

## Doubly Linked List

A doubly linked list contains a minor difference compared to a singly linked list. Unlike a singly linked list, there are 3 different nodes within a single index. These are respectively:

- Prev
- Value
- Next

Here, prev holds the address of the previous node. The value part holds the data at that index, and the next part references the value part of the next index, just like in a singly linked list. If the corresponding index is 0, the prev value is assigned as NULL; if it is the last index, the next value is assigned as NULL. Visualized version:

![image](https://github.com/user-attachments/assets/96315fb7-c2b3-43a2-a1e5-2bdbbf459727)

## Circular Linked List

In circular linked lists, the next pointer of the last node (tail) points to the node at the beginning of the list (head). For example, while the prev value of index 0 in a doubly linked list would normally be NULL, in a circular linked list, the prev value of index 0 references the value of the last index. Similarly, the next value on the last index takes the reference of the value at index 0. In a singly circular linked list, the only difference is that there is simply no PREV section. Visualized version:

![image](https://github.com/user-attachments/assets/f85e2167-9e1b-478a-b4b0-e4df84f2acbd)

## Where are Linked Lists Used?

Linked lists are particularly used in places where the previous / next action needs to be tracked. The forward and backward buttons found in web browsers generally rely on this foundation.
