# SASE @ WPI Membership Points Tally

**Website:** https://luhaitang13895.github.io/sase-membership-points/

This tool tracks membership points for SASE at WPI. It takes the attendance form responses from each event and keeps a running roster. Each member gets one point for every event they attend.

Everything runs in your browser. You don't need to install anything or create an account, and the CSVs you upload never leave your computer.

---

## Contents

1. [What the tool produces](#what-the-tool-produces)
2. [Before you start: the attendance form](#before-you-start-the-attendance-form)
3. [Step 1: Clean and export the data](#step-1-clean-and-export-the-data)
4. [Step 2: First event of the term](#step-2-first-event-of-the-term)
5. [Step 3: Every event after that](#step-3-every-event-after-that)
6. [Catching up on several events at once](#catching-up-on-several-events-at-once)
7. [Reading the warnings](#reading-the-warnings)
8. [File naming and storage](#file-naming-and-storage)
9. [Rules the tool follows](#rules-the-tool-follows)
10. [Troubleshooting](#troubleshooting)
11. [Updating or moving the website](#updating-or-moving-the-website)

---

## What the tool produces

The tool outputs a **formatted roster** CSV with one row per member:

| Name | Email | Events | Membership Points |
|---|---|---|---|
| Luhai Tang | ltang@wpi.edu | SASE Saves the Multiverse; Resume Review | 2 |
| … | … | … | … |

- **Name**: the name the person entered on the form.
- **Email**: their WPI email in lowercase. This is how the tool tells people apart.
- **Events**: every event they've attended this term, separated by semicolons.
- **Membership Points**: how many events they've attended.

Rows are sorted from most points to fewest, then alphabetically by name.

---

## Before you start: the attendance form

All events use the same Google Form. Its responses go to one Google Sheet with these columns:

```
Timestamp, Full Name, WPI Email, Graduation Year, Event
```

The tool needs three of them: **Full Name**, **WPI Email** and **Event**. The other columns are ignored. If you edit the form, keep the words "name", "email" and "event" in those question titles, because the tool finds its columns by those words.

The **Event** answer has to be spelled exactly the same for everyone at a given event, so make it a dropdown or multiple-choice question rather than free text. "Resume Review" and "Resume review" count as two different events.

---

## Step 1: Clean and export the data

Do this after every event.

1. **Open the response Google Sheet.**
2. **Fix personal emails.** Press `Ctrl + F` (`Cmd + F` on Mac) and search for `@wpi.edu`. Anyone whose row doesn't match used a personal email such as Gmail. Change it to their WPI email, because otherwise their points will be split across two rows. The website also flags any non-WPI email it finds, so you can catch ones you missed.
3. **Export the event.** You can do this one of two ways:
   - **Option A (recommended):** Filter the sheet to the event that just happened (Data → Create a filter, then filter the **Event** column). Then go to **File → Download → Comma-separated values (.csv)**. Rename the file `raw_eventName.csv`, for example `raw_ResumeReview.csv`.
   - **Option B:** Download the whole sheet as CSV without filtering. The website lets you choose which event to count from a dropdown.

> Google Sheets downloads the **current tab**. If you filtered with a filter view, check that the downloaded file contains only the event you wanted.

---

## Step 2: First event of the term

Use this only once per term, for the first event.

1. Open the website and choose the **First event of the term** tab.
2. Drag `raw_eventName.csv` into the **Raw form export** box, or click the box to browse for it.
3. If the file contains more than one event, choose the right one in the **Event to count** dropdown.
4. Check the roster and any warnings shown under the upload box (see [Reading the warnings](#reading-the-warnings)).
5. Check the **File name** box. It's filled in automatically as `fixed_<event>.csv`, and you can change it.
6. Click **Download CSV**.

Everyone who attended starts with **1 point**.

---

## Step 3: Every event after that

1. Open the website and choose the **Add an event** tab.
2. **File 1 · formatted:** drop in the **most recent** `fixed_….csv` the tool gave you, from the previous event.
3. **File 2 · raw form export:** drop in the new event's `raw_eventName.csv`.
4. If needed, choose the new event in the **Event to count** dropdown.
5. Look over the result:
   - Returning members get **+1 point**, and their point total turns blue.
   - People attending for the first time are added with **1 point** and tagged **NEW**.
   - The summary numbers show how many people were at the event, how many were returning, how many were new, and the total on the roster.
6. Click **Download CSV** and save it as `fixed_eventName.csv`.

That new file becomes File 1 for the next event, so each event builds on the one before it:

```
raw_Event1  ──(First event)──▶ fixed_Event1
fixed_Event1 + raw_Event2 ──(Add an event)──▶ fixed_Event2
fixed_Event2 + raw_Event3 ──(Add an event)──▶ fixed_Event3
…
```

> ⚠️ **Always use the newest fixed file as File 1.** If you use an older one, the events in between won't be counted. If you add the same event twice, people get double points. The tool warns you when the event you're adding is already in File 1.

---

## Catching up on several events at once

If you fall behind or take over partway through a term:

1. Download the **entire** response sheet as CSV, without filtering.
2. On the **First event of the term** tab, upload it.
3. In the **Event to count** dropdown, choose **All N events in this file**.
4. Download the result. It contains everyone's points for every event in the sheet.

After that, go back to adding events one at a time with **Add an event**.

---

## Reading the warnings

| Message | What it means | What to do |
|---|---|---|
| **Red: "…was already in the previous roster…"** | The event you're adding is already counted in File 1. | Check that File 1 is the roster from *before* this event. Don't download the result. |
| **Orange: "Not a WPI email…"** | Someone used a non-@wpi.edu address. Their email is also shown in red in the table. | Fix it in the Google Sheet, export again, and reload the file. You can also fix the email in the downloaded CSV. |
| **Orange: "N duplicate submissions were ignored"** | Someone filled out the form more than once for the same event. | Nothing. They're only counted once. |
| **Red: "This doesn't look like the attendance form export…"** | The raw file is missing the Name, Email or Event columns. | Make sure you downloaded the response sheet, not something else. |
| **Red: "This file already has a points column…"** | You put a fixed roster in the raw-file slot. | Move it to File 1. |
| **Red: "This doesn't look like a formatted roster…"** | File 1 isn't a fixed file from this tool. | Use a `fixed_….csv` the tool produced. |

---

## File naming and storage

Recommended naming:

- `raw_eventName.csv`: straight from the Google Sheet
- `fixed_eventName.csv`: the roster after that event was added

Keep every `fixed_` file in a shared Google Drive folder for the term, such as `Membership Points / A Term 2026/`. Each one is a snapshot, so if something goes wrong you can go back to the previous fixed file and redo the merge.

The newest `fixed_` file is always the current official points tally.

---

## Rules the tool follows

- **People are matched by email, ignoring capitalization and spaces.** `Ltang@wpi.edu`, `ltang@wpi.edu` and ` ltang@wpi.edu ` are the same person.
- **One point per event.** Several submissions from the same email for the same event count once.
- **Names come from the first time someone appears.** If a later submission spells their name differently, the roster keeps the original. Edit the CSV if you need to change it.
- **Matching never uses names.** Two people with the same name but different emails get separate rows. One person with two different emails also gets two rows, which is why Step 1 fixes emails first.
- **Rows with no email are skipped.**

---

## Troubleshooting

**Someone has two rows.**
They used two different emails, usually a WPI email and a personal one. Fix the email in the Google Sheet and redo the merge from the last good fixed file. You can also merge the rows by hand in the CSV by adding their points together and deleting one row.

**Someone says their points are wrong.**
Search the response Google Sheet for their name and check which emails they used. Then compare the events they attended with the **Events** column in the latest fixed file.

**The download button does nothing.**
Use **Copy CSV text**, paste it into a new text file, and save it with a `.csv` extension. You can also paste it into a new Google Sheet.

**The page looks blank or broken.**
Refresh the page. The tool loads a CSV library from the internet, so you need to be online.

**Opening the CSV in Excel shows odd characters.**
Open it in Google Sheets with **File → Import**, or use Excel's **Data → From Text/CSV** and choose UTF-8.

---

## Updating or moving the website

The whole site is the single file `index.html` in this repository. GitHub Pages serves it from the `main` branch root.

- **To make a change:** edit `index.html` on GitHub (pencil icon) or upload a new version, then commit. The site updates within a couple of minutes.
- **To check Pages settings:** go to **Settings → Pages**. Source should be **Deploy from a branch**, with Branch **main** and **/ (root)**.
- **To hand off to the next membership chair:** add them as a collaborator under **Settings → Collaborators**, or transfer the repository under **Settings → General → Transfer ownership**. Transferring changes the website address to `https://<new-owner>.github.io/sase-membership-points/`, so update the link in this README and anywhere else it's posted.

### Where the logic lives in `index.html`

| Function | What it does |
|---|---|
| `readRaw` | Reads the Google Form export and finds the Name, Email and Event columns |
| `readFormatted` | Reads a `fixed_` roster |
| `compute` | Merges the roster and the new event and adds points |
| `toCSV` | Writes the output columns |
| `showExample` | Shows the example rows displayed before any files are uploaded |

---

*Built for SASE @ WPI. Maintained by the Membership Chair. Last updated October 2026 by Luhai Tang.*
