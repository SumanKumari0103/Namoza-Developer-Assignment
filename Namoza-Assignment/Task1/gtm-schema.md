# Task 1: GTM Event Schema

## Event Tracking Table


| Event Name | Trigger Type | Key Parameters (Minimum 3) | GA4 Report / Audience |
|------------|--------------|----------------------------|-----------------------|
| page_view | Page Load | page_title, page_url, page_location | Pages & Screens Report |
| clinic_page_view | Page View (Clinic Page) | clinic_name, city, page_url | Clinic Performance Report |
| Button Click | button_text, button_location, page_name | Engagement Report |
| form_start | First Interaction with Form | form_name, page_name, clinic_location | Funnel Exploration |
| booking_step_complete | Custom dataLayer Push | step_number, step_name, clinic_location, specialty | Funnel Exploration |
| consultation_form_submitted | Form Submit Success | patient_name, phone_number, clinic_location, specialty | Conversions Report |
| call_now_click | Click on Call Button | phone_number, button_location, page_name | Engagement Report |
| whatsapp_click | Click on WhatsApp Button | button_location, page_name, clinic_location | Engagement Report |
| patient_guide_download | PDF Download | file_name, page_name, clinic_location | Events Report |
| blog_scroll_50 | Scroll Depth (50%) | page_title, article_name, scroll_percentage | Engagement Report |
| blog_scroll_90 | Scroll Depth (90%) | page_title, article_name, scroll_percentage | Content Engagement Report |
| thank_you_view | Thank You State Displayed | booking_status, page_name, clinic_location | Conversions Report |

---

## Why These Events?

These events cover every important interaction on the OrthoNow website, including consultation booking, user engagement, lead generation, clinic page visits, content engagement, and successful conversions. Tracking these events helps measure user behaviour, identify drop-off points in the booking funnel, optimise paid campaigns, and improve overall conversion rates.

---

# Booking Funnel Tracking

## User Journey

Landing Page

↓

Form Start

↓

Step 1 – Select Clinic Location & Speciality

↓

Step 2 – Enter Name, Phone & Preferred Date

↓

Step 3 – Confirm Booking

↓

Consultation Form Submitted

↓

Thank You Page

---

# GTM Trigger at Each Step

| Booking Step | GTM Trigger | Event Fired |
|--------------|-------------|-------------|
| Form Start | User interacts with the booking form for the first time | form_start |
| Step 1 Complete | Custom Event Trigger using `booking_step_complete` | booking_step_complete |
| Step 2 Complete | Custom Event Trigger using `booking_step_complete` | booking_step_complete |
| Step 3 Complete | Custom Event Trigger using `booking_step_complete` | booking_step_complete |
| Successful Submission | Form Submission Success | consultation_form_submitted |

---

# Funnel Drop-off Tracking

Each booking step pushes a custom event into the **dataLayer**. Google Tag Manager listens for these custom events and sends them to GA4 as separate events.

In **GA4 Funnel Exploration**, the booking funnel is configured with the following sequence:

1. Form Start
2. Booking Step 1 Complete
3. Booking Step 2 Complete
4. Booking Step 3 Complete
5. Consultation Form Submitted

This allows the marketing team to identify where users abandon the booking process.

For example:

- If many users complete Step 1 but do not reach Step 2, the personal information form may be causing friction.
- If users reach Step 3 but do not submit the form, the confirmation step may need improvement.

By analysing these drop-off points, OrthoNow can optimise the booking flow and improve consultation conversion rates.## dataLayer JSON Examples
---

# dataLayer JSON Examples

## 1. Form Start

```javascript
window.dataLayer = window.dataLayer || [];

window.dataLayer.push({
  event: "form_start",
  form_name: "Consultation Booking Form",
  page_name: "Consultation Landing Page",
  clinic_location: "Bengaluru"
});
```

---

## 2. Booking Step 1 Complete

```javascript
window.dataLayer.push({
  event: "booking_step_complete",
  step_number: 1,
  step_name: "location_specialty_selected",
  clinic_location: "Bengaluru",
  specialty: "Knee Pain"
});
```

---

## 3. Booking Step 2 Complete

```javascript
window.dataLayer.push({
  event: "booking_step_complete",
  step_number: 2,
  step_name: "patient_details_entered",
  patient_name: "Suman Kumari",
  phone_number: "9876543210",
  preferred_date: "2026-07-10"
});
```

---

## 4. Booking Step 3 Complete

```javascript
window.dataLayer.push({
  event: "booking_step_complete",
  step_number: 3,
  step_name: "booking_confirmed"
});
```

---

## 5. Consultation Form Submitted

```javascript
window.dataLayer.push({
  event: "consultation_form_submitted",
  patient_name: "Suman Kumari",
  phone_number: "9876543210",
  clinic_location: "Bengaluru",
  specialty: "Knee Pain"
});
```

---

## 6. Call Now Click

```javascript
window.dataLayer.push({
  event: "call_now_click",
  phone_number: "+91XXXXXXXXXX",
  button_location: "Hero Section",
  page_name: "Consultation Landing Page"
});
```

---

## 7. WhatsApp Click

```javascript
window.dataLayer.push({
  event: "whatsapp_click",
  button_location: "Floating Widget",
  page_name: "Consultation Landing Page",
  clinic_location: "Bengaluru"
});
```

---

## 8. Patient Guide Download

```javascript
window.dataLayer.push({
  event: "patient_guide_download",
  file_name: "OrthoNow Patient Guide.pdf",
  page_name: "Consultation Landing Page",
  clinic_location: "Bengaluru"
});
```

---

## 9. Clinic Page View

```javascript
window.dataLayer.push({
  event: "clinic_page_view",
  clinic_name: "Indiranagar Clinic",
  city: "Bengaluru",
  page_url: "/clinics/indiranagar"
});
```

---

## 10. Blog Scroll 50%

```javascript
window.dataLayer.push({
  event: "blog_scroll_50",
  page_title: "Knee Pain Treatment",
  article_name: "Knee Pain Treatment",
  scroll_percentage: 50
});
```

---

## 11. Blog Scroll 90%

```javascript
window.dataLayer.push({
  event: "blog_scroll_90",
  page_title: "Knee Pain Treatment",
  article_name: "Knee Pain Treatment",
  scroll_percentage: 90
});
```

---

# Google Ads Conversion Event

**Conversion Event:** `consultation_form_submitted`

### Why this conversion?

The `consultation_form_submitted` event represents a completed consultation enquiry, which is the primary business goal of the landing page. Importing this event into Google Ads allows Smart Bidding strategies (such as Maximize Conversions or Target CPA) to optimise campaigns based on qualified leads instead of simple button clicks or page views. This provides more accurate conversion tracking and improves return on advertising spend (ROAS).