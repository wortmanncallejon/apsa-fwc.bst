# apsa-fwc.bst
BibTeX style file for political science (adapted from apsa.bst, to include URLs for Working Papers).

## CSL version

`apsa-fwc.csl` ports this style to the Citation Style Language for pandoc, Quarto, and Zotero's word-processor plugins. In a Quarto document, point the YAML header to it:

```yaml
csl: apsa-fwc.csl
```

The CSL version follows the BibTeX style, with these differences:

- Citations print `(Smith 2000)`, without natbib's comma.
- Reports and dissertations omit the place of publication, and edition numbers print as ordinals ("5th ed.").
- English titles are set in title case. Set `langid` (Zotero's Language field) on other items, or their titles are title-cased too.
- Thesis, report, and manuscript types print as stored, so pandoc labels a `@phdthesis` "PhD thesis".
- Web pages and blog posts show the site name.
- An `origdate` field prints the original year first: `1835 [2000]`. In Zotero, the style reads the Original Date field directly.

## Installing and updating

TeX looks for personal style files in `TEXMFHOME`, which on macOS is `~/Library/texmf`. Check this with:

```sh
kpsewhich -var-value TEXMFHOME
```

The style file goes in `bibtex/bst/` under that directory. Create it once:

```sh
mkdir -p ~/Library/texmf/bibtex/bst
```

### After each update

Copy the edited file over the installed one (run from this repo):

```sh
cp apsa-fwc.bst ~/Library/texmf/bibtex/bst/
```

### Checking the install

Run `kpsewhich` from **outside** this repo.

```sh
cd ~ && kpsewhich apsa-fwc.bst
# expected: /Users/<you>/Library/texmf/bibtex/bst/apsa-fwc.bst
```

## Newspaper articles

The style has a `@newspaper` entry type. It renders as:

> Author. Year. "Title." *Paper*, Month Day. URL

It uses `journal` for the paper's name, `month` and the new `day` field for the date, and prints `url`. Fields such as `address`, `issn`, and `urldate` are ignored.

Out of the box, Zotero (Better BibTeX) exports newspaper articles as `@article` with no day. This style then treats them as forthcoming journal articles ("*Paper*: Forthcoming") and drops the date and URL. The postscript below exports them as `@newspaper` instead.

To treat magazine articles the same way, use `['newspaperArticle', 'magazineArticle'].includes(zotero.itemType)` in place of the newspaper condition. The BibLaTeX `entrysubtype` field is not read.

## Original publication dates

A reprint or translation can show its original date first, with the edition's year in brackets:

> Tocqueville, Alexis de. 1835 [2000]. *Democracy in America*. Chicago, IL, USA: University of Chicago Press.

Citations read "Tocqueville (1835 [2000])". The style reads a new `origdate` field and uses its first four characters as the year. When two works need a letter to tell them apart, the letter follows the original year ("1971a [1999]").

In Zotero, keep the edition's year in Date and enter the original year in the Original Date field, which books and book sections have. For other item types, add a line to Extra instead:

```
Original Date: 1835
```

Better BibTeX's BibTeX export ignores both, so the postscript below adds the date as `origdate`. Don't write the original year into the Date field itself ("2000 [1835]"): Better BibTeX would export `year = {[1835] 2000}`, which prints the years in the wrong order.

## Better BibTeX postscript

Paste this under Zotero → Settings → Better BibTeX → Export → postscript:

```js
if (Translator.BetterBibTeX) {
  if (zotero.itemType === 'newspaperArticle') {
    tex.entrytype = 'newspaper'
    if (tex.date && tex.date.day) tex.add({ name: 'day', value: tex.date.day })
  }
  const orig = zotero.originalDate || (zotero.extraFields && zotero.extraFields.kv && zotero.extraFields.kv.originalDate)
  if (orig) tex.add({ name: 'origdate', value: String(orig).slice(0, 4) })
}
```

Re-export your `.bib` file after changing the postscript.
