---
layout: post
title: A good thing's worth waiting for - new paper in JCP!
date: 2023-04-17 12:00:00+01:00
inline: false
giscus_comments: true
tags: photocatalysis nanorods semiconductor spectroscopy
related_publications: true
---

I am happy to announce that our paper *Lateral charge migration in 1D semiconductor–metal hybrid photocatalytic systems* has been published in Journal of Chemical Physics as part of the *40 Years of Colloidal Nanocrystals in JCP* special issue! You can check it out [here](https://aip.scitation.org/doi/10.1063/5.0144785)!

When I started my postdoc in the group of Maria Wächtler, one of the first measurements I performed was measuring a series of CdSe@CdS nanorods of different lengths. Sounds easy enough to analyze, right? Well, wrong. That was in Winter 2018. Now, Spring 2023, I am happy to show the result of our in-depth analysis!7

{% include figure.liquid path="assets/img/micheel_lateral_2023.png" class="img-fluid rounded z-depth-1" %}

CdSe@CdS nanorods are great photosensitizers for artificial photosynthesis, if coupled to a catalytic center (e.g. metal nanoparticles). When they are photoexcited, you can basically see three different electron transfer pathways: one very fast crossing from the nanorod to the metal particle (< 1 ps); one slower one (~ 10 ps); and a very slow one (< 100 ps) originating from electrons localized near the CdSe core. But how does each of these steps depend on the nanorod dimensions - and what would be the implications for photocatalysis? This is what we investigated, by looking at nanorods of different length (20 - 60 nm), decorated with Ni nanoparticles. Essentially, we found three important implications:

1. The very fast electron transfer step relates to interfacial electron transfer. Photoexcitation of the nanorod generates an exciton, i.e., a localized electron-hole pair, which dissociates within few ps. We find an even faster electron transfer step, in line with previous reports in similar systems. However, we found that with increasing rod length, this step contributes relatively less to the overall electron transfer. This makes immediate sense: only if we generate the exciton close to the metal tip, the interfacial transfer may occur with high probability. In longer rods, the chance for generating the exciton far away from the tip increases. So for an ideal photocatalytic system, we might expect short rods to perform great, since we harvest many electrons from this fast electron step - but in reality, short CdSe@CdS rods perform much worse than long ones. So a different effect may play a role!

2. The second electron transfer step is electron diffusion limited. After the exciton dissociates into hole (which localizes into the CdSe core) and electron (which is delocalized over the entire rod), the electron slowly diffuses to the tip. In fact, we found a perfect match of our measured kinetics and the expected diffusion rates. But even though this electron transfer is much slower than the interfacial fast transfer, it is still much faster than any other competing process like electron trapping (> 100 ps) or the electron-hole recombination (> 10 ns). On the other hand, a longer rod increases the chances of photon absorption. An ideal CdSe@CdS photocatalytic system should thus be as ong as possible - and from literature reports, we know that 70 nm rods perform very well. What's the maximum length at which we expect best behavior? We have no idea, but expect it to be in the order of 100 nm. This is interesting, because most other semiconductor nanoparticle photocatalysts (e.g. pure rods) perform better at much smaller scales (~20-30 nm). So for any economical comparison of such systems, we'd need to take into account that they perform differently depending on their length!

3. The slowest electron transfer step relates to the transfer of an electron who is "localized" more around the CdSe core, due to the Coulombic interaction of the electron's negative and the hole's positive charge. Surprisingly, we found no length dependence of this transfer, even though this should be the farthest point from the tip in the rod. We rationalized this on the basis of a shallow confinement in our quasi-Type-II structures of the electron in the core. This puts a thermodynamic barrier to the transfer step, which needs to be overcome and is the rate-limiting process. It would be interesting to see an experiment of CdSe@CdS dot-in-rod systems with different lengths *and* different core sizes to see how this influences this behavior.

I'm really glad this is finally out with a satisfying analysis of the data. On the other hand, I'm a bit sad because this is probably more or less my farewell from academia. At least it's a good one!

## related publication
<div class="publications">
  {% bibliography -f papers -q @*[key=micheel_lateral_2023]* %}
</div>
