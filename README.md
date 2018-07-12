---
title: "Publication-ready volcano plots with enhanced colouring and labeling"
author: "Kevin Blighe"
date: "`r Sys.Date()`"
package: "`r packageVersion('EnhancedVolcano')`"
output:
  rmarkdown::html_document:
    highlight: pygments
    toc: true
    fig_width: 5
bibliography: library.bib
vignette: >
  %\VignetteIndexEntry{Publication-ready volcano plots with enhanced colouring and labeling}
  %\VignetteEngine{knitr::rmarkdown}
---

Volcano plots represent a useful way to visualise the results of differential expression analyses. Here, we present a highly-configurable function that produces publication-ready volcano plots [@EnhancedVolcano].


```{r, echo=FALSE}
library(knitr)
opts_chunk$set(tidy=TRUE,message=FALSE)
```

## Example volcano plots

Following data generated by tutorial of [RNA-seq workflow: gene-level exploratory analysis and differential expression](http://master.bioconductor.org/packages/release/workflows/vignettes/rnaseqGene/inst/doc/rnaseqGene.html):


Load airway data:

```{r}

	source("https://bioconductor.org/biocLite.R")
	biocLite("airway")
	library(airway)

	library(magrittr)

	data("airway")
	airway$dex %<>% relevel("untrt")

```

Conduct differential expression using DESeq2:

```{r}

	library("DESeq2")
	dds <- DESeqDataSet(airway, design = ~ cell + dex)
	dds <- DESeq(dds, betaPrior=FALSE)

	res1 <- results(dds, contrast = c("dex","trt","untrt"))
	res1 <- lfcShrink(dds, contrast = c("dex","trt","untrt"), res=res1)

	res2 <- results(dds, contrast = c("cell", "N061011", "N61311"))
	res2 <- lfcShrink(dds, contrast = c("cell", "N061011", "N61311"), res=res2)

	res3 <- results(dds, contrast = c("cell", "N061011", "N052611"))
	res3 <- lfcShrink(dds, contrast = c("cell", "N061011", "N052611"), res=res3)

	res4 <- results(dds, contrast = c("cell", "N061011", "N052611"))
	res4 <- lfcShrink(dds, contrast = c("cell", "N061011", "N052611"), res=res4)

```

Install and load EnhancedVolcano:

```{r}

	biocLite("EnhancedVolcano")
	library(EnhancedVolcano)

```

## Example 1: plot the most basic volcano plot:

```{r}

	EnhancedVolcano(res1,
		lab = rownames(res1),
		x = "log2FoldChange",
		y = "pvalue")

```

```{r plot, fig.cap = "Example 1: plot the most basic volcano plot", echo=FALSE}
	EnhancedVolcano(res1, lab = rownames(res1), x = "log2FoldChange", y = "pvalue")
```

## Example 2: modify cut-offs for log2FC and pvalue; add title; adjust point and label size:

```{r}

	EnhancedVolcano(res2,
		lab = rownames(res2),
		x = "log2FoldChange",
		y = "pvalue",
		pCutoff = 10e-12,
		FCcutoff = 1.5,
		transcriptPointSize = 1.5,
		transcriptLabSize = 3.0,
		title = "N061011 versus N61311")

```

```{r plot, fig.cap = "Example 2: modify cut-offs for log2FC and pvalue; add title; adjust point and label size", echo=FALSE}
	EnhancedVolcano(res2, lab = rownames(res2), x = "log2FoldChange", y = "pvalue", pCutoff = 10e-12, FCcutoff = 1.5, transcriptPointSize = 1.5, transcriptLabSize = 3.0, title = "N061011 versus N61311")
```

## Example 3: adjust colour and alpha for point shading:

```{r}

	EnhancedVolcano(res2,
		lab = rownames(res2),
		x = "log2FoldChange",
		y = "pvalue",
		pCutoff = 10e-12,
		FCcutoff = 1.5,
		transcriptPointSize = 1.5,
		transcriptLabSize = 3.0,
		title = "N061011 versus N61311",
		col=c("black", "black", "black", "red3"),
		colAlpha = 1)

```

```{r plot, fig.cap = "Example 3: adjust colour and alpha for point shading", echo=FALSE}
	EnhancedVolcano(res2, lab = rownames(res2), x = "log2FoldChange", y = "pvalue", pCutoff = 10e-12, FCcutoff = 1.5, transcriptPointSize = 1.5, transcriptLabSize = 3.0, title = "N061011 versus N61311", col=c("black", "black", "black", "red3"), colAlpha = 1)
```

## Example 4: adjust axis limits:

```{r}

	EnhancedVolcano(res2,
		lab = rownames(res2),
		x = "log2FoldChange",
		y = "pvalue",
		pCutoff = 10e-12,
		FCcutoff = 1.5,
		transcriptPointSize = 1.5,
		transcriptLabSize = 3.0,
		title = "N061011 versus N61311",
		colAlpha = 1,
		xlim = c(-8, 8),
		ylim = c(0, -log10(10e-32)))

```

```{r plot, fig.cap = "Example 4: adjust axis limits", echo=FALSE}
	EnhancedVolcano(res2, lab = rownames(res2), x = "log2FoldChange", y = "pvalue", pCutoff = 10e-12, FCcutoff = 1.5, transcriptPointSize = 1.5, transcriptLabSize = 3.0, title = "N061011 versus N61311", colAlpha = 1, xlim = c(-8, 8), ylim = c(0, -log10(10e-32)))
```

## Example 5: adjust cut-off lines:

```{r}

	EnhancedVolcano(res2,
		lab = rownames(res2),
		x = "log2FoldChange",
		y = "pvalue",
		pCutoff = 10e-12,
		FCcutoff = 1.5,
		transcriptPointSize = 1.5,
		transcriptLabSize = 3.0,
		title = "N061011 versus N61311",
		colAlpha = 1,
		xlim = c(-8, 8),
		ylim = c(0, -log10(10e-32)),
		cutoffLineType = "twodash",
		cutoffLineCol = "red3",
		cutoffLineWidth = 1.5)

```

```{r plot, fig.cap = "Example 5: adjust cut-off lines", echo=FALSE}
	EnhancedVolcano(res2, lab = rownames(res2), x = "log2FoldChange", y = "pvalue", pCutoff = 10e-12, FCcutoff = 1.5, transcriptPointSize = 1.5, transcriptLabSize = 3.0, title = "N061011 versus N61311", colAlpha = 1, xlim = c(-8, 8), ylim = c(0, -log10(10e-32)), cutoffLineType = "twodash", cutoffLineCol = "red3", cutoffLineWidth = 1.5)
```

## Example 6: adjust legend position, size, and text:

```{r}

	EnhancedVolcano(res2,
		lab = rownames(res2),
		x = "log2FoldChange",
		y = "pvalue",
		pCutoff = 10e-12,
		FCcutoff = 1.5,
		transcriptPointSize = 1.5,
		transcriptLabSize = 3.0,
		colAlpha = 1,
		cutoffLineType = "twodash",
		cutoffLineCol = "red4",
		cutoffLineWidth = 1.0,
		legend=c("NS","Log (base 2) fold-change","P value","P value & Log (base 2) fold-change"),
		legendPosition = "right",
		legendLabSize = 14,
		legendIconSize = 5.0)

```

```{r plot, fig.cap = "Example 6: adjust legend position, size, and text", echo=FALSE}
	EnhancedVolcano(res2, lab = rownames(res2), x = "log2FoldChange", y = "pvalue", pCutoff = 10e-12, FCcutoff = 1.5, transcriptPointSize = 1.5, transcriptLabSize = 3.0, colAlpha = 1, cutoffLineType = "twodash", cutoffLineCol = "red4", cutoffLineWidth = 1.0, legend=c("NS","Log (base 2) fold-change","P value","P value & Log (base 2) fold-change"), legendPosition = "right", legendLabSize = 14, legendIconSize = 5.0)
```

## Example 7: plot adjusted p-values:

```{r}

	EnhancedVolcano(res2,
		lab = rownames(res2),
		x = "log2FoldChange",
		y = "padj",
		xlab = bquote(~Log[2]~ "fold change"),
		ylab = bquote(~-Log[10]~adjusted~italic(P)),
		pCutoff = 0.0001,
		FCcutoff = 1.0,
		xlim=c(-6,6),
		transcriptLabSize = 3.0,
		colAlpha = 1,
		legend=c("NS","Log2 FC","Adjusted p-value","Adjusted p-value & Log2 FC"),
		legendPosition = "bottom",
		legendLabSize = 10,
		legendIconSize = 3.0)

```

```{r plot, fig.cap = "Example 7: plot adjusted p-values", echo=FALSE}
	EnhancedVolcano(res2, lab = rownames(res2), x = "log2FoldChange", y = "padj", xlab = bquote(~Log[2]~ "fold change"), ylab = bquote(~-Log[10]~adjusted~italic(P)), pCutoff = 0.0001, FCcutoff = 1.0, xlim=c(-6,6), transcriptLabSize = 3.0, colAlpha = 1, legend=c("NS","Log2 FC","Adjusted p-value","Adjusted p-value & Log2 FC"), legendPosition = "bottom", legendLabSize = 10, legendIconSize = 3.0)
```

## Example 8: fit more labels by adding connectors:

```{r}

	EnhancedVolcano(res2,
		lab = rownames(res2),
		x = "log2FoldChange",
		y = "padj",
		xlab = bquote(~Log[2]~ "fold change"),
		ylab = bquote(~-Log[10]~adjusted~italic(P)),
		pCutoff = 0.0001,
		FCcutoff = 2.0,
		xlim = c(-6,6),
		transcriptLabSize = 3.0,
		colAlpha = 1,
		legend=c("NS","Log2 FC","Adjusted p-value","Adjusted p-value & Log2 FC"),
		legendPosition = "bottom",
		legendLabSize = 10,
		legendIconSize = 3.0,
		DrawConnectors = TRUE,
		widthConnectors = 0.5,
		colConnectors = "black")

```

```{r plot, fig.cap = "Example 8: fit more labels by adding connectors", echo=FALSE}
	EnhancedVolcano(res2, lab = rownames(res2), x = "log2FoldChange", y = "padj", xlab = bquote(~Log[2]~ "fold change"), ylab = bquote(~-Log[10]~adjusted~italic(P)), pCutoff = 0.0001, FCcutoff = 2.0, xlim = c(-6,6), transcriptLabSize = 3.0, colAlpha = 1, legend=c("NS","Log2 FC","Adjusted p-value","Adjusted p-value & Log2 FC"), legendPosition = "bottom", legendLabSize = 10, legendIconSize = 3.0, DrawConnectors = TRUE, widthConnectors = 0.5, colConnectors = "black")
```

## Example 9: only label key transcripts:

```{r}

	EnhancedVolcano(res2,
		lab = rownames(res2),
		x = "log2FoldChange",
		y = "padj",
		selectLab = c("ENSG00000106565","ENSG00000187758"),
		xlab = bquote(~Log[2]~ "fold change"),
		ylab = bquote(~-Log[10]~adjusted~italic(P)),
		pCutoff = 0.0001,
		FCcutoff = 2.0,
		xlim = c(-6,6),
		transcriptLabSize = 5.0,
		colAlpha = 1,
		legend=c("NS","Log2 FC","Adjusted p-value","Adjusted p-value & Log2 FC"),
		legendPosition = "bottom",
		legendLabSize = 10,
		legendIconSize = 3.0)

```

```{r plot, fig.cap = "Example 9: only label key transcripts", echo=FALSE}
	EnhancedVolcano(res2, lab = rownames(res2), x = "log2FoldChange", y = "padj", selectLab = c("ENSG00000106565","ENSG00000187758"), xlab = bquote(~Log[2]~ "fold change"), ylab = bquote(~-Log[10]~adjusted~italic(P)), pCutoff = 0.0001, FCcutoff = 2.0, xlim = c(-6,6), transcriptLabSize = 5.0, colAlpha = 1, legend=c("NS","Log2 FC","Adjusted p-value","Adjusted p-value & Log2 FC"), legendPosition = "bottom", legendLabSize = 10, legendIconSize = 3.0)
```

## Example 10: plot multiple volcanos on the same page:

```{r}

	p1 <- EnhancedVolcano(res1,
		lab = rownames(res1),
		x = "log2FoldChange",
		y = "pvalue",
		pCutoff = 10e-24,
		FCcutoff = 2.0,
		transcriptLabSize = 2.0,
		colAlpha = 1,
		legendPosition = "bottom",
		legendLabSize = 10,
		legendIconSize = 3.0)

	p2 <- EnhancedVolcano(res1,
		lab = rownames(res1),
		x = "log2FoldChange",
		y = "padj",
		selectLab = c("ENSG00000106565","ENSG00000187758"),
		xlab = bquote(~Log[2]~ "fold change"),
		ylab = bquote(~-Log[10]~adjusted~italic(P)),
		pCutoff = 0.0001,
		FCcutoff = 2.0,
		xlim = c(-6,6),
		transcriptLabSize = 5.0,
		colAlpha = 1,
		legend=c("NS","Log2 FC","Adjusted p-value","Adjusted p-value & Log2 FC"),
		legendPosition = "bottom",
		legendLabSize = 10,
		legendIconSize = 3.0)

	p3 <- EnhancedVolcano(res3,
		lab = rownames(res3),
		x = "log2FoldChange",
		y = "pvalue",
		title = "N061011 versus N052611",
		DrawConnectors = FALSE)

	p4 <- EnhancedVolcano(res4,
		lab = rownames(res4),
		x = "log2FoldChange",
		y = "padj",
		ylab = bquote(~-Log[10]~adjusted~italic(P)),
		title = "N061011 versus N61311",
		col = c("black", "dodgerblue", "skyblue", "gold"),
		DrawConnectors = FALSE)

	library(gridExtra)
 	library(grid)
	grid.arrange(p1, p2, p3, p4, ncol=2, top="EnhancedVolcano")
	grid.rect(gp=gpar(fill=NA))


```

```{r plot, fig.cap = "Example 10: plot multiple volcanos on the same page", echo=FALSE}
	p1 <- EnhancedVolcano(res1,
		lab = rownames(res1),
		x = "log2FoldChange",
		y = "pvalue",
		pCutoff = 10e-24,
		FCcutoff = 2.0,
		transcriptLabSize = 2.0,
		colAlpha = 1,
		legendPosition = "bottom",
		legendLabSize = 10,
		legendIconSize = 3.0)

	p2 <- EnhancedVolcano(res1,
		lab = rownames(res1),
		x = "log2FoldChange",
		y = "padj",
		selectLab = c("ENSG00000106565","ENSG00000187758"),
		xlab = bquote(~Log[2]~ "fold change"),
		ylab = bquote(~-Log[10]~adjusted~italic(P)),
		pCutoff = 0.0001,
		FCcutoff = 2.0,
		xlim = c(-6,6),
		transcriptLabSize = 5.0,
		colAlpha = 1,
		legend=c("NS","Log2 FC","Adjusted p-value","Adjusted p-value & Log2 FC"),
		legendPosition = "bottom",
		legendLabSize = 10,
		legendIconSize = 3.0)

	p3 <- EnhancedVolcano(res3,
		lab = rownames(res3),
		x = "log2FoldChange",
		y = "pvalue",
		title = "N061011 versus N052611",
		DrawConnectors = FALSE)

	p4 <- EnhancedVolcano(res4,
		lab = rownames(res4),
		x = "log2FoldChange",
		y = "padj",
		ylab = bquote(~-Log[10]~adjusted~italic(P)),
		title = "N061011 versus N61311",
		col = c("black", "dodgerblue", "skyblue", "gold"),
		DrawConnectors = FALSE)

	library(gridExtra)
 	library(grid)
	grid.arrange(p1, p2, p3, p4, ncol=2, top="EnhancedVolcano")
	grid.rect(gp=gpar(fill=NA))

```


## Acknowledgments

The development of *EnhancedVolcano* has benefited from contributions and
suggestions from:

Sharmila Rana,
[Myles Lewis](https://www.qmul.ac.uk/whri/people/academic-staff/items/lewismyles.html),

## Session info

```{r}
sessionInfo()
```
 
## References






