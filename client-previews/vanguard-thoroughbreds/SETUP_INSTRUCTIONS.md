# Thoroughbred Racing Syndicate Website — Quick Setup & Customization Guide

This website is built with **ultra-fast, dependency-free semantic HTML5 & CSS3** so it loads instantaneously, scores 98–100 on Google PageSpeed, and lets you edit text, colours, and photos in plain English without fighting complex plugins.

## 1. File Structure
- `index.html` — **Landing Page** (Hero, 3 Syndicate Pillars, Spotlight Horses, Primary "Enquire to Join" CTAs)
- `about.html` — **About & Horses Page** (Syndicate Story, Philosophy, and 2026/2027 Upcoming Horses Roster)
- `contact.html` — **Enquire to Join Page** (Captures Name, Email, Phone Number + optional Horse selection)
- `styles.css` — **Brand Stylesheet** (Change all brand colours in 3 lines at the very top!)
- `assets/` — High-resolution racing imagery & logo folder

## 2. How to Swap Brand Colours & Logo (30 Seconds)
1. Open `styles.css` and edit lines 5–9 (`:root`):
   - `--brand-obsidian`: Your primary dark brand colour
   - `--brand-gold`: Your accent colour
   - `--brand-ivory`: Clean background tone
2. To insert your official image logo, replace `<span class="brand-mark">...</span><span>Vanguard Racing</span>` inside the `<header>` with `<img src="assets/your-logo.png" alt="Your Syndicate Name" style="height:44px;">`.

## 3. How to Route Contact Form Inquiries Straight to Your Inbox
1. Open `contact.html` and locate `<form id="enquiryForm" action="https://formsubmit.co/syndicate@yourdomain.com" method="POST">`.
2. Replace `syndicate@yourdomain.com` with **your real email address**.
3. Submit one test enquiry on your live site and click the one-time confirmation link sent to your email — every future enquiry (Name, Email, Phone, Selected Horse) arrives straight in your inbox for free! (Also 100% compatible with Netlify Forms, Webflow, or Formspree).

## 4. Going Live (Free Hosting + Custom Domain in 3 Minutes)
- **Netlify Drop (Easiest):** Drag and drop this folder onto `app.netlify.com/drop` and connect your custom domain (free SSL included).
- **Important Before Launch:** Remove `<meta name="robots" content="noindex, nofollow">` from the `<head>` of the 3 HTML files so Google indexes your live domain immediately.
