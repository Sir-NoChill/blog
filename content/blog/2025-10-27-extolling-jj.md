---
title: Extolling JJ.
description: Talking to Rocks, post 2.
date: 2025-10-27
tags: [vcs, git, jj]
---

JJ is a new version control system built by some folks over at google and man.
I must say I am enjoying using it. (full name jujutsu)

## Preamble

You can read more about jj over on their [github](https://github.com/jj/jj-vcs),
but in short it is a version control system that abstracts away the storage
layer of the vcs and just tracks 'changes'. In practice, this means that you
don't need to think about how you want to structure your change tree until
after you've made your changes.

This is not a JJ tutorial. Go to the official docs if you want to learn.

As a relatively disorganized person, I tend to make very bad commits. Keeping
track of what I'm doing takes a lot of effort in git, and ensuring that
everything remains clean, synced and mergeable is hard. With JJ, there is this
new mechanic at play where I can modify how my changes 'look' in the revision
history and do many parallel 'changes' all at once without necessarily having
to think about it before hand.

I want to catalogue, for both myself and maybe even to help others:
- The workflows that seem to exist in the JJ community
- Tips I found in the community
- My (opinionated) suggestions on how to 'best' use the JJ ecosystem as someone
  migrating from git.

## Defining the JJ Workflow

In the JJ community, there are a number of blogs that talk about different
workflows. This is my attempt to build a sort of taxonomy of what I've observed
and what other people do. Broadly speaking I see (assuming a git background):
- Monolithic Commits, Granular Changes, Single Path of Reasoning
- Monolithic Commits, Monolithic Changes, Single Path of Reasoning
- Monolithic Commits, Granular Changes, Multiple Paths of Reasoning
- Monolithic Commits, Monolithic Changes, Multiple Paths of Reasoning
- Granular Commits, Granular Changes, Single Path of Reasoning
- Granular Commits, Granular Changes, Multiple Paths of Reasoning

So on three axes:
1. The granularity of the commits to the underlying storage system
2. The granularity of the changes you make to your actual repo (the jj layer)
3. The way that the 'writer of code' goes about making changes conceptually

### Granularity of Commits to the Storage System

In JJ, there is this implicit mapping between the jj commits you make and the
git commits you make (assuming a git backend again). When you make your commit
to the underlying storage system, the jj change that is mapped to that commit
becomes immutable.

Within this axis, I observe that folks choose to:
1. Decide to keep a running list of changes, and commit them once they are done
   with what would normally be one git operation
2. Keep a continually changing jj change, and when they are done commit the
   single change to the underlying git repo
3. As with 1, except instead of committing all of the changes you squash them
   into a single commit
4. Some frankenstein depending on what you feel like.

### The Case For Mutability

I think that all of these workflows are well supported, just since jj is so
versatile, but I think it's important to note that jj has mutable changes by
default, which (to me) means that we (arguably?? This requires a conversation on
what exactly youo want your VCS history to represent, I'll make another post on
that at some point) _should_ go back and modify our changes.

I justify that way of thought because when I do something for the first time I'm
very unfocused. The same is true of me writing code, so when I implement a
feature for the first time, it probably sucks and I probably tried a whole bunch
of different things in order to get it to work. Thus my jj history probably
looks something like

```
                      *
                      |
                      * 6d7e8f1 (I liked this)
                     /
        *           /
        |          /
        *         /
       /         /
*-----*----*----*----*----*--------*----*-----*---- main
      \                   |   (c-p 6d...)
       \                  * ----*----*----*-------* feature
        |                              (c-p d34...)
        \
         * ---*---*-- * (this too d34...)

```

(who would guess claude can do ascii art) point being that I have some feature
that I had a few false starts with, and there are some good commits in the
false start that I want to have in my feature. The feature then needs to be
merged (which sucks cause other people are more productive than me and have
already merged changes).

With JJ, I can take all of those dead branches and cherry pick everything
together, making it nice, to the point of being confident enough to show the
result outside of my disk. This works because when you make a new jj change
you don't need to rewrite all of your history no matter how messy it is.
You can `jj split` and `jj squash` and `jj rebase` to your hearts content
before ever making a change that others can see. You don't even need to
[rewrite history](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History)
in order for it to work.

Can I do that in git? Yes. But I am less fearful of doing it in jj because there
is another feature where it keep track of an `evlog` which is what goes on in
any given change, so you can reset to any state (even if you didn't jj commit)
and still have a nice clean history.

All of this happens on top of the underlying git repo. So none of your foibles
are visible in the base repo regardless of if you push your changes.

### The Case Against Mutability

Sometimes it is just nice to know exactly what happened. Git is ideal for this,
because the vcs _is_ the way it is stored. I am a personal fan of rebase
workflows because they tell one concise story throughout the development of a
project or feature, and it is easy to see where they started. JJ only makes
your commits immutable upon making a push to the git server, which in my opinion
is nice, but I can see where others would disagree.

When I was working on a compiler in my undergraduate, our team made the decision
to use a rebase workflow largely because of a strong-willed team member. However
the concern was raised that rebasing (which requires history rewrites) fails to
preserve the actual history of the project. I think of the rebase workflow a lot
like how my supervisor describes writing papers: we want to tell a story,
without boring the reader with what we failed to do, tried and otherwise
abandoned.

### What was JJ Intended to Do?

Truth is, I'm not quite sure at this moment. I think you are supposed to make
your commits however your specific project wants them to be. JJ does a nice
job of allowing this and keeping your own workflow constant across repos, so
this area of the taxonomy is (arguably) the least important.

## Useful Links

The following are some useful links for using JJ:

- https://andre.arko.net/2025/09/28/stupid-jj-tricks/
- https://jj-vcs.github.io/jj/latest/config/ (as a config wrangler)
- https://steveklabnik.github.io/jujutsu-tutorial/introduction/what-is-jj-and-why-should-i-care.html (for those new to it)


