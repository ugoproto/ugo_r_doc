---

[TOC]

---

**Foreword**

Notes & snippets.

---

## Stand-alone .Rdm document

It is one document with a header; the settings determine the output (HTML, Word, PDF) style and appearance:

```
---
title: "Document only"
output: html_document
---
```

## Embedding .Rmd sub-documents into a .Rmd document

It is also possible to create sub-documents to be casted in the main document.

The sub-documents have to headers, no settings. They are not even `html_fragment`. They simply contain Markdown text strings and code chunks.

The main document is like a stand-alone document. Its settings are applied to itself and its 'children'.

Within the main document, we call sub-documents with this code chunk (path and filename):


	```{r, child="sub1.Rmd"}
	```


We can add extra options: `eval=FALSE`, `echo=FALSE`, `include=FALSE`, etc. to bypass the main settings.

We can compile sub-documents to test them.

We compile the main document to wrap up the complete document ('parent + children').

---
