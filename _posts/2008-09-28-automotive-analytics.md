---
title: "Automotive Analytics"
tags:
  - Automotive
  - Google
  - JavaScript
  - Online Marketing
  - Analytics
---

## Automotive Analytics ##

There are a number of analytical software packages on there on the market and most of them offer an on-demand, Software-as-a-Service (SaaS) version.

For those who don't know, Google offer a free way of measuring activity on your website, called <a target="_blank" href="http://www.google.com/analytics/">Google Analytics</a>. They offer this service for free to encourage you to advertise with them via there Pay-per-Click (PPC) advertising.

The one major downside (if you see it as one), is that Google can use the data you gather to help them evolve there products and services in the future. If this doesn't bother you then I would suggest you get yourself set up.

## Event Tracking ##

By default, Google Analytics mainly tracks page views and visitor information but sometimes you need more information about what’s happening on your website. Currently in beta and therefore not fully available, Google have a new event tracking API (Application Programming Interface) that allows you to track custom events on your website. For example, you may want to know how many users have watched all of video or how many have bombed out after just a few seconds, or how many users have searched for a certain make or model from your used car search.

Even though it’s in beta and the information isn’t available in the analytics I believe that Google are still recording the data you capture if you start using the API now. This is how to implement it to track how many people are searching for each make and model.

First of all you need to make sure that you are using the latest analytics API, the JavaScript include is called ga.js not the urchin.js which is the older one. 

Now lets assume you have something like this simple search:

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/UsedSearch.jpg" alt="Used Search">

On the “Search Button” we need to add a little JavaScript to the onclick event:

<div style="border-bottom: gray 1px solid; border-left: gray 1px solid; padding-bottom: 4px; line-height: 12pt; background-color: #f4f4f4; margin: 20px 0px 10px; padding-left: 4px; width: 97.5%; padding-right: 4px; font-family: consolas, &#39;Courier New&#39;, courier, monospace; max-height: 200px; font-size: 8pt; overflow: auto; border-top: gray 1px solid; cursor: text; border-right: gray 1px solid; padding-top: 4px">   <div style="border-bottom-style: none; padding-bottom: 0px; line-height: 12pt; border-right-style: none; background-color: #f4f4f4; padding-left: 0px; width: 100%; padding-right: 0px; font-family: consolas, &#39;Courier New&#39;, courier, monospace; border-top-style: none; color: black; font-size: 8pt; border-left-style: none; overflow: visible; padding-top: 0px">     <pre style="border-bottom-style: none; padding-bottom: 0px; line-height: 12pt; border-right-style: none; background-color: white; margin: 0em; padding-left: 0px; width: 100%; padding-right: 0px; font-family: consolas, &#39;Courier New&#39;, courier, monospace; border-top-style: none; color: black; font-size: 8pt; border-left-style: none; overflow: visible; padding-top: 0px"><span style="color: #606060">   1:</span> onclick=<span style="color: #006080">&quot;pageTracker._trackEvent('Search', 'Quick Search', Make.value + '|' + Model.value);&quot;</span></pre>
  </div>
</div>

A better way of attaching this code is unobtrusively via an attachevent. Here is an example using JQuery:

<div style="border-bottom: gray 1px solid; border-left: gray 1px solid; padding-bottom: 4px; line-height: 12pt; background-color: #f4f4f4; margin: 20px 0px 10px; padding-left: 4px; width: 97.5%; padding-right: 4px; font-family: consolas, &#39;Courier New&#39;, courier, monospace; max-height: 200px; font-size: 8pt; overflow: auto; border-top: gray 1px solid; cursor: text; border-right: gray 1px solid; padding-top: 4px">
  <div style="border-bottom-style: none; padding-bottom: 0px; line-height: 12pt; border-right-style: none; background-color: #f4f4f4; padding-left: 0px; width: 100%; padding-right: 0px; font-family: consolas, &#39;Courier New&#39;, courier, monospace; border-top-style: none; color: black; font-size: 8pt; border-left-style: none; overflow: visible; padding-top: 0px">
    <pre style="border-bottom-style: none; padding-bottom: 0px; line-height: 12pt; border-right-style: none; background-color: white; margin: 0em; padding-left: 0px; width: 100%; padding-right: 0px; font-family: consolas, &#39;Courier New&#39;, courier, monospace; border-top-style: none; color: black; font-size: 8pt; border-left-style: none; overflow: visible; padding-top: 0px"><span style="color: #606060">   1:</span> $(<span style="color: #006080">&quot;#SearchButton&quot;</span>).click(<span style="color: #0000ff">function</span>()</pre>

    <pre style="border-bottom-style: none; padding-bottom: 0px; line-height: 12pt; border-right-style: none; background-color: #f4f4f4; margin: 0em; padding-left: 0px; width: 100%; padding-right: 0px; font-family: consolas, &#39;Courier New&#39;, courier, monospace; border-top-style: none; color: black; font-size: 8pt; border-left-style: none; overflow: visible; padding-top: 0px"><span style="color: #606060">   2:</span> {</pre>

    <pre style="border-bottom-style: none; padding-bottom: 0px; line-height: 12pt; border-right-style: none; background-color: white; margin: 0em; padding-left: 0px; width: 100%; padding-right: 0px; font-family: consolas, &#39;Courier New&#39;, courier, monospace; border-top-style: none; color: black; font-size: 8pt; border-left-style: none; overflow: visible; padding-top: 0px"><span style="color: #606060">   3:</span>     pageTracker._trackEvent(<span style="color: #006080">'Search'</span>, <span style="color: #006080">'Quick Search'</span>, $(<span style="color: #006080">'#Makes'</span>).val() + <span style="color: #006080">'|'</span> + $(<span style="color: #006080">'#Models'</span>).val());</pre>

    <pre style="border-bottom-style: none; padding-bottom: 0px; line-height: 12pt; border-right-style: none; background-color: #f4f4f4; margin: 0em; padding-left: 0px; width: 100%; padding-right: 0px; font-family: consolas, &#39;Courier New&#39;, courier, monospace; border-top-style: none; color: black; font-size: 8pt; border-left-style: none; overflow: visible; padding-top: 0px"><span style="color: #606060">   4:</span> });</pre>
  </div>
</div>


The above code simply call the _trackEvent function of the pageTraker object and passes in a few parameters:

 - **category (required)** - The name you supply for the group of objects you want to track 
 - **action (required)** - A string that is uniquely paired with each category, and commonly used to define the type of user interaction for the web object
 - **label (optional)** - An optional string to provide additional dimensions to the event data
 - **value (optional)** - An integer that you can use to provide numerical data about the user event
 
I've assigned a category of "Search" and the action of "Quick Search"; this allows us to categorise for advanced searches, new vehicle searches etc. I then specify the label that is the Make and Model values concatetated by a | character. e.g. Volkswagen|Golf I have omitted a value for this type of tracking.

I must note that this type of tracking isn't new and is included in the more advanced enterprise analytics packages all of which come with hefty price tags compared to the freeness of Google.&#160; In addition, this type of tracking would be easy to implement if you were to do your own custom bespoke solution.

Here is the link to the full <a target="_blank" href="http://code.google.com/apis/analytics/docs/eventTrackerGuide.html">Google documentation</a>.