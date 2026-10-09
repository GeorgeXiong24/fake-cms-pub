# fake-cms-pub

A front-end imitation of the SCIE CMS website (https://cms.alevel.com.cn/).

> **Disclaimer**
> This project is built purely for imitation. It contains no malicious intent, does not collect or transmit any real user data, and is not affiliated with or endorsed by the original website.

## Overview

A static HTML/CSS/JavaScript imitation of the SCIE CMS. There is no build step and no backend — everything runs in the browser. All "login" data is stored only in the browser's `sessionStorage` and is never sent anywhere.

The project consists of two pages that share data through `sessionStorage`:

- **`index.html`** — the login screen and the main UI (homepage).
- **`timetable.html`** — the course timetable page, reached from the "Course Timetable" tile on the homepage.

The layout is originally designed for mobile phones, especially iPhone.

## Project structure

```
fake-cms-pub/
├── index.html      # Login screen + main UI (homepage)
├── timetable.html  # Timetable page
├── assets/
│   └── logo.svg    # SCIE logo
├── README.md
└── .gitignore
```

## Pages

### 1. Login screen (`index.html`)

Shown first. The user fills in their student details to "sign in":

- English Name
- Chinese Name
- Student Number
- House — Metal / Water / Fire / Wood
- Grade — G1 / G2 / A1 / A2
- Form Class Number
- Personal Email
- Mobile Number
- Avatar Photo — image upload with live preview

Additional login features:

- **Input validation** — every field is validated on submit, with inline error messages shown under the field and a red border on invalid inputs:
  - English name: letters, spaces, hyphens and apostrophes only.
  - Student number: letters and numbers only.
  - Class number: a positive integer.
  - Personal Email: valid email format.
  - Mobile number: an 11-digit number starting with `1`.
  - Avatar: an image file is required.
- **Auto-fill from a `.txt` file** — upload a text file to fill all text fields at once (the avatar still needs to be chosen separately). See [Quick-fill from a `.txt` file](#quick-fill-from-a-txt-file).
- **Avatar preview** — the selected photo is shown immediately before submit.

On submit, the details are saved to `sessionStorage` under the key `fakeCmsStudent` and the main UI is rendered.

### 2. Homepage / Main UI (`index.html`)

Rendered after a successful login. It recreates the main interface:

- **Header** — logo and a menu button.
- **Collapsible panels** — Calendar, Daily Bulletin and Notice (expand/collapse on click).
- **Student function grid** — Survey, Course Timetable, Exam Timetable, Attendance, Report, Assessment, Referral Comment, FT's message, Mentoring, Calendar, Events, ECA, Achievement & GCA, Homework, Document, Resource, Leave, Course Selection, Insurance and Health, and University Application.
- **EAO function grid** — Result Services, Certification Request, Exam Entry, Identity Card, E-Agreement and Special Consideration.
- **Welcome card** — shows `Welcome, {English Name}`, the uploaded avatar, a "Show Student Info" popover, and "Change Password" / "Log out" links.
  - The **student info popover** toggles on click (and closes on outside click or the `Escape` key), showing Chinese Name, Student ID, Form Group, Status (Enrolled) and Boarding (Day Student).
- **Session restore** — if a previous session is still present in `sessionStorage`, the login screen is skipped and the main UI is shown directly.

### 3. Timetable page (`timetable.html`)

Reads the student from `sessionStorage` and renders the timetable area. It is a fixed `992px`-wide design that is scaled to fit the viewport. Sections:

- **Student info summary** — avatar, student-number and "Enrolled" tags, English name, Chinese name, pinyin (converted from the Chinese name), and the form group. A **"More Info"** collapsible shows Enrollment, Grade, House, Dormitory Kind, Dormitory, Mobile, School Email and Student Email.
- **Navigation tabs** — Timetable, Report, Homework, Attendance, FT Comment, Referral, Assessment, Exam Timetable, Teacher Availability, Free Classroom, ECA and Achievement & GCA.
- **Timetable grid** — the weekly grid with periods on the left (form time, P1–P14, lunch and pastoral) and Monday–Friday across the top. Each cell shows the subject, room and teacher. The academic year, current date, week number (Week A/B) and each day's date are computed from the real current date.
- **Assembly** — lists the form-time entry for each day (Monday–Friday), using the form group and room from the timetable's first row.
- **Mentoring** — an empty state ("No data").
- **Courses** — a table of the student's courses with teacher and attendance-related stats (TL, TA, A, L, X, S, V), plus a collapsible "Dropped Courses" table.
- **Teachers** — Form Tutor, University Admissions Counsellor and Life Teacher.

## Login details to provide

To make the flow work, fill in the login form with:

- **English Name** — e.g. `George`
- **Chinese Name** — e.g. `帅哥`
- **Student Number** — letters and/or numbers, e.g. `24001`
- **House** — one of Metal, Water, Fire, Wood
- **Grade** — one of G1, G2, A1, A2
- **Class Number** — a positive integer
- **Personal Email**
- **Mobile Number** — an 11-digit number
- **Avatar Photo** — an image file

## Quick-fill from a `.txt` file

The login screen lets you upload a `.txt` file to auto-fill the text fields (the avatar photo must still be chosen separately). 
The file must contain exactly one value per line, in this order:

1. English Name
2. Chinese Name
3. Student Number
4. House (`Metal`, `Water`, `Fire` or `Wood`)
5. Grade (`G1`, `G2`, `A1` or `A2`)
6. Class Number
7. Personal email address (not school email)
8. Mobile number

Empty lines are ignored, and the House/Grade values are matched case-insensitively.

## Dynamic / derived data

Several values are computed automatically rather than hardcoded:

- **Form group** — generated as `Grade.House + ClassNumber`.
- **School email** — `s{studentNo}.{surnamePinyin}@stu.scie.com.cn`, where the surname pinyin is derived from the first character of the Chinese name.
- **Pinyin** — the full Chinese name is converted to pinyin using [pinyin-pro](https://github.com/zh-lx/pinyin-pro) (loaded from a CDN).
- **Academic year** — current year → `YYYY-YYYY`, switching at September.
- **Current date** — today's date as `YYYY-MM-DD`.
- **Week dates** — the Monday–Friday dates of the current week, shown in the timetable header and used in the Assembly list
- **Enrollment date** — computed from the grade and the real current month: the base year is the current year if the month is August or later, otherwise the previous year; G1 uses the base year, and G2 / A1 / A2 subtract 1 / 2 / 3 years respectively. The day is always `-08-01`.
- **Course names** — course names are adjusted for the student's grade (AS/AL level substitution) and the `House1` placeholder is replaced with the student's house + class number.

## How to run

Open `index.html` directly in a browser. No server or installation is required.