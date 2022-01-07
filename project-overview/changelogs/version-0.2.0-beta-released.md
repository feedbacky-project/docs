---
description: June 5, 2020
---

# Version 0.2.0-beta released

Finally we've finally released beta version 0.2.0 of Feedbacky, see what's new below.

**TL;DR:**

* Added mail notifications for ideas
* Recoded client side app, smaller size, more lightweight
* Added roadmaps feature
* SendGrid mail provider support
* Minor UI and Dark Mode changes
* Some fixes...

See raw changelog [here](https://github.com/feedbacky-project/app/blob/master/CHANGELOG.md#020-beta-june-5-2020).

### Updating from older versions

This version requires few changes in order to work. See [updating to 0.2.0](broken-reference) for more information.

### Mails Notification Feature

From now on Feedbacky users can `Subscribe` to ideas to receive mail notifications. Users who create new ideas from now will automatically subscribe to their idea.

![Mail notification example](../../.gitbook/assets/Firefox\_Screenshot\_2020-06-05T19-10-22.478Z.png)

Users can receive notifications about:

* New idea comments by moderators
* Idea state change (when idea gets closed or opened)
* Idea tags change

### Client App Recode

Feedbacky client side app was fully recoded and no longer uses Material Design Bootstrap.\
Code was recoded with functional components and was redesigned to be more lightweight.\
Overall size of app should be much lower. CSS was reduced from 211kb to 154kb.

### Roadmaps Implemented

New feature called **Roadmaps** got implemented. Roadmaps serve the purpose of informing about upcoming or features being worked on and they depend on tags ideas have.

By default every tag is visible in the roadmap but this can be edited so tags can be ignored from the roadmap view.

![New roadmap feature preview](../../.gitbook/assets/Firefox\_Screenshot\_2020-06-05T19-24-31.072Z.png)

### SendGrid Mail Provider Support

Besides Mailgun and own SMTP server you can use [Twilio SendGrid](https://sendgrid.com) to send mails.

### Minor UI and Dark Mode changes

UI and dark mode received several small changes to ensure the best experience when using Feedbacky.\
From more noticeable things Moderation comments icons were changed, board admin panel and profile page menus design got changed.

![Board admin panel menu design update (old left, new right)](<../../.gitbook/assets/ui change 1.png>)

### Bug Fixes for Stability and Other Changes!

Here's a list of things that got fixed since previous version:

* Board admin panel no longer loses theme color in navbar after page refresh
* Social Link and Webhooks creators won't break any longer when putting invalid URL value
* Our old markdown library got replaced with marked for better stability with parsing links and other markdown stuff
* Comments will now always load in good order, previously they could load second page before first if request for second page returned faster
* Board admin panel won't blink anymore when switching between sections
* Settings edited in board admin panel now reflect changes all over the board properly

And a bunch of other changes:

* Comments are now loaded from oldest to newest
* In case of app crash, error page will be displayed instead of blank one
* All dependencies of backend and frontend were updated to their latest stable versions
* Server side messages when editing or updating stuff will now return more user friendly messages on failure

![What user sees when app crashes, not a blank page anymore](../../.gitbook/assets/Firefox\_Screenshot\_2020-06-05T19-52-37.296Z.png)
