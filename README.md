# Gerbil LibXML Bindings

This package provides Gerbil bindings to `libxml2`.

## Dependencies

You need to have `libxml2` installed in your system.

In ubuntu, it is installed by default.

## Installation

To install the package in your `$GERBIL_PATH` (`~/.gerbil` by default):
```shell
$ gerbil pkg install github.com/mighty-gerbils/gerbil-libxml
```

## API
To use bindings from this package:
```scheme
(import :clan/xml/libxml)
```

### parse-xml
``` scheme
(parse-xml source [url: "none.xml"] [encoding: (encoding "UTF-8")] [options: (options parse-xml-default-options)] [namespaces: (ns [])]) -> sxml | error

  source     := string | u8vector | port
  url        := string for base URL
  encoding   := string | #f; strings are always UTF-8 encoded for parsing.
  options    := fixnum (ior of libxml parser options)
  namespaces := alist or hash-table mapping urls to namespace prefixes
```

 Parses XML data from *source* into SXML + CDATA unless collapsed with XML_PARSE_NOCDATA.

 namespace's document defined prefixes are ignored; the *url* is used as the
 canonical prefix if it is not in the *namespaces* mapping. Signals an error on
 invalid *source*.

 *options* is fixnum denoting XML parsing options. Available options are:
```
XML_PARSE_RECOVER
XML_PARSE_NOENT
XML_PARSE_DTDLOAD
XML_PARSE_DTDATTR
XML_PARSE_DTDVALID
XML_PARSE_NOERROR
XML_PARSE_NOWARNING
XML_PARSE_PEDANTIC
XML_PARSE_NOBLANKS
XML_PARSE_XINCLUDE
XML_PARSE_NONET
XML_PARSE_NODICT
XML_PARSE_NSCLEAN
XML_PARSE_NOCDATA
XML_PARSE_NOXINCNODE
XML_PARSE_COMPACT
XML_PARSE_HUGE
```

Details of above flags can be found at [LibXML reference documentation](http://www.xmlsoft.org/html/libxml-parser.html#xmlParserOption).

There's also default options set as *parse-xml-default-options* variable which currently is set
to:
``` scheme
(fxior XML_PARSE_NOENT
       XML_PARSE_NONET
       XML_PARSE_NOBLANKS)
```

Examples
``` scheme
> (import :std/net/request)
> (import :clan/xml/libxml)
> (parse-xml (request-text (http-get "https://www.w3schools.com/xml/note.xml")))
(*TOP* (note (to "Tove") (from "Jani") (heading "Reminder") (body "Don't forget me this weekend!")))
```


### parse-html
``` scheme
(parse-html source [url: "none.html"] [encoding: (encoding "UTF-8")] [options: (options parse-html-default-options)] [filter: (filter-els [])]) -> sxml | error

  source   := string | u8vector | port
  url      := string for base URL
  encoding := string | #f; strings are always UTF-8 encoded for parsing.
  options  := fixnum (ior of libxml parser options)
  filter   := list of strings
```

Parses HTML data from given *source* into SXML + CDATA.
*source*, *encoding*, *url*, *options* as above
*filter*: list of strings, elements to be removed from the tree
Signals an error on invalid *source*.

Available *options* values are:
```
HTML_PARSE_RECOVER
HTML_PARSE_NODEFDTD
HTML_PARSE_NOERROR
HTML_PARSE_NOWARNING
HTML_PARSE_PEDANTIC
HTML_PARSE_NOBLANKS
HTML_PARSE_NONET
HTML_PARSE_NOIMPLIED
HTML_PARSE_COMPACT
HTML_PARSE_IGNORE_ENC
```

Details of above flags can be found at [LibXML reference documentation](http://www.xmlsoft.org/html/libxml-HTMLparser.html#htmlParserOption).

There's also default options set as *parse-html-default-options* variable which currently is set
to:
``` scheme
(fxior HTML_PARSE_RECOVER
       HTML_PARSE_NOERROR
       HTML_PARSE_NOWARNING
       HTML_PARSE_NONET
       HTML_PARSE_NOBLANKS)
```


# License and Copyright

© 2017-2023 The Gerbil Core Team and contributors; License: LGPLv2.1 and Apache 2.0

Originally written by vyzo.
