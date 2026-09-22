# MSC Agreements

MSC Agreements shows the **Agreements tab** of the shared MSC workbook (*ANS-Finance → Managed Service Clients → Managed Service Client MSC*) inside the Hub, and lets you edit each client's agreement terms without opening Excel. The TC and S+ increase percentages here are the same ones **Contract Renewals** shows next to each expiring contract.

---

## What's shown

| Column | Where it comes from |
|---|---|
| **Company** | The Revenue tab (calculated) |
| **Users** | The Revenue tab (calculated) |
| **Monthly MSA** | The Revenue tab (calculated) |
| **Month** | Typed in on the Agreements tab: the month the agreement renews |
| **Year Signed** | Typed in: the year the client last signed |
| **TC Increase** | Typed in: the annual TotalCommITment increase written into the agreement |
| **S+ Increase** | Typed in: the annual Security+ increase |
| **Industry** | Typed in |
| **Lifetime Value** | The Revenue tab (calculated) |

The cards along the top total the clients, the monthly MSA, the average MSA per client, and how many clients have an increase rate on file.

---

## Editing

1. Click a **Month**, **Year Signed**, **TC Increase**, **S+ Increase** or **Industry** cell and type the new value. Increases are entered as a percentage, e.g. `5` for 5%.
2. Press **Enter** to keep the change or **Esc** to cancel it. Edited cells turn amber until you save.
3. Click **Save to MSC Sheet**. The changes are written straight into the shared workbook, and the table reloads from the sheet so you see exactly what landed.

**Greyed-out columns can't be edited here.** Company, Users, Monthly MSA and Lifetime Value are formulas that pull from the Revenue tab. Change those on the Revenue tab itself; **Open MSC Sheet** takes you there.

### When a change isn't saved

The Hub checks every cell before writing it, and tells you which changes it skipped and why:

- **Changed in the sheet since you loaded it.** Someone edited that cell in Excel while you had the Hub open. Nothing is overwritten; reload, check their value, and edit again if needed.
- **The cell is a formula.** It stays a formula; change it in the sheet.
- **Client not found / appears twice.** The client was renamed, removed, or duplicated on the tab. Fix the row in the sheet.
- **The rate must be between 0% and 100%.**

---

## Access

The Hub reads and writes the workbook **as you**, so you can see and edit exactly what your own SharePoint access to ANS-Finance allows, no more. If you can't open the workbook in SharePoint, this tool will show an error saying so.

---

## Troubleshooting

**"Couldn't load the Agreements tab"**: check you can open the MSC workbook in SharePoint. If the message says a column is missing, the tab's header row was renamed; the Hub finds columns by their header text (Company, User Support, MSA Total, Month, Year Signed, TC Increase, S+ Increase, Industry, Lifetime Value).

**A client is missing**: the list is the Agreements tab itself. Add the client there (usually by adding them to the Revenue tab, which feeds it), then click **Reload**.

**Loading is slow**: the first load can take up to a minute on some networks; the status line shows while it works.

---

*Imagined by: Mike Stewart*
