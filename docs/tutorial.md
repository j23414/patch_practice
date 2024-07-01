# Hands-on tutorial

An auto-generated hands-on tutorial to practice the protocol for creating and applying patch files using the command line. This tutorial will guide you through creating a sample project, making changes, generating patch files, and applying them in different combinations.

Hands-on Tutorial: Managing Changes with Git Patches

1. Set up a sample project:

```bash
mkdir patch_practice
cd patch_practice
git init
echo "# Patch Practice Project" > README.md
git add README.md
git commit -m "Initial commit"
```

2. Create some sample files and commits:

```bash
echo "function add(a, b) { return a + b; }" > math.js
git add math.js
git commit -m "Add addition function"

echo "function subtract(a, b) { return a - b; }" >> math.js
git commit -am "Add subtraction function"

echo "function multiply(a, b) { return a * b; }" >> math.js
git commit -am "Add multiplication function"

echo "function divide(a, b) { return a / b; }" >> math.js
git commit -am "Add division function"
```

3. Generate patch files for these commits:

```bash
mkdir patches
git format-patch -o patches HEAD~4..HEAD
```

This will create four patch files in the `patches` directory.

4. Reset your branch to the initial state:

```bash
git reset --hard HEAD~4
```

5. Apply patches selectively:

```bash
# Apply the first two patches
git apply patches/0001-Add-addition-function.patch
git apply patches/0002-Add-subtraction-function.patch

# Check the content of math.js
cat math.js

# Commit these changes
git add math.js
git commit -m "Apply addition and subtraction functions"
```

6. Try a different combination:

```bash
# Reset to initial state
git reset --hard HEAD~1

# Apply first and third patches
git apply patches/0001-Add-addition-function.patch
git apply patches/0003-Add-multiplication-function.patch

# Which generates an error:
#> error: patch failed: math.js:1
#> error: math.js: patch does not apply
```

The error you're encountering is because the patches are trying to apply to a file that has changed since the patches were created. This is similar to a merge conflict, and you can resolve it manually using the `reject` option with `git apply`:

```bash
git apply --reject patches/0003-Add-multiplication-function.patch
```



``` bash

# Check the content of math.js
cat math.js

# Commit these changes
git add math.js
git commit -m "Apply addition and multiplication functions"
```

7. Apply all patches at once:

```bash
# Reset to initial state
git reset --hard HEAD~1

# Apply all patches
git am patches/*.patch

# Check the content of math.js
cat math.js
```

8. Create a new feature and generate a patch:

```bash
echo "function power(a, b) { return Math.pow(a, b); }" >> math.js
git commit -am "Add power function"

git format-patch -o patches HEAD~1
```

This will create a new patch file in the `patches` directory.

9. Practice resolving conflicts:

```bash
# Reset to the state before the power function
git reset --hard HEAD~1

# Modify the file to create a conflict
echo "function square(a) { return a * a; }" >> math.js
git commit -am "Add square function"

# Try to apply the power function patch
git apply patches/0005-Add-power-function.patch
```

This will result in a conflict. Resolve it manually by editing `math.js`, then:

```bash
git add math.js
git commit -m "Add power and square functions, resolving conflict"
```

10. Clean up:

```bash
# View the commit history
git log --oneline

# If you want to reset to the initial state:
git reset --hard <hash-of-initial-commit>
```

This tutorial covers creating patches, applying them in different combinations, handling conflicts, and managing your repository state. It provides hands-on experience with the key concepts and commands involved in working with Git patches.

Remember to replace `<hash-of-initial-commit>` with the actual hash of your initial commit when resetting at the end.