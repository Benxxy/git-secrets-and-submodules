# Usage example of git secrets and git submodules

### Working with repositories that contain submodules

###### Adding submodule to a Git repository and tracking a branch/commit

`git submodule add -b main [URL to Git repo]` - In order to add submodule and to track certain branch we execute

`git submodule add [URL to Git repo]` - If we do not want to track a branch but commits

`git submodule init` -Initialize submodule configuration

----------------
###### Cloning a repository that contains submodules

`git clone --recursive [URL to Git repo]`

`git submodule update --init --recursive` - if we already cloned repository, and we want to load its submodules then we need to update them

**Note: `--recursive` is used if there are more than one submodule**

-------------
###### Pulling with submodules
Once we have set up the submodules we can update the repository fetch/pull

`git pull --recurse-submodules` - pull all changes in the repo including changes in the submodules

`git submodule update --remote` - if we track branch in submodule we can update it with `--remote` this will fetch new commits in the submodules and update the working tree to the commit described by the branch

-------------
### Git secrets 
Git secret is a tool to encrypt files. It keeps secrets confidential and share secrets between authorized poople

Git secret relies on two dependencies `git` and `gpg`

Git secret does not support Windows so workaround would be to use Linux subsystem in Windows (WSL)

----------
First we need to install three packages:

`sudo apt-get install git gnupg git-secrets`

Then we need to create the key pair using GnuPG package

`gpg --gen-key` - Add your name and email address

Using WSL, checkout to our repository and then excecute

`git secret init` - Initialize git-secret

`git secret tell your@gpg.email` - Adding first user to git-secret

`echo "secret_file" >> .gitignore` - It is important to add file into gitignore

`git secret add secret_file` - Add secret file

`git secret hide` - Encrypt secret file

`rm secret_file` - Remove secret file not encrypted

`git secret reveal` - Decrypt secret file

---------------
Now we created the basis git-secret and to share our secret with other people every new user needs gpg key. User will send its public key to member who is
already signed to git-secret and that person can add new user. After adding new user we need to re-encrypt files

`gpg --import new_user_public_key.gpg` - Import public key of new user

`git secret tell newUser@email.address` - Adding new user to git secret

`git secret reveal`

`git secret hide -d` - -d deletes the files after they are encrypted
