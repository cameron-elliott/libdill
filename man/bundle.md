# NAME

bundle - create an empty coroutine bundle

# SYNOPSIS

**#include &lt;libdill.h>**

**int bundle(void);**

# DESCRIPTION

Coroutines are always running in bundles. Even coroutine created by **go()** gets its own bundle. Bundle is a lifetime control mechanism. When it is closed all coroutines within the bundle are canceled.

This function creates an empty bundle. Coroutines can be added to the bundle using **bundle_go()** and **bundle_go_mem()** functions.

# RETURN VALUE

Returns a handle to the bundle. In the case of an error, it returns -1 and sets _errno_ to one of the values below.

# ERRORS

* **ECANCELED**: Current coroutine is in the process of shutting down.
* **ENOMEM**: Not enough memory to allocate a new bundle.

# EXAMPLE

```c
int b = bundle();
bundle_go(b, worker());
bundle_go(b, worker());
bundle_go(b, worker());
msleep(now() + 1000);
/* Cancel any workers that are still running. */
hclose(b);
```
