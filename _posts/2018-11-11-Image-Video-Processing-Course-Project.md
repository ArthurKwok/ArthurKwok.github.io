---
layout:     post
title:      Image Video Processing Course Project 1
subtitle:   
date:       2018-11-11
author:     Arthur
header-style: text
catalog: true
mathjax: true
tags:
    - ImageVideoProcessing
    - OpenCV
---

## Code Structure

The algorithms for each task is in `CourseProj1.h`,  which has been separated for each task by comments. There are also some assistant functions that computes some middle results or analysis graphs.

In `image.cpp`, there are 6 functions called `runTask1()` to `runTask6()`, which run the algorithm with different parameters, and generate the output picture.

I use `hconcat()` and `vconcat()` to make the result pictures in one picture for each task, ans save them in the folder. Since the concat needs many columns, I just put them in a specific function.

## Task 1

> Use three different PGM images to perform the following options:
• log transformation and plot the transfer function
• γ correction with = 0.25,0.5,1,1.5,2

### log transformation

#### Analysis

The log transformation follows the formula:
$$y=log(r+1)$$

The plus one is to avoid the case $r=0$, where the log function has no definition.

The transferfunction is like:
![image_1crr685kt150rj9pctb2lpftl9.png-16.1kB][1]

By observing the transfer function, we can see that the log transformation can enhance the values in relatively low level, while reducing the effect of high level input.

#### Code
The point is that, remember to convert the data type of matrix to `CV_32F` or `CV_64F`, since the `imread` generally reads matrices as `CV_8U`. However, if you want to take transformations like log, the numbers must be float.
```cpp
Mat CourseProj1::logTransformation(Mat inMat)
{
	inMat.convertTo(inMat, CV_64F);
	Mat outMat;
	outMat.create(inMat.size(), inMat.type());

	for (int i=0; i<outMat.rows; i++)
	{
		for (int j=0; j<outMat.cols; j++)
		{
			outMat.at<double>(i,j) = std::log(inMat.at<double>(i,j)+1.0);
		}
	}

	normalize(outMat, outMat, 0.0, 1.0, NORM_MINMAX);
	return outMat;	
}
```

#### Result
![Task1_log.png-496.4kB][2]

In the result, we can see the contrast of the image has been reduced a little bit, and the some dark region in the pictures has been lightened. This phenomenum matches our observation of the transfer function.

Overall, the pictures appears to be brighter, and more obvious in some detail.

### Gamma Correction

#### Analysis

The gamma correction follows the formula:
$$y = r^{\gamma}$$

According to the textbook, the transfer functions are:

![Screenshot from 2018-11-09 11-19-30.png-117.5kB][3]

The gamma correction has three types:

 - when $\gamma<1$, the correction acts similar with the log transformation, that wil make the picture brighter, while the most dark regions remain the same;
 - when $\gamma>1$, some region will become darker, while the most bright regions remain the same.
 - when $\gamma=1$, the output is identical with the input.


#### Code
Similar as log transformation, but an input parameter `gamma` is added to perform choiceable values for gamma.
```cpp
Mat CourseProj1::myGammaCorrection(Mat inMat, double gamma)
{
	Mat outMat;
	outMat.create(inMat.size(), inMat.type());

	for (int i=0; i<outMat.rows; i++)
	{
		for (int j=0; j<outMat.cols; j++)
		{
			outMat.at<double>(i,j) = pow((inMat.at<double>(i,j)),gamma);
		}
	}

	return outMat;	
}
```

#### Result

Gamma:
![Task1_Gamma_Correction.png-1190.4kB][4]

The left-most three pictures are the original ones, and the gamma value changes from left to right is 0.25/0.5/1/1.5/2.

Same as the transfer function tells, when $\gamma<1$, then pictures are brighter than the original images; when $\gamma=1$, they are the same as original ones; when $\gamma>1$, they looks darker than original, while some white regions remains ro be white.

## Task 2
> Generate a 256x256 8-bit image with gray levels varying (using a gaussian distribution) in the range [200, 220], insert a 100x100 square in the range [80, 100]. The image represents an object in a light gray background. Use its histogram information to design an algorithm to remove the background.

#### Code
First of all, we need to generate the image as instructed.
In the function `generateTask2()`, I first generate the backe ground with a random generator `RNG`, specifying a normal distribution with mean=210 and variance=10; Then, take a sub matrix of the center, and change the values into another RNG with mean=90 and variance=10.

```cpp
Mat CourseProj1::generateTask2()
{
	Mat gen;
	gen.create(256,256,CV_8U);
	RNG rng;
	// background
	rng.fill(gen, RNG::NORMAL, 210.0, 10.0);

	// take the center as sub matrix
	Mat object(gen, Rect(77,77,100,100));
	// object
	rng.fill(object, RNG::NORMAL, 90.0, 10.0);

	gen.convertTo(gen, CV_64F);
	return gen;
}
```

In order to observate the change in histogram, I wrote an assistant function to plot the histogram of an arbitrary matrix:

```cpp
Mat CourseProj1::drawHist(Mat &inMat)
{
	Mat image;
	inMat.copyTo(image);
	normalize(image, image, 0, 255, NORM_MINMAX);
	image.convertTo(image, CV_8U);
	int bins = 256;
	int histSize[] = {bins};
	float iranges[] = {0,256};
	const float* ranges[] = {iranges}; 
	MatND image_hist;
	int channels[] = {0};
	calcHist(&image, 1, channels, Mat(), image_hist, 1, histSize, ranges, true, false);
	int hist_w = 256; int hist_h = 256;
	int bin_w = cvRound( (double) hist_w );

	Mat histImage(hist_h, hist_w, CV_8U, Scalar(0));

	normalize(image_hist, image_hist, 0, histImage.rows, NORM_MINMAX, -1, Mat());

	for (int i=1; i< 256; i++)
	{
		line( histImage, Point(i-1, hist_h - cvRound(image_hist.at<float>(i-1)) ) ,
						Point(i, hist_h - cvRound(image_hist.at<float>(i)) ),
						Scalar(255,255,255),2,8,0);
	}

	histImage.convertTo(histImage,CV_64F);
	return histImage;
}
```

#### Analysis

![Task2_.png-101.7kB][5]

The upper left image is the generated image with noisy background, with down left its histogram. We can see the histogram has two peaks: the left ans smaller peak refers to the object in center, while the right taller peak refers to the grey back ground.

In order to remove the back ground, I used a local mean filter, which discards every point if the local mean is greater than 150.

As a result, down right is the histogram of the denoised image. The left white line is all the values with 0 intensity. In the upper right picture, we can see the object in the center is the only thing left.

```cpp
Mat CourseProj1::denoiseTask2(Mat &inMat)
{
	Mat outMat;
	inMat.copyTo(outMat);

	Mat padded;
    copyMakeBorder(outMat, padded, 1, 1, 1, 1, BORDER_DEFAULT, Scalar::all(0));
    // define local mean filter
	Mat weights = (Mat_<double>(3,3) << 1.0/9.0, 1.0/9.0, 1.0/9.0, 
										1.0/9.0, 1.0/9.0, 1.0/9.0, 
										1.0/9.0, 1.0/9.0, 1.0/9.0);
    // compute local mean
	for (int x=0; x<inMat.cols; x++)
	{
		for (int y=0; y<inMat.rows; y++)
		{
			Mat local(padded, Rect(x, y, 3, 3));
			// discard if local mean > 150
			if (sum(local.mul(weights)).val[0] > 150)
				outMat.at<double>(y,x) = 0.001;
		}
	}
	outMat.convertTo(outMat, CV_64F);

	return outMat;
}
```


## Task 3
> Write a program to perform spatial filtering of an image. You can input the size of the spatial mask and the weight coefficients. Test your program using a blurring filter.

#### Analysis
To perform such a task, we need three steps:

 1. padding the image according to the size of the filter;
 2. iteration to get every local sub matrix;
 3. multiply each element of the sub matrix and the weights, and sum up to give the output.

In OpenCV, there is a function of matrix, `matrixA.mul(matrixB)` that multiply each element from `matrixA` and `matrixB`, which is perfect for our task.

#### Code
```cpp
Mat CourseProj1::localFilter(Mat inMat, int filtSize, Mat weights)
{
	Mat padded;
	Mat outMat = Mat::zeros(inMat.size(), CV_64F);
	// padding
	int pad = (filtSize - 1) / 2;
    copyMakeBorder(inMat, padded, pad, pad, pad, pad, BORDER_CONSTANT, Scalar::all(0));
	// iteration 
	for (int x=0; x<inMat.cols; x++)
	{
		for (int y=0; y<inMat.rows; y++)
		{
			Mat local(padded, Rect(x, y, filtSize, filtSize));
			// g(x,y) = sum(A.*B)
			outMat.at<double>(y,x) = sum(local.mul(weights)).val[0];
		}
	}

	return outMat;
}
```

#### Result
When testing the result, we must at first generate our weights matrix. So I've done it in `runTask3()`, generate a 5x5 mean filter:

```cpp
void runTask3(Mat image)
{
	CourseProj1 CP(image);
	Mat task3;
	Mat blur = (Mat_ <double>(5,5) << 1.0/25.0, 1.0/25.0, 1.0/25.0, 1.0/25.0, 1.0/25.0, 
									1.0/25.0, 1.0/25.0, 1.0/25.0, 1.0/25.0, 1.0/25.0, 
									1.0/25.0, 1.0/25.0, 1.0/25.0, 1.0/25.0, 1.0/25.0, 
									1.0/25.0, 1.0/25.0, 1.0/25.0, 1.0/25.0, 1.0/25.0, 
									1.0/25.0, 1.0/25.0, 1.0/25.0, 1.0/25.0, 1.0/25.0);

	hconcat(image, CP.localFilter(image, 5, blur), task3);
	namedWindow("Task3: Spatial Filter with blur weights", WINDOW_AUTOSIZE);
	imshow("Task3: Spatial Filter with blur weights", task3);
}
```
The Result picture is as below:
![task3_blur5x5.png-158.9kB][6]


## Task 4
>  Add random noise to an image, implement 3x3, 5x5 median filters to the noise added images. Also plot the image histograms before and after the filters.

#### Analysis
Similar as task2, I use the random number generator `RNG` to add random noise to an image.
Since two sizes of median filter is requested, the median filter function must be able to handle arbitrary input of filter size.
Finally, the `drawHist()` function is the same as task2.

#### Code
The `addNoise()` function is relatively easy:

```cpp
Mat CourseProj1::addNoise(Mat &inMat)
{
	Mat outMat;
	inMat.convertTo(outMat, CV_64F);

	RNG rng;
	for (int x=0; x<inMat.cols; x++)
	{
		for (int y=0; y<inMat.rows; y++)
		{
			outMat.at<double>(y,x) = outMat.at<double>(y,x) + rng.gaussian(0.05);
		}
	}

	return outMat;
}
```

For the local median filter, there are some steps:
 1. pad the matrix;
 2. iteration for each local sub matrix;
 3. for the local matrix, do: 

    1. copy to a new matrix, to make continuous
    2. reshape the matrix into 1 row
    3. copy the matrix to a vector
    4. use `nth_element` to sort the first half of the vector, which can get median value
    5. output the median value

```cpp
Mat CourseProj1::localMedianFilter(Mat inMat, int filtSize)
	{
	// median filter
	inMat.convertTo(inMat, CV_64F);
	Mat padded;
	Mat outMat = Mat::zeros(inMat.size(), CV_64F);
	// padd
	int pad = (filtSize - 1) / 2;
    copyMakeBorder(inMat, padded, pad, pad, pad, pad, BORDER_CONSTANT, Scalar::all(0));
	// iteration
	for (int x=0; x<inMat.cols; x++)
	{
		for (int y=0; y<inMat.rows; y++)
		{
			Mat Input(padded, Rect(x, y, filtSize, filtSize));
			Mat local;
			Input.copyTo(local);
			local = local.reshape(0,1); // spread local Mat to single row
			std::vector<double> vecFromMat;
			local.copyTo(vecFromMat); // Copy local Mat to vector vecFromMat
			std::nth_element(vecFromMat.begin(), vecFromMat.begin() + vecFromMat.size() / 2, vecFromMat.end());

			outMat.at<double>(y,x) = (double)vecFromMat[vecFromMat.size() / 2];
		}
	}
	return outMat;
}
```

#### Result
![Task4.png-346.4kB][7]

From left to right, the images are:
Original image, noised image, after 3x3 median filter, after 5x5 median filter.

By obeserving the histogram, we can find some traces:

 - When noise is added, the peaks in histogram of the original image has been flattened, which mean some detail and information has been erased.
 - After the median filter, it can reconstruct some peaks of the histogram, which means it can restore some details.
 - However, this kind of reconstruction is not loss-free. Even though the 5x5 histogram has many higher peaks, the actual result in the image is not satisfying, since more details are lost.

## Task 5
> Use the unsharp masking technique to enhance three images.

#### Analysis
The unsharp masking follows the steps:

1. Use the blurring filter to get a blurred image;
2. Subtract the blurred image to get the edges of the image;
3. Add a factorized result of step 2 to the original image, to get a sharpened image.

#### Code
For step 1, I used the function in Task3 to perform the blurring.
For step 2 and 3, I conbine them in one line, since this can reduce the amount of calculation.

```cpp
Mat CourseProj1::unsharpMask(Mat &inMat)
{
	Mat outMat;
	inMat.convertTo(outMat, CV_64F);
	// Step 1 : get blurred image
	Mat padded;
    copyMakeBorder(outMat, padded, 1, 1, 1, 1, BORDER_DEFAULT, Scalar::all(0));
	Mat weights = (Mat_<double>(3,3) << 1.0/9.0, 1.0/9.0, 1.0/9.0, 
										1.0/9.0, 1.0/9.0, 1.0/9.0, 
										1.0/9.0, 1.0/9.0, 1.0/9.0);

	for (int x=0; x<inMat.cols; x++)
	{
		for (int y=0; y<inMat.rows; y++)						
		{
			Mat local(padded, Rect(x, y, 3, 3));
			// Step2 and 3: subtract the blurred from original, then add it to the original
			outMat.at<double>(y,x) = outMat.at<double>(y,x) + 
									2.0 * (outMat.at<double>(y,x) - 
									sum(local.mul(weights)).val[0]);
		}
	}
	return outMat;
}
```

#### Result 
![Task5.png-643.1kB][8]

The unsharp mask can enhance the edges of the image, and make the image sharper. As we can see in the result, the edges of the bridge, houses, and hairs, has been greatly enhanced. Since the unsharp mask adds the intensity values at the edges, the whole image also looks brighter. Especially the areas that has many edges, becomes greatly brighter than the original image.


## Task 6
> Implement IHPF, BHPF, GHPF on three images.

#### Analysis
To implement these three high pass filters, the best method is the implement them in a single function.

It is more convenient for useand for programming. 

To perform a frequency domain filtering, follow the steps:

1. Pad the image for an optimal DFT size;
2. Perform DFT for a 2-channel complex image;
3. use FFT shift to shift to the center;
4. Perform frequency fomain filtering, according to different filters chosen;
5. Perform IDFT;
6. Perform FFT shift to shift back.

#### Code
There are two functions in task6:

 - `freqFilt()` is to perform DFT, FFTshift, and some conversions.
 - `genFilter()` is to calculate the high pass filter value, according to input mode.

The input parameter `int mode` has the definition:
```
// mode:
//		0 : Ideal Low Pass Filter
//		1 : Butterworth Low Pass Filter
//		2 : Gaussian Low Pass Filter
//		3 : Ideal High Pass Filter
//		4 : Butterworth High Pass Filter
//		5 : Gaussian High Pass Filter
```

```cpp
Mat CourseProj1::freqFilt(Mat &inMat, int mode, double cutoff)
{
	Mat im = inMat;

	// step1: DFT
	Mat padded;
    int m = getOptimalDFTSize( im.rows );
    int n = getOptimalDFTSize( im.cols );
    copyMakeBorder(im, padded, 0, m - im.rows, 0, n - im.cols, BORDER_CONSTANT, Scalar::all(0));

    Mat planes[] = {Mat_<double>(padded), Mat::zeros(padded.size(), CV_64FC1)};
    Mat complexI;
    merge(planes, 2, complexI);
    dft(complexI, complexI);
    split(complexI, planes);
	Mat realI = planes[0];
	Mat imagI = planes[1];

	FFTShift(realI, realI);
	FFTShift(imagI, imagI);

	// step2: H(u,v)
	double D0 = cutoff;
	double P = complexI.cols;
	double Q = complexI.rows;
	for (int u=0; u<complexI.cols; u++)
	{
		for (int v=0; v<complexI.cols; v++)
		{
			double D2;
			D2 = pow((double)u - P/2, 2) + pow((double)v - Q/2, 2);
			double H = genFilter(D2, D0, mode);
			realI.at<double>(u,v) *= H;
			imagI.at<double>(u,v) *= H;
		}
	}

	FFTShift(realI, realI);
	FFTShift(imagI, imagI);

	// step3: IDFT
	merge(planes, 2, complexI);
	idft(complexI, complexI);
    split(complexI, planes);

	im = planes[0];
	normalize(im,im,0,1,NORM_MINMAX);
	return im;	
}
```

```cpp
double CourseProj1::genFilter(double D2, double D0, int mode)
{
// mode:
//		0 : Ideal Low Pass Filter
//		1 : Butterworth Low Pass Filter
//		2 : Gaussian Low Pass Filter
//		3 : Ideal High Pass Filter
//		4 : Butterworth High Pass Filter
//		5 : Gaussian High Pass Filter

	double H;
	double order = 2.0d; // order

	switch(mode)
	{
	case 0: // IDLPF
		if (D2 < D0 * D0)
		{
			H = 1.0d;
		}
		else
		{
			H = 0.0d;
		}
	break;

	case 1: // BLPF
		H = 1.0d / (1.0d + pow(D2, order) / pow(D0, 2*order));
	break;

	case 2: // GLPF
		H = exp(-D2 / (2 * pow(D0, 2)));
	break;

	case 3: // IDHPF
		if (D2 < D0 * D0)
		{
			H = 0.0d;
		}
		else
		{
			H = 1.0d;
		}
	break;

	case 4: // BHPF
		H = 1.0d / (1.0d + pow(D0, 2*order) /pow(D2, order));
	break;

	case 5: // GHPF
		H = 1.0d - exp(-D2 / (2 * pow(D0, 2)));
	break;

	default:
		H = 1.0d;
	}

	return H;
}
```
#### Result

From left to right: Original image/ILPF/BLPF/GLPF

![Task6.png-898.2kB][9]

Since the high pass filter discards the low frequency components, the region with small changes will become grey after high pass filter.

In the result of Ideal High Pass Filter, there are many strange areas. This is due to the property of IHPF, since the filter's value change so dramatically.

The result of Gaussian High Pass Filter is the most satisfying, the changes are very natural and even.


  [1]: http://static.zybuluo.com/siluni/wptp9md7jofbfh9s6dqsopij/image_1crr685kt150rj9pctb2lpftl9.png
  [2]: http://static.zybuluo.com/siluni/f6sqcxggd5oh9xhn06v78w6p/Task1_log.png
  [3]: http://static.zybuluo.com/siluni/r59p26uoel9j7on4xjhv4xxi/Screenshot%20from%202018-11-09%2011-19-30.png
  [4]: http://static.zybuluo.com/siluni/082l7amtpqfs81lcgfenirau/Task1_Gamma_Correction.png
  [5]: http://static.zybuluo.com/siluni/zi5paixohgsenp3p7tq8u96t/Task2_.png
  [6]: http://static.zybuluo.com/siluni/1fdawj9zssuvrwkp7c9n56bz/task3_blur5x5.png
  [7]: http://static.zybuluo.com/siluni/ibnxtce3b3swctullijioteq/Task4.png
  [8]: http://static.zybuluo.com/siluni/fpk0undfd4vk8ggbhtzmhs4q/Task5.png
  [9]: http://static.zybuluo.com/siluni/pucvy887lezpor3m01xsmlnr/Task6.png