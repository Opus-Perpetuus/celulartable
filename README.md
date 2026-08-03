# Celulartable

> **Deprecated. Superseded by [prettyTables](https://github.com/Opus-Perpetuus/prettyTables).**
>
> This package was an experiment in modelling table cells as objects. The one
> capability it had that prettyTables lacked -- parsing types out of strings --
> now lives there, so there is no longer a reason to maintain it separately.
>
> It was never published to PyPI, so nothing depends on it.

Package inspired in [prettyTables](https://github.com/Opus-Perpetuus/prettyTables)
that uses the concept of cells as a class.

## Migrating

```python
# celulartable
from celulartable import CelularTable
table = CelularTable()

# prettyTables
from prettyTables import Table
table = Table()
```

The feature that motivated this package was type parsing: recognising that the
string `'12.5'` holds a number and aligning it as one. That is now
`parse_str_numbers`:

```python
from prettyTables import Table

table = Table()
table.add_column('price', ['12.50', '3', '245.75'])
table.parse_str_numbers = True   # aligned on the decimal point, not as text
print(table)
```

Setting it re-examines data already added, so the order does not matter.
Identifiers with leading zeros -- `'007'` -- are deliberately left as strings,
since converting them destroys data the table cannot recover.

The readers apply the same parsing automatically, which is the common case:

```python
Table.from_csv('data.csv')       # numeric columns already aligned
```

## What did not carry over, and why

| Piece here | Status in prettyTables |
| --- | --- |
| `type_checking/type_parsers.py` | Ported, as `parse_str_numbers` and the readers |
| `cell/cell_styles.py` | Not needed -- one style here, 42 there |
| `cell/cell.py`, `cell/cell_rows.py` | An alternative architecture, not a missing feature |
| `core/table.py` | Superseded by `prettyTables.table.Table` |

The `Cell`-as-a-class design was the point of the experiment. It reads well,
but prettyTables reaches the same output through column-oriented measurement,
which is what makes decimal-point alignment and the two-pass terminal fitting
straightforward. Porting the cell model would have meant rewriting the parts of
prettyTables that already work, for no behaviour a user could observe.

## Unfinished work, for the record

From `TODO`, none of it done, and none of it a gap in prettyTables:

- Support for empty cells -- prettyTables has `missing_value` and `Table.missing`
- Float alignment in `Cell` -- prettyTables aligns floats on the decimal point
- Border manipulation -- prettyTables ships 42 styles and `TableComposition`
- Alignment indicator -- prettyTables has it, re-added in `81e01e3`
