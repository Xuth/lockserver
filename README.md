# lockserver
A simple python lockserver with no extra dependencies.

basic usage:

To start the server on the default port simply run lockserver.py
```
$ python lockserver.py
```

To use locks in your code
```
import lockserver

with lockserver.Lock("lock_name"):
    print "doing stuff under lock <lock_name>"

with lockserver.SharedLock("lock_name"):
    print "doing stuff under shared lock <lock_name>"
    
count = lockserver.lockAccessCount("lock_name")
print "there are %d locks currently held for <lock_name>"
```

You can also use the Lock() class outside of a context manager (and this is required if you wish to 
test the locks and not wait for them).

---

The server is a single threaded select/poll socket server written only with core python libraries.  I specifically
like writing servers this way because it makes it very difficult to create race conditions and other resource
contention problems you get with multithreaded code even if it is often easier to write servers in general in 
a multithreaded or multiprocess fashion.

---

I wrote this a while ago when I wanted a basic lockserver that would work across multiple systems without requiring 
a shared filesystem and without running a database or similar.  I did manage to find a few other attempts at python
lockservers before writing my own but all of the ones I found had very obvious race conditions that weren't easy to
fix.  I then built it up for my specific environment but here it is after I've stripped most of that back out to be
more general purpose.

Things that it currently doesn't have but would be useful:

* timeouts on locks
* ability to set the port and bind address on the command line
* deleting the lockserver.info file on quit.
