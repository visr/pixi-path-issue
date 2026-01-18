Minimal reproducer for https://github.com/Deltares/Ribasim-NL/pull/333#issuecomment-2934981608
Has since been fixed in https://github.com/prefix-dev/pixi/pull/3895

`local_package` is a minimal Python package, that is added as an editable depedency in the pixi.toml.

Steps to reproduce:
1. Clone repo on Windows.
2. Install pixi (tested v0.48.0).
3. Run `pixi install`, note the `pypi: .\local_package` in the lockfile.
4. Open same repo under WSL, or move repo to Linux.
5. Run `pixi install --locked`, this fails with `Error:   × lock-file not up-to-date with the workspace`.
6. Run `pixi lock`, this prints `  ~ P local-package   0.1.0  ->  0.1.0` and changes `pypi: .\local_package` to `pypi: ./local_package` in lockfile.
7. Now `pixi install --locked` works on both Windows and Linux.

This bug was introduced in Pixi v0.47, in v0.46 the lockfile would contain:
```
- pypi: local_package
```

Or for a deeper package, it would have forward slashes:

```
- pypi: nested/local_package_two
```
