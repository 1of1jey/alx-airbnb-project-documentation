# User Stories - Airbnb Clone

## Overview
This document contains user stories derived from the use case diagram, representing key functionalities from the perspectives of different user roles.

---

## Authentication & Account Management

### US-001: User Registration
**As a** guest or host  
**I want to** register an account with my email and password  
**So that** I can access the platform and its features

**Acceptance Criteria:**
- User can sign up with email, password, first name, and last name
- Email must be unique and validated
- Password must meet security requirements (min 8 characters)
- User receives email verification
- User can choose role (guest/host) during registration

---

### US-002: User Login
**As a** registered user  
**I want to** log in to my account securely  
**So that** I can access my profile and use the platform

**Acceptance Criteria:**
- User can log in with email and password
- User can log in via OAuth (Google/Facebook)
- Invalid credentials show appropriate error message
- Successful login redirects to dashboard
- JWT token is generated for session management

---

### US-003: Profile Management
**As a** registered user  
**I want to** update my profile information  
**So that** my account details remain current and accurate

**Acceptance Criteria:**
- User can update name, phone number, and photo
- Changes are saved and reflected immediately
- User can view their booking/listing history
- Profile photo is uploaded to cloud storage

---

## Property Management (Host)

### US-004: Create Property Listing
**As a** host  
**I want to** create a property listing with details and photos  
**So that** I can rent out my property to guests

**Acceptance Criteria:**
- Host can add title, description, location, and price
- Host can select amenities and property type
- Host can upload multiple photos
- Host can set availability dates
- Listing is saved and visible to guests after creation

---

### US-005: Edit Property Listing
**As a** host  
**I want to** edit my property listing  
**So that** I can update information or correct mistakes

**Acceptance Criteria:**
- Host can modify all property details
- Host can add/remove photos
- Host can update pricing and availability
- Changes are saved and reflected immediately
- Existing bookings are not affected by price changes

---

### US-006: Delete Property Listing
**As a** host  
**I want to** delete my property listing  
**So that** I can remove properties I no longer want to rent

**Acceptance Criteria:**
- Host can delete listings without active bookings
- System prevents deletion if active bookings exist
- Deleted listings are removed from search results
- Host receives confirmation before deletion

---

## Search & Discovery (Guest)

### US-007: Search Properties
**As a** guest  
**I want to** search for properties by location and dates  
**So that** I can find available accommodations for my trip

**Acceptance Criteria:**
- Guest can search by city, state, or country
- Guest can specify check-in and check-out dates
- Guest can filter by number of guests
- Search returns only available properties
- Results show property photos, price, and rating

---

### US-008: Filter Search Results
**As a** guest  
**I want to** filter properties by price, amenities, and ratings  
**So that** I can find properties that match my preferences

**Acceptance Criteria:**
- Guest can set minimum and maximum price
- Guest can filter by amenities (Wi-Fi, pool, parking, etc.)
- Guest can filter by property type (apartment, house, villa)
- Guest can sort by price, rating, or distance
- Filters apply in real-time

---

### US-009: View Property Details
**As a** guest  
**I want to** view detailed information about a property  
**So that** I can make an informed booking decision

**Acceptance Criteria:**
- Guest can see all property details and descriptions
- Guest can view all property photos in a gallery
- Guest can see property location on a map
- Guest can view host information
- Guest can read reviews and ratings

---

## Booking System

### US-010: Create Booking
**As a** guest  
**I want to** book a property for specific dates  
**So that** I can secure accommodation for my trip

**Acceptance Criteria:**
- Guest can select check-in and check-out dates
- System validates property availability
- System prevents double-booking
- Guest can see total price calculation
- Guest can add special requests/notes
- Booking requires immediate payment

---

### US-011: Cancel Booking
**As a** guest  
**I want to** cancel my booking  
**So that** I can receive a refund based on the cancellation policy

**Acceptance Criteria:**
- Guest can cancel booking from booking details page
- System calculates refund based on policy
- Guest receives cancellation confirmation
- Host is notified of cancellation
- Refund is processed automatically

---

### US-012: Confirm/Reject Booking
**As a** host  
**I want to** confirm or reject booking requests  
**So that** I can control who stays at my property

**Acceptance Criteria:**
- Host receives notification of new booking request
- Host can view guest profile and booking details
- Host can accept or decline request
- Guest is notified of host's decision
- Payment is captured only after confirmation

---

### US-013: View Booking History
**As a** guest or host  
**I want to** view my booking history  
**So that** I can track past and upcoming reservations

**Acceptance Criteria:**
- User can see all bookings (past, current, upcoming)
- Bookings show status (pending, confirmed, canceled, completed)
- User can filter by date range and status
- User can view detailed information for each booking

---

## Payment Processing

### US-014: Make Payment
**As a** guest  
**I want to** pay for my booking securely  
**So that** I can confirm my reservation

**Acceptance Criteria:**
- Guest can pay with credit card or PayPal
- Payment is processed through secure gateway (Stripe)
- Guest receives payment confirmation
- Payment is linked to booking record
- Guest can download receipt

---

### US-015: Receive Payout
**As a** host  
**I want to** receive payment for completed bookings  
**So that** I can earn income from my property

**Acceptance Criteria:**
- Host receives payout after guest check-in
- Platform fee is automatically deducted
- Payout is sent to host's bank account or PayPal
- Host can view payout history
- Host receives payout confirmation

---

### US-016: Process Refund
**As a** guest  
**I want to** receive a refund for canceled bookings  
**So that** I can recover my payment based on the policy

**Acceptance Criteria:**
- Refund is calculated based on cancellation policy
- Refund is processed automatically
- Guest receives refund confirmation
- Refund appears in original payment method
- Processing time is communicated to guest

---

## Reviews & Ratings

### US-017: Write Review
**As a** guest  
**I want to** leave a review for a property after my stay  
**So that** I can share my experience with other users

**Acceptance Criteria:**
- Guest can only review after completed booking
- Guest can rate property from 1 to 5 stars
- Guest can write detailed comments
- Guest can upload photos (optional)
- Review is visible to all users after submission

---

### US-018: Respond to Review
**As a** host  
**I want to** respond to guest reviews  
**So that** I can address feedback and maintain my reputation

**Acceptance Criteria:**
- Host can reply to reviews of their properties
- Host can only respond once per review
- Response is displayed below the review
- Guest is notified of host's response

---

## Communication

### US-019: Send Message
**As a** guest or host  
**I want to** send direct messages to other users  
**So that** I can communicate about bookings and properties

**Acceptance Criteria:**
- User can send text messages in real-time
- Messages are organized by conversation
- User receives notification of new messages
- Message history is preserved
- Messages support booking-related inquiries

---

### US-020: Receive Notifications
**As a** user  
**I want to** receive notifications about bookings and messages  
**So that** I stay informed about important activities

**Acceptance Criteria:**
- User receives email notifications for key events
- User receives in-app notifications in real-time
- User can view notification history
- User can mark notifications as read
- Notifications include booking confirmations, cancellations, messages, and reviews

---

## Administration

### US-021: Manage Users
**As an** admin  
**I want to** manage user accounts  
**So that** I can maintain platform integrity

**Acceptance Criteria:**
- Admin can view all user accounts
- Admin can suspend or activate accounts
- Admin can search and filter users
- Admin can view user activity logs
- Admin can handle user reports

---

### US-022: Manage Properties
**As an** admin  
**I want to** oversee property listings  
**So that** I can ensure quality and compliance

**Acceptance Criteria:**
- Admin can view all property listings
- Admin can approve or reject new listings
- Admin can remove inappropriate listings
- Admin can feature properties on homepage
- Admin can view property statistics

---

### US-023: View Reports
**As an** admin  
**I want to** view system analytics and reports  
**So that** I can monitor platform performance

**Acceptance Criteria:**
- Admin can view user statistics
- Admin can see booking and revenue reports
- Admin can track property performance
- Admin can export reports as CSV/PDF
- Dashboard shows key metrics and trends

---

## Summary

**Total User Stories: 23**

- Authentication & Account Management: 3
- Property Management: 3
- Search & Discovery: 3
- Booking System: 4
- Payment Processing: 3
- Reviews & Ratings: 2
- Communication: 2
- Administration: 3

---

## Prioritization (MoSCoW Method)

### Must Have (MVP)
- US-001, US-002 (Authentication)
- US-004, US-009 (Property Management)
- US-007, US-010 (Search & Booking)
- US-014 (Payment)

### Should Have
- US-003 (Profile Management)
- US-005, US-006 (Edit/Delete Property)
- US-008, US-011, US-012 (Advanced Booking)
- US-015, US-016 (Payouts & Refunds)
- US-017, US-018 (Reviews)

### Could Have
- US-019, US-020 (Messaging & Notifications)
- US-013 (Booking History)

### Won't Have (Initial Release)
- US-021, US-022, US-023 (Admin Features)

---

## User Story Format

Each user story follows the standard format:
```
As a [role]
I want to [action]
So that [benefit]
```

With clear acceptance criteria defining "done".