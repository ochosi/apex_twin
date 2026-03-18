# Wikidata to AMIE

A single-page client-side app that searches Wikidata for historical persons and generates a link to the AMIE service (backed by Oracle APEX) with a pre-populated form.

## Fields

The following properties are fetched from Wikidata for each person:

| Field           | Wikidata Property |
|-----------------|-------------------|
| Noble title     | P97               |
| Family name     | P734              |
| Given name      | P735              |
| Sex or gender   | P21               |
| Date of birth   | P569              |
| Date of death   | P570              |
| Place of birth  | P19               |
| Place of death  | P20               |

## How it works

1. **Search** — Type a name into the search field. The app queries the Wikidata `wbsearchentities` API and presents matching items in a dropdown.
2. **Fetch details** — Select a person from the dropdown. A SPARQL query retrieves all 8 properties in a single request.
3. **Generate link** — The fetched values are assembled into an Oracle APEX `f?p=` URL that opens AMIE with the form fields pre-filled.

No server-side code is required; everything runs in the browser.

## Running locally

Open `index.html` directly in a browser, or serve it with any static file server:

```sh
python3 -m http.server 8765
# then visit http://localhost:8765
```

## Configuration

The APEX connection details are defined as constants at the top of the `<script>` block in `index.html`:

```js
const APEX_BASE  = 'https://apex.oracle.com/pls/apex/f';
const APEX_APP   = '100';
const APEX_PAGE  = '2';
const APEX_ITEMS = [
  'P2_NOBLE_TITLE', 'P2_FAMILY_NAME', 'P2_GIVEN_NAME', 'P2_SEX',
  'P2_DOB', 'P2_DOD', 'P2_POB', 'P2_POD'
];
```

Replace these with your real AMIE instance URL, application/page IDs, and page item names.
