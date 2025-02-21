# Homework F - Filesystem Organization

For problems 1-4 draw a **neat detailed sketch** of the described situation.
Practice doing all computations easily and quickly using [metric units](metric.md)
as we have done in class.  The size of all objects should be written as a small number
accompanied by appropriate binary metric units.  For example, write `4MB` and not `2^32 bytes` and definitely not `4294967296 bytes`.

1 - Suppose that you have a conventional Unix filesystem with the following properties:
- 16 bit block address
- 4KB block size
- 4 direct pointers and one indirect pointer per inode

a) What is the largest **disk** that this filesystem could possibly use?
b) What is the largest **file** that this filesystem can store?
c) Assuming a disk of 8MB, how large is the free block bitmap?

2 - Suppose that you have a conventional Unix filesystem with the following properties:
- 32 bit block address
- 16KB block size
- 8 direct pointers, one indirect pointer, and one doubly-indirect pointer per inode

a) What is the largest **disk** that this filesystem could possibly use?
b) What is the largest **file** that this filesystem can store?
c) Assuming a disk of 512GB, how large is the free block bitmap?

3 - Suppose that a future storage device offers 1PB of data storage organized in 1MB blocks.
You are tasked with desiging a Unix like filesystem such that it should be possible for
a single file to grow and fill the entire device.

a) How would you choose the filesystem parameters to make this possible?
b) Are there any downside consequences to this design decision?

4 - A commonly requested feature for filesystems are "extended tags" that allow
end users to attach an arbitrary number of key-value tags to files.
For example: `camera=nikon` or `artist=prince` or `class=opsys-sp25`.
This certainly would be handy for searching and organizing different kinds of files.
Some filesystem designs support this, but many do not.

a) Suggest where in the filesystem data structures would be a good place to store such tags.
b) What potential complications do you think would arise in a filesystem
that permitted the user to apply tags to files arbitrarily?
