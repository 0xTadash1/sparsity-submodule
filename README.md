# Sparsity Submodule

My practice on the combination of git `submodule` & `sparse-checkout`.
How to bring any directory/file from a foreign repository.

## At First (setup)

At here, I would like to use `{test/,tool/}` from `munificent/craftinginterpreters`.

```sh
git clone --depth=1 --filter=blob:none --sparse \
	https://github.com/munificent/craftinginterpreters.git \
	test/craftinginterpreters

git submodule add \
	https://github.com/munificent/craftinginterpreters.git \
	test/craftinginterpreters

# Move `test/craftinginterpreters/.git/` to `.git/`
git submodule absorbgitdirs
#
# > A repository that was cloned independently and later added as
# > a submodule or old setups have the submodules git directory
# > inside the submodule instead of embedded into the
# > superprojects git directory.
#
# -- <https://man7.org/linux/man-pages/man1/git-submodule.1.html>

git -C test/craftinginterpreters sparse-checkout set test/ tool/
```

## After the Second time (clone)

```sh
# No

git clone --recursive https://github.com/0xTadash1/sparsity-submodule

# Instead

git clone https://github.com/0xTadash1/sparsity-submodule

git submodule init
git clone --depth=1 --filter=blob:none --sparse \
	https://github.com/munificent/craftinginterpreters.git \
	test/craftinginterpreters
git submodule absorbgitdirs
git -C test/craftinginterpreters sparse-checkout set test/ tool/
```

## `git submodule absorbgitdirs`

After `git submodule deinit`, we can `checkout` the sparsity submodules again by simply
`git submodule update --init --filter=blob:none --depth=1 test/craftinginterpreters`.

This is because `git submodule absorbgitdirs` records the `sparse-checkout` configuration
under `.git/` in the superprojects.

## cf.

- <https://stackoverflow.com/questions/600079/how-do-i-clone-a-subdirectory-only-of-a-git-repository>
- <https://stackoverflow.com/questions/45688121/how-to-do-submodule-sparse-checkout-with-git>
