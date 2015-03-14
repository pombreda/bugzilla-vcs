This is an extension for Bugzilla 4.0 and above that allows you to integrate your version-control system (sometimes also called a "Software Configuration Management" or "SCM" system) with Bugzilla.

This extension is provided by [Everything Solved, Inc.](http://www.everythingsolved.com/) If you would like support, customizations, or additional features added, please [contact us](http://www.everythingsolved.com/contact/).

## Supported VCS Systems ##

Currently, the VCS extension can support **CVS**, **Subversion** (svn), **Bazaar** (bzr), **Mercurial** (hg), and **Git**.

If you would like any additional VCS to be supported, then [Everything Solved](http://www.everythingsolved.com/) can write a new driver for your VCS. Just [contact us](http://www.everythingsolved.com/contact/)!

Also, if you run into any problem with the VCSes that we already support, and you'd like them improved, we'd be happy to help.

## What Does It Do? ##

Currently, the VCS extension displays information in Bugzilla about which commit is associated with which bug, when you are viewing a bug. There is a [screenshot](http://bugzilla-vcs.googlecode.com/files/commits-highlighted.png) of what the bug view page looks like with this extension installed.

We provide a script (`hook.pl`) that you can use as part of a hook in your version control system, that updates Bugzilla whenever a commit is added to your repository. We also include a script (`sync.pl`) that will "sync" all of the existing commits in your repository to Bugzilla, by searching the commit messages for phrases like "[bug 1234](https://code.google.com/p/bugzilla-vcs/issues/detail?id=234)" and linking that commit to [bug 1234](https://code.google.com/p/bugzilla-vcs/issues/detail?id=234).

In the future, we hope to also support checking in a patch directly to your VCS from Bugzilla.

## Installation ##

The extension can be installed on Bugzilla **3.6.1** or any later version, like this:

  1. [Download](http://code.google.com/p/bugzilla-vcs/downloads/) the latest release. (Choose either the "zip" version or the "tar.gz" version, whichever is more convenient for you.)
  1. Unpack the download. This will create a directory called "VCS".
  1. Move the "VCS" directory into the "extensions" directory in your Bugzilla installation. Note that once you have done this, parts of your Bugzilla may become non-functional until you complete the rest of these installation instructions.
  1. Go to your Bugzilla directory and run:
```
  ./checksetup.pl
```
> And follow all of the instructions that checksetup.pl prints. You will probably have to install some Perl modules and then re-run checksetup.pl again.

## Configuring Support For Each VCS ##

When you run checksetup.pl after installing the VCS extension, you'll notice that if you want support for certain VCSes, you'll have to install certain extra things.

### Git ###

checksetup.pl will tell you that you have to install Git.pm. You actually can't install this using install-module.pl--it just comes with Git. So if you install Git, you should get Git.pm along with it.

### CVS ###

See http://search.cpan.org/dist/VCI/lib/VCI/VCS/Cvs.pm#REQUIREMENTS. Note that the listed programs must not only be installed, but must be in your PATH.

## Bzr ##

See http://search.cpan.org/dist/VCI/lib/VCI/VCS/Bzr.pm#REQUIREMENTS.

## Configuring the VCS Extension ##

After you install the VCS extension, you'll want to configure it. In particular, you have to specify which version control repositories may be linked to Bugzilla. (This is for security reasons--there are many situations in which users should not be allowed to link any arbitrary VCS to Bugzilla.)

Currently, the configuration for the extension is done via the Parameters control panel, in the "Administration" area of Bugzilla.

## Using hook.pl and sync.pl ##

For full information on each script, you should run: `perldoc hook.pl` or `perldoc sync.pl` from the extension directory. Also, you can run each script with a `--help` switch for a short description of their capabilities.

You should run sync.pl from the root of the Bugzilla directory, like: `perl extensions/VCS/sync.pl`.

hook.pl can simply be run via `perl hook.pl`. hook.pl can be copied to anywhere, all by itself--it is not dependent upon Bugzilla code or the VCS extension's code. However, it does require that the [RPC-XML](http://search.cpan.org/dist/RPC-XML/) Perl module be installed on the system where it runs.

## WebService Extensions ##

The extension adds a method to the WebServices (both XML-RPC and JSON-RPC) called **VCS.add\_commit**. To see how to use it, look at the "hook.pl" script that is included with the extension.

## Contributing ##

The VCS extension is of course an open-source project, licensed under the Mozilla Public License. If you'd like to see something about the VCS extension improved, then please feel free to submit a patch! Just create an Issue here and attach your patch.

You can get the code for the extension via bzr:

**bzr co bzr://bzr.mozilla.org/bugzilla/extensions/vcs/trunk VCS**

You can also browse the repository in your web browser:

http://bzr.mozilla.org/bugzilla/extensions/vcs/trunk

Also, the VCS extension depends heavily upon the Perl [VCI](http://search.cpan.org/dist/VCI) library. If you want to see additional support for other VCSes or improved performance in the existing VCS support, see the [VCI home page](http://vci.everythingsolved.com/) for information about how to contribute to VCI.