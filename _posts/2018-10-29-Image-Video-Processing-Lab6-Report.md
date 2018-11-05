---
layout:     post
title:      Image Video Processing Lab6 Report
subtitle:   
date:       2018-11-05
author:     Arthur Kwok
header-img: img/post-bg-miui6.jpg
catalog: true
tags:
    - ImageVideoProcessing
---

## 0. Code Strcure
The Project code was written under the environment of Ubuntu 18.04 and Visual Studio Code.

I used the OOP structure, creating a class called `Lab6`.
In the constructor of `Lab6` I input the image as `Mat`, along with the `width` and `height` information.

class definition:


## 1. homomorphic filter
code:
```cpp
Mat Lab6::homomorphic()
{
	Mat im;
	image.convertTo(im, CV_64FC1);
	normalize(im, im, 0, 1, NORM_MINMAX);

	// step1: ln
	for (int i=0; i<im.cols; i++)
	{
		for (int j=0; j<im.rows; j++)
		{
			im.at<double>(i,j) = log(1.0 + im.at<double>(i,j));
		}
	}
	// print(im);

	// step2: DFT
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

	print(realI);

	// step3: H(u,v)
    double gamma_H = 2.0d;
	double gamma_L = 0.5d;
	double D0 = 30.0d;

	double P = complexI.cols;
	double Q = complexI.rows;
	for (int u=0; u<complexI.cols; u++)
	{
		for (int v=0; v<complexI.cols; v++)
		{
			double D2;
			D2 = pow((double)u - P/2, 2) + pow((double)v - Q/2, 2);
			double H;
			H = (gamma_H - gamma_L)*(1.0d - exp(-(D2/(D0*D0)))) + gamma_L;
			realI.at<double>(u,v) *= H;
			imagI.at<double>(u,v) *= H;
		}
	}
	FFTShift(realI, realI);
	FFTShift(imagI, imagI);

	// step4: IDFT
	merge(planes, 2, complexI);
	idft(complexI, complexI);
    split(complexI, planes);
	// print(planes[0]);

	// step5: exp
	cv::exp(planes[0], planes[0]);
	cv::exp(planes[1], planes[1]);
	print(planes[0]);

	// magnitude(planes[0], planes[1], im);
	// print(im);

	return planes[0];
}
```

## 2. bandreject filter
code:

```cpp
Mat Lab6::bandReject(Mat im)
{
	im.convertTo(im, CV_64FC1);
	normalize(im, im, 0, 1, NORM_MINMAX);

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
	double D0 = 58.0d;
	double W = 10.0d;
	double P = complexI.cols;
	double Q = complexI.rows;
	for (int u=0; u<complexI.cols; u++)
	{
		for (int v=0; v<complexI.cols; v++)
		{
			double D2;
			D2 = pow((double)u - P/2, 2) + pow((double)v - Q/2, 2);
			double H;
			H = 1.0d - exp(-(pow(D2-D0*D0,2) / (D2*W*W)));
			realI.at<double>(u,v) *= H;
			imagI.at<double>(u,v) *= H;
		}
	}

	FFTShift(realI, realI);
	FFTShift(imagI, imagI);

	// step4: IDFT
	merge(planes, 2, complexI);
	idft(complexI, complexI);
    split(complexI, planes);

	magnitude(planes[0], planes[1], im);
	normalize(im,im,0,1,NORM_MINMAX);
	return im;
	
}

```
result:
![banreject.png-357.2kB][1]

## 3. correlation
code:

```cpp
Mat Lab6::compare(Mat head, Mat judge1, Mat judge2)
{
	Mat result1;
	Mat result2;
	matchTemplate(judge1, head, result1, TM_CCORR);
	matchTemplate(judge2, head, result2, TM_CCORR);

	double min1, max1, min2, max2;
	minMaxLoc(result1, &min1, &max1);
	minMaxLoc(result2, &min2, &max2);

	if (max1 > max2)
	{
		std::cout << "The first picture has cat!" << std::endl;
	}
	else
	{
		std::cout << "The second picture has cat!" << std::endl;
	}

}
```

input image:
template:
![cat.jpg-24.8kB][2]
piciture1:
![landscape_cat.jpg-104.5kB][3]
picture2:
![landscape_nocat.jpg-189.5kB][4]

result:
```cpp
The first picture has cat!
```


  [1]: http://static.zybuluo.com/siluni/5c7c1q0x3527n9aad4t4jo61/banreject.png
  [2]: http://static.zybuluo.com/siluni/nrxtp3d599jcek2u2g6hr9c2/cat.jpg
  [3]: http://static.zybuluo.com/siluni/jei52h51y4laa4e7lfg5gk6w/landscape_cat.jpg
  [4]: http://static.zybuluo.com/siluni/hbxumr37xlcqf8vvv3gpd4sw/landscape_nocat.jpg