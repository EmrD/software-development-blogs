# How the CPU Works: Buses, ALU, and Control Unit

When a computer executes a program, the central processing unit (CPU) acts as the brain of the system. However, the CPU cannot perform tasks in isolation—it relies on internal components to process data and dedicated communication pathways, called **buses**, to exchange information with memory and input/output (I/O) devices.

In this article, you can find how data moves through a computer system via system buses and how the core CPU components process those signals.

## System Buses: The CPU's Highway System

A **bus** is a collection of physical wires or conductive traces on a circuit board that carries electrical signals between the CPU, memory (RAM), and peripherals. System buses are split into three main types based on the type of information they carry.


```

```
   +---------------------------------------------+
   |                  Control Bus                |
   +---------------------------------------------+

```

[CPU] <================== Address Bus ==> [Memory / Devices]
<====== Data Bus =================>

```

### 1. Data Bus

The Data Bus transfers the actual data being processed or the program instructions between the CPU, RAM, and external devices.

- **Bidirectional:** Data can travel in both directions (e.g., from RAM into the CPU during a *read* operation, or from the CPU into RAM during a *write* operation).
- **Width Matters:** The width of the data bus (measured in bits, such as 32-bit or 64-bit) determines how much data the CPU can transfer in a single clock cycle.

### 2. Address Bus

Before reading or writing data, the CPU must specify *where* that data is located in memory or an I/O device. The Address Bus carries these memory addresses from the CPU to RAM.

- **Unidirectional:** Addresses travel only in one direction—from the CPU outward to memory or peripherals.
- **Addressing Capacity:** The width of the address bus determines the maximum amount of physical RAM the CPU can access ($2^n$ memory locations, where $n$ is the number of address lines).

### 3. Control Bus

While the address bus specifies the location and the data bus carries the content, the Control Bus manages and synchronizes the entire operation by carrying control signals.

- **Bidirectional & Mixed:** Signals can originate from the CPU or be sent back to the CPU by hardware devices.
- **Common Control Signals:**
  - **Memory Read / Write:** Indicates whether data should be fetched from or saved to memory.
  - **Clock Signals:** Synchronizes timing across hardware.
  - **Interrupt Signals:** Allows hardware devices to request immediate CPU attention.
  - **Bus Request / Grant:** Coordinates which component has control of the system bus.

---

## Core CPU Components: ALU and Control Unit

Inside the CPU, incoming signals from these buses are processed by two main structural blocks: the **Arithmetic Logic Unit (ALU)** and the **Control Unit (CU)**.


```

```
              +-------------------------+
              |       Control Unit      |
              |           (CU)          |
              +------------+------------+
                           | Controls & Directs
                           v

```

+--------------+          +------------+          +--------------+
| Input Data   | =======> |    ALU     | =======> | Output Data  |
| (via Bus)    |          +------------+          | (via Bus)    |
+--------------+                                  +--------------+

```

### The Arithmetic Logic Unit (ALU)

The ALU is the CPU's primary execution unit. It receives numeric data from internal registers and performs arithmetic and logical computations on them.

1. **Arithmetic Operations:** Basic math functions such as addition, subtraction, multiplication, and division.
2. **Logic Operations:** Boolean logic comparisons such as `AND`, `OR`, `NOT`, and `XOR`.
3. **Bit-Shift Operations:** Moving bits to the left or right inside a register to multiply, divide, or isolate specific flags.

Once the calculation is completed, the ALU stores the output back into registers or sends it out to system memory via the Data Bus.

### The Control Unit (CU)

If the ALU is the calculator of the CPU, the Control Unit is the manager. The CU does not perform mathematical calculations itself; instead, it directs the flow of data through the CPU and coordinates all operations using the **Instruction Cycle**.

1. **Fetch:** Retrieves the next instruction from memory via the Address and Data buses.
2. **Decode:** Interprets what action the instruction requires (e.g., "Add Register A to Register B").
3. **Execute:** Signals the ALU, registers, or external devices over the Control Bus to perform the action.
4. **Store:** Directs the resulting output back to a register or memory location.

---

## How It All Works Together: A Quick Example

To see how these components interact, consider a simple operation: **loading a number from RAM and adding it to an existing value**.

1. **Address Bus:** The Control Unit puts the RAM address of the target number onto the Address Bus.
2. **Control Bus:** The CU sends a "Memory Read" signal through the Control Bus.
3. **Data Bus:** RAM receives the signal, reads the address, and places the stored value onto the Data Bus, transferring it into CPU registers.
4. **ALU Processing:** The CU instructs the ALU to add this new number to an existing register value.
5. **Completion:** The ALU computes the result, and the CU routes the output back to a register or outputs it to memory via the Data Bus.

---

## Summary

Understanding system architecture comes down to seeing how signals move and process:

- **Address Bus:** Specifies *where* data goes (Unidirectional).
- **Data Bus:** Carries *what* data is moved (Bidirectional).
- **Control Bus:** Dictates *when* and *how* operations occur.
- **Control Unit (CU):** Directs traffic and manages the instruction lifecycle.
- **Arithmetic Logic Unit (ALU):** Performs the actual math and logic calculations.

```
