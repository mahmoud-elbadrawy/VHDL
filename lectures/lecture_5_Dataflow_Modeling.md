# 📘 VHDL Course – Lecture 5: Dataflow Modeling

Welcome to **Lecture 5** of this VHDL learning series.
This lecture explains **dataflow modeling** - the most fundamental way to describe **combinational logic** in VHDL.

---

## 🎯 What is Dataflow Modeling?

**Dataflow modeling** describes the **flow of data** through the circuit using:
- **Concurrent signal assignments**
- **Boolean equations**
- **Logical operators**
- **Direct signal connections**

📌 **Key Concept:**
> Data flows from inputs to outputs through logical operations

---

## 🧠 Modeling Styles in VHDL

### 1️⃣ Dataflow Modeling (Concurrent)
```vhdl
architecture Dataflow of Example is
begin
    output <= input1 and input2;
    result <= a xor b or c;
end Dataflow;
```

### 2️⃣ Behavioral Modeling (Sequential)
```vhdl
architecture Behavioral of Example is
begin
    process(inputs)
    begin
        if condition then
            output <= value;
        end if;
    end process;
end Behavioral;
```

### 3️⃣ Structural Modeling (Component-based)
```vhdl
architecture Structural of Example is
    component AND_GATE port(...);
begin
    U1: AND_GATE port map(...);
end Structural;
```

📌 **Dataflow** is the **simplest and most intuitive** for combinational logic!

---

## 📋 Concurrent Signal Assignment

### Basic Assignment
```vhdl
signal_name <= expression;
```

### Key Points:
- **<=** is the assignment operator (not =)
- Assignment is **immediate and concurrent**
- Expression can include operators, functions, and other signals
- All assignments execute **simultaneously**

### Example:
```vhdl
result <= a and b;
sum <= x + y;
output <= input when enable = '1' else '0';
```

---

## 🔍 Understanding Dataflow

### How It Works:

1. **All assignments are evaluated simultaneously**
2. **Right-hand side** (RHS) expressions use **current signal values**
3. **Left-hand side** (LHS) signals get **new values**
4. **Delta delay** ensures proper ordering in simulation

### Circuit Perspective:
- Each assignment represents a **combinational logic block**
- Signals represent **wires** connecting logic blocks
- No memory elements (flip-flops) are inferred

---

## 🏗️ Practical Example: Direct Switch-to-LED Connection

The **Switches_LEDs** project demonstrates the **simplest dataflow modeling**.

### Design Specification:

**Inputs:**
- `switch1`, `switch2` – Two switch inputs (std_logic)

**Outputs:**
- `led1`, `led2` – Two LED outputs (std_logic)

### Functionality:
- **LED1** directly follows **Switch1**
- **LED2** directly follows **Switch2**

### VHDL Implementation:

```vhdl
entity Switches_LEDs is
port (
    switch1: in std_logic;
    switch2: in std_logic;
    led1: out std_logic;
    led2: out std_logic
);
end Switches_LEDs;

architecture Behavioral of Switches_LEDs is

begin

led1 <= switch1;
led2 <= switch2;

end Behavioral;
```

### Key Elements:
- **Direct assignment** - No logic operations
- **Concurrent execution** - Both assignments happen simultaneously
- **Wire-like behavior** - LEDs mirror switch states instantly
- **No process block** - Pure dataflow architecture

---

## 🎨 Dataflow Operators

### Logical Operators
```vhdl
and_result <= a and b;
or_result <= a or b;
xor_result <= a xor b;
not_result <= not a;
nand_result <= a nand b;
nor_result <= a nor b;
xnor_result <= a xnor b;
```

### Relational Operators
```vhdl
equal <= (a = b);
not_equal <= (a /= b);
greater <= (a > b);
less <= (a < b);
greater_equal <= (a >= b);
less_equal <= (a <= b);
```

### Arithmetic Operators
```vhdl
sum <= a + b;
difference <= a - b;
product <= a * b;
quotient <= a / b;
remainder <= a mod b;
power <= a ** 2;
```

### Concatenation
```vhdl
combined <= a & b & c;  -- Concatenate signals
vector <= "00" & input; -- Concatenate with literals
```

---

## 💡 Complex Dataflow Examples

### 1️⃣ Combinational Logic
```vhdl
-- Half Adder
sum <= a xor b;
carry <= a and b;

-- Full Adder
sum <= a xor b xor cin;
cout <= (a and b) or (a and cin) or (b and cin);
```

### 2️⃣ Multiplexer
```vhdl
-- 2-to-1 MUX
output <= input0 when select = '0' else input1;

-- 4-to-1 MUX
output <= input0 when select = "00" else
          input1 when select = "01" else
          input2 when select = "10" else
          input3;
```

### 3️⃣ Decoder
```vhdl
-- 2-to-4 Decoder
enable0 <= '1' when address = "00" else '0';
enable1 <= '1' when address = "01" else '0';
enable2 <= '1' when address = "10" else '0';
enable3 <= '1' when address = "11" else '0';
```

### 4️⃣ ALU Operations
```vhdl
-- Simple ALU
result <= a + b when operation = "00" else
          a - b when operation = "01" else
          a and b when operation = "10" else
          a or b;
```

---

## ⚡ Dataflow vs Behavioral Modeling

### Dataflow (Concurrent)
```vhdl
architecture Dataflow of ALU is
begin
    result <= a + b when op = "00" else
              a - b when op = "01" else
              a and b;
end Dataflow;
```

**Advantages:**
- ✅ **Intuitive** - Direct hardware representation
- ✅ **Concurrent** - All operations happen simultaneously
- ✅ **Efficient** - Minimal simulation overhead
- ✅ **Readable** - Clear data flow

**Disadvantages:**
- ❌ **Limited** - Cannot describe sequential logic
- ❌ **No variables** - Only signal assignments

### Behavioral (Sequential)
```vhdl
architecture Behavioral of ALU is
begin
    process(a, b, op)
    begin
        case op is
            when "00" => result <= a + b;
            when "01" => result <= a - b;
            when others => result <= a and b;
        end case;
    end process;
end Behavioral;
```

**Advantages:**
- ✅ **Flexible** - Can describe any logic
- ✅ **Variables** - Temporary storage
- ✅ **Sequential** - Can model state machines

**Disadvantages:**
- ❌ **Overhead** - Process management
- ❌ **Less intuitive** - Software-like thinking

---

## 🔄 Simulation Behavior

### Delta Delay Concept:
- **Concurrent assignments** have **zero simulation time**
- **Delta delays** ensure proper evaluation order
- **Multiple evaluations** may occur in same time step

### Example Timeline:
```
Time 0ns: switch1 changes to '1'
  → Delta 1: led1 <= switch1 ('1')
  → Delta 2: All assignments complete

Time 10ns: switch2 changes to '0'
  → Delta 1: led2 <= switch2 ('0')
  → Delta 2: All assignments complete
```

📌 **Immediate response** - No propagation delay in simulation!

---

## ⚠️ Important Considerations

### 1️⃣ Signal Declaration
All signals must be declared in architecture:
```vhdl
architecture Dataflow of Example is
    signal intermediate: std_logic;
begin
    intermediate <= a and b;
    output <= intermediate or c;
end Dataflow;
```

### 2️⃣ Type Consistency
All operands in expressions must be compatible:
```vhdl
-- Correct:
result <= a and b;  -- Both std_logic

-- Wrong:
result <= a and "00";  -- Type mismatch
```

### 3️⃣ Multiple Drivers
A signal can have **only one driver** in dataflow:
```vhdl
-- Wrong (multiple drivers):
output <= a when sel = '0' else '0';
output <= b when sel = '1' else '0';

-- Correct (single assignment):
output <= a when sel = '0' else b;
```

### 4️⃣ Combinational Loops
Avoid feedback loops that create oscillators:
```vhdl
-- Wrong (combinational loop):
q <= not q;  -- Creates infinite loop

-- Correct (with clock):
process(clock)
begin
    if rising_edge(clock) then
        q <= not q;
    end if;
end process;
```

---

## 🧪 Testing Dataflow Designs

### Switch-LED Test Cases:
- **switch1 = '0', switch2 = '0'** → led1 = '0', led2 = '0'
- **switch1 = '1', switch2 = '0'** → led1 = '1', led2 = '0'
- **switch1 = '0', switch2 = '1'** → led1 = '0', led2 = '1'
- **switch1 = '1', switch2 = '1'** → led1 = '1', led2 = '1'

### Waveform Verification:
```
Time: 0ns   10ns  20ns  30ns
switch1: 0     1     0     1
switch2: 0     0     1     1
led1:    0     1     0     1
led2:    0     0     1     1
```

📌 **Perfect mirroring** - LEDs follow switches instantly!

---

## 🎯 Hardware Synthesis

Dataflow modeling generates **pure combinational circuits**:

```
switch1 ───── led1
switch2 ───── led2
```

### Synthesis Results:
- **No flip-flops** - Only combinational logic
- **Direct connections** - Wires between switches and LEDs
- **Zero latency** - Immediate signal propagation
- **Resource efficient** - Minimal FPGA resources

---

## 💡 Best Practices

### 1️⃣ Clear Signal Naming
```vhdl
-- Good:
sum <= a + b;
carry <= a and b;

-- Bad:
s <= x + y;
c <= x and y;
```

### 2️⃣ Group Related Assignments
```vhdl
-- Half Adder
sum <= a xor b;
carry <= a and b;

-- Full Adder
sum <= a xor b xor cin;
cout <= (a and b) or (cin and (a xor b));
```

### 3️⃣ Use Parentheses for Clarity
```vhdl
-- Clear precedence:
result <= (a and b) or (c and d);

-- Ambiguous:
result <= a and b or c and d;
```

### 4️⃣ Comment Complex Expressions
```vhdl
-- 4-bit parity calculation
parity <= data(0) xor data(1) xor data(2) xor data(3);
```

---

## ✅ Summary

- **Dataflow modeling** uses **concurrent signal assignments**
- Describes **combinational logic** with Boolean equations
- **Immediate and parallel** execution of all assignments
- **Direct hardware mapping** - intuitive for digital circuits
- **No memory elements** - pure combinational behavior
- **Efficient synthesis** - minimal resource usage
- Perfect for **simple logic** like the switch-to-LED example

---
