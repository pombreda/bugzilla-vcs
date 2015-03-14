# Introduction #

## The Goal ##
The goal of this exercise it to see Git commits in Bugzilla.
It is understood that the commiter includes the word _bug 345_ somewhere in her commit comment, e.g.:
```
git commit -m"Bug 43: Cool fix"
```
We also want the commit id's as displayed in Bugzilla become links to GitHub commit info page.

## Prerequisites ##
Before you begin, make sure the following are true:
  1. You have Bugzilla installed and configured.
  1. User john@zilla.com is a valid Bugzilla user.
  1. Your company is called _My Solutions Inc_.
  1. You have an existing Git repository hosted on GitHub as https://github.com/mySolutions/crazyWidgets

## Assumptions ##
Let's assume that:
  1. Your Bugzilla runs on Linux under the user _bugzi_ in _home/bugzi/public\_html/_.


# Details #
Install [bugzilla-vcs](http://code.google.com/p/bugzilla-vcs/).
When _./checksetup.pl_ is run the first time it will likely ask you to install VCI perl extension. After the extension is installed run _./checksetup.pl_ for the second time and all should be well.

Once that is done:
  1. Create new private public key pair on your bugzilla account as per http://help.github.com/linux-set-up-git/ section "_Set Up SSH Keys_". You do **not** want to use any passphrase. We will synchronize with GitHub from cron and we don't want SSH asking for any passphrase.
  1. Upload public key to GitHub under your GitHub profile.
  1. Now you can execute "ssh git@github.com" and confirm that it says "_Hi username! You've successfully authenticated, but GitHub does not provide shell access. Connection to github.com closed._"
  1. Init Git repository on the machine where Bugzilla runs. In _/home/bugzi/_ issue:
```
mkdir crazyWidgets
cd crazyWidgets
git init
git remote add origin git@github.com:mySolutions/crazyWidgets
git pull origin master
```
  1. The above commands should also get the code down from GitHub.
  1. Now create a new file _/home/bugzi/syncgitzilla.sh_ with the following content:
```
cd crazyWidgets
git pull origin master
cd ../public_html
perl extensions/VCS/sync.pl --type=Git --as=john@zilla.com --project=crazyWidgets /home/bugzi
```
  1. Make it executable. Note that john@zilla.com is Bugzilla user under whom the commit info will be added to Bugzilla. We have passed _/home/bugzi_ as the repository to _sync.pl_ although _/home/bugzi_ is **not** Git repository - it is repository parent. We have passed the actual repository as _--project=crazyWidgets_.
  1. We can now setup a cron to run the _/home/bugzi/syncgitzilla.sh_ script every 15 minutes or so.

We now have comments in Bugzilla added to the bug reports, providing the commiter issued "_bug 544_" in commit comment.

To linkify commit id's in bug report page:
  1. Login to bugzilla as administrator.
  1. Navigate to Administration->Parameters->VCS.
  1. Under _vcs\_repos_ type:
```
Git /home/bugzi/
```
  1. Under _vcs\_web_ type:
```
/home/bugzi/ https://github.com/mySolutions/crazyWidgets/commit/%revno%
```

Done.