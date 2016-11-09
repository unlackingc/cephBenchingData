---
title: journal file system
tags: journal,FileSystem
grammar_cjkRuby: true
---
[toc]

# 特征
1. The journal is a special file that logs the changes destined for the file system in a circular buffer。[出处][1]

# 不同的journal策略

Three of the most common strategies are writeback, ordered, and data.[出处][2][metadata journal]
1. data journal = direct journal (直接写内容)
2. metadata journal  （{ordered journal || writeback journal} ∈ metadata journal）(先写data block再写journal[出处][3][P14])

# 现有的文件系统
1. jfs2(extents)
2. xfs
3. ext3
4. ReiserFS

# journal 如何工作详细
[出处][3]

# 其他
1. rate input/output (I/O—through bandwidth reservation for file system users) and direct I/O, where data is copied directly between the disk and the user space buffer (rather than being staged through multiple buffers). 2. Although the delayed allocation method reduces fragmentation, over time, a large file system can become fragmented. An online defragmentation tool (e4defrag) has been developed to address this. You can use the tool to defragment individual files or an entire file system
2. The basic idea is as follows. When updating the disk, before overwriting the structures in place, first write down a little note (somewhere else on the disk, in a well-known location) describing what you are about to do. Writing this note is the “write ahead” part, and we write it to a structure that we organize as a “log”; hence, write-ahead logging.
3. By forcing the data write first, a file system can guarantee that a pointer
will never point to garbage. Indeed, this rule of “write the pointed to object
before the object with the pointer to it” is at the core of crash consistency,
and is exploited even further by other crash consistency schemes
[GP94] (see below for details).[出处][3][P14]
4. Entitled optimistic crash consistency [C+13], this new approach issues as many writes to disk as possible and uses a generalized form of the transaction checksum [P+05], as well as a few other techniques, to detect inconsistencies should they arise

# 其他一致性方法
1. Soft Updates[出处][3][P17] （Soft Updates requires intricate knowledge of each file system data structure and thus adds a fairamount of complexity to the system.）
2. copy-on-write（只写新文件，即使在附加写的时候也把整个文件重新写一遍）
3. backpointer-based consistency
# 文献
[维基百科][4]


  [1]: http://www.ibm.com/developerworks/library/l-journaling-filesystems/index.html
  [2]: http://www.ibm.com/developerworks/library/l-journaling-filesystems/index.html
  [3]: http://pages.cs.wisc.edu/~remzi/OSTEP/file-journaling.pdf
  [4]: https://en.wikipedia.org/wiki/Journaling_file_system
