# C# Garbage Collection

## Fundamentals of memory

- Each process has its own, separate virtual address space. All processes on the same computer share the same physical memory, and share the page file if there is one.
- As an application developer, you work only with virtual address space and never manipulate physical memory directly. The garbage collector allocates and frees virtual memory for you on the managed heap.
- Virtual memory can be in three states:
  - Free. The block of memory has no references to it and is available for allocation.
  - Reserved. The block of memory is available for your use and cannot be used for any other allocation request. However, you cannot store data to this memory block until it is committed.
  - Committed. The block of memory is assigned to physical storage.
  - Virtual address space can get fragmented. This means that there are free blocks, also known as holes, in the address space.
  - You can run out of memory if you run out of virtual address space to reserve or physical space to commit.

## Conditions for a garbage collection

- The system has low physical memory. This is detected by either the low memory notification from the OS or low memory indicated by the host.
- The memory that is used by allocated objects on the managed heap surpasses an acceptable threshold. This threshold is continuously adjusted as the process runs.
- The GC.Collect method is called. In almost all cases, you do not have to call this method, because the garbage collector runs continuously. This method is primarily used for unique situations and testing.

## The managed heap

There is a managed heap for each managed process. All threads in the process allocate memory for objects on the same heap.

> **Important**
> The size of segments allocated by the garbage collector is implementation-specific and is subject to change at any time, including in periodic updates. Your app should never make assumptions about or depend on a particular segment size, nor should it attempt to configure the amount of memory available for segment allocations.

回收大小跟頻率都是動態調整

>The intrusiveness (frequency and duration) of garbage collections is the result of the volume of allocations and the amount of survived memory on the managed heap.

## Generations

- **Generation 0**. This is the youngest generation and contains short-lived objects. An example of a short-lived object is a temporary variable. Garbage collection occurs most frequently in this generation.<br>
Newly allocated objects form a new generation of objects and are implicitly generation 0 collections, unless they are large objects, in which case they go on the large object heap in a generation 2 collection.<br>
Most objects are reclaimed for garbage collection in generation 0 and do not survive to the next generation.

- **Generation 1**. This generation contains short-lived objects and serves as a buffer between short-lived objects and long-lived objects.

- **Generation 2**. This generation contains long-lived objects. An example of a long-lived object is an object in a server application that contains static data that is live for the duration of the process.

Garbage collections occur on specific generations as conditions warrant. Collecting a generation means collecting objects in that generation and all its younger generations. A generation 2 garbage collection is also known as a full garbage collection, because it reclaims all objects in all generations (that is, all objects in the managed heap).

### Survival and promotions

- Objects that are not reclaimed in a garbage collection are known as survivors, and are promoted to the next generation.
- When the garbage collector detects that the survival rate is high in a generation, it increases the threshold of allocations for that generation, so the next collection gets a substantial size of reclaimed memory. The CLR continually balances two priorities: not letting an application's working set get too big and not letting the garbage collection take too much time.

### Ephemeral generations and segments

Because objects in generations 0 and 1 are short-lived, these generations are known as the ephemeral generations.

Ephemeral generations must be allocated in the memory segment that is known as the ephemeral segment. Each new segment acquired by the garbage collector becomes the new ephemeral segment and contains the objects that survived a generation 0 garbage collection. The old ephemeral segment becomes the new generation 2 segment.

## What happens during a garbage collection

A garbage collection has the following phases:

- A marking phase that finds and creates a list of all live objects.
- A relocating phase that updates the references to the objects that will be compacted.
- A compacting phase that reclaims the space occupied by the dead objects and compacts the surviving objects. The compacting phase moves objects that have survived a garbage collection toward the older end of the segment.

The garbage collector uses the following information to determine whether objects are live:

- **Stack roots**. Stack variables provided by the just-in-time (JIT) compiler and stack walker. Note that JIT optimizations can lengthen or shorten regions of code within which stack variables are reported to the garbage collector.
- **Garbage collection handles**. Handles that point to managed objects and that can be allocated by user code or by the common language runtime.
- **Static data**. Static objects in application domains that could be referencing other objects. Each application domain keeps track of its static objects.

>Before a garbage collection starts, all managed threads are suspended except for the thread that triggered the garbage collection.

所有的managed threads在等待觸發GC的thread完成GC前會suspended

## Manipulating unmanaged resources

When a finalizable object is discovered to be dead, its finalizer is put in a queue so that its cleanup actions are executed, but the object itself is promoted to the next generation. Therefore, you have to wait until the next garbage collection that occurs on that generation (which is not necessarily the next garbage collection) to determine whether the object has been reclaimed.

Finalize就當成一般GC物件，需要等待GC觸發，你也可以用Dispose就等於手動觸發。

## Workstation and server garbage collection

靠北如果覺得GC自己tune的不好你還可以塞configuration file setting給他...

後面感覺對碼農幫助不大，不想看了...