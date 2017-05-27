---
title: "Using Team Foundation Server (TFS) and Orchard CMS"
tags:
  - Orchard CMS
  - Team Foundation Server
  - Source Control
---

Orchard CMS works really well with TortoiseHg as detailed in the <a href="http://docs.orchardproject.net/Documentation/Setting-up-a-source-enlistment" target="_blank">documentation</a>, but if you use Team Foundation Server then there are a few hoops you need to jump through to get it to play nicely. You may be tempted to just right click on the solution and click "Add Solution to Source Control", but this will cause you a few issues - especially if you work in a team and have predefined modules as part of a recipe. If you follow this step by step you should be fine and other team members are able to get all the third party DLL's too.

 1. Create Team Project from Team Explorer
 2. Map project to local path
 3. Manually add files in Source Control Explorer
 4. Include all DLL’s from "lib" folder
 5. Open solution from Source Control Explorer and bind files
 6. Check-in all files
 7. Make all bin folders writeable
 8. Make App_Data folder writeable