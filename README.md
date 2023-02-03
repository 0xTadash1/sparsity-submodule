# Sparsity Submodule

My practice about the combination of git `submodule` & `sparse-checkout`.
To bringing the specific directories/files from a foreighn repository.

## At First (setup)

At here, I would like to use `{test/,tool/}` from `munificent/craftinginterpreters`.

```bash
git clone --depth=1 --filter=blob:none --sparse \
	https://github.com/munificent/craftinginterpreters.git \
	test/craftinginterpreters

git submodule add \
	https://github.com/munificent/craftinginterpreters.git \
	test/craftinginterpreters

# Move `test/craftinginterpreters/.git/` to `.git/`
git submodule absorbgitdirs

git -C test/craftinginterpreters sparse-checkout set test/ tool/
```

## After the Second time

```bash
git clone --recursive https://example.com/repo-with-sub-modules

# or, without `--recursive`

git clone https://example.com/repo-with-sub-modules
git submodule update --init
```

## cf.

- <https://stackoverflow.com/questions/600079/how-do-i-clone-a-subdirectory-only-of-a-git-repository>
- <https://stackoverflow.com/questions/45688121/how-to-do-submodule-sparse-checkout-with-git>
