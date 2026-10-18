# Changelog

## 1.0.2 (18 Oct 2026)

- The Make Parts Now palette keeps working after the add-in is stopped and started again in the same Fusion session.
  Before, it could show an old screen that didn't respond to clicks.
- Order opens checkout only on makepartsnow.com, and design files upload only to makepartsnow.com.
- Clearer wording: "1 sub-component" instead of "sub-component(s)", and the quote summary counts units, such as
  "Parts (4 units)".
- Built on the shared Make Parts Now CAD core, the same code as our FreeCAD workbench. Nothing else changes in what
  you see or what is sent.

## 1.0.0 (15 Oct 2026)

First release.

- A Make Parts Now palette docked beside your model: click up to 25 parts, choose SLS PA12 nylon (Gray or Black Dyed)
  or an FDM plastic and a colour for each, set quantities and a US ZIP code, and get an instant quote.
- Each part's price, a printability checklist (units match Fusion, fits the build volume, one solid body, warnings
  such as thin walls), shipping, the total before tax, ship and arrival dates, and the first-order promotion code.
- **Order** opens checkout on makepartsnow.com. **Save quote to design** (one undo step) and **Delete my uploaded
  files**.
- No account and no order minimum. You accept once before anything is uploaded; anonymous usage counts are off unless
  you opt in.
- Your name, company and file path are removed from the STEP header before upload.
