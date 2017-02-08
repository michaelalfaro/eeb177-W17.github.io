---
layout: post
title: Week 5- Writing with Markdown and LaTeX
date:   2017-02-08
author: Gaurav Kandlikar
---

Scientific documents are often complicated combinations of text, figures, tables, and references. Managing large documents quickly gets complicated amd cumbersome when using word processors like MS Word. Many scientists find it easier to manage such documents with LaTeX, which saves text in a plain text format and compiles text, images, tables, and references into beautifully typeset documents. See Allesina & Wilmes Chapter 7 for a comprehensive explanation of the power of LaTeX.

Please download [this zip file](https://github.com/eeb177-W17/eeb177-W17.github.io/raw/master/weekly_content/week5/exercise-5.zip)<sup>0</sup> to follow along in this exercise. Extract this zip file into a new `exercise-5` folder within your `lab-work` directory.

### Exercise 0: Markdown
Markdown is a simple markup language that we can use for simple(r) documents- for example, I use it all the time for taking notes in classes. Markdown is basically a way for you to write in plain-text (i.e. just text; no bolding, no underlining) but in a way that you can render your plaintext to have bells and whistles (like bolding, underlining). 

Markdown files are saved as `<file>.md` or `<file>.markdown`. Let's explore the file `md-example.md` in the `markdown` folder of `exercise 5`. Open it using gedit- you should see the following text.

<pre>
## Here is a markdown file

Those `#` symbols above indicate that we want a heading. 

We can italicize characters by wrapping them in a pair of asterisks (`*`): for example, *here is some italicized text*

We can bold characters by wrapping them in a pair of two asterisks (`**`): for example, **here is some bolded text**

We can italicize and bold characters by wrapping them in a pair of three asterisks (`***`): for example, ***here is some italicized text*** 

We can include links in this way: `[text we want to show up](link we want it to point to)`: for example, [this is a link to the course github page](http://eeb177-w17.github.io)

We can include figures in this way: `![alt-text-to-figure](link to figure or path to figure)`: for example, [this is picture of an awesome forest in New Guinea](http://www.abc.net.au/cm/lb/7231362/data/4-tree-diversity-data.jpg)
</pre>  

If you add, commit, and push this file to Github, you will see that github turns all of the asterisks and links into a very pretty document. 

### Exercise 1: Your first LaTeX document 
The first LaTeX document we will compile is one that comes from your `CSB` folder. Navigate to the `latex_1` directory. You should see a file named `latex-doc-1.tex`. Open this file using `gedit`. You should see this document:  

<pre>
\documentclass[12pt]{article}  
\title{A simple \LaTeX\ document}     
\author{Stefano Allesina, Madlen Wilmes}  
\date{}  
\begin{document}  
\maketitle  
\begin{abstract}  
Here we type a compelling abstract of our paper.  
\end{abstract}  
\section{Introduction}  
Every scientific publication needs an introduction. 
\section{Materials \& Methods}  
The Hardy-Weinberg equilibrium model constitutes the null model of population genetics. It characterizes the distributions of genotype  frequencies in populations that are not evolving.  
\end{document}
</pre>

Let's explore some of the important components. Note that there is a "preamble" to this document- in other words, we start writing the document officially at the line `begin{document}`; everything before that line is the preamble. In this preamble we include the title, author names, and date; later, we will see that we can import packages in this preamble section. Let's update the author declaration in the preamble from Stefano Allesina and Madlen Wilmes to just include your name. 

After we begin the document, note that the first thing included is the command `\maketitle`- without this command, the document would not have a title or author section!  After we make the title, we can begin and end as many sections as needed in our document - here, there are just three short sections.  

#### Compiling a tex file to PDF

So far so good, but how to make a pretty pdf?! Let's save and quit gedit and return to the `latex_1` directory in terminal. Assuming that you have TeXLive installed, you can run the following command to compile the `.tex` file into `.pdf`:  

`pdflatex latex-doc-1.tex`

After you run this command, you should see these four documents in your working directory if you run `ls`: 

`latex-doc-1.aux  latex-doc-1.log  latex-doc-1.pdf  latex-doc-1.tex`

Hurray,there's our PDF! Let's open it by running `see latex-doc-1.pdf`.

### Exercise 2: Including figures in your LaTeX documents

Figures are important in science. This exercise explains how to include and refer to figures in a LaTeX document. Navigate into the `latex_2` directory within `exercise-5`. Here, you should see a file `latex-doc-2.tex` and a directory named `figures`. Check that within the `figures` directory, there is a figure called `popeye.png`. 

Use Gedit to open the file `latex-doc-2.tex`. You should see the following document:

<pre>
\documentclass[12pt]{article}
\title{A simple \LaTeX\ document}
\author{Stefano Allesina, Madlen Wilmes}
\date{}
\usepackage{graphicx}
\begin{document}
\maketitle
\begin{abstract}
Here we type a compelling abstract of our paper.
\end{abstract}
\section{Introduction}
Every scientific publication needs an introduction.
\section{Materials \& Methods}
The Hardy-Weinberg equilibrium model constitutes the null model of population genetics. It characterizes the distributions of genotype  frequencies in populations that are not evolving.
\section{Referring to a Figure}
Here is a sentence that references the figure below (Figure \ref{fig:pop})

\begin{figure}
  \label{fig:pop}
\begin{center}
 \includegraphics[width=0.5\linewidth]{figures/popeye.png}
\end{center}
\caption{Here's a caption!}
 \end{figure}

\end{document}
</pre>

Let's take a close look at this document. Notice that we have a slightly modified preamble section- we are now importing the graphics package with the command `\usepackage{graphicx}`. You will always need to import this document to be able to import figures! 

This second critical component in this file is the part that imports in the figure- the lines 

<pre>
\begin{figure}
  \label{fig:pop}
\begin{center}
 \includegraphics[width=0.5\linewidth]{figures/popeye.png}
\end{center}
\caption{Here's a caption!}
 \end{figure}
</pre>

Here, we are telling LaTeX that we want to import a figure, that we want to call the figure "pop", that the figure is saved as `figures/popeye.png` (Note: this is a relative path!), that we want it centered, and that we want it to have the caption "Here's a caption!". 

The third important component is actually referring to the figure in our document. We do that in the sentence `Here is a sentence that references the figure below (Figure \ref{fig:pop})`. Note that we don't call it "Figure 1" explicitly anywhere- we just say "fig:pop".

Again, we can compile this tex file into a pdf using the command `pdflatex latex-doc-2.tex` and check the pdf output using `see latex-doc-2.pdf`. 

### Exercise 3: Formatting text and writing equations 

Navigate into the `latex_3` directory and use gedit to open the file `latex-doc-3.tex`. This document is identical to `latex-doc-1.tex` but includes the following section: 

<pre>
\section{Formatting text and Writing an equation}

\textbf{Here is a bolded sentence}     
\newline
\textit{Here is an italicized sentence}     
\newline
\textit{\textbf{Here is a sentence that is bolded and italicized}}    
\newline
\textsc{Here is a sentence in small caps}   
\newline

Here's a pair of equations from coexistence theory:
\newline
$\frac{dN_{1}}{dt} = r_{1}N_{1} (1 - \alpha_{11}N_{1} - \alpha_{12}N_{2})$
$\frac{dN_{2}}{dt} = r_{2}N_{2} (1 - \alpha_{22}N_{2} - \alpha_{21}N_{1})$
</pre>  

This chunk demonstrates how we can format text to include bolds, italics, etc., and also demonstrates the beauty of LaTeX rendered equations. For more details, please refer to CSB Chapter 7.5- there is a great diversity of ways to format text and typeset math. 

### Exercise 4: Cite yo sources!

Scientific writing requires diligent management of references. It's hard (impossible?) to manually keep track of all of your references in a big paper. Some tools like EndNote or Mendeley are making it easier to manage references in MS Word documents, but LaTeX has solved this problem a while back because LaTeX was designed and developed for scientific writing.  

For every `.tex` file we have that contains the body of our paper, we should have a corresponding `.bib` file that is our bibliography.   

Within the subdirectory `latex_4`, you will find the file `sample_biblio.bib`. This bibliography has just two references: 

<pre>
@article{Hardy1908,
  title={Mendelian proportions in a mixed population},
  author={Hardy, Godfrey},
  year={1908},
  journal={Science},
  volume={28},
  pages={49-50}
}

@book{Weinberg1908,
  title={Uber den Nachweis der Vererbung beim Menschen},
  author={Weinberg, Wilhelm},
  year={1908},
  publisher={Jahreshefte des {V}ereins {V}arterl{\"a}ndische {N}aturkunde in {W}{\"u}rttemberg},
  volume={64},
  page={369--382}
}
</pre>

Let's take a look at how we cite these references in our main document. Open latex-doc-4.tex. Everything in this document is identical to what we saw in `latex-doc-1.tex`, except the following lines:

<pre>
The Hardy-Weinberg equilibrium model constitutes the null model of population genetics. It characterizes the distributions of genotype frequencies in populations that are not evolving (\cite{Hardy1908, Weinberg1908}).

\bibliography{sample_biblio.bib}
\bibliographystyle{plain}
</pre>

Notice that we now have a `\cite` command, and that the items we cite are the names that we gave our references in the bibliography files. Generating a PDF with a bibliography is a little more tricky, since we first need to compile the `.tex` file, then ask BibTex to figure out which of the references in the full bibliography are indeed used, then re-compile a full PDF:

<pre>
pdflatex latex-doc-4.tex
pdflatex latex-doc-4.tex
bibtex sample_biblio.bib
pdflatex latex-doc-4.tex
pdflatex latex-doc-4.tex
</pre>

As always, we can check the output pdf with `see latex-doc-4.pdf`.

-----------

A note on LaTeX: Writing with LaTeX has a very steep learning curve. You will often be frustrated by the fact that even a simple word document that you wish to write keeps throwing error messages. My advice to you (for whatever it's worth) is to stick with it, try to patiently find the causes of errors, scour Stack Overflow for advice, etc. It's worth spending the effort now; later, when you are working on big projects, you will reap the rewards. 

-----------

## Homework- back to Python

### Analysing sequence data in a big file

So far in this class, you have been introduced to various constructs in python that we can use to analyse data. Now, we will put a lot of those constructs together to build a script that will perform a fairly sophisticated and real-world analysis on a real-world data file.

The data file we will be using is the `Marra2014_data.fasta` file that you have already used for several CSB assignments. Recall the structure and contents of `fasta` files- all such files include header lines that begin with a `>` character, and lines in between headings are DNA or protein sequences.  

The task for this week is to write a **python** script that will *import* in this data file from wherever it is currently stored (`CSB/unix/data/`), and *export* a csv-formatted data file to the *current working directory*. The csv file should have two columns:   The first column of the CSV output should be the name of each unique contig; the second column should be the number of times the sequence of nucleotide sequence `AATG` appears in that contig. 

For example, if the following contig were in the data file (it isn't), your output csv should have a row that reads `contig99999, 2`)

```
>contig99999  length=61  numreads=2  gene=isogroup99999  status=it_thresh
ATCCTAGCTACTCTGGAGACTGAAATGTGAAGTTCAAAGTCAGCTCAAGCAAGAGAAATG
```

because the sequence AATG appears twice:
ATCCTAGCTACTCTGGAGACTGA**AATG**TGAAGTTCAAAGTCAGCTCAAGCAAGAGA**AATG**

Submit this as a Python Notebook named `sequence_finding.ipynb` within the `python_exercises` directory in `exercise-5`.

#### Here are some hints:

I have provided you with the output file `gauravs_output.csv`, which is exactly the file that your script should export. 

I recommend that you **begin by writing pseudocode** that sketches out the logic that you plan to follow in your script.  

I recommend that you chunk up your goals into bite-sized pieces. For example, write a code chunk that just counts the AATGs; a separate code chunk that just captures the name of the contig, etc. and then put them all together. 

I recommend that you make a subset of the full data file which includes just a few sequences (maybe 3-5) that you can use to make sure that your program is running correctly. 

These are some lines of code that I used when I wrote a solution to this (not presented here in any order) (NOTE: you don't necessarily need to use these lines- just putting them here for ideas):  

`if (re.match(pattern=">", string=line)):`       
`contig_name = re.search(">(\w*)\s.*", line).group(1)`       
`output.write(key+","+str(value)+"\n")`
    
I had also defined two empty lists - one for contig names and one for the number of AATGs, at the beginning of the script. 

<sup>0</sup>https://github.com/eeb177-W17/eeb177-W17.github.io/raw/master/weekly_content/week5/exercise-5.zip