
### From me to EO, PR:
As discussed yesterday, here is the paper by Florian Lemarié et al about his ABL: https://gmd.copernicus.org/articles/14/543/2021/

I wonder if their fig. 3 can answer your question maybe? The ABL1D is like an additionnal layer between atmospheric reanalysis and the ASL bulk module. The ASL bulk module  thus remains the one talking to SI3 and OCE, unlike in a normal coupling situation with OASIS, i guess?
And here is how the talking is done with SI3 step by step: (cf section 3.3)?
I hope this help! In any case, would you mind telling  me again which surface process you wondered if it was still included or not when the ABL is on?


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

### My comments back:

Thank you very much for the explanations. I understand better the questions you have/had.
But i still need some time to think more about it and make sure i fully understand your comments and conclusions…

I do understand that the ABL1D from Florian follows your “option 2” below. Their model is clearly presented as an intermediate interactive layer rather than an full atmospheric model coupled in the way it is usually done.
The way they present their ABL1D model is to say that “instead of directly using the atmospheric fields at 10m to constrain the oceanic model (via the Bulk formulation) as in the usual forcing strategy, the goal of the ABL1D model is to estimate a correction to the 10m large-scale atmospheric fields, due to both the fine resolution in the oceanic surface fields and the two-way air–sea interactions. “

What i understand less in in your previous comments is this part:

 > _“But ice and the ice-ocean mixture are more challenging. Firstly, the near-surface temperature profile in the ice reacts very quickly to changes in the atmosphere, so a “good” coupled ice-atmosphere model should couple the atmospheric and ice thermodynamics every time step. Or, preferably, solve the whole column together.[…] Florian, however, effectively assumes that the surface temperature of the ice is fixed during the atmospheric sub-stepping”_

Florian’s ABL1D does run at the same time-step as the ocean and sea ice model (which is 720 seconds in our NANUK4 configuration), so, as far as i understand, the atmosphere and ice do talk to each other every time-step. The turbulent part in the ABL is supposed to react to the changes in the sea ice and ocean conditions every time step.


Also i’m a bit confused about your second point, where you said:

 > _“Secondly, it is not at all clear how to treat the mixed ice-ocean surface from the atmospheric point of view. If we, for the sake of argument, imagine that all the open water is contained in a lead or a single polynya. Then we can imagine that the atmospheric conditions right above the opening are very different for those right over the ice but that at some (unknown!) height, everything in the atmospheric column is perfectly mixed. Florian’s approach implicitly assumes that this mixing happens at the very lowest level of the atmospheric model. This is sometimes right and sometimes wrong - depending on the ice conditions!”_

I would have thought that it was exactly the point of such ABL1D model to diagnose the boundary layer height, above which the atmosphere is not affected anymore by the changing conditions at the surface. In Florian’s ABL1D, the height of the boundary layer is diagnosed _“using an integral Richardson number criteria (Sect. 3.2 and 3.3 in Lemarié et al., 2012) […]”._

In the 2021 paper , they have one figure (FIG11) showing that the height of the ABL in average over one year, is tightly correlated with the average contours of sea ice cover. In average over 1 year the ABL height over sea ice is about 100-300 meters while over the arctic ocean it is more 1 km.
But I agree of course that one thing to check is how it behaves at shorter time scales.

Or maybe i don’t see your point? - Sorry for the naive questions/answers! ;)
This discussion is at least very useful for me to identify better the key things to check in Florian’s ABL1D and in the first test simulations i’m doing these days.
We should also consider organizing a little discussion soon directly with Florian.


### Last round from EO:

* To my Point 1:
> That’s interesting. Full atmospheric models run at a shorter time step than ocean models, so I assumed this would also hold for the ABL models. But apparently, I was wrong. I thought the ABL would do something like ten time steps for each ocean time step, which is why I wrote the part above. But this is good - it makes the model setup simpler and easier to understand.

* To my Point 2:
* > Florian’s approach does a good job of distinguishing between open water and full ice cover. And this is, indeed, the first-order effect. The second-order question is what happens when the ocean is partially ice-covered. Here, the two extreme representations are that all the open water is on one side of the grid cell and all the ice on the other, so we effectively have two atmospheric columns, which should start mixing at some height because of advection and diffusion. The other extreme is to say that the ocean is covered with a huge number of tiny ice floes separated by tiny openings. In that case, we can say that we should have a huge number of atmospheric columns (one for each floe and opening), but that’s actually not needed, as they will mix very close to the surface and are all the same above. This second one is a very common, implicit assumption, and also one that Florian makes (probably implicitly also). But since we’re interested in leads, then this is an assumption we should be interested in challenging.

> This is a very nice discussion, and as you see, I don’t have the complete picture either and have made some false assumptions! We should certainly try to set up a meeting with everyone involved, also Florian and Richard. It could be a mini-workshop of sorts, where we present what we’ve each done and talk about the assumptions we make, etc. That may have to wait a bit until both you and Heather have some results. What do you think?


