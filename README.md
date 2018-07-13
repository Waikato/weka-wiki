# Weka Wiki

Not really a wiki, but replacing some of the content that was hosted on
weka.wikispaces.com before it shut down. Uses [mkdocs](http://www.mkdocs.org/) 
to generate the content.

The wiki can be found at this location:

https://waikato.github.io/weka-wiki/

**Please note, this is still work in progress!**


## Installation

*mkdocs* works with Python 2.7 and 3.x.

Best approach is to install mkdocs (>= 0.16.0) in a virtual environment 
(`venv` directory):

* Python 2.7

  ```
  virtualenv -p /usr/bin/python2.7 venv
  ```

* Python 3.6 (Python 3.5 works as well)

  ```
  virtualenv -p /usr/bin/python3.6 venv
  ```

* Install the mkdocs package

  ```
  ./venv/bin/pip install mkdocs
  ```


## Content

In order for content to show up, it needs to be added to the configuration, 
i.e., in the `pages` section of the `mkdocs.yml` file.

Some pointers:

* [Images and media](http://www.mkdocs.org/user-guide/writing-your-docs/#images-and-media)
* [Linking](http://www.mkdocs.org/user-guide/writing-your-docs/#linking-documents)
* [mkdocs.yml](http://www.mkdocs.org/user-guide/configuration/)


## Build

[mkdocs](http://www.mkdocs.org/) is used to generate HTML from the 
markdown documents and images:

```
./venv/bin/mkdocs build --clean
```


## Testing

You can test what the site looks like, using the following command
and opening a browser on [localhost:8000](http://127.0.0.1:8000):

mkdocs monitors setup and markdown files, so you can just add and edit
them as you like, it will automatically rebuild and refresh the browser.

```
./venv/bin/mkdocs build --clean && ./venv/bin/mkdocs serve
```

## Deploying

You can deploy the current state to Github pages with the following command:

```
./venv/bin/mkdocs gh-deploy --clean
```

## jenkins on CentOS

CentOS does not have Python 3.5 packages directly available through packages,
but rather through *software collections* (`scl`). 

Python 3.5 is enabled like this (from an interactive shell):

```bash
/bin/scl enable rh-python35 bash
```

Assuming that the weka-wiki repo is installed in `/var/lib/jenkins/repos/weka-wiki`, 
you can use the *Execute shell* build step with this script to automatic deploy
the github pages *if* the repo has changed:

```bash
cd /var/lib/jenkins/repos/weka-wiki
COUNT=`git pull | grep -v "Already up-to-date" | wc -l`
if [ $COUNT -gt 0 ]
then 
  /bin/scl enable rh-python35 - << \EOF
cd /var/lib/jenkins/repos/weka-wiki
/var/lib/jenkins/repos/weka-wiki/venv/bin/mkdocs gh-deploy
EOF
fi
```

For getting the jenkins user to deploy to github, you need to:

* create an ssh key as user jenkins

  ```bash
  ssh-keygen -t rsa -f ~/.ssh/github_wekawiki
  ```

* add the following to `~/.ssh/config`

  ```
  Host github-wekawiki
     HostName github.com
     User git
     IdentityFile ~/.ssh/github_wekawiki
     IdentitiesOnly yes
  ```

* add the public ssh key under the repo's *Settings -> Deploy keys* 
  with *write* access

* update the `.git/config` file in the checked out repo, changing the 
  *remote origin* URL to this (using the just created identity):

  ```
  url = git@github-wekawiki:Waikato/weka-wiki.git
  ```

