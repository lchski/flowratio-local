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

Open the HTML file in a browser and it should work. But warning: the constants
specifying how often to bug the user and how quickly to consider them AFK are
currently very aggressive, to facilitate testing.

Also it currently clears out the local storage every time it starts up (for the
same reason) so don't rely on it to keep data for you yet.

## Todo


### Features

#### Bare minimum

I think these are needed to reach the dangerous dogfooding point.

- [x] Show notification when pinging
- [ ] Randomise interval between pings
- [ ] Import/export targeting CSV
- [ ] Autodetect when running again after sleep, closed tab, etc. Add "off" tagged
   (practically, if the time since the last ping is "long", add T/n "off"
   samples.) Off means flowratio was off, not anything else.
- [ ] Set sane default times
- [ ] Stop clearing localstorage automatically

#### Nice-to-haves

- [ ] Summarise the data at least a little bit
- [ ] Autocomplete when typing

#### Advanced features unlikely to happen

- [ ] Give user ability to clear out log (or otherwise clean it up?) – the user
      can do this by means of export/import

### Apparent bugs

- [ ] Notification disappears after a brief while, despite requireInteraction:
      true – why is that? I'd prefer it to time out with the afk auto-submission.

### Latent bugs

- [ ] Set some sort of mutex in local storage that prevents two flowratio tabs
      from being active at the same time. Maybe just have a random id generated
      on page load, and store that as the active tab in localstorage. Each
      second, read from localstorage and ensure this tab is still active.
      Otherwise blank out and ask user to refresh to reset to this tab.

### Fun refactoring

- [ ] Convert from oldschool setTimeout etc to moderner async/await code?
