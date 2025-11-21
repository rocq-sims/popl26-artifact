# rocq-sims

This README documents the artifact for the POPL'26 paper *A Family of Sims with Diverging Interests*.
This artifact mainly contains a frozen version of the `rocq-sims` library.
For an up-to-date version of the `rocq-sims` library, see its repository at https://github.com/rocq-sims/rocq-sims/

The paper contains clickable links to this development.
The README in the `rocq-sims` directory/submodule documents the main library.
The README in the `sims-compcert` directory/submodule documents the CompCert instantiation of the theory.

## How to build

This development mainly depends on Rocq 8.20 and the `coinduction` library.
The CTree instantiation depends on the CTree 2.0 library.
The CompCert instantiation depends on CompCert (pinned to commit 1670ae76afdb2546402898b6e9064d7229845d24 from May 2025).

The following command fetches the source code of our development, plus the CompCert dependency.

```sh
git clone --recurse-submodules https://github.com/rocq-sims/popl26-artifact.git
cd popl26-artifact
```

If you downloaded an archive from Zenodo, use these commands from the directory of this README instead
(to fetch the CompCert dependency, if you intend to build this part of the development):
```sh
git clone https://github.com/AbsInt/CompCert.git
cd CompCert
git checkout 1670ae76afdb2546402898b6e9064d7229845d24
cd ..
```

The following commands build the main development in a fresh environment, including examples and the CTree instantiation.

```sh
# prepare a fresh switch
opam update
opam switch create rocq-sims ocaml-base-compiler.4.14.2
eval $(opam env --switch=rocq-sims --set-switch)
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

## Implementation notes

The theorem statements in Rocq should closely match the statements in the paper.
Note the following small differences in the implementation.

In the paper, case analyses in `divpresF` and `µsimF` use logical `or` connectors,
whereas the Rocq development uses more practical Rocq variants. These are equivalent.

The formal definition of `µsimF` from Section 3 does not appear as is in the Rocq development,
because it is a special case of the more general parameterized `simF` from Section 4
(with parameters `(freeze_div, lock, delay)`).

## Axioms

A few results depend on the law of excluded middle: comparisons with other notions of simulation,
the up-to bind technique, the forward/backward implication and the CompCert instantiation.
