----
title: Obtaining Field Variability with FIELDimageR [R]
----

There is no field that is created equal. This is why obtaining covariates such as field variations can be important factors in helping explain data. Field variations could include slope, soil, pests/disease, etc. Pretty much anything that can influence the performance of a crop and can help explain why something happened aka variation. Field Images have become more available with the use of drones. There is also companies that sell satellite images which then can be processes for what ever you want. In this example, I will go through a quick method to obtain field variants using an overhead image of a field obtained through Google Maps. Ideally we would get a real image via drones but google satellite images are adequate	. I will be showing a very easy to use R package designed for analyzing images from drones but has many other applications as well. This R package is called [FIELDimageR](https://www.opendronemap.org/fieldimager/). The example below does follow the packages example fairly close but tweaked to our example which is slightly more complex than the package example. 

We will start by loading the required libraries and setting the working directory.
```` R
#Load libraries
library(FIELDimageR)
library(raster)

#Set working dir
setwd("~/Documents/Image_Phenotyping/")
````
Then we will load in our image and view it with *plotRGB* to make sure it loaded in correctly.

```` R
#Load in image and plot
imgIn = stack("Field_Image.png")
plotRGB(imgIn, r = 1, g = 2, b = 3)
````
<img src="/assets/img/Field_Img1.png">

Most images will need trimming of the borders to just target the area of interest. Using *fieldCrop* we can grab just our intended area.
```` R
#Crop image
imgIn.Crop = fieldCrop(mosaic = imgIn)
````
<img src="/assets/img/Field_Img2.png">

In our example, you can see that the fields are not directly vertical. We can fix the rotation of the image using *fieldRotate* which will realign the image to be vertical based on a line you draw along the side of the plots.

```` R
#Fix rotation of image
imgIn.Rotated = fieldRotate(mosaic = imgIn.Crop, clockwise = F, h=F) # h=horizontal
````
<img src="/assets/img/Field_Img3.png">

Now this is where you begin to segment the image based on colors to filter out or select areas in your image. I like to use *fieldIndex* to test out all the indices for my image. This gives me an idea of what induces I need to use to segment each area of my image. By plotting the defat RGB indexes, I can see that I can filter out the in between plots using a green index. I don’t believe you can use the direct RGB for filleting on so I usually use the *myIndex* parameter to create my own index to filter on. You use *fieldMask* to segment the area you want and don’t want to select.

```` R
#View indeces for image segmenting
fieldIndex(imgIn.Rotated, myIndex = "Green+Green")

#Remove soil using green bands
imgIn.RemSoil = fieldMask(mosaic = imgIn.Rotated, 
                         myIndex = "Green + Green", 
                         cropValue = 175, 
                         cropAbove = F)
````
#### Testing Induces
<img src="/assets/img/Field_Img4.png">

#### Removing Soil/Background
<img src="/assets/img/Field_Img5.png">

#### End Result of Selected Fields
<img src="/assets/img/Field_Img6.png">

We now have our fields segments out and now we can divide our fields based on as many blocks as you want. For this example, I separated out each field and then blocked half of each field so we can try to capture as much variability as we can. You can use *fieldShape* to create these blocks in your image which then we can extract the average index data for each block.

```` R
#Set fields
imgIn.Shape = fieldShape(mosaic = imgIn.RemSoil,
                        ncols = 5, 
                        nrows = 2)
````
<img src="/assets/img/Field_Img7.png">

Last we can extract color index data from the images. We first assigned which indices we want data for using *fieldIndex*. Then we combine the segmentation with the shape data we assigned earlier. This puts the data into a variable were we can extract the data from using *fieldInfo* and then put that data into a data frame by targeting the data info in the shape variable. This will put the data into a usable form that then we can add to the collected field data like yield we obtain from each plot.

```` R
#Get data of indices
imgIn.Indices = fieldIndex(mosaic = imgIn.RemSoil$newMosaic, 
                         Red = 1, 
                         Green = 2, 
                         Blue = 3)

#Pull data indices
imgIn.Info = fieldInfo(mosaic = imgIn.Indices,
                     fieldShape = imgIn.Shape$fieldShape)

#View data of each field
outData = imgIn.Info$fieldShape@data
````
<img src="/assets/img/Field_Img8.png">
<img src="/assets/img/Field_Img9.png">

And there you have it! We have successfully extracted variability of a field or crop trial using an image that we can then use to account for variability in our experiments. FieldimageR is a really amazing package and I have to give so much thanks to the developer of the package because it really is an amazing simple tool for R that can process data from images. Good Job. FieldimageR can do so much more that just work on field images and I highly recommend any agriculture R user to explore this package for themself.

Enjoy!
