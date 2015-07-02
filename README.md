# Browser Action Recommendation Dataset

This is a dataset of anonymized user activity during long-term unsupervised interaction with a web browser. The data comes from [TabRec browser extension](http://tabber.fiit.stuba.sk), which logs user activity, recommends appropriate browser actions for detected common patterns, and collects user feedback for action recommendations and pattern confirmations. The dataset is accompanying a paper: [add details]

The browser extension is open-sourced:
* [TabRec extension](https://github.com/martin-svk/tabrec)
* [TabRec server API](https://github.com/martin-svk/tabrec-api)

Usage observations cover a period of March 2015 to July 2015. The recommendation observations cover a period of April 2015 to July 2015, individual recommendations were added gradually. There are two files available, one [tab interaction indicator/browser action recommendation] observation per line, with the following variables:

----------------------------------

### usage_log.csv

#### timestamp

Timestamp of tab interaction indicator observation.

#### user

Anonymized GUID of a user.

#### session_id

Identifier of user's browsing session.

#### event

* TAB_CREATED - New tab was opened
* TAB_REMOVED - Tab was closed
* TAB_ACTIVATED - Tab was focused
* TAB_UPDATED - Tab was updated (e.g., new url was inserted)
* TAB_MOVED - Tab was moved within window
* TAB_DETACHED - Tab moved between windows (detached from old)
* TAB_ATTACHED - Tab moved between windows (attached to new)

#### tab_id

Tab identifier within user's browser.

#### window_id

Window identifier within user's browser.

#### index_from, index_to

Indices of tab movement.

#### sha_url, sha_domain, sha_subdomain, sha_path

Anonymized URL of the tab. For example, when the user is visiting `http://server1.example.org/a/resource.html?parameter=x`, the URL components are as follows:

| Component | Value |
| :-------- | :---- |
| url       | http://server1.example.org/a/resource.html?parameter=x |
| domain    | example.org |
| subdomain | server1.example.org |
| path      | /a/resource.html | 

A `sha_[url_component]` is obtained using [jshashes](https://www.npmjs.com/package/jshashes) library in a manner of `sha_component = SHA1.hex(component)`.

----------------------------------

### recommendation_log.csv

#### timestamp

Timestamp of recommendation feedback observation.

#### user

Anonymized GUID of a user.

#### detected_pattern

* MULTI_ACTIVATE - User focused four tabs in a constant time gap.
* MULTI_ACTIVATE_V2 - User focused four tabs in a constant time gap (excluding some time after accepting).	
* MULTI_ACTIVATE_V3 - User focused four tabs (excluding tabs next to each other) in his running average time gap.
* MULTI_ACTIVATE_V4 - User focused four tabs (at least three different tab ids) in thresholded running average time gap.
* COMPARE_V0 - User is swiching between two tabs (at least four activates) in thresholded running average time gap.
* REFRESH_V0 - User is monitoring (refreshing) specific tab three times in thresholded running average time gap.	
* MULTI_CLOSE_V0 - User closed four tabs consecutively (without other event interuption) in thresholded running average time gap.

#### recommendation

* TAB_DOMAIN_SORT - Will sort all opened tabs in current window by domain URLs.
* TAB_DOMAIN_SORT_V2 - Will sort all opened tabs in current window by domain URLs and wait some time after execution and dont trigger again.
* NO_ADVICE - No action is performed, confirmation prompt is displayed.

#### feedback

* ACCEPTED - Recommendation manually accepted
* AUTOMATIC - Recommendation automatically accepted (per user settings in extension)
* REJECTED - Recommendation manually rejected
* REVERTED - Recommendation previously accepted (manually or automatically) was reverted
* YES - Pattern interpretation accepted
* NO - Pattern interpretation rejected

