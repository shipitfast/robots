### Fixed: `Gr00tPolicy` refuses a `groot_version` that names no release in service mode

Service mode reads `groot_version` to choose the wire shape - `n1.7` adds the
time axis an N1.7 server requires - but the release guard ran only when
`model_path=` was given, so `create_policy("groot", port=5555,
groot_version="N1.7")` was accepted and sent legacy `(B, ...)` tensors to an
N1.7 server; the typo surfaced as a server-side shape error. The guard now runs
in both modes, so a mis-cased or blank `groot_version` is refused at
construction with a `ValueError` naming the parameter and the accepted
spellings.
