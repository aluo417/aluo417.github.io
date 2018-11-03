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

## Math Mode Codes

### Greek Letters

| \\(\alpha\\) \alpha  |  \\(\theta\\) \theta | \\(\tau\\) \tau  | \\(\beta\\) \beta  |
| \\(vartheta\\) \vartheta  |  \\(\pi\\) \pi | \\(\upsilon\\) \upsilon  | \\(\gamma\\) \gamma  |
|  \\(\varpi\\) \varpi |  \\(\phi\\) \phi | \\(\delta\\) \delta  | \\(\kappa\\) \kappa  |
| \\(\rho\\) \rho  | \\(\varphi\\) \varphi  | \\(\epsilon\\) \epsilon  | \\(\lambda\\) \lambda  |
| \\(\varrho\\) \varrho  |\\(\chi\\) \chi   |\\(\varepsilon\\) \varepsilon   | \\(\mu\\) \mu  |
| \\(\sigma\\) \sigma  | \\(\psi\\) \psi  | \\(\zeta\\) \zeta  | \\(\nu\\) \nu  |
|  \\(\varsigma\\) \varsigma | \\(\omega\\) \omega  | \\(\eta\\) \eta  | \\(\xi\\) \xi  |
| \\(\Gamma\\) \Gamma  | \\(\Lambda\\) \Lambda  |  \\(\Sigma\\) \Sigma |  \\(\Psi\\) \Psi |
| \\(\Delta\\) \Delta  | \\(\Xi\\) \Xi  | \\(\Upsilon\\) \Upsilon  | \\(\Omega\\) \Omega  |
| \\(\Theta\\) \Theta  | \\(\Pi\\) \Pi  | \\(\Phi\\) \Phi  |   |

### Binary Operation Symbols

| \\(\pm\\) \pm  | \\(\cap\\) \cap  | \\(\diamond\\) \diamond  | \\(\oplus\\) \oplus  |
| \\(\mp\\) \mp  | \\(\cup\\) \cup  | \\(\bigtriangleup\\) \bigtriangleup  | \\(\ominus\\) \ominus  |
| \\(\times\\) \times  | \\(\uplus\\) \uplus  | \\(\bigtriangledown\\) \bigtriangledown  | \\(\otimes\\) \otimes  |
| \\(\div\\) \div  | \\(\sqcap\\) \sqcap  | \\(\triangleleft\\) \triangleleft  | \\(\oslash\\) \oslash  |
| \\(\ast\\) \ast  | \\(\sqcup\\) \sqcup  | \\(\triangleright\\) \triangleright  | \\(\odot\\) \odot  |
| \\(\star\\) \star  | \\(\vee\\) \vee  | \\(\lhd\\) \lhd  | \\(\bigcirc\\) \bigcirc  |
| \\(\circ\\) \circ  | \\(\wedge\\) \wedge  | \\(\rhd\\) \rhd  | \\(\dagger\\) \dagger  |
| \\(\bullet\\) \bullet  | \\(\setminus\\) \setminus  | \\(\unlhd\\) \unlhd  | \\(\ddagger\\) \ddagger  |
| \\(\cdot\\) \cdot  | \\(\wr\\) \wr  | \\(\unrhd\\) \unrhd  | \\(\amalg\\) \amalg  |

### Relation Symbols

| \\(\leq\\) \leq | \\(\geq\\) \geq | \\(\equiv\\) \equiv | \\(\models\\) \models |
| \\(\prec\\) \prec | \\(\succ\\) \succ | \\(\sim\\) \sim | \\(\perp\\) \perp |
| \\(\preceq\\) \preceq | \\(\succeq\\) \succeq | \\(\simeq\\) \simeq | \\(\mid\\) \mid |
| \\(\ll\\) \ll | \\(\gg\\) \gg | \\(\asymp\\) \asymp | \\(\parallel\\) \parallel |
| \\(\subset\\) \subset | \\(\supset\\) \supset | \\(\approx\\) \approx | \\(\bowtie\\) \bowtie |
| \\(\subseteq\\) \subseteq | \\(\supseteq\\) \supseteq | \\(\cong\\) \cong | \\(\Join\\) \Join |
| \\(\sqsubset\\) \sqsubset | \\(\sqsupseteq\\) \sqsupseteq | \\(\doteq\\) \doteq | \\(\frown\\) \frown |
| \\(\in\\) \in | \\(\ni\\) \ni | \\(\propto\\) \propto | \\(\vdash\\) \vdash |
| \\(\dashv\\) \dashv | \\(\\) | \\(\\) | \\(\\) |

### Punctuation Symbols

| \\(\colon\\) \colon | \\(\ldotp\\) \ldotp | \\(\cdotp\\) \cdotp |


### Arrow Symbols

| \\(\leftarrow\\) \leftarrow | \\(\longleftarrow\\) \longleftarrow | \\(\uparrow\\) \uparrow |
| \\(\Leftarrow\\) \Leftarrow | \\(\Longleftarrow\\) \Longleftarrow | \\(\Uparrow\\) \Uparrow |
| \\(\rightarrow\\) \rightarrow | \\(\longrightarrow\\) \longrightarrow | \\(\downarrow\\) \downarrow |
| \\(\Rightarrow\\) \Rightarrow | \\(\Longrightarrow\\) \Longrightarrow | \\(\Downarrow\\) \Downarrow |
| \\(\leftrightarrow\\) \leftrightarrow | \\(\longleftrightarrow\\) \longleftrightarrow | \\(\updownarrow\\) \updownarrow |
| \\(\Leftrightarrow\\) \Leftrightarrow | \\(\Longleftrightarrow\\) \Longleftrightarrow | \\(\Updownarrow\\) \Updownarrow |
| \\(\mapsto\\) \mapsto | \\(\longmapsto\\) \longmapsto | \\(\\) |
| \\(\hookleftarrow\\) \hookleftarrow | \\(\hookrightarrow\\) \hookrightarrow | \\(\nearrow\\) \nearrow |
| \\(\leftharpoonup\\) \leftharpoonup | \\(\rightharpoonup\\) \rightharpoonup | \\(\searrow\\) \searrow |
| \\(\leftharpoondown\\) \leftharpoondown | \\(\rightharpoondown\\) \rightharpoondown | \\(\swarrow\\) \swarrow |
| \\(\rightleftharpoons\\) \rightleftharpoons | \\(\leadsto\\) \leadsto | \\(\nwarrow\\) \nwarrow |

### Miscellaneous Symbols

| \\(\ldots\\) \ldots | \\(\cdots\\) \cdots | \\(\vdots\\) \vdots | \\(\ddots\\) \ddots |
| \\(\aleph\\) \aleph | \\(\prime\\) \prime | \\(\forall\\) \forall | \\(\infty\\) \infty |
| \\(\hbar\\) \hbar | \\(\emptyset\\) \emptyset | \\(\exists\\) \exists | \\(\Box\\) \Box |
| \\(\imath\\) \imath | \\(\nabla\\) \nabla | \\(\neg\\) \neg | \\(\Diamond\\) \Diamond |
| \\(\jmath\\) \jmath | \\(\surd\\) \surd | \\(\flat\\) \flat | \\(\triangle\\) \triangle |
| \\(\ell\\) \ell | \\(\top\\) \top | \\(\natural\\) \natural | \\(\clubsuit\\) \clubsuit |
| \\(\wp\\) \wp | \\(\bot\\) \bot | \\(\sharp\\) \sharp | \\(\diamondsuit\\) \diamondsuit |
| \\(\Re\\) \Re | \\(\|\\) \| | \\(\backslash\\) \backslash | \\(\heartsuit\\) \heartsuit |
| \\(\Im\\) \Im | \\(\angle\\) \angle | \\(\mho\\) \mho | \\(\spadesuit\\) \spadesuit |
| \\(\\) | \\(\\) | \\(\\) | \\(\\) |
| \\(\\) | \\(\\) | \\(\\) | \\(\\) |



| \\(\\) | \\(\\) | \\(\\) | \\(\\) |
| \\(\\) | \\(\\) | \\(\\) | \\(\\) |
| \\(\\) | \\(\\) | \\(\\) | \\(\\) |
| \\(\\) | \\(\\) | \\(\\) | \\(\\) |
| \\(\\) | \\(\\) | \\(\\) | \\(\\) |


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


