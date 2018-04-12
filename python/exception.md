## NotImplemented is not an Exception

reference: https://help.semmle.com/wiki/display/PYTHON/NotImplemented+is+not+an+Exception

`NotImplemented` is not an Exception, but is often mistakenly used in place of `NotImplementedError`. Executing `raise NotImplemented` or `raise NotImplemented()` will raise a `TypeError`.
