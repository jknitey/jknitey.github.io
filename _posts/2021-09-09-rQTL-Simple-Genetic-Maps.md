---
title: Building Simple Genetic Maps with rQTL
---

One thing I have noticed that seems to always stump people is creating genetic maps. I am not going to lie but am not a pro map maker but I think I am a practical map maker. There are some older genetic map making tools out there like join map and others that can be bulky but with rQTL being a very popular QTL mapping program, I feel my mapping knowledge can help some genetic mappers out there. Genetic maps can be confusing and timely to create and what does not help is the massive amount of markers we have access to now days can take something like rQTL hours to build a genetic map. After many map builds I feel I have found a way using rQTL to build a decent genetic map in little time and not using much computational power. Once again I am no genetic map expert but a practical one. Please find my method below:

### Step 1
Here we will load the rQTL library and our data. Also make a copy of our data so if we make a mistake we can easily fix it.

``` R
#Load library
library(qtl)

#Read in data
data = read.cross(format = "csv", 
                  file = "listera_map.csv", 
                  genotypes = c("CC","CB","BB"))

#Copy data for  mistakes
data2 = data
```

### Step 2
Next we will use the simple est.map function from rQTL to make our initial genetic map. I usually know which markers are on which chromosome so I keep my chromosome assignments and genetic map on top of these. We then plot our new map and overwrite our current map so we can keep building on top of it. I have found that if you keep building on top of your maps the computation time is greatly reduced each time you run est.map which save you lots of time!

``` R
#Use est.map for a quick high throughput genetic map build
new_map = est.map(data2, offset = 0)
#Plot your new map
plotMap(new_map)
#Overwrite current map with new map
data2 = replace.map(data2, new_map)
```
Below is our first genetic map:

<img src="/assets/img/Gen_Map1.png">

I try to get my genetic maps around 100 cM per chromosome. You can see that chromosome 1, 4, 8, and 17 have very large cM gaps. These chromosomes are the ones to target for marker cleansing. Using the pull.map function you can pull each chromosome and inspect what markers are causing the large cM gaps.

``` R
#Use below to pull the problamatic chromosomes. Our example 1, 4, 8, 17
pull.map(data2, chr = c(“1”, “4”, “8”, “17”))
```
#### Output of pull.map

```
> pull.map(data2, chr = 1)
    D10M44       D1M3      D1M75     D1M215     D1M309     D1M218         M1     D1M451 
  0.000000   1.580718  30.799709  49.356357  60.996035  64.674245 180.044503 300.215953 
    D1M504     D1M113     D1M355     D1M291     D1M209     D1M155 
301.056413 311.696692 312.539793 316.210350 324.506039 325.915690

> pull.map(data2, chr = 4)
     D4M2    D4M178        M2    D4M187    D4M251 
  0.00000  21.90262  92.59906 213.49665 258.56434 

> pull.map(data2, chr = 8)
     D8M94     D8M339     D8M178     D8M242     D8M213         M3     D8M156 
  0.000000   1.710261  12.865416  31.320484  38.044010  98.976118 188.156455 

> pull.map(data2, chr = 17)
  D17M260    D17M66    D17M88   D17M129        M4 
  0.00000  14.38698  20.69463  47.47369 131.71929 
```

You can see that markers "M1", "M2", "M3", "M4" are causing some very large gaps in our map. Since I usually deal with thousands of markers, I will simply remove these markers from our data and remap shown below. I personally feel since we have hundreds or thousands of markers we can afford to lose a few ;).

``` R
#Remove markers that are causing gap issues. Our example "M1", "M2", "M3", "M4"
data2 = drop.markers(data2, markers = c("M1", "M2", "M3", "M4"))

#Remap with removed markers and “newer map”
new_map = est.map(data2, offset = 0)
plotMap(new_map)
data2 = replace.map(data2, new_map)
```

<img src="/assets/img/Gen_Map2.png">

You can now see that when we rebuild on our genetic map we get a nice clean map that gives us around 100 cM chromosomes and all this took us less than an hour to do!

``` R
#When you are happy with map, overwrite your original data for QTL mapping
data = data2

#Save your new genetic map and data to a file
write.cross(data, format = "csv", "filename")
```

And now you have a beautiful genetic map you can use for you QTL mapping which took very little time to do. I hope this quick and dirty method to build genetic maps can help someone in the future :)

ENJOY!
