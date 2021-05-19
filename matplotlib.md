# Matplotlib

Some useful plots.

## Table Of Content

- [Matplotlib](#matplotlib)
  - [Table Of Content](#table-of-content)
  - [Simple plot](#simple-plot)
  - [Misc](#misc)
  - [Axis and for-loop](#axis-and-for-loop)

## Simple plot

```python
#!/usr/bin/env python3


import pandas as pd
from matplotlib import pyplot as plt
from matplotlib import rc
import matplotlib.ticker as tck
import os.path


# latex support

rc('font', **{'family': 'serif', 'serif': ['Palatino']})
rc('text', usetex=True)

# plot

files_to_plot = ['bla-vs-com.csv']

fig, axs = plt.subplots(1,1)

# FIRST PLOT

data1 = pd.read_csv(files_to_plot[0], delimiter=",")

labels = data1[data1.columns[0]]
x = data1[data1.columns[1]]
y = data1[data1.columns[2]]

axs.plot(x, y, '--', marker='o', label="First", color="blue")

# set legend and title on the bottom

axs.set(xlabel='x-axis', ylabel='y-axis')

plt.tight_layout(pad=1.5)
plt.subplots_adjust(bottom=0.19)
fig.legend(loc="lower center", ncol=4)

plt.savefig(f'{os.path.basename(__file__)[:-3]}.pdf')
plt.show()
```

## Misc

```python
# add labels to points
def labelpoints(x, y, labels):
    for i, txt in enumerate(labels):
        axs.annotate(txt, (x[i], y[i]))

# use: labelpoints(x, y, labels)

# remove yticks
axs.get_xaxis().set_ticks([])

# locators

axs.xaxis.set_major_locator(tck.MultipleLocator(3))
axs.xaxis.set_minor_locator(tck.MultipleLocator(1))
```

## Axis and for-loop

```python
#!/usr/bin/env python3

import pandas as pd
from matplotlib import pyplot as plt
from matplotlib import rc
import matplotlib.ticker as tck
import os.path


# latex support

rc('font', **{'family': 'serif', 'serif': ['Palatino']})
rc('text', usetex=True)

#files

files = ['bla--monomer-different-molecule.opj.1.txt',
        'bla--dimer-different-molecule.opj.1.txt',
        'bla--trimer-different-molecule.opj.1.txt',
        'bla--tetramer-different-molecule.opj.1.txt',
        'bla--pentamer-different-molecule.opj.1.txt']

fig, axs = plt.subplots(2,3, figsize=(15,8))  # generate 6 boxes (3x2)
fig.delaxes(axs[1,2]) # remove last empty box

# MONOMER

data = pd.read_csv(files[0], delimiter=";")

x = data[data.columns[0]]  # get first column
xray = data[data.columns[1]]
m1 = data[data.columns[3]]

axs[0,0].set_title('Monomer')
axs[0,0].plot(x, xray, marker='o', color="black", label='_nolegend_')
axs[0,0].plot(x, m1, marker='o', color="orange", label='_nolegend_')
              
# DIMER

data = pd.read_csv(files[2], delimiter=";")

m1 = data[data.columns[3]]
m2 = data[data.columns[5]]

axs[0,1].set_title('Dimer')
axs[0,1].plot(x, xray, marker='o', label='_nolegend_', color="black")
axs[0,1].plot(x, m1, marker='o', color="orange")
axs[0,1].plot(x, m2, marker='o', color="red")

# TRIMER

data = pd.read_csv(files[2], delimiter=";")

m1 = data[data.columns[3]]
m2 = data[data.columns[5]]
m3 = data[data.columns[7]]

axs[0,2].set_title('Trimer')
axs[0,2].plot(x, xray, marker='o', label='_nolegend_', color="black")
axs[0,2].plot(x, m1, marker='o', label='_nolegend_', color="orange")
axs[0,2].plot(x, m2, marker='o', label='_nolegend_', color="red")
axs[0,2].plot(x, m3, marker='o', label='_nolegend_', color="green")

# TETRAMER

data = pd.read_csv(files[3], delimiter=";")

m1 = data[data.columns[3]]
m2 = data[data.columns[5]]
m3 = data[data.columns[7]]
m4 = data[data.columns[9]]

axs[1,0].set_title('Tetramer')
axs[1,0].plot(x, xray, marker='o', label='_nolegend_', color="black")
axs[1,0].plot(x, m1, marker='o', label='_nolegend_', color="orange")
axs[1,0].plot(x, m2, marker='o', label='_nolegend_', color="red")
axs[1,0].plot(x, m3, marker='o', label='_nolegend_', color="green")
axs[1,0].plot(x, m4, marker='o', label='_nolegend_', color="blue")

# PENTAMER

data = pd.read_csv(files[4], delimiter=";")

m1 = data[data.columns[3]]
m2 = data[data.columns[5]]
m3 = data[data.columns[7]]
m4 = data[data.columns[9]]
m5 = data[data.columns[11]]

axs[1,1].set_title('Pentamer')
axs[1,1].plot(x, xray, marker='o', label='X-ray', color="black")
axs[1,1].plot(x, m1, marker='o', label='Molecule 1', color="orange")
axs[1,1].plot(x, m2, marker='o', label='Molecule 2', color="red")
axs[1,1].plot(x, m3, marker='o', label='Molecule 3', color="green")
axs[1,1].plot(x, m4, marker='o', label='Molecule 4', color="blue")
axs[1,1].plot(x, m5, marker='o', label='Molecule 5',color="indigo")

# set axs globally

for column in range(2):
    for row in range(3):
        axs[column,row].set(xlabel='Bond number', ylabel='Bond length [Å]')
        axs[column,row].xaxis.set_major_locator(tck.MultipleLocator(1))
        axs[column,row].yaxis.set_major_locator(tck.MultipleLocator(0.03))
     
# set legend and title on the bottom

plt.tight_layout(pad=1.5)
plt.subplots_adjust(bottom=0.15)
fig.legend(loc="lower center", ncol=6)

plt.savefig(f'{os.path.basename(__file__)[:-3]}.pdf')
plt.show()
```

## IR spectra comparison

```python
#!/usr/bin/env python

import pandas as pd
from matplotlib import pyplot as plt
from matplotlib import rc


# latex support

rc('font', **{'family': 'serif', 'serif': ['Palatino']})
rc('text', usetex=True)

input1 = 'IR-monomer-gas-xtb-norm.txt'
input2 = 'IR-xray-norm.txt'

output = "xray-vs-xtb"

fig, ax1 = plt.subplots(1,1)  # overlapping plots

# plot

data1 = pd.read_csv(input1, delimiter=" ")
data2 = pd.read_csv(input2, delimiter=" ")

x1 = data1[data1.columns[0]]
x2 = data2[data2.columns[0]]
y1 = data1[data1.columns[1]] 
y2 = data2[data2.columns[1]] 

ax1.set_xlim(300,3500)
ax1.set_ylim(0,.75)
maxy1 = y1[1000:3500].max()

ax1.set_xlabel("Wavenumber [cm$^{-1}$]", labelpad=12)
ax1.set_ylabel("Intensity [a.u.]", labelpad=12)

# Turn off tick labels
ax1.set_yticklabels([])

ax1.plot(x1, y1*0.7/3+0.02, label="pyrl monomer in gas phase (xTB)")
ax1.plot(x2, y2*0.2+0.35, label="pyrl exp monomer (from CCDC 1496525)")

# set legend and title on the bottom

plt.legend()
plt.tight_layout(pad=1.2)
plt.subplots_adjust(bottom=0.2)

# save and show
# plt.show()
plt.savefig(f'{output}.pdf') 
```
