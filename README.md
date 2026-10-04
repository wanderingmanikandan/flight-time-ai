# Flight Time Lab

A beginner-friendly aerospace and AI lesson: predict time in the air from distance.

## Open the app on your computer

1. Extract the ZIP folder first.
2. Open `index.html` in Chrome or Edge. When needed, right-click the file and select **Open with → Google Chrome**.
3. Keep **1 · Start simple** selected for your first attempt.
4. Click **Next: train the model →**, then **Train the model**.
5. Click **Next: predict a time →**.
6. Enter `1000` and click **Predict flight time**. The easy lesson gives **100 minutes (1 hour 40 minutes)**.

No installation, API key, dataset upload, or server is required. The app, calculations, graphs, and examples are all inside `index.html`. Source links need an internet connection; the lesson itself has no external dependencies.

## Two lessons, the same three steps

**Start simple:** 20 made-up examples generated at an assumed constant speed of 600 km/h. The model learns from 16 examples and is checked against 4 separate examples. This is the original constant-ground-speed simplification. Airport ground time, wind, and differences between flight phases are not modeled.

**Try real data:** 100 recorded flights from OpenIntro's historical nycflights data. The model learns from 80 examples and is checked against 20 separate examples. This optional lesson is NOT a constant-speed model. It learns an approximate relation from actual airborne times. Recorded airborne times include airborne phases, although the app does not model them separately.

The buttons and workflow are identical in both lessons. Switching lessons resets the model and prediction history.

## A 15-minute student activity

First, complete the easy lesson. Predict the time for 500, 1,000, and 2,000 km. Record the answers: 50, 100, and 200 minutes.

Next, select **Try real data** and repeat the three steps with the same distances. The displayed estimates are about 58, 97, and 176 minutes. Explain why these answers need not follow the assumed 600 km/h rule.

Finally, open **Check your understanding** and answer the three questions. No programming knowledge is needed to use the app.

## Publish on GitHub Pages

1. Sign in to GitHub and create a new **public** repository named `flight-time-ai`.
2. Use **Add file → Upload files**. In a newly empty repository, GitHub may instead show an **uploading an existing file** link.
3. Upload the five files from this extracted package. Do not upload the ZIP itself. Put `index.html` directly at the repository's top level, not inside another folder.
4. Use the commit message `Add Flight Time Lab app` and click **Commit changes**.
5. Open **Settings → Pages** in that repository.
6. Under **Build and deployment**, select **Deploy from a branch**.
7. Select **main** and **/(root)**, then click **Save**.
8. Wait for publication. Return to **Settings → Pages** and use **Visit site**. Share that published link with students, not the GitHub code-file link.

The expected project-site format is `https://YOUR-USERNAME.github.io/flight-time-ai/`.
This package has not been uploaded to or published on your GitHub account.

Official instructions: https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site

## What is in this package?

- `index.html` — the complete app, built-in student explanations, graphs, quiz, and embedded data.
- `REAL_FLIGHT_DATA.csv` — the 100 real records, with units, source-row numbers, train/test labels, and provenance.
- `DATA_SOURCES.md` — source links, preparation rules, model details, and limitations.
- `README.md` — these operating and publishing instructions.
- `LICENSE.txt` — GNU General Public License version 3.

The app's data-download buttons also export the easy dataset or the real dataset. **Save my predictions** exports the student's current-session predictions. Reloading the page clears unsaved predictions.

## Important limits

This is a classroom exercise, not a flight-planning, ticket-duration, arrival-time, or delay-prediction tool. Only distance is used. The real data are a small historical sample from flights departing the New York area in 2013, not Indian or live worldwide flight data.

The easy lesson has zero test error because its data follow an exact invented rule. That is not evidence of real-aircraft accuracy. In the real lesson, the average held-out error is approximately 9.1 minutes on these 20 test rows; individual errors can be greater. That figure is neither a confidence interval nor a future accuracy guarantee.

## Verification

The implementation passed 63 automated checks in headless Chromium, including model fitting, independent numerical comparisons, prediction validation, switching lessons, CSV downloads, quiz feedback, offline execution of the embedded HTML, and layouts at 390, 760, and 1280 pixels. Browser coefficients and test errors were checked against an independent Python calculation. The managed test browser did not allow direct file-URL navigation; the HTML was loaded directly into the browser for these checks. Live GitHub deployment was not performed.

## License and credits

Code and accompanying material: GNU GPL version 3; see `LICENSE.txt`. Preserve data attribution when redistributing. The OpenIntro R package declares GPL-3; OpenIntro credits Hadley Wickham's nycflights13, whose documentation credits the U.S. Bureau of Transportation Statistics. See `DATA_SOURCES.md`.

This independent teaching app is not endorsed by OpenIntro, BTS, GitHub, or any airline.
