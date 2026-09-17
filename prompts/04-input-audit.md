# The Input Audit

Every text box on your app is a place where a stranger types instructions to
your server. Unsanitized, that input closes your SQL quote and appends its
own query, or plants a script that runs in every other visitor's browser, or
uploads a file your server ends up executing. This prompt traces each path
from input to something dangerous.

## The prompt

```
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
```
