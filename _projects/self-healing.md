---
layout: page
title: Self-Healing Polymers
description: during my PhD, I investigated the optical properties and energy transfer processes of self-healing polymers
img: assets/img//projects/self-healing.png
importance: 1
category: work
---

It was a popular song from the 90s that said “Life in plastic, it's fantastic” - and indeed, the premises of the “Polymer Age” are fantastic. By now, polymers have become ubiquitous and are used in basically every conceivable way. This is an ongoing trend, and as polymers become even smarter, they might one day completely eliminate traditionally used materials from industrial and everyday applications from the market.

However, polymers – and by extension, all materials – have one inherent flaw: they break, losing their functionality, rendering them useless. Traditionally,
material design was only concerned with improving the resistance of polymers towards damage, which in turn resulted in longer lifespans. This “damage prevention” approach still cannot fully prevent material failure. The “damage management” approach, on the other hand, presents a conceptually different way to handle material damage. Instead of avoiding the accumulation of damage, it tries to cope with it and, by means of self-healing materials, only looks at the net usage time of the material. However, self-healing research is still in its infancy: as of now, it cannot heal more sophisticated material failures, such as the photooxidative cleavage of bonds, that lead to material failure not on a mechanic, but on an optoelectronic level. Additionally, spectroscopic methods are rarely applied in the investigation of self-healing systems, and are mainly used to only uncover the chemistry behind the healing event, and not to probe the material properties.{% cite ahner_self-healing_2015 %}

During my PhD studies, I investigated polymeric systems which included dynamic bonds and chromophoric units. By applying a thermal trigger, the chromophores within the polymer chain reshuffle and change their orientation towards each other. These changes are readily visible as changes in absorption (when the conjugated system length changes) or fluorescence spectra, quantum yield, or lifetimes (when two chromophores are adjacent enough to undergo energy transfer).{% cite micheel_photophysical_2019 %}

## Energy transfer in dynamically linked polymers

Dynamically linked polymers (termed *dynamers* by Nobel laureat Jean-Marie Lehn) are polymers, in which structural units are connected by reversible bonds. In my PhD studies, these were basically always Diels Alder bonds which can be cleaved by changing the temperature and initiating the *retro* Diels Alder reaction. We introduced resonance energy acceptors and donors into these dynamers and investigated the intra- and inter-polymer exchange of these chromophores via fluorescence spectroscopy. {% cite ahner_thermally_2016 micheel_photophysics_2019 %}

Covalent reversible bonds are not the only possibility to introduce dynamic behavior to polymers. We also investigated inter-polymer hydrogen bonds by using different functional groups (urea vs. urethane). The urea bond, which introduces much stornger and/or more hydrogen bonds, brings polymers into much closer contact, and we observed the formation of very planar neighboring polymer chains. {% cite ahner_directed_2017 %}

## Restoration of absorption

The most ambitious goal of this project was to restore the absorption properties of a photo-oxidized conjugated polymer. For this, we used imine bonds as a reversible, conjugated linker (in contrast to the non-conjugated Diels Alder bonds mentioned above). At elevated temperatures, polymers undergo imine metathesis and exchange their chromophoric subunits. The increasing conjugation results in a redshift of the absorption.

Photo-oxidation destroys these conjugated polymers. However, we still have lots of different monomers or oligomeric chromophores in the film - if we again heat the film, these undergo exchange with the photo-oxidized chromophores! This will increase the absorption as long as we have monomers in the film. {% cite ahner_self-healing_2017 %} While this exchange worked really well, we failed to incorporate the chromophores in an actual useful device, because conjugated imines are notorious for being extremely good light absorbers, but extremely bad ad doing anything with this light. I planned to write a review article *Why conjugated imines suck and cannot get anything done*, but then started my postdoc and this article is still in writing hell. It is an interesting story, going back to early experiments by Ciamician and the early photochemistry community, but maybe that is for another day...

## References
<div class="publications">
  {% bibliography -f papers -q @*[key=ahner_self-healing_2015]* %}
  {% bibliography -f papers -q @*[key=ahner_thermally_2016]* %}
  {% bibliography -f papers -q @*[key=ahner_directed_2017]* %}
  {% bibliography -f papers -q @*[key=ahner_self-healing_2017]* %}
  {% bibliography -f papers -q @*[key=micheel_photophysics_2019]* %}
  {% bibliography -f papers -q @*[key=micheel_photophysical_2019]* %}
</div>
