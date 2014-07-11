scraperJSON
===========

The scraperJSON standard for defining web scrapers as JSON objects.

### Purpose

scraperJSON is a JSON schema for defining web scrapers in a standardised way. Defining web scrapers in such a way enables mass-scale scraping and mining of similar data from many different sources, for example:

- academic articles (see [journal-scrapers](https://github.com/ContentMine/journal-scrapers))
- news sites
- weather sources
- financial/sports/educational sites

### Status

The specification is still in early drafting and is currently evolving very fast as our understanding of the potential needs of the system in real use develops.

Because of this, the standard is simple described in text here, with a reference implementation in a Node.js library, [thresher](https://github.com/ContentMine/thresher), and a command-line app [quickscrape](https://github.com/ContentMine/quickscrape).

The schema will be formally defined once we reach a stable set of features.

### Specification

The current schema is described below.

There can be two keys in the root object:

- ***url*** - a string-form regular expression specifying which URL(s) this scraper targets
- ***elements*** - a dictionary of elements to scrape

Elements are defined as key-value pairs, where the key is a description of the element, and the value is a dictionary of specifiers defining the element and its processing. Allowed keys in the specifier dictionary are:

- ***selector*** - an XPath selector targetting the element to be selected.
- ***attribute*** - a string specifying the attribute to extract from the selected element. **Optional** (omitting this key is equivalent to giving it a value of `text`). In addition to html attributes there are two special attributes allowed:
    - `text` - extracts any plaintext inside the selected element
    - `html` - extracts the inner HTML of the selected element
- ***download*** - a boolean flag: true if the element is a URL to a resource that must be downloaded. **Optional** (omitting this key is equivalent to giving it a value of `false`).

Example:
```json
{
  "url": "plos.*\\.org",
  "elements": {
    "fulltext_pdf": {
      "selector": "//meta[@name='citation_pdf_url']",
      "attribute": "content",
      "download": true
    },
    "title": {
      "selector": "//meta[@name='citation_title']"
    }
  }
}
```
