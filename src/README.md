# Mergington High School Activities API

A simple FastAPI application that allows students to view extracurricular
activities and teachers to manage student enrollment.

## Features

- View all available extracurricular activities
- View activity participants without logging in
- Require teacher login to register or unregister students

## Getting Started

1. Install the dependencies:

   ```
   pip install -r requirements.txt
   ```

2. Set teacher credentials in environment variables. Do not store passwords in
   source control:

   ```
   export TEACHER_USERNAME=teacher
   export TEACHER_PASSWORD='choose-a-strong-password'
   ```

3. From the repository root, run the application:

   ```
   uvicorn app:app --app-dir src --reload
   ```

   Set `COOKIE_SECURE=true` when serving the app over HTTPS. It defaults to
   `false` for local HTTP development.

4. Open http://localhost:8000.

## API Endpoints

| Method | Endpoint                                                          | Description                                                         |
| ------ | ----------------------------------------------------------------- | ------------------------------------------------------------------- |
| GET    | `/activities`                                                     | Get all activities and current participants (public)                |
| POST   | `/auth/login`                                                     | Log in with configured teacher credentials                          |
| GET    | `/auth/me`                                                        | Check the current teacher session                                   |
| POST   | `/auth/logout`                                                    | End the current teacher session                                     |
| POST   | `/activities/{activity_name}/signup?email=student@mergington.edu` | Register a student (teacher login required)                         |
| DELETE | `/activities/{activity_name}/unregister?email=student@mergington.edu` | Unregister a student (teacher login required)                    |

## Data Model

The application uses a simple data model with meaningful identifiers:

1. **Activities** - Uses activity name as identifier:

   - Description
   - Schedule
   - Maximum number of participants allowed
   - List of student emails who are signed up

2. **Students** - Uses email as identifier:
   - Name
   - Grade level

Activity data and teacher sessions are stored in memory, which means they
reset when the server restarts. Teacher credentials are supplied through
environment variables and are never stored in the repository. Sessions use
an HttpOnly, SameSite cookie and expire after eight hours.
