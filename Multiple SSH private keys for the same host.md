# Multiple SSH private keys for the same host

1. [Generating a new SSH key and adding it to the ssh-agent](https://docs.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) and [add the public key to GitHub account](https://docs.github.com/en/github/authenticating-to-github/adding-a-new-ssh-key-to-your-github-account).

   ```
   $ ssh-keygen -t ed25519 -C "your_email@youremail.com"
   
   # start the ssh-agent in the background
   $ eval `ssh-agent -s`
   
   # you can delete all cached keys before
   $ ssh-add -D
   
   # add keys as following. To add keys automatically, insert it into `~/.bashrc` file
   $ ssh-add ~/.ssh/id_ed25519
   
   # you can check your saved keys
   $ ssh-add -l
   ```
   
2. modify the ssh config file `~/.ssh/config`

   ```
   # Use project_1 or project_2 as the host to access the repository
   IgnoreUnknown UseKeychain
   Host project_1
       HostName github.com
       User git
       IdentityFile /etc/ssh/my_project_1_github_deploy_key
       AddKeysToAgent yes
       UseKeychain yes
       IdentitiesOnly yes
   Host project_2
       HostName github.com
       User git
       IdentityFile /etc/ssh/my_project_2_github_deploy_key
       AddKeysToAgent yes
       UseKeychain yes
       IdentitiesOnly yes
   ```

3. [Testing your SSH connection](https://docs.github.com/en/github/authenticating-to-github/testing-your-ssh-connection)

   ```
   $ ssh -T git@github.com
   $ ssh -T git@project_2
   ```

4. now you are all set to clone your repositories, with the host configured in `~/.ssh/config` file

   ```
   $ git clone git@project_2:org2/project2.git /path/to/project2
   ```

   or change the remote origin in the `.git/config` file after cloning

   ```
   [remote "origin"]
           url = git@project_2:org2/project2.git
   ```

   and the corresponding key will be used for different repository. 

#### PS.

   * If you are using Git Extensions, just specify OpenSSH as the ssh client in Settings.

   * If security is not your concern, specify no passphrase when create keys will save you much of time.

   * If you are using Putty, then 

     1. generate keys via putty directly (and export as OpenSSH keys if necessary), rather than import OpenSSH keys and do the conversion, which may cause issues.

     2. pay attention to the type of key to generate, e.g. RSA or Ed25519.

     3. you can debug the connection via command `plink -v git@github.com`

     

