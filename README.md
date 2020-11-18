A step-by-step C# implementation of the Docstrum algorithm for pdf documents

### How to run C# code in Jupyter Lab / How Install .NET Interactive
https://devblogs.microsoft.com/cesardelatorre/using-ml-net-in-jupyter-notebooks/

https://devblogs.microsoft.com/dotnet/net-interactive-is-here-net-notebooks-preview-2/ (if the previous fails when installing)

# Description
>**Version 2 is the latest update and handles rotated words/lines/paragraphs. This is the version implemenented in PdfPig.**

![rotated example](https://github.com/BobLd/DocumentLayoutAnalysis/blob/master/DocumentLayoutAnalysis/DocumentLayoutAnalysis/doc/docstrum%20example%202.png)

Link to original paper: [The Document Spectrum for Page Layout Analysis](https://ieeexplore.ieee.org/document/244677) by Lawrence O'Gorman

From [_Performance Comparison of Six Algorithms for Page Segmentation_](https://www.researchgate.net/publication/220932988_Performance_Comparison_of_Six_Algorithms_for_Page_Segmentation): The Docstrum algorithm by O'Gorman is a __bottom-up__ approach based on __nearest-neighborhood clustering__ of connected components extracted from the document image. After noise removal, the connected components are separated into two groups, one with dominant characters and another one with characters in titles and section heading, using a character size ratio factor _fd_. Then, _K_ nearest neighbors are found for each connected component. Then, text-lines are found by computing the transitive closure on within-line nearest neighbor pairings using a threshold _ft_. Finally, text-lines are merged to form text blocks using a parallel distance threshold _fpa_ and a perpendicular distance threshold _fpe_. - [wiki](https://en.wikipedia.org/wiki/Document_layout_analysis#Example_of_a_bottom_up_approach)

## Variables used in structural block determination
The variables can be accessed by using the `GetStructuralBlockingParameters()` function.

```csharp
public static bool GetStructuralBlockingParameters(PdfLine i, PdfLine j, double epsilon,
    out double angularDifference, out double normalisedOverlap, out double perpendicularDistance)
    {
      ...
    }
```
From the original paper by O'Gorman:
>![Fig. 8 - Structural block determination](https://github.com/BobLd/simple-docstrum/blob/master/images/docstrum3.svg)

>**Fig. 8. Variables used in structural block determination.**
The two text lines, represented by segments *i* and *j*, are to be tested here to determine if they should be grouped into the same block. Their angular difference is *θ<sub>ij</sub>*. The overlap length of segment *i* on segment *j* is *p<sub>j</sub>*, (and that is normalized to obtain the overlap parameter). The parallel distance between *i* and *j* is *d<sup>a</sup><sub>ij</sub> = p<sub>j</sub>* in this case. The perpendicular distance betwen *i* and *j* is *d<sup>e</sup><sub>ij</sub>*.

# Results
## 0.0 Open pdf document
![raw](images/raw_v1.png)

## 0.1 Extract words and preprocess
![words](images/words_v1.png)

## 1. Estimate within-line and between-line spacing
### 1.1 Within-line (between words) spacing
![](images/wl_dist_v1.png)
### 1.2 Between-line spacing
![](images/bl_dist_v1.png)

## 2. Get lines
![lines](images/lines_v1.png)

## 3. Get paragraphs blocks
![paragraphs](images/paragraphs_v1.png)
