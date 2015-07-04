# systemd-alarm
A systemd unit that turns your computer into a overly energy-intensive alarm-clock.

# Installation & configuration
As a regular user, put the following files in `~/.config/systemd/user` and make the following ajustments to each of them.

#### alarmclock.service
* Make sure the path to `alarmclock` in the following line matches the actual location of the script.
```
ExecStart=/usr/bin/timeout 45m /home/user/.config/systemd/user/alarmclock
```

#### alarmclock.timer
* Make sure the OnCalendar option matches the time and schedule at which you want your computer to wake up.

#### alarmclock
* Run `$ pactl list short sinks` and copy the name of the audio sink you want your alarm sound to play (ideally not headphones). Replace the value of `sinkName` by that name.
* Replace the value of `sound` by the sound clip you want to use as an alarm.
* Try running the script manually once to make sure it works properly.


# Utilisation
* To enable the alarm, simply type (as a regular user) `systemctl --user enable alarmclock.timer`.
* You can then verify that the timer has been set properly by ensuring it's listed in the output of `systemctl --user list-timers`.
* Once the alarm starts, it can be stopped with a `systemctl --user stop alarmclock`.
* Don't forget to run `$systemctl --user daemon-reload` after any changes to alarmclock.timer and alarmclock.service to make sure they've been registered properly.

# Notes
* This whole setup is loosely based on [Joey Hess' guide on the matter](http://joeyh.name/blog/entry/a_programmable_alarm_clock_using_systemd/). Many thanks to him for putting that guide together.
* This repository mostly consists of a bash script and two systemd unit files. Naturally, it will not work without systemd. The script also uses pulseaudio (and `pactl`) for sound control and `ffplay` for audio decoding.
