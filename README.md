# Usage
Put `pbuilder.sty` in your LaTeX/Overleaf folder, then in the preamble add
`\usepackage{pbuilder}`.

# Basic Usage
```
... % other preamble
\usepackage{pbuilder}

\begin{document}

<Intro>

% Problem 1
\problem{...}
\solution{...}

% Problem 2
\problem{...}
\solution{...}

... % more problems

\subsection*{Problems}
\buildproblems

\subsection*{Solutions}
\buildsolutions
```

# More examples
Hints section
```
... 
% Problem 1
\problem{...}
\solution{...}

% Problem 2
\problem{...}
\solution{...}
\hint{...} % not every problem needs to have a hint!

% Problem 3
\problem{...}
\solution{...}

...

\subsection*{Hints}
\buildhints
```

Custom formats (e.g. if you want to have the solutions repeat the problem and the solutions)

```
...
% in preamble
\renewcommand\solutionitemformat{
    \textbf{Problem \getproblemnumber.} \getproblem
    
    \textit{Solution.} \getsolution

    }{}
    
}
\renewcommand\solutionoverallformat[1]{
  #1  
}

...

\subsection*{Solutions}
\buildsolutions         % same usage!

```
New fields - e.g. if you wanted to add sources for some of the problems.

```
...
% in preamble
\newfield{problemsource}


% and maybe I want to incorporate it into the solutions:

\renewcommand\solutionitemformat{
    \ifcsdef{getsolution}{
        \textbf{Problem \getproblemnumber.} \ifcsdef{getproblemsource}{(\getproblemsource)}{} \getproblem
    
        \textit{Solution.} \getsolution

    }{}
    
}
...
```

Multiple sections

```
...         % preamble

\section{Section A}

% Problem A1
\problem{...}
\solution{...}

% Problem A2
\problem{...}
\solution{...}
\hints{...}

...

\subsection*{Problems}
\buildproblems

\saveandreset{sectionA} % resets the problem counter and stores everything so far

\newpage
\section{Section B}

% Problem B1
\problem{...}
\solution{...}

...

\subsection*{Problems}
\buildproblems

\saveandreset{sectionB}

\newpage
\section*{Hints}
\buildallhints

\newpage
\section*{Solutions}
% maybe we only have solutions for Section A...

\load{sectionA}
\buildsolutions

```

