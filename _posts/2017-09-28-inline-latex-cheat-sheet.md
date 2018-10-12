---
title: Inline Latex Cheat Sheet
description: Notes for common use of inline Latex in Markdown.
categories:
- notes
tags: Latex, Markdown
---

> Writing markdown notes related to *Finance* topics requires massive use of mathematical formula. Instead of seeking around, I archieve all the useful notes.

## Formatting Commands

### Inline Formatting Commands

These format only a little bit of text at a time. Because they only affect a small amount of text, these commands are a single command that receives the text in curly braces ({ and }) afterwards. Some important examples we will use are below. To show what they do, the actual command is used on the text in curly braces.

- \textbf{$$\textbf{Text to write in bold}$$}
- \textit{$$\textit{Text to write in italics}$$}
- \underline{$$\underline{Text to underline}$$}

### \begin...\end Formatting Commands (Also Called Environments )

When a large amount of text is formatted at one time with a command, it is easier to not have to contain it within brackets. This is why some format- ting commands have two pairs of commands: \begin{format command} and \end{format command}. Think of them as the left and right curly braces of the in-line formatting commands. Anything between a \begin and \end with the same format command will be formatted.

## Math Mode Commands

- \leq is \\(\leq\\)
- \geq is \\(\geq\\)
- \neq is \\(\neq\\)
- \(Greek letter)
	- \Omega is \\(\Omega\\)
	- \sigma is \\(\sigma\\)
	- \phi is \\(\phi\\)
	- \varphi is \\(\varphi\\)
- \in is \\(\in\\)
- \subset is \\(\subset\\)
- \subseteq is \\(\subseteq\\)
- \cup is \\(\cup\\)
- \cap is \\(\cap\\)
- \vee is \\(\vee\\)
- \wedge is \\(\wedge\\)
- \neg is \\(\neg\\)
- \ldots is \\(\ldots\\)
- \cdots is \\(\cdots\\)
- \cdot is \\(\cdot\\)

| Input  | Display  |  Input |  Display |
|:---:|:---:|:---:|:---:|
| \leq  |  \\(\leq\\) |  - \geq |  \\(\geq\\) |
|  - \neq |  \\(\neq\\) |  \Omega | \\(\Omega\\)  |
|  \sigma | \\(\sigma\\) |  \phi | \\(\phi\\)  |
|  \varphi |  \\(\varphi\\) |  \in |  \\(\in\\) |
|  \subset |  \\(\subset\\) | \subseteq |  \\(\subseteq\\) |
| \cup  |  \\(\cup\\) |  \cap | \\(\cap\\)  |
|  \vee | \\(\vee\\)  | \wedge  | \\(\wedge\\)  |
|  \neg |  \\(\neg\\) |  \ldots | \\(\ldots\\)  |
| \cdots  |  \\(\cdots\\) |  \cdot |  \\(\cdot\\) |
|  \infty |  \\(\infty\\) |  \sum |  \\(\sum\\) |
| \prod  | \\(\prod\\)  | \int  | \\(\int\\)  |
| \partial  | \\(\partial\\)  | \mid  |  \\(\mid\\) |
| \leftarrow  | \\(\leftarrow\\)  |  \Leftarrow |  \\(\Leftarrow\\) |
| \;  |  add extra space |  x^2 | \\(x^2\\)  |
|  x_i  | \\(x_i\\) |  \frac{m}{n} | \\(\frac{m}{n}\\)  |


