# IIIF Presentation Manifests

IIIF presentation manifests play a key role in the DSE-AS projects in two fields:

* import of images into Transkribus
* contextual presentation of images on the web

Given that the image resources are distributed over a number of archives and libraries, IIIF manifests are created to handle the information in a uniform way.

## Principle 

This repository contains an automated workflow that generates IIIF presentation manifests for documents.

The generation is based on a short declaration containing the image links.

When *pushing* a commit containing one or more `.toml` files to the `main` branch of this repository, manifests are generated and made available automatically.

## Specifics

The document declaration uses a minimal language called [TOML](https://en.wikipedia.org/wiki/TOML).

The files are conventionally saved in the `input` directory and named following the pattern `{type}_nnnnn`, e.g. `shortform_00001` or `letter_00001`. 

The concordance of documents and document identifiers is defined elsewhere.

## TOML template

```
mf_id = "{type}_nnnnn.json"
mf_lang = "en"
mf_label = "{some title/label of document}"

# IIIF image links as a sequence of key/value pairs under `[mf_data]`

[mf_data]
"{mf_id}_nnn" = "https://{iiif-server}/{path}/{identifier}/info.json"
```
Note that double quotes are not allowed within values (use [other quotes](https://docs.annemarie-schwarzenbach.ch/arbeitsschritte/IIIF-manifest-anlegen#erforderliche-angaben-fur-die-input-dateien) instead).

## Contributing manifests

* [Create a new file](https://github.com/pdaengeli/i3f/new/main/input) and fill in the information needed (for more convenience use/modify the template).
* Name the file according to the project convention
* Select "Commit changes…"
  <details><summary>🖼️</summary>
  
  ![image](https://github.com/user-attachments/assets/92beb3b7-9f10-4626-8c3b-82502c9cddef)
  </details>
* Enter extended description (optional), make sure to commit to directly to the `main` branch and click the button "Commit changes"
  <details><summary>🖼️</summary>
  
  ![image](https://github.com/user-attachments/assets/0e8a00a8-c4cc-4e51-aab7-05f9130cdac9)
  </details>
* Wait for the generation of the manifest and check its availability/integrity.

Manifests may be tested using

* https://presentation-validator.iiif.io/
* https://presentation-validator.iiif.io/validate?version=3.0&url= + manifest URL [programmatically]
* https://uv-v4.netlify.app/#?manifest= + manifest URL [visually]
* or other viewers (Mirador is known to be fickly at times)

## Generation workflow ([`generate-manifests.yaml`](https://github.com/dse-as/i3f/blob/main/.github/workflows/generate-manifests.yaml))

All files in the directory `input` are processed in a Jupyter notebook that creates a IIIF manifest for each input file. After processing the files are moved to the directory `input-done` and the manifests moved to `_generated/presentation`. All three directories are then pushed to the repository (`input` (now empty), `input-done`, `_generated/presentation`). The last step is the publication of `_generated/presentation` as a directory listing at https://iiif.annemarie-schwarzenbach.ch/presentation/.

## Development

`ipynb` outputs may be accessed under `debug` (`debug/generate-manifests.html`, `debug/generate-manifests.ipynb`).
