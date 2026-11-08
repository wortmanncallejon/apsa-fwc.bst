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

Out of the box, Zotero (Better BibTeX) exports newspaper articles as `@article` with no day. This style then treats them as forthcoming journal articles ("*Paper*: Forthcoming") and drops the date and URL. To export them as `@newspaper` instead, add this postscript under Zotero → Settings → Better BibTeX → Export → postscript:

```js
if (Translator.BetterBibTeX && zotero.itemType === 'newspaperArticle') {
  tex.entrytype = 'newspaper'
  if (tex.date && tex.date.day) tex.add({ name: 'day', value: tex.date.day })
}
```

To treat magazine articles the same way, use `['newspaperArticle', 'magazineArticle'].includes(zotero.itemType)` as the second condition. The BibLaTeX `entrysubtype` field is not read.
