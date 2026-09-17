---
description: Trace every path where user input reaches something dangerous
---

Trace every path where user input reaches something dangerous in this repo:

1. SQL or NoSQL queries built with string concatenation or template literals
   instead of parameters
2. Input passed to eval, exec, child_process, or shell commands
3. File uploads: is the filename sanitized, is the type verified server-side,
   can I upload an executable or path-traverse with ../
4. User content rendered as HTML without escaping, including
   dangerouslySetInnerHTML and markdown renderers
5. Endpoints with no server-side validation at all

Output: file and line | input source | what it reaches | a working example
payload | severity | fix.
