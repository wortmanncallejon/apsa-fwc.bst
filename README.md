# apsa-fwc.bst
BibTeX style file for political science (adapted from apsa.bst, to include URLs for Working Papers).

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
