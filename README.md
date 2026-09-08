# Std

The standard library for Vale: general-purpose rules you extend to build your
own style.

Every rule here comes from a published style -- Google, Microsoft, IBM --
where it had already survived real use. Std carries the machinery with
neutral messages at `suggestion` level; the opinions (levels, wording, doc
links) are yours to add.

```
Std/
├── Abbreviations/   Acronyms, Latin
├── DateTime/        DateFormat, TimeFormat
├── Grammar/         Contractions, PassiveVoice
├── Punctuation/     OxfordComma, Ellipses, Spacing
├── Readability/     SentenceLength
└── Usage/           FirstPersonSingular, FirstPersonPlural,
                     GenderedPronouns, GenderedTerms
```

The directory is the name: `Std/Grammar/PassiveVoice.yml` is
`Std.Grammar.PassiveVoice` everywhere a rule name goes -- alerts, config,
`extends`. New families join the tree without renaming what exists, which is
the stability a parent owes its children.

## Install

> Rule inheritance and nested rules require Vale v3.20.0 or later;
> `meta.json` enforces this at sync time.

```ini
Packages = https://github.com/vale-cli/Std/releases/latest/download/Std.zip

[*.md]
BasedOnStyles = Std
```

```console
$ vale sync
```

Enabled stock, it's a reasonable generic style. Tune it from your config --
levels and toggles as always, scalars with the bracket key:

```ini
Std.Readability.SentenceLength = error
Std.Readability.SentenceLength[max] = 30
Std.Usage.FirstPersonPlural = NO
```

To change what a rule *matches*, extend it in a style of your own:

```yaml
# styles/House/OxfordComma.yml
extends: Std.Punctuation.OxfordComma
message: "Serial comma, always."
level: error
```

Three lines, your name on the alert, Std's machinery underneath. `tokens+`,
`tokens-`, `swap+`, and `swap-` edit a parent's lists and mappings.

## The rules

| Rule | Checks | From |
| ---- | ------ | ---- |
| `Abbreviations.Acronyms` | Acronyms that never get a spelled-out first use | Google |
| `Abbreviations.Latin` | 'e.g.', 'i.e.', 'etc.', 'vs.' with replacements | Google + IBM |
| `DateTime.DateFormat` | Ambiguous numeric and day-first dates | Google |
| `DateTime.TimeFormat` | Consistent time-of-day formatting | Google |
| `Grammar.Contractions` | Contractions over their long forms, with sentence-final guards | Microsoft |
| `Grammar.PassiveVoice` | Passive voice, via the full irregular-participle list | Google |
| `Punctuation.Ellipses` | ASCII and Unicode ellipses | Google + IBM |
| `Punctuation.OxfordComma` | A missing serial comma, with false-positive guards | Google |
| `Punctuation.Spacing` | Missing or doubled space at sentence boundaries | Microsoft |
| `Readability.SentenceLength` | Sentences over 25 words | IBM |
| `Usage.FirstPersonSingular` | 'I', 'me', 'my' and contractions | Microsoft |
| `Usage.FirstPersonPlural` | 'we', 'us', 'ours', 'let's' | Google |
| `Usage.GenderedPronouns` | 'he/she'-style constructions | Google |
| `Usage.GenderedTerms` | Gendered job titles and idioms, with replacements | Google + Microsoft |

A rule enters the library once it exists in two or more published styles, or
has otherwise proven itself. Std is where patterns get promoted, never where
they debut. Each file names its source; [`NOTICE`](NOTICE) has the full
attribution.

## Tests

Every rule is tested, with a case that must trip it and one that must stay
clean -- a Vale rule that matches nothing fails silently, so a rule is only
proven by a fixture that fires it. CI runs the whole set on every push.

## License

MIT. The license and `NOTICE` ship inside the archive.
