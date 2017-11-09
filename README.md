# vim-vfs

Virtual File System for Vim interface.

This plugin only provide interface to the VFS.

## Usage

TBD

## Interfaces

### File Object

```
{
  "id": "identifier of the file",
  "name": "name of the file"
}
```

### Namespace and protocol

```
autoload/vfs/xxx.vim
```

vim-vfs load `xxx.vim` automatically when it's required to use the protocol `xxx`.
xxx should be used as protocol name when vim open with `:e xxx:/path/to/the/file`.

### Listing Files

When the URI is ended with slash, it is treated as directory. vim-vfs call `vfs#readdir(uri)`. `vfs#readdir` must return list of dictionary like below.

```
[
  {"name": "name of the somefile"},
  {"name": "name of the otherfile"}
]
```


## License

[NYSL](http://www.kmonos.net/nysl/index.en.html)

