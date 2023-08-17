---
title : Better testing - Pytest
notetype : feed
date : 28-11-2022
---

# Parametrized

# TemporaryFile, TemporaryDirectory



from tempfile import NamedTemporaryFile, TemporaryDirectory


# Mocks

From https://github.com/apache/airflow/blob/main/tests/operators/test_python.py
```python
@unittest.mock.patch("airflow.operators.python.prepare_virtualenv")

def test_pip_install_options(self, mocked_prepare_virtualenv):

def f():

import funcsigs # noqa: F401

mocked_prepare_virtualenv.side_effect = prepare_virtualenv

self._run_as_operator(

f,

requirements=["funcsigs==0.4"],

system_site_packages=False,

pip_install_options=["--no-deps"],

)

mocked_prepare_virtualenv.assert_called_with(

venv_directory=unittest.mock.ANY,

python_bin=unittest.mock.ANY,

system_site_packages=False,

requirements_file_path=unittest.mock.ANY,

pip_install_options=["--no-deps"],

)

def test_templated_requirements_file(self):
```

# Pytest mark

@pytest.mark.usefixtures("clear_db")

What does it do?

