# Voodoo

Voodoo is the OCaml documentation generator.

## Getting started

This package is intended to be used as part of an
[ocurrent](https://github.com/ocurrent/ocurrent) pipeline, for example
via [ocaml-docs-ci](https://github.com/ocurrent/ocaml-docs-ci). As such,
it's tricky to use in isolation.

Having said that, there is some rudimentary support for using it this way. The steps below will guide you to build to build a small documentation website locally.

### Setup your development environment

You need Opam, you can install it by following [Opam's documentation](https://opam.ocaml.org/doc/Install.html).

With Opam installed, you can install the dependencies with:

With Opam installed, you can install the dependencies in a new local switch with:

```bash
make switch
```

Or globally, with:

```bash
make deps
```

Then, build the project with:

```bash
make build
```

### Generating an example

To create an example, run:

```bash
make example
```

This will create a sample website with the documentation of `ocaml-base-compiler` in `_generated/output/packages/`.

Here are some command that will be executed when runnning the above:

```bash
$ mkdir _generated; cd _generated
$ opam exec -- dune exec -- voodoo-prep
$ opam exec -- dune exec -- voodoo-do -p ocaml-base-compiler -b
$ opam exec -- dune exec -- voodoo-gen pkgver -o output/
$ opam exec -- dune exec -- voodoo-gen generate-json -o output/
$ opam exec -- dune exec -- voodoo-gen packages -o output/
```

Read further for the detailed version of the above commands.

```bash
$ voodoo-prep
Warning: No universes have been specified: will generate dummy universes
$ voodoo-do -p ocaml-base-compiler -b
$ voodoo-do -p result -b
$ voodoo-do ...
```

If the packages are done out-of-order, voodoo-prep will alert that there are
missing dependencies:

```bash
$ voodoo-do -p odoc -b
...
Missing dependency: Stdlib c21c5d26416461b543321872a551ea0d
...
```

In this case, we need to run `voodoo-do -p ocaml-base-compiler -b` first.

Note that when being used in this mode, the `-b` (blessed) switch should
always be passed to `voodoo-do`

> At this point, to view the output, use odoc to generate the support files:
> 
> ```bash
> $ odoc support-files -o html
> ```
> 
> and load the package index in your browser:
> 
> ```bash
> $ open html/packages/ocaml-base-compiler/4.11.1/index.html
> ```

An alternative to executing `voodoo-do -p ...` in order is simply to run
`voodoo-do` with no arguments. This naively executes voodoo-do in order
based on the current switch's dependencies, and is slow and inefficient.

To generate the website with the documentation, run:

```bash
$ voodoo-gen pkgver -o output/
$ voodoo-gen generate-json -o output/
$ voodoo-gen packages -o output/
```

And serve it with:

```bash
$ opam install dream-serve
$ dream-serve output/packages/
```
