# AI-WhatsApp-Booking-FAQ-Assistant

An AI-powered automation system that handles customer WhatsApp messages for a UK hospitality venue: answering FAQs, managing booking requests against live calendar availability, and escalating anything it can't confidently handle to staff. Built and deployed as a real client project, not a tutorial exercise.

Why this exists:
Small hospitality venues get a constant stream of repetitive WhatsApp messages (opening hours, table bookings, menu questions) with no staff bandwidth to answer instantly, especially outside service hours. This system gives the venue a 24/7 first responder that handles the routine volume and only pulls a human in when it actually needs one.

How it works:
Customer sends WhatsApp message
Twilio WhatsApp API
n8n webhook trigger
Google Gemini: intent classfifcation - FAQ: Generate answer via Gemini, Reply sent by Twillio
-Booking request: check/update Google calendar
-Unclear or high stakes: Escalate to staff via Gmail, Staff follow up manually

Every inbound message is classified by intent first, then routed down one of three branches: answer directly, handle a booking, or hand off to a person. That third branch matters as much as the other two — the system is designed to know what it shouldn't try to handle.

Tech stack:
Layer                                      	Tool
Workflow orchestration	                    n8n (self-hosted)
Messaging channel	                          Twilio (WhatsApp Business API)
Intent classification & NL responses      	Google Gemini
Booking / availability	                    Google Calendar API
Staff escalation                           	Gmail API
Hosting	                                    Docker
Local dev tunneling	                        ngrok

Features:
Automated FAQ responses (hours, menu, location, policies)
Booking requests checked and confirmed against real calendar availability
Escalation branch for anything outside the bot's confidence, routed to staff by email
Fully self-hosted, no reliance on a third-party SaaS platform

Status:
Live and working end-to-end (FAQ, booking, and escalation branches all functioning) as of the current deployment. Currently running on free-tier and personal credentials as a first client deployment; migrating to dedicated client-owned credentials is the next production step before scaling to additional venues.
Currently WhatsApp-only. Facebook and Instagram messaging are next, expanding this from a single-channel bot into a broader multi-channel social media assistant.

Setup:
Clone the repo and start n8n via Docker:
bash
   docker compose up -d
Expose the local instance for webhook testing:
bash
   ngrok http 5678
Add the required nodes to complete the automation.
Add credentials in n8n for Twilio, Google Gemini, Google Calendar, and Gmail.
Point your Twilio WhatsApp sandbox/number's webhook at your n8n webhook URL.

What I learned building this:
Designing intent classification that's reliable enough to trust with real customer messages, not just demo inputs.
Structuring an n8n workflow so three very different outcomes (answer, book, escalate) share one entry point cleanly.
The gap between "works on my free tier" and "ready to run on a client's own infrastructure and credentials" — currently closing that gap.
Multiple executions and failure doesn't automatically mean failure but a different approach/perspective is required. Number of versions of the same workflow will probably head into the thousands.

Roadmap:
Migrate to client-owned Twilio/Google credentials for production.
Add Facebook Messenger and Instagram DM support.
Reframe as a general-purpose "Social Media Assistant" for hospitality venues.

Demo:
<img width="1284" height="2398" alt="IMG_0979" src="https://github.com/user-attachments/assets/86dc75ec-ca40-4de6-bb2a-4ea78a8ca990" />

Workflow:
<img width="692" height="315" alt="Screenshot 2026-08-25 173647" src="https://github.com/user-attachments/assets/fac01a76-88a8-48a9-a972-84af8797df26" />

