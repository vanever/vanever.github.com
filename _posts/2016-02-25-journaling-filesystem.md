---
layout: post
title: Journaling File System
---

Recently in my project, I need to redirect linux kernel network block request to local to form a cache layer.

I use local file to store a copy of network block device, which means the IO path is like this:

FS -> VFS -> BIO -> | -> VFS -> BIO

There are some drawbacks in this mode:

  1. double page cache
  2. double journal costs

To have a concept of what is the expense in double journal. We need to understand the journal mode in modern file systems.

EXT4 has three modes:
* writeback: journal metadata changes only, fastest but weakest
* ordered: in my centos 7, this is the default journal mode, journals metadata as well as the related data changes which cause the metadata to change
* journal: journal all metadata and data changes

XFS only journal metadata, just like the `writeback` mode in EXT4.

Hence, looks like default xfs and ext4 mount options, the double journal costs can be accepted.

