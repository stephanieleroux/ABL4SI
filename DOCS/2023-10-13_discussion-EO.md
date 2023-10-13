
### To EO, PR:
> As discussed yesterday, here is the paper by Florian Lemarié et al about his ABL: https://gmd.copernicus.org/articles/14/543/2021/
I wonder if their fig. 3 can answer your question maybe? The ABL1D is like an additionnal layer between atmospheric reanalysis and the ASL bulk module. The ASL bulk module  thus remains the one talking to SI3 and OCE, unlike in a normal coupling situation with OASIS, i guess?
And here is how the talking is done with SI3 step by step:
 (cf section 3.3)?
I hope this help!
In any case, would you mind telling  me again which surface process you wondered if it was still included or not when the ABL is on?
cheers
Stephanie


### EO's answer 1
> Hi Stephanie, Very informative - thanks!
> I’ll try to explain better what I was thinking about at the meeting. And then I hope it will also be clear why what you just shared is so informative. :)
> When it comes to coupling atmosphere and ocean, there are two basic approaches. The atmosphere receives SST, keeps it fixed over several atmospheric time steps, and then sends back either
 1. the surface fluxes calculated by the atmosphere, which the ocean then uses directly to update SST or
 2. the 2 and 10 m atmospheric data, for which the ocean model uses bulk formulas to calculate the fluxes it then uses to update SST
> Option one is clearly cleaner, while option two introduces a lot of back-and-forth that can cause errors. But there are still people that use option two - in particular, the CESM and Florian Lemarié, it seems :)
> But ice and the ice-ocean mixture are more challenging. Firstly, the near-surface temperature profile in the ice reacts very quickly to changes in the atmosphere, so a “good” coupled ice-atmosphere model should couple the atmospheric and ice thermodynamics every time step. Or, preferably, solve the whole column together. Heather, Richard, and I are working on this, but it’s a technical challenge for several reasons. Florien, however, effectively assumes that the surface temperature of the ice is fixed during the atmospheric sub-stepping, but this is wrong. That error will affect the atmosphere, but I don’t know how significant that effect is. We need to find out - either by reading or testing!
> Secondly, it is not at all clear how to treat the mixed ice-ocean surface from the atmospheric point of view. If we, for the sake of argument, imagine that all the open water is contained in a lead or a single polynya. Then we can imagine that the atmospheric conditions right above the opening are very different for those right over the ice but that at some (unknown!) height, everything in the atmospheric column is perfectly mixed. Florian’s approach implicitly assumes that this mixing happens at the very lowest level of the atmospheric model. This is sometimes right and sometimes wrong - depending on the ice conditions!
> So, as far as I understand (not having read the paper or the code), his approach is okay for the ocean but problematic for the ice. But remember that it’s not only Florian doing this; respectable models like CESM and even PolarWRF (as far as I understand) also do this. So we should be kind to Florian, but we can do better. ;)
