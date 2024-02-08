---
layout: paperpost
section-type: post
title: "MotionDeltaCNN: Sparse CNN Inference of Frame Differences in Moving Camera Videos with Spherical Buffers and Padded Convolutions"
authors: "Mathias Parger, Chengcheng Tang, Thomas Neff, Christopher D Twigg, Cem Keskin, Robert Wang, Markus Steinberger"
venue: ICCV
category: paper
links: {
    Paper: "http://openaccess.thecvf.com/content/ICCV2023/papers/Parger_MotionDeltaCNN_Sparse_CNN_Inference_of_Frame_Differences_in_Moving_Camera_ICCV_2023_paper.pdf",
}
tags: ["CNN", "machine learning", "CUDA", "CV", "Meta Reality Labs"]
---

<img src="/img/motiondeltacnn.jpg"/>

MotionDeltaCNN is a follow-up paper to <a href="/paper/2022/06/16/deltacnn.html">DeltaCNN</a> and the last of the three research collaborations I did with Meta Reality Labs. This is probably a good time to thank them for the great time, and for enabling me to do this research. Special thanks go to Chengcheng Tang: thanks for all the help, it was a blast!

One of the main limitations of DeltaCNN is the fact that it requires static cameras to achieve sparse inputs, and to therefore speedup inference.
Even small camera motion, like a shift of a single pixel from one frame to another might already require near dense updates when the background has high frequency features.

In MotionDeltaCNN, we extend DeltaCNN to support moving cameras.
The concept is easy to understand, but difficult to implement in practice: if we know the camera motion between two frames, we can calculate the distance between the pixels in previous frames to the pixels in the current frame.
In practice, this gets a bit more difficult, because the pixel distance also depends on the depth of the pixel.
Also, how do you store the previous input and outputs when you have constant motion?

MotionDeltaCNN solves this by using the homography between two images - which can be calculated offline or using some internal sensors.
With this matrix, we can align the new frame onto the previous frame.
In overlapping regions, we subtract the previous input from the current to increase sparsity, and the remaining pixels of the new frame are processed densely as "full" values.

But this results in many problems: how do you correctly convolve newly added pixels if the input to it is a mix of "full" and "Delta" values. And how do you store the new regions? Allocate new memory? Over-allocate the buffers in the first place to allow for large motion?
How to handle parallax?
This paper ended up being much more complex than I initially thought, but I am very happy it made it into ICCV as a result.
If you want to know how all that was solved, please take a look at the paper and the video.

<div class="paper-info-header">
    <iframe width="80%" height="auto" class="paper-video" src="https://www.youtube.com/embed/Q1MtBUcgdPA?si=Gbi1FpkfP1Ovsvpl"  title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>