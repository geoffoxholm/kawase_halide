## Introduction

The Kawase filter is a blur filter introduced by Masaki Kawase during
the Game Developers Conference (GDC) in 2003. It has been used in games
as a fast approximation to effects such as bloom, photographic light
streaks, depth-of-field, lens flare and ghosting, etc, in a time when
pixel shaders were very primitive and full of restrictions<sup>[1](#footnotes)</sup>.

The original presentation is available online at the author's website: ["*Frame Buffer Postprocessing Effects in DOUBLE-S.T.E.A.L (Wreckless)*"][1]

Kawase filters can achieve smooth and wide image blurs. Conceptually, it
is a multi-pass filter (i.e., the output of one pass is the input to the
subsequent pass), where in each pass four blocks of 2x2 pixels are
blended together. The separation between blocks may vary across passes.

[This article][2] from Intel's Developer Zone provides more
intuition.


## Problem

Formally, we can denote an *n*-pass Kawase filter with a sequence of
numbers *K* = {*d<sub>1</sub>*, *d<sub>2</sub>*,..., *d<sub>n</sub>*}, such that *d<sub>i</sub>* denotes the
distance between the 2x2 blocks in pass *i*.

Under certain circumstances, it is possible to obtain good
approximations to Gaussian filters. For example, by downsampling an
image to ½ x ½ of its resolution, then applying the Kawase filter K = {
0, 1, 2, 2, 3 }, and finally up-sampling the image back to its original
resolution, the result is comparable to a 35 pixel radius Gaussian
filter (with tail &sigma; = 3) over the original resolution image (see
the images below for comparison).

Your task is to verify such claim by implementing the Kawase filter K =
{ 0, 1, 2, 2, 3 } in Halide and comparing the resulting blur to the
aforementioned Gaussian blur. You are free to implement both the
algorithm and scheduler portions in any way you see appropriate,
including how to handle out-of-bounds sampling. Halide implementations
for image downsampling, upsampling, Gaussian filtering or generalized
Kawase filters (i.e., arbitrary number of passes and distances) are not
required.

## Closing Remarks

You may browse for sample images [here][3].

If there are any interesting things you'd like to draw our attention to
in your solution, please say so in a comment or in some documentation.
Also, if you consult any resources to help you out (online or
otherwise), please let us know what they were.

If possible please get back to us with the code sample within a week.

Send us a link to your code on Dropbox, Google Drive, or OneDrive --
***do not*** send an e-mail with a ***.zip*** attachment since it won't
get through Adobe's email filtering system!

## Expected Results

Original image (full resolution):
![Original][original]

Kawase filter result (applied to down-sampled image, then upscaled back
to original size):

K = { 0, 1, 2, 2, 3 }

![kawase-upscaled][upscaled]

Gaussian filter result (radius of 35 pixels, with tail &sigma; = 3)
applied to the full resolution image:

![gauss-r35.jpg][gauss]

---

### Footnotes

1. In the context of graphics hardware, blocks of 2x2 pixels can be
    (bi-linearly) filtered with just a single hardware-accelerated
    texture sampling instruction. The cost per-pixel on each pass is
    constant: four texture sampling instructions. Texture access is also
    quite coherent since pixels are rasterized and shaded together.
    Memory bandwidth can be reduced by down-sampling the image prior to
    applying the filter. Since the filter acts as a low-pass filter
    aimed at interactive frame rates, the lower resolution still
    provides acceptable image quality.


[1]: http://www.daionet.gr.jp/~masa/archives/GDC2003_DSTEAL.ppt
[2]: https://software.intel.com/en-us/blogs/2014/07/15/an-investigation-of-fast-real-time-gpu-based-image-blur-algorithms
[3]: https://www.pexels.com/public-domain-images/

[original]: media/walnuts.jpg
[upscaled]: media/kawase-upscaled.jpg
[gauss]: media/gauss-r35.jpg