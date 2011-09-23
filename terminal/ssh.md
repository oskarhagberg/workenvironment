# SSH

## multiple ssh private keys

In quite a few situations its preferred to have ssh keys dedicated for a service or a specific role. Eg. a key to use for home / fun stuff and another one to use for Work things, and another one for Version Control access etc. Creating the keys is simple, just use

    ssh-keygen -t rsa -f ~/.ssh/id_rsa.work -C "Key for Word stuff"

Use different file names for each key. Lets assume that there are 2 keys, `~/.ssh/id_rsa.work` and `~/.ssh/id_rsa.misc`. The simple way of making sure each of the keys works all the time is to now create config file for ssh:

    touch ~/.ssh/config
    chmod 600 ~/.ssh/config
    echo "IdentityFile ~/.ssh/id_rsa.work" >> ~/.ssh/config
    echo "IdentityFile ~/.ssh/id_rsa.misc" >> ~/.ssh/config

This would make sure that both the keys are always used whenever ssh makes a connection. However, ssh config lets you get down to a much finer level of control on keys and other per-connection setups. And I recommend, if you are able to, to use a key selection based on the Hostname. My `~/.ssh/config` looks like this :

<pre><code>
Host *.home.lan
  IdentityFile ~/.ssh/id_dsa.home
  User kbsingh

Host *.vpn
  IdentityFile ~/.ssh/id_rsa.work
  User karanbir
  Port 44787

Host *.d0.karan.org
  IdentityFile ~/.ssh/id_rsa.d0
  User admin
  Port 21871
</code></pre>

Of course, if I am connecting to a remote host that does not match any of these selections, ssh will default back to checking for and using the *usual* key, `~/.ssh/id_dsa` or `~/.ssh/id_rsa`.