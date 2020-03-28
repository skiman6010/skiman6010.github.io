---
layout: post
title: Speeding up SQL Transactional Jobs
---

Yesterday, I was working on SQL code that would truncate the table, pull the newest data of completed jobs by a department with an Action_Date (just the time the job was completed) of `GetDate() - 366` (so one year back from the current date) and then inserts that into the empty table. This took about half an hour to run, which was acceptable on the daily AM job schedule. 

The problem was that one of our reporters wanted to create a new report that expands from 2014 to the current date. This seems like an easy fix, right? Just change the `GetDate() - 366` to `@dt = 2014-01-01`. Right, this is a quick and dirty fix that would work, except the performance on large datasets is atrocious, because it has to check every single record for the date, of course. On our datasets it leads to a 3x increase in runtime because of how much extra data it is having to check and insert and that runtime will only get larger as time goes on and more data needs to be checked. 

Thankfully, this data is completely transactional. As soon as a job is marked as complete, the data can't be changed in the table. So there is no backlog of data being changed. That six month old row will always be the same. This is good news. After considering this for a while, I came to a conclusion on how to proceed that would not only fix the long runtime, but also make the job run faster than it ever has before. My solution was the following: 

```
DELETE FROM Table WHERE Table.dateval > GetDate() - 5 

SET @dt = GetDate() - 5
INSERT INTO Table
select ...
where Table.dateval > @dt
```

So first off, instead of just truncating the entire table, I just do a `Delete FROM` the table where the date the job was finished is greater than the current date minus five days. I choose five because that would give me some leeway in case the Daily AM jobs failed more than once so I would not miss any new data coming in, it is just a small amount of safety padding. Next, I just insert those last four days (Yes, four days. This is because of how subtracting by the GetDate() function works.) of data into the table using a WHERE clause to check to make sure the date the job was completed was done in the last four days. I also set our four day date value as a variable (@dt) so I could use it in some other checks in my `LEFT JOIN` statements in other parts of this code. 

All in all, a simple but elegant solution that really sped up our Daily AM jobs. 



