---
layout: post
title: New paper in ACS Appl. Nano Mater. on polydopamine-coated nanorods
date: 2024-06-25 09:00:00+02:00
inline: false
giscus_comments: true
tags: photocatalysis nanorods semiconductor spectroscopy
related_publications: true
---

Another work from my postdoc phase in the Wächtler group just got published: [Screening Cobalt-based Catalysts on Multicomponent CdSe@CdS Nanorods for Photocatalytic Hydrogen Evolution in Aqueous Media](https://pubs.acs.org/doi/10.1021/acsanm.4c01645) was published in ACS Applied Nanomaterials.

{% include figure.liquid path="assets/img/boecker_screening_2024-toc.jpg" class="img-fluid rounded z-depth-1" %}

A bit of a follow-up to our 2021 publication, we once again looked at a ternary semiconductor nanoparticle:polydopamine:molecular catalyst system. The semiconductor (in our case, a cadmium selenide/sulfide based nanord) serves as a photosensitizer that absorbs light and generates charges for catalysis. The polydopamine shell serves a) as a charge relay that transfers the charges from the nanorod to the catalyst and b) as a scaffold for functionalization. As catalyst, we once again used Rhodium-based molecular catalysts, covalently bound to the polydopamine surface.

While in our first publication on the topic, we looked at light-driven NAD+ reduction to NADH (similar to nature!), we now investigated light-driven hydrogen production and screened five different Rh-catalysts for their activity. All of them have previously shown (low) activity for hydrogen evlution in electrocatalysis, and we found it interetsing to see whether their activity in photo-redox-caalysis would change.

Most of the paper goes into in-depth characterization of the material, but to me, the most important aspect is that at first, we could not see clear trends in catalytic turnover (defined as generated hydrogen molecules per catalytic site). When we calculated turnover based on the number of nanorods, we found that a large number of catalysts per nanorod were beneficial to hydrogen evolution. This means: just functionalize each nanorod with as much catalyst as possible and you'll generate lots of hydrogen. In this case, the maximum turnover was in the order of 18,000 molecules of hydrogen per nanorod, which is quite a lot (but not as good as metal-functionalized rods).

{% include figure.liquid path="assets/img/boecker_screening_2024-activity.jpg" class="img-fluid rounded z-depth-1" %}

But this does not tell us anything about the activity of a single catalytic site - how active is a single molecular catalyst on the surface? And indeed, when we looked at the turnover on the basis of total catalysts (and not nanorods), we found: the less catalyst attached to the nanorod, the higher the individual activity of the catalyst. In fact, this was already observed for metal catalysts (where a single metal-nanoparticle is much more efficient than multiple particles) and makes immediate sense, since all catalysts shae the same electron source. And if they have to share with fewer catalytic sites, the chance of two electrons transferred to a rhodium catalyst increases (you need two electrons to generate molecular hydrogen).

What does this tell us now about the optimal system? I have no idea. There should be a sweet spot somewhere, where you can perfectly balance individual catalytic efficiency and total catalytic efficiency, but that was outside the scope of our publication. Since the catalysts we screened all have slightly different catalytic efficiency, we can also not perfectly compare them to each other, and selectively functionalizing a system with a knon number of molecules is a lot of work. Still, it shows that such complex systems require a lot of optimization for them to show their actual maximum efficiency, and that the evaluation what constitues as **efficiency** is not so straightforward.

**This work was collaborative work between Leibniz-IPHT/RPTU Kaiserslautern-Landau, MPIP Mainz, JGU Mainz, Jena University, and Ulm University, funded by the DFG-project CataLight.**

## related publication
<div class="publications">
  {% bibliography -f papers -q @*[key=boecker_rhodium-complex-functionalized_2021]* %}
  {% bibliography -f papers -q @*[key=boecker_screening_2024]* %}
</div>
