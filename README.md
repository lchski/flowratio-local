# FlowRatio Tracker

I've long considered Tippett's snap reading a superior time tracking
methodology, but it comes with some practical difficulties. Daniel Reeves
[convinced me those are no big deal](https://messymatters.com/tagtime).
Unfortunately, while Reeves' implementation is technically cross-platform, I
felt like I could spend some time getting it going on Windows.

So I figured how hard can it be to write a pure JavaScript version that runs
anywhere? Well, I'm not there yet, so we'll see.

Here are some other implementations of Reeves' original concept:
http://doc.beeminder.com/tagtime

## Running

Open the HTML file in a browser and it should work.

If using it for development and you want faster feedback, add ?dev=1 to the URL
to (a) get much more aggressive time constants, and (b) have it clear out local
storage on startup.

## Todo


### Features

#### Bare minimum

I think these are needed to reach the dangerous dogfooding point.

- [x] Show notification when pinging
- [x] Randomise interval between pings
- [x] Export to JSON
- [ ] Change export to CSV
- [ ] Autodetect when running again after sleep, closed tab, etc. Add "off" tagged
  Off means flowratio was off, not anything else. This can be accomplished
  practically by storing the time for the next ping in localstorage and then if
  now is singificantly greater than nextping, it means the ping happened when
  flowratio was unable to bug the user, i.e. "off"
- [x] Set sane default times
- [x] Stop clearing localstorage automatically

#### Nice-to-haves

- [ ] Import from CSV
- [ ] Summarise the data at least a little bit
- [ ] Autocomplete when typing

#### Advanced features unlikely to happen

- [ ] Give user ability to clear out log (or otherwise clean it up?) – the user
      can do this by means of export/import

### Apparent bugs

- [x] When AFK, it gets into an infinite loop somehow
- [ ] Notification disappears after a brief while, despite requireInteraction:
      true – why is that? I'd prefer it to time out with the afk auto-submission.

### Latent bugs

- [ ] Set some sort of mutex in local storage that prevents two flowratio tabs
      from being active at the same time. Maybe just have a random id generated
      on page load, and store that as the active tab in localstorage. Each
      second, read from localstorage and ensure this tab is still active.
      Otherwise blank out and ask user to refresh to reset to this tab.
- [ ] What happens if one desires to adjust lambda while using flowratio? Maybe
      the lambda rate needs to be stored along with the ping time everywhere.
      (That lets us translate the ping into "1/lambda minutes of work")

### Fun refactoring

- [ ] Convert from oldschool setTimeout etc to moderner async/await code?
