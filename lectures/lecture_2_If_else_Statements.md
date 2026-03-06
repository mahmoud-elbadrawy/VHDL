# 📘 VHDL Course – Lecture 2: If-Else Statements

Welcome to **Lecture 2** of this VHDL learning series.
This lecture explains **conditional logic** using **if-elsif-else statements** in VHDL.

---

## 🎯 What is an If-Else Statement?

An **if-else statement** is a **conditional control structure** that allows a circuit to:
- Make decisions based on inputs
- Execute different logic based on conditions
- Choose between multiple outputs

📌 **Real-world analogy:**
> "If it's raining, take an umbrella. Else if it's sunny, wear sunscreen. Else, do nothing."

---

## 🧠 VHDL Conditional Statements

VHDL provides **three main conditional statements**:

1. **if-else**
2. **if-elsif-else**
3. **case-when** (covered in later lectures)

📌 In this lecture, we focus on **if-elsif-else** statements.

---

## 📋 If-Elsif-Else Syntax

### Basic Structure

```vhdl
if (condition_1) then
    -- Execute this if condition_1 is TRUE
    statement_1;
elsif (condition_2) then
    -- Execute this if condition_1 is FALSE and condition_2 is TRUE
    statement_2;
elsif (condition_3) then
    -- Execute this if previous conditions are FALSE and condition_3 is TRUE
    statement_3;
else
    -- Execute this if ALL previous conditions are FALSE
    statement_4;
end if;
```

### Key Points:
- **if** - First condition to check
- **elsif** - Alternative condition (can have multiple)
- **else** - Default action when all conditions are false
- **end if** - Marks the end of the conditional block

📌 Only **ONE** execution path is taken!

---

## 🔍 Understanding Conditions

A **condition** is an expression that evaluates to:
- **TRUE** (1)
- **FALSE** (0)

### Common Condition Types:

#### 1️⃣ Single Bit Comparison
```vhdl
if (signal = '1') then
    -- Execute if signal is HIGH
elsif (signal = '0') then
    -- Execute if signal is LOW
end if;
```

#### 2️⃣ Multi-bit Comparison
```vhdl
if (Sel = "00") then
    Y <= A;
elsif (Sel = "01") then
    Y <= B;
end if;
```

#### 3️⃣ Logical Operations
```vhdl
if (A = '1' and B = '1') then
    -- Both A AND B are HIGH
    Y <= '1';
elsif (A = '1' or B = '1') then
    -- Either A OR B is HIGH
    Y <= '1';
else
    Y <= '0';
end if;
```

---

## 🏗️ Priority Encoding

**Priority encoding** is a common pattern in VHDL.

It assigns **priority levels** to conditions:
- First condition has **highest priority**
- Last condition has **lowest priority**
- Only the highest priority TRUE condition executes

### Example:
```vhdl
if (Sel(0) = '1') then          -- Highest Priority
    Y <= A;
elsif (Sel(1) = '1') then       -- Medium Priority
    Y <= B;
elsif (Sel(2) = '1') then       -- Lower Priority
    Y <= C;
else                             -- Lowest Priority
    Y <= D;
end if;
```

📌 If **Sel(0) = '1'**, Y always gets A, regardless of Sel(1) or Sel(2).

---

## 🎨 Use Cases for If-Else

### 1️⃣ Multiplexers
Selecting one input from multiple inputs based on a selector signal.

### 2️⃣ Priority Encoders
Assigning priority to different input signals.

### 3️⃣ Arithmetic Operations
Performing different calculations based on control signals.

### 4️⃣ State Machines
Transitioning between different states based on conditions.

### 5️⃣ Error Handling
Checking for invalid inputs and handling them appropriately.

---

## 📝 Practical Example: Priority Multiplexer

The **If_else** project demonstrates a **4-to-1 priority multiplexer**.

### Design Specification:

**Inputs:**
- `A`, `B`, `C`, `D` – Four input signals (std_logic)
- `Sel` – 3-bit selector signal (std_logic_vector)

**Output:**
- `Y` – Selected output (std_logic)

### Priority Logic:
- **If** Sel(0) = '1' → Select A (Highest Priority)
- **Elsif** Sel(1) = '1' → Select B
- **Elsif** Sel(2) = '1' → Select C
- **Else** → Select D (Default)

### VHDL Implementation:

```vhdl
entity If_else is
port(
    A: in std_logic;
    B: in std_logic;
    C: in std_logic;
    D: in std_logic;
    Sel: in std_logic_vector(2 downto 0);
    Y: out std_logic
);
end If_else;

architecture Behavioral of If_else is
begin
    process ( A, B, C, D, Sel)
    begin
        if Sel(0) = '1' then 
            Y <= A;
        elsif Sel(1) = '1' then
            Y <= B;
        elsif Sel(2) = '1' then
            Y <= C;
        else 
            Y <= D; 
        end if; 
    end process;
end Behavioral;
```

### Process Block:
- **process** - Declares a sequential process
- **sensitivity list** - (A, B, C, D, Sel) - Triggers the process when these signals change
- **if-elsif-else** - Implements priority selection logic

---

## 🧪 Truth Table Example

| Sel(2) | Sel(1) | Sel(0) | Output |
|--------|--------|--------|--------|
| X      | X      | 1      | A      |
| X      | 1      | 0      | B      |
| 1      | 0      | 0      | C      |
| 0      | 0      | 0      | D      |

📌 X = don't care (can be 0 or 1)

**Notice:**
- Even if Sel = "111" (all bits = 1), output is A (highest priority)
- Sel(0) has highest priority, Sel(2) has lowest

---

## ⚠️ Important Considerations

### 1️⃣ Sensitivity List
Always include **all signals** that affect the output:
```vhdl
process ( A, B, C, D, Sel)  -- Include all inputs!
```

❌ **Wrong:**
```vhdl
process (Sel)  -- Missing A, B, C, D
```

### 2️⃣ Signal Assignment
Use **<=** (non-blocking assignment) inside processes:
```vhdl
Y <= A;  -- Correct
Y = A;   -- Wrong in simulation
```

### 3️⃣ Latch Inference
Always provide a **default value** in else clause to avoid latches:
```vhdl
else 
    Y <= D;  -- Without this, a latch might be inferred
end if;
```

---

## 💡 Comparison: If-Else vs Case

### If-Else (Priority Encoding)
- Best for **priority logic**
- Condition-based decisions
- Can check different signals

```vhdl
if Sel(0) = '1' then
    Y <= A;
elsif Sel(1) = '1' then
    Y <= B;
```

### Case Statement (Covered Later)
- Best for **enumerated choices**
- Cleaner when checking one signal
- Equal priority evaluation

```vhdl
case Sel is
    when "00" => Y <= A;
    when "01" => Y <= B;
end case;
```

---

## 🔄 Simulation Waveforms

### Test Scenario:
- **A = '1'**, B = '0', C = '0', D = '0'
- Selector changes: Sel = "001" → "010" → "100" → "000"

### Expected Output:
- Sel = "001": Y = A = '1' (priority to Sel(0))
- Sel = "010": Y = B = '0'
- Sel = "100": Y = C = '0'
- Sel = "000": Y = D = '0'

📌 Simulation helps verify priority logic is working correctly!

---

## ✅ Summary

- **If-elsif-else** provides conditional logic in VHDL
- **Priority encoding** assigns priority levels to conditions
- Only **ONE** path executes (the first TRUE condition)
- Always include **all signals** in sensitivity list
- Always provide **default values** to avoid latches
- **4-to-1 priority multiplexer** is a practical example

---
