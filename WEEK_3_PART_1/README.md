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

![Image](https://github.com/user-attachments/assets/f9a841eb-2402-4c37-b0b0-a33db9372cf9)

```

Then, inside the Yosys shell:

```tcl
read_verilog /home/nagasuresh/suresh/VSDBabySoC/src/module/vsdbabysoc.v
read_verilog -I /home/nagasuresh/suresh/VSDBabySoC/src/include /home/nagasuresh/suresh/VSDBabySoC/src/module/rvmyth.v
read_verilog -I /home/nagasuresh/suresh/VSDBabySoC/src/include /home/nagasuresh/suresh/VSDBabySoC/src/module/clk_gate.v
```

🖼️ 
![Image](https://github.com/user-attachments/assets/b17259f6-fab3-468c-a91a-143e3b5705ee)
![Image](https://github.com/user-attachments/assets/162a9dc1-1a64-4b18-8144-51b9489096be)
![Image](https://github.com/user-attachments/assets/e3cd5043-f472-4750-9578-63852b2e4868)

---

### 🧩 Step 2: Load Liberty Files for Synthesis

```tcl
read_liberty -lib /home/nagasuresh/suresh/VSDBabySoC/src/lib/avsdpll.lib
read_liberty -lib /home/nagasuresh/suresh/VSDBabySoC/src/lib/avsddac.lib
read_liberty -lib /home/nagasuresh/suresh/VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

🖼️ 
![Image](https://github.com/user-attachments/assets/d512b31e-9a8f-4310-be37-a5fc4d332df5)
![Image](https://github.com/user-attachments/assets/95bf71e9-75ce-44dc-bdb3-16e56246df86)
![Image](https://github.com/user-attachments/assets/cbd08ef6-2a69-40af-b49e-adfc7928aae2)

---

### 🧠 Step 3: Run Synthesis Targeting Top Module

```tcl
synth -top vsdbabysoc
```
![Image](https://github.com/user-attachments/assets/b96e78c6-a736-424b-8029-a2327c94cd93)

![Image](https://github.com/user-attachments/assets/560ecf3e-584e-450b-a997-47f56a870e2a)

![Image](https://github.com/user-attachments/assets/df9768ae-5ed2-4a46-9496-9b80d42052e9)

![Image](https://github.com/user-attachments/assets/b0ab8b64-3f4c-42d1-84f1-305fe49d87d2)

![Image](https://github.com/user-attachments/assets/5e404af6-bf5f-46db-81c2-fd67ada569dd)

![Image](https://github.com/user-attachments/assets/d8b4538b-0062-4595-b74a-19212971484e)
🖼️ 


---

### 🔄 Step 4: Map D Flip-Flops to Standard Cells

```tcl
dfflibmap -liberty /home/nagasuresh/suresh/VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

🖼️ 
![Image](https://github.com/user-attachments/assets/66216e61-172d-47ff-9e51-b210095df062)

---

### ⚡ Step 5: Optimization and Technology Mapping

```tcl
opt
abc -liberty /home/nagasuresh/suresh/VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib -script +strash;scorr;ifraig;retime;{D};strash;dch,-f;map,-M,1,{D}
```

🖼️ 
![Image](https://github.com/user-attachments/assets/d85b157a-ca01-4695-8d10-ed40b2bfbf40)

![Image](https://github.com/user-attachments/assets/ac9091ba-7c80-47c2-a27e-c50fbfdcc6aa)

---

### 🧹 Step 6: Final Clean-Up and Renaming

```tcl
flatten
setundef -zero
clean -purge
rename -enumerate
```

🖼️ *
![Image](https://github.com/user-attachments/assets/30347b63-a41a-452a-ab7c-d52455e0f3da)
---

### 📊 Step 7: Check Design Statistics

```tcl
stat
```

🖼️ 
![Image](https://github.com/user-attachments/assets/7f66043e-2d57-4edd-9572-aa0a0d0c9c64)

![Image](https://github.com/user-attachments/assets/a0a2fe00-b50f-4f51-8c66-5664a05b0f18)

---

### 💾 Step 8: Write the Synthesized Netlist

```tcl
write_verilog -noattr /home/nagasuresh/suresh/VSDBabySoC/output/post_synth_sim/vsdbabysoc.synth.v
```

🖼️ 
![Image](https://github.com/user-attachments/assets/7e90511a-dc05-4b6c-b363-7b0bae499094)
– Synthesized Netlist Saved Successfully*

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

🖼️ 
![Image](https://github.com/user-attachments/assets/b6124928-34e9-4eaa-a54b-c63702ce5511)
– Compilation Output*

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
---
### 📈 Step 4: View Waveforms in GTKWave

```bash
gtkwave post_synth_sim.vcd
```

🖼️ 

– GTKWave Output*
![Image](https://github.com/user-attachments/assets/eea8ebcb-b0d1-4abd-8433-5ad636cdb07a)

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

