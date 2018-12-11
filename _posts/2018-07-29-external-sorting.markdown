---
layout: default
title:  "External Sorting"
date:   2018-07-29
categories: shell bash git vim
---

* TOC
{:toc}
## External Sorting

How will you sort a file whose size is greater than amount of memory available?



For e.g. If you are given a 20GB file and ~1GB of memory (heap size), how will you sort the entries in the file?

Make sure to reduce the number of disk seeks that you do. This is the most expensive part of the sort. Once you seek the head of the disk to the correct location, reading in subsequent bytes are cheaper.

### Sorting individual chunks

The first step is to logically split the original file into chunks of size equal to the available memory. Call this the CHUNK_SIZE. The last chunk can contain number of items less than the CHUNK_SIZE.

Read the first chunk from the original file, sort it using an in-memory sort like Merge sort or Quick sort and write the chunk back to the disk as a separate file.

Repeat this for all the chunks.

Now we have split the original file into chunks and each chunks is sorted. At this point you can delete the original file if you want.



``CHUNK_SIZE = Available memory size``

``CHUNKS = n/CHUNK_SIZE``

|                 | Per chunk                                                    | Entire file                               |
| --------------- | ------------------------------------------------------------ | ----------------------------------------- |
| Time complexity | ``O(CHUNK_SIZE log CHUNK_SIZE)``                             | ``O(CHUNKS * CHUNK_SIZE log CHUNK_SIZE)`` |
| Disk seeks      | ``2`` (1 for reading from the huge file and 1 for writing the sorted chunk) | ``2 * CHUNKS``                            |

### Merging the sorted chunks  

The next step is to merge the sorted chunks to form a single file. Once again of the entire file can fit in the available memory, we can do an in-memory merge.

There are 2 approaches to merging the individual chunks

* Perform a k-way merge
* Perform multiple 2-way merges

#### K-way merge

K is equal to the number of chunks i.e ``k = CHUNKS``. In this approach, a small portion of each chunk is read into the memory and we create a min-heap out of the first element from each chunk (which is also the smallest element in each chunk). If we remove the smallest element from the min-heap now, we know that it is the smallest element in the given input file. The data type inserted into the min-heap has reference to the chunk from  which the item was fetch, so when a value from the min-heap is removed, we read the next item (i.e. next smallest element) from the chunk and insert it into the heap. The output is written to a single huge file as and when the output buffer becomes full. If we continue doing this, we would have merged all the chunks into a single file.

If we read from the next smallest element from a chunk as and when needed, we will incur a disk seek. So there will be ``n`` disk seeks - making the method very inefficient.

We can create a buffer for each chunk and a bigger buffer for the output. Let the output buffer size be ``OUTPUT_BUFFER_SIZE``.The buffer size for the chunks will be ``BUFFER_SIZE = (CHUNK_SIZE - OUTPUT_BUFFER_SIZE) /CHUNKS`` i.e. we split the remaining available memory among the chunks. Now for every disk seek of a chunk file, we fill the buffer allotted to it completely and go back to the chunk for more data only when the buffer becomes empty. With this improved approach, number of disk seeks for writing the output file = ``n/OUTPUT_BUFFER_SIZE``. Number of disk seeks to read the chunks while merging = ``CHUNKS * (CHUNK_SIZE/BUFFER_SIZE)`` = ``n/BUFFER_SIZE``.

|                 |                                                              |
| --------------- | ------------------------------------------------------------ |
| Time complexity | ``O(n log k)``                                               |
| Disk seeks      | ``n/OUTPUT_BUFFER_SIZE`` + ``n/BUFFER_SIZE`` = ``CHUNKS^2 + CHUNKS^3`` |



#### Multiple 2-way merges

In this approach, we merge 2 files at a time. At each stage, we merge 2 files, producing new chunks. The number of chunks will decrease by a factor of 2 in each stage. We repeat this until a single file is produced as the output.

We can split the available memory (``CHUNK_SIZE``) into 4 parts - we allocate 2 parts for the output buffer and 1 part each for each of the 2 chunks being merged. We read from the chunk file until we fill its buffer and go back to reading it again only when the buffer is empty. We write to the output file only when the output buffer is full.

Number of stages = ``log (CHUNKS)``

Time complexity of merging in a stage = O(n)

Assume that the CHUNK_SIZE(stage) is the size of the input chunks to the stage. (Note: you can also assume that it is the size of the output chunks produced by the stage, but change the calculations appropriately).



CHUNK_SIZE(1) = CHUNK_SIZE (i.e. available memory) 

CHUNK_SIZE(stage + 1) = CHUNK_SIZE(stage) * 2



CHUNKS(1) = CHUNKS or n/CHUNK_SIZE

CHUNKS(stage + 1) = CHUNKS(stage)/2

As the number of stages increase, the chunk size increases by a factor of 2 and the number of chunks decrease by a factor of 2.

OUTPUT_BUFFER_SIZE = CHUNK_SIZE(1)/ 2

BUFFER_SIZE = CHUNK_SIZE(1)/4

Number of disk seeks per merge = 

``CHUNK_SIZE(stage + 1)/OUTPUT_BUFFER_SIZE + 2 * CHUNK_SIZE(stage) / BUFFER_SIZE``



Number of disk seeks per stage = ``(CHUNKS(stage)/2) * (CHUNK_SIZE(stage + 1)/OUTPUT_BUFFER_SIZE + 2 * CHUNK_SIZE(stage) / BUFFER_SIZE)``



Number of seeks for all the stages = ``6 * CHUNKS (log CHUNKS)``



### Conclusion

Based on the number of seeks (which forms the major cost factor of external sorting), multistage 2-way merges are better than the k-way merge.
