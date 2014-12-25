---
layout: post
title: "git-repo delete obsolete projects"
date: 2014-05-25 00:23:17 +0800
comments: true
categories: programming
tags: programming python git repo
---

The place I work for uses git-repo to manage multiple git projects.

It's quite convenient when we want the same git action to be performed on multiple projects.

However, some of the features from repo are designed for AOSP, while not really suitable for managing our own projects.

One is when we repo init to another manifest, repo sync will delete every project in previous manifest but not in current one.

This blog marks what know from tracing git-repo source code.

repo architecture
====

When we download the repo file according to AOSP instruction, we only download a file they called 'launcher'.

This launcher provides very limited functions. Most other commands are downloaded through 'init' process.
