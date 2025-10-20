Overview
This document outlines all the key features and functionalities required for the Airbnb Clone backend system. The system is designed to support a scalable, secure, and robust rental marketplace platform.

1. User Management
1.1 User Registration
Functionality: Allow users to create accounts as guests or hosts
Features:

Email and password registration
Password strength validation
Email verification
Secure password hashing (bcrypt)
JWT token generation upon successful registration
Role assignment (guest/host)

API Endpoints:

POST /api/auth/register

Database Tables: User
