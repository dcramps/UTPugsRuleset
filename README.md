# Adding maps to redirect
If you are just adding a map to the server that doesn't need to be in a ruleset, follow this section first. If it's for a ruleset, eg CTF PUGs, follow the `Cloning rulesets` and `Adding a map` sections first.

## Upload pak to UTPugs.us redirect. 
Make sure that paks being updated are deleted from the redirect _before_ uploading a new version.

## Sync redirect
SSH info is in the UTPugs admin Discord.
Log into the hub using whatever SSH client you want. I use Ubuntu for Windows: https://ubuntu.com/tutorials/tutorial-ubuntu-on-windows#1-overview

The command you need to run is `ssh username@ipaddress` where `username` and `ipaddress` are credentials from the admin Discord. Once you are logged in, you can run the `redirect.sh` script:

NYC: `cd utroot` then `bash redirect.sh`

Chi: `bash redirect.sh`

The script will find the map on the redirect and add it. If you get a message like this, it means you fucked up and the new/updated pak is not on the redirect:

```
utroot@utpugatl:~$ bash redirect.sh
Fetching file list
No new paks
Use './redirect.sh -f' to force a sync
```

If you get a message like this, the hub is in use so you have to wait and try again later:

```
utroot@utpugatl:~$ bash redirect.sh
Not syncing. There may be a game instance running.
Use './redirect.sh -k' to be a jerk and sync anyways.
```


# Cloning rulesets
We use git for this so that everything is in one location and can be synced to each hub, and can easily be rolled back. Setting up git and cloning this repo only needs to happen once.
  
## Set up git
Follow these instructions to download the GitHub app: https://help.github.com/en/desktop/getting-started-with-github-desktop/installing-and-authenticating-to-github-desktop

## Clone this repo

Open the GitHub app, and click this button:
![1](https://dc.wtf/sQtnFejW.png)

Paste in the URL to this repo -- `https://github.com/dcramps/UTPugsRuleset.git` -- and click Clone.
![2](https://dc.wtf/gkPqGAbP.png)

Click Show in Explorer
![3](https://dc.wtf/9Bbv7fk1.png)

Open `DefaultRules_disable.json` in a text editor.
![4](https://dc.wtf/jk8WtUdJ.png)


You should see a giant list of bullshit. These are the rules for the hubs. Some highlights:

`uniqueTag` - Not visible in game, but used to distinguish each ruleset. If you change one, make sure you don't change it to the same thing as another one.
`categories` - This determines where in the hub menu a ruleset will live. The strings in here map to the tabs at the top of the hub Create a Match menu.
`mapPrefixes` - This is used for rulesets without specific map lists. For example, `DM` will set this rule to list all Deathmatch maps
`customMapList` - This is where the map list is specified (more on this below)
`maxPlayers` - Duh
`gameOptions` - This is a giant shitty string specifying all of the mutators used in a rule. Usually you won't need to change this.

# Adding a map

## Create a new branch based on `master`

![5](https://dc.wtf/jWmem9oe.png)

Give it a fun name
![6](https://dc.wtf/x2ULYPms.png)

## Find the path of the pak being added. 
For mutators, usually the mutator name is all you need, eg: `MutTeamSkins`. For maps, you must get the full path from the map author. If you are a map author, you can find your map's path by hovering over it in the editor:
  
![a](https://dc.wtf/pPeDRwDy.png)
  
The full path of the map required for rulesets is the path in the popup + the name of the umap file. In this example the full path is `/Game/dc/Ironic/DM-Alanis`

## Add it to the list.
Find the gametype you want to add to. PUG CTF is right at the top of the `DefaultRules_disable.json` file.

Paste your path into the `customMapList` section. 

Some things to note:
- Make sure the maps stay in alphabetical order based on the map's name. This makes it easier to find maps when voting and setting up a match.
- The path must be surrounded with double quotes, e.g: `"/Game/dc/Ironic/DM-Alanis"`
- If the map is anywhere in the list other than the very end, it *must end with a comma* or it will break the ruleset.
- The path must begin with exactly 8 spaces. No tabs. If you use tabs, you will die.


You can verify that you're not a retard by pasting the entire contents of `DefaultRules_disable.json` into https://jsonlint.com and clicking validate. If it says it's valid, you are good to go. If it gives you an error, go back to the file in your text editor and fix it.

## Commit changes
Once you are sure that you didn't fuck anything up, you can commit your changes to your branch. The GitHub app will highlight the new map in green. If you removed anything, it will highlight it in red.

![7](https://dc.wtf/sKmyj85z.png)

Write a commit message describing what you did and click Commit
![8](https://dc.wtf/kLtlIIJq.png)

## Publish your branch

You should now see a `Publish branch` button. Click it! It will sit there for a second figuring out some things.
![9](https://dc.wtf/Z0yYEc0m.png)

The button will now change to `Create Pull Request`. Click it.
![10](https://dc.wtf/Ttvz3PRl.png)

## Create a Pull Request
Your browser will open to the Pull Request screen. It should look like this:
![11](https://dc.wtf/QXhHV0Gs.png)

Make sure that it says `base: master` and `compare: <your branch name>` before continuing.
Click the `Reviewers` gear and add `dcramps` as a reviewer
Click `Create pull request`

If everything is good, someone will merge the changes to master. You should get an email when that happens. Feel free to ping dc/phantaci to look at changes.

Once the merge is done, you can sync the redirect using the info at the top of this page. The map will be downloaded onto the hub and the rules will sync. The hub should reboot within ~30 seconds.

## Check out the `master` branch again
Once everything is merged into master, you should check it out again so you are in the right spot next time you want to add a map.

Open the GitHub app and click the current branch button.
![12](https://dc.wtf/XVU1lrrX.png)

Click on `master`, then on `Fetch origin`
![13](https://dc.wtf:443/5aaDLtSE.png)

Every time you open the GitHub app you should make sure you fetch origin if it does not automatically update.
