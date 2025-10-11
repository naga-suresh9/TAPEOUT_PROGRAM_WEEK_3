# 🕒 Week 3 – Post-Synthesis GLS & STA Fundamentals

### 🎓 Certificate

Completed **Udemy TVSD – Static Timing Analysis 1** ✅
![Image](https://github.com/user-attachments/assets/21ae6a2c-a87f-48e4-b416-67b3b6e56a0b)

---

### **1. Purpose of STA** 🔍

* STA = **Static Timing Analysis**: Verify **timing correctness** without running full simulations.
* Ensures **data reaches flip-flops/latches on time**.
* Detects **setup ⏱️ and hold ⏳ violations** that may break functionality.

---

### **2. Key Concepts** ⚡

#### **Clock ⏰**

* Timing reference for sequential circuits.
* **Primary clocks:** External inputs
* **Derived clocks:** Generated internally (dividers/multipliers)
* Parameters: period, duty cycle, skew, jitter

#### **Data Path ➡️**

* Path between sequential elements (FFs/latches).
* **Critical path:** Longest delay → limits max frequency
* **Short path:** Shortest delay → may violate hold time

---

### **3. Timing Checks** ✅❌

#### **Setup Time ⏱️**

* Data must arrive **before clock edge**
* Violation if data is **late**
* Slack = Tclk – (Tdata + Tsetup)
* Positive slack = ✅, Negative slack = ❌

#### **Hold Time ⏳**

* Data must remain **stable after clock edge**
* Violation if data **changes too early**
* Slack = Tdata – Thold
* Positive slack = ✅, Negative slack = ❌

---

### **4. Slack 💡**

* Margin by which timing is met
* Positive = OK, Negative = Violation
* Helps **prioritize paths for optimization**

---

### **5. Path-Based Analysis 🛣️**

* Evaluates **all timing paths** between sequential elements
* Longest path → **Setup check**
* Shortest path → **Hold check**
* Essential for **timing optimization**

---

### **6. Practical Steps in OpenSTA 🖥️**

1. Load **post-synthesis netlist & SDC constraints**
2. Define **clock(s)**
3. Run **`report_timing`** commands
4. Analyze **slack & violations**
5. Optimize **logic or constraints** if needed


