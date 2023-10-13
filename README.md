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

## Todo

- [ ] Show notification when pinging
- [ ] Autodetect when running again after sleep
- [ ] Autodetect when starting again tab has been closed for a long time (and do
      what? Ask user to fill in afk or ignore?)
- [ ] Randomise interval between pings
- [ ] Summarise the data at least a little bit
- [ ] Give user ability to clear out log
- [ ] Autocomplete when typing
