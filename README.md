# vim-vfs

**This is work in progress**

Virtual File System for Vim interface.

This plugin only provide interface to the VFS.

## Usage

TBD

## Design

### Namespace and Protocol

```
autoload/vfs/xxx.vim
```

vim-vfs load `xxx.vim` automatically when it's required to use the scheme `xxx`.
`xxx` should be used as scheme name when vim read/write the URI. ex: `:e xxx://path/to/the/file`.

The scheme name must always be lowercase letters. but not contains `+`, `-` or `.`.

```
scheme      = ALPHA *( ALPHA / DIGIT )
```

### Objects

#### URI

vim-vfs use strictly URI generic syntax RFC 3986. On Windows, the path pointed to local path `C:\Program Files\foo\bar.txt` should be `xxx://C:/Program%20Files/foo/bar.txt`.

```
URI         = scheme ":" hier-part [ "?" query ] [ "#" fragment ]

hier-part   = "//" authority path-abempty
            / path-absolute
            / path-rootless
            / path-empty
```

Percent encodes `%e4%b8%96%e7%95%8c` will be decoded to UTF-8 string.

See https://www.ietf.org/rfc/rfc3986.txt

#### File Object

```
{
  "id": "identifier of the file",
  "name": "name of the file"
}
```

#### Errors

If the function throw an exception, vim-vfs catch all of them. It should be string. And if the exception have prefix `vfs#xxx:`, it will be displayed as message from the function.

### API

#### Listing Files

When the URI is ended with slash, it is treated as a directory. vim-vfs call `vfs#readdir(uri)`. `vfs#readdir` must return list of dictionary like below.

```
[ {File Object}, {File Object}, ...]
```

#### Read File

When the URI is not ended with slash, it is treated as a file. vim-vfs call `vfs#readfile(uri)`. `uri` is a string to fully path to the uri. `vfs#readfile` must return dictionary like below.

```
{
  "id": "identifier of the file",
  "name": "name of the file",
  "content": "content of the file"
}
```

The content should be encoded as `&encoding` always. So if you use DBCS(double byte character set) encodings, and the content have UTF-8, it will lost some bytes when read the file. If the content is binary, object have list of strings which way is used on `systemlist()`.

```
{
  "id": "identifier of the file",
  "name": "name of the file",
  "filetype": "filetype of the content(optional)",
  "content": ["content", "of", "the", "file"]
}
```

#### Write File

vim-vfs call `vfs#writefile(uri, content)`. `content` can be passed string or list which is separated with `CR`. This is a way to pass binary-content like `systemlist()`. `vfs#writefile` does not return anything.

### User Interface

TBD

## TODO

### Nested schemes

Open the content bundled in zip file. And the zip file is located on remote SSH server.

```
:e ssh://host//home/thinca/vim.zip:/runtime/syntax.vim
```

### Better Name?

* vfsrw
* vimfs
* something other


## License

[NYSL](http://www.kmonos.net/nysl/index.en.html)
