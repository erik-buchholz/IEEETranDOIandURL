# IEEETran.bst with DOI and Access Date Support

This extends the default IEEE bibliography styles `IEEETran.bst` and `IEEETranN.bst` by the following features:

1. Access Date in the format `(Accessed 2024-07-29)` added to all `@misc` entries containing the fields `url` and `urldate`.
2. A linked DOI for `@article`, `@inproceedings`, and `@incollection`.
	1. **Note that you have to include `\usepackage{doi}` for the DOIs to be linked.** Otherwise, they will appear as text only.
3. Warning messages if both `doi` and `url` are missing in `@article`, `@inproceedings`, and `@incollection`.
4. Article titles are printed in **title case** rather than **sentence case**.

## Usage

For the standard IEEE conference/journal template or if you are using `\usepackage{cite}` for your citations, use the [IEEEtranDOIandURLwithDate.bst](IEEEtranDOIandURLwithDate.bst). 
However, this file is not compatible with `natbib`. I.e., if you use `\usepackage{natbib}`, use the file [IEEEtranNDOIandURLwithDate.bst](IEEEtranNDOIandURLwithDate.bst) instead. 

## Title Case

We modified this style to print titles in title case rather than sentence case.
I.e., if your `.bib` file contains the title `title = {{{POT}}: Python Optimal Transport},`, it will be printed as `POT: Python Optimal Transport`, even if you don't use curly braces to protect the capitalisation (such as `{{POT}}: {{P}}ython {{O}}ptimal {{T}}ransport`).

If you want to revert to sentence case, you just need to uncomment all occurrences of the following line in the `.bst` file you are using:

```bibtex
% "t" change.case$  % If you want your titles to be in sentence case rather than title case, uncomment this line.
```

## How to Configure Zotero to Work with Title Case

**Goal:** You want your references to be in title-case, i.e., *SoK: Can Trajectory Generation Combine Privacy and Utility?* instead of *SoK: can trajectory generation combine privacy and utility?*.

**Problem:** Zotero expects titles to be stored in sentence-case. So, you will end up with a weird mix of sentence and title-case entries if you are not careful. Moreover, the BetterBibTex export will export all title-case titles in curly brackets, which is not recommended.

### Step 1: Configure Zotero Exports

In order to export correctly, BetterBibTex is configured as follows. This is only necessary if you

1) You use `others` as names in your bibliography to manually enforce `et al.` for entries with many authors.
2) Care about how your BibTeX commands are ordered.

**Postscript Settings:**

```postscript
// Rename Others to others for correct handling
// (The linter will change others to capital letters, which will conflict with BibTeX)
for (const creator of zotero.creators) {
  if (creator.name === 'Others') {
    creator.name = 'others';
  }
}
tex.addCreators();


if (Translator.BetterTeX) {
  // the bib(la)tex fields are ordered according to this array.
  // If a field is not in this list, it will show up after the ordered fields.
  // https://github.com/retorquere/zotero-better-bibtex/issues/512

  const front = ['title', 'booktitle', 'author', 'year', 'month', 'date', 'pages', 'publisher']
  const order = front.filter(field => tex.has[field]).concat(Object.keys(tex.has).filter(field => !front.includes(field)))
  for (const [field, value] of order.map(f => [f, tex.has[f]])) {
    delete tex.has[field]
    tex.has[field] = value
  }
}
```

Activate:
- [x] Apply title-casing to titles
- [x] Apply case-protection to capitalised words by enclosing them in braces

Deactivate for thesis / activate if space for references is limited:
- [ ] Use journal abbreviations

### Step 2: Use Zotero Linter to Update All Titles

- Install Zotero Linter: [Zotero Linter](https://github.com/northword/zotero-format-metadata)
- Select *all* entries
- Right click
- *Linter > Lint and Fix*

Then, all titles should be sentence case. This is correct! We will change them to title title-case on export. It is not recommended to use title-case in Zotero, as this can lead to unexpected behaviour!

### Step 3: Modify Your `.bst` File (Already included in this repository)

After the previous steps, BetterBibTex will export titles in title-case (despite being sentence case in Zotero!), but only wrap acronyms and real names in braces: `title = {{{SoK}}: Can Trajectory Generation Combine Privacy and Utility?},`

Now, it is up to your `.bst` files what to do.
This line: `"t" change.case$` changes your titles to sentence case. So, if you want to use title-case instead, simply comment these lines everywhere and you are done:

Example for title-case:

```bibTex
FUNCTION {format.article.title.electronic}
{ title duplicate$ empty$ 'skip$
    { this.to.prev.status
      this.status.std
      cap.status.std
      % "t" change.case$ # Deactivated case modification
    }
  if$
  "title" bibinfo.check
  duplicate$ empty$ 
    { skip$ } 
    { select.language }
  if$
}
```

Example for sentence case:

```bibTeX
FUNCTION {format.article.title.electronic}
{ title duplicate$ empty$ 'skip$
    { this.to.prev.status
      this.status.std
      cap.status.std
      "t" change.case$ # Not commented out
    }
  if$
  "title" bibinfo.check
  duplicate$ empty$ 
    { skip$ } 
    { select.language }
  if$
}
```

### Step 4: Enjoy without Manually Editing All Titles :)



## References & Acknowledgements

1. DOI Support: Gist by ezod - https://gist.github.com/ezod/3373556
1. URL Acces Date: By Ian Ooi: https://gist.github.com/philmtd/f75f4071779ae053d86e04db525466f7
