---
title: "File based calculators"
teaching: 15
exercises: 15
questions:
       - "How can I calculate standard properties using a file-based calculator?"
       - "What happens behind the scenes in a file-based calculator?"
       - "When does ASE cache results from a calculation?"
objectives:
       - "Calculate properties using a file-based `Calculator` object"
       - "Understand how file-based calculators work behind the scenes"
       - "Understand when results can be retrieved from the cache, and when they will be re-calculated"
keypoints:
       - "Typical academic codes are controlled by _input files_ and write their results to _output files_"
       - "The workflow for file-based and built-in calculators are the same"
       - "However behind the scenes, file-based calculators work differently"
       - "The Calculator object caches calculation results"
       - "After changing a parameter the cache is invalidated"
---

> ## Code connection
> In this episode we explore the [`ase.calculators.castep` module](https://databases.fysik.dtu.dk/ase/ase/calculators/castep.html), which is a file-based calculator for calculating standard properties (energy, forces and stress) from a set of atomic positions.
{: .callout}

### Typical academic codes are controlled by _input files_ and write their results to _output files_

- This workflow is convenient for batch calculations on clusters (and for Fortran programmers...)
- To interoperate with this type of code, the ASE Calculator needs to: i) prepare appropriate input; ii) call the executable; iii) read the output
- This will allow us to build a similar workflow to the built-in Calculators explored in the previous episode.

### The workflow for file-based and built-in calculators are the same

- The file-based calculator [CASTEP](https://castep-docs.github.io/castep-docs/) implements plane-wave pseudopotential electronic structure calculations in periodic boundary conditions. 
- To calculate a system energy we use the same workflow as introduced in [episode 4](../04-in-built-calculators/index.html).
- First, we build an `Atoms` object

~~~
import ase.build
from ase.visualize import view

atoms = ase.build.bulk('Ag', cubic=True)
view(atoms, viewer='ngl')
~~~
{: .python}

- Second, we attach a calculator: in this case, Castep.

~~~
from ase.calculators.castep import Castep
atoms.calc = Castep(xc='pbesol', directory='silver', kpts=(6, 6, 6))
~~~
{: .python}

- Third, we calculate an energy. This will take a few seconds to run;
  although the unit cell is small we specified quite a few k-points.

~~~
print("Energy: ", atoms.get_potential_energy())
~~~
{: .python}

~~~
Energy: -2611.299475375
~~~
{: .output}

- As part of this process some data was also written to the Calculator `results`.

~~~
atoms.calc.results
~~~
{: .python}

~~~
{'energy_without_dispersion_correction': -2611.288222757,
 'free_energy_without_dispersion_correction': -2611.310727993,
 'energy_zero_without_dispersion_correction': -2611.299475375,
 'forces': array([[ 2.e-05, -1.e-05, -0.e+00],
                  [-2.e-05,  1.e-05,  0.e+00]]),
 'charges': array([-0.,  0.]),
 'energy': -2611.299475375,
 'free_energy': -2611.310727993}

~~~
{: .output}

> ## Note
> Castep is one of the calculators that doesn't support get_properties() yet... We can still get a nice results container this way, though!
{: .callout}

### However behind the scenes, file-based calculators work differently

- When we requested the potential energy, the Calculator generated input files based on the *label* and *directory* parameters: "silver/castep.cell" and "silver/castep.param". 
- These are human-readable files: you can take a look at them.

~~~
cat silver/castep.cell silver/castep.param
~~~

- Hopefully the content looks somewhat familiar from this morning's CASTEP tutorial
- ASE has added some comment lines with extra information which can be used in a "restart"; generally this isn't needed, however.

> ## Exercise: Calculating energy and forces
> Can you find the energy and forces in this file? Do they agree with the values from ASE?
>
> Hint: CASTEP produces a few different energy values.
{: .challenge}

### The Calculator object caches calculation results

- This means we can mostly use the property getters without worrying about wastefully running unnecessary calculations.
- For example, we can request the forces and receive them instantly as no new calculation is required.
- The forces were already present they are simply retrieved from the cache.

~~~
print(atoms.get_forces())
~~~
{: .python}

~~~
array([[ 2.e-05, -1.e-05, -0.e+00],
       [-2.e-05,  1.e-05,  0.e+00]])

~~~
{: .output}

### After changing a parameter the cache is invalidated

- For example, we can select the PBE functional instead of PBEsol.
- Now when the potential energy is requested, a new calculation is performed.

~~~
atoms.calc.set(xc='PBE')
atoms.get_potential_energy()
~~~
{: .python}

~~~
MOPAC Job: "isopropyl-alcohol.mop" ended normally on Apr  3, 2023, at 21:29.

-2.8723671252448977
~~~
{: .output}

> ## Discussion
> What happens to calc.results when a parameter is changed? When might we prefer to use `atoms.get_forces()` vs `atoms.calc.results['forces']`?
{: .discussion}
