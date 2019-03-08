#Gas Transfer
####Lynn Li (ll643) time spent: 9
####Allison Tran (ant42) time spent: 8
####Group 3
####Due 03/08/2019

# Equations
$\ln \frac{C^{*} -C}{C^{*} -C_{0} } =-\hat{k}_{v,l} (t-t_{0} )  \Rightarrow Equation 1$

$OTE=\frac{\hat{k}_{v,l} \left(C^{star} -C\right)VRT}{MW_{O_{2} } Q_{air} P_{air} f_{O_{2} } }\Rightarrow Equation 2$


# Results
![Figure1](https://github.com/ll643/Lab-4/blob/master/group3_DO_vs_time.png)

Figure 1: Plot of Dissolved Oxygen vs Time for all the experimental flow rates

![Figure2](https://github.com/ll643/Lab-4/blob/master/group3_DO_vs_time_rep.png)

Figure 2: Representative Plot of Dissolved Oxygen vs Time for 5 experimental different flow rates (100, 250, 450, 650, 950)

![Figure3](https://github.com/ll643/Lab-4/blob/master/group3_O2_vs_time.png)

Figure 3: Plot of model Concentration of O2 vs Time for a flow rate of 500 and experimental plot of Concentration of O2 vs time

![Figure4](https://github.com/ll643/Lab-4/blob/master/group3_kvl_vs_air_flow_rate.png)

Figure 4: Plot of experimental kvl values vs Air Flow Rate

![Figure5](https://github.com/ll643/Lab-4/blob/master/group3_OTE_vs_air_flow_rate.png)

Figure 5: Plot of experimental Oxygen Transfer Efficiency (OTE) vs Air Flow Rate

#### Question 8: Comment on the oxygen transfer efficiency and the trend or trends that you observe.
Looking at Figure 5, we can see that there is a general decrease of OTE as air flow rate increases. Though we have several outliers to the data, there is a general downwards trend. This makes sense because Q is in the denominator of Equation 2 which solves for OTE using various lab variables; therefore as Q increases there should be a decrease in OTE. Inconsistencies and outliers are due to differentiating experimental setups between lab groups, errors with ProCoDA, and errors with the DO probe apparatus.

#### Question 9: Propose a change to the experimental apparatus that would increase the efficiency.
A change to the apparatus that would increase the efficiency of this lab would be to increase the size of the reactor beaker. This way, the aeration would not be able to occur in close proximity to the DO probe. A lot of the inaccuracies of this lab were resultant from air bubbles accumulating under the DO probe. This caused the probe to record inaccurate data.

# Conclusions
Through completing the gas transfer lab twice, we became extremely familiar and comfortable with using the gas transfer apparatus and ProCoDa. As can be seen by Figure 2, we conclude that in general as flow rate increases, the oxygen concentration vs time curves become steeper and more logarithmic in nature. Based on the lab conditions, the calculated C* value is 8.893917006212314 mg/L. Looking at Figure 3, our experimental O2 vs Time curve is almost exactly as logarithmic as the model curve. This is expected since our model curve was calculated based on the experimental data. However, for Figures 4 and 5 there are a lot of outliers and inconsistencies on the graphs. This could be due to experimental error and issues with varying lab apparatus, or due to human error through differentiating lab setups between groups. As mentioned above, there is a general decreasing trend when OTE is being compared to airflow rate and an increasing trend when kvl is being compared to airflow rate. It's important to note that the increments of the y-axis in Figure 5 are extremely small therefore there is a not a large difference between the OTE values which is what we expect. Ideally all of the OTE values would be the same.

#Suggestions
Removing the needle valve from the set up made the experiment a million times easier. There were no instructions on how to use the needle valve which was the main obstacle the first time around. We also had issues with the solenoid valves not working properly which forced us to pause or restart our experiment several times. It would have saved us a lot of time if they were tested before. Lastly, we think it would be really helpful to have a trouble shooting page with common ProCoDa issues because it will save a lot of time in lab and make things a lot easier for TAs during lab time.

#Appendix
```python
#Question 2
import aguaclara
import os
from pathlib import Path
from aguaclara.core.units import unit_registry as u
import aguaclara.core.physchem as pc
u.define('equivalent = mole = eq')
import aguaclara.research.environmental_processes_analysis as epa
from scipy import optimize
from scipy import stats
from aguaclara.core import utility as ut
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
import collections

def aeration_data(DO_column, dirpath):
    filenames = os.listdir(dirpath)
    airflows = ((np.array([i.split('.', 1)[0] for i in filenames])).astype(np.float32))
    idx = np.argsort(airflows)
    airflows = (np.array(airflows)[idx])*u.umole/u.s
    filenames = np.array(filenames)[idx]
    filepaths = [os.path.join(dirpath, i) for i in filenames]
    DO_data=[epa.column_of_data(i,0,DO_column,-1,'mg/L') for i in filepaths]
    time_data=[(epa.column_of_time(i,0,-1)).to(u.s) for i in filepaths]
    aeration_collection = collections.namedtuple('aeration_results','filepaths airflows DO_data time_data')
    aeration_results = aeration_collection(filepaths, airflows, DO_data, time_data)
    return aeration_results

# The column of data containing the dissolved oxygen concentrations
DO_column = 2
dirpath = "/Users/tran/Desktop/Aeration 2"
#dirpath = "/Users/LynnLi/Desktop/Aeration"
filepaths, airflows, DO_data, time_data = aeration_data(DO_column,dirpath)

DO_min = 2 * u.mg/u.L
DO_max = 6 * u.mg/u.L
for i in range(airflows.size):
  idx_start = (np.abs(DO_data[i]-DO_min)).argmin()
  idx_end = (np.abs(DO_data[i]-DO_max)).argmin()
  time_data[i] = time_data[i][idx_start:idx_end] - time_data[i][idx_start]
  DO_data[i] = DO_data[i][idx_start:idx_end]

for i in range(airflows.size):
  plt.plot(time_data[i], DO_data[i],'-')
plt.xlabel(r'$Time (s)$')
plt.ylabel(r'Oxygen Concentration $\left ( \frac{mg}{L} \right )$')
plt.legend(airflows.magnitude)
plt.title('Dissolved Oxygen vs Time')
plt.show()

#Representative Plot
plt.plot(time_data[0], DO_data[0],'-', time_data[5], DO_data[5],'-', time_data[8], DO_data[8],'-', time_data[14], DO_data[14],'-', time_data[23], DO_data[23],'-')
plt.xlabel(r'$Time (s)$')
plt.ylabel(r'Oxygen Concentration $\left ( \frac{mg}{L} \right )$')
plt.legend(['100', '250', '450', '650', '950'])
plt.title('Dissolved Oxygen vs Time')
plt.show()
#Quantity(8.893917006212314, 'milligram / liter')

#Question 3
temp = (22*u.celsius).to(u.kelvin)
barometric = (101.3*u.kPa).to(u.atm)
C_star=epa.O2_sat(barometric, temp)
#<Quantity(8.893917006212314, 'milligram / liter')>
print ('C* is',C_star, '.')

#Question 4
File_number_1 = np.array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11])
File_number_2 = np.array([13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23])
kvl_array_first = np.zeros(12)
kvl_array_second = np.zeros(11)

for i in range(File_number_1.size):
  Time = time_data[i] - time_data[i][0]
  C_Data = np.log((C_star - DO_data[i])/(C_star - DO_data[i][0]))
  slope, intercept, r_value, p_value, std_err = stats.linregress(time_data[i],C_Data)
  kvl_array_first[i] = slope

kvl_array_first

for j in range(File_number_2.size):
  Time = time_data[j] - time_data[j][0]
  C_Data = np.log((C_star - DO_data[j])/(C_star - DO_data[j][0]))
  slope, intercept, r_value, p_value, std_err = stats.linregress(time_data[j],C_Data)
  kvl_array_second[j] = slope

kvl_array_second

kvl_array = np.concatenate((kvl_array_first,kvl_array_second), axis=0)

print('The -kvl values for the data sets we chose are',kvl_array)

#Question 5
#using equation 103
C = DO_data[10]
C_zero = C[0]
kvl_450 = kvl_array[10]
time = (time_data[10]).magnitude
C_array = np.zeros(len(time))*u.mg/u.L

for i in range(time.size):
    C_array[i] = C_star-((C_star - C_zero)*np.exp((kvl_450)*time[i]))

plt.plot(time_data[10],C_array, '-')
plt.plot(time_data[10],DO_data[10],'o')
plt.xlabel('Time (s)')
plt.ylabel('Concentration of Oxygen (mg/L)')
plt.title('Concentration of Oxygen vs Time')
plt.show()

#Question 6
Airflow_array = [100, 125, 175, 200, 225, 250, 350, 400, 450, 475, 500, 525, 575, 650, 700, 725, 750, 775, 800, 825, 850, 925, 950]
plt.plot(Airflow_array, -kvl_array, 'o')
plt.xlabel('Air Flow Rate (μmole/s)')
plt.ylabel('Kvl')
plt.title('Kvl vs Air Flow Rate')
plt.show()

#Question 7
Oxygen_Deficit = 6*(u.mg/u.L)
Volume = 750*u.mL
R = 0.082*((u.L*u.atm)/(u.K*u.mol))
Molecular_Weight_O2 = 32*(u.g/u.mol)
#Q_Air = 450*u.um**3/u.sec
fO2 = 0.21
File_number_1 = np.array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11])
File_number_2 = np.array([13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23])
Airflow_array_1 = np.array([100, 125, 175, 200, 225, 250, 350, 400, 450, 475, 500, 525])
Airflow_array_2 = np.array([575, 650, 700, 725, 750, 775, 800, 825, 850, 925, 950])
OTE_array = np.zeros(23)

for i in range(File_number_1.size):
  Q = (Airflow_array_1[i])*(u.um**3/u.sec)
  OTE = ((-kvl_array[i]) * Oxygen_Deficit * Volume * R * temp)/(Molecular_Weight_O2 * Q * barometric * fO2)
  OTE_array[i] = OTE.magnitude

for j in range(File_number_2.size):
  Q = (Airflow_array_2[j])*(u.um**3/u.sec)
  OTE = (-(kvl_array[j+12]) * Oxygen_Deficit * Volume * R * temp)/(Molecular_Weight_O2 * Q * barometric * fO2)
  OTE_array[j+12] = OTE.magnitude

plt.plot(Airflow_array, OTE_array, 'o')
plt.xlabel('Air Flow Rate (μmole/s)')
plt.ylabel('OTE')
plt.title('OTE vs Air Flow Rate')
plt.show()
```
