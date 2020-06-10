# Machine learning assisted data labelling

## Image multi-class and bounding box labelling

## Architecture

![alt text](https://github.com/balakreshnan/customvisionai2020/blob/master/images/mlassitresize.jpg "Architecture")

## Steps

- Copy Images to Blob location
- Resize the images - https://github.com/balakreshnan/customvisionai2020/blob/master/ReSize/resizeimages.md
- Create a GPU cluster
- Create a Data Set inside Azure machine learning service
- Create a Data label project
- Do labeling - serve as input to ML labelling
- Run labelling
- Validate output
- Conclustion

## Step 1

First lets copy the data to blob storage for further processing

## Step 2

Apply resize of images using the above URL
https://github.com/balakreshnan/customvisionai2020/blob/master/ReSize/resizeimages.md

## Step 3

Now create a new cluster for training. Select GPU since ML Assit works with GPU only as of writing to this document.

## Step 4

Now time to creaet a dataset
First click on Dataset and then click Create new data set

![alt text](https://github.com/balakreshnan/customvisionai2020/blob/master/images/dataset1.jpg "dataset")

Now create data store where the file are saved:

![alt text](https://github.com/balakreshnan/customvisionai2020/blob/master/images/dataset2.jpg "dataset")

Select the Path

![alt text](https://github.com/balakreshnan/customvisionai2020/blob/master/images/dataset3.jpg "dataset")

Confirm the details

![alt text](https://github.com/balakreshnan/customvisionai2020/blob/master/images/dataset4.jpg "dataset")

## Step 5

Create a Data label project

![alt text](https://github.com/balakreshnan/customvisionai2020/blob/master/images/datalabel1.jpg "DataLabel")

Select the existing data set created above

![alt text](https://github.com/balakreshnan/customvisionai2020/blob/master/images/datalabel2.jpg "DataLabel")

Enable Incremental refresh

![alt text](https://github.com/balakreshnan/customvisionai2020/blob/master/images/datalabel3.jpg "DataLabel")

Add labels

![alt text](https://github.com/balakreshnan/customvisionai2020/blob/master/images/datalabel4.jpg "DataLabel")

Provide label instruction if available for labellers

![alt text](https://github.com/balakreshnan/customvisionai2020/blob/master/images/datalabel5.jpg "DataLabel")

Select ML Assit labelling

![alt text](https://github.com/balakreshnan/customvisionai2020/blob/master/images/datalabel7.jpg "DataLabel")

Click Create project

Now go to project and you will land in home page

![alt text](https://github.com/balakreshnan/customvisionai2020/blob/master/images/datalabel8.jpg "DataLabel")

Experiments section should show the details

- First step go to Label data
- Go through pictures and create bounding boxes
- Validate the bounding boxes.
- The Tool will show how many pictures to do bounding boxes
- Once bounding box is done then we back to home screen and wait.
- ML will automatically start to run based on what was labelled. Usualy takes time to do this task.

The tool will guide and lets us know how much more it needed to label. Usually it needs per label 25 images for 100 images.

Once the experiment is completed then we can back and validate what ML as done and then draw boxes where is it missing.

Also check the training cluster details

Training:
![alt text](https://github.com/balakreshnan/customvisionai2020/blob/master/images/datalabel9.jpg "DataLabel")

Validations:
![alt text](https://github.com/balakreshnan/customvisionai2020/blob/master/images/datalabel10.jpg "DataLabel")

inference:
![alt text](https://github.com/balakreshnan/customvisionai2020/blob/master/images/datalabel11.jpg "DataLabel")

## Conclusion

More to come.