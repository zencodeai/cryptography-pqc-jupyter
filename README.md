# Post-Quantum Cryptography Notebooks Setup (Miniconda)

This README explains how to set up a Python environment using **Miniconda** in order to run the Jupyter notebooks

The environment will include:

- Python 3.12 (or comparable 3.x version)
- `jupyter` / `jupyterlab`
- The `cryptography` package with PQC algorithms support.

---

## 1. Install Miniconda

If you do not already have Miniconda installed:

1. Download Miniconda for your platform from the official website.
2. Run the installer and follow the prompts.
3. Open a new terminal so the `conda` command is available.

On macOS / Linux, you can usually verify with:

```bash
conda --version
```

On Windows, use the **Anaconda Prompt** or a terminal where `conda` is on the PATH.

---

## 2. Create a Conda Environment

Create a dedicated environment for ML-KEM experiments. In this example we call it `cryptography-pqc`,
but you can choose any name you like.

```bash
conda create -n cryptography-pqc python=3.12 -y
```

Activate the environment:

```bash
conda activate cryptography-pqc
```

You should now see `(cryptography-pqc)` (or your chosen name) in your shell prompt.

---

## 3. Install Required Packages

Inside the activated environment, install Jupyter.

```bash
# Jupyter Notebook or JupyterLab
conda install -y jupyterlab
```

Note that **liboqs-python** cannot be installed durectly via **pip** or **conda**. See [OQS github](https://github.com/open-quantum-safe/liboqs-python) for installation instrucations

> **Note:** Make sure you are still inside the `cryptography-pqc` environment when you run `pip`,
> otherwise you may install into a different Python environment than intended.

---

## 4. (Optional) Verify OpenSSL / Backend

The `cryptography` package uses a backend (commonly OpenSSL) to provide cryptographic primitives.
Newer versions expose ML-KEM (Kyber) through the KEM API. To verify, you can start a Python REPL
and try:

```python
from cryptography.hazmat.primitives.kem import MLKEM768
```

If this import works without errors, you have ML-KEM support available.

---

## 5. Launch Jupyter and Open the Notebook

From the directory where the notebook is located, run:

```bash
conda activate cryptography-pqc
jupyter lab
```

or, if you prefer the classic notebook interface:

```bash
conda activate cryptography-pqc
jupyter notebook
```

A browser window should open. Navigate to the chosen notebook and open it.

---

## 6. Run the Notebook

Inside Jupyter:

1. Select the `Kernel` menu (if available) and ensure it uses the Python kernel from your `cryptography-pqc` environment.
2. Run the cells in order (e.g., `Run All`), or step through them one by one.

The notebook will:

- Check your environment (`Python`, `cryptography`, ML-KEM support)
- Demonstrate key generation, encapsulation, and decapsulation
- Provide a simple benchmark
- Give a high-level mathematical explanation of ML-KEM

---

## 7. Updating / Recreating the Environment

If you need to update packages later, you can run:

```bash
conda activate cryptography-pqc
conda update cryptography jupyterlab
```

If your environment becomes inconsistent, you can remove and recreate it:

```bash
conda deactivate
conda env remove -n cryptography-pqc
conda create -n cryptography-pqc python=3.11 -y
conda activate cryptography-pqc
conda install -y jupyterlab cryptography
```

---

## 8. Troubleshooting

- **`ModuleNotFoundError: No module named 'cryptography'`**

  Ensure your environment is activated (`conda activate cryptography-pqc`) before starting Jupyter.

- **Import error for `MLKEM768`**

  Your installed version of `cryptography` may be too old or built against a backend that does not
  expose ML-KEM. Try updating the package:

  ```bash
  conda activate cryptography-pqc
  conda update cryptography
  ```

- **Jupyter uses the wrong Python environment**

  Start Jupyter *from within* the activated environment, or explicitly configure a kernel via:
  `python -m ipykernel install --user --name cryptography-pqc --display-name "Python (cryptography-pqc)"`.

Once everything is working, you should be able to run the ML-KEM notebook end-to-end.
