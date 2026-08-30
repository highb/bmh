# TODO

- **Link checking**: add `lychee` (or similar) as a mise task and CI step.
  Lychee timed out on `cargo:lychee` install; try `npm:lychee` or a prebuilt
  binary via `aqua:lycheeverse/lychee`. Task should run against a `hugo serve`
  base URL. CI should fail on broken links.