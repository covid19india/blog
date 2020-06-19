---
layout: post
title:  "Rows and rows of data"
date:   2020-06-19 22:26:00 +0900
categories: update
---

A message written sometime back for the new volunteers who join our data_ops team


Thanks for responding to the call and going through the Raw_Data sheet that I had shared with the team. If you haven’t, please do check it out here : ...


1. How do we add the new cases

    - Our list of sources are available here : https://telegra.ph/Covid-19-Sources-03-19 . The members and bots in this group monitor these sources and can post the new tweets, bulletins here and then update it in the Raw_Data sheet.

2. What sources are allowed to be considered for update? 

    - State bulletins
    - Tweets by official handles (Heath Depts, CM, Health Minister)
    - State dashboards
    - ANI, PIB, AIR tweets

3. How to update data? 

    - Verify the information : Make sure that the source is as per 2., and not contradictory. If you have confusions, please discuss in the data_ops group.

    - Some states provide patient level information - in that case, add one row per patient with num_cases as 1 and enter all the details : Age, Gender, Notes. If some supporting source (news articles provide this information, please add them in Source_2 column)

    - Some states only provide District Level information of new cases, recovery, deaths. In that case, add it at that level by adding the num_cases value accordingly so that the district totals are matching.

    - Once you update the data, always make sure that the state totals are matching as per the source that you’re using. If the source just mentions phrases like “25 New cases in Tripura” without a total count of cases, it’s better to stay away. But you can use a source that says “25 cases in Tripura making the total cases 100”.

    - If district-wise numbers are added, please do make sure that the totals are mathcing, by looking at the district-wise sheet.

    - Always update Source column

    - If someone else is already updating some cases in the sheet, it’s better to wait for a few minutes and let them finish the update. But there is no rule that updates has to be in order.

    - Do not delete the entries made by another person, please do discuss any error in the group. One of the admins (which we will rotate periodically) will be able to fix the issues. If some serious error is noted, please PM couple of admins.

4. How can num_cases be negative?
Sometimes, states move patients from one district to another. This causes our districts to have more cases than what the bulletin is reporting. In that case, we will reduce the case from one district to be added in another. 

5. Sheet is slow to load. What should I do?
As the number of rows increase, the Google Sheet performance goes down. One of the admins will then move the entries to a new sheet from where we can continue.

Other notes: 

- Follow the instant update channel to see the current data. The website might have slight delays. https://t.me/covid19indiaorg_updates

- Pace yourself. All these numbers and updates affects your mental and physical health. Stay enough time offline and stay healthy!


--

Bob