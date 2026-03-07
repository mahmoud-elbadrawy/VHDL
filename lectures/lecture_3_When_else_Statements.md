# 📘 VHDL Course – Lecture 3: When-Else Statements

Welcome to **Lecture 3** of this VHDL learning series.
This lecture explains **conditional signal assignment** using **when-else statements** in VHDL.

---

## 🎯 What is a When-Else Statement?

A **when-else statement** is a **concurrent conditional signal assignment**.

It allows:
- Conditional assignment of values to signals
- Clean and readable syntax
- Implementation of combinational logic outside a process

📌 **Key Difference from if-elsif-else:**
- **if-elsif-else** → Used inside processes (sequential)
- **when-else** → Used outside processes (concurrent)

---

## 🧠 Concurrent vs Sequential Logic

### Sequential Logic (Process-based)
```vhdl
process (inputs)
begin
    if condition_1 then
        output <= value_1;
    elsif condition_2 then
        output <= value_2;
    else
        output <= value_3;
    end if;
end process;
```

### Concurrent Logic (When-else)
```vhdl
output <= value_1 when condition_1 else
          value_2 when condition_2 else
          value_3;
```

📌 Both describe **combinational logic** in this context!

---

## 📋 When-Else Syntax

### Basic Structure

```vhdl
signal_assignment <= value_1 when condition_1 else
                     value_2 when condition_2 else
                     value_3 when condition_3 else
                     default_value;
```

### Key Points:
- **when** - Condition to evaluate
- **else** - What to assign if the previous condition is FALSE
- Order matters - **First TRUE condition** takes priority
- Must provide a **default value** at the end (no final else required, but recommended)

### Example:
```vhdl
z <= A when assign_A = '1' else
     B when assign_B = '1' else
     C;
```

📌 This is a **three-way conditional assignment**, not a multiplexer!

---

## 🔍 Understanding When-Else

### How It Evaluates:

1. Check **condition_1**
   - If TRUE → Assign **value_1**
   - Go to END
2. If condition_1 is FALSE, check **condition_2**
   - If TRUE → Assign **value_2**
   - Go to END
3. If condition_2 is FALSE, check **condition_3**
   - If TRUE → Assign **value_3**
   - Go to END
4. If all conditions FALSE → Assign **default_value**

### Priority Encoding:
**Only the FIRST TRUE condition executes!**

### Example:
```vhdl
Z <= A when X = '1' else
     B when Y = '1' else
     C;
```

If **X = '1' and Y = '1'**, then **Z = A** (X has priority)

---

## 🏗️ Practical Example: Priority Assignment

The **When_else** project demonstrates **conditional signal assignment**.

### Design Specification:

**Inputs:**
- `A`, `B`, `C` – Three input signals (std_logic)
- `assign_A` – Select signal for A (std_logic)
- `assign_B` – Select signal for B (std_logic)

**Output:**
- `z` – Assigned signal (std_logic)

### Selection Logic:
- **If** assign_A = '1' → Select A (Highest Priority)
- **Elsif** assign_B = '1' → Select B (Medium Priority)
- **Else** → Select C (Default)

### VHDL Implementation:

```vhdl
entity when_else is
port (
    A: in std_logic;
    B: in std_logic;
    C: in std_logic;
    assign_A: in std_logic;
    assign_B: in std_logic;
    z: out std_logic
);
end when_else;

architecture Behavioral of when_else is

begin
    z <= A when assign_A = '1' else
         B when assign_B = '1' else
         C;

end Behavioral;
```

### Concurrent Assignment:
- **No process block** - Direct hardware implementation
- **Evaluated continuously** - Whenever inputs change
- **Combinational logic** - No state memory

---

## 🧪 Truth Table Example

| assign_A | assign_B | Output |
|----------|----------|--------|
| 1        | X        | A      |
| 0        | 1        | B      |
| 0        | 0        | C      |

📌 X = don't care (can be 0 or 1)

**Notice:**
- Even if assign_A = '1' and assign_B = '1', output is A
- assign_A has highest priority
- assign_B has medium priority
- C is the default fallback

---

## 🎨 Use Cases for When-Else

### 1️⃣ Simple Multiplexers
Selecting one input from multiple inputs based on conditions.

```vhdl
output <= input_0 when select = '0' else
          input_1;
```

### 2️⃣ Conditional Value Assignment
Assigning different values based on signal conditions.

```vhdl
LED <= RED when danger = '1' else
       YELLOW when warning = '1' else
       GREEN;
```

### 3️⃣ Priority Logic
Implementing priority-based selection.

```vhdl
data <= high_priority when ready_hi = '1' else
        low_priority when ready_lo = '1' else
        default_data;
```

### 4️⃣ Status Indication
Selecting status based on multiple flags.

```vhdl
status <= "IDLE" when idle = '1' else
          "BUSY" when busy = '1' else
          "ERROR";
```

---

## ⚡ When-Else vs If-Elsif-Else

### When-Else (Concurrent)
```vhdl
z <= A when condition_1 else
     B when condition_2 else
     C;
```

**Advantages:**
- ✅ Cleaner syntax
- ✅ More readable
- ✅ No process overhead
- ✅ Explicitly combinational

**Disadvantages:**
- ❌ Limited to signal assignment
- ❌ Cannot perform complex sequential logic

### If-Elsif-Else (Sequential)
```vhdl
process (A, B, C, condition_1, condition_2)
begin
    if condition_1 then
        z <= A;
    elsif condition_2 then
        z <= B;
    else
        z <= C;
    end if;
end process;
```

**Advantages:**
- ✅ More flexible
- ✅ Can perform complex logic
- ✅ Can generate state machines

**Disadvantages:**
- ❌ More verbose
- ❌ Requires process block
- ❌ Less clear intent

---

## 📊 Comparison with Case-When

### When-Else (Concurrent)
- **Condition-based** - Evaluates arbitrary conditions
- **Priority encoded** - First TRUE condition wins
- **Flexible logic** - Can check different signals

```vhdl
output <= A when enable = '1' else
          B when reset = '1' else
          C;
```

### Case-When (Concurrent)
- **Value-based** - Checks single signal against multiple values
- **Equal priority** - All cases evaluated equally
- **Enumerated checks** - Compares to specific values

```vhdl
case select is
    when "00" => output <= A;
    when "01" => output <= B;
    when others => output <= C;
end case;
```

📌 **When to use:**
- **When-else** → Multiple conditions on different signals
- **Case-when** → Single signal with multiple values

---

## 🔄 Simulation Waveforms

### Test Scenario:
- **A = '1'**, B = '0', C = '1'
- Signals change: assign_A = '1' → '0' → '1' → '0' (with assign_B transitions)

### Expected Behavior:

| Time | assign_A | assign_B | Output (z) |
|------|----------|----------|------------|
| 0    | 1        | 0        | A = '1'    |
| 10   | 0        | 1        | B = '0'    |
| 20   | 0        | 0        | C = '1'    |
| 30   | 1        | 1        | A = '1'    |

📌 Output changes **immediately** when inputs change (combinational logic)!

---

## ⚠️ Important Considerations

### 1️⃣ Concurrent Execution
When-else statements are **evaluated continuously**:
```vhdl
-- ALL these execute simultaneously
a <= x when enable else 0;
b <= y when enable else 0;
c <= z when enable else 0;
```

### 2️⃣ Always Provide Default Value
Without a default, the signal's previous value is retained (latch):
```vhdl
z <= A when assign_A = '1' else
     B when assign_B = '1' else
     C;  -- C is the default!
```

### 3️⃣ Signal vs Variable
When-else assignments use **signals** (<=), not variables (=):
```vhdl
z <= A when condition else B;  -- Correct

-- Inside process:
temp := A when condition else B;  -- Correct (variable)
```

### 4️⃣ Type Consistency
All assigned values must be the **same type**:
```vhdl
z <= A when condition else B;  -- A and B same type

-- Wrong:
z <= A when condition else "00";  -- Different types
```

---

## 🎯 Hardware Interpretation

When-else statements generate **combinational logic circuits**:

```
         ┌─────────────┐
assign_A─┤             │
         │ Multiplexer │──── z
assign_B─┤   Logic     │
A,B,C  ──┤             │
         └─────────────┘
```

### Circuit Implementation:
- **No memory elements** - No flip-flops
- **No state** - Outputs depend only on current inputs
- **Immediate updates** - Output updates as soon as inputs change
- **Propagation delay** - Small delay due to gate delays

---

## 💡 Style Recommendations

### Good Style:
```vhdl
output <= input_0 when select = "00" else
          input_1 when select = "01" else
          input_2 when select = "10" else
          input_3;
```

### Alignment for Readability:
```vhdl
result <= value_A when (condition_A and flag) else
          value_B when condition_B else
          value_default;
```

### Comments for Complex Logic:
```vhdl
z <= A when (high_priority = '1') else      -- Priority 1
     B when (medium_priority = '1') else    -- Priority 2
     C;                                      -- Default
```

---

## ✅ Summary

- **When-else** is a **concurrent conditional assignment**
- Used for **combinational logic** outside processes
- **Priority encoding** - First TRUE condition wins
- Simpler and more readable than if-elsif-else for single assignments
- Different from **case-when** (check specific values vs arbitrary conditions)
- Always provide a **default value** to avoid latches
- Generates **combinational circuits** with no memory

---
