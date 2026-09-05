# CIT Repair System

A campus/building facility repair-ticketing system built with PHP and MySQL. Users submit repair requests with photo evidence, technicians pick up jobs from a shared pool and upload proof of completion, and admins manage users and oversee all tickets.

## Features

- **Role-based accounts** — `user`, `technician`, and `admin` roles with separate views and permissions.
- **Ticket submission** — users log a repair request with room number, category, details, and an optional photo.
- **Job pool** — technicians browse and claim open tickets.
- **Ticket lifecycle** — tickets move through `pending` → `waiting_material` → `fixing` → `complete`.
- **Proof of work** — technicians can attach a photo as proof when resolving a ticket.
- **My Tickets** — users track the status of everything they've submitted.
- **Technician history** — technicians can view their completed job history.
- **Admin tools** — manage users (including banning) and oversee all tickets across the system.

## Tech Stack

- PHP (procedural)
- MySQL / MariaDB
- HTML/CSS (custom stylesheet in `assets/css`)

## Project Structure

```
├── index.php           # Landing / entry point
├── login.php            # User login
├── register.php         # New account registration
├── logout.php           # Session termination
├── create_ticket.php     # Submit a new repair ticket
├── my_tickets.php        # A user's own ticket history
├── job_pool.php          # Open tickets available for technicians to claim
├── tech_history.php      # A technician's completed job history
├── ticket_action.php     # Handles status updates / claiming / proof upload
├── admin_jobs.php        # Admin view of all tickets
├── admin_users.php       # Admin user management (roles, bans)
├── includes/             # Shared PHP includes (e.g. DB connection, helpers)
├── assets/css/           # Stylesheets
└── creating database.txt # SQL schema for setting up the database
```

## Database Schema

Two core tables (see `creating database.txt` for the full SQL):

- **`users`** — `id`, `username`, `email`, `password` (hashed), `role` (`user`/`technician`/`admin`), `is_banned`, `created_at`
- **`tickets`** — `id`, `user_id`, `technician_id`, `room_number`, `category`, `details`, `status`, `user_image`, `tech_proof_image`, `created_at`, `updated_at`

## Getting Started

### Prerequisites

- PHP 7.4+ with MySQL support
- MySQL or MariaDB

### Setup

1. **Clone the repo**
   ```bash
   git clone https://github.com/sanddie123/cit-repair-system.git
   cd cit-repair-system
   ```

2. **Create the database**
   Run the SQL in `creating database.txt` against your MySQL server:
   ```bash
   mysql -u root -p < "creating database.txt"
   ```
   This creates the `cit_fixit` database, the `users` and `tickets` tables, and seeds a default admin account.

3. **Configure the database connection**
   Update the connection settings in `includes/` (or wherever `db.php`/config lives) to match your local MySQL credentials.

4. **Serve the app**
   ```bash
   php -S localhost:8000
   ```
   Then open `http://localhost:8000` in your browser.

### Default Admin

The seed script creates a default admin account (`SuperAdmin`). **Change this password immediately** in production — see the comment in `creating database.txt`.

## Security Notes

- Passwords are stored hashed (bcrypt via PHP's `password_hash`).
- This project was built as a coursework/learning system — review input sanitization, session handling, and file-upload validation before using it in any real production environment.
