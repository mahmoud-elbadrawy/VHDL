# 📘 VHDL Course – Lecture 8: Clock Divider and Counter

Welcome to **Lecture 8** of this VHDL learning series.
This lecture explains a **clock divider with a counter** based on the `clockdivider.vhd` design.

---

## 🎯 What is a Clock Divider?

A **clock divider** generates a slower clock from a faster input clock.
In this design, a 4-bit register divides the input clock frequency by using the most significant bit of a binary counter.

📌 **Key Concept:**
> A counter can create a lower-frequency clock by using one of its output bits.

---

## 🧠 Design Overview

The `clockdivider.vhd` design has:
- `clk` input: fast input clock
- `reset` input: synchronous/asynchronous reset signal
- `clk_out` output: divided clock signal
- `count_out` output: 4-bit counter value driven by the divided clock

### Entity Interface:
```vhdl
entity clockdivider is
port (
    clk : in std_logic;
    reset : in std_logic;
    clk_out : out std_logic;
    count_out : out std_logic_vector(3 downto 0)
);
end clockdivider;
```

---

## 🔧 Architecture Behavior

### Internal signals:
- `divider : std_logic_vector(3 downto 0)` - main divider counter
- `count : std_logic_vector(3 downto 0)` - output counter driven by the divided clock

### Core logic:
```vhdl
process(clk, reset)
begin
    if reset = '1' then
        divider <= "0000";
    elsif rising_edge(clk) then
        divider <= divider + '1';
    end if;
end process;

clk_out <= divider(3);

process(divider(3), reset)
begin
    if reset = '1' or count = "1010" then
        count <= "0000";
    elsif rising_edge(divider(3)) then
        count <= count + '1';
    end if;
end process;

count_out <= count;
```

---

## 📋 How It Works

### Divider logic
- `divider` increments on every rising edge of `clk`.
- `clk_out` is assigned from `divider(3)`.
- Since `divider` is 4 bits, `divider(3)` toggles every 8 clock cycles.
- This means `clk_out` has a period of 16 input clock cycles.

### Counter logic
- The second process uses `rising_edge(divider(3))`.
- The `count` register increments on each rising edge of the slower clock.
- When `count` reaches `1010` (decimal 10), it resets to `0000`.

### Reset behavior
- If `reset` = '1', both `divider` and `count` are cleared to zero.
- This ensures the design starts in a known state.

---

## 💡 Important Notes

### Library choice
This design uses the `IEEE.std_logic_unsigned.ALL` library for vector addition.
For modern VHDL, `IEEE.NUMERIC_STD.ALL` is preferred and safer for arithmetic.

### Generated clock frequency
- With a 4-bit divider, `clk_out` is the MSB of the counter.
- The output frequency is `clk / 16`.

### Counter range
- `count_out` cycles from `0000` to `1001`.
- It resets at `1010`, giving a decimal count range of 0–9.

---

## 🧪 Use Cases

This design can be used for:
- blinking LEDs at a slower rate
- creating human-visible timing from a fast system clock
- driving slower logic blocks or counters
- building clocked display refresh circuits

---

## ✅ Summary

- The design shows how to build a slower clock and a synchronized counter in VHDL.
- Reset logic keeps the circuit in a known state.
