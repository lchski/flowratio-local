# FlowRatio Tracker

A work sampling based time tracker. That runs in the browser with no additional
anything. Try it out: https://xkqr.org/flowratio/

Here's some background: https://two-wrongs.com/flowratio-stochastic-time-tracking.html

## Usage

Open the web page, and allow notifications. If it says "PENDING", go do whatever
you were going to do next. At some point in the future, you'll get a
notification and it will ask you about what you are doing. Write down a short
keyword that indicates what you are doing RIGHT NOW.

If you want to structure your keywords, you can use a colon to separate a
subcategory from a main category, like "work:docs" for writing documentation at
work.

Once you've done this a few times, you can click on report to see how you spend
your time.

### Tips

You don't need to track everything you do in great detail! If you're curious
e.g. how much time you spend in meetings, you can just tag "meetings" and
"other" for a few weeks and you'll find out. Or if you want to increase the
amount of time you spend on writing documentation, you can just tag "docs" and
"other" for a while and try to make the number go up.

## Development

If you want to test your changes, open index.html in a browser and it should
work. Maybe you have to go through a separate web server like `python -m
http.server 8001`.

In development you might want faster feedback. Add ?dev=1 to the URL to get much
more aggressive time constants. If you want to clear out the data because you
have toyed around, load it with ?clear=1 – but remember to remove that before
you refresh in case you collect data you want to keep.

## Todo


### Features, Analytics Application

- [x] Percent of time spent on each high-level category with error bars
- [x] Total time spent on each thing?, with error bars
- [ ] Timespan selector for above display – this takes figuring out how to
      re-render the flame graph without breaking everything
- [x] Allow selecting a category and the above happens for only entries in that
  - Even cooler would be a flamegraph – can I get that to happen? Could a
    flamegraph sort of supersede everything else I want to do? I think it
    might... EXCEPT it doesn't have error bars! How do I get error bars on that?
    Or should I indicate insecurity some other way?
- [x] Convert displays to pretty graphical plots
- [x] Migrate history display to analytics
- [x] Won't do: migrate CSV export/import to analytics. The reason is that I
   want analytics to be read-only, and the main application is the only thing that
   writes to the data.

### Bugs, Analytics Application

- [ ] Discoverability of the colon syntax is nonexistent

### Features, Main Application

I think these are the bare minimum needed to reach the dangerous dogfooding point.

- [x] Show notification when sampling
- [x] Randomise interval between samples
- [x] Export to JSON
- [x] Change export to CSV
- [x] Autodetect when running again after sleep, closed tab, etc. Add "off" tagged
  Off means flowratio was off, not anything else. This can be accomplished
  practically by storing the time for the next sample in localstorage and then if
  now is singificantly greater than nextping, it means the sample happened when
  flowratio was unable to bug the user, i.e. "off"
  - Autodetection in place, remains to be tested
- [x] Set sane default times
- [x] Stop clearing localstorage automatically

There are also some nice-to-haves.

- [x] Import from CSV
- [ ] Autocomplete when typing
- [ ] Nice design

I can sense at least one advanced feature unlikely to happen.

- [x] Allow user to edit the recent history of tags directly in the application.
      The user can do things like fix spelling errors by export/import.
      - But aaaactually, I fairly often get pings when I'm presenting things in
        meetings and it would be really convenient to be able to change the afk
        to a "work:meeting" tag after, so I'll add this feature anyway.
- [ ] Give user ability to clear out log– the user can do this by means of
      export/import.

### Bugs, Main Application

These bugs are apparent.

- [x] When AFK, it gets into an infinite loop somehow
- [x] Fan speed goes up when using – caught in rerendering loop or just tired
      computer?
  - I'm going to chalk this up to tired computer because I cannot see much CPU
    usage in about:performance nor in the profiler
- [x] Notification disappears after a brief while, despite requireInteraction:
      true – why is that? I'd prefer it to time out with the afk
      auto-submission.
  - I think this might just be the GNOME notification system, because on
    Windows it persists until the afk auto-submission.
- [x] Subsequent prompts triggered when afk should be auto-tagged afk, not
      flowratio. That said, this is probably a low-prio bug since under
      realistic settings it will rarely happen. (But it does happen all the time
      under development, so it's easy to reproduce!)
- [ ] When my Windows laptop is closed, FlowRatio can generate both "off" and
      "afk" events, sometimes at the same time. Something something modern
      standby and how do I prevent it?
- [ ] When my Windows laptop is closed, FlowRatio seems to generate an excessive
      amount of pings, on average every 17 minutes. Both of these bugs seem to
      happen also on Linux.

These bugs are latent.

- [ ] Set some sort of mutex in local storage that prevents two flowratio tabs
      from being active at the same time. Maybe just have a random id generated
      on page load, and store that as the active tab in localstorage. Each
      second, read from localstorage and ensure this tab is still active.
      Otherwise blank out and ask user to refresh to reset to this tab.
- [x] What happens if one desires to adjust lambda while using flowratio? Maybe
      the lambda rate needs to be stored along with the sample time everywhere.
      (That lets us translate the sample into "1/lambda minutes of work")
- [ ] If flowratio has been off for a long time it will sit in a blocking loop
      to generate all the "off" samples. It might be possible to generate a single
      "off" sample with a fat lambda instead. How big of a problem is it? Someone
      starting up flowratio after a year with a lambda of 45 minutes will have
      12,000 "off" samples generated. I'm going to say that this is not going to
      be a problem for at least six months.
- [ ] Changes to currentTime re-renders the entire application. With small
      history a-re-render-a-second is fine but I should break the time-dependent
      parts out from the ones that don't care about time. And/or memoize the
      history so at least not that re-renders every second but only when it
      changes.

This is a fun refactoring project I might play with at some point.

- [ ] Convert from oldschool setTimeout etc to moderner async/await code?
