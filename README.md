# vim-vfs

This is work in progress.

Virtual File System for Vim interface.

This plugin only provide interface to the VFS.

## Usage

TBD

## Interfaces

### URI

vim-vfs use strictly URI generic syntax RFC 3986. On Windows, the path pointed to local path `C:\Program Files\foo\bar.txt` should be `xxx://C:/Program%20Files/foo/bar.txt`.

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
`xxx` should be used as protocol name when vim open with `:e xxx:/path/to/the/file`.

### Listing Files

When the URI is ended with slash, it is treated as a directory. vim-vfs call `vfs#readdir(uri)`. `vfs#readdir` must return list of dictionary like below.

```
[ {File Object}, {File Object}, ...]
```

### Read File

When the URI is not ended with slash, it is treated as a file. vim-vfs call `vfs#readfile(uri)`. `uri` is a string to fully path to the uri. `vfs#readfile` must return dictionary like below.

```
{
  "id": "identifier of the file",
  "name": "name of the file",
  "content": "content of the file"
}
```

If the content is binary, object have list of strings which way is used on `systemlist()`.

```
{
  "id": "identifier of the file",
  "name": "name of the file",
  "content": ["content", "of", "the", "file"]
}
```

### Write File

vim-vfs call `vfs#writefile(uri, content)`. `content` can be passed string or list which is separated with `CR`. This is a way to pass binary-content like `systemlist()`. `vfs#writefile` does not return anything.

## License

[NYSL](http://www.kmonos.net/nysl/index.en.html)
