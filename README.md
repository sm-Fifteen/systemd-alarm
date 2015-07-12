# systemd-alarm
A systemd unit that turns your computer into a overly energy-intensive alarm-clock.

# Installation & configuration
As a regular user, put the content of this repository in `~/.config/systemd/user` and make the following ajustments to each of them.

#### alarmclock.timer
* Make sure the OnCalendar option matches the time and schedule at which you want your computer to wake up.

#### start-alarmclock
* Run `$ pactl list short sinks` and copy the name of the audio sink you want to handle your alarm sound (ideally not headphones). Replace the value of `sinkName` by that name.
* Set the value of `soundClipPath` by the **absolute** path of sound clip you want to use as an alarm.
* Set `initialVolume` to the volume your alarm should start playing at in percents (e.g : "25%").
* If you want your alarm's volume to gradually increase, set `volumeSteps`, `volumeStepIncrease` and `volumeStepDuration` to match how many times, by how much (e.g : "+5%") and and the interval between each bump (e.g : "10s"), respectively.
* If you *don't* want your alarm's volume to increase at all, you can also comment the aforementionned variables.
* If you want your alarm to stop running if it's not stopped manually, just set `alarmTimeout` to the maximum duration in seconds. Alternatively, you can use the 'm', 'h' and 'd' suffix to specify that duration minutes, hours or days instead (e.g : "1h").
* You can also comment `alarmTimeout` if you want your alarm to keep playing indefinitely until stopped manually.
* Try running the script manually once you're done, to make sure it works properly.

# Usage
* To enable the alarm, simply type (as a regular user) `systemctl --user enable alarmclock.timer`.
* You can then verify that the timer has been set properly by ensuring it's listed in the output of `systemctl --user list-timers`.
* Once the alarm starts, it can be stopped with a `systemctl --user stop alarmclock`. Feel free to create an alias for that command.
* Don't forget to run `$systemctl --user daemon-reload` after any changes to alarmclock.timer and alarmclock.service to make sure they've been registered properly.

# Notes
* This whole setup is loosely based on [Joey Hess' guide on the matter](http://joeyh.name/blog/entry/a_programmable_alarm_clock_using_systemd/). Many thanks to him for putting that guide together.
* This repository mostly consists of a bash script and two systemd unit files. Naturally, it will not work without systemd. The script also uses pulseaudio (and `pactl`) for sound control and `ffplay` for audio decoding.
