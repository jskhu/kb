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