# ansible_public

#### ssh-key with passphrase, with ssh-agent - borrowed from [link](https://unix.stackexchange.com/questions/90853/how-can-i-run-ssh-add-automatically-without-a-password-prompt)

Adding the following to `~/.bash_profile` will automatically start ssh-agent and load the ssh-key(s) on login:


    if [ -z "$SSH_AUTH_SOCK" ] ; then
      eval `ssh-agent -s`
      ssh-add
    fi

Now the passphrase must be entered upon every login. While slightly better from a usability perspective, this has the drawback that ssh-agent prompts for the passphrase regardless of if the key is to be used or not during the login session. Each new login also spawns a distinct ssh-agent instance which remains running with the added keys in memory even after logout, unless explicitly killed.

To kill ssh_agent on logout, add the following to `~/.bash_logout`

    if [ -n "$SSH_AUTH_SOCK" ] ; then
      eval `/usr/bin/ssh-agent -k`
    fi

or the following to `~/.bash_profile`

    trap 'test -n "$SSH_AUTH_SOCK" && eval `/usr/bin/ssh-agent -k`' 0
