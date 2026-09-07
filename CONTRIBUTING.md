# Contributing

These pages are the source for docs.flightportrait.com. Fixes
welcome as pull requests.

- Markdown pages; nav in `docs.json`.
- `openapi.json` is generated from the API (same Python as the API tests):
  `python api/export_openapi.py openapi.json` from a checkout of
  flightportrait/network
- Check claims against the code: firmware in
  [flightportrait/frame](https://github.com/flightportrait/frame),
  map and feed in
  [flightportrait/network](https://github.com/flightportrait/network),
  API at
  [data.flightportrait.com/docs](https://data.flightportrait.com/docs).
- Plain sentences, exact numbers, no exclamation marks, no em
  dashes.

Contributions are licensed under Apache-2.0.
