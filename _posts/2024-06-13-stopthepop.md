---
layout: paperpost
section-type: post
title: "StopThePop: Sorted Gaussian Splatting for View-Consistent Real-time Rendering"
authors: "Lukas Radl, Michael Steiner, Mathias Parger, Alexander Weinrauch, Bernhard Kerbl, Markus Steinberger"
venue: Arxiv
category: paper
links: {
    Paper: "https://arxiv.org/abs/2402.05919",
    Website: "https://r4dl.github.io/StopThePop/",
    Github: "https://github.com/r4dl/StopThePop"
}
teaser-img: "/img/stopthepop.png"
tags: ["3DGS", "3d reconstruction", "machine learning"]
---

StopThePop was by far the most focused paper I have been involved in. From concept to submission, it took us only about month to finish this paper - and it was accepted at Siggraph 2024 :-)
The idea was clear: we want to add per-pixel sorting of Gaussians for better view-consistency. 

For context: The original 3D Gaussian Splatting implementation suffers from view-consistency on camera rotation due to a combination of simplifications they use to increase the frame rate during inference. 
While the effect can be subtle for some scenes, it is clearly noticeable on shiny or reflective surfaces.
However, implementing a full per-pixel sorting of all Gaussians would result in huge run-time costs which might not be worth the gain in image quality.

Markus contacted me in December if I was interested in working on a paper to submit to Siggraph by the end of January.
Since I was in-between jobs at that time, I had some time to spare and I was really excited to do something with 3DGS.

StopThePop is one of those rare papers that are a perfect fit for our team - we have plenty of NeRF experience, and we are CUDA-enthusiasts. Per-pixel sorting is very expensive, and can only be done with extensive CUDA kernel optimization.

But even with a fast sorting implementation, we were still quite a bit slower than the original approach. So, we decided to spend some time to accelerate other parts of the rendering pipeline as well. For example, by implementing a better culling approach, we can save a lot of time in the subsequent parts of the pipeline.
We showed that with a few simple optimizations, we can achieve similar performance with much better view consistency.

I am looking forward to seeing how people are going to use StopThePop - if it finds use in Virtual Reality, or if it could even help reconstructing better surfaces with more consistent normals, or for inverse rendering. Time will tell.