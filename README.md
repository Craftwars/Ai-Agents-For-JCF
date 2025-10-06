# JC - Ai Email Assistant – Auto Quote Email (Make Scenario)
**Generated:** 2025-10-05

This blueprint catches a webhook from GoHighLevel when an opportunity moves to **Quoted**, drafts a quote email via **JC Ai Email Assistant (OpenAI)**, and:
- **Sends immediately** if `Outside Carrier Quote Link 1` is present (included in the email body).
- **Creates a Gmail draft** for manual review when no link is present.

## 1) Webhook Payload (from GHL)
Send a JSON body like:
{
  "opportunity_id": "{{opportunity.id}}",
  "contact_name": "{{contact.name}}",
  "contact_email": "{{contact.email}}",
  "contact_phone": "{{contact.phone}}",
  "business_name": "{{custom_values.business_name}}",
  "policy_type": "{{custom_values.policy_type}}",
  "quote_premium": "{{custom_values.quote_premium}}",
  "quote_summary": "{{custom_values.quote_summary}}",
  "agent_name": "{{user.name}}",
  "agent_email": "{{user.email}}",
  "custom_values": {
    "outside_carrier_quote_link_1": "{{custom_values.outside_carrier_quote_link_1}}"
  }
}

## 2) What to Replace in the Scenario After Import
- **OpenAI connection**: `REPLACE_WITH_YOUR_OPENAI_CONNECTION_ID`
- **Gmail connection**: `REPLACE_WITH_YOUR_GMAIL_CONNECTION_ID`
- **GHL API key**: `REPLACE_WITH_YOUR_GHL_API_KEY`
- (Optional) Update the subject: `Your {{1.output.body.policy_type}} Quote from JC Family Insurance`

**From address used:** `sales@jcfamilyinsurance.com.

## 3) Gmail Behavior
- If `outside_carrier_quote_link_1` is **present** → module sends the email immediately and appends the link at the bottom.
- If **absent** → module creates a **draft** so you can review and attach a file before sending.

## 4) Notes / Logging
A note is posted to the Opportunity: `JC Ai Email Assistant sent email (link present) on 2025-10-05.`  
or `JC Ai Email Assistant created draft on 2025-10-05.` (message text is customizable in Module 10 body).

## 5) Tips
- Ensure the GHL workflow trigger is **Pipeline Stage Changed → Quoted** or **Opportunity Updated (Quote Details not empty)**.
- Confirm the custom field key in GHL is exactly **outside_carrier_quote_link_1** or update the scenario mapping.
