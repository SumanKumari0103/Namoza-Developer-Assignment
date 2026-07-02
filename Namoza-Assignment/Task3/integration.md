# Task 3: Integration Design

## Overview

The objective is to automatically process every consultation enquiry submitted through the OrthoNow landing page. The integration should ensure that the lead is stored in HubSpot CRM, the patient receives a WhatsApp confirmation within two minutes, and the Google Ads conversion is tracked for campaign optimisation.

---

## End-to-End Integration Architecture

1. The patient fills out the consultation form on the OrthoNow landing page and clicks **Book Free Consultation**.

2. The front-end validates the form and fires the following GTM event:

```javascript
window.dataLayer.push({
  event: "consultation_form_submitted"
});
```

3. At the same time, the form data is sent securely to the backend using an HTTPS POST request.

4. The backend checks HubSpot CRM for an existing contact using the **HubSpot CRM Search API** with the patient's phone number. Since HubSpot primarily performs duplicate management using email addresses, searching by phone number helps prevent duplicate healthcare records.

5. If the phone number already exists, the backend updates the existing contact. Otherwise, it creates a new contact with the following properties:

- Name
- Phone Number
- Clinic Preference
- Source = "Google Ads - Consultation Landing Page"
- Lead Status = "New Enquiry"

6. After the CRM operation is completed successfully, the backend sends a request to the **Karix WhatsApp Business API** to deliver an automatic confirmation message to the patient.

7. Google Tag Manager captures the `consultation_form_submitted` event and forwards it to Google Analytics 4. This event is then imported into Google Ads as a conversion so that Smart Bidding strategies such as **Target CPA** and **Maximize Conversions** can optimise campaigns based on completed consultation enquiries.

---

## Biggest Failure Point and Fallback

The biggest failure point is the HubSpot CRM API. If HubSpot is unavailable, new consultation requests may fail to reach the CRM.

To prevent data loss, I would implement a retry mechanism. If the retry also fails, the lead would be temporarily stored in a database or message queue and processed again once HubSpot becomes available. This ensures that no patient enquiry is lost.

---

## WhatsApp SLA and Monitoring

The confirmation message must be delivered within two minutes. Delays may occur due to API failures, network issues, or temporary service outages.

To meet this SLA, I would use asynchronous background processing with automatic retries. The integration would be monitored using:

- Google Tag Manager Preview Mode
- Google Analytics 4 Events
- HubSpot Contact Dashboard
- Karix Delivery Reports
- Application Logs
- API Response Monitoring

Alerts should be configured to notify the support team whenever message delivery exceeds two minutes or an API request fails. This enables quick investigation and ensures reliable patient communication.

---

## Conclusion

This integration architecture provides a reliable and scalable workflow for managing healthcare enquiries. Every consultation request is securely captured, stored in HubSpot CRM, acknowledged through WhatsApp, and tracked in Google Analytics 4 and Google Ads. This approach improves lead management, campaign optimisation, and the overall patient experience while ensuring that no valuable enquiries are lost.