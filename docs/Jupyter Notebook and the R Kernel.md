# Jupyter Notebook

**Foreword**

Notes.

-----

[TOC]

-----

## Distribution

- [Jupyter.org (Official Website)](http://jupyter.org/)
- On Windows, use the Anaconda distribution.
	- The stack includes the Jupyter (IPython) Notebook, the Cloud (online resources), the Navigator (home screen), the prompt (shell), the QTConsole, and the Spyder IDE.
- On Linux, the Jupyter Stack can easily be installed through the bash with `pip`. 
	- Launch Jupyter Notebook (`jupyter notebook`) or any tools through the bash or in the GUI.
- Anaconda2 has Python 2 by default (root) and Anaconda3, no surprise, has Python 3!
- However, other kernels can be installed on any distribution (another Python version, R, bash, etc.).

## Implementing the R Kernel (or irkernel)

- Start R (admin or sudo mode).
- Enter the commands:

```r
install.packages(c('repr','pbdZMQ', 'devtools')) # reprisalready on CRAN

devtools::install_github('IRkernel/IRdisplay')

devtools::install_github('IRkernel/IRkernel')

IRkernel::installspec()
```

- Configure Jupyter in R: `IRkernel::installspec(user = FALSE)`.
- Start Jupyter Notebook.