# fake-cms-pub

A front-end imitation of the SCIE CMS website (https://cms.alevel.com.cn/).

> **Disclaimer**
> This project is built purely for imitation. It contains no
> malicious intent, does not collect or transmit any real user data, and is not
> affiliated with or endorsed by the original website.

## What it is

A single-page static HTML imitation of the SCIE CMS homepage. It recreates the
overall look, layout and basic interactions of the original site.

## Currently completed features

- **Login screen** shown before the main UI, with:
  - English name, Chinese name, student number
  - House selector (Metal / Water / Fire / Wood)
  - Grade selector (G1 / G2 / A1 / A2)
  - Class number
  - Avatar photo upload with live preview
- **Input validation** — required fields are checked, and inline error messages
  are shown when something is missing or invalid.
- **Data propagation** — after a valid login, the entered details are placed
  into the corresponding spots in the main UI:
  - English name → the welcome heading
  - Chinese name, student number and form group → the "Show Student Info" popover
  - Uploaded photo → the profile avatar
- **Main UI imitation** — header, collapsible panels (Calendar, Daily Bulletin,
  Notice), the Student and EAO function grids, and the welcome card with the
  student info popover. (Only the main interface is completed currently, other features in detail are currently under development.)

## Login details to provide

To make the flow work properly, fill in the login form with:

- **English Name** — e.g. `George`
- **Chinese Name** — e.g. `帅哥`
- **Student Number** — letters and/or numbers, e.g. `24001`
- **House** — one of Metal, Water, Fire, Wood
- **Grade** — one of G1, G2, A1, A2
- **Class Number** — a positive integer
- **Avatar Photo** — an image file

The form group shown in the main UI is generated automatically from these
values in the format `Grade.House + ClassNumber`.

## Further improvements

- **Add the timetable section in the main ui with randomly generated lessons.**
- **Re-check the real cms platform to ensure there is no other ways to verify the authenticity of the website & user id.**
- **Ensure the ui shape and color match that of the real webpage exactly.**
