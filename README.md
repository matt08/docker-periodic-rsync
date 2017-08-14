docker-periodic-rsync
=====================

***docker-periodic-rsync*** is a [*Docker*](http://www.docker.com/) image based on *Debian 8* with *cron*, *ssh*, *tar*, *wget* and [*rsync*](http://en.wikipedia.org/wiki/Rsync) for one-time or periodic remote *rsync* copy jobs.

Usage
=====

Requirements:

- setup passwordless SSH login on remote machines ([setup](http://www.tecmint.com/ssh-passwordless-login-using-ssh-keygen-in-5-easy-steps/))
- `/root/.ssh`: mount your passwordless SSH public and private keys (`id_rsa`/`id_rsa.pub`, chown to user `root`)
- `/data`: mount preferred target directory
- `/etc/cron.d`: mount your crontab file (for periodic usage)

For one-time usage (need specify command):

```bash
$ docker run -it --rm -v /srv/backup/.ssh:/root/.ssh -v /srv/backup/data:/data gw000/periodic-rsync rsync -zave ssh user@server.remote:dir/ /data
```

For periodic usage (prepare crontab file `/srv/backup/cron.d/backup`):

```
# /etc/cron.d/backup: system-wide crontab
SHELL=/bin/sh

# m h dom mon dow user  command
*/5 *   *   *   * root  rsync -zave ssh user@server.remote:dir/ /data
```

```bash
$ docker run -d -v /srv/backup/.ssh:/root/.ssh -v /srv/backup/cron.d:/etc/cron.d -v /srv/backup/data:/data --name backup gw000/periodic-rsync
```


Feedback
========

If you encounter any bugs or have feature requests, please file them in the [issue tracker](https://github.com/matt08/docker-periodic-rsync/issues/) or even develop it yourself and submit a pull request over [GitHub](https://github.com/matt08/docker-periodic-rsync/).


License
=======

Copyright &copy; 2016 *gw0* [<http://gw.tnode.com/>] &lt;<gw.2016@tnode.com>&gt;

This library is licensed under the [GNU Affero General Public License 3.0+](LICENSE_AGPL-3.0.txt) (AGPL-3.0+). Note that it is mandatory to make all modifications and complete source code of this library publicly available to any user.
