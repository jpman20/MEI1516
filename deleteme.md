*This text will be italic*

**This text will be bold**


| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 


````python
#!/usr/bin/env python
import sys, markdown,re

MIMETEX_LOC="http://some.server.com/cgi-bin/mimetex.cgi"

def sanitizeLatex(text):
    return re.sub(r"\\",r"%5C", text)

def wrapLatexBlock(text):
    return '<img alt="equation" class="block" src="%s?%s"></img>'%(MIMETEX_LOC,text)

def wrapLatexInline(text):
    return '<img alt="equation" class="inline" src="%s?%s"></img>'%(MIMETEX_LOC,text)

def prepLatexBlock(matchobj):
    return wrapLatexBlock(sanitizeLatex(matchobj.group()[2:-2]))

def prepLatexInline(matchobj):
    return wrapLatexInline(sanitizeLatex(matchobj.group()[1:-1]))


if __name__ == "__main__":
    # initialise markdown
    md=markdown.Markdown()
    raw_md=open(sys.argv[1],"r").read()

    ##
    # deal with embedded latex
    ##
    raw_md=re.sub(r'\$\$(.*?)\$\$',prepLatexBlock, raw_md)
    raw_md=re.sub(r'\$(.*?)\$',prepLatexInline, raw_md)

    ##
    # once latex is parsed, convert md to html
    ##
    main_html=md.convert(raw_md)

    # hey presto!
    print(main_html)
````


