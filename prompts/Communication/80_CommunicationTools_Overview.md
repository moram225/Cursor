# 80_CommunicationTools_Overview.md

**Core Feature:** Communication Tools (Reminders and Notifications)

**Purpose:** This feature provides communication tools within the Veterinary PMS, primarily focusing on appointment reminders and various types of notifications.  It aims to enhance client communication, reduce missed appointments, and ensure timely information delivery to both clients and clinic staff. This feature will integrate with Appointment Scheduling, Patient Management, and potentially other features to trigger relevant communications.

**User Stories:**

*   As a **Receptionist**, I want to be able to **set up automated appointment reminders** to be sent to clients via SMS and/or Email before their scheduled appointments.
*   As a **Receptionist**, I want to be able to **customize reminder messages** and timing (e.g., 24 hours before appointment, 1 hour before appointment).
*   As a **Receptionist**, I want to be able to **view a log of sent reminders** to track communication history and troubleshoot delivery issues.
*   As a **Veterinarian**, I want to **receive notifications for new appointments scheduled in my name** or for urgent patient-related updates.
*   As a **Technician**, I want to **receive notifications for tasks assigned to me** (e.g., lab tests to be performed, medications to be administered).
*   As a **Clinic Administrator**, I want to **configure default reminder settings** for the clinic (e.g., default reminder times, channels).
*   As a **Client**, I want to **receive appointment reminders** via SMS and/or Email so I don't forget my pet's appointment.
*   As a **Client**, I want to be able to **choose my preferred communication channels** (SMS, Email) for reminders (future enhancement - initially clinic-managed preferences).

**Key Functionalities:**

*   **Appointment Reminders:**
    *   Automated sending of appointment reminders via SMS and/or Email.
    *   Configurable reminder timing (e.g., 1 day before, 2 hours before).
    *   Customizable reminder message templates (with placeholders for client name, patient name, appointment time, etc.).
    *   Management of reminder channels (SMS, Email, potentially in-app notifications later).
    *   Logging of reminder sending status (sent, failed, delivery status if possible from provider).

*   **General Notifications:**
    *   System-generated notifications for various events (e.g., new appointment, appointment cancellation, task assignment, low stock alert - link to other features to trigger notifications).
    *   Notification delivery to relevant users (staff, potentially clients for certain notifications in the future).
    *   In-app notification display (future enhancement - initially focus on SMS/Email).
    *   Potentially user-configurable notification preferences for different event types (future enhancement).

*   **Communication Channel Management:**
    *   Integration with SMS gateway providers (e.g., Twilio, Nexmo - *for now, a simplified/stubbed SMS sending service can be implemented, actual integration can be a later phase*).
    *   Email sending capabilities (using SMTP or email service providers - *similarly, simplified email sending initially*).
    *   Management of SMS and Email templates.
    *   Tracking communication credits/usage (if applicable for SMS).

**Database Tables (Relevant Entities):**

*   **Appointments**: To link reminders to appointments.
*   **DimClients**:  To get client contact information for reminders.
*   **CentralUsers** (or TenantUsers): For sending notifications to staff.
*   **LookupReminderTemplates**: Stores templates for reminder messages (SMS, Email).
*   **LookupNotificationTypes**: Defines types of system notifications.
*   **FactCommunicationLogs**:  Logs of sent SMS/Email messages, including status and timestamps.
*   **AppSettings**: For storing default reminder settings, communication provider credentials (future).

**Instructions for AI Agent:**

*   Based on this overview and the database schema defined in `10d_Database_Tenant_Transactional_Schema.md`, `10e_Database_Tenant_Lookup_Schema.md`, and potentially new lookup/fact tables for communication logs, proceed to create the Backend API definitions in `82_CommunicationTools_Backend_API.md`.
*   Focus on creating API endpoints for:
    *   Managing reminder settings and templates.
    *   Managing notification types and configurations.
    *   Triggering and scheduling appointment reminders (initially manual trigger, then automated scheduling based on appointment time).
    *   Viewing communication logs.
    *   Potentially basic endpoints for sending test SMS/Emails (for configuration and testing purposes).
*   For now, assume simplified SMS/Email sending services in the backend services (no real integration with SMS gateways yet - focus on service structure and logging).
*   Ensure API design follows RESTful principles, uses appropriate HTTP methods and status codes, and includes authentication and validation.
*   After defining the API, generate the corresponding NestJS backend services in `83_CommunicationTools_Backend_Services.md`. These services will handle reminder scheduling, notification sending logic, communication logging, and interaction with simplified SMS/Email sending mechanisms.