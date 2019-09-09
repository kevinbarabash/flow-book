# Chapter 8: .js.flow files

The official docs on
[Creating Library Definitions](https://flow.org/en/docs/libdefs/creation)
briefly mentions .js.flow files and that docs will be coming. In the meantime
this chapter will provide some unofficial docs until the official docs arrive.

.js.flow files are an alternative to adding library definitions in the
flow-typed folder. The benefit of these is that they can be shipped with a
library and flow will find them when importing dist/index.js (or whatever the
"main" file is for an node package).

Unlike the files in flow-typed though, a top level `declare module` statement
isn't needed. All declarations are scoped to the module being corresponding to
the .js.flow file. Only those that are explicilty exported will be available to
other modules importing the package in question.

Since .js.flow files correspond to regular .js files, they can also be used to
provide types for files being imported from within a module, e.g.
`import Bar from "foo/bar.js"`;

These files can also import types from other files so it's preferable to use
them instead of library definitions in flow-typed folder.

TODO: provide some examples
