# 📘 VHDL Course – Lecture 6: Structural Modeling

Welcome to **Lecture 6** of this VHDL learning series.
This lecture explains **structural modeling** - building complex circuits by connecting simpler components in VHDL.

---

## 🎯 What is Structural Modeling?

**Structural modeling** describes a circuit by:
- **Defining components** (sub-modules)
- **Instantiating components** in the architecture
- **Connecting component ports** with signals
- **Hierarchical design** approach

📌 **Key Concept:**
> Structure defines how components are connected, not how they work internally

---

## 🧠 Modeling Styles in VHDL

### 1️⃣ Structural Modeling (Component-based)
```vhdl
architecture Structural of FullAdder is
    component HalfAdder port(...);
begin
    HA1: HalfAdder port map(...);
    HA2: HalfAdder port map(...);
end Structural;
```

### 2️⃣ Dataflow Modeling (Concurrent)
```vhdl
architecture Dataflow of FullAdder is
begin
    sum <= a xor b xor cin;
    cout <= (a and b) or (cin and (a xor b));
end Dataflow;
```

### 3️⃣ Behavioral Modeling (Sequential)
```vhdl
architecture Behavioral of FullAdder is
begin
    process(a, b, cin)
    begin
        sum <= a xor b xor cin;
        cout <= (a and b) or (cin and (a xor b));
    end process;
end Behavioral;
```

📌 **Structural** is ideal for **hierarchical and reusable designs**!

---

## 🏗️ Component Declaration

### Syntax:
```vhdl
component component_name is
port (
    port_name : mode type;
    -- more ports...
);
end component;
```

### Example:
```vhdl
component HalfAdder is
port (
    A : in std_logic;
    B : in std_logic;
    SUM : out std_logic;
    CARRY : out std_logic
);
end component;
```

📌 **Component declaration** must match the **entity** of the instantiated module!

---

## 🔗 Component Instantiation

### Syntax:
```vhdl
instance_label : component_name port map (
    port_name => signal_name,
    -- more mappings...
);
```

### Example:
```vhdl
HA1 : HalfAdder port map (
    A => input_a,
    B => input_b,
    SUM => sum1,
    CARRY => carry1
);
```

📌 **Instance label** is required for each instantiation!

---

## 📋 Port Mapping

### Positional Mapping:
```vhdl
HA1 : HalfAdder port map (input_a, input_b, sum1, carry1);
```

### Named Mapping (Recommended):
```vhdl
HA1 : HalfAdder port map (
    A => input_a,
    B => input_b,
    SUM => sum1,
    CARRY => carry1
);
```

📌 **Named mapping** is clearer and less error-prone!

---

## 🏗️ Practical Example: Full Adder using Half Adders

The **FullAdder** project demonstrates **structural modeling** by connecting two **HalfAdder** components.

### Design Specification:

**Inputs:**
- `A`, `B` – Two input bits (std_logic)
- `CIN` – Carry input (std_logic)

**Outputs:**
- `SUM` – Sum output (std_logic)
- `COUT` – Carry output (std_logic)

### Functionality:
- **SUM** = A ⊕ B ⊕ CIN
- **COUT** = (A ∧ B) ∨ (CIN ∧ (A ⊕ B))

### Structural Implementation:

```vhdl
entity FullAdder is
port (
    A : in std_logic;
    B : in std_logic;
    CIN : in std_logic;
    SUM : out std_logic;
    COUT : out std_logic
);
end FullAdder;

architecture Structural of FullAdder is

    -- Component Declaration
    component HalfAdder is
    port (
        A : in std_logic;
        B : in std_logic;
        SUM : out std_logic;
        CARRY : out std_logic
    );
    end component;

    -- Internal Signals
    signal sum1, carry1, carry2 : std_logic;

begin

    -- First Half Adder: A + B
    HA1 : HalfAdder port map (
        A => A,
        B => B,
        SUM => sum1,
        CARRY => carry1
    );

    -- Second Half Adder: sum1 + CIN
    HA2 : HalfAdder port map (
        A => sum1,
        B => CIN,
        SUM => SUM,
        CARRY => carry2
    );

    -- Final Carry Output
    COUT <= carry1 or carry2;

end Structural;
```

### Key Elements:
- **Component declaration** - Defines HalfAdder interface
- **Signal declarations** - Internal connections (sum1, carry1, carry2)
- **Component instantiations** - Two HalfAdder instances (HA1, HA2)
- **Port mapping** - Connects inputs/outputs using named association
- **Additional logic** - COUT combines carry outputs with OR gate

---

## 🔍 Understanding the Full Adder Structure

### Circuit Diagram:
```
     A ──┐
         │     SUM
     B ──┼──  HA1 ──┐
         │          │
         └─────┐    │
               │    │
           CIN─┼── HA2 ── SUM
               │    │
               └────┼─── COUT
                    │
               carry1 ─ OR ─ COUT
               carry2 ───┘
```

### Signal Flow:
1. **HA1** adds A and B → sum1, carry1
2. **HA2** adds sum1 and CIN → SUM, carry2
3. **COUT** = carry1 OR carry2

### Hierarchical Design:
- **FullAdder** (top level)
  - **HalfAdder** (sub-component)
    - XOR and AND gates (primitive level)

---

## ⚡ Structural vs Other Modeling Styles

### Structural (Hierarchical)
```vhdl
architecture Structural of FullAdder is
    component HalfAdder port(...);
begin
    HA1: HalfAdder port map(...);
    HA2: HalfAdder port map(...);
    COUT <= carry1 or carry2;
end Structural;
```

**Advantages:**
- ✅ **Modular** - Reusable components
- ✅ **Hierarchical** - Complex systems from simple parts
- ✅ **Clear structure** - Easy to understand connections
- ✅ **IP reuse** - Use existing verified components

**Disadvantages:**
- ❌ **Verbose** - Many lines for simple circuits
- ❌ **Component management** - Must declare all components

### Dataflow (Equations)
```vhdl
architecture Dataflow of FullAdder is
begin
    SUM <= A xor B xor CIN;
    COUT <= (A and B) or (CIN and (A xor B));
end Dataflow;
```

**Advantages:**
- ✅ **Concise** - Direct equations
- ✅ **Efficient** - Minimal code

**Disadvantages:**
- ❌ **No hierarchy** - Flat design
- ❌ **Hard to reuse** - Not modular

### Behavioral (Processes)
```vhdl
architecture Behavioral of FullAdder is
begin
    process(A, B, CIN)
    begin
        SUM <= A xor B xor CIN;
        COUT <= (A and B) or (CIN and (A xor B));
    end process;
end Behavioral;
```

**Advantages:**
- ✅ **Flexible** - Any logic possible
- ✅ **Sequential** - Can add timing

**Disadvantages:**
- ❌ **Simulation overhead** - Process management
- ❌ **Synthesis issues** - May not optimize well

---

## 🧪 Testing Structural Designs

### Full Adder Test Cases:
- **A=0, B=0, CIN=0** → SUM=0, COUT=0
- **A=0, B=0, CIN=1** → SUM=1, COUT=0
- **A=0, B=1, CIN=0** → SUM=1, COUT=0
- **A=0, B=1, CIN=1** → SUM=0, COUT=1
- **A=1, B=0, CIN=0** → SUM=1, COUT=0
- **A=1, B=0, CIN=1** → SUM=0, COUT=1
- **A=1, B=1, CIN=0** → SUM=0, COUT=1
- **A=1, B=1, CIN=1** → SUM=1, COUT=1

### Verification:
Each test case verifies the HalfAdder components work correctly together!

---

## ⚠️ Important Considerations

### 1️⃣ Component Declaration Location
Components are declared in the architecture declarative region:
```vhdl
architecture Structural of TopLevel is
    -- Component declarations here
    component SubModule port(...);
begin
    -- Instantiations here
end Structural;
```

### 2️⃣ Signal Declarations
Internal signals connect component ports:
```vhdl
signal internal_sum, internal_carry : std_logic;
```

### 3️⃣ Port Mapping Rules
- **All ports must be mapped**
- **Directions must match** (in↔in, out↔out)
- **Types must be compatible**

### 4️⃣ Component Binding
Components bind to entities by name (default) or can be explicitly bound.

### 5️⃣ Multiple Instances
Each instance needs a unique label:
```vhdl
U1: MyComponent port map(...);
U2: MyComponent port map(...);
```

---

## 💡 Best Practices

### 1️⃣ Use Named Port Mapping
```vhdl
-- Good:
HA1: HalfAdder port map (
    A => input_a,
    B => input_b,
    SUM => sum_out,
    CARRY => carry_out
);

-- Avoid:
HA1: HalfAdder port map (input_a, input_b, sum_out, carry_out);
```

### 2️⃣ Meaningful Instance Labels
```vhdl
-- Good:
First_Adder: HalfAdder port map(...);
Second_Adder: HalfAdder port map(...);

-- Bad:
U1: HalfAdder port map(...);
U2: HalfAdder port map(...);
```

### 3️⃣ Group Related Components
```vhdl
-- Data path components
Adder1: HalfAdder port map(...);
Adder2: HalfAdder port map(...);

-- Control components
Mux1: Multiplexer port map(...);
```

### 4️⃣ Document Component Interfaces
```vhdl
-- Half Adder: Computes A + B = SUM + CARRY*2
component HalfAdder is
port (
    A : in std_logic;     -- First input bit
    B : in std_logic;     -- Second input bit
    SUM : out std_logic;  -- Sum output
    CARRY : out std_logic -- Carry output
);
end component;
```

---

## 🎯 Hardware Synthesis

Structural modeling creates **hierarchical netlists**:

```
FullAdder
├── HalfAdder (HA1)
│   ├── XOR gate
│   └── AND gate
├── HalfAdder (HA2)
│   ├── XOR gate
│   └── AND gate
└── OR gate
```

### Synthesis Benefits:
- **Modular optimization** - Each component optimized separately
- **IP integration** - Easy to include pre-designed blocks
- **Design reuse** - Components can be used in multiple designs
- **Verification** - Test components individually

---

## ✅ Summary

- **Structural modeling** builds circuits from **component instances**
- Uses **component declarations** and **port mapping**
- **Hierarchical design** - Complex systems from simple parts
- **FullAdder example** shows connecting two **HalfAdder** components
- **Named port mapping** is recommended for clarity
- Perfect for **modular and reusable** digital designs

---