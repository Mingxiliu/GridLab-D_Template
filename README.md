# GridLab-D_Template

General Info
        1. This is an Minimum Working Example (MWE) for users to run Gridlab-D to simulate a distribution feeder. 
        2. This MWE is compatiable with Gridlab-D v5.3.0. Earlier version of Gridlab-D might not work.
        3. The distribution feeder was modified from the standard [IEEE 13-bus test feeder](https://cmte.ieee.org/pes-testfeeders/resources/) in the way that transformers, capacitors, swwitches, etc. are ignored and three phases are decoupled. That being said, it is a simple single-phase feeder.
        4. Nodes are re-labled:
            n0: 650; feeder head
            n1: 632
            n2: 645
            n3: 646
            n4: 633
            n5: 634
            n6: 671
            n7: 692
            n8: 675
            n9: 684
            n10:611
            n11:652
            n12:680

Copyright
        Mingxi Liu, Ph.D. @ University of Utah, Salt Lake City, USA
 
Get Started
        1. Install Gridlab-D on a Windows PC. If you are using Mac, please install VMWare Fusion to virtually run.
        2. Go to the [Gridlab-D Repo](https://github.com/gridlab-d/gridlab-d/releases) to download the .exe installation file for Version v5.3.0.
        3. Install Gridlab-D v5.3.0 on your (virtual) windows PC. Follow the prompts and set all paths to default.
        4. Check if Gridlab-D was successfully installed: Open your command line tool. (Please enable bash if you have not). Type <gridlabd --version>, if you see the output <Gridlab-D 5.3.0-xxxxx(xxxxxx:xxx) 64-bit Windows Release>, it means your Gridlab-D was successfully installed.
        5. Install the git tools, if you havn't. https://git-scm.com/ 
        6. Install your preferred coding software. You can choose to use Visual Studio Code (https://code.visualstudio.com/) which allows you to connect to Github and use Copilot. It is completely up to you. 
        7. You now should have all necessary softwares to run the provided code. 

Obtain the Files
    There are two ways you can obtain the files
        1. Preferred: Go to https://github.com/Mingxiliu/GridLab-D_Template to download the entire folder and save it to a local path you prefer
        2. In your command line tool, navigate to a folder that you prefer. <git clone https://github.com/Mingxiliu/GridLab-D_Template.git> to clone the entire folder to your local path. Note that: PLEASE DO NOT commit or push anything to the repo. Only modify the files locally.

Run the Model
        1. Open your command line tool (or terminal) inside/outside your coding software.
        2. The IEEE_13_Single_House_EV_Solar.glm is the main Gridlab-D file that configures the distribution grid.
        3. base_load.csv contains the 24-hour nodal aggregated baseline load schedules of houses/buildings. The assumption is that each node, except the node n0, has 70 houses/buildings connected. You should make changes to this profile to reflect your design. Data in the provided file were randomly generated.
        4. EV_load.csv contains the 24-hour nodal aggregated EV charging load schedules. The assumption is that each node, except the node n0, has 70 EVs connected. You should make changes to this profile to reflect your design. Data in the provided file were randomly generated.
        5. house_load.csv contains the 24-hour nodal aggregated load schedules of NEWLY DESIGNED buildings. You should make changes to this profile to reflect your design. Data in the provided file were randomly generated.
        6. In your command line tool (or terminal) inside/outside your coding software, type <gridlabd IEEE_13_Single_House_EV_Solar.glm> then ENTER.
        7. Check for any [ERROR] messsages.
        8. Ignore any [WARNING] messages.
        9. You should not get any [FATAL] [ERROR] messages.
        10. Check if the simulation can successfully output some .csv files.

Modify the Model and Load/Generation Schedules
        1. Everything before Line 330 configures the network. ONLY make changes after Line 330
        2. Only apply loads and generations to Phase A.
        3. Writings in the schedule files, including base_load.csv, EV_load.csv, house_load.csv, and all schedules you will create are very sensitive. Please follow the rules when creating/modifying those files
            a. They have to be .csv files.
            b. The date format has to be <YYYY-MM-DD HH:MM:SS>, e.g., 2017-07-01 00:00:00. However, if you open that file in Microsoft Excel and save it without changes, the date format will be automatically changed by Excel. Therefore, it is recommended to 
                i.  Don't touch the first column
                i.  Only edit the second column
                ii. You don't have to add any imaginary power unless you really want to. So keep it as +0i.
                iv. After saving it, go to your coding software and open the .csv file. Manually change the date to the format indicated above.
                v.  Add a recorder in IEEE_13_Single_House_EV_Solar.glm the to monitor the load/generation you just edited/added
                v.  Run <gridlabd IEEE_13_Single_House_EV_Solar.glm>, check the output of the recorder to make sure the schedules are correctly loaded
        4. When attaching new loads and/or generations, just follow the same style in the configuration in node n2.
        5. Remember, when attaching new loads and/or generations, the attached loads and/or generations are aggregated. For example, if you want to place a EV charging station with 20 Level-3 chargers to node n3, then the schedule you attach to node n3 should reflect the aggregarted charging schedule of all 20 chargers. If you want to place 35 Level-2 EV chargers to a building you designed on node n4, then you need to attach a building load schedule to n4 and attach a EV charging schedule to reflect the aggregated charging schedule of those 35 chargers.

Data Processing
        1. Since this is a simple single-phase feeder, only nodal voltage magnitudes need to be observed.
        2. Nodal voltage magnitudes are reported by the group recorder <voltage_mag_phase_A> with the output file All_Nodes_Voltage_A_ang.csv.
        3. Before running <gridlabd IEEE_13_Single_House_EV_Solar.glm>, please close all .csv output files, otherwise the new output cannot be written.
        4. The nominal voltage for each node is 2401.7771. When processing the data, we normally use p.u. (per unit). The p.u. voltage is calculated as (actual voltage magnitude)/nominal voltage. In general, we want to keep all nodal voltage magnitudes within 0.95-1.05 p.u. When processing the data, reporting the results, and generating any figures, please use p.u.
