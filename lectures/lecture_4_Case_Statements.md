# 📘 VHDL Course – Lecture 4: Case Statements

Welcome to **Lecture 4** of this VHDL learning series.
This lecture explains **case statements** for **value-based selection** in VHDL.

---

## 🎯 What is a Case Statement?

A **case statement** is a **concurrent or sequential control structure** that:
- Selects one of several alternatives based on a **single expression**
- Compares the expression against multiple **constant values**
- Executes the corresponding branch when a match is found

📌 **Key Difference from if-elsif-else:**
- **if-elsif-else** → Checks different conditions (priority-based)
- **case** → Checks one expression against multiple values (enumeration-based)

---

## 🧠 Case Statement Types

### 1️⃣ Concurrent Case (Outside Process)
```vhdl
case expression is
    when value_1 => assignment_1;
    when value_2 => assignment_2;
    when others => default_assignment;
end case;
```

### 2️⃣ Sequential Case (Inside Process)
```vhdl
process (inputs)
begin
    case expression is
        when value_1 => assignment_1;
        when value_2 => assignment_2;
        when others => default_assignment;
    end case;
end process;
```

📌 Both types are **combinational logic** in this context!

---

## 📋 Case Statement Syntax

### Basic Structure

```vhdl
case expression is
    when value_1 =>
        statement_1;
        statement_2;
    when value_2 =>
        statement_3;
    when value_3 | value_4 =>
        statement_4;  -- Multiple values with |
    when others =>
        default_statement;
end case;
```

### Key Points:
- **case** - Starts the case statement
- **expression** - The value to be evaluated (must be discrete)
- **when** - Specifies the value to match
- **=>** - Separates the value from the statements
- **| (pipe)** - Allows multiple values for one branch
- **others** - Catches all unmatched values (recommended)
- **end case** - Ends the case statement

---

## 🔍 Understanding Case Evaluation

### How It Works:

1. Evaluate the **expression** once
2. Compare expression against each **when value**
3. Execute statements for the **first matching value**
4. If no match, execute **others** branch (if present)

### Important Characteristics:
- **No priority** - All when branches are equal
- **Mutual exclusivity** - Only one branch executes
- **Complete coverage** - Use **others** to handle all cases
- **Deterministic** - Same input always gives same output

---

## 🏗️ Practical Example: XOR Gate with Case

The **XOR** project demonstrates **case statement implementation**.

### Design Specification:

**Inputs:**
- `a`, `b` – Two input signals (std_logic)

**Output:**
- `y` – XOR result (std_logic)

### XOR Truth Table:
| a | b | y |
|---|---|---|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

### VHDL Implementation:

```vhdl
entity xor_caseothers is
    Port ( a : in STD_LOGIC;
           b : in STD_LOGIC;
           y : out STD_LOGIC);
end xor_caseothers;

architecture Behavioral of xor_caseothers is

begin

    p: process(a, b) is 
    variable tmp: std_logic_vector(1 downto 0); 
    begin
        tmp := a & b; 
        case tmp is 
        when "00" => y <= '0';
        when "01" => y <= '1';
        when "10" => y <= '1';
        when "11" => y <= '0';
        when others => y <= '0';
        end case; 
     end process; 

end Behavioral;
```

### Key Elements:
- **Process block** - Sequential execution
- **Variable tmp** - Concatenates inputs (a & b)
- **Case statement** - Evaluates tmp against all possible combinations
- **Others clause** - Handles any unexpected values

---

## 🎨 Use Cases for Case Statements

### 1️⃣ Digital Logic Gates
Implementing truth tables directly.

```vhdl
case inputs is
    when "00" => output <= '0';  -- AND gate
    when "01" => output <= '0';
    when "10" => output <= '0';
    when "11" => output <= '1';
end case;
```

### 2️⃣ Multiplexers
Selecting based on encoded values.

```vhdl
case select is
    when "00" => output <= input_0;
    when "01" => output <= input_1;
    when "10" => output <= input_2;
    when "11" => output <= input_3;
end case;
```

### 3️⃣ State Machines
Transitioning between enumerated states.

```vhdl
case current_state is
    when IDLE => next_state <= START;
    when START => next_state <= RUNNING;
    when RUNNING => next_state <= DONE;
    when DONE => next_state <= IDLE;
end case;
```

### 4️⃣ ALU Operations
Selecting arithmetic/logic operations.

```vhdl
case operation is
    when "000" => result <= a + b;      -- ADD
    when "001" => result <= a - b;      -- SUB
    when "010" => result <= a and b;    -- AND
    when "011" => result <= a or b;     -- OR
    when "100" => result <= a xor b;    -- XOR
    when others => result <= (others => '0');
end case;
```

---

## 📊 Comparison: Case vs If-Elsif-Else vs When-Else

### Case Statement
```vhdl
case selector is
    when "00" => output <= A;
    when "01" => output <= B;
    when "10" => output <= C;
    when others => output <= D;
end case;
```

**Best for:**
- ✅ Single expression with multiple values
- ✅ Equal priority evaluation
- ✅ Complete enumeration
- ✅ Clear intent for multiplexers

### If-Elsif-Else Statement
```vhdl
if selector(0) = '1' then
    output <= A;
elsif selector(1) = '1' then
    output <= B;
elsif selector(2) = '1' then
    output <= C;
else
    output <= D;
end if;
```

**Best for:**
- ✅ Priority-based selection
- ✅ Complex conditions
- ✅ Different signals in conditions

### When-Else Statement
```vhdl
output <= A when selector = "00" else
          B when selector = "01" else
          C when selector = "10" else
          D;
```

**Best for:**
- ✅ Concurrent assignments
- ✅ Simple conditional logic
- ✅ Clean, readable syntax

---

## 🔧 Advanced Case Features

### 1️⃣ Multiple Values with Pipe (|)
```vhdl
case data is
    when "000" | "001" | "010" => category <= LOW;
    when "011" | "100" | "101" => category <= MEDIUM;
    when "110" | "111" => category <= HIGH;
end case;
```

### 2️⃣ Range Values
```vhdl
case count is
    when 0 to 9 => digit <= '0';
    when 10 to 99 => digit <= '1';
    when others => digit <= '2';
end case;
```

### 3️⃣ Don't Care Values
```vhdl
case address is
    when "00--" => device <= UART;    -- Last 2 bits don't matter
    when "01--" => device <= SPI;
    when others => device <= NONE;
end case;
```

---

## ⚠️ Important Considerations

### 1️⃣ Complete Coverage
Always use **others** to handle all possible values:
```vhdl
case std_logic_signal is
    when '0' => output <= '1';
    when '1' => output <= '0';
    when others => output <= 'X';  -- Handles 'U', 'Z', etc.
end case;
```

### 2️⃣ Expression Type
Expression must be a **discrete type** (integer, enumerated, bit_vector, etc.):
```vhdl
-- Valid:
case count is when 0 => ...  -- integer
case state is when IDLE => ...  -- enumerated
case vector is when "00" => ...  -- bit_vector

-- Invalid:
case real_number is when 1.5 => ...  -- real not allowed
```

### 3️⃣ Synthesis Considerations
- **Full case** - All values covered (no others needed)
- **Parallel case** - No overlapping values
- **One-hot encoding** - For state machines

### 4️⃣ Variable vs Signal Assignment
```vhdl
-- Inside process:
variable temp: std_logic;
case selector is
    when "00" => temp := '1';  -- Variable assignment
    when others => temp := '0';
end case;

-- Concurrent:
case selector is
    when "00" => output <= '1';  -- Signal assignment
    when others => output <= '0';
end case;
```

---

## 🧪 Simulation and Testing

### Test Cases for XOR:
- **a = '0', b = '0'** → y = '0'
- **a = '0', b = '1'** → y = '1'
- **a = '1', b = '0'** → y = '1'
- **a = '1', b = '1'** → y = '0'

### Waveform Verification:
```
Time: 0ns   10ns  20ns  30ns
a:     0     0     1     1
b:     0     1     0     1
y:     0     1     1     0
```

📌 The **others** clause ensures robustness against 'U' (uninitialized) values!

---

## 🎯 Hardware Implementation

Case statements generate **combinational logic circuits**:

```
         ┌─────────────┐
 a ──────┤             │
         │   Decoder   │──── y
 b ──────┤   + Logic   │
         └─────────────┘
```

### Circuit Characteristics:
- **No memory** - Pure combinational logic
- **Parallel evaluation** - All cases evaluated simultaneously
- **Multiplexer-like** - Selects one of several outputs
- **Efficient synthesis** - Optimized for FPGA/ASIC

---

## 💡 Best Practices

### 1️⃣ Use Others Clause
```vhdl
case state is
    when IDLE => next <= START;
    when START => next <= RUN;
    when others => next <= ERROR;  -- Safe default
end case;
```

### 2️⃣ Group Related Values
```vhdl
case opcode is
    when ADD | SUB | MUL | DIV => operation <= ARITHMETIC;
    when AND | OR | XOR | NOT => operation <= LOGIC;
    when others => operation <= INVALID;
end case;
```

### 3️⃣ Clear Naming
```vhdl
type state_type is (IDLE, START, RUNNING, DONE);
-- Better than: type state_type is (S0, S1, S2, S3);
```

### 4️⃣ Comment Complex Cases
```vhdl
case complex_condition is
    when "0001" =>  -- Reset condition
        output <= '0';
    when "0010" =>  -- Normal operation
        output <= input;
    when others =>  -- Error state
        output <= 'X';
end case;
```

---

## ✅ Summary

- **Case statements** provide **value-based selection**
- Compare a **single expression** against **multiple constant values**
- **No priority** - All when branches are equal
- **Complete coverage** with **others** clause is essential
- Best for **multiplexers**, **state machines**, and **enumerated logic**
- Generates **combinational circuits** with parallel evaluation
- More structured than if-elsif-else for single-expression decisions

---