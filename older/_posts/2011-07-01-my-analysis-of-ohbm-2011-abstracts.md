---
date: '2011-07-01T15:53:00.000-07:00'
description: ''
published: true
slug: 2011-07-01-my-analysis-of-ohbm-2011-abstracts
tags:
- http://schemas.google.com/blogger/2008/kind#post
- legacy-blogger
time_to_read: 5
title: My analysis of OHBM 2011 abstracts
---

*This was originally posted on blogger [here](http://www.russpoldrack.org/2011/07/my-analysis-of-ohbm-2011-abstracts.html)*.

As the past chair of the <a href="http://www.humanbrainmapping.org/i4a/pages/index.cfm?pageid=1">Organization for Human Brain Mapping</a>&nbsp;(which just ended its <a href="http://www.humanbrainmapping.org/i4a/pages/index.cfm?pageID=3419">2011 meeting</a> in Quebec City), I was tasked with giving the "Meeting Highlights" talk which traditionally closes the meeting. &nbsp;It's a pretty daunting challenge to summarize an entire meeting with such little time for preparation, so I took the tack of doing lots of mining on the full text of the abstracts prior to the meeting. &nbsp;My entire wrap-up talk is available <a href="http://euphoria.clm.utexas.edu/shared/Poldrack_OHBMWrapup_June2011.pdf">here</a>;&nbsp;below I present the main results of my text mining, along with some additional analyses that didn't make it into the talk.<br /><br /><b>Overall meeting stats:</b><br /><br /><table border="2" cellpadding="4"><tbody><tr> <td>Number of Abstracts</td> <td>2230</td> </tr><tr> <td>Number of Unique Authors</td> <td>7622</td> </tr><tr> <td>Mean # of abstracts/author</td> <td>1.64 (max=33)</td> </tr><tr> <td>Mean # of authors/abstract</td> <td>5.6 (max=41)</td> </tr></tbody></table><br /><b>Authorship distribution:</b><br /><br />The OHBM is known for being an international organization, and the authorship data confirm this. &nbsp;In order to visualize the authorship data, I used the Google Maps API to identify the latitude/longitude for each affiliation in the authorship list. This was successful for more than 90% of the abstracts. &nbsp;These latitude/longitude values were uploaded into Google Fusion Tables, from which I exported a KML file &nbsp;(<a href="https://github.com/poldrack/ohbm2011/blob/master/latlng.kml?raw=true">available here</a>) which I then opened in Google Earth.&nbsp;&nbsp;(That's a lot of Google!)<br /><br />Using Google Earth I then created a tour that circled the globe, showing all of the author locations on a path from Quebec City to Beijing (location of the 2012 meeting). &nbsp;Here is the video:<br /><br /><div class="separator" style="clear: both; text-align: center;"></div><br /><br />Each red pin represents the location of an author at the meeting. <br /><br /><b>Authorship networks:</b><br /><br />Using the abstracts I created a coauthorship network and did some basic analyses on this network (using the <a href="http://networkx.lanl.gov/">Networkx toolbox</a> in Python and the <a href="http://nwb.cns.iu.edu/">Network Workbench</a>). &nbsp;The code and an anonymized version of the graph (in graphml format) are available <a href="https://github.com/poldrack/ohbm2011">via github</a>. &nbsp;Here is an overall view of the network:<br /><br /><div class="separator" style="clear: both; text-align: center;"><a href="http://3.bp.blogspot.com/-NnKEFdPjjK8/Tgzz4RY2h3I/AAAAAAAACLw/8CK45FXpntY/s1600/network_all_nodes.png" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="311" src="http://3.bp.blogspot.com/-NnKEFdPjjK8/Tgzz4RY2h3I/AAAAAAAACLw/8CK45FXpntY/s400/network_all_nodes.png" width="400" /></a></div><br />This shows one giant connected component with 4600 authors (60.3%), along with a large number of much smaller components (the second largest component had 103 authors). &nbsp;Focusing in on the giant component, here is the spring-embedded visualization:<br /><br /><div class="separator" style="clear: both; text-align: center;"><a href="http://1.bp.blogspot.com/-ysSNMv3to_M/Tg2snEHiA3I/AAAAAAAACMA/vTQAJjI3RP4/s1600/network_giant_only.png" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="465" src="http://1.bp.blogspot.com/-ysSNMv3to_M/Tg2snEHiA3I/AAAAAAAACMA/vTQAJjI3RP4/s640/network_giant_only.png" width="640" /></a></div><br /><br />Here are the network statistics:<br /><br /><table border="2" cellpadding="4"><tbody><tr> <td>Clustering coefficient</td> <td>0.88</td> </tr><tr> <td>Average degree</td> <td>8.10</td> </tr><tr> <td>Average shortest path length (giant component only)</td> <td>6.96</td> </tr><tr> <td>Maximum shortest path length</td> <td>18</td> </tr><tr> <td>Modularity (giant component only)</td> <td>0.92</td> </tr></tbody></table><br /><br />Here is the degree distribution plotted in log space, with a degree distribution for a matched random graph for comparison:<br /><br /><div class="separator" style="clear: both; text-align: center;"><a href="http://3.bp.blogspot.com/-7srigLHtuPQ/Tg38uj888YI/AAAAAAAACME/w1WMRP0UrBw/s1600/degree_distribution.png" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="240" src="http://3.bp.blogspot.com/-7srigLHtuPQ/Tg38uj888YI/AAAAAAAACME/w1WMRP0UrBw/s320/degree_distribution.png" width="320" /></a></div><br />The degree distribution has a long tail compared to the random network, which is what one would expect from this kind of network (for background on this kind of analysis, see Mark Newman's paper&nbsp;<a href="http://arxiv.org/abs/cond-mat/0007214/">The structure of scientific collaboration networks</a>). <br /><br />Using <a href="http://en.wikipedia.org/wiki/PageRank">PageRank centrality</a>, I identified the 10 most central authors in this network (listed with number of abstracts and centrality value):<br /><ol><li>Paul Thompson (33 abstracts: 0.002020)</li><li>Vince Calhoun (21 abstracts: 0.001816)</li><li>Arno Villringer (23 abstracts: 0.001756)</li><li>Arthur Toga (30 abstracts: 0.001625)</li><li>Yong He (19 abstracts: 0.001416)</li><li>Peter Fox (26 abstracts: 0.001381)</li><li>Michael Milham (24 abstracts: 0.001340)</li><li>Alan Evans (16 abstracts: 0.001318)</li><li>Robert Turner (23 abstracts: 0.001292)</li><li>Daniel Margulies (13 abstracts: 0.001194)</li></ol><b>Content analysis:</b><br /><b><br /></b><br />Using the full text from the articles, I created several tag clouds (using <a href="http://www.wordle.net/">Wordle</a>) to show different aspects of the content. &nbsp;The first was created from the entire abstract text after filtering out standard stop words along with anatomical regions and author names. <br /><br /><div class="separator" style="clear: both; text-align: center;"><a href="http://2.bp.blogspot.com/-eRbX4tZZTfg/Tg2rUXGmIXI/AAAAAAAACL0/uFRWE24-X9Q/s1600/abstract_tagcloud.png" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="348" src="http://2.bp.blogspot.com/-eRbX4tZZTfg/Tg2rUXGmIXI/AAAAAAAACL0/uFRWE24-X9Q/s640/abstract_tagcloud.png" width="640" /></a></div><br />The second was created using a count of all anatomical terms (from the PubBrain anatomical lexicon):<br /><br /><div class="separator" style="clear: both; text-align: center;"><a href="http://2.bp.blogspot.com/-ehJW42nlL3E/Tg2rl5yg5eI/AAAAAAAACL4/56YqpsIuULA/s1600/anatomy_tagcloud.png" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="292" src="http://2.bp.blogspot.com/-ehJW42nlL3E/Tg2rl5yg5eI/AAAAAAAACL4/56YqpsIuULA/s640/anatomy_tagcloud.png" width="640" /></a></div><br />The third was created using a count of all of the terms in the <a href="http://www.cognitiveatlas.org/">Cognitive Atlas </a>lexicon of mental concepts:<br /><br /><div class="separator" style="clear: both; text-align: center;"><a href="http://4.bp.blogspot.com/-u4mxX556Fng/Tg2r1tIrNsI/AAAAAAAACL8/1v3OV9rxHD4/s1600/concept_tagcloud.png" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="294" src="http://4.bp.blogspot.com/-u4mxX556Fng/Tg2r1tIrNsI/AAAAAAAACL8/1v3OV9rxHD4/s640/concept_tagcloud.png" width="640" /></a></div><br />These tag clouds give a good overview of the major topics at the meeting.<br /><b><span class="Apple-style-span" style="font-weight: normal;"><br /></span></b><br /><b><span class="Apple-style-span" style="font-weight: normal;">If you have other ideas for mining of these data, let me know and I'll give it a try. I have also done topic modeling using latent Dirichlet allocation, and may get around to writing about that in the future.</span></b>