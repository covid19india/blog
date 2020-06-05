---
layout: post
title:  "A sneak peek into data operations"
date:   2020-05-27 15:00:00 +0900
categories: update
---

This is more like a reflective write than an informative one. An article to remind ourselves how we used to manage data ops, how we do now and how it might look in the future (who knows about that?!).

Two months ago when we started there was so little structure and method in the way data was reported. We used to refer to multiple news sources, discard a few, accept a few, debate and then update! It was quite a chore. But given that the number of cases were limited, this was still manageable. We had enough bandwidth. Nowadays, it's so very different that a two months younger us would've found the two months older us quite mechanical and boring! But here goes the workflow anyway.

**Sources** - it all starts with the source!

Our sources range from twitter news handles (mostly ANI now) to state bulletins to state dashboards to govt bodies tweeting on twitter. In true Indian spirit, the diversity in the format of data given out by each state is mindboggling. The bulletins given by Delhi and Karnataka are as different as paranthas and idli dosas are (no exaggeration here. That's how different they actually are)! 

Once the source gives a bulletin - a tweet/an actual bulletin, we start with the data entries in a google sheet managed by the backbone of the whole system - the data ops team. As I mentioned before, during the initial days, this meant figuring out what the tweet actually meant - figuring out the district, the city et al. Nowadays, these are given out in the bulletins (there are a few outliers even now). This data is entered into the sheet per district - the confirmed, the recovered and the deceased. This data is the increase for that district for that day, i.e the delta for that district. Hence every district potentially requires 3 entries to be made (confirmed delta, recovery delta, deceased delta). Then the sources are cited in the sheets and the state numbers are checked to make sure that there are no duplicates. 

This, is an ideal scenario. More often than not, we end up with lots of "Unknown" districts on our site. Some bulletins do not contain district details. In cases as such, we make entries with no district mentioned. We add the delta to the state's category and wait for future updates that can allow us to "expand" the entries. I'll get to that bit next.

**Data updation** - the magixxx as we call it.

So what if we do not get data with district splits but get that split later in some other bulletin down the line? We do row expansion. 

Row expansion means we take the total count that the bulletin with no details had, make a -ve count entry equalling that value (for the same date), and then add district splits that total the actual count. Sounds confusing? It is. 

Consider the following example: 

    Karnataka, 20, Recovered, , 30/05/2020 (Entries for a bulletin with no details given at 1:00pm)
    ..
    ..
    Karnataka, 10, Bengaluru Urban, Recovered, 30/05/2020 (Entries for a detailed bulletin given at 4:00pm) 
    Karnataka, 10, Gadag, Recovered, 30/05/2020
    Karnataka, -20, , Recovered, 30/05/2020 (Note the empty district and the negative value).

The above method ensures that the districts end up with proper counts for those days and the state count remains the same. It's a "jugaad" that works. These weren't easy for us to handle during the first two versions of the sheets. We did not have number of cases column then. That brings me to the next point. Challenges!

**Challenges** - stormy seas make skillful sailors (not an endorsement to go out in a storm).

Data quality - this requires a post on its own. I mentioned before that we have a "number of cases" column. That column was introduced because of the lack of demographics data. Most of the rows we were entering had no information other than the district, state and the date. Made little sense for us to keep so many entries. We began to merge them into single district entries. There are only a handful of states that provide detailed information.

Formats - oh my! Where do I start? From the myriad of dashboards to the bulletins that are sent out as pics. From pdfs to tweets. This truely is a challenge. Imagine making entries for 75 districts for the three categories where the bulletin comes as a pic with district names in Hindi! Or those bulletins which do not provide confirmed cases so you have to calculate it from active and outcome counts! That's what we have to deal with.

Discrepancies - State district numbers change. And change as in, they reconcile and reallocate patients from one district to another. This means at any point in time, we make entries, then sit and figure out when and where the district numbers started to mismatch! To add to this, now many have multitide of categories - migrants, other states, airports, domestic airports, railways, foreign returnees, deportees, Italians, BSF... There's also an "Unassigned to states" that has cropped up! Then there are the mismatches between state and district bulletins (possibly due to time of reporting). Sigh!

Rush hours - The evenings nowadays are the rush hours. Not in terms of the traffic on roads (hopefully not), but in terms of the bulletins that come. A large number of states release bulletins at around the same time. This requires us to update each and every district that sees a change in number. India has 700+ districts. So even if 50% of these get updated, we need 350 districts to get updated. Sometimes these entries pile up and we end up updating late into the night (and we hear quite a lot about it on twitter :P).

**Future** - hopefully a brighter one.

From data operations point of view, we are trying to automate certain tasks that we volunteers were doing manually. This saves not just time, but also reduces the stress on the people involved. This might even free us up to focus on other things that can help - more time on analysis, representation of data and others. As the clichéd sentence goes - "we are living for today" as of now. Many asked us two months ago how we would manage when the cases per day would be in thousands. We didn't have any answers then. Yet, somehow we have been able to. The same stands for the future - we don't know.

**Parting thoughts** (Ran out of witty stuff to write)

The above write shows just a glimpse of what it is like. There are aspects that are totally kept out of it - the technical challenges to keep things running, the auxillary teams that capture data related to testing et al, the analysis team that tries to make sense of the data, the twitter following that keeps us on our toes, the constant battle to remember that these numbers are humans, the constant stream of information about the plight of different sections of the society... In short, this is a weird phase in our lives. Maybe writing these blogs will turn out to be a way to speak our hearts and minds out. Who knows! For now, this is where we are. 

Until we write next,  
Stay safe!  
Bee