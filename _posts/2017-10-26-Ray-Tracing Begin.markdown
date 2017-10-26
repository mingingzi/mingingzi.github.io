---
published: true
layout: post
title: Ray-Tracing Begins
date: 2017-10-26T17:00:00.000Z
author: MagentaM
header-img: img/post-bg-2015.jpg
tags:
  - C++ - graphics - rayTracing
subtitle: ' "Ray-Tracing Begins"'
---
## Ray-Tracing Beginner

Starting from today, I'm learning ray-tracing by following Peter Shirley's  Ray Tracing in One Weekend. Welcome to visit my GitHub page to check out my progress.
 
As a math major without any background in C++, I bumped into problem of running the code provided in the chapter one immediately after reading the book for three minutes... There's nowhere to generate a .ppm image file!
 
Finally I figured out how to make it work. Code is provided below.
```C++
#include <iostream>
#include <fstream>

using namespace std;

int main() {
	ofstream ppm;
	ppm.open("image.ppm");
	int nx = 200;
	int ny = 100;
	ppm << "P3\n" << nx << " " << ny << "\n255\n";
	for (int j = ny - 1; j >= 0; j--) {
		for (int i = 0; i < nx; i++) {
			float r = float(i) / float(nx);
			float g = float(j) / float(ny);
			float b = 0.2;
			int ir = int(255.99 * r);
			int ig = int(255.99 * g);
			int ib = int(255.99 * b);
			ppm << ir << " " << ig << " " << ib << "\n";
		}
	}
	return 0;
}
```
---
## Notes for Beginners Like Me
_oftream ppm_ is output file stream type, which means everything after _ppm_(connected with <<) will go into _ppm_ as output.
