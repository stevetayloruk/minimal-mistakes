---
title: "iFit Schedule Workout Fix for Shared Maps"
tags:
  - iFit
  - Bookmarklet
---

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/ifit-workout.jpg" alt="ifit schedule workout link">

If you are a regular user of the [ifit][1] website you may have noticed that recently they have given the website a make over.  As part of this revamp, they have taken away some key functionality - the schedule workout link from the workout details page.  This has angered many customers and their frustration has been aired on the [Zendesk support page][2].  Even though these comments have been acknowledged, nothing has happened.

I looked at the code and found that the functionality exists but all that's missing is the link that fires the action.  So, I've create a bookmarklet that re-adds the Schedule Workout link back to the page.  You can install it by dragging the link below to your bookmarks/favourites bar. 

<a href="javascript:(function(){javascript:(function(){var e={userId:window.user._id,workoutId:window.location.href.split('/')[4],scheduleDate:new Date};$('.links').append('<div class=&quot;link&quot;><a href=&quot;#&quot;>Schedule Workout</a></div>').on('click',function(){ifit.user.calendar.createEvent(e,function(e){if(e.success){location.href='https://www.ifit.com/me/schedule/list'}else{alert('Error adding workout to schedule')}})})})()})();">Add Schedule Workout Link</a>

Once installed, go to the [iFit Maps][3] website and find a workout.  Once on the workout details page, click the bookmark and the Schedule Workout link will appear at the top right of the map.  click the link to add the workout to your schedule.




  [1]: http://www.ifit.com
  [2]: https://ifit.zendesk.com/entries/21490279-Share-maps-workouts-among-users
  [3]: http://www.ifitmaps.com/