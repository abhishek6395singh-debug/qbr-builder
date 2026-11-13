# QBR Builder

A single-page tool for preparing a client-facing quarterly business review. Fill in the quarter and it builds a slide deck as you type, checks the content, and exports an editable PowerPoint file.

- **Scorecard:** enter target, actual and last quarter for each measure. Met, At risk or Missed is calculated, and the overall Green, Amber or Red is suggested from it.
- **Sections:** wins, issues and risks, next-quarter milestones, and the decisions you need from the client.
- **Readiness checks:** about ten checks on the content, such as whether every miss has an explanation, every win has a number, every open issue has an owner, an action and a date, and every decision names who decides and by when.
- **Deck:** a live 16:9 preview, one slide per section, with long lists continuing on extra slides. Download as `.pptx` with your notes in the speaker notes.
- **Draft with Claude (optional):** when the page is opened inside the Claude artifact viewer, a panel drafts the executive summary and speaker notes from your figures. Numbers in the draft that are not in your data are flagged before you apply it. This panel is hidden everywhere else.

The page opens on a fictional quarter so you can see how it works. Use **Start blank** to begin your own.

## Run it

Open `index.html` in a browser. No build step. Fonts load from Google Fonts and fall back to system fonts when offline. The PowerPoint export loads PptxGenJS 3.12.0 from cdnjs, so the download needs an internet connection. Everything else works offline.

To host it, turn on GitHub Pages (Settings, Pages, deploy from `main`, root folder). On a free plan the repository must be public.

## How status is worked out

For a measure where higher is better, Met is at or above target, At risk is within the chosen band below target (2% of the target by default), and Missed is anything lower. For lower-is-better measures the same logic applies the other way. A target of zero means any value above zero is Missed.

The suggested overall status is Red when more than a third of measures are missed, Amber when anything is missed or at risk, and Green otherwise. You can override it, and a check warns you if your choice differs from the scorecard.

## Tested and not tested

Tested:

- the status rules on 11 edge cases, including a zero target and values such as `1,200`
- the readiness checks on the sample and on a deliberately broken copy
- the number check on a draft with invented figures and one without
- a generated `.pptx`, opened and rendered: 7 slides, notes on each, tables and cards intact
- the Download button saving that file from the standalone page
- the page in Chromium on desktop and phone width, in light and dark, with no console errors

Not tested:

- the Claude draft, which needs the Claude artifact viewer
- loading PptxGenJS from the real cdnjs address. The download button was tested in Chromium with the same library version served locally, and the saved file opened with 7 slides.
- very long text in a table cell, which could crowd a slide
- Safari and Firefox

The number check catches figures that are not in your data. It does not check that the wording is right, so read every draft.

## Limits

- Dates and numbers are shown as typed or in a fixed `15 Jan 2027` style. There is no locale handling.
- The PowerPoint uses Calibri. If it is missing on your machine, PowerPoint substitutes a similar font.
- Data is saved in your browser's local storage only. Clear site data and it is gone.

## Where to change things

All logic sits in the script in `index.html`. The model, checks, deck layout and PowerPoint builder are between the `MODEL-START` and `MODEL-END` comments. The sample quarter is `SAMPLE`.

## Data

The sample is fictional. Meridian Fund Services and Northbridge Delivery are invented names. Do not commit real client figures to this repository, and do not use real client data with the Claude draft unless your organisation allows it.
