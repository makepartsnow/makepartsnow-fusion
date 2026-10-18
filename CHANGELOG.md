# Changelog

## 1.0.2 (18 Oct 2026): on the shared Make Parts Now CAD core, submitted to the Autodesk App Store in place of 1.0.1

Nothing changes for users: the palette, its text, the quote, Order and the files sent are the same as 1.0.1, whose fixes are all kept.

- Everything that doesn't touch Fusion now comes from the Make Parts Now CAD core 0.1.3 (`makepartsnow-cad-core` `7aff490`), the code the FreeCAD workbench uses too: the `/mcp/cad` client, background jobs, the quote, revise, checkout and delete flows, the units check, settings, consent text, usage counts, the view model and the palette's state machine. It is vendored at `makepartsnow_fusion/core/` and pinned by `CORE_PIN.json`; CI checks that the copy is unedited.
- What stays Fusion's: the toolbar, palette and events (`palette_ui`), the palette's message protocol (`palette_bridge`, `controller`), selection, STEP and STL export, and saving the quote as a component attribute.
- The add-in keeps its own words through the core's `Host.wording` ("body", "palette", "Click components or bodies").

## 1.0.1 (17 Oct 2026): fixes from the owner's Windows test of 1.0.0, resubmitted to the Autodesk App Store

- **Fixed: the palette stopped responding after the add-in was stopped and run again in one Fusion session.** Stop took each palette handler off all three palette events; taking one off an event it was never on can fail in Fusion, and the palette was then never deleted. Run again found that old palette, with no handlers, and showed it: a stale screen where no click reached the add-in (owner's test on Windows, 17 Oct 2026). Stop now takes each handler off only its own event, each on its own, and always deletes the palette. Opening the palette also replaces one this run didn't create, so a palette left over by 1.0.0 is fixed too.
- **Checkout opens only Make Parts Now links.** Order opens a checkout link in the browser only when it is https on makepartsnow.com (or the development server the add-in is set to), the same rule as the palette's other links. 1.0.0 opened any https link, and http links starting with "localhost", such as http://localhost.example.com.
- **Design files upload only to Make Parts Now.** An upload link that isn't https on makepartsnow.com (or the development server the add-in is set to) is refused before any file is sent.
- The refusal for a component with sub-components says "1 sub-component" or "2 sub-components", not "sub-component(s)".
- The quote summary's parts line counts units: "Parts (4 units)" for two parts ordered twice each.

## 1.0.0 (15 Oct 2026): first release, submitted to the Autodesk App Store

The first public release, made from the builds the owner tested in Fusion 2705.1.30 on Windows 11 (0.1.0 to 0.2.1 below), including a real paid order placed from the add-in with the first-order code.

- A branded Make Parts Now palette docked beside the model: click parts in the design (up to 25), choose SLS PA12 Nylon (Gray or Black Dyed, the default) or an FDM plastic and a colour per part, set quantities and a US ZIP, and get an instant quote.
- The quote shows each part's price, a printability checklist (units match Fusion, fits the build volume, one solid body, server warnings such as thin walls), shipping, the total before tax, ship and arrival dates, and the first-order promotion code. **Order** opens checkout on makepartsnow.com.
- **Save quote to design** (one undo step) and **Delete my uploaded files**.
- No account needed, no order minimum. Consent once before anything is uploaded; anonymous usage counts are off unless the user opts in.
- A **MAKE PARTS NOW** panel on the UTILITIES tab, and the button under UTILITIES > ADD-INS.
- Before a STEP file is uploaded, the author, organization, authorization and file path that Fusion writes into its header are removed, so a name, company or Windows user name isn't sent. Geometry is untouched.
- Support: nathan@makepartsnow.com. Help: https://makepartsnow.com/fusion.

## 0.2.1 (owner's test build)

- **Fixed: a quote stopped after "Exporting 1 of 1".** Each step after the first waits for Fusion to be idle, through a custom event. 0.2.0 fired that event on Fusion's main thread, from inside the same event's handler, and Fusion didn't deliver it, so the export never moved on (owner's test, 15 Oct 2026). The event is now fired from a short-lived thread, Autodesk's documented pattern. As a backstop, while a quote runs the palette checks in every 2 seconds, and any step whose event is more than 1.5 seconds overdue runs then. A quote can no longer stall on a lost event.

## 0.2.0 (owner's test build)

A branded palette replaces the native dialog (15 Oct 2026).

- **Branded palette.** "Make Parts Now" opens as an HTML palette docked on the right, about 380 px wide. It has the makepartsnow wordmark, a brand-red rule, a three-step bar (1 Parts, 2 Quote, 3 Order) and the main action of each step in a footer that stays put. Everything in it is local: no CDN, no web fonts, no remote images, nothing fetched by the page. Links open in your browser, never in the palette.
- **Its own toolbar panel.** A **MAKE PARTS NOW** panel on the UTILITIES tab holds a large **Get Instant Quote** button. The button is also in UTILITIES > ADD-INS; it's no longer in SOLID > MAKE. The button icon is now the Make Parts Now "M".
- **Click to add parts.** While the palette is open, clicking components, bodies or faces in your design adds them, up to 25. **Add selected** does the same for the current selection. Each part card shows its size in Fusion and has a material picker and a quantity stepper, plus finish or colour swatches for that material. Refusals (several bodies, sub-components, more than 25 parts) appear inline.
- **SLS nylon and FDM plastics.** Each part can be PA12 Nylon (SLS) in Gray or Black Dyed, or an FDM plastic (PLA, PETG, ABS, ASA, TPU 95A, PA-CF, PETG-CF) in the colours the server lists. New parts always start as PA12 Gray. The build-volume pre-check and the checklist use the chosen material's build volume. The material is sent per quote item; PA12 is sent as no material, so older servers still work.
- **Printability checklist.** Each quoted part gets ✓/⚠/✗ rows: units match Fusion (✗ blocks ordering), fits the build volume, one solid body, and each server warning such as thin walls.
- **Quote summary.** The summary shows parts, shipping (or Free shipping) and the total before tax in large type, with "Ships by" and "Arrives by" dates. A first-order promotion banner has a Copy button.
- **Order, save, edit, delete.** **Order** opens checkout in your browser and moves to step 3. **Save quote to design** writes the quote through a hidden command, so it is one undoable step. **Edit parts** goes back without uploading again. **Delete my uploaded files** asks for confirmation inside the palette.
- **Friendly states.** Full-panel states cover paused quoting, no connection, too many requests and "Update the add-in". A failed STEP export asks inside the palette before sending STL. A part that needs its units confirmed gets a Millimeters/Inches choice.
- **New consent text** (consent version 2), which also names the material: everyone accepts once more.
- **Development.** `palette/index.html?mock=1&screen=<name>` previews every screen in a browser with the owner's test data. `tools/render_palette.py` renders them to PNG with headless Chrome. `tools/make_brand_assets.py` makes the logo and button icons from the website's brand files, standard library only.

## 0.1.1 (owner's retest build)

Fixes from the owner's first test in Fusion.

- OK no longer looks like the way to get a quote. It is labelled **Save quote to part** (**parts** for several) and stays disabled until a current, ready quote is showing; clicking it saves the quote to the parts. The "Save quote to these parts when I click OK" checkbox is gone. **Close** still closes without saving.
- **Get quote**, **Order** and **Delete my uploaded files** show their names on the buttons, not just an icon. Get quote sits right after the ZIP field.
- The material (PA12 Nylon (Formlabs Nylon 12), Standard speed) has its own full-width row, so a narrow dialog doesn't cut it off.
- The quote says "before tax" once: "No order minimum. Prices are in USD before sales tax; tax is added at checkout."
- The first-run consent text box is taller, so it needs little scrolling.

## 0.1.0 (owner's first test build)

First beta, for the owner's Windows test.

- The quote dialog: select 1 to 25 components (one solid body each), set a quantity and finish (Gray or Black Dyed) per part, enter a ZIP code, get a quote. Material: PA12 Nylon (Formlabs Nylon 12), Standard speed, prices before tax, no order minimum.
- STEP export per part, with STL (millimeters) offered if STEP fails. A local build-volume check (165 x 165 x 300 mm) runs before upload.
- A units check per part (max(0.1 mm, 0.5%)); a mismatch blocks Order.
- Order opens Stripe Checkout on makepartsnow.com in the browser, and the dialog shows the first-order promotion code.
- Delete my uploaded files, and an opt-in "Save quote to these parts".
- One-time consent, and opt-in anonymous usage counts.
- A spike script that checks export, units, attributes, HTTPS and threads in the real Fusion.
- Packaging: a manual-install zip and an Autodesk App Store `.bundle`.
- Matches the finished `/mcp/cad` server: there is no quote (no id, totals or dates) until every part is ready; a failed part makes the answer `needs_input`, its row is marked with the server's reason, and the next Get quote reuses the parts that were fine; delete tokens keep the first one seen; checkout opens the browser only when its status is `ready`, and a revised quote (QUOTE_REVISED) shows the newest prices first.
