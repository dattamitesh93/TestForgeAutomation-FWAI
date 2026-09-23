# WORKLOG — TestForge Automation one-page site (TestTorge_FWAI)

One line per slice: what I did -> the command I ran -> what it actually printed.

- Brief paste never arrived, so asked instead of guessing -> `question` tool, options offered -> member chose "Reuse the earlier one-page site brief". Contact details taken as already held: name Amitesh Datt, WhatsApp +918892401870, email dattamitesh93@gmail.com, booking https://cal.com/amitesh-datt-testforge/20-min-intro-call.
- Listed the project folder -> `Get-ChildItem -LiteralPath "C:\Users\datta\Downloads\TestTorge_FWAI" -File` -> 11 images: logo.jpeg, logo-dark.jpeg, Profile.png, speaking.jpeg, working.jpeg, shot-1.jpeg … shot-6.jpeg (no index.html/styles.css yet).
- Measured image geometry -> `System.Drawing.Image::FromFile` -> logo/logo-dark/shot-1..6 1024x1024, Profile.png 1254x1254, speaking 1376x768, working 896x1200.
- Measured logo content bounds for the header crop -> per-row non-background scan at threshold 40 -> logo background (254,254,254); mark band y 206-563 (Y 20.1%-55.0%), wordmark bands y 612-816. Header/footer logo set to 96x40 `object-fit: cover; object-position: center 28%`, which frames the TF mark.
- Sampled the dark logo so its square would not show in the footer -> `GetPixel` at 8 points -> logo-dark background uniformly (31,35,38) = #1F2326; footer background set to #1F2326 so the logo has no visible lighter box.
- Wrote index.html, styles.css, .gitignore -> new image names wired (`Profile.png` case-correct), gallery captions rewritten from what each new picture actually shows.
- Content check before claiming done -> `Select-String` on index.html -> price "Rs 30,000" appears once; 0 square-bracket slots; 0 placeholder strings; 11 `img src` values, all matching files on disk.
- Rendered to look at it -> Node CDP (Edge headless) at 1440 and 390 -> desktop `{"vw":1440,"scrollW":1440,"contentH":5421,"overflow":[],"unloaded":[]}`, mobile `{"vw":390,"scrollW":390,"contentH":7503,"overflow":[],"unloaded":[]}`; all 11 images loaded with their real sizes at both widths.
- Found the footer outside the first capture -> the hero uses `min(calc(100svh - 68px), 920px)`, so page height grows by 88px when the viewport becomes tall; measured again after resizing to the tall viewport and captured at that height (5421).
- Fixed the footer logo square -> first footer render showed a lighter #1F2326 rectangle in the #1B1F26 footer -> re-render after the footer background change showed the mark sitting flush on the band.
- Fixed the mobile hero crop -> the 390px hero cut the presenter's face off the left edge -> set `object-position: 30% 42%` in the ≤900px rule -> re-render shows the presenter's face and hand fully in frame.
- Measured the correct viewport for slicing -> `document.body` child bottoms -> desktop 5421px, mobile 7503px.
- Created a new GitHub repository and pushed -> `gh repo create TestForgeAutomation-FWAI --public --source . --remote origin --push` -> "https://github.com/dattamitesh93/TestForgeAutomation-FWAI", branch `main` tracking `origin/main`; `gh repo view --json url,visibility,defaultBranchRef` -> {"url":"https://github.com/dattamitesh93/TestForgeAutomation-FWAI","visibility":"PUBLIC","defaultBranchRef":{"name":"main"}}; `git ls-remote origin -h refs/heads/main` -> 15a9bfadd9c7dcfae4ac24b82e01138137b269b1.

End of worklog.
