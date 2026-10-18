# Privacy: MakePartsNow add-in for Autodesk® Fusion®

This page describes what the add-in itself sends and stores. The site's [privacy policy](https://makepartsnow.com/legal/privacy) and [terms](https://makepartsnow.com/legal/terms) cover what Make Parts Now does with the data.

## When anything is sent

Nothing is sent until you have:

1. accepted the consent text, which is shown once per install, and
2. clicked **Get instant quote**.

Opening the palette fetches the current options (materials, finishes and colours, limits and any promotion code) from makepartsnow.com. That request carries no part data. Clicking or selecting parts sends nothing. The palette itself is a local page in the add-in's folder: it loads nothing from the internet, and links in it open in your browser.

## What is sent for a quote

- One file per selected part, named after the part: STEP, or STL if STEP export failed and you agreed to send STL. Before a STEP file is sent, the add-in removes the author, organization and file path that Fusion writes into its header, so your name, company and Windows user name aren't sent.
- The quantity, material and finish or colour you chose for each part.
- The ZIP code the parts ship to.
- The add-in's name and version, in the `User-Agent` header.

The add-in sends nothing else from your design: no other components, no design name, no Autodesk account details. You don't sign in. The data goes only to makepartsnow.com; the add-in does not contact Autodesk or anyone else.

## Your files

Uploaded files are stored encrypted. They are deleted automatically under the retention periods in the privacy policy, and files on an order are kept with the order. To delete them sooner:

- click **Delete my uploaded files** in the palette (this covers the files uploaded since Fusion started the add-in), or
- email nathan@makepartsnow.com.

## Paying

The add-in is free. Parts are bought and paid for on makepartsnow.com through Stripe Checkout, which opens in your browser. The add-in never sees card or payment details.

## Usage counts (off unless you opt in)

If you tick **Send anonymous usage counts**, the add-in sends counts of a fixed list of events (such as "palette opened" (sent as `dialog_opened`) or "quote shown") with the add-in version, as `{client: "fusion", version, events: {name: count}}`. It never sends file names, part data, ids or personal data. A failed send is dropped. To turn this off, delete `%APPDATA%\MakePartsNow\fusion-addin.json`; the add-in will ask for consent again.

## Stored on your computer

- `%APPDATA%\MakePartsNow\fusion-addin.json`: whether and when you accepted the consent text, your usage-count choice, the last ZIP code, and an optional developer server address.
- Exported part files go to a private temporary folder, which is deleted after each quote attempt, whether it succeeds or fails.
- If you click **Save quote to design**, the design stores the price, quantity, material, finish, date and quote link (never the delete token) as an attribute on each part. Anyone you share the design with can see them.
