In this notebook we present some insights about the simulation of the **lithographic process**. This document is not intended to provide a complete description of the mechanisms at stake when dealing with lithography but rather give some elements **about what's going on and how to simulate it.**

Complete description of **lithography simulation** can be found in numerous textbooks (see References below) but **numerical implementation** is usually not describe thoroughly. The Jupyter NoteBook is a perfect support to share the numerical aspects relative to the simulation of lithography. We will limit our development to *scalar models(标量模型)* for imaging, valid for numerical aperture up to 0.5 ~ 0.6. Such models are not suited to cutting edge lithography equipments, though they are of paramount importance to understand more complex systems. With time, the vectorial aspect of light could be added/discussed within the framework of this notebook.
>标量模型, NA最多0.5~0.6

For it is now the most commonly used language we will use Python, the codes are actually running on Python 3.6

Reader of this notebook are assumed to possess fundamental knowledge about lithography. For those who don't, reading the references at the bottom of this paragraph is strictly recommend but we propose here a very short reminder.

The Figure below describes the most important elements inside a photolithography tool, namely, the source **S**, optical lenses **(Lc0, Lc1, L1, L2)**, pupils **(P1, P2)**, photomask **(M)** and substrate.
![[support/img/Pasted image 20240604225936.png|408]]
Optical lithography is a process by which patterns written on a physical support (**a photomask**) are optically transferred to a substrate covered with a polymer: a **photoresist**. Classicaly, optical lithography is composed of two parts : **==the exposition of the photoresist and its developement==**. In its simplest form the photomask is composed of areas of constant transmission that are opaque or transparent to an illumination. Patterns on the photomask are ==**in the order of**== the wavelentgh of illumination, thus optical diffraction happens. A projection lens recombines the figure of diffraction of the mask, turns it back into an image and projects it on the substrate ==applying a magnification== at the same time (patterns on the mask are 4 or 5 times larger than the image projected on the substrate. *The image received by the substrate of the photomask is called the aerial image*. Because of diffraction and aberration effects in the projection optics this image is not a perfect replication of the mask patterns.

The notebook starts with a presentation of the aerial image computation. Two approachs will be presented: ==the Abbe and Hopkins integration schemes.== Numerical implementation of the two approachs will be given and assessed on classic mask test cases.

The second part of the notebook will adress the simulation of the exposition: h==ow the aerial image propagates into the photoresist and what chemical changes are happening==. Finally, the simulation of development, the process by which the photoresist is selectively removed from the substrate, will be described. For this notebook only **==i-line==** resists will be considered.
## Aerial image formulation
There is a lot to say about the theory of imaging, but in this notebook we will directly go to the imaging equation that describe the optical aspect of photolithography. In the framework of Fourier optics and scalar diffraction theory the intensity received by a substrate is written as :$$I(x,y)=\int\cdots\int_\infty^{-\infty}\tilde{J}(f,g)\tilde{H}(f+f^{\prime},g+g^{\prime})\tilde{H}^*(f+f^{\prime\prime},g+g^{\prime\prime})\tilde{O}(f^{\prime},g^{\prime})\tilde{O}(f^{\prime\prime},g^{\prime\prime})e^{-2i\pi[(f^{\prime}-f^{\prime\prime})x+(g^{\prime}-g^{\prime\prime})y]}dfdgdf^{\prime}dg^{\prime}df^{\prime\prime}dg^{\prime\prime}$$
- $\tilde{J}(f,g)$ is the **==effective source==**, it represents the spectrum of plane waves incident on the mask.
- $\tilde{H}(f,g)$ is the projection lens transfer function, it represents the frequency cutoff applied by the projection lens aperture to the mask spectrum but also the lens aberrations.
- $\tilde{O}(f,g)=\mathcal{F}[O]$ is the mask Fourier transform (also called mask spectrum)

The-notation means that the function is expressed in the frequency domain. Two numerical implentations are possible to compute this equation: **the Abbe and Hopkins approach.** Before giving the numerical implementation each function of the imaging equation will be described.
> FHO分别表示光源, 透镜和掩膜
### Mask $𝑂(𝑥,𝑦)$ and Mask Fourier Transform 𝑂~(𝑓,𝑔) definitions
Both Abbe and Hopkins methods require the computation of the mask Fourier
Transform: $\tilde{O}(f,g).$
$$\mathcal{F}[O]:f,g\mapsto\tilde{O}(f,g)=\iint_\infty^{-\infty}O(x,y)e^{-2i\pi(xf+gy)}dxdy$$
For the sake of simplicity and to make computation faster we are going to consider a one
dimensional mask $O(x).$
$$\mathcal{F}[O]:f\mapsto\tilde{O}(f)=\int_\infty^{-\infty}O(x)e^{-2i\pi xf}dx$$
We start by a simple example: a mask made of a quartz hole centered on the $x$ axis. The
mask function $O(x)$ correspond to its optical transmission. For chrome on glass photomask, this function is binary. The glass does not block incident light, the transmission is 1. Chromium blocks light, its transmission is 0.
> 都需要计算F[O], 使用的是1D, 01掩膜, 形状就是个中央小孔


![[support/img/Pasted image 20240606082724.png|317]]
From there we compute the Fourier Transform of the mask.
With numeric Fourier Transform one has to use the **==shift function==** to recenter the low frequency at the center for both spectrum and **==support==**. With also compute the analytic Fourier Transform of the mask: the Fourier Transform of a hole is a sinc function (Careful for normalisation, do not forget pixel size !).![[support/img/Pasted image 20240606083000.png|313]]

## Source definition
The source (S) illuminates the mask (M) at different angles of incidence depending on its shape. In lithography, the source is imaged by a collection lens (Lc0) at the entrance pupil (P1) of a another lens (L1). The optical configuration used is called *Köhler illumination* and it is illustrated on the Figure below. This configuration ensures that a source point creates a set of plane waves uniformly illuminating the mask at a specific angle relative to the optical axis.
> 保证了入射到mask上的光是有一定角度的平行光

 The source point on the optical axis (green point) creates a set of plane waves parallel to the optical axis. As the source points moves further from the optical axis the angle of incidence on the mask increases. The Köhler illumination ensures the uniform illumination of the mask: ==**each point of the mask receives the same amount/directional light.**==
 ![[support/img/Pasted image 20240606083421.png|316]]• S: source 
 • Lc0: collection lens 0 
 • P1: entrance pupil of L1
 • L1: lens 1 
 • M: mask plane
 Different shapes of source exist, the most simple one is the circular source, defined by
its radius $\sigma$ that ==**corresponds to the source coherence**==. The source coherence in lithography shouldn't be mistaken with the general meaning of coherence in optics. In lithography, all source points are considered to be incoherent with each other from a spatial point of view. They do not have any phase relationship with each other. However the source is coherent from a temporal point of view since it is monochromatic. **This consideration is the starting point of the Abbe theory: the overall image can be**
**computed as the incoherent sum of each source point contribution.**
> 半径是$\sigma$, 对应了光源的相干性
> 在光刻中, 所有的source是空间相干而时间不相干的
> Abbe计算;依据不相干进行.

Depending on the $\sigma$ value the source is said to be:

- Coherent if $\sigma=0:$there is only one source point that is spatially coherent with itself
-  Incoherent if $\sigma=\inf:$infinity of source points no spatial coherence.
- Partially coherent if $0<\sigma<\inf$
> [!question] 相干性和$\sigma$

In pratice all sources used in lithography are partially coherent. But many textbooks use
a coherent source to present fundamental properties of imaging before moving to partially coherent source.
If we consider a circular source of coherence $\sigma$ the image of the source through the optical system is also circular, it is a circle of radius $\frac{\sigma NA}\lambda.$ In a photolithography tool the image of the source is projected at the entrance pupil of the projection lens (P2). The image of the source at the entrance pupil of the projection lens is called the *effective source*.lt is not the Fourier Transform of the source!It is an image of the source that is projected on the same plane as the mask spectrum. Thus, it is convenient
to express it on a frequency support.
The Figure below continues the ray tracing of the previous Figure in absence of a mask.
The effective source is located at the P2 plane.![[support/img/Pasted image 20240606084636.png|362]]
At this point we have all the source points that contribute to the formation of the aerial
image both in direct and frequency domain.

The angle of incidence of the plane wave on the mask is extremely important. When
illuminating an object with light parallel to the optical axis, the figure of diffraction of the
object is formed at infinity (Fraunhofer diffraction regime) centered on optical axis.
However, it is possible to bring this figure of diffraction to a finite distance by using a
converging lens just after the object. This is exatcly what is happening in optical
ithography. The source illuminates the mask and the (Lc1) lens brings back the infinite figure of diffraction at the entrance pupil of the projection lens. That is the reason why the effective source and the mask spectrum are expressed on the same plane (same frequency support).
If the optical system is supposed to be invariant by translation: an illumination that is not
oarallel to the optical axis results in a shift of the figure of diffraction without changing
the amplitude of the diffraction orders (this assumption stands for low NA projection
lens and the absence of polarisation). The frequency shift $\Delta f$ associated to an oblique
illumination $\theta$ can be compute with this formula :
$$\Delta f=\frac{\sin(\theta)}\lambda $$
Since the source effective is expressed in frequency space, the frequency shift directly
correspond to the frequency location of the source point.