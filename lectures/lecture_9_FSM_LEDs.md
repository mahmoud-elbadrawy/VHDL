# 📘 VHDL Course – Lecture 9: FSM LEDs

Welcome to **Lecture 9** of this VHDL learning series.
This lecture explains a **finite state machine (FSM)** for LED control, based on the `FSM_LEDs.vhd` behavioral template.

---

## 🎯 What is an FSM?

A **finite state machine** is a digital circuit that moves through a set of states based on inputs and produces outputs.
FSMs are used for sequencers, controllers, and LED patterns.

📌 **Key Concept:**
> An FSM has a state register, next-state logic, and output logic.

---

## 🧠 FSM Structure in VHDL

A typical FSM has three parts:
1. **State declaration** – define the states
2. **Next-state logic** – determine the next state from the current state and inputs
3. **Output logic** – drive outputs using the current state and/or inputs

### Basic architecture:
```vhdl
architecture Behavioral of FSM_LEDs is
    type state_type is (S0, S1, S2, S3);
    signal state_reg, state_next : state_type;
begin
    process(clk, reset)
    begin
        if reset = '1' then
            state_reg <= S0;
        elsif rising_edge(clk) then
            state_reg <= state_next;
        end if;
    end process;

    process(state_reg, input_signal)
    begin
        case state_reg is
            when S0 =>
                state_next <= S1;
                leds <= "100";
            when S1 =>
                state_next <= S2;
                leds <= "010";
            when S2 =>
                state_next <= S3;
                leds <= "001";
            when others =>
                state_next <= S0;
                leds <= "000";
        end case;
    end process;
end Behavioral;
```

---

## 🔧 FSM_LEDs.vhd Template

The current `FSM_LEDs.vhd` file is a behavioral template with the library and architecture skeleton.
It provides the starting structure for the FSM design in VHDL.

### Current file structure:
```vhdl
entity FSM_LEDs is
end FSM_LEDs;

architecture Behavioral of FSM_LEDs is
begin
end Behavioral;
```

This is the standard place to add:
- clock and reset ports
- state type declarations
- combinational and sequential processes

---

## 📋 Common FSM LED Design

### Example ports:
```vhdl
entity FSM_LEDs is
port (
    clk : in std_logic;
    reset : in std_logic;
    led_out : out std_logic_vector(2 downto 0)
);
end FSM_LEDs;
```

### Example state table:
|State | LED Output | Next State |
|------|------------|------------|
| S0   | 100        | S1         |
| S1   | 010        | S2         |
| S2   | 001        | S3         |
| S3   | 111        | S0         |

This creates a repeating LED pattern driven by the FSM.

---

## 💡 Moore vs Mealy FSM

### Moore machine:
- Outputs depend only on the current state
- Easier to understand and synthesize

### Mealy machine:
- Outputs depend on current state and inputs
- Can react faster to input changes, but outputs may change asynchronously

For `FSM_LEDs`, a **Moore FSM** is usually a good choice because LED patterns depend on state only.

---

## ✅ Summary

- **Lecture 9** covers FSM design for LEDs using the `FSM_LEDs.vhd` template.
- The current VHDL file is a behavioral skeleton ready for FSM implementation.
- FSMs need state definitions, next-state logic, and output logic.
- LED FSMs are a classic example of sequential digital design in VHDL.
