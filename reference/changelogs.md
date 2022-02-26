---
description: Changelogs for legacy versions.
---

# Older Releases

{% hint style="info" %}
<mark style="color:blue;">**Deprecation notice**</mark>

This section will be preserved for historical purposes but will no longer be updated with current Feedbacky releases.

You can follow up to date [changelogs](https://app.feedbacky.net/b/feedbacky-official/changelog) on our official board.
{% endhint %}

## Version `0.5.0-beta`

**TL;DR** - See full [**changelog**](https://github.com/feedbacky-project/app/blob/master/CHANGELOG.md#020-beta-june-5-2020).

* User suspension
* Letter avatars
* Comment on closed ideas

## Version `0.3.1-beta`

**TL;DR** - See full [**changelog**](https://github.com/feedbacky-project/app/blob/master/CHANGELOG.md#031-beta-july-6-2020).

* Added slugs to ideas

## Version `0.3.0-beta`

This version was Initially planned to be released as 0.2.1 but since we removed Private Boards it should be marked as a bigger release.

**TL;DR** - See full [**changelog**](https://github.com/feedbacky-project/app/blob/master/CHANGELOG.md#030-beta-june-9-2020).

* Removed private boards
* UI improvements

### The removal of Private Boards

Private Boards feature was removed due to being unpopular and unused feature and contained security exploit. This version also aims to fix regressions caused by previous update.

## Version `0.2.0-beta`

**TL;DR** - See full [**changelog**](https://github.com/feedbacky-project/app/blob/master/CHANGELOG.md#020-beta-june-5-2020).

* Added mail notifications for ideas
* Re-coded client side app, smaller size, more lightweight
* Added roadmaps feature
* SendGrid mail provider support
* Minor UI and Dark Mode changes

### Mails Notification Feature

From now on Feedbacky users can `Subscribe` to ideas to receive mail notifications. Users who create new ideas from now will automatically subscribe to their idea.

![Mail notification example](../.gitbook/assets/0.2.0\_1.png)

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

![New roadmap feature preview](../.gitbook/assets/0.2.0\_2.png)

### SendGrid Mail Provider Support

Besides Mailgun and own SMTP server you can use [Twilio SendGrid](https://sendgrid.com) to send mails.

### Minor UI and Dark Mode changes

UI and dark mode received several small changes to ensure the best experience when using Feedbacky.\
From more noticeable things Moderation comments icons were changed, board admin panel and profile page menus design got changed.

![Board admin panel menu design update (old left, new right)](../.gitbook/assets/0.2.0\_3.png)

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

![What user sees when app crashes, not a blank page anymore](../.gitbook/assets/0.2.0\_4.png)
