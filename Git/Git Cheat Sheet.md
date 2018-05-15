# Git Cheat Sheet

## Uso básico

![Uso básico](https://marklodato.github.io/visual-git-guide/basic-usage.svg "Uso básico")

## `git diff`

**Este modo simboliza linhas adicionadas e removidas por espaço em branco e depois as diferencia.**
```bash
git diff --color-words
```

**Diff-highlight pairs up matching lines of diff output and highlights sub-word fragments that have changed.**
```bash
git diff | /usr/local/Cellar/git/2.16.2/share/git-core/contrib/diff-highlight/diff-highlight
```

## `git log`

![Git log](https://www.atlassian.com/dam/jcr:52d530ce-7f51-48e3-920b-a18f776048d3/01.svg "Git log")

**Exibe uma breve visão de todos os commits que estão em `some-feature`que não estão em `master`.**
```bash
git log --oneline master..some-feature
```

**Limit the number of commits by `<limit>`.**
```bash
git log -n <limit>
```

**Condense each commit to a single line. This is useful for getting a high-level overview of the project history.**
```bash
git log --oneline
```

**Along with the ordinary `git log` information, include which files were altered and the relative number of lines that were added or deleted from each of them.**
```bash
git log --stat
```

**Display the patch representing each commit. This shows the full diff of each commit, which is the most detailed view you can have of your project history.**
```bash
git log -p
```

**Search for commits by a particular author. The `<pattern>`argument can be a plain string or a regular expression.**
```bash
git log --author="<pattern>"
```

**Search for commits with a commit message that matches `<pattern>`, which can be a plain string or a regular expression.**
```bash
git log <since>..<until>
```

*A few useful options to consider. The `--graph` flag that will draw a text based graph of the commits on the left hand side of the commit messages. `--decorate` adds the names of branches or tags of the commits that are shown. `--oneline` shows the commit information on a single line making it easier to browse through commits at-a-glance.*
```bash
git log --graph --decorate --oneline
```

### Filtering the Commit History

#### By Date

If you’re looking for a commit from a specific time frame, you can use the `--after` or `--before` flags for filtering commits by date.

```bash
git log --after="2014-7-1"
```

You can also pass in relative references like  `"1 week ago"`  and  `"yesterday"`:

```bash
git log --after="yesterday"
```

To search for a commits that were created between two dates, you can provide both a `--before` and `--after` date.

```bash
git log --after="2014-7-1" --before="2014-7-4"
```

#### By Author

```bash
git log --author="Allyson Silva"
```

#### By Message

To filter commits by their commit message, use the `--grep` flag. This works just like the `--author` flag discussed above, but it matches against the commit message instead of the author.

```bash
git log --grep="JRA-224:"
```

## `git stash`

<img src="https://www.atlassian.com/dam/jcr:35edaf68-e8b1-484e-b5f0-292c532f048a/04.svg" title="Git Stash" alt="Git Stash" width="485" height="665">

### Stashing untracked or ignored files

<img src="https://www.atlassian.com/dam/jcr:d6fec41a-dc66-4af6-8b0f-c23d271eaf8e/01.svg" title="Git Stash Options" alt="Git Stash Options" width="525" height="325">

**Using the `--include-untracked`**

<img src="https://www.atlassian.com/dam/jcr:f7dd5493-a98d-449e-ae37-146d6270ccf7/05.svg" title="Git stash --include-untracked" alt="Git stash --include-untracked" width="485" height="665">

### Viewing stash diffs

```bash
git stash show -p
```

### Creating a branch from your stash

```bash
git stash branch branch-name stash@{1}
```

### Cleaning up your stash

```bash
git stash drop stash@{1}
```

## `git-blame`

**The `-L` option will restrict the output to the requested line range. Here we have restricted the output to lines 1 through 5.**
```bash
git blame -L 1,5 README.md
```

**The `-e` option shows the authors email address instead of username.**
```bash
git blame -e README.md
```

**The `-w` option ignores whitespace changes.**
```bash
git blame -w README.md
```

**The `-M` option detects moved or copied lines within in the same file.**
```bash
git blame -M README.md
```

**The `-C` option detects lines that were moved or copied from other files.**
```bash
git blame -C README.md
```

## Undoing Commits

-   `git revert`  Is the best tool for undoing **shared public changes**.
-   `git reset`  Is best used for undoing **local private changes**.

## `git revert`

> The `git revert` command can be considered an 'undo' type command, however, it is not a traditional undo operation. Instead of removing the commit from the project history, it figures out how to invert the changes introduced by the commit and appends a new commit with the resulting inverse content. This prevents Git from losing history, which is important for the integrity of your revision history and for reliable collaboration.

> A revert is an operation that takes a specified commit and creates a new commit which inverses the specified commit. `git revert`can only be run at a commit level scope and has no file level functionality.

*Reverting should be used when you want to apply the inverse of a commit from your project history. This can be useful, for example, if you’re tracking down a bug and find that it was introduced by a single commit. Instead of manually going in, fixing it, and committing a new snapshot, you can use `git revert` to automatically do all of this for you.*

The `git revert` command is used for undoing changes to a repository's commit history. Other 'undo' commands like, [git checkout](https://www.atlassian.com/git/tutorials/using-branches/git-checkout) and [git reset](https://www.atlassian.com/git/tutorials/undoing-changes/git-reset), move the `HEAD` and branch ref pointers to a specified commit. `Git revert` also takes a specified commit, however, `git revert` does not move ref pointers to this commit. A revert operation will take the specified commit, inverse the changes from that commit, and create a new "revert commit". The ref pointers are then updated to point at the new revert commit making it the tip of the branch.

It's important to understand that `git revert` undoes a single commit—it does not "revert" back to the previous state of a project by removing all subsequent commits. In Git, this is actually called a reset, not a revert.

<img src="https://www.atlassian.com/dam/jcr:a6a50d78-48e3-4765-8492-9e48dec8fd2f/04%20.svg" title="Git revert vs Git reset" alt="Git revert vs Git reset" width="525" height="375">

Reverting undoes a commit by creating a new commit. This is a safe way to undo changes, as it has no chance of re-writing the commit history. For example, the following command will figure out the changes contained in the 2nd to last commit, create a new commit undoing those changes, and tack the new commit onto the existing project.

```bash
git checkout hotfix
git revert HEAD~2
```

This can be visualized as the following:
![Reverting the 2nd to last commit](https://www.atlassian.com/dam/jcr:73d36b14-72a7-4e96-a5bf-b86629d2deeb/06.svg "Reverting the 2nd to last commit")

Contrast this with  `git reset`, which  _does_  alter the existing commit history. For this reason,  `git revert`  should be used to undo changes on a public branch, and  `git reset`  should be reserved for undoing changes on a private branch.

You can also think of  `git revert`  as a tool for undoing  _committed_changes, while  `git reset HEAD`  is for undoing  _uncommitted_changes.

## `git reset`

> O comando `reset` move o ramo atual para uma nova posição, e opcionalmente atualiza o stage e o diretório de trabalho. Ele também é usado para copiar arquivos do histórico para o stage sem alterar o diretório de trabalho.

Se um  _commit_  é realizado sem um nome de arquivo, o ramo atual é movido para aquele  _commit_, e então o  _stage_  é atualizado para coincidir com esse  _commit_. Se  `--hard`  é fornecido, o diretório de trabalho também é atualizado. Se  `--soft`  é fornecido, nem o  _stage_ nem o diretório de trabalho são atualizados.

![Git reset](https://marklodato.github.io/visual-git-guide/reset-commit.svg "Git reset")

![Git reset](https://www.atlassian.com/dam/jcr:5c4cb0f2-856c-4d39-b014-9340ef1d05e4/01.svg "Git reset")

> The  `git reset`  command is a complex and versatile tool for undoing changes. It has three primary forms of invocation. These forms correspond to command line arguments  `--soft`, `--mixed`, `--hard`. The three arguments each correspond to Git's three internal state management mechanism's, The Commit Tree (HEAD), The Staging Index, and The Working Directory.

> A reset is an operation that takes a specified commit and resets the "three trees" to match the state of the repository at that specified commit. A reset can be invoked in three different modes which correspond to the three trees.

```bash
git checkout hotfix
git reset HEAD~2
```

This can be visualized as the following:
![Resetting the hotfix branch to HEAD-2](https://www.atlassian.com/dam/jcr:4c7d368e-6e40-4f82-a315-1ed11316cf8b/02-updated.png "Resetting the hotfix branch to HEAD-2")

This usage of `git reset` is a simple way to undo changes that haven’t been shared with anyone else. It’s your go-to command when you’ve started working on a feature and find yourself thinking, “Oh crap, what am I doing? I should just start over.”

In addition to moving the current branch, you can also get  `git reset`  to alter the staged snapshot and/or the working directory by passing it one of the following flags:

- `--soft`  – The staged snapshot and working directory are not altered in any way.
- `--mixed`  – The staged snapshot is updated to match the specified commit, but the working directory is not affected. This is the default option.
- `--hard`  – The staged snapshot and the working directory are both updated to match the specified commit.

## Rewriting history

*Checkout and reset are generally used for making local or private 'undos'. They modify the history of a repository that can cause conflicts when pushing to remote shared repositories. Revert is considered a safe operation for 'public undos' as it creates new history which can be shared remotely and doesn't overwrite history remote team members may be dependent on.*

![Rewriting history](https://www.atlassian.com/dam/jcr:8e57216e-269e-49e6-aff2-5c03b8512e73/hero.svg "Rewriting history")

### Changing older or multiple commits

> Git rebase gives you the power to modify your history, and interactive rebasing allows you to do so without leaving a “messy” trail. This creates the freedom to make and correct errors and refine your work, while still maintaining a clean, linear project history.

To modify older or multiple commits, you can use  `git rebase`  to combine a sequence of commits into a new base commit. In standard mode,  `git rebase`  allows you to literally rewrite history — automatically applying commits in your current working branch to the passed branch head. Since your new commits will be replacing the old, it's important to not use  `git rebase`  on commits that have been pushed public, or it will appear that your project history disappeared.

In these or similar instances where it's important to preserve a clean project history, adding the `-i` option to `git rebase` allows you to run `rebase interactive`. This gives you the opportunity to alter individual commits in the process, rather than moving all commits. You can learn more about interactive rebasing and additional rebase commands on the [git rebase page](https://www.atlassian.com/git/tutorials/rewriting-history/git-rebase).

#### Multiple messages

Each regular Git commit will have a log message explaining what happened in the commit. These messages provide valuable insight into the project history. During a rebase, you can run a few commands on commits to modify commit messages.

-   **Reword** or `r` will stop rebase playback and let you rewrite the individual commit message during.
-   **Squash** or `s` during rebase playback, any commits marked  `s`  will be paused on and you will be prompted to edit the separate commit messages into a combined message. More on this in the squash commits section below.
-   **Fixup** or `f` has the same combining effect as *squash*. Unlike *squash*, *fixup* commits will not interrupt rebase playback to open an editor to combine commit messages. The commits marked `f` will have their messages discarded in-favor of the previous commit's message.

#### Squash commits for a clean history

The `s` "squash" command is where we see the true utility of rebase. Squash allows you to specify which commits you want to merge into the previous commits. This is what enables a "clean history." During rebase playback, Git will execute the specified rebase command for each commit. In the case of squash commits, Git will open your configured text editor and prompt to combine the specified commit messages. This entire process can be visualized as follows:

![git rebase -i](https://www.atlassian.com/dam/jcr:2d03f5b6-eaa6-4e78-9dd7-3686ba2a7665/05.svg "git rebase -i")

Note that the commits modified with a rebase command have a different ID than either of the original commits. Commits marked with pick will have a new ID if the previous commits have been rewritten.

-------------

-  Use  `git commit --amend`  to change your latest log message.
-  Use  `git commit --amend`  to make modifications to the most recent commit.
-   Use  `git rebase`  to combine commits and modify history of a branch.
-   `git rebase -i`  gives much more fine grained control over history modifications than a standard git rebase.

## `git reflog`

> Reference logs, or "reflogs" are a mechanism Git uses to record updates applied to tips of branches and other commit references. Reflog allows you to go back to commits even though they are not referenced by any branch or tag. After rewriting history, the reflog contains information about the old state of branches and allows you to go back to that state if necessary. Every time your branch tip is updated for any reason (by switching branches, pulling in new changes, rewriting history or simply by adding new commits), a new entry will be added to the reflog.

*It's important to note that the reflog only provides a safety net if changes have been committed to your local repository and that it only tracks movements of the repositories branch tip. Additionally reflog entries have an expiration date. The default expiration time for reflog entries is 90 days.*

The most basic Reflog use case is invoking:
```bash
git reflog
```

This is essentially a short cut that's equivalent to:
```bash
git reflog show HEAD
```

### Filtros de reflogs

Uma vez que cada `reflog` tem um tempo implícito associado a ele, você pode filtrar, não só pela história, mas também pelo tempo. Várias formas de suporte incluem:

- 1.minute.ago (1.minuto.atrás)
- 1.hour.ago (1.hora.atrás)
- 1.day.ago (1.dia.atrás)
- Yesterday (ontem)
- 1.week.ago (1.semana.atrás)
- 1.month.ago (1.mês.atrás)
- 1.year.ago (1.ano.atrás)
- 2011-05-17.09:00:00

O formato da hora é mais útil se você quiser voltar ao estado de um branch como de uma hora atrás, ou quer ver que diferenças foram feitas na última hora (por exemplo, `git diff @{1.hour.ago}`). Observe que, se um branch está faltando, então ele assume o branch atual (assim `@{1.hour.ago}` refere-se a `master@{1.hour.ago}` se no branch `master`).

## `git rebase`

### What is git rebase?

> Rebasing is the process of moving or combining a sequence of commits to a new base commit. Rebasing is most useful and easily visualized in the context of a feature branching workflow.

The general process can be visualized as the following:

![Git rebase](https://www.atlassian.com/dam/jcr:e4a40899-636b-4988-9774-eaa8a440575b/02.svg "Git rebase")

From a content perspective, rebasing is changing the base of your branch from one commit to another making it appear as if you'd created your branch from a different commit. Internally, Git accomplishes this by creating new commits and applying them to the specified base. It's very important to understand that even though the branch looks the same, it's composed of entirely new commits.

![Rebasing the feature branch onto master](https://www.atlassian.com/dam/jcr:5b153a22-38be-40d0-aec8-5f2fffc771e5/03.svg "Rebasing the feature branch onto master")

The major benefit of rebasing is that you get a much cleaner project history. First, it eliminates the unnecessary merge commits required by git merge. Second, as you can see in the above diagram, rebasing also results in a perfectly linear project history—you can follow the tip of feature all the way to the beginning of the project without any forks. This makes it easier to navigate your project with commands like git log, git bisect, and gitk.

### The Golden Rule of Rebasing

Once you understand what rebasing is, the most important thing to learn is when _not_ to do it. The golden rule of `git rebase` is to never use it on _public_ branches.
