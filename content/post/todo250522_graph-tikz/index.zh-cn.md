



以下是使用LaTeX + TikZ绘制上述图形的代码：

```latex
\documentclass[tikz,border=3.14mm]{standalone}
\usetikzlibrary{arrows.meta,positioning,shapes.geometric}
\begin{document}
\begin{tikzpicture}[auto,>={Latex[round]},font=\sffamily,
node distance=1.5cm and 1cm,
block/.style={rectangle, draw, fill=blue!20, text width=5em, text centered, rounded corners, minimum height=4em},
sum/.style={circle, draw, fill=blue!20},
mul/.style={circle, draw, fill=blue!20},
subblock/.style={rectangle, draw, fill=blue!20},
subsum/.style={circle, draw, fill=blue!20},
submul/.style={circle, draw, fill=blue!20}]

% Nodes
\node [subblock] (x) {x};
\node [submul, right=of x] (mul1) {$\times$};
\node [subblock, right=of mul1] (z) {z};
\node [subsum, right=of z] (phi) {$\phi$};
\node [subblock, right=of phi] (h) {h};
\node [submul, right=of h] (mul2) {$\times$};
\node [subblock, right=of mul2] (o) {o};
\node [sum, right=of o] (l) {l};

% Upper left part
\node [block, above=of mul1] (W1) {W$^{(1)}$};
\node [block, above=of mul2] (W2) {W$^{(2)}$};
\node [subsum, above=of phi] (sum1) {$\ell_2$};
\node [block, above=of sum1] (s) {s};
\node [sum, right=of sum1] (sum2) {$+$};
\node [subblock, right=of sum2] (J) {J};

% Upper right part
\node [block, right=of W2] (L) {L};
\node [block, right=of l] (y) {y};


% Arrows
\draw [->] (x) -- (mul1);
\draw [->] (mul1) -- (z);
\draw [->] (z) -- (phi);
\draw [->] (phi) -- (h);
\draw [->] (h) -- (mul2);
\draw [->] (mul2) -- (o);
\draw [->] (o) -- (l);

\draw [->] (W1) -- (mul1);
\draw [->] (z) -- (sum1);
\draw [->] (sum1) -- (W2);
\draw [->] (s) -- (sum1);
\draw [->] (sum1) -- (sum2);
\draw [->] (sum2) -- (J);
\draw [->] (W2) -- (mul2);
\draw [->] (sum2) -- (L);
\draw [->] (L) -- (y);
\draw [->] (y) -- (l);
\end{tikzpicture}
\end{document}
```

这段代码使用LaTeX的TikZ库来绘制流程图，定义了节点类型（如矩形块、圆形求和节点、圆形乘法节点等），并使用箭头连接这些节点。你可以根据需要调整节点的位置、大小、颜色等属性，来满足你的具体需求。