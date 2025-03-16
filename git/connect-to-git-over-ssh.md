# Connect to GitHub over SSH

1. Create new SSH key with your email.

```bash
ssh-keygen -t ed25519 -C "your_email@example.com" -f ~/.ssh/zhunio-github
```

2. Create `~/.ssh/config` file (if it does not exists).

```bash
touch ~/.ssh/config
```

3. Modify `~/.ssh/config` file to automatically load keys into the ssh-agent.

```ssh
Host zhunio github.com
  HostName github.com
  AddKeysToAgent yes
  IdentityFile ~/.ssh/zhunio-github
```

4. Start ssh-agent.

```bash
eval "$(ssh-agent -s)"
```

5. Add your SSH private key to the ssh-agent

```bash
ssh-add ~/.ssh/zhunio-github
```

5. Add the `~/.ssh/zhunio-github.pub` (public key) to your account in GitHub.

6. Clone the repo using the ssh format

```bash
git clone git@github.com:Zhunio/dotfiles.git
```
