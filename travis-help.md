# travis-help

## Publish to GitHub

### Install Travis Client
Installation on your server where the project is cloned (git clone <project>)
```sh
sudo apt-get install ruby-dev
gem install travis
```

### Create a GitHub API Token
Go to settings->developer settings->Personal access tokens (https://github.com/settings/tokens) in the GitHub UI. Create a new token, removing every permission except repo.


### Generate secure API key
See https://docs.travis-ci.com/user/encryption-keys/

```sh
cd <project dir with .travis>
/usr/local/bin/travis encrypt
Reading from stdin, press Ctrl+D when done
<My GitHub API Key>
<Ctrl+D>
Pro Tip: You can add it automatically by running with --add.
Shell completion not installed. Would you like to install it now? |y| y
Detected repository as dalijolijo/BitCore, is this correct? |yes| yes
Please add the following to your .travis.yml file:
  secure: 
  "qYnoa+ [...] hzfd" 
```
OR: You can add it to .travis automatically by running with --add
```sh
cd <project dir with .travis>
/usr/local/bin/travis encrypt --add
Shell completion not installed. Would you like to install it now? |y| y
Detected repository as dalijolijo/BitCore, is this correct? |yes| yes
Reading from stdin, press Ctrl+D when done
<My GitHub API Key>
<Ctrl+D>
```

### Modify .travis to publish binaries
See https://docs.travis-ci.com/user/deployment/releases/

#### Alternative 1:
```sh
before_deploy:
      # Set up git user name and tag this commit
      - git config --local user.name "dalijolijo"
      - git config --local user.email "<e-mail>"
      - git tag "$(date +'%Y%m%d%H%M%S')-$(git log --format=%h -1)"
deploy:
      provider: releases
      api_key: 
         secure: qYnoa+JrpVms4Cno+oOo3j+c7GOwRPnZYWNC....
      file_glob: true
      file: out/**/zip/*
      skip_cleanup: true
```

#### Alternative 2:
```sh
deploy:
      provider: releases
      api_key:
        secure: qYnoa+JrpVms4Cno+oOo3j+c7GOwRPnZYWNC....
      file_glob: true
      file: out/**/zip/*
      skip_cleanup: true
      on:
        tags: true
```
		
### Trigger deployment
Trigger build with tagging
```sh
git tag --list
git tag 0.15.1.2
git push origin --tags
 * [new tag]         0.15.1.2 -> 0.15.1.2
```

### Update tags with main project (upstream)
```sh
git remote add upstream https://github.com/whoever/whatever.git
git pull upstream --tags
```

### Delete tags
```sh
#local
git tag -d 0.15.1.10 0.15.1.11 0.15.1.12
#remote 
git push --delete origin 0.15.1.10 0.15.1.11 0.15.1.12
```
