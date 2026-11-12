# Reproduce the data-engineering environment

Source: https://github.com/python-openxml/python-docx/commit/e45454602b53e8e572b179ccf1c91093ec9f4ed7

This repository republishes the tracked files of that upstream commit under their original licenses.
Upstream CI workflows are omitted from the evaluation snapshot. This commit adds environment instructions
and the dependency lock only. The source license is MIT; original license and attribution files remain.

Python 3.13.15 and uv were used on Linux. From a fresh checkout:

```sh
uv venv --python 3.13.15 .venv
export PYTHONUTF8=1 PYTHONDONTWRITEBYTECODE=1 PYTEST_DISABLE_PLUGIN_AUTOLOAD=1 CBOR2_BUILD_C_EXTENSION=0 SETUPTOOLS_SCM_PRETEND_VERSION_FOR_CBOR2=5.6.5 SETUPTOOLS_SCM_PRETEND_VERSION_FOR_CALDAV=2.0.1 SETUPTOOLS_SCM_PRETEND_VERSION_FOR_POOCH=1.9.0.dev0
mkdir -p .venv/tmp
export TMPDIR="$PWD/.venv/tmp"
uv pip install --python .venv/bin/python -r requirements-domain.lock
uv pip install --python .venv/bin/python --no-build-isolation --no-deps -e .
.venv/bin/python -B -m pytest -q -o addopts= -p no:cacheprovider tests/test_table.py tests/test_document.py tests/oxml/test_table.py tests/opc/test_rel.py tests/text/test_paragraph.py
```

The selected tests use small fixtures or in-process substitutes. They need no remote service after dependency setup.
Start a fresh checkout and rebuild its virtual environment to restore initial state.
