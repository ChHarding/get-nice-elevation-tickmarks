### Get nice elevation tickmarks
- Calculates nicely spaced ticks with nice numbers for elevation axis based on the min/max of the data. 
- As numbers get higher and thus have wider labels, spacing between ticks increases.
- This is defined by a dictionary of step sizes for ranges (max - min) in a dictionary, where the keys is the range and the values are the step sizes (intervals) used to go from the min tick to the max tick.
- The defaults are based on my specific font and plotwidth settings but can be adjused for your plot, after some experimentation. 

```
step_sizes = {
        10: 2,
        50: 10,   
        100: 25,  
        250: 50,
        500: 100,
        1000: 250,
        2500: 500,
        5000: 1000,
        10000: 5000,
    }
```
Explantion:
  - Data range is 10:  The key 10 corresponds to a step size of 2. tick marks at intervals of 2, e.g., 0, 2, 4, 6, 8, 10 if the data goes from 0 to 10
  - Data range is 100: The key 100 corresponds to a step size of 25. tick marks at intervals of 25, e.g., 0, 25, 50, 75, 100 if the data goes from 0 to 100

There's also some logic that determines if the min and/or max values get additional ticks, based if the closes "inner" tick is at least half a step size away (which is typically the case).

Examples (with values based my font and plotwidth settings!)

![image](https://github.com/user-attachments/assets/b0f92b8e-58b3-494d-b886-58308af33df4)
![image](https://github.com/user-attachments/assets/e8768e07-a9a9-4967-8540-381a0280aec9)




Here are some results for different ranges using the above dictionary
```
min: 4018  max: 4539  range:  521  ticks: [4018, 4250, 4500]
min: 4360  max: 5339  range:  979  ticks: [4360, 4500, 4750, 5000, 5250]
min: 1526  max: 2487  range:  961  ticks: [1526, 1750, 2000, 2250, 2487]
min: 4711  max: 4864  range:  153  ticks: [4711, 4750, 4800, 4850]
min: 1353  max: 1912  range:  559  ticks: [1353, 1500, 1750, 1912]
min: 5439  max: 5857  range:  418  ticks: [5439, 5500, 5600, 5700, 5800, 5857]
min: 3455  max: 3666  range:  211  ticks: [3455, 3500, 3550, 3600, 3650]
min:  797  max: 1701  range:  904  ticks: [797, 1000, 1250, 1500, 1701]
min: 5167  max: 6061  range:  894  ticks: [5250, 5500, 5750, 6000]
min: -390  max:   76  range:  466  ticks: [-390, -300, -200, -100, 0, 76]
```

### How to use
- in your code import the calculate_ticks function from the calculate_ticks module
- if needed, define your own dictionary and give to the function together with the data's min and max
- it will return a list of tickmark values that you can use with your plotting package.

``` python
from calculate_ticks import calculate_ticks
import random
import matplotlib.pyplot as plt 

# get random min and max values
elevmin = random.randint(-500, 6000)
elevmax = elevmin + random.randint(100, 1000)   

# generate 100 random values between min and max
elevs = [random.randint(elevmin, elevmax) for _ in range(1000)]

# depending on the range of the values, how many steps should be used?
step_sizes = {
        10: 2,
        50: 10,   
        100: 25,  
        250: 50,
        500: 100,
        1000: 250,
        2500: 500,
        5000: 1000,
        10000: 5000,
    }

# calculate the tickmarks
ticks = calculate_ticks(elevmin, elevmax)
print(f"min: {elevmin:4d}  max: {elevmax:4d}  range: {elevmax-elevmin:4d}", " ticks:", ticks)

# create a histogram from elevs
import matplotlib.pyplot as plt
plt.hist(elevs)
plt.title(f"Elevation histogram")
plt.xlabel("Elevation")
plt.ylabel("Count")
plt.xlim(elevmin, elevmax)
plt.show()
```

