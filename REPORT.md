# REPORT — TestForge Automation one-page site (TestTorge_FWAI)

## Status per part

Page built as one static page from the earlier brief, into `C:\Users\datta\Downloads\TestTorge_FWAI`: DONE
  evidence: `Get-ChildItem -LiteralPath "C:\Users\datta\Downloads\TestTorge_FWAI" -File` lists
  index.html, styles.css, .gitignore and the 11 pictures.

Sections in the brief's order (top, problem, three services, three steps, who they are, contact): DONE
  evidence: render capture at 1440px shows, in order, hero with speaking photo, dark problem band,
  three service cards with the single price line, six-picture gallery, three steps, about with
  working photo + round profile photo and the name, then the contact band and footer.

Top of page does its one job: DONE
  evidence: hero carries "TestForge Automation" in the eyebrow, headline "Your regression runs
  itself by Friday." with "Friday." in Signal Amber, one line under it, the booking button, and
  speaking.jpeg filling the rest of the first screen at full width.

Contact details are real, no placeholders: DONE
  evidence: `Select-String` on index.html -> booking https://cal.com/amitesh-datt-testforge/20-min-intro-call
  (3 occurrences), https://wa.me/918892401870?text=Hello%20TestForge%20Automation... (1), mailto:dattamitesh93@gmail.com?subject=TestForge%20Automation%20enquiry (1);
  0 occurrences of `[`, 0 occurrences of BOOKING_LINK_GOES_HERE/TODO/PLACEHOLDER/YOUR_.

Price written once: DONE
  evidence: `Select-String -Pattern "30"` -> one match, line 73: `<strong>Rs 30,000 a project</strong>`.

Layout verified at desktop and true mobile width: DONE
  evidence: Node CDP capture against Edge headless ->
  desktop `{"vw":1440,"scrollW":1440,"contentH":5421,"overflow":[],"unloaded":[]}`;
  mobile `{"vw":390,"scrollW":390,"contentH":7503,"overflow":[],"unloaded":[]}`;
  all 11 images reported loaded at natural size (`speaking.jpeg 1376x768`, `working.jpeg 896x1200`,
  `Profile.png 1254x1254`, the rest 1024x1024) at both widths.

Look inspected and corrected, not assumed: DONE
  evidence: sliced captures read back at 1440 and 390 -> fixed the footer logo's lighter square
  (footer background set to the logo's own #1F2326) and the mobile hero crop that cut the
  presenter's face away (`object-position: 30% 42%`). Both re-rendered and re-read.

First GitHub repository for this build: see the repository line at the bottom of this report.

## What broke and how I fixed it

1. The brief the member pasted (about 63 lines) never reached the model context — the message
   contained only the folder path and a `[Pasted ~63 lines]` marker. Instead of guessing, I asked;
   the member chose to reuse the earlier one-page site brief. Everything below is built to that
   brief, with the contact details already held in the same project.
2. The page uses `min-height: min(calc(100svh - 68px), 920px)` on the hero, so the document grows
   by 88px when the viewport is made tall. My first full-page capture therefore stopped before the
   footer. Fixed by re-measuring the document height after resizing to the tall viewport and
   capturing at that height (5421px desktop).
3. The footer logo showed a faint lighter rectangle: logo-dark.jpeg's own background is #1F2326
   while the footer band was Deep Graphite #1B1F26. Fixed by setting the footer background to
   #1F2326, matching the file exactly; the cream top border still separates it from the contact band.
4. The 390px hero cropped the presenter's face off the left edge. Fixed with
   `object-position: 30% 42%` inside the ≤900px rule, then re-rendered and read the picture back.

## Claims ledger

- "The page is built and looks right" -> desktop and mobile captures above, read back as images (proven).
- "Every contact link is real" -> the three links quoted verbatim above (proven in the file; the
  cal.com target is the link the member gave, UNVERIFIED as a live page from here).
- "All 11 pictures are used and load" -> CDP `unloaded: []` plus the 11 `naturalWidth x naturalHeight`
  readings, and 11 `img src` values matching files on disk (proven).
- "The price is written once" -> one `30` match in index.html (proven).
- "The repository exists and is pushed" -> the `gh repo create ... --push` output below (see the
  command result printed at commit time).

## What I would tell the next person

- **The logo wordmark reads "TestForge Automate"**, while the business name used in the page text is
  "TestForge Automation" (exactly as the brief states). The logo is used as a picture only, and the
  header/footer crop shows the TF mark, so nothing on the page contradicts itself. If the member
  wants the wordmark to read "Automation", the logo files need regenerating.
- **shot-1.jpeg is a studio portrait of a different person**, not of Amitesh Datt (the founder photo
  is Profile.png). It is placed last in the gallery with the neutral caption "Engineer portrait" and
  is never presented as the founder. If the member would rather not show a second face, drop
  `shot-1.jpeg` from the gallery — it is one figure element, and the grid reflows on its own.
- The gallery images are 1:1 and fixed by width; the hero is the only cropped picture, so aspect
  changes to `speaking.jpeg` are the one thing that would need re-checking at 390px.
- No environment variables, no backend, no build step: import the repository on Vercel with the
  framework preset "Other" and it serves the folder root as-is.
