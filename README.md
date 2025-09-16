# GridLab-D Template

## 📌 General Info
- This is a **Minimum Working Example (MWE)** for users to run **GridLab-D** to simulate a distribution feeder.  
- Compatible with **GridLab-D v5.3.0**. Earlier versions may not work.  
- Based on the [IEEE 13-bus test feeder](https://cmte.ieee.org/pes-testfeeders/resources/), but simplified:
  - Transformers, capacitors, switches, etc. are ignored.  
  - Three phases are decoupled → single-phase feeder.  
- Node re-labeling:
  ```
  n0 : 650  (feeder head)
  n1 : 632
  n2 : 645
  n3 : 646
  n4 : 633
  n5 : 634
  n6 : 671
  n7 : 692
  n8 : 675
  n9 : 684
  n10: 611
  n11: 652
  n12: 680
  ```

---

## © Copyright
**Mingxi Liu, Ph.D.**  
University of Utah, Salt Lake City, USA  

---

## 🚀 Get Started
1. Install **GridLab-D** on a Windows PC.  
   - Mac users: install **VMWare Fusion** to run Windows virtually.  
2. Download installer from the [GridLab-D Releases](https://github.com/gridlab-d/gridlab-d/releases).  
3. Install **GridLab-D v5.3.0** (use default paths).  
4. Verify installation:
   ```bash
   gridlabd --version
   ```
   Example output:
   ```
   Gridlab-D 5.3.0-xxxxx(xxxxxx:xxx) 64-bit Windows Release
   ```
5. Install [Git](https://git-scm.com/).  
6. Install a code editor (e.g., [Visual Studio Code](https://code.visualstudio.com/)).  
7. You now have all the necessary software to run the provided code.  

---

## 📂 Obtain the Files
- **Option 1 (preferred):** Download directly from [GridLab-D_Template](https://github.com/Mingxiliu/GridLab-D_Template).  
- **Option 2 (clone via Git):**
  ```bash
  git clone https://github.com/Mingxiliu/GridLab-D_Template.git
  ```
  > ⚠️ Do **not** commit or push changes to the repo. Modify files locally only.

---

## ▶️ Run the Model
1. Open a terminal (inside/outside your editor).  
2. The main file is:
   ```
   IEEE_13_Single_House_EV_Solar.glm
   ```
3. Input CSVs:
   - `base_load.csv` → contains the 24-hour nodal aggregated baseline load schedules of houses/buildings. The assumption is that each node, except the node n0, has 70 houses/buildings connected. You should make changes to this profile to reflect your design. Data in the provided file were randomly generated. 
   - `EV_load.csv` → contains the 24-hour nodal aggregated EV charging load schedules. The assumption is that each node, except the node n0, has 70 EVs connected. You should make changes to this profile to reflect your design. Data in the provided file were randomly generated. 
   - `house_load.csv` → contains the 24-hour nodal aggregated load schedules of **NEWLY DESIGNED** buildings. You should make changes to this profile to reflect your design. Data in the provided file were randomly generated.
   - ⚠️ Data is **randomly generated** → modify for your design.  

4. Run:
   ```bash
   gridlabd IEEE_13_Single_House_EV_Solar.glm
   ```
5. Check output:  
   - No `[FATAL]` or `[ERROR]`.  
   - `[WARNING]` can be ignored.  
6. Confirm `.csv` outputs are created.  

---

## ✏️ Modify the Model & Schedules
- In the `.glm` file:  
  - Only edit **after Line 330**.  
  - Only use **Phase A**.  

- CSV file rules:  
  - Must be `.csv`.  
  - Format:  
    ```
    YYYY-MM-DD HH:MM:SS,value+0i
    +15m,value+0i
    ```
  - If you open that file in Microsoft Excel and save it without changes, the date format will be automatically changed by Excel. Therefor it is recommended that
    - Don’t edit the **first column** in Excel (it may reformat dates).
    - Only edit the **second column**.
    - If Excel changes the format, fix manually in a text editor. 
  - You don't have to add any imaginary power unless you really want to. So keep it as `+0i`.

- Testing modifications:  
  1. Add a recorder in `.glm` to monitor the load/generation you just edited/added.  
  2. Run simulation.  
  3. Verify recorder output.  

- Adding new loads/generations:  
  - Follow **node n2** style.  
  - Remember: when attaching new loads and/or generations, the attached loads and/or generations are aggregated. For example, if you want to place a EV charging station with 20 Level-3 chargers to node n3, then the schedule you attach to node n3 should reflect the aggregarted charging schedule of all 20 chargers. If you want to place 35 Level-2 EV chargers to a building you designed on node n4, then you need to attach a building load schedule to n4 and attach an EV charging schedule to reflect the aggregated charging schedule of those 35 chargers.

---

## 📊 Data Processing
1. Observe **nodal voltage magnitudes** (single-phase feeder).  
2. Recorder:
   ```
   voltage_mag_phase_A
   ```
   Output file:
   ```
   All_Nodes_Voltage_A_ang.csv
   ```
3. Before running, close all `.csv` output files (otherwise new results won’t be written).  
4. Nominal voltage:
   ```
   2401.7771
   ```
   Per-unit voltage:
   ```
   V_pu = actual_voltage / 2401.7771
   ```
   - Target range: **0.95 – 1.05 p.u.**  
5. Always use **p.u. values** when processing, reporting, or plotting.  
