# `LineParser` (`RunServer.LineParser` | `RS.LP`)
[`_rsruntime/lib/rs_lineparser.py`](/_rsruntime/lib/rs_lineparser.py "Source")  
[Standalone doc: parts/RunServer/RunServer.LineParser.md](RunServer.LineParser)  

## handle_line(...)
```python
def handle_line(line: str)
```

[`_rsruntime/lib/rs_lineparser.py@123:128`](/_rsruntime/lib/rs_lineparser.py#L123)

<details>
<summary>Source Code</summary>

```python
def handle_line(self, line: str):
    pfx, lin = RS.MCLang.strip_prefix(line)
    if pfx is None: return self.hooks_no_prefix(lin) # returns nothing!
    self.hooks_prefix(lin, *pfx)
    if (m := self.chat_patt.fullmatch(lin)) is not None:
        self.hooks_chat(RS.UserManager[m.group('username')], m.group('message'), bool(m.group('not_secure')))
```
</details>

> <no doc>

## init2()
```python
def init2()
```

[`_rsruntime/lib/rs_lineparser.py@108:109`](/_rsruntime/lib/rs_lineparser.py#L108)

<details>
<summary>Source Code</summary>

```python
def init2(self):
    self.chat_patt = RS.MCLang.lang_to_pattern(RS.MCLang.lang['chat.type.text'], ('username', 'message'), prefix_suffix=r'^(?P<not_secure>(?:\[Not Secure\] )?){}$')
```
</details>

> <no doc>

## register_callback(...)
```python
def register_callback(patt: Pattern, callback: Callable(Match, Match, struct_time) | Callable(Match), with_prefix: bool = True)
```

[`_rsruntime/lib/rs_lineparser.py@110:116`](/_rsruntime/lib/rs_lineparser.py#L110)

<details>
<summary>Source Code</summary>

```python
def register_callback(self, patt: re.Pattern, callback: typing.Callable[[re.Match, re.Match, time.struct_time], None] | typing.Callable[[re.Match], None], with_prefix: bool = True):
    '''
        Registers a callback
            If keep_prefix, then lines that have the prefix are passed. callback should have the signature: `callback(line: re.Match, prefix: re.Match, time: time.struct_time)`
            Otherwise, lines that don't have a prefix are passed; the callback should have the signature: `callback(line: re.Match)`
    '''
    (self.hooks_prefix if with_prefix else self.hooks_no_prefix).register(patt, callback)
```
</details>

> Registers a callback  
> If keep_prefix, then lines that have the prefix are passed. callback should have the signature: `callback(line: re.Match, prefix: re.Match, time: time.struct_time)`  
> Otherwise, lines that don't have a prefix are passed; the callback should have the signature: `callback(line: re.Match)`

## register_chat_callback(...)
```python
def register_chat_callback(callback: Callable(ForwardRef('RS.UM.User'), str, bool))
```

[`_rsruntime/lib/rs_lineparser.py@117:122`](/_rsruntime/lib/rs_lineparser.py#L117)

<details>
<summary>Source Code</summary>

```python
def register_chat_callback(self, callback: typing.Callable[[typing.ForwardRef('RS.UM.User'), str, bool], None]):
    '''
        Registers a callback for when chat is recieved
            The callback should have the signature `callback(user: RS.UserManager.User, message: str, insecure: bool)`
    '''
    self.hooks_chat.register(callback)
```
</details>

> Registers a callback for when chat is recieved  
> The callback should have the signature `callback(user: RS.UserManager.User, message: str, insecure: bool)`