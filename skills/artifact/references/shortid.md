# shortid — tabela de escala

The shortid exists for **one reason: don't clobber**. Two files with the same
date and slug must not collide. The *encoding* (base36, hex, numeric, random,
hash-derived) is cosmetic — pick whatever from the table. The guarantee comes
not from the generator but from the **verify-and-regenerate** rule below.

## The invariant (this is the actual standard)

- **4 characters**, matching the existing suffixes (`k7q2`, `phkj`, `dbiz`…).
- **Charset `[0-9a-z]`** — lowercase alphanumeric. The whole corpus is lowercase;
  keep it that way. Mixed-case generators must be piped through
  `tr 'A-Z' 'a-z'` (or `LC_ALL=C tr -dc 'a-z0-9'`) so a case-insensitive
  filesystem can't collide two ids that differ only in case.
- **Must not already exist.** After minting the full name, run
  `fd <date>-<slug>-<shortid>` (or `fd <date>-<slug>`) in the catalog dir; if
  anything matches, regenerate. This loop — not the generator — is what prevents
  overwrites.

## The table

Any row satisfies the invariant once its charset is lowercased. Two families:
**random** (fast, stateless) and **derived** (id is a function of slug+time, so
the same artifact tends toward the same id).

| # | Command | Family | Charset out | Note |
|---|---------|--------|-------------|------|
| 1 | `LC_ALL=C tr -dc 'a-z0-9' < /dev/urandom \| head -c4` | random | `[0-9a-z]` | **default** — pure shell, already lowercase, no deps |
| 2 | `printf '%s%s' "$slug" "$(date +%s%N)" \| sha256sum \| head -c4` | derived | `[0-9a-f]` hex | pure shell, deterministic-ish; hex only (never `g-z`) |
| 3 | `python3 -c 'import hashlib,sys,time;h=int(hashlib.sha256((sys.argv[1]+str(time.time_ns())).encode()).hexdigest(),16);a="0123456789abcdefghijklmnopqrstuvwxyz";print("".join(a[(h//36**i)%36] for i in range(4)))' "$slug"` | derived | `[0-9a-z]` base36 | full base36 via a big-int hash → the classic minted look (`g-z` possible) |
| 4 | `openssl rand -base64 8 \| tr -dc 'a-z0-9' \| head -c4` | random | `[0-9a-z]` | needs `openssl`; note the `tr` drops uppercase (shrinks entropy, fine at 4 chars) |
| 5 | `LC_ALL=C tr -dc 'A-Za-z0-9' < /dev/urandom \| fold -w4 \| head -n1` | random | mixed → lowercase | append `\| tr 'A-Z' 'a-z'` to honor the charset invariant |

Default is row **1**: no dependency, lowercase by construction, and the
verify-and-regenerate rule makes its randomness collision-proof in practice.
Reach for a **derived** row (2 or 3) only if you want the id reproducible from
the slug; row 3 for the full base36 look.
