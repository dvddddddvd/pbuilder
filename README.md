# Usage
Put `pbuilder.sty` in your LaTeX/Overleaf folder, then in the preamble add
`\usepackage{pbuilder}`.

# Basic Usage
```latex
... % other preamble
\usepackage{pbuilder}

\begin{document}

... % intro

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
```latex
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

```latex
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

```latex
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

```latex
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

Examples
```latex
% in preamble

% format of the examples
\newcommand\exampleitemformat{
    \ifcsdef{getproblemsource}{\begin{example}[\getproblemsource]}{\begin{example}}%
        \getproblem
    \end{example}
    
    \ifcsdef{getsolution}{\textit{Solution.} \getsolution}{}
}

\newcommand\exampleitemoverall[1]{
    #1
}

% some commands to reload the "Examples" section each time
\saveandreset{Examples} % init

\newcommand\loadexample{%
    \load{Examples}%
}
\newcommand\showexample{
    \showonecustom{\exampleitemformat}{\exampleitemoverall}
    \saveandreset{Examples}
}

...

% an example in text
\loadexample
\problem{...}
\solution{...}
\showexample

% you might want this, for instance, if you want to let your students try all the examples ahead of the session
\section*{All examples used} 
\loadexample
\buildproblems

```

# Some documentation

## Data organization
The rough org structure of data is:

```
sections    problems    fields

Section A - Problem 1 - problem
          |           | solution
          |           | (hint?)
          - Problem 2 - problem
          |           | solution
          |           | (hint?)
Section B - Problem 1 - problem
          |           | solution
          |           | (hint?)
          - Problem 2 - problem
          |           | solution
          |           | (hint?)     
```

## Fields
`fields` are things like `problem(s)`, `solution(s)` and `hint(s)`, but you can also define your own in the preamble using

```latex
\newfield{<field>}
```

This makes the command `\<field>`.

The package ships with the following by default:
```latex
%% standard template
\newfield[key]{problem}
\newfield{solution}
\newfield{hint}
```

The `[key]` option just indicates that we increase the counter whenever we see a new `\problem`. This means that the other fields don't have to be in order, as long as it is between the current `\problem` and the next `\problem`.

```latex
% this works
\problem{...}
\solution{...}
\hint{...}

% this also works
\problem{...}
\hint{...}
\solution{...}
```

## Statuses
`status(es)` are basically `field(s)` but boolean.
```latex
% e.g. if we want to track which problems were used

% preamble
\newstatus{used}

% a problem
\problem{...}
\used
```

## "Compilation"
Each `<field>` comes with a `\build<field>s` command. This compiles all instances of the field given so far "in memory".

`\build<field>s` can be customized by changing `\<field>itemformat` and `\<field>overallformat`:

```latex
% output of \build<field>s
\<field>overallformat{
    \<field>itemformat
    \<field>itemformat
    ...
    \<field>itemformat
}
```

The defaults are
```latex
\newcommand\fieldoverallformat[#1]{
    \begin{enumerate}
        #1
    \end{enumerate}
}

\newcommand\fielditemformat[#1]{
    \item[\getproblemnumber.] \get<field>
}
```
Ok, this is a slight simplification.
## Custom item formats

In `\<field>itemformat`, you are allowed to use **any** field available via `\get<field>`. In addition, you can refer to the problem number (within the section) by `\getproblemnumber`.

Useful patterns:
- If you want to check e.g. if there's a solution, you can do
```latex
\ifcsdef{getsolution}{%
    % if there is a solution
}{%
    % if there isn't a solution
}
```
The default `\<field>itemformat` actually skips over problems whose `<field>` is blank.

## Sections
One can store and wipe the memory using `\saveandreset{<section-name>}`. This is typically happens after you compile the problems.

To recover a previously stored section, you can do `\load{<section-name>}`.

## Compiling across sections

TODO
 
## Other commands
e.g. `\showonecustom{...}{...}`


### TODOs:
- finish basic docs
- more examples
    - statuses
    - problem bank