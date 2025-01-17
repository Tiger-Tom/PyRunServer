# `ShaeLib` (`RunServer.ShaeLib` | `RS.SL`)
[Standalone doc: parts/RunServer/RunServer.ShaeLib.md](RunServer.ShaeLib)  

## `concurrency` (`RunServer.ShaeLib.concurrency` | `RS.SL.concurrency`)
[Standalone doc: parts/RunServer/ShaeLib/RunServer.ShaeLib.concurrency.md](RunServer.ShaeLib.concurrency)  

### `locked_resource` (`RunServer.ShaeLib.concurrency.locked_resource` | `RS.SL.concurrency.locked_resource`)
[Standalone doc: parts/RunServer/ShaeLib/concurrency/RunServer.ShaeLib.concurrency.locked_resource.md](RunServer.ShaeLib.concurrency.locked_resource)  

#### `LockedResource` (`RunServer.ShaeLib.concurrency.locked_resource.LockedResource` | `RS.SL.concurrency.locked_resource.LockedResource`)
[`_rsruntime/ShaeLib/concurrency/locked_resource.py`](/_rsruntime/ShaeLib/concurrency/locked_resource.py "Source")  
[Standalone doc: parts/RunServer/ShaeLib/concurrency/locked_resource/RunServer.ShaeLib.concurrency.locked_resource.LockedResource.md](RunServer.ShaeLib.concurrency.locked_resource.LockedResource)  
> Adds a "lock" parameter to class instances (and slots!)  
> This should be used in tandem with the @locked decorator:
>> class DemoLocked(LockedResource): # note subclass
>>> def __init__(self):
>>>> super().__init__() # note super init, needed to setup .lock  
>>>> print("initialized!")
>>> @locked # note decorator  
>>> def test_lock(self):
>>>> print("lock acquired!")

#### `basic` (`RunServer.ShaeLib.concurrency.locked_resource.basic` | `RS.SL.concurrency.locked_resource.basic`)
#####  OR `b` (`RunServer.ShaeLib.concurrency.locked_resource.b` | `RS.SL.concurrency.locked_resource.b`)
[`_rsruntime/ShaeLib/concurrency/locked_resource.py`](/_rsruntime/ShaeLib/concurrency/locked_resource.py "Source")  
[Standalone doc: parts/RunServer/ShaeLib/concurrency/locked_resource/RunServer.ShaeLib.concurrency.locked_resource.basic.md](RunServer.ShaeLib.concurrency.locked_resource.basic)  
> basic(LockedResource, LR, locked, lockd)

##### locked(...)
```python
@staticmethod
def locked(func: Callable)
```

[`_rsruntime/ShaeLib/concurrency/locked_resource.py@76:92`](/_rsruntime/ShaeLib/concurrency/locked_resource.py#L76)

<details>
<summary>Source Code</summary>

```python
def locked(func: typing.Callable):
    '''
        Waits to acquire the method's self's .lock attribute (uses "with")
        This should be used in tandem with the LockedResource superclass:
            class DemoLocked(LockedResource): # note subclass
                def __init__(self):
                    super().__init__() # note super init, needed to setup .lock
                    print("initialized!")
                @locked # note decorator
                def test_lock(self):
                    print("lock acquired!")
    '''
    @wraps(func)
    def locked_func(self: LockedResource, *args, **kwargs):
        with self.lock:
            return func(self, *args, **kwargs)
    return locked_func
```
</details>

> Waits to acquire the method's self's .lock attribute (uses "with")  
> This should be used in tandem with the LockedResource superclass:
>> class DemoLocked(LockedResource): # note subclass
>>> def __init__(self):
>>>> super().__init__() # note super init, needed to setup .lock  
>>>> print("initialized!")
>>> @locked # note decorator  
>>> def test_lock(self):
>>>> print("lock acquired!")

##### locked(...)
```python
@staticmethod
def locked(func: Callable)
```

[`_rsruntime/ShaeLib/concurrency/locked_resource.py@76:92`](/_rsruntime/ShaeLib/concurrency/locked_resource.py#L76)

<details>
<summary>Source Code</summary>

```python
def locked(func: typing.Callable):
    '''
        Waits to acquire the method's self's .lock attribute (uses "with")
        This should be used in tandem with the LockedResource superclass:
            class DemoLocked(LockedResource): # note subclass
                def __init__(self):
                    super().__init__() # note super init, needed to setup .lock
                    print("initialized!")
                @locked # note decorator
                def test_lock(self):
                    print("lock acquired!")
    '''
    @wraps(func)
    def locked_func(self: LockedResource, *args, **kwargs):
        with self.lock:
            return func(self, *args, **kwargs)
    return locked_func
```
</details>

> Waits to acquire the method's self's .lock attribute (uses "with")  
> This should be used in tandem with the LockedResource superclass:
>> class DemoLocked(LockedResource): # note subclass
>>> def __init__(self):
>>>> super().__init__() # note super init, needed to setup .lock  
>>>> print("initialized!")
>>> @locked # note decorator  
>>> def test_lock(self):
>>>> print("lock acquired!")

#### `cls` (`RunServer.ShaeLib.concurrency.locked_resource.cls` | `RS.SL.concurrency.locked_resource.cls`)
[`_rsruntime/ShaeLib/concurrency/locked_resource.py`](/_rsruntime/ShaeLib/concurrency/locked_resource.py "Source")  
[Standalone doc: parts/RunServer/ShaeLib/concurrency/locked_resource/RunServer.ShaeLib.concurrency.locked_resource.cls.md](RunServer.ShaeLib.concurrency.locked_resource.cls)  
> cls(LockedClass, LC, classlocked, clslockd, iclasslocked, iclslockd)

##### LockedClass(...)
```python
@staticmethod
def LockedClass(lock_class: AbstractContextManager = RLock, I_KNOW_WHAT_IM_DOING: bool = False)
```

[`_rsruntime/ShaeLib/concurrency/locked_resource.py@52:74`](/_rsruntime/ShaeLib/concurrency/locked_resource.py#L52)
> Adds a "classlock" class variable  
> This should be used in tandem with either the @classlocked or @iclasslocked decorators
>> see help(classlockd) or help(iclasslocked) for real demo code
> Note that, unless you pass I_KNOW_WHAT_IM_DOING=True, lock_class is instantiated immediately in order to check if it is an instance of AbstractContextManager
>> This is to try to help emit a warning if LockedClass is used on a user class without being called (@LockedClass instead of @LockedClass())  
>> The lock_class must be instantiated to check, as threading.RLock and threading.Lock are actually functions  
>> I_KNOW_WHAT_IM_DOING=True disables both the immediate instantiation of lock_class, the isinstance check, and the warning
> lock_class is the type of lock (or really any context manager will do) to use (defaults to RLock)  
> Short demo code:
>> @LockedClass()  
>> class Locked: ...
> or, to use a custom lock:
>> @LockedClass(threading.Semaphore)  
>> class CustomLocked: ...

##### LockedClass(...)
```python
@staticmethod
def LockedClass(lock_class: AbstractContextManager = RLock, I_KNOW_WHAT_IM_DOING: bool = False)
```

[`_rsruntime/ShaeLib/concurrency/locked_resource.py@52:74`](/_rsruntime/ShaeLib/concurrency/locked_resource.py#L52)
> Adds a "classlock" class variable  
> This should be used in tandem with either the @classlocked or @iclasslocked decorators
>> see help(classlockd) or help(iclasslocked) for real demo code
> Note that, unless you pass I_KNOW_WHAT_IM_DOING=True, lock_class is instantiated immediately in order to check if it is an instance of AbstractContextManager
>> This is to try to help emit a warning if LockedClass is used on a user class without being called (@LockedClass instead of @LockedClass())  
>> The lock_class must be instantiated to check, as threading.RLock and threading.Lock are actually functions  
>> I_KNOW_WHAT_IM_DOING=True disables both the immediate instantiation of lock_class, the isinstance check, and the warning
> lock_class is the type of lock (or really any context manager will do) to use (defaults to RLock)  
> Short demo code:
>> @LockedClass()  
>> class Locked: ...
> or, to use a custom lock:
>> @LockedClass(threading.Semaphore)  
>> class CustomLocked: ...

##### classlocked(...)
```python
@staticmethod
def classlocked(func: Callable)
```

[`_rsruntime/ShaeLib/concurrency/locked_resource.py@93:112`](/_rsruntime/ShaeLib/concurrency/locked_resource.py#L93)
> Similar to @locked, but uses cls's .classlock attribute  
> Does NOT imply classmethod (use @iclasslocked if you want to do that)  
> Meant to be used with the @LockedClass class decorator:
>> @LockedClass  
>> class DemoLockedClass:
>>> @classmethod # note: @classmethod BEFORE @classlocked  
>>> @classlocked # could both be replaced by a single @iclasslocked  
>>> def test_lock(cls):
>>>> print("class lock acquired!")
>>> @classlocked  
>>> def test_lock_2(self):
>>>> print("class lock acquired on non-classmethod!")

##### classlocked(...)
```python
@staticmethod
def classlocked(func: Callable)
```

[`_rsruntime/ShaeLib/concurrency/locked_resource.py@93:112`](/_rsruntime/ShaeLib/concurrency/locked_resource.py#L93)
> Similar to @locked, but uses cls's .classlock attribute  
> Does NOT imply classmethod (use @iclasslocked if you want to do that)  
> Meant to be used with the @LockedClass class decorator:
>> @LockedClass  
>> class DemoLockedClass:
>>> @classmethod # note: @classmethod BEFORE @classlocked  
>>> @classlocked # could both be replaced by a single @iclasslocked  
>>> def test_lock(cls):
>>>> print("class lock acquired!")
>>> @classlocked  
>>> def test_lock_2(self):
>>>> print("class lock acquired on non-classmethod!")

##### iclasslocked(...)
```python
@staticmethod
def iclasslocked(func: Callable)
```

[`_rsruntime/ShaeLib/concurrency/locked_resource.py@113:123`](/_rsruntime/ShaeLib/concurrency/locked_resource.py#L113)

<details>
<summary>Source Code</summary>

```python
def iclasslocked(func: typing.Callable):
    '''
        Is the same as @classlocked (it even calls it), but also wraps the method in classmethod
        Meant to be used with @LockedClass:
            @LockedClass()
            class Locked:
                @iclasslocked
                def classlocked_classmethod(cls):
                    print("class lock acquired!")
    '''
    return classmethod(classlocked(func))
```
</details>

> Is the same as @classlocked (it even calls it), but also wraps the method in classmethod  
> Meant to be used with @LockedClass:
>> @LockedClass()  
>> class Locked:
>>> @iclasslocked  
>>> def classlocked_classmethod(cls):
>>>> print("class lock acquired!")

##### iclasslocked(...)
```python
@staticmethod
def iclasslocked(func: Callable)
```

[`_rsruntime/ShaeLib/concurrency/locked_resource.py@113:123`](/_rsruntime/ShaeLib/concurrency/locked_resource.py#L113)

<details>
<summary>Source Code</summary>

```python
def iclasslocked(func: typing.Callable):
    '''
        Is the same as @classlocked (it even calls it), but also wraps the method in classmethod
        Meant to be used with @LockedClass:
            @LockedClass()
            class Locked:
                @iclasslocked
                def classlocked_classmethod(cls):
                    print("class lock acquired!")
    '''
    return classmethod(classlocked(func))
```
</details>

> Is the same as @classlocked (it even calls it), but also wraps the method in classmethod  
> Meant to be used with @LockedClass:
>> @LockedClass()  
>> class Locked:
>>> @iclasslocked  
>>> def classlocked_classmethod(cls):
>>>> print("class lock acquired!")

#### `cls_decors` (`RunServer.ShaeLib.concurrency.locked_resource.cls_decors` | `RS.SL.concurrency.locked_resource.cls_decors`)
[`_rsruntime/ShaeLib/concurrency/locked_resource.py`](/_rsruntime/ShaeLib/concurrency/locked_resource.py "Source")  
[Standalone doc: parts/RunServer/ShaeLib/concurrency/locked_resource/RunServer.ShaeLib.concurrency.locked_resource.cls_decors.md](RunServer.ShaeLib.concurrency.locked_resource.cls_decors)  
> cls_decors(LockedClass, LC)

##### LockedClass(...)
```python
@staticmethod
def LockedClass(lock_class: AbstractContextManager = RLock, I_KNOW_WHAT_IM_DOING: bool = False)
```

[`_rsruntime/ShaeLib/concurrency/locked_resource.py@52:74`](/_rsruntime/ShaeLib/concurrency/locked_resource.py#L52)
> Adds a "classlock" class variable  
> This should be used in tandem with either the @classlocked or @iclasslocked decorators
>> see help(classlockd) or help(iclasslocked) for real demo code
> Note that, unless you pass I_KNOW_WHAT_IM_DOING=True, lock_class is instantiated immediately in order to check if it is an instance of AbstractContextManager
>> This is to try to help emit a warning if LockedClass is used on a user class without being called (@LockedClass instead of @LockedClass())  
>> The lock_class must be instantiated to check, as threading.RLock and threading.Lock are actually functions  
>> I_KNOW_WHAT_IM_DOING=True disables both the immediate instantiation of lock_class, the isinstance check, and the warning
> lock_class is the type of lock (or really any context manager will do) to use (defaults to RLock)  
> Short demo code:
>> @LockedClass()  
>> class Locked: ...
> or, to use a custom lock:
>> @LockedClass(threading.Semaphore)  
>> class CustomLocked: ...

##### LockedClass(...)
```python
@staticmethod
def LockedClass(lock_class: AbstractContextManager = RLock, I_KNOW_WHAT_IM_DOING: bool = False)
```

[`_rsruntime/ShaeLib/concurrency/locked_resource.py@52:74`](/_rsruntime/ShaeLib/concurrency/locked_resource.py#L52)
> Adds a "classlock" class variable  
> This should be used in tandem with either the @classlocked or @iclasslocked decorators
>> see help(classlockd) or help(iclasslocked) for real demo code
> Note that, unless you pass I_KNOW_WHAT_IM_DOING=True, lock_class is instantiated immediately in order to check if it is an instance of AbstractContextManager
>> This is to try to help emit a warning if LockedClass is used on a user class without being called (@LockedClass instead of @LockedClass())  
>> The lock_class must be instantiated to check, as threading.RLock and threading.Lock are actually functions  
>> I_KNOW_WHAT_IM_DOING=True disables both the immediate instantiation of lock_class, the isinstance check, and the warning
> lock_class is the type of lock (or really any context manager will do) to use (defaults to RLock)  
> Short demo code:
>> @LockedClass()  
>> class Locked: ...
> or, to use a custom lock:
>> @LockedClass(threading.Semaphore)  
>> class CustomLocked: ...

#### `etc` (`RunServer.ShaeLib.concurrency.locked_resource.etc` | `RS.SL.concurrency.locked_resource.etc`)
[`_rsruntime/ShaeLib/concurrency/locked_resource.py`](/_rsruntime/ShaeLib/concurrency/locked_resource.py "Source")  
[Standalone doc: parts/RunServer/ShaeLib/concurrency/locked_resource/RunServer.ShaeLib.concurrency.locked_resource.etc.md](RunServer.ShaeLib.concurrency.locked_resource.etc)  
> etc(locked_by, lockdby)

##### locked_by(...)
```python
@staticmethod
def locked_by(lock: AbstractContextManager)
```

[`_rsruntime/ShaeLib/concurrency/locked_resource.py@125:132`](/_rsruntime/ShaeLib/concurrency/locked_resource.py#L125)

<details>
<summary>Source Code</summary>

```python
def locked_by(lock: AbstractContextManager):
    def func_locker(func: typing.Callable):
        @wraps(func)
        def locked_func(*args, **kwargs):
            with lock:
                return func(*args, **kwargs)
        return locked_func
    return func_locker
```
</details>

> <no doc>

##### locked_by(...)
```python
@staticmethod
def locked_by(lock: AbstractContextManager)
```

[`_rsruntime/ShaeLib/concurrency/locked_resource.py@125:132`](/_rsruntime/ShaeLib/concurrency/locked_resource.py#L125)

<details>
<summary>Source Code</summary>

```python
def locked_by(lock: AbstractContextManager):
    def func_locker(func: typing.Callable):
        @wraps(func)
        def locked_func(*args, **kwargs):
            with lock:
                return func(*args, **kwargs)
        return locked_func
    return func_locker
```
</details>

> <no doc>

#### `func_decors` (`RunServer.ShaeLib.concurrency.locked_resource.func_decors` | `RS.SL.concurrency.locked_resource.func_decors`)
[`_rsruntime/ShaeLib/concurrency/locked_resource.py`](/_rsruntime/ShaeLib/concurrency/locked_resource.py "Source")  
[Standalone doc: parts/RunServer/ShaeLib/concurrency/locked_resource/RunServer.ShaeLib.concurrency.locked_resource.func_decors.md](RunServer.ShaeLib.concurrency.locked_resource.func_decors)  
> func_decors(locked, lockd, classlocked, clslockd, iclasslocked, iclslockd, locked_by, lockdby)

##### classlocked(...)
```python
@staticmethod
def classlocked(func: Callable)
```

[`_rsruntime/ShaeLib/concurrency/locked_resource.py@93:112`](/_rsruntime/ShaeLib/concurrency/locked_resource.py#L93)
> Similar to @locked, but uses cls's .classlock attribute  
> Does NOT imply classmethod (use @iclasslocked if you want to do that)  
> Meant to be used with the @LockedClass class decorator:
>> @LockedClass  
>> class DemoLockedClass:
>>> @classmethod # note: @classmethod BEFORE @classlocked  
>>> @classlocked # could both be replaced by a single @iclasslocked  
>>> def test_lock(cls):
>>>> print("class lock acquired!")
>>> @classlocked  
>>> def test_lock_2(self):
>>>> print("class lock acquired on non-classmethod!")

##### classlocked(...)
```python
@staticmethod
def classlocked(func: Callable)
```

[`_rsruntime/ShaeLib/concurrency/locked_resource.py@93:112`](/_rsruntime/ShaeLib/concurrency/locked_resource.py#L93)
> Similar to @locked, but uses cls's .classlock attribute  
> Does NOT imply classmethod (use @iclasslocked if you want to do that)  
> Meant to be used with the @LockedClass class decorator:
>> @LockedClass  
>> class DemoLockedClass:
>>> @classmethod # note: @classmethod BEFORE @classlocked  
>>> @classlocked # could both be replaced by a single @iclasslocked  
>>> def test_lock(cls):
>>>> print("class lock acquired!")
>>> @classlocked  
>>> def test_lock_2(self):
>>>> print("class lock acquired on non-classmethod!")

##### iclasslocked(...)
```python
@staticmethod
def iclasslocked(func: Callable)
```

[`_rsruntime/ShaeLib/concurrency/locked_resource.py@113:123`](/_rsruntime/ShaeLib/concurrency/locked_resource.py#L113)

<details>
<summary>Source Code</summary>

```python
def iclasslocked(func: typing.Callable):
    '''
        Is the same as @classlocked (it even calls it), but also wraps the method in classmethod
        Meant to be used with @LockedClass:
            @LockedClass()
            class Locked:
                @iclasslocked
                def classlocked_classmethod(cls):
                    print("class lock acquired!")
    '''
    return classmethod(classlocked(func))
```
</details>

> Is the same as @classlocked (it even calls it), but also wraps the method in classmethod  
> Meant to be used with @LockedClass:
>> @LockedClass()  
>> class Locked:
>>> @iclasslocked  
>>> def classlocked_classmethod(cls):
>>>> print("class lock acquired!")

##### iclasslocked(...)
```python
@staticmethod
def iclasslocked(func: Callable)
```

[`_rsruntime/ShaeLib/concurrency/locked_resource.py@113:123`](/_rsruntime/ShaeLib/concurrency/locked_resource.py#L113)

<details>
<summary>Source Code</summary>

```python
def iclasslocked(func: typing.Callable):
    '''
        Is the same as @classlocked (it even calls it), but also wraps the method in classmethod
        Meant to be used with @LockedClass:
            @LockedClass()
            class Locked:
                @iclasslocked
                def classlocked_classmethod(cls):
                    print("class lock acquired!")
    '''
    return classmethod(classlocked(func))
```
</details>

> Is the same as @classlocked (it even calls it), but also wraps the method in classmethod  
> Meant to be used with @LockedClass:
>> @LockedClass()  
>> class Locked:
>>> @iclasslocked  
>>> def classlocked_classmethod(cls):
>>>> print("class lock acquired!")

##### locked(...)
```python
@staticmethod
def locked(func: Callable)
```

[`_rsruntime/ShaeLib/concurrency/locked_resource.py@76:92`](/_rsruntime/ShaeLib/concurrency/locked_resource.py#L76)

<details>
<summary>Source Code</summary>

```python
def locked(func: typing.Callable):
    '''
        Waits to acquire the method's self's .lock attribute (uses "with")
        This should be used in tandem with the LockedResource superclass:
            class DemoLocked(LockedResource): # note subclass
                def __init__(self):
                    super().__init__() # note super init, needed to setup .lock
                    print("initialized!")
                @locked # note decorator
                def test_lock(self):
                    print("lock acquired!")
    '''
    @wraps(func)
    def locked_func(self: LockedResource, *args, **kwargs):
        with self.lock:
            return func(self, *args, **kwargs)
    return locked_func
```
</details>

> Waits to acquire the method's self's .lock attribute (uses "with")  
> This should be used in tandem with the LockedResource superclass:
>> class DemoLocked(LockedResource): # note subclass
>>> def __init__(self):
>>>> super().__init__() # note super init, needed to setup .lock  
>>>> print("initialized!")
>>> @locked # note decorator  
>>> def test_lock(self):
>>>> print("lock acquired!")

##### locked(...)
```python
@staticmethod
def locked(func: Callable)
```

[`_rsruntime/ShaeLib/concurrency/locked_resource.py@76:92`](/_rsruntime/ShaeLib/concurrency/locked_resource.py#L76)

<details>
<summary>Source Code</summary>

```python
def locked(func: typing.Callable):
    '''
        Waits to acquire the method's self's .lock attribute (uses "with")
        This should be used in tandem with the LockedResource superclass:
            class DemoLocked(LockedResource): # note subclass
                def __init__(self):
                    super().__init__() # note super init, needed to setup .lock
                    print("initialized!")
                @locked # note decorator
                def test_lock(self):
                    print("lock acquired!")
    '''
    @wraps(func)
    def locked_func(self: LockedResource, *args, **kwargs):
        with self.lock:
            return func(self, *args, **kwargs)
    return locked_func
```
</details>

> Waits to acquire the method's self's .lock attribute (uses "with")  
> This should be used in tandem with the LockedResource superclass:
>> class DemoLocked(LockedResource): # note subclass
>>> def __init__(self):
>>>> super().__init__() # note super init, needed to setup .lock  
>>>> print("initialized!")
>>> @locked # note decorator  
>>> def test_lock(self):
>>>> print("lock acquired!")

##### locked_by(...)
```python
@staticmethod
def locked_by(lock: AbstractContextManager)
```

[`_rsruntime/ShaeLib/concurrency/locked_resource.py@125:132`](/_rsruntime/ShaeLib/concurrency/locked_resource.py#L125)

<details>
<summary>Source Code</summary>

```python
def locked_by(lock: AbstractContextManager):
    def func_locker(func: typing.Callable):
        @wraps(func)
        def locked_func(*args, **kwargs):
            with lock:
                return func(*args, **kwargs)
        return locked_func
    return func_locker
```
</details>

> <no doc>

##### locked_by(...)
```python
@staticmethod
def locked_by(lock: AbstractContextManager)
```

[`_rsruntime/ShaeLib/concurrency/locked_resource.py@125:132`](/_rsruntime/ShaeLib/concurrency/locked_resource.py#L125)

<details>
<summary>Source Code</summary>

```python
def locked_by(lock: AbstractContextManager):
    def func_locker(func: typing.Callable):
        @wraps(func)
        def locked_func(*args, **kwargs):
            with lock:
                return func(*args, **kwargs)
        return locked_func
    return func_locker
```
</details>

> <no doc>

#### `locked` (`RunServer.ShaeLib.concurrency.locked_resource.locked` | `RS.SL.concurrency.locked_resource.locked`)
[`_rsruntime/ShaeLib/concurrency/locked_resource.py`](/_rsruntime/ShaeLib/concurrency/locked_resource.py "Source")  
[Standalone doc: parts/RunServer/ShaeLib/concurrency/locked_resource/RunServer.ShaeLib.concurrency.locked_resource.locked.md](RunServer.ShaeLib.concurrency.locked_resource.locked)  
> Waits to acquire the method's self's .lock attribute (uses "with")  
> This should be used in tandem with the LockedResource superclass:
>> class DemoLocked(LockedResource): # note subclass
>>> def __init__(self):
>>>> super().__init__() # note super init, needed to setup .lock  
>>>> print("initialized!")
>>> @locked # note decorator  
>>> def test_lock(self):
>>>> print("lock acquired!")

#### `superclasses` (`RunServer.ShaeLib.concurrency.locked_resource.superclasses` | `RS.SL.concurrency.locked_resource.superclasses`)
[`_rsruntime/ShaeLib/concurrency/locked_resource.py`](/_rsruntime/ShaeLib/concurrency/locked_resource.py "Source")  
[Standalone doc: parts/RunServer/ShaeLib/concurrency/locked_resource/RunServer.ShaeLib.concurrency.locked_resource.superclasses.md](RunServer.ShaeLib.concurrency.locked_resource.superclasses)  
> superclasses(LockedResource, LR)

## `misc` (`RunServer.ShaeLib.misc` | `RS.SL.misc`)
[Standalone doc: parts/RunServer/ShaeLib/RunServer.ShaeLib.misc.md](RunServer.ShaeLib.misc)  

### `BetterPPrinter` (`RunServer.ShaeLib.misc.BetterPPrinter` | `RS.SL.misc.BetterPPrinter`)
[`_rsruntime/ShaeLib/misc/betterprettyprinter.py`](/_rsruntime/ShaeLib/misc/betterprettyprinter.py "Source")  
[Standalone doc: parts/RunServer/ShaeLib/misc/RunServer.ShaeLib.misc.BetterPPrinter.md](RunServer.ShaeLib.misc.BetterPPrinter)  

#### format(...)
```python
@staticmethod
def format(self, obj, _indent_: int = 0) -> Generator[str, None, None]
```

[`_rsruntime/ShaeLib/misc/betterprettyprinter.py@35:67`](/_rsruntime/ShaeLib/misc/betterprettyprinter.py#L35)
> <no doc>

#### formats(...)
```python
@staticmethod
def formats(self, obj, joiner: str = '') -> str
```

[`_rsruntime/ShaeLib/misc/betterprettyprinter.py@68:69`](/_rsruntime/ShaeLib/misc/betterprettyprinter.py#L68)

<details>
<summary>Source Code</summary>

```python
def formats(self, obj, joiner: str = '') -> str:
    return joiner.join(self.format(obj))
```
</details>

> <no doc>

#### writes(...)
```python
@staticmethod
def writes(...)
```
<details>
<summary>Parameters...</summary>

```python
    self, obj, fp=<_io.TextIOWrapper name='<stdout>' mode='w' encoding='utf-8'>,
    end: str = '\n', delay: float | None = None, collect: list | Callable(str) | None = None
```
</details>

[`_rsruntime/ShaeLib/misc/betterprettyprinter.py@70:78`](/_rsruntime/ShaeLib/misc/betterprettyprinter.py#L70)

<details>
<summary>Source Code</summary>

```python
def writes(self, obj, fp=sys.stdout, end: str = '\n', delay: float | None = None, collect: list | typing.Callable[[str], None] | None = None):
    for tok in self.format(obj):
        fp.write(tok)
        if delay: time.sleep(delay) # for aesthetic or testing purposes
        if collect is not None:
            if callable(collect): collect(tok)
            else: collect.append(fp)
    fp.write(end)
    return collect
```
</details>

> <no doc>

## `net` (`RunServer.ShaeLib.net` | `RS.SL.net`)
[Standalone doc: parts/RunServer/ShaeLib/RunServer.ShaeLib.net.md](RunServer.ShaeLib.net)  

### `fetch` (`RunServer.ShaeLib.net.fetch` | `RS.SL.net.fetch`)
[Standalone doc: parts/RunServer/ShaeLib/net/RunServer.ShaeLib.net.fetch.md](RunServer.ShaeLib.net.fetch)  

#### `CHUNK_FETCH_ABORT` (`RunServer.ShaeLib.net.fetch.CHUNK_FETCH_ABORT` | `RS.SL.net.fetch.CHUNK_FETCH_ABORT`)
[Standalone doc: parts/RunServer/ShaeLib/net/fetch/RunServer.ShaeLib.net.fetch.CHUNK_FETCH_ABORT.md](RunServer.ShaeLib.net.fetch.CHUNK_FETCH_ABORT)  
> The base class of the class hierarchy.  
>   
> When called, it accepts no arguments and returns a new featureless  
> instance that has no instance attributes and cannot be given any.

#### `chunk_fetch` (`RunServer.ShaeLib.net.fetch.chunk_fetch` | `RS.SL.net.fetch.chunk_fetch`)
[`_rsruntime/ShaeLib/net/fetch.py`](/_rsruntime/ShaeLib/net/fetch.py "Source")  
[Standalone doc: parts/RunServer/ShaeLib/net/fetch/RunServer.ShaeLib.net.fetch.chunk_fetch.md](RunServer.ShaeLib.net.fetch.chunk_fetch)  
> Fetch and yield bytes from the URL in chunks of chunksize  
> Yields a Chunk object  
> If the URL is cached, and ignore_cache is false, then yields the data (as Chunk, with from_cache=True) and returns it  
> Once all data has been read and yielded, it is returned as bytes, and added to the cache if add_to_cache is true
>> Cache is not written to if CHUNK_FETCH_ABORT is used to interrupt the download

#### `fetch` (`RunServer.ShaeLib.net.fetch.fetch` | `RS.SL.net.fetch.fetch`)
[`_rsruntime/ShaeLib/net/fetch.py`](/_rsruntime/ShaeLib/net/fetch.py "Source")  
[Standalone doc: parts/RunServer/ShaeLib/net/fetch/RunServer.ShaeLib.net.fetch.fetch.md](RunServer.ShaeLib.net.fetch.fetch)  
> Fetch bytes from the URL  
> If the URL is cached, and ignore_cache is false, then returns the cached value  
> Otherwise, fetch the data and return it, as well as add it to the cache if add_to_cache is true

#### `fetch_nocache` (`RunServer.ShaeLib.net.fetch.fetch_nocache` | `RS.SL.net.fetch.fetch_nocache`)
[Standalone doc: parts/RunServer/ShaeLib/net/fetch/RunServer.ShaeLib.net.fetch.fetch_nocache.md](RunServer.ShaeLib.net.fetch.fetch_nocache)  
> partial(func, *args, **keywords) - new function with partial application  
> of the given arguments and keywords.

##### fetch(...)
```python
@staticmethod
def fetch(...) -> bytes
```
<details>
<summary>Parameters...</summary>

```python
    url: str, add_to_cache: bool = True, ignore_cache: bool = False,
    add_headers
```
</details>

[`_rsruntime/ShaeLib/net/fetch.py@24:34`](/_rsruntime/ShaeLib/net/fetch.py#L24)

<details>
<summary>Source Code</summary>

```python
def fetch(url: str, *, add_to_cache: bool = True, ignore_cache: bool = False, **add_headers) -> bytes:
    '''
        Fetch bytes from the URL
        If the URL is cached, and ignore_cache is false, then returns the cached value
        Otherwise, fetch the data and return it, as well as add it to the cache if add_to_cache is true
    '''
    h = hash(url)
    if (not ignore_cache) and (h in cache): return cache[h]
    d = request('GET', url, headers={'User-Agent': user_agent} | add_headers).data
    if add_to_cache: cache[h] = d
    return d
```
</details>

> Fetch bytes from the URL  
> If the URL is cached, and ignore_cache is false, then returns the cached value  
> Otherwise, fetch the data and return it, as well as add it to the cache if add_to_cache is true

#### `flush_cache` (`RunServer.ShaeLib.net.fetch.flush_cache` | `RS.SL.net.fetch.flush_cache`)
[`_rsruntime/ShaeLib/net/fetch.py`](/_rsruntime/ShaeLib/net/fetch.py "Source")  
[Standalone doc: parts/RunServer/ShaeLib/net/fetch/RunServer.ShaeLib.net.fetch.flush_cache.md](RunServer.ShaeLib.net.fetch.flush_cache)  
> Removes a URL entry from the cache, or everything if url is None

#### `foreach_chunk_fetch` (`RunServer.ShaeLib.net.fetch.foreach_chunk_fetch` | `RS.SL.net.fetch.foreach_chunk_fetch`)
[`_rsruntime/ShaeLib/net/fetch.py`](/_rsruntime/ShaeLib/net/fetch.py "Source")  
[Standalone doc: parts/RunServer/ShaeLib/net/fetch/RunServer.ShaeLib.net.fetch.foreach_chunk_fetch.md](RunServer.ShaeLib.net.fetch.foreach_chunk_fetch)  
> Calls callback for each Chunk yielded by chunk_fetch, then returns the bytes

### `pattern` (`RunServer.ShaeLib.net.pattern` | `RS.SL.net.pattern`)
[Standalone doc: parts/RunServer/ShaeLib/net/RunServer.ShaeLib.net.pattern.md](RunServer.ShaeLib.net.pattern)  

## `timing` (`RunServer.ShaeLib.timing` | `RS.SL.timing`)
[Standalone doc: parts/RunServer/ShaeLib/RunServer.ShaeLib.timing.md](RunServer.ShaeLib.timing)  

### `PerfCounter` (`RunServer.ShaeLib.timing.PerfCounter` | `RS.SL.timing.PerfCounter`)
[`_rsruntime/ShaeLib/timing/perfcounter.py`](/_rsruntime/ShaeLib/timing/perfcounter.py "Source")  
[Standalone doc: parts/RunServer/ShaeLib/timing/RunServer.ShaeLib.timing.PerfCounter.md](RunServer.ShaeLib.timing.PerfCounter)  
> Provides an object-oriented (because why not) way to use (and format) time.perf_counter

#### fromhex(...)
```python
@classmethod
def fromhex(string)
```
> Create a floating-point number from a hexadecimal string.  
>   
> >>> float.fromhex('0x1.ffffp10')  
> 2047.984375  
> >>> float.fromhex('-0x1p-1074')  
> -5e-324

### `TimedLoadDebug` (`RunServer.ShaeLib.timing.TimedLoadDebug` | `RS.SL.timing.TimedLoadDebug`)
[`_rsruntime/ShaeLib/timing/timed_load_debug.py`](/_rsruntime/ShaeLib/timing/timed_load_debug.py "Source")  
[Standalone doc: parts/RunServer/ShaeLib/timing/RunServer.ShaeLib.timing.TimedLoadDebug.md](RunServer.ShaeLib.timing.TimedLoadDebug)  
> Helper class for debugging time spent doing things

#### final(...)
```python
@staticmethod
def final(self)
```

[`_rsruntime/ShaeLib/timing/timed_load_debug.py@27:29`](/_rsruntime/ShaeLib/timing/timed_load_debug.py#L27)

<details>
<summary>Source Code</summary>

```python
def final(self):
    self.logfn(self.msgfmt[0][1].format(opc=self.ocounter, ipc=self.icounter))
    self.ocounter = None # stop accidental multiple final() calls
```
</details>

> <no doc>

#### foreach(...)
```python
@classmethod
def foreach(logfunc: Callable(str), each: tuple[tuple[str, Callable], Ellipsis], tld_args)
```

[`_rsruntime/ShaeLib/timing/timed_load_debug.py@40:45`](/_rsruntime/ShaeLib/timing/timed_load_debug.py#L40)

<details>
<summary>Source Code</summary>

```python
@classmethod
def foreach(cls, logfunc: typing.Callable[[str], None], *each: tuple[tuple[str, typing.Callable[[], None]], ...], **tld_args):
    '''Executes each callable (second element of every "each" tuple) in each and times it with TimedLoadDebug, setting {c} as the first element of every "each" tuple'''
    tld = cls(logfunc, iterable=(n for n,c in each), **tld_args)
    for n,c in each:
        with tld: c()
```
</details>

> Executes each callable (second element of every "each" tuple) in each and times it with TimedLoadDebug, setting {c} as the first element of every "each" tuple

#### ienter(...)
```python
@staticmethod
def ienter(self)
```

[`_rsruntime/ShaeLib/timing/timed_load_debug.py@30:32`](/_rsruntime/ShaeLib/timing/timed_load_debug.py#L30)

<details>
<summary>Source Code</summary>

```python
def ienter(self):
    self.icounter = PerfCounter(sec='', secs='')
    self.logfn(self.msgfmt[1][0].format(c=next(self.cur[0]), opc=self.ocounter, ipc=self.icounter))
```
</details>

> <no doc>

#### iexit(...)
```python
@staticmethod
def iexit(...)
```
<details>
<summary>Parameters...</summary>

```python
    self, exc_type: type | None, exc_value: typing.Any | None,
    traceback: traceback
```
</details>

[`_rsruntime/ShaeLib/timing/timed_load_debug.py@34:37`](/_rsruntime/ShaeLib/timing/timed_load_debug.py#L34)

<details>
<summary>Source Code</summary>

```python
def iexit(self, exc_type: type | None, exc_value: typing.Any | None, traceback: TracebackType):
    r = self.msgfmt[2](exc_type, exc_value, traceback)
    if r is False: return
    self.logfn(self.msgfmt[1][1].format(c=next(self.cur[1]), opc=self.ocounter, ipc=self.icounter) if r is None else r)
```
</details>

> <no doc>

### `Timer` (`RunServer.ShaeLib.timing.Timer` | `RS.SL.timing.Timer`)
[`_rsruntime/ShaeLib/timing/timer.py`](/_rsruntime/ShaeLib/timing/timer.py "Source")  
[Standalone doc: parts/RunServer/ShaeLib/timing/RunServer.ShaeLib.timing.Timer.md](RunServer.ShaeLib.timing.Timer)  

#### clear(...)
```python
@staticmethod
def clear(timer: BaseTimer) -> BaseTimer
```

[`_rsruntime/ShaeLib/timing/timer.py@84:86`](/_rsruntime/ShaeLib/timing/timer.py#L84)

<details>
<summary>Source Code</summary>

```python
@staticmethod
def clear(timer: BaseTimer) -> BaseTimer:
    return timer.stop()
```
</details>

> <no doc>

#### set_timer(...)
```python
@staticmethod
def set_timer(...) -> Timer.BaseTimer
```
<details>
<summary>Parameters...</summary>

```python
    timer_type: type[Timer.BaseTimer], func: Callable, secs: float,
    activate_now: bool = True
```
</details>

[`_rsruntime/ShaeLib/timing/timer.py@80:83`](/_rsruntime/ShaeLib/timing/timer.py#L80)

<details>
<summary>Source Code</summary>

```python
@staticmethod
def set_timer(timer_type: type['Timer.BaseTimer'], func: typing.Callable, secs: float, activate_now: bool = True) -> 'Timer.BaseTimer':
    if activate_now: return timer_type(func, secs).start()
    return timer_type(func, secs)
```
</details>

> <no doc>

## `types` (`RunServer.ShaeLib.types` | `RS.SL.types`)
[Standalone doc: parts/RunServer/ShaeLib/RunServer.ShaeLib.types.md](RunServer.ShaeLib.types)  

### `Hooks` (`RunServer.ShaeLib.types.Hooks` | `RS.SL.types.Hooks`)
[`_rsruntime/ShaeLib/types/hooks.py`](/_rsruntime/ShaeLib/types/hooks.py "Source")  
[Standalone doc: parts/RunServer/ShaeLib/types/RunServer.ShaeLib.types.Hooks.md](RunServer.ShaeLib.types.Hooks)  
> The most caustic generic hooks class
>> Has no difference in behavior from GenericHooks other than typehinting
>>> basically syntactic sugar for dict[typing.Hashable, typing.Callable]
> Also serves as a container for the other types of hooks

#### register(...)
```python
@staticmethod
def register(self, hook: HookType, callback: FuncType)
```

[`_rsruntime/ShaeLib/types/hooks.py@22:25`](/_rsruntime/ShaeLib/types/hooks.py#L22)

<details>
<summary>Source Code</summary>

```python
def register(self, hook: HookType, callback: FuncType):
    '''Adds a callback to be called by __call__(hook)'''
    if hook not in self.hooks: self.hooks[hook] = set()
    self.hooks[hook].add(callback)
```
</details>

> Adds a callback to be called by __call__(hook)

#### unregister(...)
```python
@staticmethod
def unregister(self, hook: HookType, callback: FuncType)
```

[`_rsruntime/ShaeLib/types/hooks.py@26:29`](/_rsruntime/ShaeLib/types/hooks.py#L26)

<details>
<summary>Source Code</summary>

```python
def unregister(self, hook: HookType, callback: FuncType):
    '''Removes a callback that would be called by __call__(hook) (if it exists)'''
    if hook not in self.hooks: return
    self.hooks[hook].remove(callback)
```
</details>

> Removes a callback that would be called by __call__(hook) (if it exists)

#### unregister_hook(...)
```python
@staticmethod
def unregister_hook(self, hook: HookType)
```

[`_rsruntime/ShaeLib/types/hooks.py@30:33`](/_rsruntime/ShaeLib/types/hooks.py#L30)

<details>
<summary>Source Code</summary>

```python
def unregister_hook(self, hook: HookType):
    '''Deletes all callbacks that would be called by __call__(hook)'''
    if hook not in self.hooks: return
    del self.hooks[hook]
```
</details>

> Deletes all callbacks that would be called by __call__(hook)

### `fbd` (`RunServer.ShaeLib.types.fbd` | `RS.SL.types.fbd`)
[Standalone doc: parts/RunServer/ShaeLib/types/RunServer.ShaeLib.types.fbd.md](RunServer.ShaeLib.types.fbd)  

#### `INIBackedDict` (`RunServer.ShaeLib.types.fbd.INIBackedDict` | `RS.SL.types.fbd.INIBackedDict`)
[`_rsruntime/ShaeLib/types/fbd.py`](/_rsruntime/ShaeLib/types/fbd.py "Source")  
[Standalone doc: parts/RunServer/ShaeLib/types/fbd/RunServer.ShaeLib.types.fbd.INIBackedDict.md](RunServer.ShaeLib.types.fbd.INIBackedDict)  
> A FileBackedDict implementation that uses ConfigParser as a backend

##### bettergetter(...)
```python
@staticmethod
def bettergetter(...) -> Deserialized | Any
```
<details>
<summary>Parameters...</summary>

```python
    self, key: Key, default: ForwardRef('FileBackedDict.Behavior.RAISE') | Any = Behavior.RAISE,
    set_default: bool = True
```
</details>

[`_rsruntime/ShaeLib/types/fbd.py@137:153`](/_rsruntime/ShaeLib/types/fbd.py#L137)

<details>
<summary>Source Code</summary>

```python
def bettergetter(self, key: Key, default: typing.ForwardRef('FileBackedDict.Behavior.RAISE') | typing.Any = Behavior.RAISE, set_default: bool = True) -> Deserialized | typing.Any:
    '''
        Gets the value of key
            If the key is missing, then:
                if default is Behavior.RAISE: raises KeyError
                otherwise: returns default, and if set_default is truthy then sets the key to default
    '''
    key = self.key(key)
    _tree = self._gettree(key, make_if_missing=(default is not self.Behavior.RAISE) and set_default, fetch_if_missing=True, no_raise_keyerror=(default is not self.Behavior.RAISE))
    self._validate_transaction(key, self._transaction_types.PRE_GETITEM, _tree=_tree)
    if _tree is None: return default
    if key[-1] in _tree:
        val = _tree[key[-1]]
        self._validate_transaction(key, self._transaction_types.POST_GETITEM, (val,), _tree=_tree)
        return self._deserialize(val)
    if set_default: self.setitem(key, default, _tree=_tree)
    return default
```
</details>

> Gets the value of key  
> If the key is missing, then:
>> if default is Behavior.RAISE: raises KeyError  
>> otherwise: returns default, and if set_default is truthy then sets the key to default

##### contains(...)
```python
@staticmethod
def contains(self, key: Key, _tree: MutableMapping | None = None) -> bool
```

[`_rsruntime/ShaeLib/types/fbd.py@188:194`](/_rsruntime/ShaeLib/types/fbd.py#L188)

<details>
<summary>Source Code</summary>

```python
@locked
def contains(self, key: Key, *, _tree: MutableMapping | None = None) -> bool:
    '''Returns whether or not the key exists'''
    key = self.key(key) if (_tree is None) else key
    sect = self._gettree(key, make_if_missing=False, fetch_if_missing=True, no_raise_keyerror=True) if (_tree is None) else _tree
    if (sect is None) or (key[-1] not in sect): return False
    return True
```
</details>

> Returns whether or not the key exists

##### get(...)
```python
@staticmethod
def get(...) -> Deserialized
```
<details>
<summary>Parameters...</summary>

```python
    self, key: Key, default: ForwardRef('FileBackedDict.Behavior.RAISE') | Serializable = Behavior.RAISE,
    _tree: MutableMapping | None = None
```
</details>

[`_rsruntime/ShaeLib/types/fbd.py@160:175`](/_rsruntime/ShaeLib/types/fbd.py#L160)

<details>
<summary>Source Code</summary>

```python
@locked
def get(self, key: Key, default: typing.ForwardRef('FileBackedDict.Behavior.RAISE') | Serializable = Behavior.RAISE, *, _tree: MutableMapping | None = None) -> Deserialized:
    '''
        Gets the value of key
            If the key is missing, then raises KeyError if default is Behavior.RAISE, otherwise returns default
    '''
    key = self.key(key) if (_tree is None) else key
    sect = _tree if (_tree is not None) else \
           self._gettree(key, make_if_missing=False, fetch_if_missing=True, no_raise_keyerror=(default is not self.Behavior.RAISE))
    self._validate_transaction(key, self._transaction_types.PRE_GETITEM, _tree=sect)
    if sect is None: return default
    if key[-1] not in sect:
        raise KeyError(f'{key}[-1]')
    val = sect[key[-1]]
    self._validate_transaction(key, self._transaction_types.POST_GETITEM, (val,), _tree=_tree)
    return self._deserialize(sect[key[-1]])
```
</details>

> Gets the value of key  
> If the key is missing, then raises KeyError if default is Behavior.RAISE, otherwise returns default

##### get(...)
```python
@staticmethod
def get(...) -> Deserialized
```
<details>
<summary>Parameters...</summary>

```python
    self, key: Key, default: ForwardRef('FileBackedDict.Behavior.RAISE') | Serializable = Behavior.RAISE,
    _tree: MutableMapping | None = None
```
</details>

[`_rsruntime/ShaeLib/types/fbd.py@160:175`](/_rsruntime/ShaeLib/types/fbd.py#L160)

<details>
<summary>Source Code</summary>

```python
@locked
def get(self, key: Key, default: typing.ForwardRef('FileBackedDict.Behavior.RAISE') | Serializable = Behavior.RAISE, *, _tree: MutableMapping | None = None) -> Deserialized:
    '''
        Gets the value of key
            If the key is missing, then raises KeyError if default is Behavior.RAISE, otherwise returns default
    '''
    key = self.key(key) if (_tree is None) else key
    sect = _tree if (_tree is not None) else \
           self._gettree(key, make_if_missing=False, fetch_if_missing=True, no_raise_keyerror=(default is not self.Behavior.RAISE))
    self._validate_transaction(key, self._transaction_types.PRE_GETITEM, _tree=sect)
    if sect is None: return default
    if key[-1] not in sect:
        raise KeyError(f'{key}[-1]')
    val = sect[key[-1]]
    self._validate_transaction(key, self._transaction_types.POST_GETITEM, (val,), _tree=_tree)
    return self._deserialize(sect[key[-1]])
```
</details>

> Gets the value of key  
> If the key is missing, then raises KeyError if default is Behavior.RAISE, otherwise returns default

##### is_autosyncing(...)
```python
@staticmethod
def is_autosyncing(self) -> bool
```

[`_rsruntime/ShaeLib/types/fbd.py@97:100`](/_rsruntime/ShaeLib/types/fbd.py#L97)

<details>
<summary>Source Code</summary>

```python
@locked
def is_autosyncing(self) -> bool:
    '''Returns whether or not the internal watchdog timer is ticking'''
    return self.watchdog.is_alive()
```
</details>

> Returns whether or not the internal watchdog timer is ticking

##### items_full(...)
```python
@staticmethod
def items_full(self, start_key: Key, key_join: bool = True) -> Generator[tuple[str | tuple[str, ...], Deserialized], None, None]
```

[`_rsruntime/ShaeLib/types/fbd.py@197:200`](/_rsruntime/ShaeLib/types/fbd.py#L197)

<details>
<summary>Source Code</summary>

```python
@locked
def items_full(self, start_key: Key, key_join: bool = True) -> typing.Generator[tuple[str | tuple[str, ...], Deserialized], None, None]:
    '''Iterates over every (key, value) pair, yielding the entire key'''
    yield from ((k, self[k]) for k in self.keys(start_key, key_join))
```
</details>

> Iterates over every (key, value) pair, yielding the entire key

##### items_short(...)
```python
@staticmethod
def items_short(self, start_key: Key)
```

[`_rsruntime/ShaeLib/types/fbd.py@201:204`](/_rsruntime/ShaeLib/types/fbd.py#L201)

<details>
<summary>Source Code</summary>

```python
@locked
def items_short(self, start_key: Key):
    '''Iterates over every (key, value) pair, yielding the last part of the key'''
    yield from ((k[-1], self[k]) for k in self.keys(start_key, False))
```
</details>

> Iterates over every (key, value) pair, yielding the last part of the key

##### key(...)
```python
@classmethod
def key(key: Key, top_level: bool = False) -> tuple[str, Ellipsis]
```

[`_rsruntime/ShaeLib/types/fbd.py@65:78`](/_rsruntime/ShaeLib/types/fbd.py#L65)

<details>
<summary>Source Code</summary>

```python
@classmethod
def key(cls, key: Key, *, top_level: bool = False) -> tuple[str, ...]: # key key key
    '''Transform a string / tuple of strings into a key'''
    if isinstance(key, str): key = key.split(cls.key_sep)
    if not key: raise ValueError('Empty key')
    if any(cls.key_sep in part for part in key):
        raise ValueError(f'Part of {key} contains a "{cls.key_sep}"')
    elif (not top_level) and (len(key) == 1):
        raise ValueError('Top-level key disallowed')
    elif not cls.key_topp_patt.fullmatch(key[0]):
        raise ValueError(f'Top-level part of key {key} does not match pattern {cls.key_topp_patt.pattern}')
    elif not all(map(cls.key_part_patt.fullmatch, key[1:])):
        raise ValueError(f'Part of sub-key {key[1:]} (full: {key}) not match pattern {cls.key_part_patt.pattern}')
    return tuple(key)
```
</details>

> Transform a string / tuple of strings into a key

##### keys(...)
```python
@staticmethod
def keys(self, start_key: Key | None = None, key_join: bool = True) -> Generator[str | tuple[str, ...], None, None]
```

[`_rsruntime/ShaeLib/types/fbd.py@205:214`](/_rsruntime/ShaeLib/types/fbd.py#L205)

<details>
<summary>Source Code</summary>

```python
@locked
def keys(self, start_key: Key | None = None, key_join: bool = True) -> typing.Generator[str | tuple[str, ...], None, None]:
    '''Iterates over every key'''
    if start_key is None:
        skey = ()
        target = self._data
    else:
        skey = self.key(start_key, top_level=True)
        target = self._gettree(skey+(None,), make_if_missing=False)
    yield from map((lambda k: self.key_sep.join(skey+(k,))) if key_join else (lambda k: skey+(k,)), target.keys())
```
</details>

> Iterates over every key

##### path_from_topkey(...)
```python
@staticmethod
def path_from_topkey(self, topkey: str)
```

[`_rsruntime/ShaeLib/types/fbd.py@79:81`](/_rsruntime/ShaeLib/types/fbd.py#L79)

<details>
<summary>Source Code</summary>

```python
def path_from_topkey(self, topkey: str):
    '''Returns the Path corresponding to the top-key's file'''
    return (self.path / topkey).with_suffix(self.file_suffix)
```
</details>

> Returns the Path corresponding to the top-key's file

##### readin(...)
```python
@staticmethod
def readin(self, topkey: str)
```

[`_rsruntime/ShaeLib/types/fbd.py@127:132`](/_rsruntime/ShaeLib/types/fbd.py#L127)

<details>
<summary>Source Code</summary>

```python
@locked
def readin(self, topkey: str):
    '''Reads in a top-level key'''
    path = self.path_from_topkey(topkey)
    self._from_string(topkey, path.read_text())
    self.mtimes[topkey] = path.stat().st_mtime
```
</details>

> Reads in a top-level key

##### readin_modified(...)
```python
@staticmethod
def readin_modified(self)
```

[`_rsruntime/ShaeLib/types/fbd.py@116:126`](/_rsruntime/ShaeLib/types/fbd.py#L116)

<details>
<summary>Source Code</summary>

```python
@locked
def readin_modified(self):
    '''Reads in top-level keys that have been changed'''
    for tk,tm in self.mtimes.items():
        p = self.path_from_topkey(tk)
        if not p.exists():
            del self.mtimes[tk]
        nt = p.stat().st_mtime
        if nt == tm: continue
        self.mtimes[tk] = tm
        self._from_string(tk, p.read_text())
```
</details>

> Reads in top-level keys that have been changed

##### setitem(...)
```python
@staticmethod
def setitem(...)
```
<details>
<summary>Parameters...</summary>

```python
    self, key: Key, val: Serializable,
    _tree: MutableMapping | None = None
```
</details>

[`_rsruntime/ShaeLib/types/fbd.py@178:185`](/_rsruntime/ShaeLib/types/fbd.py#L178)

<details>
<summary>Source Code</summary>

```python
@locked
def setitem(self, key: Key, val: Serializable, *, _tree: MutableMapping | None = None):
    '''Sets a key to a value'''
    key = self.key(key) if (_tree is None) else key
    sect = (self._gettree(key, make_if_missing=True, fetch_if_missing=True) if (_tree is None) else _tree)
    self._validate_transaction(key, self._transaction_types.SETITEM, (val,), _tree=sect)
    sect[key[-1]] = self._serialize(val)
    self.dirty.add(key[0])
```
</details>

> Sets a key to a value

##### sort(...)
```python
@staticmethod
def sort(self, by: Callable(str | tuple[str, ...]) -> Any = <lambda>)
```

[`_rsruntime/ShaeLib/types/fbd.py@277:283`](/_rsruntime/ShaeLib/types/fbd.py#L277)

<details>
<summary>Source Code</summary>

```python
def sort(self, by: typing.Callable[[str | tuple[str, ...]], typing.Any] = lambda k: k):
    '''Sorts the data of this INIBackedDict in-place, marking all touched sections as dirty'''
    for top,cfg in self._data.items():
        self.dirty.add(top)
        cfg._sections = dict(sorted(cfg._sections.items(), key=lambda it: by((it[0],))))
        for sname,ssect in cfg._sections.items():
            cfg._sections[sname] = dict(sorted(ssect.items(), key=lambda it: by(tuple(it[0].split('.')))))
```
</details>

> Sorts the data of this INIBackedDict in-place, marking all touched sections as dirty

##### start_autosync(...)
```python
@staticmethod
def start_autosync(self)
```

[`_rsruntime/ShaeLib/types/fbd.py@89:92`](/_rsruntime/ShaeLib/types/fbd.py#L89)

<details>
<summary>Source Code</summary>

```python
@locked
def start_autosync(self):
    '''Starts the internal watchdog timer'''
    self.watchdog.start()
```
</details>

> Starts the internal watchdog timer

##### stop_autosync(...)
```python
@staticmethod
def stop_autosync(self)
```

[`_rsruntime/ShaeLib/types/fbd.py@93:96`](/_rsruntime/ShaeLib/types/fbd.py#L93)

<details>
<summary>Source Code</summary>

```python
@locked
def stop_autosync(self):
    '''Stops the internal watchdog timer'''
    self.watchdog.stop()
```
</details>

> Stops the internal watchdog timer

##### sync(...)
```python
@staticmethod
def sync(self)
```

[`_rsruntime/ShaeLib/types/fbd.py@83:87`](/_rsruntime/ShaeLib/types/fbd.py#L83)

<details>
<summary>Source Code</summary>

```python
@locked
def sync(self):
    '''Convenience method for writeback_dirty and readin_modified'''
    self.writeback_dirty()
    self.readin_modified()
```
</details>

> Convenience method for writeback_dirty and readin_modified

##### values(...)
```python
@staticmethod
def values(self, start_key: Key) -> Generator[[Deserialized], None, None]
```

[`_rsruntime/ShaeLib/types/fbd.py@216:219`](/_rsruntime/ShaeLib/types/fbd.py#L216)

<details>
<summary>Source Code</summary>

```python
@locked
def values(self, start_key: Key) -> typing.Generator[[Deserialized], None, None]:
    '''Iterates over every value'''
    yield from map(self.getitem, self.keys(start_key))
```
</details>

> Iterates over every value

##### writeback(...)
```python
@staticmethod
def writeback(...)
```
<details>
<summary>Parameters...</summary>

```python
    self, topkey: str, only_if_dirty: bool = True,
    clean: bool = True
```
</details>

[`_rsruntime/ShaeLib/types/fbd.py@106:112`](/_rsruntime/ShaeLib/types/fbd.py#L106)

<details>
<summary>Source Code</summary>

```python
@locked
def writeback(self, topkey: str, *, only_if_dirty: bool = True, clean: bool = True):
    '''Writes back a top-level key'''
    if topkey in self.dirty:
        if clean: self.dirty.remove(topkey)
    elif only_if_dirty: return
    self.path_from_topkey(topkey).write_text(self._to_string(topkey))
```
</details>

> Writes back a top-level key

##### writeback_dirty(...)
```python
@staticmethod
def writeback_dirty(self)
```

[`_rsruntime/ShaeLib/types/fbd.py@102:105`](/_rsruntime/ShaeLib/types/fbd.py#L102)

<details>
<summary>Source Code</summary>

```python
@locked
def writeback_dirty(self):
    while self.dirty:
        self.writeback(self.dirty.pop(), only_if_dirty=False, clean=False)
```
</details>

> <no doc>

#### `JSONBackedDict` (`RunServer.ShaeLib.types.fbd.JSONBackedDict` | `RS.SL.types.fbd.JSONBackedDict`)
[`_rsruntime/ShaeLib/types/fbd.py`](/_rsruntime/ShaeLib/types/fbd.py "Source")  
[Standalone doc: parts/RunServer/ShaeLib/types/fbd/RunServer.ShaeLib.types.fbd.JSONBackedDict.md](RunServer.ShaeLib.types.fbd.JSONBackedDict)  
> A FileBackedDict implementation that uses JSON as a backend

##### bettergetter(...)
```python
@staticmethod
def bettergetter(...) -> Deserialized | Any
```
<details>
<summary>Parameters...</summary>

```python
    self, key: Key, default: ForwardRef('FileBackedDict.Behavior.RAISE') | Any = Behavior.RAISE,
    set_default: bool = True
```
</details>

[`_rsruntime/ShaeLib/types/fbd.py@137:153`](/_rsruntime/ShaeLib/types/fbd.py#L137)

<details>
<summary>Source Code</summary>

```python
def bettergetter(self, key: Key, default: typing.ForwardRef('FileBackedDict.Behavior.RAISE') | typing.Any = Behavior.RAISE, set_default: bool = True) -> Deserialized | typing.Any:
    '''
        Gets the value of key
            If the key is missing, then:
                if default is Behavior.RAISE: raises KeyError
                otherwise: returns default, and if set_default is truthy then sets the key to default
    '''
    key = self.key(key)
    _tree = self._gettree(key, make_if_missing=(default is not self.Behavior.RAISE) and set_default, fetch_if_missing=True, no_raise_keyerror=(default is not self.Behavior.RAISE))
    self._validate_transaction(key, self._transaction_types.PRE_GETITEM, _tree=_tree)
    if _tree is None: return default
    if key[-1] in _tree:
        val = _tree[key[-1]]
        self._validate_transaction(key, self._transaction_types.POST_GETITEM, (val,), _tree=_tree)
        return self._deserialize(val)
    if set_default: self.setitem(key, default, _tree=_tree)
    return default
```
</details>

> Gets the value of key  
> If the key is missing, then:
>> if default is Behavior.RAISE: raises KeyError  
>> otherwise: returns default, and if set_default is truthy then sets the key to default

##### contains(...)
```python
@staticmethod
def contains(self, key: Key, _tree: MutableMapping | None = None) -> bool
```

[`_rsruntime/ShaeLib/types/fbd.py@188:194`](/_rsruntime/ShaeLib/types/fbd.py#L188)

<details>
<summary>Source Code</summary>

```python
@locked
def contains(self, key: Key, *, _tree: MutableMapping | None = None) -> bool:
    '''Returns whether or not the key exists'''
    key = self.key(key) if (_tree is None) else key
    sect = self._gettree(key, make_if_missing=False, fetch_if_missing=True, no_raise_keyerror=True) if (_tree is None) else _tree
    if (sect is None) or (key[-1] not in sect): return False
    return True
```
</details>

> Returns whether or not the key exists

##### get(...)
```python
@staticmethod
def get(...) -> Deserialized
```
<details>
<summary>Parameters...</summary>

```python
    self, key: Key, default: ForwardRef('FileBackedDict.Behavior.RAISE') | Serializable = Behavior.RAISE,
    _tree: MutableMapping | None = None
```
</details>

[`_rsruntime/ShaeLib/types/fbd.py@160:175`](/_rsruntime/ShaeLib/types/fbd.py#L160)

<details>
<summary>Source Code</summary>

```python
@locked
def get(self, key: Key, default: typing.ForwardRef('FileBackedDict.Behavior.RAISE') | Serializable = Behavior.RAISE, *, _tree: MutableMapping | None = None) -> Deserialized:
    '''
        Gets the value of key
            If the key is missing, then raises KeyError if default is Behavior.RAISE, otherwise returns default
    '''
    key = self.key(key) if (_tree is None) else key
    sect = _tree if (_tree is not None) else \
           self._gettree(key, make_if_missing=False, fetch_if_missing=True, no_raise_keyerror=(default is not self.Behavior.RAISE))
    self._validate_transaction(key, self._transaction_types.PRE_GETITEM, _tree=sect)
    if sect is None: return default
    if key[-1] not in sect:
        raise KeyError(f'{key}[-1]')
    val = sect[key[-1]]
    self._validate_transaction(key, self._transaction_types.POST_GETITEM, (val,), _tree=_tree)
    return self._deserialize(sect[key[-1]])
```
</details>

> Gets the value of key  
> If the key is missing, then raises KeyError if default is Behavior.RAISE, otherwise returns default

##### get(...)
```python
@staticmethod
def get(...) -> Deserialized
```
<details>
<summary>Parameters...</summary>

```python
    self, key: Key, default: ForwardRef('FileBackedDict.Behavior.RAISE') | Serializable = Behavior.RAISE,
    _tree: MutableMapping | None = None
```
</details>

[`_rsruntime/ShaeLib/types/fbd.py@160:175`](/_rsruntime/ShaeLib/types/fbd.py#L160)

<details>
<summary>Source Code</summary>

```python
@locked
def get(self, key: Key, default: typing.ForwardRef('FileBackedDict.Behavior.RAISE') | Serializable = Behavior.RAISE, *, _tree: MutableMapping | None = None) -> Deserialized:
    '''
        Gets the value of key
            If the key is missing, then raises KeyError if default is Behavior.RAISE, otherwise returns default
    '''
    key = self.key(key) if (_tree is None) else key
    sect = _tree if (_tree is not None) else \
           self._gettree(key, make_if_missing=False, fetch_if_missing=True, no_raise_keyerror=(default is not self.Behavior.RAISE))
    self._validate_transaction(key, self._transaction_types.PRE_GETITEM, _tree=sect)
    if sect is None: return default
    if key[-1] not in sect:
        raise KeyError(f'{key}[-1]')
    val = sect[key[-1]]
    self._validate_transaction(key, self._transaction_types.POST_GETITEM, (val,), _tree=_tree)
    return self._deserialize(sect[key[-1]])
```
</details>

> Gets the value of key  
> If the key is missing, then raises KeyError if default is Behavior.RAISE, otherwise returns default

##### is_autosyncing(...)
```python
@staticmethod
def is_autosyncing(self) -> bool
```

[`_rsruntime/ShaeLib/types/fbd.py@97:100`](/_rsruntime/ShaeLib/types/fbd.py#L97)

<details>
<summary>Source Code</summary>

```python
@locked
def is_autosyncing(self) -> bool:
    '''Returns whether or not the internal watchdog timer is ticking'''
    return self.watchdog.is_alive()
```
</details>

> Returns whether or not the internal watchdog timer is ticking

##### items_full(...)
```python
@staticmethod
def items_full(self, start_key: Key, key_join: bool = True) -> Generator[tuple[str | tuple[str, ...], Deserialized], None, None]
```

[`_rsruntime/ShaeLib/types/fbd.py@197:200`](/_rsruntime/ShaeLib/types/fbd.py#L197)

<details>
<summary>Source Code</summary>

```python
@locked
def items_full(self, start_key: Key, key_join: bool = True) -> typing.Generator[tuple[str | tuple[str, ...], Deserialized], None, None]:
    '''Iterates over every (key, value) pair, yielding the entire key'''
    yield from ((k, self[k]) for k in self.keys(start_key, key_join))
```
</details>

> Iterates over every (key, value) pair, yielding the entire key

##### items_short(...)
```python
@staticmethod
def items_short(self, start_key: Key)
```

[`_rsruntime/ShaeLib/types/fbd.py@201:204`](/_rsruntime/ShaeLib/types/fbd.py#L201)

<details>
<summary>Source Code</summary>

```python
@locked
def items_short(self, start_key: Key):
    '''Iterates over every (key, value) pair, yielding the last part of the key'''
    yield from ((k[-1], self[k]) for k in self.keys(start_key, False))
```
</details>

> Iterates over every (key, value) pair, yielding the last part of the key

##### key(...)
```python
@classmethod
def key(key: Key, top_level: bool = False) -> tuple[str, Ellipsis]
```

[`_rsruntime/ShaeLib/types/fbd.py@65:78`](/_rsruntime/ShaeLib/types/fbd.py#L65)

<details>
<summary>Source Code</summary>

```python
@classmethod
def key(cls, key: Key, *, top_level: bool = False) -> tuple[str, ...]: # key key key
    '''Transform a string / tuple of strings into a key'''
    if isinstance(key, str): key = key.split(cls.key_sep)
    if not key: raise ValueError('Empty key')
    if any(cls.key_sep in part for part in key):
        raise ValueError(f'Part of {key} contains a "{cls.key_sep}"')
    elif (not top_level) and (len(key) == 1):
        raise ValueError('Top-level key disallowed')
    elif not cls.key_topp_patt.fullmatch(key[0]):
        raise ValueError(f'Top-level part of key {key} does not match pattern {cls.key_topp_patt.pattern}')
    elif not all(map(cls.key_part_patt.fullmatch, key[1:])):
        raise ValueError(f'Part of sub-key {key[1:]} (full: {key}) not match pattern {cls.key_part_patt.pattern}')
    return tuple(key)
```
</details>

> Transform a string / tuple of strings into a key

##### keys(...)
```python
@staticmethod
def keys(self, start_key: Key | None = None, key_join: bool = True) -> Generator[str | tuple[str, ...], None, None]
```

[`_rsruntime/ShaeLib/types/fbd.py@205:214`](/_rsruntime/ShaeLib/types/fbd.py#L205)

<details>
<summary>Source Code</summary>

```python
@locked
def keys(self, start_key: Key | None = None, key_join: bool = True) -> typing.Generator[str | tuple[str, ...], None, None]:
    '''Iterates over every key'''
    if start_key is None:
        skey = ()
        target = self._data
    else:
        skey = self.key(start_key, top_level=True)
        target = self._gettree(skey+(None,), make_if_missing=False)
    yield from map((lambda k: self.key_sep.join(skey+(k,))) if key_join else (lambda k: skey+(k,)), target.keys())
```
</details>

> Iterates over every key

##### path_from_topkey(...)
```python
@staticmethod
def path_from_topkey(self, topkey: str)
```

[`_rsruntime/ShaeLib/types/fbd.py@79:81`](/_rsruntime/ShaeLib/types/fbd.py#L79)

<details>
<summary>Source Code</summary>

```python
def path_from_topkey(self, topkey: str):
    '''Returns the Path corresponding to the top-key's file'''
    return (self.path / topkey).with_suffix(self.file_suffix)
```
</details>

> Returns the Path corresponding to the top-key's file

##### readin(...)
```python
@staticmethod
def readin(self, topkey: str)
```

[`_rsruntime/ShaeLib/types/fbd.py@127:132`](/_rsruntime/ShaeLib/types/fbd.py#L127)

<details>
<summary>Source Code</summary>

```python
@locked
def readin(self, topkey: str):
    '''Reads in a top-level key'''
    path = self.path_from_topkey(topkey)
    self._from_string(topkey, path.read_text())
    self.mtimes[topkey] = path.stat().st_mtime
```
</details>

> Reads in a top-level key

##### readin_modified(...)
```python
@staticmethod
def readin_modified(self)
```

[`_rsruntime/ShaeLib/types/fbd.py@116:126`](/_rsruntime/ShaeLib/types/fbd.py#L116)

<details>
<summary>Source Code</summary>

```python
@locked
def readin_modified(self):
    '''Reads in top-level keys that have been changed'''
    for tk,tm in self.mtimes.items():
        p = self.path_from_topkey(tk)
        if not p.exists():
            del self.mtimes[tk]
        nt = p.stat().st_mtime
        if nt == tm: continue
        self.mtimes[tk] = tm
        self._from_string(tk, p.read_text())
```
</details>

> Reads in top-level keys that have been changed

##### setitem(...)
```python
@staticmethod
def setitem(...)
```
<details>
<summary>Parameters...</summary>

```python
    self, key: Key, val: Serializable,
    _tree: MutableMapping | None = None
```
</details>

[`_rsruntime/ShaeLib/types/fbd.py@178:185`](/_rsruntime/ShaeLib/types/fbd.py#L178)

<details>
<summary>Source Code</summary>

```python
@locked
def setitem(self, key: Key, val: Serializable, *, _tree: MutableMapping | None = None):
    '''Sets a key to a value'''
    key = self.key(key) if (_tree is None) else key
    sect = (self._gettree(key, make_if_missing=True, fetch_if_missing=True) if (_tree is None) else _tree)
    self._validate_transaction(key, self._transaction_types.SETITEM, (val,), _tree=sect)
    sect[key[-1]] = self._serialize(val)
    self.dirty.add(key[0])
```
</details>

> Sets a key to a value

##### sort(...)
```python
@staticmethod
def sort(self, by: Callable(tuple[str, Ellipsis]) -> Any = <lambda>)
```

[`_rsruntime/ShaeLib/types/fbd.py@374:378`](/_rsruntime/ShaeLib/types/fbd.py#L374)

<details>
<summary>Source Code</summary>

```python
def sort(self, by: typing.Callable[[tuple[str, ...]], typing.Any] = lambda k: k):
    '''Sorts the data of this JSONBackedDict (semi-)in-place, marking all touched sections as dirty'''
    for top,dat in self._data.items():
        self.dirty.add(top)
        self._data[top] = self._sorteddict(dat, (top,), by)
```
</details>

> Sorts the data of this JSONBackedDict (semi-)in-place, marking all touched sections as dirty

##### start_autosync(...)
```python
@staticmethod
def start_autosync(self)
```

[`_rsruntime/ShaeLib/types/fbd.py@89:92`](/_rsruntime/ShaeLib/types/fbd.py#L89)

<details>
<summary>Source Code</summary>

```python
@locked
def start_autosync(self):
    '''Starts the internal watchdog timer'''
    self.watchdog.start()
```
</details>

> Starts the internal watchdog timer

##### stop_autosync(...)
```python
@staticmethod
def stop_autosync(self)
```

[`_rsruntime/ShaeLib/types/fbd.py@93:96`](/_rsruntime/ShaeLib/types/fbd.py#L93)

<details>
<summary>Source Code</summary>

```python
@locked
def stop_autosync(self):
    '''Stops the internal watchdog timer'''
    self.watchdog.stop()
```
</details>

> Stops the internal watchdog timer

##### sync(...)
```python
@staticmethod
def sync(self)
```

[`_rsruntime/ShaeLib/types/fbd.py@83:87`](/_rsruntime/ShaeLib/types/fbd.py#L83)

<details>
<summary>Source Code</summary>

```python
@locked
def sync(self):
    '''Convenience method for writeback_dirty and readin_modified'''
    self.writeback_dirty()
    self.readin_modified()
```
</details>

> Convenience method for writeback_dirty and readin_modified

##### values(...)
```python
@staticmethod
def values(self, start_key: Key) -> Generator[[Deserialized], None, None]
```

[`_rsruntime/ShaeLib/types/fbd.py@216:219`](/_rsruntime/ShaeLib/types/fbd.py#L216)

<details>
<summary>Source Code</summary>

```python
@locked
def values(self, start_key: Key) -> typing.Generator[[Deserialized], None, None]:
    '''Iterates over every value'''
    yield from map(self.getitem, self.keys(start_key))
```
</details>

> Iterates over every value

##### writeback(...)
```python
@staticmethod
def writeback(...)
```
<details>
<summary>Parameters...</summary>

```python
    self, topkey: str, only_if_dirty: bool = True,
    clean: bool = True
```
</details>

[`_rsruntime/ShaeLib/types/fbd.py@106:112`](/_rsruntime/ShaeLib/types/fbd.py#L106)

<details>
<summary>Source Code</summary>

```python
@locked
def writeback(self, topkey: str, *, only_if_dirty: bool = True, clean: bool = True):
    '''Writes back a top-level key'''
    if topkey in self.dirty:
        if clean: self.dirty.remove(topkey)
    elif only_if_dirty: return
    self.path_from_topkey(topkey).write_text(self._to_string(topkey))
```
</details>

> Writes back a top-level key

##### writeback_dirty(...)
```python
@staticmethod
def writeback_dirty(self)
```

[`_rsruntime/ShaeLib/types/fbd.py@102:105`](/_rsruntime/ShaeLib/types/fbd.py#L102)

<details>
<summary>Source Code</summary>

```python
@locked
def writeback_dirty(self):
    while self.dirty:
        self.writeback(self.dirty.pop(), only_if_dirty=False, clean=False)
```
</details>

> <no doc>

### `shaespace` (`RunServer.ShaeLib.types.shaespace` | `RS.SL.types.shaespace`)
[Standalone doc: parts/RunServer/ShaeLib/types/RunServer.ShaeLib.types.shaespace.md](RunServer.ShaeLib.types.shaespace)  

#### `ShaeSpace` (`RunServer.ShaeLib.types.shaespace.ShaeSpace` | `RS.SL.types.shaespace.ShaeSpace`)
[`_rsruntime/ShaeLib/types/shaespace.py`](/_rsruntime/ShaeLib/types/shaespace.py "Source")  
[Standalone doc: parts/RunServer/ShaeLib/types/shaespace/RunServer.ShaeLib.types.shaespace.ShaeSpace.md](RunServer.ShaeLib.types.shaespace.ShaeSpace)  
> A decorator to turn a class('s __dict__) into a SimpleNamespace

#### `SlottedSpace` (`RunServer.ShaeLib.types.shaespace.SlottedSpace` | `RS.SL.types.shaespace.SlottedSpace`)
[`_rsruntime/ShaeLib/types/shaespace.py`](/_rsruntime/ShaeLib/types/shaespace.py "Source")  
[Standalone doc: parts/RunServer/ShaeLib/types/shaespace/RunServer.ShaeLib.types.shaespace.SlottedSpace.md](RunServer.ShaeLib.types.shaespace.SlottedSpace)  
> Creates and instantiates a namespace with a preset __slots__ attribute

#### `SlottedSpaceType` (`RunServer.ShaeLib.types.shaespace.SlottedSpaceType` | `RS.SL.types.shaespace.SlottedSpaceType`)
[`_rsruntime/ShaeLib/types/shaespace.py`](/_rsruntime/ShaeLib/types/shaespace.py "Source")  
[Standalone doc: parts/RunServer/ShaeLib/types/shaespace/RunServer.ShaeLib.types.shaespace.SlottedSpaceType.md](RunServer.ShaeLib.types.shaespace.SlottedSpaceType)  
> Creates a namespace with a preset __slots__ attribute

#### `slotted_ShaeSpace` (`RunServer.ShaeLib.types.shaespace.slotted_ShaeSpace` | `RS.SL.types.shaespace.slotted_ShaeSpace`)
[`_rsruntime/ShaeLib/types/shaespace.py`](/_rsruntime/ShaeLib/types/shaespace.py "Source")  
[Standalone doc: parts/RunServer/ShaeLib/types/shaespace/RunServer.ShaeLib.types.shaespace.slotted_ShaeSpace.md](RunServer.ShaeLib.types.shaespace.slotted_ShaeSpace)  
> A decorator to turn a class('s __dict__) into a SlottedSpace