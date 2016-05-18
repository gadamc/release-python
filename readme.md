# Releasing a Python package to PyPI

The following instructions show you step-by-step how to release a new version of your software to the [PyPI repository](https://pypi.python.org/). It assumes you are using `git` as your source control manager.

Besides major, minor and patch releases, version numbers ending in `a` (alpha), `b` (beta), and `rc` (release candidate) may also be uploaded to PyPI.
However, the PyPI service providers discourage uploading development and nightly snapshots. 


## Getting set up

For this release process to work, there are a handful of prerequistes.

1. Register for an account on [PyPI](https://pypi.python.org/pypi?%3Aaction=register_form) as well as an account on [Read The Docs](https://readthedocs.org)
2. \[Optional\] [Setup your ~/.pypirc file](https://docs.python.org/2/distutils/packageindex.html#the-pypirc-file)
3. Install and/or upgrade the following python packages
    * `pip install --upgrade pip`
    * `pip install wheel`
    * `pip install twine`
4. You must be a maintainer to the PyPI package index.

## Pushing to PyPI

### Pull in latest master to local

Checkout the branch you want to release (typically master) and ensure it is up to date with everything you want to release and test against.
    
    git checkout master
    git pull --rebase

### Update Version Number

1. Remove the `dev` and other extensions from the version number in all of the necessary locations for your project. For example

    `2.0.0.dev -> 2.0.0`
    
2. If necessary, set the Development Status 'classifier' array in setup.py to

    ```
    Development Status :: 5 - Production/Stable
    ```

3. Update the `CHANGES.rst` with an entry for this version.

4. Commit these files `git commit -a -m "Prepare for 2.0.0 release"`.

5. Push the commit `git push origin master`

### Tag the commit

Create an **annotated** tag for the build e.g.:

1. Run `git tag -a "2.0.0"`.
2. An editor will open for the tag message.
3. Paste into the editor the `CHANGES.rst` entry for this version.
4. Push the tag `git push origin "2.0.0"`.

While the packaging process doesn't require a tag, we require it so we can identify the commits a release was cut from.

### Clean, build and upload to PyPI

1. Clean out old build/dist folders `rm -rf build dist` 

2. Build source and wheel distributions `python setup.py sdist bdist_wheel`

3. Upload with `twine` (DO NOT use `python setup.py sdist bdist_wheel upload` as it is not secure!).
You should be asked for your PyPI credentials. 

    `twine upload dist/*`
    
### Update to next development phase

1. Update the version number and append `dev`. By updating **after** publishing to PyPI, it is clear
to developers which version is being worked on in the open-source repository and allows developers to use the different versions available without confusion. Unless we've explicitly decided upon the next version number, update the version number to the next logical value by adding `1` to the patch number and add `.dev`:

    `2.0.0 -> 2.0.1.dev`

    If we have explicitely decided upon the next version number, then updated the version number appropriately, such as

    `2.1.0.dev`


2. If necessary, set the Development Status 'classifier' array in setup.py to something other than `Development Status :: 5 - Production/Stable`.  We would only do this if we intend for the next release to be a non-stable release.

    Some options are

    ```
    Development Status :: 1 - Planning
    Development Status :: 2 - Pre-Alpha
    Development Status :: 3 - Alpha
    Development Status :: 4 - Beta
    Development Status :: 5 - Production/Stable
    ```
    
    [Complete list](https://pypi.python.org/pypi?%3Aaction=list_classifiers)

3. Update `CHANGES.rst` by adding a new line for the next release, such as
    
    ```
    2.1.0 (Unreleased)
    ```

4. Commit these files `git commit -a -m "Starting next development phase at 2.1.0.dev."`.

5. Push the commit `git push origin master` 
