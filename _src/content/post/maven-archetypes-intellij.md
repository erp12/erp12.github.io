---
title: "Managing Maven Archetypes in IntelliJ"
date: 2017-09-04T00:29:43-04:00
draft: false
---

<iframe class="post-vid" width="560" height="315" src="https://www.youtube.com/embed/rYDp-BNkLFg" frameborder="0" allowfullscreen></iframe>

Recently I started using [IntelliJ](https://www.jetbrains.com/idea/) for Scala development. When you install
IntelliJ you can also install it's Scala plugin and hit the ground running using
[SBT](http://www.scala-sbt.org/). However, I needed a maven project.

Starting maven projects is much easier when you start from a maven archetype.
An archetype is a template of a project. In my case, I wanted to use a basic
Scala archetype which provided everything I would need to write my application
and tests in Scala.

If you take a look at the list of default archetypes that IntelliJ knows about
at install, you will see one named `scala-archetype-simple`. You might think
that is the perfect archetype for my needs, however I wasn't able to get it to
work and every online source pointed me towards using a different archetype,
which we need to add to IntelliJ.

## Adding an Archetype to IntelliJ

The "Add Archetype..." button is what we want (duh). A simple dialog pops up
and we can enter the GroupId, ArtifactId, and Version of the archetype we would
like to add. In my case I entered:

```
groupId: net.alchim31.maven
artifactId: scala-archetype-simple
version: 1.6
```

When we click "OK", the archetype is added to the list in the IntelliJ create
project wizard. Every time we start a new Maven project, we should see the new
archetype in the list.

What if we speed through this process and make a typo? What if we accidently
add the same Archetype twice? ... I did these things, and my archetype list
got a little cluttered... but how do we remove elements from the archetype list?

## Removing an Archetype from IntelliJ

Unfortunately, there is no simple "Remove Archetype" button. Luckily, all the
archetypes added by users are stored in a simple XML file which we can edit.
The trick is finding where the `UserArchetypes.xml` file is our computer.

It varies based on operating system. Here are all the locations I could find
online:

- Mac: `~/Library/Caches/IntelliJIdea12/Maven/Indices/UserArchetypes.xml`
- Linux: `~/.IntelliJIdea10/system/Maven/Indices/UserArchetypes.xml`
- Windows: `C:\Users\[USR]\.IdeaIC2017\system\Maven\Indices\UserArchetypes.xml`


> Note: For the windows path, replace [USR] with your PC username. You may
also have to change the name of the `.IdeaIC2017` folder depending on what
version of IntelliJ you are using.

Open the `UserArchetypes.xml` file in your favorite text editor, find the line
which specifies the archetype you would like to remove from the IntelliJ list.
Delete the XML entry, save the file, and restart IntelliJ. You should see that
archetype is no longer in the list!

## Sources

- https://maven.apache.org/guides/introduction/introduction-to-archetypes.html
- https://stackoverflow.com/questions/30080293/scala-signature-error-for-scala-module-in-intellij-idea-maven-project
- https://stackoverflow.com/questions/4361567/where-are-added-archetypes-stored-in-intellij
