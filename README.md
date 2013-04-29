discourse-units
===============

These are systemd units for starting `discourse` as a systemd compatible service.

The three main units are:

 * `discourse-thin` which will start three instances of the `thin` application server.

 * `discourse-sidekiq` which will start one instance of sidekiq w/ 25 workers for async tasks.

 * `discourse-clockwork` which will start one instance of clockwork for scheduled tasks.


This setup assumes you have an `http` user which is responsible for running your `discourse` instance.


Installation
============

 1. Clone this repository with `$ git clone https://github.com/drbawb/discourse-units`
 2. Copy the units to your distribution's recommended directory. (For e.g: on Arch they are located in `/etc/systemd/system/`; other popular locations are listed in the [systemd-unit manpage.](http://www.freedesktop.org/software/systemd/man/systemd.unit.html) )
 3. You can use `# systemctl enable discourse-<unit>` to run the service at boot time.

That's it! You can now use systemctl to manage the unit.

For e.g: `systemctl restart discourse-<unit>` will restart the unit, `journalctl -u discourse-<unit> -f` will display the logs for the units, etc.

Because the thin process forks: the logs are still written to your application's directory. As such you cannot manage them from systemd. The logs should be found at: `$DISCOURSE_ROOT/log/<environment>.log` 

Notes
=====

On my setup the `http` user has `rbenv` installed to manage multiple Ruby versions.

Since systemd does not setup the `http` user's environment these shims are not loaded. As such: I have hardcoded the path to the shim for my Ruby 2.0 gems. This can be seen in the ExecStart and ExecStop lines of all the units.

This will need to be changed to suit your configuration.

If you have Ruby installed system-wide, you can probably just use `bundle exec` without the fully qualified path.
Otherwise you will need to either (A) set the fully qualified path to your `Bundler` executable, or (B) source the environment variables which configure your Ruby version manager.

In addition: the path for the PIDFile will likely be wrong. -- You can set this to any writable directory.
