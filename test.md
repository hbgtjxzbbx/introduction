
# An Introduction to Fluid-based Image Registration



This article is face to readers who are not familiar with
**fluid** image registration methods, and the object is to let you guys be
interested in this topic :)  
I will try to make the whole story non-painful. As an overview, let’s
see what we are going to talk about.

-   What is the fluid-based image registration

-   Why fluid models

-   Model Gallery

Hmmm, let’s start....

What is the fluid-based image registration
==========================================
Before we start to answer what is fluid-based image registration? 

Let’s first review what image registration is: {\it Image registration is to find a spatial transformation between image pairs, the source image (or moving image) and the target image (or fixed image).}

Smile.gif 
 
A desired transformation can be evaluated by “similarity” and “smoothness” that we often refer to “regularization”. 

There were bunches of registration methods, like rigid, elastic to name a few. For those who are interested in those professional topics, please refers to “numerical methods for image registration” (enjoy your reading :))


Back to fluid based image registration, literally, we view the image registration process as a continuous flow; both images are taken as fluids, and the source image is flow toward to target image Recall that there is a continuous time axis, so the flow is dynamic).


Before step further, let's would introduce some notations to help formulate the fluid registration problem. Let's denote source image as <img alt="$I_0$" src="svgs/88fbd05154e7d6a65883f20e1b18a817.png" align="middle" width="13.778655000000004pt" height="22.46574pt"/>, target image as<img alt="$I_1$" src="svgs/d906cd9791e4b48a3b848558acda5899.png" align="middle" width="13.778655000000004pt" height="22.46574pt"/>. The image coordinate as <img alt="$\bf x$" src="svgs/e90cf27c8829e726a86b08ea364a5ae9.png" align="middle" width="9.977220000000004pt" height="14.61206999999998pt"/> (typically a 2d or 3d vector). To simplify notation, we will not bold <img alt="$\bf x$" src="svgs/e90cf27c8829e726a86b08ea364a5ae9.png" align="middle" width="9.977220000000004pt" height="14.61206999999998pt"/> in the rest of the article.

In fluid-based image registration method, each location typically discreted by image coordinate x has its own velocity at time point t, v(x,t), it can either be temporally variant <img alt="$v(x,t)$" src="svgs/06276a612fb0886861de7866e7c32037.png" align="middle" width="43.980255pt" height="24.65759999999998pt"/> or -invariant <img alt="$v(x,t)=v(x,0)$" src="svgs/35c9efd763f279ffd327d2e85f3d60c8.png" align="middle" width="112.16122500000002pt" height="24.65759999999998pt"/> during the registration. For the image at time point t (here we assume <img alt="$t\in[0,1]$" src="svgs/489dfef0eefc2611fce620116590ce89.png" align="middle" width="58.90401000000001pt" height="24.65759999999998pt"/>), we denote it as <img alt="$I(t)$" src="svgs/d3971921eb7f38e1fe59e8ecc77ce9da.png" align="middle" width="27.237540000000003pt" height="24.65759999999998pt"/>. <img alt="$I(1)$" src="svgs/9c0cfc4ae19b0625d2e3ccc6dcd9d129.png" align="middle" width="29.520645000000002pt" height="24.65759999999998pt"/> refers to the resulting warped image (compared with <img alt="$I_1$" src="svgs/d906cd9791e4b48a3b848558acda5899.png" align="middle" width="13.778655000000004pt" height="22.46574pt"/>, the target image).

Back to the “similarity” and “smoothness”. On one hand, we are try to get an <img alt="$I(1)$" src="svgs/9c0cfc4ae19b0625d2e3ccc6dcd9d129.png" align="middle" width="29.520645000000002pt" height="24.65759999999998pt"/> that close to <img alt="$I_1$" src="svgs/d906cd9791e4b48a3b848558acda5899.png" align="middle" width="13.778655000000004pt" height="22.46574pt"/>. The term "close" depends on the similarity metric. One the other hand, we would like to regularize the flow to be an continuous, both spatially and temporally, flow. So we can define the problem like this 

<p align="center"><img alt="\begin{align}&#10;v^* = \underset{v}{\text{argmin}}~ \frac 12 \int_0^1 \|v(x,t) \|^2_L \mathrm{d} t + \operatorname{Sim}(I(1),I_1),\label{eq:general_obj}&#10;\end{align}" src="svgs/083b44e11db8c157c006291813730941.png" align="middle" width="518.5554pt" height="40.70352pt"/></p>

The <img alt="$L$" src="svgs/ddcb483302ed36a59286424aa5e0be17.png" align="middle" width="11.187330000000003pt" height="22.46574pt"/> here refers the regularizer that encourages the property of the velocity field. So the first term is the regularization term while the second term is the similarity term. The objective is to find the optimal velocity filed <img alt="$v^*(x,t)$" src="svgs/498ea4934ee83767404a7264c980e022.png" align="middle" width="51.53742pt" height="24.65759999999998pt"/>.

So far so good? Now, let's introduce the famous advection equation. We simply the notation by denoting <img alt="$I=I(x,t) $" src="svgs/af2ca1644a2260ed973c86213cda2e6c.png" align="middle" width="74.3721pt" height="24.65759999999998pt"/> and <img alt="$v=v(x,t)$" src="svgs/098d1cb765c1e30bd67ca13586a2c81c.png" align="middle" width="74.455755pt" height="24.65759999999998pt"/>. Besides, <img alt="$\langle \cdot,\cdot\rangle$" src="svgs/dff7ca1cc78224648b56689353df221a.png" align="middle" width="29.22381pt" height="24.65759999999998pt"/> refers to dot product and <img alt="$\nabla$" src="svgs/47c28f1929c18f887420345e9225e08b.png" align="middle" width="13.698795000000004pt" height="22.46574pt"/> refers to spatial gradient.
<p align="center"><img alt="$$\partial_t I + \langle \nabla I, v\rangle=0$$" src="svgs/d60044fb9f3c2b130de8f46b65e4c4ad.png" align="middle" width="124.12273499999999pt" height="16.438356pt"/></p>
It may tell you nothing at the first glance (at least, I felt like that). To those who interests where it froms, here is the link (\url{https://www.youtube.com/watch?v=atvw5iseoGQ}).
A short summary is according to this equation, once we have <img alt="$I(x,t)$" src="svgs/57c8270313ab3f7aae749b7d50ae1a0c.png" align="middle" width="43.93851pt" height="24.65759999999998pt"/> and v(x,t), we can moving the image <img alt="$I(x,t)$" src="svgs/57c8270313ab3f7aae749b7d50ae1a0c.png" align="middle" width="43.93851pt" height="24.65759999999998pt"/> according to <img alt="$v(x,t)$" src="svgs/06276a612fb0886861de7866e7c32037.png" align="middle" width="43.980255pt" height="24.65759999999998pt"/> ( the v here is extactly the same as what you understand in physical way) and get the next time step image by computing <img alt="$I(t+\delta t)= I(t)+\langle \nabla I, v\rangle  \delta t$" src="svgs/6717459201b769133cb11d25bb6a982d.png" align="middle" width="195.16645499999998pt" height="24.65759999999998pt"/>.

Finally, For the typical fluid-based registration, we get the formulation like this
<p align="center"><img alt="\begin{align}&#10;&amp;v^* = \underset{v}{\text{argmin}}~ \frac 12 \int_0^1 \|v(x,t) \|^2_L \mathrm{d} t + \operatorname{Sim}(I(1),I_1),\nonumber \\ &amp;\quad\text{s.t.}\quad\partial_t I + \langle \nabla I, v\rangle=0;\quad I(0)=I_0 \label{eq:general}&#10;\end{align}" src="svgs/a2601a311fd91a879413af3bcf06acff.png" align="middle" width="518.5554pt" height="65.196615pt"/></p>


\section{Why fluid-based models}
So what's the advantage of the fluid-based registration methods compared with other registration methods, i.e. elastic registration methods. Let's denote the transformation map as <img alt="$\varphi$" src="svgs/417a5301693b60807fa658e5ef9f9535.png" align="middle" width="10.753545000000003pt" height="14.155350000000013pt"/> and the warped image can be represented by <img alt="$I_0(\varphi)$" src="svgs/c24f92b8e0a01363987a1f4849c0166f.png" align="middle" width="38.13942pt" height="24.65759999999998pt"/> or <img alt="$I_0\circ\varphi$" src="svgs/096410a70877f9994e18392b7e72c37d.png" align="middle" width="40.878915pt" height="22.46574pt"/>. For the source image, we can define identity map <img alt="$id$" src="svgs/cfcff0bf5660cb069d20c1125652ecd6.png" align="middle" width="14.219205000000004pt" height="22.831379999999992pt"/>, that <img alt="$I_0=I_0\circ id$" src="svgs/2bdac968a502e09094a5812d1bb2c8a3.png" align="middle" width="80.86287pt" height="22.831379999999992pt"/>. One key difference is that the fluid-based method penalized on velocity field, <img alt="$\partial_t \varphi (t) = v(t)\,$" src="svgs/ddbdaf52293f0c51c089902ea006ae3f.png" align="middle" width="93.18688499999999pt" height="24.65759999999998pt"/>, while the elastic registration methods directly penalize on the displacement map <img alt="$\varphi - id$" src="svgs/503171873ae833b0ffb605ac60b1aaa7.png" align="middle" width="45.06381000000001pt" height="22.831379999999992pt"/>. A tiny velocity field can accumlated to large displacement after a large time t, which means the regularization on velocity can be much smaller than penalization on the displacement map. Besides, the velocity field is continous in time, as long as the velocity field is smooth and no-folding the accumulated displacement field is also smooth and inversible, which is much easier to be realized compared with model that penalizes on the displacement map. Let's see an example from[], which illustrate how powerful fluid-based models compared with elastic models (the image refers to ).



\section{Typical models}

Now, let's start explore a family of fluid-based registration.
Here is the full menu.
<p align="center"><img alt="\begin{itemize}&#10;    \item Vector Momentum-parameterized Stationary Velocity Field (vSVF)&#10;    \item Large Displacement Diffeo-morphic Metric Mapping (LDDMM)&#10;    \item Region-specific Diffeomorphic Metric Mapping (RDMM)&#10;\end{itemize}" src="svgs/ee4222a8885de474043a6d22614d7773.png" align="middle" width="502.9233pt" height="82.19178pt"/></p>


One more time, let's recall <img alt="$v(x,t)$" src="svgs/06276a612fb0886861de7866e7c32037.png" align="middle" width="43.980255pt" height="24.65759999999998pt"/> determines the how image <img alt="$I(0)$" src="svgs/b5f0b38e877b2984875a2472f9d06cdc.png" align="middle" width="29.520645000000002pt" height="24.65759999999998pt"/> flows into <img alt="$I(1)$" src="svgs/9c0cfc4ae19b0625d2e3ccc6dcd9d129.png" align="middle" width="29.520645000000002pt" height="24.65759999999998pt"/> and the regularizer <img alt="$L(x,t)$" src="svgs/888d188b2ac2f66ce43be398ac1f1bb2.png" align="middle" width="46.609694999999995pt" height="24.65759999999998pt"/> determines how <img alt="$v(x,t)$" src="svgs/06276a612fb0886861de7866e7c32037.png" align="middle" width="43.980255pt" height="24.65759999999998pt"/> is regularized.

The regularizer <img alt="$L$" src="svgs/ddcb483302ed36a59286424aa5e0be17.png" align="middle" width="11.187330000000003pt" height="22.46574pt"/> is can be anything, like LASSO, Least Square ...
Here we may skip some awsome math<img alt="$^*$" src="svgs/cdcac8939f3840cd8cddf40059a4cf58.png" align="middle" width="6.735267000000005pt" height="22.638659999999973pt"/> (\href{http://campar.in.tum.de/twiki/pub/DefRegTutorial/WebHome/MICCAI_2010_Tutorial_Def_Reg_Darko.pdf}{link}). We introduce momentum m, and velocity field can be smoothed from <img alt="$v=G * m $" src="svgs/765024b8b78ff03a1ed8d30b6d97265c.png" align="middle" width="73.358175pt" height="22.46574pt"/>, where <img alt="$G$" src="svgs/5201385589993766eea584cd3aa6fa13.png" align="middle" width="12.924780000000005pt" height="22.46574pt"/> is gaussian smoother.

{\it <img alt="$*$" src="svgs/7c74eeb32158ff7c4f67d191b95450fb.png" align="middle" width="8.219277000000005pt" height="15.297149999999977pt"/>(Optional) The key idea is to get a velocity field v, which can be define in Sobolev spaces, where <img alt="$\mathrm{H}^{s} \equiv\left\{v:\|v\|_{\mathrm{H}^{s}}&lt;\infty\right\}$" src="svgs/8c8efe47575d37767aa9b25567763bfb.png" align="middle" width="160.17490500000002pt" height="24.65759999999998pt"/> with <img alt="$\langle u, h\rangle_{\mathrm{H}^{s}}=\sum_{k=0}^{s}\left\langle\mathrm{D}^{k} u, \mathrm{D}^{k} h\right\rangle_{\mathrm{L}^{2}}$" src="svgs/c6643ffef016ee743499186e96085953.png" align="middle" width="219.29275499999997pt" height="27.94572000000001pt"/>, meaning sum of any order derivatives on <img alt="$v$" src="svgs/6c4adbc36120d62b98deef2a20d5d303.png" align="middle" width="8.557890000000002pt" height="14.155350000000013pt"/> are bounded.  <img alt="$\|v\|^2_L=\langle L^\dagger L v,v\rangle$" src="svgs/20fd010b49857b432588cd6a083f8f06.png" align="middle" width="123.18504pt" height="27.91271999999999pt"/>, we introduce <img alt="$m=L^\dagger L v$" src="svgs/acc60acf5c71d52a6a1a209d5a773ee8.png" align="middle" width="74.13252pt" height="27.91271999999999pt"/> or <img alt="$v=\left(L^{\dagger} L\right)^{-1} m$" src="svgs/784704808425c37c1d54258adb605fb8.png" align="middle" width="109.58920499999999pt" height="34.64868pt"/>. According Green's function, assume <img alt="$v$" src="svgs/6c4adbc36120d62b98deef2a20d5d303.png" align="middle" width="8.557890000000002pt" height="14.155350000000013pt"/> is in Sobolev spaces and <img alt="$s=\infty$" src="svgs/3fc3d2f9800a53693017630d13986894.png" align="middle" width="46.061564999999995pt" height="14.155350000000013pt"/>, it is equivalent to get <img alt="$v=\left(L^{\dagger} L\right)^{-1} m =G * m $" src="svgs/9115e850f285d0038b217e2d3f6ee9c9.png" align="middle" width="174.38965499999998pt" height="34.64868pt"/>}

The advantage of introducing momentum <img alt="$m$" src="svgs/0e51a2dede42189d77627c4d742822c3.png" align="middle" width="14.433210000000003pt" height="14.155350000000013pt"/> is that we can optimize over momentum <img alt="$m$" src="svgs/0e51a2dede42189d77627c4d742822c3.png" align="middle" width="14.433210000000003pt" height="14.155350000000013pt"/>, even if the optimal m is not smooth, we can get an smooth <img alt="$v$" src="svgs/6c4adbc36120d62b98deef2a20d5d303.png" align="middle" width="8.557890000000002pt" height="14.155350000000013pt"/> by applying gaussian smoother. In practice, the multi-gaussian smoother allows multi-scale regularier and can get better registration results.

mtov.png

Before, we get into each method, let's first see how these methods related.

tree.png




\subsection{Vector Momentum-parameterized Stationary Velocity Field (vSVF)}

The relative simple model is Momentum-parameterized Stationary Velocity Field (vSVF), where it assume the velocity field <img alt="$v$" src="svgs/6c4adbc36120d62b98deef2a20d5d303.png" align="middle" width="8.557890000000002pt" height="14.155350000000013pt"/> is temporal invariant, which refers the momentum <img alt="$m$" src="svgs/0e51a2dede42189d77627c4d742822c3.png" align="middle" width="14.433210000000003pt" height="14.155350000000013pt"/> is also temporal invariant. Besides the regularizer(or you can just view it as a gaussian smoother) is fixed or spatio-temporal invariant.

See how vSVF performs:


The following part is optional:

We have <img alt="$v(x,t)=v(x,0)$" src="svgs/35c9efd763f279ffd327d2e85f3d60c8.png" align="middle" width="112.16122500000002pt" height="24.65759999999998pt"/>, and <img alt="$L(x,t)=const$" src="svgs/7895a303868a529d790e6580ebfcd0c1.png" align="middle" width="107.11767pt" height="24.65759999999998pt"/>, so <img alt="$\int_0^1 \|v(x,t) \|^2_L \mathrm{d} t = |v(x,0) \|^2_L = \langle m(x,0),v(x,0)\rangle$" src="svgs/19e788218549bb2c4304beb50c8a997a.png" align="middle" width="337.617555pt" height="33.18744000000001pt"/> the Eq. \ref{eq:general} turns into
<p align="center"><img alt="\begin{align}&#10;&amp;m(x,0)^* = \underset{v}{\text{argmin}}~ \frac 12 \langle m(x,0),v(x,0)\rangle  + {Sim}(I(1),I_1),\nonumber \\ &amp;\quad\text{s.t.}\quad\partial_t I + \langle \nabla I, v(x,0)\rangle=0;\quad I(0)=I_0\ .\label{eq:vsvf}&#10;\end{align}" src="svgs/0a206de864594f3ff901a06ef5aae66b.png" align="middle" width="544.14525pt" height="60.910575pt"/></p>

The objective is to search optimal <img alt="$m(x,0)$" src="svgs/a06f78b3f5f7a2bdec32c685e6510e48.png" align="middle" width="52.13868pt" height="24.65759999999998pt"/>, which determines how the image flows.


\subsection{Large Displacement Diffeo-morphic Metric Mapping (LDDMM)}

Maybe you have noticed the limits of the vSVF, since the v(x,t) is fixed along time axes, which can be a large constrain. So let's remove it, and then comes famous Large Displacement Diffeo-morphic Metric Mapping (LDDMM). In LDDMM, the v(x,t)= v(x,t)! However, the regularizier (or you can just view it as a gaussian smoother), like vSVF, is fixed.


See how LDDMM performs:



The following part is optional:

We have <img alt="$v(x,t)$" src="svgs/06276a612fb0886861de7866e7c32037.png" align="middle" width="43.980255pt" height="24.65759999999998pt"/>, and <img alt="$L(x,t)=const$" src="svgs/7895a303868a529d790e6580ebfcd0c1.png" align="middle" width="107.11767pt" height="24.65759999999998pt"/>, but it can be proven, in optimal condition (beyond this introduction), the energy is conserved, <img alt="$\int_0^1 \|v(x,t) \|^2_L \mathrm{d} t = |v(x,0) \|^2_L = \langle m(x,0),v(x,0)\rangle$" src="svgs/19e788218549bb2c4304beb50c8a997a.png" align="middle" width="337.617555pt" height="33.18744000000001pt"/>. The Eq. \ref{eq:general} turns into
<p align="center"><img alt="\begin{align}&#10;&amp;m(x,0)^* = \underset{v}{\text{argmin}}~ \frac 12 \langle m(x,0),v(x,0)\rangle  + {Sim}(I(1),I_1),\nonumber \\ &amp;\quad\text{s.t.}\quad\partial_t I + \langle \nabla I, v(x,t)\rangle=0;\quad I(0)=I_0\ .\label{eq:lddmm}&#10;\end{align}" src="svgs/95b72ffbfea7909d49be1ed55c98c8a5.png" align="middle" width="544.14525pt" height="60.910575pt"/></p>
The objective is to search optimal <img alt="$m(x,0)$" src="svgs/a06f78b3f5f7a2bdec32c685e6510e48.png" align="middle" width="52.13868pt" height="24.65759999999998pt"/>, which determines how mometum <img alt="$m(x,t)$" src="svgs/b85a4c4954041bc2d83cf66528eddfa7.png" align="middle" width="49.85557500000001pt" height="24.65759999999998pt"/> evolves, since the velocity is smoothed from <img alt="$m$" src="svgs/0e51a2dede42189d77627c4d742822c3.png" align="middle" width="14.433210000000003pt" height="14.155350000000013pt"/>, it indirectly determines how the image flows.



\subsection{Region-specific Diffeomorphic Metric Mapping (RDMM)}

At this point, you may already predict the improvement in RDMM (if you ignore the family tree above.):)
<img alt="$L(x,t)=L(x,t)$" src="svgs/b90f8d3b03721e22cd2087be5519c663.png" align="middle" width="115.137pt" height="24.65759999999998pt"/>! Every parameter own its freedom.
And yes, RDMM is a generalization on the LDDMM. 
In RDMM, we give the <img alt="$L$" src="svgs/ddcb483302ed36a59286424aa5e0be17.png" align="middle" width="11.187330000000003pt" height="22.46574pt"/> full freedom. But we add a reasonable assumption, the <img alt="$L(x,t)$" src="svgs/888d188b2ac2f66ce43be398ac1f1bb2.png" align="middle" width="46.609694999999995pt" height="24.65759999999998pt"/> flows with the image,<img alt="$I(t)$" src="svgs/d3971921eb7f38e1fe59e8ecc77ce9da.png" align="middle" width="27.237540000000003pt" height="24.65759999999998pt"/>,  So what's the influence? Region-specific regularizer, which means each region (pixel level) has its own regularizer during the registration. The regularizer tracks how image moves. If at <img alt="$t=0$" src="svgs/1c899e1c767eb4eac89facb5d1f2cb0d.png" align="middle" width="36.07296pt" height="21.18732pt"/>, the regularizer of a region is large, then this region will not experience crazy deformation, vice versa.
So let's see how RDMM with a pre-defined regularizer peforms.


But since RDMM is a generalization of LDDMM, can it perform regular task? Of course, see the following task with an optimized regularizer.



The following part is optional:

First, how the regularizer <img alt="$L(x,t)$" src="svgs/888d188b2ac2f66ce43be398ac1f1bb2.png" align="middle" width="46.609694999999995pt" height="24.65759999999998pt"/> tracks the image <img alt="$I(t)$" src="svgs/d3971921eb7f38e1fe59e8ecc77ce9da.png" align="middle" width="27.237540000000003pt" height="24.65759999999998pt"/>?
The advevtion equation. The same idea on advect the image. 
<p align="center"><img alt="\begin{align}&#10;\partial_t L + \langle \nabla L,v\rangle=0^* \label{eq:adv_reg}&#10;\end{align}" src="svgs/066cbb2792ffb352b98a0fcbc0ab15c6.png" align="middle" width="418.6479pt" height="16.438356pt"/></p>

{\it <img alt="$*$" src="svgs/7c74eeb32158ff7c4f67d191b95450fb.png" align="middle" width="8.219277000000005pt" height="15.297149999999977pt"/> The regularizer <img alt="$L$" src="svgs/ddcb483302ed36a59286424aa5e0be17.png" align="middle" width="11.187330000000003pt" height="22.46574pt"/> is a multi-gaussian smoother, so in practice, the parameters of the smoother is advected. To ensure the spatial smoothness of the mutli-gaussian smoother(since each location has its own smoother with different parameters), a pre-parameter (similar idea like introducing momentum) is introduced. All these are beyond this introduction, for those who are interests, please refer to to\href{https://arxiv.org/pdf/1906.00139.pdf}{RDMM}}

It can be proven, in optimal condition (beyond this introduction) of a specific formulation between <img alt="$v$" src="svgs/6c4adbc36120d62b98deef2a20d5d303.png" align="middle" width="8.557890000000002pt" height="14.155350000000013pt"/> and <img alt="$L$" src="svgs/ddcb483302ed36a59286424aa5e0be17.png" align="middle" width="11.187330000000003pt" height="22.46574pt"/> can conserve energy of term <img alt="$\int_0^1 \|v(x,t) \|^2_{L(x,t)} \mathrm{d} t$" src="svgs/809d75df2bbf6481fd92d07af83fc256.png" align="middle" width="132.99874499999999pt" height="33.18744000000001pt"/>, <img alt="$\int_0^1 \|v(x,t) \|^2_{L(x,t)} \mathrm{d} t = |v(x,0) \|^2_{L(x,0)} = \langle m(x,0),v(x,0)\rangle$" src="svgs/363784c5ef334f8c524234d183e21003.png" align="middle" width="392.400855pt" height="33.18744000000001pt"/>. The Eq. \ref{eq:general} turns into
<p align="center"><img alt="\begin{align}&#10;&amp;m(0)^{*},L(x,0)^{*}=\underset{m(0),L(x,0)}{\operatorname{argmin}} \frac{1}{2}\langle m(x,0),v(x,0)\rangle+\operatorname{sim}\left(I(1), I_{1}\right)+\operatorname{Reg}\left(L(x,0)\right)\nonumber \\&#10;\quad\text{s.t.}&amp;\quad\partial_t I + \langle \nabla I, v)\rangle=0, \quad\partial_t L + \langle \nabla L,v\rangle=0, \quad I(0)=I_0\ .\label{eq:rdmm}&#10;\end{align}" src="svgs/b5b553f5260fe085166360afc5510be9.png" align="middle" width="629.4618pt" height="65.29413pt"/></p>
The objective is to search optimal <img alt="$m(x,0)$" src="svgs/a06f78b3f5f7a2bdec32c685e6510e48.png" align="middle" width="52.13868pt" height="24.65759999999998pt"/> and <img alt="$L(x,0)$" src="svgs/a33a6b8593b7156f069bfb2f86d74b04.png" align="middle" width="48.89280000000001pt" height="24.65759999999998pt"/>, which determines how mometum <img alt="$m(x,t)$" src="svgs/b85a4c4954041bc2d83cf66528eddfa7.png" align="middle" width="49.85557500000001pt" height="24.65759999999998pt"/> and the regularizer <img alt="$L(x,t)$" src="svgs/888d188b2ac2f66ce43be398ac1f1bb2.png" align="middle" width="46.609694999999995pt" height="24.65759999999998pt"/> evolve, since the velocity is smoothed from <img alt="$m$" src="svgs/0e51a2dede42189d77627c4d742822c3.png" align="middle" width="14.433210000000003pt" height="14.155350000000013pt"/> by <img alt="$L(x,t)$" src="svgs/888d188b2ac2f66ce43be398ac1f1bb2.png" align="middle" width="46.609694999999995pt" height="24.65759999999998pt"/>, it indirectly determines how the image flows.

