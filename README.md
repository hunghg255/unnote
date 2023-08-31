
# What is UnNote?

[UnNote](https://unnote.vercel.app/) is an open-source website similar to Pastebin where you can store any piece of code, and generate links for easy sharing

However, what makes UnNote special is that it works with **no database**, and **no back-end code**. Instead, the data is compressed and **stored entirely in the link** that you share, nowhere else!


### Because of this design:

-   🗑️ Your data **cannot be deleted** from UnNote
-   🔞 Your data **cannot be censored**
-   👁️ The server hosting UnNote (or any clone of it) **cannot read or access** your data
-   ⏳ Your data will be accessible **forever** (as long as you have the link)
-   🔀 You can access your data on **every UnNote clone**,
-   🔍 Google **will not index** your data, even if your link is public

> **Note:** This project is a copy of [Topaz's paste service][topaz-example], with a reworked design and a few additional features (syntax highlighting, line numbers, offline usage, embedding...)

### You can use a short link to reduce url size

[short-url](https://url8.vercel.app/)

## How it works

When you click on "Generate Link", UnNote compresses the whole text using the
[LZMA algorithm](https://en.wikipedia.org/wiki/Lempel%E2%80%93Ziv%E2%80%93Markov_chain_algorithm), encodes it in
[Base64](https://en.wikipedia.org/wiki/Base64), and puts it in the optional URL fragment, after the first `#` symbol: `unnote.vercel.app/#<your data goes here>`

When you open a link, UnNote reads, decodes, and decompresses whatever is after the `#`, and displays the result in the editor.

This process is done entirely **in your browser**, and the web server hosting UnNote [never has access to the fragment](https://en.wikipedia.org/wiki/Fragment_identifier)

## Other features

### Embedded UnNote snippets

You can include UnNote code snippets into your own website by clicking the _Embed_ button and using the generated HTML code.

Here is an example of generated code and how it looks (click on the screenshot to see the interactive version)

```html
<iframe
    width="100%"
    height="243"
    frameborder="0"
    src="https://unnote.vercel.app/?l=py#XQAAAQAbAQAAAAAAAAA0m0pnuFI8c+qagMoNTEcTIfyUWbZjtjmBYcmJSzoNwS5iVMWHzvowv3IPM0vOG5cjrtDRTSVP/0biTIrrahfmbkuMQBBeSiSGpaJOqYJiKmUDYn2Gp1RtWE6gm8fLHMB4eyZ3+rEbUQwWyMcmWqvZ7m96RUeFyZdYbE85JGvhghqF8cyPB0ZjV0OQWsDxn5O5ysMrIcL+pKPk89EtLjAHhA1LZL9F3hzAtTx7I+GlyrxhhXGxAN//CvtaAA=="
></iframe>
```

[![iframe](https://raw.githubusercontent.com/bokub/UnNote/images/pagerank.png)](https://jsfiddle.net/cqr2kxf5/)

Feel free to edit the `height` and `width` attributes, so they suit your needs

### Offline usage

When you visit UnNote for the first time, its code is saved in your browser cache. After that, every UnNote link you open
will load really quick, even if your internet connexion is slow.

What if you have no internet connexion at all? No problem, UnNote will still work perfectly!

### Editor features

-   Syntax highlighting (use the language selector)
-   Enable / disable line wrapping (use the button next to the language selector)
-   Delete line (`Ctrl+D`)
-   Multiple cursors (`Ctrl+Click`)
-   Usual keyboard shortuts (`Ctrl+A`, `Ctrl+Z`, `Ctrl+Y`...)

## Maximum sizes for links

UnNote is great for sharing code snippets on various platforms.

These are the maximum link lengths on some apps and browsers.

| App     | Max length |
| ------- | ---------- |
| Reddit  | 10,000     |
| Twitter | 4,088      |
| Slack   | 4,000      |
| QR Code | 2,610      |
| Bitly   | 2,048      |

| Browser         | Max length                | Notes                                   |
| --------------- | ------------------------- | --------------------------------------- |
| Google Chrome   | (win) 32,779 (mac) 10,000 | Will not display, but larger links work |
| Firefox         | >64,000                   |                                         |
| Microsoft IE 11 | 4,043                     | Will not show more than 2,083           |
| Microsoft Edge  | 2,083                     | Anything over 2083 will fail            |
| Android         | 8,192                     |                                         |
| Safari          | Lots                      |                                         |

## Generate UnNote links

UnNote links can be created easily from your system's command line:

```bash
# Linux
echo -n 'Hello World' | lzma | base64 -w0 | xargs -0 printf "https://unnote.vercel.app/#%s\n"

# Mac
echo -n 'Hello World' | lzma | base64 | xargs -0 printf "https://unnote.vercel.app/#%s\n"

# Windows / WSL / Linux
echo -n 'Hello World' | xz --format=lzma | base64 -w0 | printf "https://unnote.vercel.app/#%s\n" "$(cat -)"
```
