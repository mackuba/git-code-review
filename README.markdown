# git code-review

`git-code-review` is an extension to git that is meant to help you with doing regular code reviews of other team members' commits.

It does not expect any specific branch structure or way of working, you can use it if you only work on `master` branch most of the time, or if you want to track feature branches in progress before they're finished. In fact, it's little more than just a secondary branch structure mirroring the `origin/*` branches, but one that only changes when you run the `git-code-review` script. The point is to tell you which branches were created or modified since the last time you ran the tool, and give you the commit ranges and GitHub compare links that you should look at.

`git-code-review` stores its data in `.git/review` directory, and keeps files in a structure that literally looks the same as `.git/refs/remotes`. When you run the tool, it does `git fetch`, compares the latest branch revisions to the ones stored in `.git/review`, shows you the differences and then updates the stored branch revisions.

That way, you can do `git pull` whenever you want to make sure you're running the latest version of the code, but only do reviews when it suits you, e.g. every morning before you start coding.


## Installation

Just download the script, put it somewhere in your `$PATH` and make it executable:

    curl https://raw.github.com/jsuder/git-code-review/master/git-code-review > /usr/local/bin/git-code-review
    chmod a+x /usr/local/bin/git-code-review

If you work on multiple computers, I recommend that you put the `review` directory somewhere in your Dropbox folder and create a link to it in the local repo:

    mkdir ~/Dropbox/Projects/whatever/review
    ln -s ~/Dropbox/Projects/whatever/review .git/review

## Running

You can just run the script directly, but you can also use it as if it was a git command:

    git code-review

On the first run it will just store the current branch revisions:

    Fetching latest updates...
    Branch origin/master created: 313a63b
    Nothing to review, back to work!

If you run it again e.g. when you come to work next day, there will probably be some new commits and the script will show you which branches have changed:

    Fetching latest updates...
    remote: Counting objects: 365, done.
    remote: Compressing objects: 100% (147/147), done.
    remote: Total 270 (delta 213), reused 176 (delta 119)
    Receiving objects: 100% (270/270), 92.89 KiB | 97 KiB/s, done.
    Resolving deltas: 100% (213/213), completed with 74 local objects.
    From github.com:jsuder/holepicker
       abc0825..0a711db  master     -> origin/master
    Branch origin/master updated: 313a63b..0a711db
     -> https://github.com/jsuder/holepicker/compare/313a63b...0a711db

Now you can e.g. do `git log -p` or `git diff` with the ranges you're interested in, or copy the link to your browser, or just click it if your terminal supports that (e.g. Cmd+click in iTerm).

If the repo's URL doesn't look like a GitHub URL, then `git-code-review` just won't print the GitHub URL, but will still print the commit ranges.

Note: the ranges listed in `git fetch` output (here: `abc0825..0a711db`) and the ones printed by `git-code-review` (`313a63b..0a711db`) don't have to be the same - that's the whole point. The former will be the difference since the last time you did `git fetch` or `git pull`, which might have been an hour ago, and the latter will be the difference since the last time you called `git-code-review`, which is everything you haven't reviewed yet.


## Credits

Created by [Jakub Suder](http://psionides.eu), licensed under MIT License.
