# 📘 VHDL Course – Lecture 7: Four-Bit Dataflow Arithmetic

Welcome to **Lecture 7** of this VHDL learning series.
This lecture explains **dataflow arithmetic modeling** using a **4-bit adder** design based on the `fourbit_dataflow.vhd` file.

---

## 🎯 What is Four-Bit Dataflow Arithmetic?

**Four-bit dataflow arithmetic** describes multi-bit addition using concurrent signal assignments and numeric types.
In VHDL, this style is ideal for **combinational arithmetic circuits** like adders and subtractors.

📌 **Key Concept:**
> Dataflow arithmetic combines bit vectors with arithmetic operators and type conversions to create hardware-friendly adder logic.

---

## 🧠 VHDL Types for Arithmetic

### Standard logic vectors
- `std_logic_vector` is a bit-vector type used for input/output ports.
- It is not directly supported by arithmetic operators.

### Unsigned numbers
- `unsigned` is used for arithmetic on bit vectors representing non-negative integers.
- `numeric_std` library must be imported to use `unsigned` and conversion functions.

### Conversions
```vhdl
unsigned_vector <= unsigned(std_logic_vector_value);
std_logic_vector_value <= std_logic_vector(unsigned_value);
```

📌 Use `numeric_std` for reliable arithmetic on vectors.

---

## 🏗️ fourbit_dataflow.vhd Design

The `fourbit_dataflow` design performs:
- 4-bit addition of inputs `A` and `B`
- produces 4-bit sum output `Y`
- generates carry output `C`

### Entity Interface:
```vhdl
entity fourbit_dataflow is
    Port (
        A : in STD_LOGIC_VECTOR (3 downto 0);
        B : in STD_LOGIC_VECTOR (3 downto 0);
        Y : out STD_LOGIC_VECTOR (3 downto 0);
        C : out STD_LOGIC
    );
end fourbit_dataflow;
```

### Architecture Behavior:
```vhdl
architecture Dateflow of fourbit_dataflow is
signal tmp : unsigned(4 downto 0);
begin
    tmp <= unsigned('0' & A) + unsigned('0' & B);
    Y <= std_logic_vector(tmp(3 downto 0));
    C <= std_logic(tmp(4));
end Dateflow;
```

---

## 🔧 How the Design Works

1. **Extend the inputs**: each 4-bit vector is prefixed with `'0'` to create a 5-bit unsigned value.
2. **Add the values**: the two 5-bit unsigned operands are added together.
3. **Assign the sum**: the lower 4 bits of the result drive output `Y`.
4. **Capture carry-out**: the highest bit of the 5-bit result becomes carry output `C`.

### Why the extra bit?
- Adding two 4-bit values can produce a 5-bit result.
- The extra bit stores the carry-out beyond the 4-bit sum.

---

## 📋 Important Dataflow Rules

### Concurrent signal assignments
All assignments in the architecture execute concurrently, not sequentially.

### Single driver rule
Each signal must have only one driver. In this design:
- `tmp` is driven by the addition expression
- `Y` and `C` are driven by their respective conversions

### Type compatibility
- `A` and `B` are `std_logic_vector`
- The addition uses `unsigned` conversions
- Output `Y` is converted back to `std_logic_vector`
- Output `C` is converted to `std_logic`

---

## 🧪 Four-Bit Adder Test Cases

|   A  |   B  |   Y  | C |
|------|------|------|---|
| 0000 | 0000 | 0000 | 0 |
| 0001 | 0010 | 0011 | 0 |
| 0111 | 0001 | 1000 | 0 |
| 1111 | 0001 | 0000 | 1 |
| 1010 | 0101 | 1111 | 0 |
| 1111 | 1111 | 1110 | 1 |

📌 The carry output `C` shows when the sum exceeds 4 bits.

---

## 💡 Best Practices for Dataflow Arithmetic

- Use `numeric_std` instead of older arithmetic libraries.
- Convert `std_logic_vector` values to `unsigned` for addition.
- Keep arithmetic operations in a single concurrent assignment where possible.
- Name internal signals clearly when additional arithmetic stages are needed.

Example:
```vhdl
signal sum5 : unsigned(4 downto 0);
sum5 <= unsigned('0' & A) + unsigned('0' & B);
Y <= std_logic_vector(sum5(3 downto 0));
C <= std_logic(sum5(4));
```

---

## ✅ Summary

- The design uses `unsigned` arithmetic and conversions from `std_logic_vector`.
- The output includes a 4-bit sum and a carry-out bit.
- This is a clean example of **combinational arithmetic** in VHDL.
