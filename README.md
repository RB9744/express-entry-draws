# Express Entry draws (Canada) in CSV and JSON, 2015 to today

Every Express Entry invitation round IRCC has held since the system started on 31 January 2015: draw number, date, program or category, invitations issued and the CRS cut-off score. 454 rounds as of 28 October 2026, including round 91a/91b (the two draws of 30 May 2018).

The numbers come straight from IRCC's own rounds feed (the JSON that feeds the "Rounds of invitations" page on canada.ca). We rebuild the files from that feed, so nothing is typed by hand.

Maintained by the team behind [CRS Calculators](https://crscalculators.com/), where the same data drives the [draw history page](https://crscalculators.com/express-entry-draws/) and the CRS score calculator updated for the November 2026 rules.

## Files

| File | Content |
|---|---|
| `data/express-entry-draws.csv` | one row per round: `draw_number, draw_date, draw_name, invitations, crs_cutoff, tie_break_rule, distribution_as_of` |
| `data/express-entry-draws.json` | same rows as an array of objects |
| `data/ircc_rounds_raw.json` | untouched copy of IRCC's feed at the time of the last update, with the score distribution fields (`dd1` to `dd18`) |

## Quick use

```js
const url = 'https://raw.githubusercontent.com/RB9744/express-entry-draws/main/data/express-entry-draws.json';
const draws = await (await fetch(url)).json();
const cec2026 = draws.filter(d => d.draw_name === 'Canadian Experience Class' && d.draw_date.startsWith('2026'));
```

```python
import pandas as pd
df = pd.read_csv('https://raw.githubusercontent.com/RB9744/express-entry-draws/main/data/express-entry-draws.csv')
df[df.draw_name.str.contains('French')].crs_cutoff.describe()
```

## Notes on the data

- `draw_name` is IRCC's wording. Category names changed over the years ("No Program Specified" is what IRCC calls the old all-program draws; "General" was used in 2024).
- `tie_break_rule` is the date and time IRCC used to break ties at the cut-off score. It is empty for rounds before November 2017, when IRCC did not publish it.
- `invitations` is the number of invitations to apply (ITAs) issued, not the number of people who accepted.
- Round numbers are IRCC's. 91a and 91b are both dated 30 May 2018.
- Score distributions (how many candidates sat in each CRS band on the distribution date) are only in the raw file.

## Updates

We refresh the files after each round. If a round is missing, open an issue.

## License

Data: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Credit "CRS Calculators" with a link to https://crscalculators.com/. The source figures are public information published by Immigration, Refugees and Citizenship Canada.
