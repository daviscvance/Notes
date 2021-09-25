# Version Control Management using Fig (Mercurial)

[TOC]

## Resources

*   [Fig's Basics Codelab (go/fig-codelab)](http://go/fig-codelab)
*   [Larger Fig Cheatsheet (go/fig-cli)](http://go/fig-cli)
*   [Fig's Glossary (go/fig-glossary)](http://go/fig-glossary)
*   [Fig's FAQ (go/fig-faq)](http://go/fig-faq)

## Overview

[Mercurial](https://www.mercurial-scm.org/) is an open source version control
management tool and is often simplified as `hg` i.e. the chemical symbol of
Mercury. Fig is Google's internal version based on `hg` and used in the CLI.
Commits are chained together and branched in a number of ways to create a large
tree (maybe an old fig, if you will). Each submission cooperates with all of its
descendants,sending any changes down the line. See go/fig for more overview.

### Example of a [Chain](https://www.youtube.com/watch?v=gGAiW5dOnKo)

<span class="tiny_text" style="font-size:0.5em;">(Thanks
[Mike](http://go/mikebeaumier))</span>

When you build a chain of CLs using `hg`, you may have all of these CLs reviewed
in parallel even though they may sequentially depend on one another (e.g. the
root of a linked list of CLs is changed, and we must now propagate it to the
child CLs.)

Basically, when we change an upstream CL, all downstream CLs are orphaned (a
turn of phrase used because the parent is gone).

Here's an example from the
[codelab](https://g3doc.corp.google.com/codelab/fig/g3doc/index.md):

<pre><code class="no-auto-prettify lang-shell">$ <strong>hg xl</strong>
@  <span class="hash-draft"><span class="hash-prefix">10</span>adedc1</span> <span class="xl-user">{{USERNAME}}</span> <span class="xl-tag">cl/131000007</span> <span class="xl-tag">tip</span> <span class="will-update">&lt;will update http://cl/131000007&gt;</span>
|  <span class="xl-desc">Adding some text to hello_world.txt</span>
| *  <span class="hash-draft"><span class="hash-prefix">f0</span>05b411</span> <span class="xl-user">{{USERNAME}}</span> <span class="xl-tag">cl/131000001</span> <span class="xl-orphan">orphan</span> <span class="xl-exported">&lt;exported as http://cl/131000001&gt;</span>
| |  <span class="xl-desc">Making a change to hello_world.txt</span>
| x  <span class="hash-draft"><span class="hash-prefix">da7a</span>ba5e</span> <span class="xl-user">{{USERNAME}}</span> <span class="xl-tag">cl/131000007</span> <span class="xl-obsolete">obsolete</span> <span class="xl-exported">&lt;exported as http://cl/131000007&gt;</span>
|/   <span class="xl-desc">Adding some text to hello_world.txt</span>
o  <span class="hash-public"><span class="hash-prefix">dec0</span>ded3</span> cl/126388176 <span class="xl-age">17 minutes ago</span> <span class="xl-user">sippingbird-prod</span> <span class="xl-tag">p4head</span>
|  <span class="xl-desc outsidenarrow">Some random commit outside of scope</span>
~</code></pre>

We fix the orphan by moving the orphan to be "on top" or "after" the new change
made to create a new parent. The reason we have a "new" parent rather than a
modified old parent is because every time we submit changes or upload changes to
a CL, this creates a new revision.

Similarly, if child revisions are changed and not the parent, we can `evolve`
the parent to the new children.

### A sample workflow

*   Commit some changes with `hg commit -m <message>`
*   Upload changes to critique `hg uc`
*   Mail changes for review `hg mail -m <reviewer> -r .`
*   Review and implement changes
*   Amend the CL with `hg amend`
*   Upload revision with `hg uc`
*   Submit the final result `hg submit`

IMPORTANT: Every CL you upload with Fig will exist inside its own separate CitC
client. You should usually amend your CLs inside the Fig workspace and then
re-upload them, instead of editing the other clients directly. If you do end up
editing a CL's client directly, you can (pull the edits back into your Fig
workspace)[http://go/fig-faq#pull-back].

WARNING: Lossy CLs should NOT be submitted via the Critique UI.

## Cheatsheet

-   Commands have shortened versions see: `hg help`.
-   Refer to the current working revision with `.`.
-   `hg githelp` can help you translate hg to git.
-   The head or root revision that the client is synced to is `p4head`.
-   The most recent commit is referred to as `tip`.

### Commands

| Description                            | Command                             |
| -------------------------------------- | ----------------------------------- |
| Create a new CitC client managed by    | `hgd -f <myworkspacename>`          |
: fig                                    :                                     :
| Add a file to a commit                 | `hg add`                            |
| Commit changes                         | `hg commit`                         |
| See history of changes                 | `hg xl`                             |
| Check modified files / status of       | `hg st[atus]`                       |
: client                                 :                                     :
| Copy a revision                        | `hg graft <rev>`                    |
| Upload changes to piper (like git      | `hg upload chain` OR `hg uc`        |
: push), creating a CL                   :                                     :
| Change working revision                | `hg checkout <rev>`                 |
| Amend a CL with new changes based on   | `hg amend`                          |
: review                                 :                                     :
| Amend a chain of CLs smartly with      | `hg abs[orb]`                       |
: different changes spanning a few       :                                     :
: revisions                              :                                     :
| Propagate changes down the chain       | `hg evo[lve]`                       |
| Mail a CL for review while running     | `hg mail -m <startblock> -r <rev>`  |
: presumbits                             :                                     :
| Sync to head                           | `hg sync [@<rev>]`                  |
| Diff between current revision and a    | `hg diff --from <rev> [--to <rev>]` |
: revision (head rev = `p4head`)         :                                     :
| Change to a revision pointed to by an  | `hg update cl/123456`               |
: active CL                              :                                     :
| Move to a child or parent revision     | `hg next` OR `hg prev[ious]`        |
| Submit a chain of of CLs ending with a | `hg submit -r cl/<CL ###>`          |
: particular number.                     :                                     :
| Perform operations on multiple CLs,    | `hg histedit`                       |
: including reordering a chain           :                                     :
| - HistEdit keep the commit (default)   | `hg histedit` > `pick`              |
| - HistEdit keep the commit but give    | `hg histedit` > `edit`              |
: chance to amend it                     :                                     :
| - HistEdit reword the commit message   | `hg histedit` > `mess`              |
| - HistEdit combine a commit with the   | `hg histedit` > `fold`              |
: preceding commit                       :                                     :
| - HistEdit combine a commit with the   | `hg histedit` > `roll`              |
: preceding commit (like `fold`) but     :                                     :
: discarding the description             :                                     :
| > HistEdit prune a commit              | `hg histedit` > `drop`              |
| Split pieces of a single commit into   | `hg split`                          |
: multiple changelists                   :                                     :
| Remove an obsolete CL                  | `hg cls-drop --prune cl/<cl#>`      |
| Move a revision to a new location the  | `hg rebase -s <rev> -d`             |
: tree                                   :                                     :
| Place a change on the shelf for later. | `hg she[lve]`                       |
: This can allow jumping revisions       :                                     :
: without worrying about committing      :                                     :
| Take a changed file off of the shelf   | `hg unshe[lve]`                     |
| Format any language for compliance     | `hg fixdot --no-amend --format=sql` |

### Flags

Description                         | Flag
----------------------------------- | --------------------
Create a new commit for the same CL | `--same_cl`
Mark a commit as obsolete on drop   | `--prune`
Select the files to operate on      | `--interactive` `-i`
Rebase source revision to move      | `--source` `-s`
Rebase destination revision as top  | `--destination`
Preview an absorb without prompt    | `-pn`
