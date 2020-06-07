---
layout: post
title:  "The Wind-Walker"
date:   2020-06-07 23:56:00 +0900
categories: update
---

![Alt Text](/assets/images/windwalker.gif)

*Approximate representation of how our Google Sheet works*

>At the inception of covid19india.org, we needed a full-blown collaborative editing platform, a simple website to represent the data and an automated process to update and show the data. **Literally, overnight**. Using Google Sheets to do this was a no-brainer. 

All the information that is being collected by the volunteers by going through streams of official Twitter handles, state bulletins and dashboard is entered in to a Google Sheet which we call as _Raw_Data_.

Many folks criticized that this is primitive. That we will soon face performance issues. One of our beloved developer created a full-blown Django app to enter the details too. However, the versatility of Google Sheet has no match! Collaborative editing and conflict management is something that can't be built overnight.

Volunteers have entered more than 100,000 rows of data since the inception of the site. Unsurprisingly, the sheet gets pretty heavy to load once it crosses 15,000 rows of data. Laptops start spewing fumes, people start shouting. We had to start using partitions. We could’ve just moved to a new sheet and then do all the calculations in the backend in a programming language of choice. But the sheet itself is a contraption that enables the volunteers to cross-check their entries while they are making the edits. Also, there is comfort in seeing the aggregated values in a tabular form for quick checks.

This means that every three weeks, and now sadly, every two weeks - we have to move to a new version of the sheet. Below is the hustle to do the same.

You can stop reading this post here. Rest of the content is uninformative for the folks who haven’t seen the Patient Database sheet, but this is just a documentation for the volunteers :)

Also, this is Stretchhhhh Blog!

### How to shift to new version of the sheet : 

For these steps, consider that we are moving from v5 (till end of Jun04) to v6(starting from Jun05)

1. Make sure that the entries for the previous day have been completely filled in v5. i.e, since v6 starts from 05th June, all entries upto and including 04th June should be in v5.

2. Notify the change in group.

3. Disable Actions in `api` repo in Github.

—Cursor on v5 file—
3. Make all Editors as Viewers to avoid any entries being made during transition.

4. Go to Google Script Editor >> Triggers and stop all the scripts triggers on the sheet.

4. Do File >> Make a Copy

5. Rename new sheet to `COVID19-India : Patient Database_v6_Jun05` . Version and the date from which the new version starts should be in the file name

—Cursor on v6 file— (Tip : Close the v5 tab in your browser to avoid any tragedies happening)

6. Apply Filter (Not Filter View)

7. Sort by `Date Announced` A-Z

8. Delete any entry that has date greater than Jun04 (Last date of v5). So, in our case any entry made for Jun05 will be deleted. This is to get the final tally at the end of v5 in Statewise and Districtwise sheet. Later, we will copy these from the previous sheet.

9. Go to `Statewise` and select & copy the whole sheet.

10. Go to `Statewise_Previous_Partition` and do paste **values only**.  Only the following columns are required in Previous Partition sheet: 
- State
- Confirmed
- Recovered
- Deaths
- Active
- Migrated_Other

11. Go to `Districtwise` and select & copy the whole sheet

12. Go to `Districtwise_Previous_Partition` and paste **values only**. Only the following columns need to be retained
- SlNo
- State_Code
- State
- District_Key
- District
- Confirmed
- Active
- Recovered
- Deceased
- Migrated_Other

13. Go to `Statewise_Daily` sheet. Select all contents and paste it back as values. This is done to freeze all the values which were computed using formula from Raw_Data

15. Go to `Cases_Time_Series` sheet. Select all contents and paste it back as values. This is done to freeze all the values which were computed using formula from Raw_Data

13. Finally, go back to `Raw_Data` and delete all the entries. The sheet should be empty now.

14. Verify that the `Statewise` sheet is showing value that is as of Jun04 end.

—Cursor on v5 file—
15. If all good, copy and normal paste the values for Jun05 (if any) that were adding in v5. 

—Cursor on v6 file—
16. Check in `Statewise` sheet and verify that India CARD values are as of this switching operation started.

—Cursor on v5 file—
17. If all good, delete the Jun05 entry rows from v5.

—Cursor on v6 file—
18. Data shift is now complete. Go to Tools >> Script Editor. Rename the Sheet script project to `Patientdb_v6. 

19. Go to Triggers. Enable the following triggers. Sheet owner will have to provide access for the script to run.

- `stateLastTimeUpdate` : Every Minute
- `updateCasesTimeSeriesSheet_After12` : Every day 00:00 hours IST
- `updateStatewiseDaily_After12` : Every day 00:00 hours IST

20. Publish the Entire Document from File >> Publish to the Web

—Check your heart rate, praise couple of Hail Marys—

20. Enable actions in `api`.

21. If the numbers look good, reassign the Editor permission to the volunteers.

(There are some further changes required at the code end, which I will be adding in a separate document)

Until we write next,  
Stay safe!  
Bob
