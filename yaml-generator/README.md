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
codes. Otherwise the form is left as it was and the problems are listed. An
unfinished file, such as a downloaded draft, loads, and what's left to fill in
appears in the to-do list. Only the YAML is loaded, never the CSVs: each metric's
`bounds:` and `ratios:` say which columns the CSVs have. The date entered is
reset to today. A blank value, or a block with only blank values under it, counts
as missing.

**Download YAML template** is for people who would rather edit the YAML in a
text editor. It writes a blank `assessment.yaml` with every key in the order the
form writes them, and comments that give each field's allowed codes and whether
it is required. Every metric is there but commented out: uncomment the ones you
have (in most editors, select the block and press Ctrl+/ or Cmd+/). Inside a
metric, help lines start with `##`, so they stay comments when the block is
uncommented. The comments follow the form's language; keys and codes are always
English. Check the filled-in file with **Load existing YAML file**. The template
is built from the form's own tables, so it is made only by the button and no
copy is kept in the repository.
