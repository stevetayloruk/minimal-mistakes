---
title: "User Journeys for the New Orchard Website"
tags:
  - CMS
  - Orchard CMS
  - Orchardproject.net
---

User journeys are actions our users (see our [personas][1]) take to achieve their goals. The primary user goals have already been mapped to the [project requirements][2] so we can take those and map out the journeys. I find it good practice to look at the existing user journeys on the current site and then look at how these can be improved. It's not feasible to map out every possible user journey as there could be thousands of scenarios, so it's best to focus on the priority requirements for each of our target users.

## Tools and Approach ##
As with most other planning documents, its not the actual deliverable but the thinking that goes into creating the document(s) that is valuable. Documents are great when sharing with other stakeholders but often need explaining and walking through and therefore I tend not to spend too much time on the presentation of the documents (who has time for that?). I tend to use good old fashioned pencil and paper when working solo but if I need to share with other people, then I tend to use Paper by fiftythree (<a href="https://paper.fiftythree.com/11053339-Steve-Taylor" target="_blank">You can follow me here</a>) on an iPad Pro with an Apple Pencil or Microsoft Visio for more professional style. 

With user journeys, there are many different pieces of documentation that can be produced but I tend to stick to just two for most of the time. These are:

 - User Journeys
 - Task Models

**User Journeys** tend to take a more holistic view of the user journey and can take into account other channels and offline journeys e.g. visiting a retail store for an ecommerce site. For our needs though, we only need to concentrate on the journey through the website or possibly from a search engine.

**Task Models** are finer grained with a view on a specific task in mind. For example, a checkout process. I tend to use task models for feature based experiences or if I just want to map out a micro journey.

NB: I use the term "User Journey" in a generic sense throughout this post and covers user journeys and task models.

## User Journeys ##

Here are the primary user journeys we are going to cover in this post:

 1. Try Orchard (All Users)
 2. Download Orchard (Creators)
 3. View Resources (Creators & End Users)
 4. Product Reassurance (Researchers)


## 1. Try Orchard (All Users) ##
Based on all the research done to date it's becoming obvious that the the website's primary need is to appeal to users that are "new" to Orchard. Therefore, the ability to quickly try and evaluate Orchard is a must. Even though this is likely to fall into the End Users category of our audience, it could well cover all our target users, which puts this journey really high on the list of things that the new website should prioritise for. In the following journey, we take the place of a creator, in this case an ASP.NET developer that is looking for CMS’s with the aim to adopt one for their latest project. Due to the amount of CMS’s on the market they are looking to quickly evaluate 2 or 3 CMS’s and compare.

**Current User Journey on orchardproject.net**

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/Orchard-Website---User-Journeys.jpg" alt="Current User Journey">
<a href="{{ site.url }}{{ site.baseurl }}/assets/downloads/Orchard-Website-User-journeys.pdf" target="_blank" >Download the PDF</a>

As you can see from the current user journey, the user is having to go through numerous steps just to try Orchard out. This is mainly around the lack of signposting to TryOrchard and DotNest services and therefore a reliance on downloading code. 

Depending on the user's ability and connection etc. it could take 5-15 minutes to get Orchard into a state to evaluate. This is obviously not an ideal user experience and we must fix this on the new website. The steps needed, require a level of technical know-how and would rule out a large proportion of our target audience. 

Another big reason to promote a 'try' option is that if a user is visiting the site on a mobile/tablet/xbox/etc. then they can't download and compile the code, so a 'try' option would cater for them too.

**Proposed User Journey**

As mentioned previously, the website needs to cater for 'new' Orchard users and as such, needs to promote try over download. This doesn't mean we hide the download away as we still have a large proportion of users that will want to spend the time to evaluate in-depth - we just make the download a secondary CTA. 

If we had a simple and easy form (email, password), this could post to DotNest in a new browser tab and provision a free trail account. What could also be nice is that the default content of the newly provisioned site contains useful infomation such as a getting started guide, case studies etc. 

It's important that the journey doesn't end when the user is directed to DotNest and so this boilerplate content can help, facilitate or convince the user that Orchard is right for them. A follow up email can be automatically sent after a period of time to provide guidance or support - most probably via a CRM such as Microsoft Dynamics. This is how the new user journey could look:

<a href="https://paper.fiftythree.com/11053339-Steve-Taylor/13045592" target="_blank"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/new-journey-1-try-orchard.png" alt="New Journey 1 - Try Orchard"></a>

## 2. Download Orchard (Creators) ##

Trying out Orchard frontend/backend is not going to cut it for everyone that wants to evaluate Orchard. There are some users that still want to get down and dirty with the code, either after they have tried it or straight away. 

**Current User Journey on orchardproject.net**

The current download journey isn't too bad. There are clear links on the homepage and there is a dedicated section on the primary navigation. The problem comes when entering the site from a page that isn't the homepage or educating the user on which download is best.

<a href="https://paper.fiftythree.com/11053339-Steve-Taylor/13045586" target="_blank"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/existing-journey-2-download-orchard.png" alt="Existing Journey 2 - Download Orchard"></a>

**Proposed User Journey**

There is no need to map a new journey for this scenario. All we need to do is to make the download available throughout the website (most probably the header) and on a dedicated page explaining to the user which is the best option for them. It may be even worth combining Download and Try options on the same page for when users land directly on that download page - as this page could be linked to quite often.

## 3. View Resources (Creators & End Users) ##

The resources are from the community and hosted on their websites/blogs. Currently, the Orchard website lists them in a feed along with some teaser information. I think this is a good idea as it encourages the community to contribute and get recognised for their effort. The downsides are that this list is not searchable or filterable. The tags don't link to a filter list and there is no categorisation. This means that the user needs to paginate through the feed to find all the posts that are relevant. For example, the following journey shows a user looking for all articles related to module development.

**Current User Journey on orchardproject.net**

<a href="https://paper.fiftythree.com/11053339-Steve-Taylor/13045596" target="_blank"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/existing-journey-3-view-resources.png" alt="Existing Journey 3 - View Resources"></a>

**Proposed User Journey**

Keeping the feed is still a good, but giving it it's own section of the site and providing search and filter tools will help users find what they are looking for. In addition, each article teaser can have related articles which aids discovery. It may well be beneficial to index parts of the documentation too - such as the guides.

<a href="https://paper.fiftythree.com/11053339-Steve-Taylor/13045601" target="_blank"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/new-journey-3-view-resources.png" alt="New Journey 3 - View Resources"></a>

## 4. Product Reassurance (Researchers) ##

Initially for most users they want to know and understand what Orchard is and what it can offer along with reassurances that not only it can deliver on the user's requirements, but it's a product that people use and isn't going to die overnight. 

**Current User Journey on orchardproject.net**

Currently there is no on-site presence of example sites, except for website links that are shown in the notes of the podcast articles. 

ShowOrchard is a community site that showcases sites that are built on Orchard and has a small set of interviews, but is not massively promoted or signposted on the Orchard website - it's just a small logo on the homepage.

<a href="https://paper.fiftythree.com/11053339-Steve-Taylor/13045589" target="_blank"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/existing-journey-4-product-reassurance.png" alt="Existing Journey 4 - Product Reassurance"></a>

**Proposed User Journey**

The Orchard website needs to show some key case studies along with accompanying testimonials and just like the article feed, if the user wants to explore more, then we pass them through to the ShowOrchard website. If possible, it would be good to enrich the content on the ShowOrchard website with case studies.

<a href="https://paper.fiftythree.com/11053339-Steve-Taylor/13047779" target="_blank"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/new-journey-4-product-reassurance.png" alt="New Journey 4 - Product Reassurance"></a>

## Summary & Next Steps ##
Now we've mapped out the key user journeys we have identified inefficiencies and failings with the current website in how it caters for our target users. Without these journeys we couldn't look at how these problems can be addressed in the Information Architecture(IA) of the site; which is the next step. 

 1. <a href="/the-new-orchard-cms-website">Objectives & Audience</a>
 2. <a href="/orchard-cms-website-audit">Orchard CMS Website Audit</a>
 3. <a href="/comparing-cms-vendor-websites">Comparing CMS Vendor Websites</a>
 4. <a href="/humanising-our-audience-with-personas">Humanising our Audience with Personas</a>
 5. <a href="/project-requirements-for-the-new-orchard-website">Project Requirements for the New Orchard Website</a> 
 6. User Journeys for the New Orchard Website (this post)
 7. <a href="/information-architecture-for-the-new-orchard-website">Information Architecture for the New Orchard Website</a> 

  [1]: /humanising-our-audience-with-personas
  [2]: /project-requirements-for-the-new-orchard-website
