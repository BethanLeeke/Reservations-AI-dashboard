# Reservations AI Dashboard

A browser-based dashboard for the Vale Resort and Hensol Castle reservations team. It gathers the quoting, forecasting and daily operational tools behind one page so they can be reached from a single link.

**Live site:** <https://bethanleeke.github.io/Reservations-AI-dashboard/>

## Tools

| Tab | What it does |
| --- | --- |
| **Golf quoting tool** | Golf break quotations — customer details, green fees, buggies, catering, accommodation and discounts, producing a printable PDF quote. |
| **Spa quoting tool** | Spa break quotations for the five spa packages, priced by room and by night of the week, with custom items and discounts. The PDF quote carries what the package includes and its dining arrangements, helpful information, the treatment choices for that package and a link to the spa terms. |
| **Forecast** | Drop in the week's reports (Ten Day On Books for both sites, group bookings, activity and event schedules, res diary) to build a rooms, sleepers, group and diner forecast. |
| **Morning Brief** | The same reports plus the inventory pickup, turned into the daily brief: rooms, sleepers, groups, diners and VIPs. |
| **Golf Matrix pricing** | Guest-facing and reservations-facing breakdowns of a golf booking, priced from the golf matrix workbook. |
| **Daily arrivals** | Drop in the Detail Arrivals export to review the day's arrivals — filter, sort, approve and export. |

Each tab is a self-contained page with a shared tab bar across the top, so you can move between tools without going back to the dashboard.

## How it works

There is no build step, package manager or server. Every tool is a single HTML file with its CSS and JavaScript inline, which is why the repository can be served as a static site by GitHub Pages.

**Nothing you load leaves your computer.** The forecast, morning brief, matrix pricing and arrivals tools all read the spreadsheets you drop onto them in the browser itself. No report, guest list or price file is uploaded anywhere.

Anything a tool remembers between visits — the golf matrix, the Prepared By list — is kept in that browser's local storage. It is per browser and per machine, not shared between colleagues.

## How the spa quoting tool prices a break

Rates and inclusions come from the reservations team's spa package sheet. Every package is one night, priced per person based on two guests sharing, so the room rate is twice the per person rate. Golf & Spa is the exception: it is priced per couple.

- **Each night is charged at the rate for the day it falls on** - Sunday to Thursday, Friday or Saturday - so a Friday-and-Saturday stay is a Friday night plus a Saturday night.
- **Rooms with two guests** are charged the room rate. **A room with one guest** is charged the per person rate plus the £50 single supplement. With two guests in every room this is simply rooms × room rate.
- **Golf & Spa** needs two guests per room - one spa guest and one golfer.
- **Together Spa Break** needs at least 10 guests, and takes a £50 per person deposit at booking rather than full payment.

The tool will not generate a quote that breaks these rules; it says what needs fixing instead.

The PDF's Sunday Lunch details appear only when the stay takes in a Sunday, from arrival day to departure day. Treatment choices follow the package sheet: the Full Day list for the full day packages, Golf & Spa and Together, and the Half Day list for the two half day packages.

The quote can also carry the booking's details. Tick the **treatment times available** (09:30 to 17:00) and they are listed on the PDF; each guest's time is then picked from the ticked times. **Guests & treatments** gives one row per guest - name, treatment from that package's list, and time - and Golf & Spa rooms list a spa guest and a golfer, the golfer having no treatment. There is also a **double or twin preference** per room and a free-text **dietary requirements** box. All of these appear in the quote preview and in a Your Guests & Treatments section of the PDF, and only what has been filled in is shown; anything left blank reads as to be confirmed.

The rates live at the top of the spa tool's script (`SPA_PACKAGES`), so a new rate card means updating those numbers. The tool's source is `Reservations/Spa Quoting Tool.html` in the sales team's shared folder.

## Running it locally

Clone the repository and open `index.html`, or serve the folder:

```bash
git clone https://github.com/BethanLeeke/Reservations-AI-dashboard.git
cd Reservations-AI-dashboard
python -m http.server 8000
```

Then visit <http://localhost:8000>.

Allow pop-ups when generating a PDF: the quoting tool opens a print-friendly window and then uses the browser's print dialog.

## Project structure

```text
.
|-- index.html                  # dashboard landing page
|-- golf-quoting-tool/          # golf break quotations
|-- spa-quoting-tool/           # spa break quotations
|-- forecast/                   # hotel forecast builder
|-- morning-brief/              # morning brief builder
|-- golf-matrix-pricing/        # golf matrix pricing tool
`-- daily-arrivals/             # daily arrivals report
```

## What is deliberately not in this repository

This is a **public** repository, because that is what GitHub Pages serves from on this account. Two things were therefore kept out of it:

1. **The arrivals export's guest list.** The source file for the Daily arrivals tab arrived with a real day's arrivals baked into it — 113 guests with names, confirmation numbers, room numbers, balances and notes. That data was stripped out. The tool starts empty and reads whatever report you drop onto it, which is how it is meant to be used.
2. **The golf matrix workbook.** The rates and agent commission sheets live in the `.xlsx` file, not in the tool. Load your own copy through the tool's **Update matrix** button; it is then remembered in that browser.

Keep it that way: do not commit a filled-in arrivals export, a golf matrix, or any other report containing guest or rate data.

## Related

The sales team's quoting tools live separately at
<https://bethanleeke.github.io/wedding-quote-tool/>. The golf quoting tool here began as the one on that site and differs in two ways: golf breaks only, with the golf day side removed, and the reservations team on the Prepared By list behind the shared `reservations@valeresort.com` inbox rather than individual addresses.

Both sites are served from `bethanleeke.github.io`, so they share the browser's local storage. The two golf tools therefore keep their Prepared By lists under different keys and will not overwrite each other.

## Important note

This is an internal aid. Tariffs, VAT treatment, terms, forecasts and generated totals should be checked against the latest approved commercial information before anything is sent to a customer or relied on operationally.
