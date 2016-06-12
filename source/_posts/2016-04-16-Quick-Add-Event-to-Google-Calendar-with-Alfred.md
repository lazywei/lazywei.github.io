title: Quick Add Event to Google Calendar with Alfred
tags:
  - productivity
  - os x
  - tools
date: 2016-04-16 23:17:14
---


{% fullimage "/img/2016/4/16-alfred-with-google-calendar.png", "Alfred with Google Calendar Quick Add", "Alfred with Google Calendar Quick Add" %}

Here is a quick tip for integrating Alfred with Google Calendar.
Go to Alfred 2's Preferences, and then go to the 'Web Search' under 'Features'. Click the 'Add Custom Search' on the bottom right.

In the 'Search URL' input box, enter the following URL:

```
http://www.google.com/calendar/event?ctext=+{query}+&action=TEMPLATE&pprop=HowCreated%3AQUICKA
```

Next, set the proper keyword, and then you're ready to roll!
