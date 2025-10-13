# rocq-sims

This README documents the artifact for the POPL'26 submission *A Family of Sims with Diverging Interests*.

The de-anonymized paper contains clickable links to this development.
The README in the `rocq-sims` submodule documents the main library.
The README in the `sims-compcert` submodule documents the CompCert instantiation.

## How to build

This development mainly depends on Rocq 8.20 and the `coinduction` library.
The CTree instantiation depends on the CTree 2.0 library.
The CompCert instantiation depends on CompCert (pinned to commit 1670ae76afdb2546402898b6e9064d7229845d24 from May 2025).

The following command fetches the source code of our development, plus the CompCert dependency.

```sh
git clone --recurse-submodules https://github.com/rocq-sims/popl26-artifact.git
cd popl26-artifact
```

The following commands build the main development in a fresh environment, including examples and the CTree instantiation.

```sh
# prepare a fresh switch
opam update
opam switch create rocq-sims ocaml-base-compiler.4.14.2
eval $(opam env --switch=rocq-sims)
opam repo add rocq-released https://rocq-prover.org/opam/released
opam update
# build our development
cd rocq-sims
opam install . --deps-only
dune build
```

The CompCert instantiation is built separately and depends on the main development having been built.

```sh
# build compcert
opam install menhir
cd CompCert
./configure x86_64-linux
make
# build our instantiation
cd ../sims-compcert
coq_makefile -f _CoqProject -o Makefile
make
```
