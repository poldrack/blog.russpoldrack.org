---
date: '2018-01-22T06:39:00.000-08:00'
description: ''
published: true
slug: 2018-01-22-defaults-in-r-can-make-debugging
tags:
- http://schemas.google.com/blogger/2008/kind#post
- legacy-blogger
time_to_read: 5
title: Defaults in R can make debugging incredibly hard for beginners
---

*This was originally posted on blogger [here](http://www.russpoldrack.org/2018/01/defaults-in-r-can-make-debugging.html)*.

<div>I am teaching a <a href="https://psych10.github.io/">new undergraduate statistics class at Stanford</a>, and an important part of the course is teaching students to run their own analyses using R/RStudio. &nbsp;Most of the students have never coded before, and debugging turns out to be one of the major challenges. Working with students over the last few days I have found that a couple of the default features in R can combine to make debugging very difficult on occasion.&nbsp; Changing these defaults could have a big impact on new users' early learning experiences.</div><div><br /></div><div>One of the datasets that we use is the NHANES dataset via the NHANES library. &nbsp;Over the last few days several students have experienced very strange problems, where the NHANES data frame doesn’t contain the appropriate data, even after restarting R and reloading the NHANES library. &nbsp;It turns out that this is due to several “features” in R: </div><div><ul><li>Users are asked when exiting whether to save the workspace image, and the default is to save it.</li><li>The global workspace (saved in&nbsp;~/.RData) is by default automatically loaded upon starting R.</li><li>When a package is loaded that contains a data object, this object is masked by any object in the global workspace with the same name. &nbsp;</li></ul><div><br /></div></div><div>Here is an example. &nbsp;First I load the NHANES library, and check that the NHANES data frame contains the appropriate data. </div><div><br /></div><div><span style="font-family: Courier New, Courier, monospace;">&gt; library(NHANES) </span></div><div><span style="font-family: Courier New, Courier, monospace;">&gt; head(NHANES) </span></div><div><span style="font-family: Courier New, Courier, monospace;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ID SurveyYr Gender Age AgeDecade AgeMonths Race1 Race3&nbsp;&nbsp;&nbsp;&nbsp;Education MaritalStatus&nbsp;&nbsp;&nbsp;&nbsp;HHIncome HHIncomeMid </span></div><div><span style="font-family: Courier New, Courier, monospace;">1 51624&nbsp;&nbsp;2009_10&nbsp;&nbsp;&nbsp;male&nbsp;&nbsp;34&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;30-39&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;409 White&nbsp;&nbsp;&lt;NA&gt;&nbsp;&nbsp;High School&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Married 25000-34999&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;30000 </span></div><div><span style="font-family: Courier New, Courier, monospace;">2 51624&nbsp;&nbsp;2009_10&nbsp;&nbsp;&nbsp;male&nbsp;&nbsp;34&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;30-39&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;409 White&nbsp;&nbsp;&lt;NA&gt;&nbsp;&nbsp;High School&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Married 25000-34999&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;30000 </span></div><div><span style="font-family: Courier New, Courier, monospace;">3 51624&nbsp;&nbsp;2009_10&nbsp;&nbsp;&nbsp;male&nbsp;&nbsp;34&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;30-39&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;409 White&nbsp;&nbsp;&lt;NA&gt;&nbsp;&nbsp;High School&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Married 25000-34999&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;30000 </span></div><div><span style="font-family: Courier New, Courier, monospace;">4 51625&nbsp;&nbsp;2009_10&nbsp;&nbsp;&nbsp;male&nbsp;&nbsp;&nbsp;4&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;0-9&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;49 Other&nbsp;&nbsp;&lt;NA&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;NA&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;NA&gt; 20000-24999&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;22500 </span></div><div><span style="font-family: Courier New, Courier, monospace;">5 51630&nbsp;&nbsp;2009_10 female&nbsp;&nbsp;49&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;40-49&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;596 White&nbsp;&nbsp;&lt;NA&gt; Some College&nbsp;&nbsp;&nbsp;LivePartner 35000-44999&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;40000 </span></div><div><span style="font-family: Courier New, Courier, monospace;">6 51638&nbsp;&nbsp;2009_10&nbsp;&nbsp;&nbsp;male&nbsp;&nbsp;&nbsp;9&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;0-9&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;115 White&nbsp;&nbsp;&lt;NA&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;NA&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;NA&gt; 75000-99999&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;87500 </span></div><div><br /></div><div>Now let’s say that I accidentally set NHANES to some other value: </div><div><br /></div><div><span style="font-family: Courier New, Courier, monospace;">NHANES=NA </span></div><div><span style="font-family: Courier New, Courier, monospace;">&gt; NHANES </span></div><div><span style="font-family: Courier New, Courier, monospace;">[1] NA </span></div><div><br /></div><div>Now I quit RStudio, clicking the default “Save” option to save the workspace, and then restart RStudio. I get a message telling me that the workspace was loaded, and I see that my altered version of the NHANES variable still exists. &nbsp;I would think that reloading the NHANES library should fix this, but this is what happens: </div><div><br /></div><div><span style="font-family: Courier New, Courier, monospace;">&gt; library(NHANES) </span></div><div><span style="font-family: Courier New, Courier, monospace;"><br /></span></div><div><span style="font-family: Courier New, Courier, monospace;">Attaching package: ‘NHANES’ </span></div><div><span style="font-family: Courier New, Courier, monospace;"><br /></span></div><div><span style="font-family: Courier New, Courier, monospace;">The following object is masked _by_ ‘.GlobalEnv’: </span></div><div><span style="font-family: Courier New, Courier, monospace;"><br /></span></div><div><span style="font-family: Courier New, Courier, monospace;">&nbsp;&nbsp;&nbsp;&nbsp;NHANES </span></div><div><span style="font-family: Courier New, Courier, monospace;"><br /></span></div><div><span style="font-family: Courier New, Courier, monospace;">&gt; NHANES </span></div><div><span style="font-family: Courier New, Courier, monospace;">[1] NA </span></div><div><br /></div><div>That is, objects in the global environment take precedence over newly loaded objects.&nbsp; If one didn't know how to parse that warning they would have no idea that this loading operation is having no effect.&nbsp; The only way rid ourselves of this broken variable is either restart R after removing ~/.RData, or remove the variable from the global workspace: </div><div><br /></div><div><span style="font-family: Courier New, Courier, monospace;">&gt; rm(NHANES, envir = globalenv()) </span></div><div><span style="font-family: Courier New, Courier, monospace;">&gt; library(NHANES) </span></div><div><span style="font-family: Courier New, Courier, monospace;">&gt; head(NHANES) </span></div><div><span style="font-family: Courier New, Courier, monospace;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ID SurveyYr Gender Age AgeDecade AgeMonths Race1 Race3&nbsp;&nbsp;&nbsp;&nbsp;Education MaritalStatus&nbsp;&nbsp;&nbsp;&nbsp;HHIncome HHIncomeMid </span></div><div><span style="font-family: Courier New, Courier, monospace;">1 51624&nbsp;&nbsp;2009_10&nbsp;&nbsp;&nbsp;male&nbsp;&nbsp;34&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;30-39&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;409 White&nbsp;&nbsp;&lt;NA&gt;&nbsp;&nbsp;High School&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Married 25000-34999&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;30000 </span></div><div><span style="font-family: Courier New, Courier, monospace;">2 51624&nbsp;&nbsp;2009_10&nbsp;&nbsp;&nbsp;male&nbsp;&nbsp;34&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;30-39&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;409 White&nbsp;&nbsp;&lt;NA&gt;&nbsp;&nbsp;High School&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Married 25000-34999&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;30000 </span></div><div><span style="font-family: Courier New, Courier, monospace;">3 51624&nbsp;&nbsp;2009_10&nbsp;&nbsp;&nbsp;male&nbsp;&nbsp;34&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;30-39&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;409 White&nbsp;&nbsp;&lt;NA&gt;&nbsp;&nbsp;High School&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Married 25000-34999&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;30000 </span></div><div><span style="font-family: Courier New, Courier, monospace;">4 51625&nbsp;&nbsp;2009_10&nbsp;&nbsp;&nbsp;male&nbsp;&nbsp;&nbsp;4&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;0-9&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;49 Other&nbsp;&nbsp;&lt;NA&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;NA&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;NA&gt; 20000-24999&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;22500 </span></div><div><span style="font-family: Courier New, Courier, monospace;">5 51630&nbsp;&nbsp;2009_10 female&nbsp;&nbsp;49&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;40-49&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;596 White&nbsp;&nbsp;&lt;NA&gt; Some College&nbsp;&nbsp;&nbsp;LivePartner 35000-44999&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;40000 </span></div><div><span style="font-family: Courier New, Courier, monospace;">6 51638&nbsp;&nbsp;2009_10&nbsp;&nbsp;&nbsp;male&nbsp;&nbsp;&nbsp;9&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;0-9&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;115 White&nbsp;&nbsp;&lt;NA&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;NA&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;NA&gt; 75000-99999&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;87500 </span></div><div><br /></div><div>This seems like a combination of really problematic default behaviors to me: automatically saving and then loading the global workspace by default, and masking objects loaded from libraries with objects in the workspace.&nbsp; Together they have resulted in hours of unnecessary confusion and frustration for my students, at exactly the point in their learning curve where it is most problematic to do so.</div><div><br /></div><div>I have one simple suggestion for the R developers: Please turn off automatic loading of the workspace by default. &nbsp;It would be as simple as changing the default on one radio box, and it would potentially save new users lots of time and frustration. </div><div><br /></div><div>Until that happens, beginning R users should do the following: </div><!--?xml version="1.0" encoding="UTF-8"?--> <br /><div><ul><li>Under the Preferences panel (the General Tab in R), unselect the “Restore .RData into workspace on startup” option. &nbsp;</li><li>I would also recommend setting the “Save workspace to .RData on exit” preference to “Never”, since I find that I generally only restart R when I want the entire workspace cleared out, so this option will never be of use to me.</li></ul></div>

---

## 1 comments captured from [original post](http://www.russpoldrack.org/2018/01/defaults-in-r-can-make-debugging.html) on Blogger

**muswellbrook said on 2018-01-22**

I agree. I found exactly the same problem when I first started using R and quickly discovered the solutions you describe here. But the problem is the quirky default behavior. And I suspect most experienced R users don’t even think about it anymore (because they have changed their default settings). But it is annoying and especially so when you discover there is no appropriate command to clear the workspace and the  recommended method is to restart R
