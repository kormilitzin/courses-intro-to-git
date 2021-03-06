---
title       : Collaborating
description : >-
  This chapter shows Git's other greatest feature: how you can share
  changes between repositories to collaborate at scale.

--- type:ConsoleExercise lang:shell xp:100 skills:1 key:a87bbd3948
## How can I create a brand new repository?

So far,
you have been working with repositories that we created.
If you want to create a repository for a new project,
you can simply say `git init project-name`,
where "project-name" is the name you want the new repository's root directory to have.

One thing you should *not* do is create one Git repository inside another.
While Git does allow this,
updating **nested repositories** becomes very complicated very quickly,
since you need to tell Git which of the two `.git` directories the update is to be stored in.
Very large projects occasionally need to do this,
but most programmers and data analysts try to avoid getting into this situation.

*** =instructions

Use a single command to create a new Git repository called `optical` below your home directory.

*** =solution
```{shell}
git init optical
```

*** =sct
```{python}
Ex() >> test_student_typed(r'\s*git\s+init\s+optical\s*',
                           fixed=False,
                           msg='Use `git init` and a directory name.')
```

<!-- -------------------------------------------------------------------------------- -->

--- type:BulletConsoleExercise key:a4330ec681
## How can I turn an existing project into a Git repository?

Experienced Git users instinctively start new projects by creating repositories.
If you are new to Git,
though,
or working with people who are,
you will often want to convert existing projects into repositories.
Doing so is simple:
just run `git init` in the project's root directory,
or `git init /path/to/project` from anywhere else on your computer.

*** =pre_exercise_code
```{python}
repl = connect('bash')
repl.run_command('cd dental')
repl.run_command('rm -rf .git')
repl.run_command('clear')
repl.run_command('pwd')
```

*** =type1: ConsoleExercise
*** =key1: 101686c0e7

*** =xp1: 10

*** =instructions1

You are in the directory `dental`,
which is not yet a Git repository.
Use a single command to convert it to a Git repository.

*** =hint1

Use `git init` with no arguments.

*** =sample_code1
```{shell}
```

*** =solution1
```{shell}
git init
```

*** =sct1
```{python}
Ex() >> test_student_typed(r'\s*git\s+init\s*',
                           fixed=False,
                           msg='Use `git init`.')
```

*** =type2: ConsoleExercise
*** =key2: e65f050907

*** =xp2: 10

*** =instructions2

Check the status of your new repository.

*** =hint2

As always, use `git status`.

*** =sample_code2
```{shell}
```

*** =solution2
```{shell}
git status
```

*** =sct2
```{python}
Ex() >> test_student_typed(r'\s*git\s+status\s*',
                           fixed=False,
                           msg='Check the status as you normally would.')
```

<!-- -------------------------------------------------------------------------------- -->

--- type:BulletConsoleExercise key:9fb3b2ed49
## How can I create a copy of an existing repository?

Sometimes you will join a project that is already running,
inherit a project from someone else,
or continue working on one of your own projects on a new machine.
In each case,
you will **clone** an existing repository instead of creating a new one.
Cloning a repository does exactly what the name suggests:
it creates a copy of an existing repository (including all of its history) in a new directory.

To clone a repository,
use the command `git clone URL`,
where `URL` identifies the repository you want to clone.
This will normally be something like `https://github.com/datacamp/project.git`,
but for this lesson,
we will use **filesystem URLs** of the form `file:///existing/project`.
The number of slashes at the start is important:
the first part of the URL is `file://`,
and then there is a third slash to start the absolute path `/existing/project`.

When you clone a repository,
Git uses the name of the existing repository as the name of the clone's root directory:
for example,
`git clone file:///existing/project` will create a new directory called `project`.
If you want to call the clone something else,
add the directory name you want to the command.

*** =pre_exercise_code
```{python}
repl = connect('bash')
repl.run_command('rm -rf dental')
repl.run_command('clear')
repl.run_command('pwd')
```

*** =type1: ConsoleExercise
*** =key1: 2b06ff535f

*** =xp1: 10

*** =instructions1

You have just inherited the dental data analysis project from a colleague,
who tells you that all of their work is in a repository in `/home/thunk/repo`.
Use a single command to clone this repository
to create a new repository called `dental` inside your home directory
(so that the new repository is in `/home/repl/dental`).

*** =hint1

Remember to count the slashes after `file:` carefully.

*** =sample_code1
```{shell}
```

*** =solution1
```{shell}
git clone file:///home/thunk/repo dental
```

*** =sct1
```{python}
Ex() >> test_student_typed(r'\s*git\s+clone\s+file:///home/thunk/repo\s+(/home/repl/|~/|\./|)dental\s*',
                           fixed=False,
                           msg='Use `git clone` and the absolute or relative path of the directory to clone to.')
```

<!-- -------------------------------------------------------------------------------- -->

--- type:MultipleChoiceExercise lang:shell xp:50 skills:1 key:61567f27d4
## How can I find out where a cloned repository originated?

When you a clone a repository,
Git remembers where the original repository was.
It does this by storing a **remote** in the new repository's configuration.
A remote is like a browser bookmark with a name and a URL.
If you are in a repository,
you can list the names of its remotes using `git remote`.

If you want more information, you can use `git remote -v` (for "verbose"),
which shows the remote's URLs.
Note that "URLs" is plural:
it's possible for a remote to have several URLs associated with it for different purposes,
though in practice each remote is almost always paired with just one URL.

<hr>

You are in the `dental` repository.
How many remotes does it have?

*** =instructions
- None.
- 1.
- 2.

*** =hint

Run `git remote` in the `dental` repository.

*** =pre_exercise_code
```{shell}
repl = connect('bash')
repl.run_command('rm -rf dental')
repl.run_command('git clone file:///home/thunk/repo dental')
repl.run_command('clear')
repl.run_command('cd dental')
```

*** =sct
```{python}
Ex() >> test_mc(2, ['No: there are some remotes.',
                    'Correct!',
                    'No: there is just one remote.'])
```

<!-- -------------------------------------------------------------------------------- -->

--- type:BulletConsoleExercise key:1e327efda1
## How can I define remotes?

When you clone a repository,
Git automatically creates a remote called `origin`
that points to the original repository.
You can add more remotes using:

```
git remote add remote-name URL
```

and remove existing ones using:

```
git remote rm remote-name
```

You can connect any two Git repositories this way,
but in practice,
you will almost always connect repositories that share some common ancestry.

*** =pre_exercise_code
```{python}
repl = connect('bash')
repl.run_command('cd dental')
```

*** =type1: ConsoleExercise
*** =key1: a0e2cf2d0f

*** =xp1: 10

*** =instructions1

You are in the `dental` repository.
Add `file:///home/thunk/repo` as a remote called `thunk` to it.

*** =hint1

Be sure to count the slashes properly in the remote URL.

*** =sample_code1
```{shell}
```

*** =solution1
```{shell}
git remote add thunk file:///home/thunk/repo
```

*** =sct1
```{python}
Ex() >> test_student_typed(r'\s*git\s+remote\s+add\s+thunk\s+file:///home/thunk/repo\s*',
                           fixed=False,
                           msg='Use `git remote add` with the name and URL of the remote.')
```

<!-- -------------------------------------------------------------------------------- -->

--- type:BulletConsoleExercise key:4d5be24350
## How can I pull in changes from a remote repository?

Git keeps track of remote repositories so that you can
**pull** changes from those repositories
and **push** changes to them.
Pulling changes is straightforward:
the command `git pull remote branch`
gets everything in `branch` in the remote repository identified by `remote`
and merges it into the current branch of your local repository.
For example,
if you are in the `quarterly-report` branch of your local repository,
the command:

```
git pull thunk latest-analysis
```

would get changes from `latest-analysis` branch
in the repository associated with the remote called `thunk`
and merge them into your `quarterly-report` branch.

*** =pre_exercise_code
```{python}
repl = connect('bash')
repl.run_command('rm -rf dental')
repl.run_command('git clone file:///home/thunk/repo dental')
repl.run_command('git -C dental reset --hard HEAD~2')
repl.run_command('clear')
repl.run_command('cd dental')
```

*** =type1: ConsoleExercise
*** =key1: cb79240464

*** =xp1: 10

*** =instructions1

You are in the `master` branch of the repository `dental`,
which has a remote called `origin`.
Pull all of the changes in the `master` branch of that remote repository
into the `master` branch of your repository.

*** =hint1

Use `git pull` with the name of the remote (`origin`) and the name of the branch (`master`).

*** =sample_code1
```{shell}
```

*** =solution1
```{shell}
git pull origin master
```

*** =sct1
```{python}
Ex() >> test_student_typed(r'\s*git\s+pull\s+origin\s+master\s*',
                           fixed=False,
                           msg='Use `git pull` with the name of the remote and the name of the branch.')
```

<!-- -------------------------------------------------------------------------------- -->

--- type:BulletConsoleExercise key:2b1e228738
## What happens if I try to pull when I have unsaved changes?

Just as Git stops you from switching branches when you have unsaved work,
it also stops you from pulling in changes from a remote repository
when doing so might overwrite things you have done locally.
The fix is simple:
either commit your local changes or revert them,
and then try to pull again.

*** =pre_exercise_code
```{python}
repl = connect('bash')
repl.run_command('rm -rf dental')
repl.run_command('git clone file:///home/thunk/repo dental')
repl.run_command('git -C dental reset --hard HEAD~1')
repl.run_command('echo "One final thing..." >> dental/report.txt')
repl.run_command('clear')
repl.run_command('cd dental')
```

*** =type1: ConsoleExercise
*** =key1: f03c277669

*** =xp1: 10

*** =instructions1

You are in the dental repository,
which was cloned from a remote called `origin`.
Use `git pull` to bring in changes from that repository.

*** =hint1

Remember to use `origin` *and* `master` (the first is the remote name, the second is the branch name).

*** =sample_code1
```{shell}
```

*** =solution1
```{shell}
git pull origin master
```

*** =sct1
```{python}
Ex() >> test_student_typed(r'\s*git\s+pull\s+origin\s+master\s*',
                           fixed=False,
                           msg='Use `git pull origin master` to pull in changes.')
```

*** =type2: ConsoleExercise
*** =key2: 301f2bd8a3

*** =xp2: 10

*** =instructions2

Discard the changes in your repository.

*** =hint2

Read the message printed by `git pull` for a reminder of how to do this.

*** =sample_code2
```{shell}
```

*** =solution2
```{shell}
git checkout -- .
```

*** =sct2
```{python}
Ex() >> test_student_typed(r'\s*git\s+checkout\s+--.*',
                           fixed=False,
                           msg='Use `git checkout --` and a path.')
```

*** =type3: ConsoleExercise
*** =key3: 2cad24ea7a

*** =xp3: 10

*** =instructions3

Re-try the `git pull`.

*** =hint3

Use the same arguments to `git pull` as before.

*** =sample_code3
```{shell}
```

*** =solution3
```{shell}
git pull origin master
```

*** =sct3
```{python}
Ex() >> test_student_typed(r'\s*git\s+pull\s+origin\s+master\s*',
                           fixed=False,
                           msg='Use `git pull origin master` to pull in changes.')
```

<!-- -------------------------------------------------------------------------------- -->

--- type:BulletConsoleExercise key:b3ba4dd987
## How can I push my changes to a remote repository?

The complement of `git pull` is `git push`,
which pushes the changes you have made locally into a remote repository.
The most common way to use it is:

```
git push remote-name branch-name
```

which pushes the contents of your branch `branch-name`
into a branch with the same name
in the remote repository associated with `remote-name`.
It's possible to use different branch names at your end and the remote's end,
but doing this quickly becomes confusing:
it's almost always better to use the same names for branches across repositories.

*** =pre_exercise_code
```{python}
repl = connect('bash')
repl.run_command('rm -rf dental')
repl.run_command('git clone file:///home/thunk/repo dental')
with open('dental/data/northern.csv', 'a') as writer:
    writer.write('2017-11-01,bicuspid\n')
repl.run_command('clear')
repl.run_command('cd dental')
```

*** =type1: ConsoleExercise
*** =key1: 75df26b3a7

*** =xp1: 10

*** =instructions1

You are in the `master` branch of the `dental` repository,
which has a remote called `origin`.
You have changed `data/northern.csv`;
add it to the staging area.

*** =hint1

Use `git add` with the name of the changed file.

*** =sample_code1
```{shell}
```

*** =solution1
```{shell}
git add data/northern.csv
```

*** =sct1
```{python}
Ex() >> test_student_typed(r'\s*git\s+add.+\s*',
                           fixed=False,
                           msg='Use `git add` as normal.')
```

*** =type2: ConsoleExercise
*** =key2: eebd73b616

*** =xp2: 10

*** =instructions2

Commit your changes with the message "Added more northern data."

*** =hint2

Use `git commit` with `-m` and a message in quotes.

*** =sample_code2
```{shell}
```

*** =solution2
```{shell}
git commit -m "Added more northern data."
```

*** =sct2
```{python}
Ex() >> test_student_typed(r'\s*git\s+commit.+',
                           fixed=False,
                           msg='Use `git commit -m "message"` as normal.')
```

*** =type3: ConsoleExercise
*** =key3: 037b960128

*** =xp3: 10

*** =instructions3

Push your changes to the remote repository's `master` branch.

*** =hint3

Use `git push` with the name of the remote (`origin`) and the name of the branch (`master`).

*** =sample_code3
```{shell}
```

*** =solution3
```{shell}
git push origin master
```

*** =sct3
```{python}
Ex() >> test_student_typed(r'\s*git\s+push\s+origin\s+master\s*',
                           fixed=False,
                           msg='Use `git push` with a remote name and a branch name.')
```

<!-- -------------------------------------------------------------------------------- -->

--- type:BulletConsoleExercise key:b34c87b35c
## What happens if my push conflicts with someone else's work?

Overwriting your own work by accident is bad;
overwriting someone else's is worse.
To prevent this happening,
Git does not allow you to push changes to a remote repository
unless you have merged the contents of the remote repository into your own work.

*** =pre_exercise_code
```{python}
repl = connect('bash')
repl.run_command('rm -rf dental')
repl.run_command('git clone file:///home/thunk/repo dental')
repl.run_command('git -C dental reset --hard HEAD~1')
with open('dental/data/northern.csv', 'a') as writer:
    writer.write('2017-11-01,bicuspid\n')
repl.run_command('clear')
repl.run_command('cd dental')
repl.run_command('git add data/northern.csv')
repl.run_command('git commit -m "Adding a record"')
```

*** =type1: ConsoleExercise
*** =key1: 2b8815a979

*** =xp1: 10

*** =instructions1

You have made changes to the `dental` repository.
Use `git push` to push those changes to the remote repository `origin`.

*** =hint1

Use `git push` with the name of the remote (`origin`) and the name of the branch (`master`).

*** =sample_code1
```{shell}
```

*** =solution1
```{shell}
git push origin master
```

*** =sct1
```{python}
Ex() >> test_student_typed(r'\s*git\s+push\s+origin\s+master\s*',
                           fixed=False,
                           msg='Use `git push` with the remote name and the branch name.')
```

*** =type2: ConsoleExercise
*** =key2: 0af344b542

*** =xp2: 10

*** =instructions2

In order to prevent you overwriting remote work,
Git has refused to execute your push.
Use `git pull` to bring your repository up to date with `origin`.

*** =hint2

Use `git pull` with the name of the remote (`origin`) and the name of the branch (`master`).

*** =sample_code2
```{shell}
```

*** =solution2
```{shell}
# Run this command *without* '--no-edit':
git pull --no-edit origin master
```

*** =sct2
```{python}
Ex() >> test_student_typed(r'\s*git\s+pull.*\s+origin\s+master\s*',
                           fixed=False,
                           msg='Use `git pull` with the remote name and the branch name.')
```

*** =type3: ConsoleExercise
*** =key3: 9ddb421159

*** =xp3: 10

*** =instructions3

Now that you have merged the remote repository's state into your local repository,
try the push again.

*** =hint3

Use `git push` with the name of the remote (`origin`) and the name of the branch (`master`).

*** =sample_code3
```{shell}
```

*** =solution3
```{shell}
git push origin master
```

*** =sct3
```{python}
Ex() >> test_student_typed(r'\s*git\s+push\s+origin\s+master\s*',
                           fixed=False,
                           msg='Use `git push` with the remote name and the branch name.')
```
