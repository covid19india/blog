---
layout: post
title:  "Midweek Motivation - Automation!"
date:   2020-06-16 15:00:00 +0900
categories: update
---

The monsoon has arrived!!! (a totally random update that has no connection to the post here)   
So today, lets talk about automation. Before we start off on the automation train, it's important to recap on some basics of the way we capture data and the pros and cons of the same. Also, brace yourself for a semi-technical article. 

**Data format and challenges of data collection**  
We capture the confirmed, recovered/discharged, deceased numbers of districts every day. That is, the delta for those days. Now this leads to some interesting problems. First, the number of districts is quite large, 750+ across India. Second, the data sources are varied - tweets/websites/pdfs/jpgs. Third, state governments often move cases from one district to another - AKA reassignment. 
This means that those members who work in the data ops often need to consider all the districts of a state to make sure no district numbers are changed by the states due to reassignments. Mentally, this is an exhausting exercise, not to mention the time it takes and the possible human errors that might creep in.

Overall, entering this data is difficult. The challenge also is in the diversity of the bulletins - column orders are different, data reported is different, some use vernacular names et al. This over a span of three months made us realise that we need to automate some of those tasks that are time consuming and error prone. 

**Strategies for automation**  
One of the first things we started to do was to use `curl` command to fetch data from certain dashboards and use our district api to figure out the deltas. This was a very rough and adhoc approach. Soon enough, we realised that using shell script to extract data isn't very effective in terms of reusability. 

To make it more generic and efficient we decided to go the python route. The first step was to list out all the dashboards - this was easy since we already had a source list that we monitor for regular updates. Next we had to figure out how to extract the data from these websites. We also wanted something quick and workable. The obvious choice was `beautiful soup`. This python package provides easy ways to access the html contents of a website. A few experiments in, we had the basic idea of beautiful soup and possible ways to pull the data and get the daily deltas.

However, we soon realised the diversity of these state run dashboards. Some are vanilla html, some are modals, some send js files with all the data, some have json endpoints, some are post, some are get requests, some even put up pics in place of tables! Truely no singular solution was going to work. This meant we had to keep one base layer for calculation of delta, and everything above it would be customized based on state dashboards. This isn't the cleanest possible solution from an engineering point of view. However, it's something that works and is quick to develop. 

This intial _automation_ gave us a base to work with. With incremental additions, as of today, we have AP, AS, CH, LA, GJ, MH, OR, PY, TR, RJ dashboards scraped. Rajasthan has removed the table and has replaced the table with a pic. However, the code still remains, maybe they will revert back to the older mode some day `¯\_(ツ)_/¯`. 

**All good, until the bulletin is a jpg**  
So scrapping website is all good. You issue a get request, parse the html, get the things you want, and send it to your delta calculator. But what if there is no website? And what if the one with no website has 75+ districts and sends the daily bulletin as a jpg image? That's when you start to think about _OCR_. Why? Because there is no other option left. 

OCR isn't new. There are quite a few mature libraries that provide character recognition. First we started to experiment with `tesseract` to see how it would work. With the intial few experiments, it was clear that tesseract had to be trained before we could use it in a reliable way. With little to no knowledge on tesseract and with time running out, we explored `opencv`. The accuracy we got wasn't that great with opencv either. Then, we stumbled across `Google Vision API`. This had quite a good quality in terms of detection. It also gave bounds of the texts detected. Using the bounds of texts, we were able to recreate the table in a csv format. The initial outputs weren't that clean. There were few columns missed, few wrongly mapped columns and all. However, it was a huge leap forward from the delta one had to calculate manually from a jpg image!  
The next hurdle was that of language. If the district names are in Hindi, and the api we use has english districts how do you actually figure out districts? Time for a dirty and quick hack. We used a small dictionary mapping to figure out which hindi word meant which english word. We could've gone down the route of translations using google or similar stuff, but felt this makeshift solution was better and more flexible. 

With the major issues sorted, we began beta runs of the OCR code. The beta runs showed us the need for better debugging. It showed us how important it was for someone running the code to know what was happening. So in a true jugaad spirit, we added matplotlib to plot the texts detected. With this, we had a decent enough solution that we could use for daily updates. Using this, we currently extract data out of UP, BR, PB and RJ bulletins. Of course none of this is completely foolproof. All of these need debugging skills. Even google api struggles with some of the images given in these bulletins. Sometimes, we run one step, correct the intermediate output manually and run the next step. The process still needs a lot of refinement!

So as you might've guessed, we are using OCR with a pinch of salt. We get the deltas, we verify it to make sure all looks good, then update the data.

**PDF Bulletins, the final kind?**  
So once we were done with jpgs (kinda), the next was to tackle pdf bulletins. These are quite simple compared to the jpg bulletins. There are quite a few pdf converters like `pdftotext`, `camelot`, `pdfminer` et al. For our simple usecase, we are currently relying on `pdftotext` and `camelot`. The plan is to completely migrate to camelot as it specializes in table reads. The approach involves reading the pdf page that has relevant table, extracting required data and converting it into a csv that the delta calculator can consume. This is currently being used for KA, TN, HR (occassionaly when they give out a good pdf) and PB (again occassionally when they provide a good pdf). Most of the pdf based automation is way more accurate than the OCR based ones. However, pdfs are hard to come by `-__-` 


**So do we no more need humans to update?**  
Automation is a tricky word. It can give a false impression that we have removed human intervention. In our case, automation is just a tool that helps the volunteers to make better decisions. It reduces their work on certain levels and allows for end-of-the-day tallying with state dashboards. We still have an active data operations team that works crazy hard to keep the site updated. We still have lots of states that cannot be automated due to the lack of good quality bulletins. Even with the dashboards that are scraped, a lot of them are delayed by quite a few hours for us to update using those.


With cases increasing by the day, it looks like we will have to rely more on these automated processes that help us in updating the site faster and better. There's always a moral question of how much automation is good and what is the line that we shouldn't cross. Especially when we are dealing with data which represents human beings. A philosophical question that probably has varied answers? Either ways, as of now, it's a human being who still tirelessly updates the data for the site to show :) (with some help from a few scripts)


Until we write again,  
Stay safe!  
Bee.


