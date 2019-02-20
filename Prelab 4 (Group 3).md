Lynn Li (ll643) and Allison Tran (ant42)
Prelab 4 (Gas Transfer)
Due 02/20/2019
Group 3

#Question 1
#####Calculate the mass of sodium sulfite needed to reduce all the dissolved oxygen in 600 mL of pure water in equilibrium with the atmosphere at 22 degrees C.

Constants from : https://www.umass.edu/mwwp/protocols/lakes/oxygen_lake.html
```python
from aguaclara.core.units import unit_registry as u
u.define('equivalent = mole = eq')
import aguaclara.research.environmental_processes_analysis as epa
from aguaclara.core import utility as ut

Volume_Water = (600*(u.mL)).to(u.L)
Dissolved_Oxygen = (8.8*(u.mg/u.L)) #100% saturation at 22 degree C
Mass_Oxygen = Volume_Water*Dissolved_Oxygen
Molar_Mass_Oxygen = (32000*(u.mg))/(1*(u.mole))
Mol_Oxygen = Mass_Oxygen/Molar_Mass_Oxygen
#2Na2SO3 + O2 → 2Na2SO4
#Source: https://chemiday.com/en/reaction/3-1-0-150
Mol_Na2SO3 = Mol_Oxygen*2
Molar_Mass_Na2SO3 = (126000*(u.mg))/(1*(u.mol))
Mass_Na2SO3 = Mol_Na2SO3*Molar_Mass_Na2SO3
print ('The mass of sodium sulfite needed to reduce all the dissolved oxygen in 600mL of pure water in equilibrium with the atmosphere at 22 degrees C is', ut.round_sf(Mass_Na2SO3,3),'.')
```

#Question 2
#####Describe your expectations for dissolved oxygen concentration as a function of time during a reaeration experiment. Assume you have added enough sodium sulfite to consume all of the oxygen at the start of the experiment. What would the shape of the curve look like?

As sodium sulfite is being added, the DO concentration will  exponentially decrease from the initial DO concentration, past its critical point, until it reaches zero. Afterwards as time goes on, the DO concentration will increase exponentially until it reaches the saturated DO concentration. On the oxygen sag curve, this will look like an inverse exponential curve that is concave up which starts on the y-axis at the initial DO concentration, dips at the zero concentration point, and then a square root curve that stops once it hits DOsat.

#Question 3
#####Why is ${\widehat{k}}_{\nu, l}$ not zero when the gas flow rate is zero? How can oxygen transfer into the reactor even when no air is pumped into the diffuser?

${\widehat{k}}_{\nu, l}$ is the overall volumetric oxygen transfer coefficient. It is specific for a particular flow rate AND reactor design/configuration; therefore it is not necessarily zero if the gas flow rate is zero.

${\widehat{k}}_{\nu, l}$ is a function of the interface surface area (A), the liquid volume (V), the oxygen diffusion coefficient in water (D), and the thickness of the laminar boundary layer ($\delta$):
$${\widehat{k}}_{\nu, l} = f(D, \delta, A, V)$$
$${\widehat{k}}_{\nu, l} = \frac{AD}{V \delta}$$

Oxygen can be transferred into the reactor even when no air is being pumped into the diffuser because oxygen transfer is controlled by the partial pressure of oxygen in the atmosphere (0.21 atm) and the corresponding equilibrium concentration in water (approximately 10 mg/L). Therefore oxygen transfer can occur at the surface of the reactor. However, the efficiency of this type of oxygen transfer is very low.

# Question 4
#####Describe your expectations for ${\widehat{k}}_{\nu, l}$ as a function of gas flow rate. Do you expect a straight line? Why?
We expect that as gas flow rate increases, ${\widehat{k}}_{\nu, l}$ will linearly increase as well.

We can see how ${\widehat{k}}_{\nu, l}$ varies with gas flow rate by integrating:
$$\frac{dC}{dt}={\widehat{k}}_{\nu, l}(C^*-C)$$
$$\int _{C_{0} }^{C}\frac{dC}{C^{*} -C}  =\int _{t_{0} }^{t}\hat{k}_{v,l} dt$$
$$\Rightarrow ln(\frac{C^{*}-C}{C^{*}-C_0})=-{\widehat{k}}_{\nu, l}(t-t_0)$$
Which is in the linear form of: $y=mx+b$ where ${\widehat{k}}_{\nu, l}$ is the slope of the line. Therefore we expect a straight line.

# Question 5
#####A dissolved oxygen probe was placed in a small vial in such a way that the vial was sealed. The water in the vial was sterile. Over a period of several hours the dissolved oxygen concentration gradually decreased to zero. Why? (You need to know how dissolved oxygen probes work!)

The DO concentration eventually became zero because the vial was sealed. A dissolved oxygen probe reduces $O_2$ to $H_2O$ by applying an electrical potential of 0.8V. Because the vial was sealed, no new oxygen was introduced into the vial after all of the initial oxygen present was reduced to water. Therefore, the DO concentration gradually decreased to zero.
