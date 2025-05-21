<a href="https://git.aio.sh/tuff/spotify-badge">
<img src="https://spotify.aio.sh/api/now-playing.svg" width="540" height="52">
</a>

<a href="https://github.com/larsassink">
<img src="https://github-readme-stats.vercel.app/api?username=larsassink&show_icons=true&theme=github_dark&hide_border=true&cache_seconds=3600">
</a>


---

# Killing Zscaler on macOS

Zscaler can be annoying if you're trying to stop it. Despite having administrative rights, usually it asks for a password.

Pick one of the following options to take back control.


## Using a Shell Script

- Open Terminal or whatever terminal you prefer (e.g. [iTerm2](https://iterm2.com/)).
- Type `git clone https://github.com/bkahlert/kill-zscaler.git`
- Type `cd kill-zscaler` to change into the newly cloned repository.
- Make sure the scripts are executable by running `chmod +x kill-zscaler.sh start-zscaler.sh`
- Type `./kill-zscaler.sh` to kill Zscaler.
- To use Zscaler again, reboot or type `./start-zscaler.sh`.

## Using a Shell

- Open Terminal or whatever terminal you prefer (e.g. [iTerm2](https://iterm2.com/)).
- Type `find /Library/LaunchAgents -name '*zscaler*' -exec launchctl unload {} \;;sudo find /Library/LaunchDaemons -name '*zscaler*' -exec launchctl unload {} \;`
to kill Zscaler.
- To use Zscaler again, reboot or
  type `open -a /Applications/Zscaler/Zscaler.app --hide; sudo find /Library/LaunchDaemons -name '*zscaler*' -exec launchctl load {} \;`.

### Using an Alias

To kill Zscaler by typing `kill-zscaler` (and to start it with `start-zscaler`) do the following steps:

- Open the shell initialization file of your shell
  - Bash: ~/.bashrc
  - ZSH: ~/.zshrc
  - For more information aliases, read https://medium.com/@rajsek/zsh-bash-startup-files-loading-order-bashrc-zshrc-etc-e30045652f2e or any other appropriate Google
    match.
- Add the contents of `kill-zscaler.alias.txt` or the following lines to it:
  ```shell
  alias start-zscaler="open -a /Applications/Zscaler/Zscaler.app --hide; sudo find /Library/LaunchDaemons -name '*zscaler*' -exec launchctl load {} \;"
  alias kill-zscaler="find /Library/LaunchAgents -name '*zscaler*' -exec launchctl unload {} \;;sudo find /Library/LaunchDaemons -name '*zscaler*' -exec launchctl unload {} \;"
  ```
- Open a new shell (or type `source ~/.bashrc` / `source ~/.zshrc` / … to load your changes)
- Type `kill-zscaler` to kill Zscaler
- To use Zscaler again, reboot or type `start-zscaler`.


# Sharing Zscaler

To share an existing Zscaler VPN tunnel you can use [share-zscaler.v2.sh](share-zscaler.v2.sh) on the machine
with Zscaler installed as follows:
```shell
./share-zscaler.sh \
  --probe foo.bar.internal \
  --domain internal
```

- The script sets up network address translation (NAT) on the VPN client machine
  so that its VPN tunnel can be shared.
  - The `--prope` argument can be any hostname you want to connect to using the VPN tunnel.
    It's used to determine the connection details of your VPN connection.
  - The domains specified with one or more `--domain` arguments are used to
    customize the DNS name resolution on your host.
    This makes your host use your VPN client's name resolution for the specified domains (and sub-domains).
- It prints a configuration script that needs to be run on your host to make it use the just shared tunnel. 

If you prefer to have a one-liner without having to download anything you can use the following
command *at your own risk*:

```shell
bash -c "$(curl -so- https://raw.githubusercontent.com/bkahlert/kill-zscaler/main/share-zscaler.v2.sh)" -- \
  --probe foo.bar.internal \
  --domain internal
```
