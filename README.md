# PII Scrubber

A self-contained, fully local HTML tool that redacts personally identifiable information (PII) from transcripts and other client documents. Runs entirely in the browser — no network calls, no data leaves your machine. Open the HTML file directly to use it.

## Use it

Download `pii-scrubber.html` and double-click it. That's the whole install. It works with Wi-Fi off, which is the simplest way to confirm the privacy claim for yourself.

## What it detects

**Structured identifiers** (exact pattern matching, high confidence): email addresses, URLs, US SSNs, credit card numbers (Luhn-validated), phone numbers, IPv4/IPv6/MAC addresses, US street addresses, IBANs, AWS access keys, and JWTs. Off by default but available: ZIP codes, CA/UK postal codes, dates, @handles, and long hex tokens.

**Names and organizations** (heuristic — read the caveat below): person names and company names, found by anchor-and-propagate. The tool identifies a name once through a high-confidence signal, then redacts every other mention of that person throughout the document, including later first-name-only references.

Anchors used: transcript speaker labels (`Sarah Chen:`, `>> Sarah Chen:`, `Sarah Chen (00:14):`), WebVTT voice tags, honorifics and titles (`Dr.`, `Ms.`, `CEO`), a built-in list of common given names followed by a surname, and corporate suffixes (`Inc`, `LLC`, `Ltd`, `Group`, `Partners`).

Each distinct person keeps a stable placeholder, so `[PERSON_1]` is the same human everywhere and the transcript stays readable.

## Important caveat about names

Names have no fixed shape. `Sarah Chen` is structurally identical to `Last Tuesday`, so unlike an email address or SSN, **a name cannot be detected reliably**. This tool raises recall substantially, but it will miss names, and a miss means PII stays in your document.

It is strongest on **speaker-labeled transcripts**, where nearly every participant is named by an anchor. It is weakest on **prose**, where an unfamiliar name mentioned only in passing has nothing to anchor on. The built-in given-name list also skews toward common Anglo, Hispanic, South Asian, East Asian, Arabic, and European names, so less common names are likelier to be missed.

Two mitigations:

- **Custom terms** — paste in any name, codename, or account ID you know appears. This is exact, reliable, and the right place for anything critical.
- **Aggressive name guessing** (off by default) — redacts *any* capitalized multi-word phrase. Catches unusual names, but also redacts place names, headings, and committee names. Useful as a review pass.

**Always read the scrubbed output before sharing it.** Treat this tool as a way to cut manual redaction effort, not as a guarantee.

## Privacy

The page has no `fetch`, `XMLHttpRequest`, `WebSocket`, or `sendBeacon` calls, no external scripts, stylesheets, images, or fonts, and no `localStorage`, cookies, or other persistence. File loading uses the browser's local `FileReader`; the download button builds the file in memory. Close the tab and nothing remains.

The text boxes set `spellcheck="false"`, because some browsers' enhanced spell-check sends typed text to the vendor's servers.
