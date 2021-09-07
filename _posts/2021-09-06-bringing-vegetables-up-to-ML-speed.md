---
title: Bring Vegetables Up To ML Speed
description: The method behind the madness
---

Digital phenotyping and machine learning (ML) have both become very hot topics over the past few years. Both of these topics have been very limited to either row crops like corn/wheat or something that has thousands of images, not like vegetables. I took on the challenge to see if ML could be an approach too high throughput digital phenotyping vegetables and this is my journey.

<hr>

#### How it all began

Searching for possible high throughput image phenotyping methods is quite challenging because there are not many and the ones available are not very adjustable or adaptive. My next search was to make something myself and robust! After some searching I happened to stumble across, of course TensorFlow. After some investigation, TensorFlow was a bit more complex than I wanted to get into and seemed like a steep learning curve, so I put aside for the moment. I then stumbled across something called [YOLO](https://github.com/AlexeyAB/darknet) which is a fairly simple and very good ML model, so I gave this a shot. 

#### The process

ML models take a lot of work to create so finding loop holes can be vert helpful. There are some models out there like the COCO model that are trained already and used for benchmarking models. You can use these models first to look for your vegetable of interest, or second, use it to help pseudo label tons of images for you! Third option is to label from scratch images for you model. The first method is obviously the best but the second method can save you so much time, trust me. The third method requires some nice long TV show or music and many hand massages.

Once you know which option above is for you, you can find a labeling tool that fits you [here](https://www.folio3.ai/blog/labelling-images-annotation-tool/). Most are quite good and the ones I have used are [Roboflow](https://roboflow.com/) and [LableIMG](https://pypi.org/project/labelImg/). I like the anonymous and open source of LableIMG, but it has some glitches, however, works well. Roboflow was amazing and is very easy to use but they started to really lockdown their users and make people pay for their system.. annoying. If you don’t care for anonymous go Roboflow, else LableIMG is good. FYI, LableIMG is a python built tool and will need to be ran with python.

After you have images annotated you can now start training a model following YOLO’s manual. You will need some GPUs for this training and something like Google Colab notebook can help if you don’t have access to GPU/TPUs. After many hours of model training you should have a ML model that you can begin to use for high throughout image phenotyping!

Most likely if you just train a model to recognize a vegetable the best thing you can do is counting. 

<img src="/assets/img/Count_Apples.png"> 

However, if you standardize your images like take then all in the same environment and same camera distance, you can then compare image to image. Another option which is my favorite and most robust is getting your model to recognize your vegetable AND a standard like a coin or poker chip (see below). Then you can use this standard to normalize your images/vegetables which make your images and analysis very robust. This also allows you to get real measure guesstimates of your vegetables.

<img src="/assets/img/Annotated_Apple.png"> 

YOLOv4 does not produce results that are very tabular (shown below) so the results will need to be digested into a usable format.

```bash
sports ball: 99%    (left_x:  135   top_y:   85   width:  493   height:  471)
apple: 96%    (left_x:  655   top_y:  362   width: 1036   height:  867)
```

I achieved this using R which is my main data analysis language but something like python can also be used. What I ended up doing was removing all data that was not useful and only keeping the width and height of the bounding boxes produced by YOLO. I then used some loops to cycle through each image to first count the vegetables in an image and then get the width and height of each vegetable in an image. Once you have the vegetable shape data you can normalize each one using you standard in the same image. I was then left with a bunch of vegetable data that I could then compare to each other across images. You can take this a step further and get areas and ratios per vegetable. Further analysis can be done to get things such as uniformity and then using ML, again, to get predictions of other traits. Below is a small example of image normalizing:

```
Cleaned data:
sports ball: width:  493   height:  471
apple: width: 1036   height:  867
```

Baseballs have a diameter of about 7.4 cm. This means for the ball above in the image, every 65.14 pixels  is about 1 cm. (493+471)/2 = 482 avg pixel of image ball. Then 482/7.4 cm = 65.14. But remember this is only for this image and anything in this image! This normalization should be done for each image separately.

###### Boom! 
You should now have a very robust high throughput digital phenotyping method you can use in your vegetable program! You can even take you ML model further and use it with videos but this takes much deeper analysis and planning.
