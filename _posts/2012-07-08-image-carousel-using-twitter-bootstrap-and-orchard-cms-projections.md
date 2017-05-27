---
title: "Image Carousel using Twitter Bootstrap and Orchard CMS Projections"
tags:
  - Orchard CMS
  - Twitter Bootstrap
  - Image Carousel
---

There are a few great modules in the gallery that can aid you in the creation of an image carousel but with the invention of Projections this functionality has become really easy to implement with very little code.  If like me you're a fan of <a href="http://twitter.github.com/bootstrap/index.html" target="_blank">Twitter's Bootstrap framework</a> you'll no doubt have noticed they offer an image carousel component.  This article will show you how to bring the <a href="http://twitter.github.com/bootstrap/javascript.html#carousel" target="_blank">bootstrap carousel</a> into Orchard CMS. 

## Create a content type ##
First of all we need to create a content type for each of the images we want to show in our carousel.  This type should contain a way of choosing an image as a minimum requirement - I've opted for a mediapicker field.  I've also added a numeric field that acts as an ordering system.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/carousel-content-type.gif" alt="Add Content Type">

## Add a few images ##
Upload at least 3 images so that we can test the rotation and order and give each a different priority.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/carousel-image-add.gif" alt="Add Images">

You should now have at least 3 carousel images in your content list:

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/content-list.gif" alt="Content List">

## Create Query ##
Next thing we need to do is create a new query and call it descriptive e.g. "Carousel images for homepage". Add a new filter for the query that just brings back all carousel images that have images assigned.  If you wanted to put this carousel on different pages then giving each image another field such as group will allow you to filter based on that otherwise a custom filter that uses the url of the page would work.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/filter.gif" alt="Image Filter">

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/filter-detail.gif" alt="Image Filter Detail">

Next create a sort and chose the priority field and sort ascending.  Alternatively, you can randomise the selection by selecting the random sort order.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/sort.gif" alt="Image Sort">

The last item of a query is the layout.  By default the two layouts that are out-the-box aren't quite what we want so we need to create our own that uses the markup required from Bootstrap.

## Create a Layout Provider ##
In your theme project create a folder called Providers and then under that create a new one called Layouts. Within there copy over the gridlayout.cs, gridlayoutforms.cs and layoutshapes.cs from the projections module.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/projection-module.gif" alt="Projection Module">

Rename the files in your theme from Grid to Carousel:

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/theme-files.gif" alt="Theme Files">

Optionally, modify the CarouselLayoutForm.cs to expose the classes/properties of the carousel.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/carousel-layout-code.gif" alt="Layout Properties">

Next we add the html markup to the LayoutShapes.cs file that will determine how our carousel is rendered.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/layout-code.gif" alt="Layout Code">

And finally we need to register this new layout, so we need to edit the CarouselLayout.cs file like so:

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/register-code.gif" alt="Register Code">

Now we have our query in place we need to re-build our theme project and we should be able to see a new layout.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/layout-list.gif" alt="Layout List">

If you have exposed the html properties on then be sure to fill them in.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/properties.gif" alt="Properties">

All that's left to do now is add it to our page.  Go to widgets and create a projection widget in the you zone of choice and layer. Then select the query we just created with the new layout.  You should have a fully working image carousel - be sure to include the Bootstrap JavaScript and CSS for this to work.