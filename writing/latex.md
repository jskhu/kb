# Latex Tips and Tricks

Dump of useful latex commands

## Easy \mathbf implementation

Source: https://tex.stackexchange.com/questions/45712/faster-way-of-writing-mathbf
```latex
\documentclass{article}

\def\*#1{\mathbf{#1}}

\def\ab{ab}
\begin{document}
 $\*v, \*w, \*\ab, \*\Gamma$.
\end{document}
```

## arg min

Source: https://tex.stackexchange.com/questions/5223/command-for-argmin-or-argmax
```latex
\newcommand{\argmin}{\operatornamewithlimits{arg\,min}}
```

## Multiple plots
Source: https://tex.stackexchange.com/questions/140833/arranging-multiple-plots-in-a-grid-inside-a-figure-subfloat
```latex
\documentclass{article}
\usepackage{graphicx}
\usepackage{subfig}

\begin{document}

\begin{figure}
\begin{tabular}{cc}
  \includegraphics[width=65mm]{it} &   \includegraphics[width=65mm]{it} \\
(a) first & (b) second \\[6pt]
 \includegraphics[width=65mm]{it} &   \includegraphics[width=65mm]{it} \\
(c) third & (d) fourth \\[6pt]
\multicolumn{2}{c}{\includegraphics[width=65mm]{it} }\\
\multicolumn{2}{c}{(e) fifth}
\end{tabular}
\caption{caption}
\end{figure}
\end{document}
```

## Numpy to bmatrix
Source: https://stackoverflow.com/questions/17129290/numpy-2d-and-1d-array-to-latex-bmatrix
```python
import numpy as np

def bmatrix(a):
    """Returns a LaTeX bmatrix

    :a: numpy array
    :returns: LaTeX bmatrix as a string
    """
    if len(a.shape) > 2:
        raise ValueError('bmatrix can at most display two dimensions')
    lines = str(a).replace('[', '').replace(']', '').splitlines()
    rv = [r'\begin{bmatrix}']
    rv += ['  ' + ' & '.join(l.split()) + r'\\' for l in lines]
    rv +=  [r'\end{bmatrix}']
    return '\n'.join(rv)
```