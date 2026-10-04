# How Computers Work and the Binary Numeral System

Computers are used in many fields today. In this article, you can find how computers follow a path when performing a process and the methods they use while doing so.

## The Binary Numeral System (Binary)

Computers mostly operate using the binary numeral system. Of course, exceptions like quantum computers need to be ignored here. Computers process all operations by converting them into binary format, which is expressed with values of 0 and 1. The source of this is that the processors in computers perform operations using circuit elements called transistors. You can find more details about this in the article about transistors.

## The Decimal Numeral System (Decimal)

The expression Decimal is the numeral system we use to say numbers today. In fact, without realizing it, we use the base-10 numeral system in daily life. However, since this has no direct equivalent in computer processors, we need to convert these numbers.

## Binary => Decimal Conversions

To convert a number from the binary numeral system into a decimal base that we can understand, we do the following sequentially:

- Starting from the end of the number, we write powers of 2, increasing the power by 1 for each step to the left. Here, we start the power from 0.
- Then, for each digit, we multiply the number by the value underneath it.
- Finally, the sum of all the results we find gives us the equivalent of that binary number in the decimal system.

To make it clearer, you can find the image below. As an example, an expression like 0111 is used here, but this is not a full byte example; it is a half-byte example. You can find the details regarding this in the article about bits and bytes.

![image](https://github.com/user-attachments/assets/beb9a5f4-0cfd-49fe-9a75-09e74d9c718f)

## Decimal => Binary Conversions

To convert a number in the decimal system to the binary system, we can look at which powers of 2 the number can be written as the sum of, and verify it using the same method.

## How Computers Use This

Computer processors perform the above-mentioned conversions many times per second. While doing this, they use the numbers 0 and 1 to dictate whether current will pass through a transistor. They take these as inputs in different types from the user and convert them into numbers understood by computers. Some of these conversions are as follows:

- Decimal => Binary
- Text => Binary (With the help of the ASCII Table)
- Image => Binary (With the help of encodings like base64)
- Audio => Binary (By looking at certain parameters of the sound, e.g., volume level, pause points, etc.)

In short, computers use 0s and 1s while displaying any media to us. In fact, we can say they use them to do every task. For instance, the reason we can read this text properly right now is the numbers 0 and 1.
