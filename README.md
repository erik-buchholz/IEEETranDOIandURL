# IEEETran.bst with DOI and Access Date Support

This extends the default IEEE bibliography styles `IEEETran.bst` and `IEEETranN.bst` by the following features:

1. Access Date in the format `(Accessed 2024-07-29)` added to all `@misc` entries containing the fields `url` and `urldate`.
2. A linked DOI for `@article`, `@inproceedings`, and `@incollection`.
	1. **Note that you have to include `\usepackage{doi}` for the DOIs to be linked.** Otherwise, they will appear as text only.

## Usage

For the standard IEEE conference/journal template or if you are using `\usepackage{cite}` for your citations, use the [IEEEtranDOIandURLwithDate.bst](IEEEtranDOIandURLwithDate.bst). 
However, this file is not compatible with `natbib`. I.e., if you use `\usepackage{natbib}`, use the file [IEEEtranNDOIandURLwithDate.bst](IEEEtranNDOIandURLwithDate.bst) instead. 

## References & Acknowledgements

1. DOI Support: Gist by ezod - https://gist.github.com/ezod/3373556
1. URL Acces Date: By Ian Ooi: https://gist.github.com/philmtd/f75f4071779ae053d86e04db525466f7
