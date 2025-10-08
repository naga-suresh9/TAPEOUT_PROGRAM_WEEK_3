# ⚙️ Gate-Level Simulation (GLS) of BabySoC

## 🎯 Purpose of GLS

Gate-Level Simulation (GLS) is performed **after synthesis** to verify the design’s **functional correctness and timing behavior**.
While RTL simulations check logic at a higher abstraction, GLS uses the **synthesized netlist** — consisting of actual **logic gates and interconnections** — to ensure that the design works as expected in real hardware.

---

## 🧩 Key Aspects of GLS for BabySoC

### 🕒 1. Verification with Timing Information

* GLS includes **Standard Delay Format (SDF)** files for accurate timing verification.
* Ensures the design performs correctly under **real-world propagation delays**.

### ✅ 2. Post-Synthesis Design Validation

* Confirms logical behavior is preserved after synthesis.
* Detects issues such as **glitches, setup/hold violations**, or **metastability**.

### 🧰 3. Tools Used

* **Synthesis:** Yosys
* **Simulation:** Icarus Verilog
* **Waveform Viewing:** GTKWave

### 🧠 4. Importance for BabySoC

BabySoC integrates multiple modules such as:

* RISC-V processor core
* PLL (Phase-Locked Loop)
* DAC (Digital-to-Analog Converter)

GLS ensures these modules **interact properly** and satisfy **timing constraints** after synthesis.

---

## 🚀 Step-by-Step Execution Plan

### 🧱 Step 1: Load the Top-Level Design and Modules

Launch Yosys:

```bash
yosys
```

Then, inside the Yosys shell:

```tcl
read_verilog /home/nagasuresh/suresh/VSDBabySoC/src/module/vsdbabysoc.v
read_verilog -I /home/nagasuresh/suresh/VSDBabySoC/src/include /home/nagasuresh/suresh/VSDBabySoC/src/module/rvmyth.v
read_verilog -I /home/nagasuresh/suresh/VSDBabySoC/src/include /home/nagasuresh/suresh/VSDBabySoC/src/module/clk_gate.v
```

🖼️ *Image Placeholder – Yosys Loading Modules*

---

### 🧩 Step 2: Load Liberty Files for Synthesis

```tcl
read_liberty -lib /home/nagasuresh/suresh/VSDBabySoC/src/lib/avsdpll.lib
read_liberty -lib /home/nagasuresh/suresh/VSDBabySoC/src/lib/avsddac.lib
read_liberty -lib /home/nagasuresh/suresh/VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

🖼️ *Image Placeholder – Liberty File Loading*

---

### 🧠 Step 3: Run Synthesis Targeting Top Module

```tcl
synth -top vsdbabysoc
```

🖼️ *Image Placeholder – Yosys Synthesis Output*

---

### 🔄 Step 4: Map D Flip-Flops to Standard Cells

```tcl
dfflibmap -liberty /home/nagasuresh/suresh/VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

🖼️ *Image Placeholder – DFF Mapping*

---

### ⚡ Step 5: Optimization and Technology Mapping

```tcl
opt
abc -liberty /home/nagasuresh/suresh/VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib -script +strash;scorr;ifraig;retime;{D};strash;dch,-f;map,-M,1,{D}
```

🖼️ *Image Placeholder – ABC Optimization Logs*

---

### 🧹 Step 6: Final Clean-Up and Renaming

```tcl
flatten
setundef -zero
clean -purge
rename -enumerate
```

🖼️ *Image Placeholder – Clean-Up Logs*

---

### 📊 Step 7: Check Design Statistics

```tcl
stat
```

🖼️ *Image Placeholder – Design Statistics Output*

---

### 💾 Step 8: Write the Synthesized Netlist

```tcl
write_verilog -noattr /home/nagasuresh/suresh/VSDBabySoC/output/post_synth_sim/vsdbabysoc.synth.v
```

🖼️ *Image Placeholder – Synthesized Netlist Saved Successfully*

---

## 🧪 Post-Synthesis Simulation & Waveform Analysis

### 🧰 Step 1: Compile the Testbench

```bash
iverilog -o /home/nagasuresh/suresh/VSDBabySoC/output/post_synth_sim/post_synth_sim.out \
-DPOST_SYNTH_SIM -DFUNCTIONAL -DUNIT_DELAY=#1 \
-I /home/nagasuresh/suresh/VSDBabySoC/src/include \
-I /home/nagasuresh/suresh/VSDBabySoC/src/module \
/home/nagasuresh/suresh/VSDBabySoC/src/module/testbench.v
```

🖼️ *Image Placeholder – Compilation Output*

---

### 📂 Step 2: Move to Output Directory

```bash
cd /home/nagasuresh/suresh/VSDBabySoC/output/post_synth_sim/
```

---

### ▶️ Step 3: Run the Simulation

```bash
./post_synth_sim.out
```

🖼️ *Image Placeholder – Simulation Terminal Output*

---

### 📈 Step 4: View Waveforms in GTKWave

```bash
gtkwave post_synth_sim.vcd
```

🖼️ *Image Placeholder – GTKWave Output*

---

## ✅ Summary

| Task               | Description                       | Tool Used      |
| ------------------ | --------------------------------- | -------------- |
| Logic Synthesis    | Convert RTL to gate-level netlist | Yosys          |
| DFF Mapping        | Map flip-flops to std cells       | Yosys          |
| Netlist Generation | Generate synthesized Verilog      | Yosys          |
| Simulation         | Verify functionality              | Icarus Verilog |
| Waveform Analysis  | Timing & signal check             | GTKWave        |

🧾 **Outcome:**

* Verified BabySoC netlist after synthesis
* Ensured functional and timing correctness
* Observed expected waveform transitions in GTKWave

---

✨ *End of Post-Synthesis Simulation Report for BabySoC*

