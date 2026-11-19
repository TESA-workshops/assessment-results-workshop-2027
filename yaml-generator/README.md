# DFO stock assessment output YAML/CSV generator

A one-page form that writes an `assessment.yaml` file for a stock assessment
and lists the CSV columns to match. See [`fsar-data/README.md`](../fsar-data/README.md)
for the data format, and [`dictionary.md`](dictionary.md) for
what each CSV column means.

Open `index.html` in a browser. It needs no server or install, so it can also
be hosted as a static page (e.g. GitHub Pages). It loads
[js-yaml](https://github.com/nodeca/js-yaml) from a CDN, so needs an internet
connection to open an existing file.

**Load existing YAML file** loads a file back into the form to edit. The file is
loaded only if the form can hold all of it exactly, with no unknown keys or
codes and nothing left to fill in. Otherwise the form is left as it was and the
problems are listed. Only the YAML is loaded, never the CSVs: each metric's
`bounds:` and `ratios:` say which columns the CSVs have. The date entered is
reset to today.
