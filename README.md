# FLASK INTERCOMM SYSTEM
Flask Intercom SystemA minimal but secure, real-time group voice communication web application built with Flask. This system uses Google Sign-In for authentication, enforces admin controls, and includes essential security features for a robust and scalable intercom solution. It is designed to be simple to deploy and manage, with all significant events logged for administrative oversight.FeaturesAuthentication:Google Sign-In Only: Secure, passwordless authentication using Google OAuth 2.0.User Auto-Registration: New users are automatically added to the database upon their first successful login.Admin Role: A designated admin user (configured via email) has access to special administrative functions.User Limit: The system enforces a configurable maximum number of registered users (default is 50) to control access.Admin Controls (/admin):User Management: View a complete list of all registered users.Modify Permissions: Promote or demote users to/from admin status.Remove Users: Securely delete users from the database.Activity Dashboard: View detailed logs of all significant system and user actions.Protected Route: The admin panel is only accessible to users with is_admin status.Voice Communication:Real-Time Voice: WebRTC-powered, browser-to-browser voice communication.Signalling Server: Flask-SocketIO manages the signalling required to establish peer connections.Single Voice Room: A shared "main room" for all authenticated users to join and communicate.Basic Controls: Users can join, leave, mute, and unmute their audio stream.Security:Server-Side Token Validation: Google ID tokens are verified on the backend for every request to protected routes.Login CAPTCHA: A simple, math-based CAPTCHA ("What is X + Y?") prevents automated login attempts.Rate Limiting: Protects sensitive endpoints (like login and admin actions) from brute-force attacks.Secure Sessions: Uses secure, server-side cookies and implements session timeouts for inactivity.Standard Web Protections: Includes CSRF protection for all forms and basic measures against XSS attacks.Database & Logging:Backend: Uses SQLite for simplicity and ease of setup.User & Log Tables: A clear schema to store user profiles and detailed activity logs.Comprehensive Logging: Logs all critical events, including logins, logouts, voice channel activity, admin actions, and blocked login attempts due to user limits.Architecture & TechnologiesThis application follows a modular structure to separate concerns and improve maintainability.Backend: FlaskReal-Time Communication: Flask-SocketIOPeer-to-Peer Connection: WebRTCDatabase: SQLite with Flask-SQLAlchemyAuthentication: Google OAuth 2.0 (via google-auth-library)Frontend: Jinja2 Templates with Bootstrap for styling.Database Schemausers TableStores user profile information obtained from Google Sign-In.ColumnTypeConstraintsDescriptionidIntegerPrimary Key, AutoincrementUnique identifier for each user.google_idStringUnique, Not NullThe user's unique Google ID.emailStringUnique, Not NullThe user's Google account email.display_nameStringNot NullThe user's display name from Google.is_adminBooleanNot Null, Default: FalseTrue if the user is an administrator.date_joinedDateTimeNot Null, Default: CURRENT_TIMESTAMPTimestamp of the user's first login.logs TableRecords all significant actions performed within the application.ColumnTypeConstraintsDescriptionidIntegerPrimary Key, AutoincrementUnique identifier for each log entry.timestampDateTimeNot Null, Default: CURRENT_TIMESTAMPThe time the event occurred.user_idIntegerForeign Key (users.id)The user who performed the action. Can be NULL for system events.actionStringNot NullA short description of the action (e.g., "User Login", "Admin Delete User").detailsTextNullableAdditional details about the event (e.g., "Target User ID: 15").Installation and SetupFollow these steps to get the application running locally.1. PrerequisitesPython 3.8+A Google Cloud Platform project with OAuth 2.0 credentials enabled.2. Clone the Repositorygit clone https://github.com/your-username/flask-intercom-system.git
cd flask-intercom-system
3. Set Up a Virtual Environment# For macOS/Linux
python3 -m venv venv
source venv/bin/activate

# For Windows
python -m venv venv
.\venv\Scripts\activate
4. Install Dependenciespip install -r requirements.txt
5. Configure Environment VariablesCreate a .env file in the root directory and add the following configuration. Do not commit this file to version control.# Flask Configuration
SECRET_KEY='a_very_strong_random_secret_key_here'
FLASK_ENV='development'

# Google OAuth Credentials
GOOGLE_CLIENT_ID='your_google_client_id.apps.googleusercontent.com'
GOOGLE_CLIENT_SECRET='your_google_client_secret'

# Application Configuration
ADMIN_EMAIL='your_admin_email@gmail.com'
MAX_USERS=50
6. Initialise the DatabaseRun the following command to create the SQLite database and tables:flask db init
flask db migrate -m "Initial migration"
flask db upgrade
Alternatively, you can run a custom script:python init_db.py
Running the ApplicationOnce the setup is complete, start the Flask development server:python app.py
The application will be available at http://127.0.0.1:5000.Project Structure/flask-intercom-system
|
├── app/
|   ├── __init__.py         # Application factory, initialises extensions
|   ├── models.py           # SQLAlchemy database models (User, Log)
|   ├── auth.py             # Authentication routes (Google Sign-In, logout)
|   ├── admin.py            # Admin dashboard routes and logic
|   ├── main.py             # Main application routes (voice room)
|   └── static/
|       ├── css/
|       └── js/               # JavaScript for WebRTC and Socket.IO
|   └── templates/
|       ├── index.html        # Main voice room page
|       ├── login.html        # Login page with CAPTCHA
|       ├── admin.html        # Admin dashboard
|       └── _base.html        # Base template
|
├── migrations/             # Flask-Migrate files
|
├── .env                    # Environment variables (DO NOT COMMIT)
├── .gitignore              # Git ignore file
├── app.py                  # Main entry point to run the application
├── config.py               # Configuration settings
├── requirements.txt        # Python dependencies
└── README.md               # This file
Future EnhancementsMultiple Voice Rooms: Allow users to create and join different voice channels.Database Upgrade: Migrate from SQLite to a more robust database like PostgreSQL for production environments.Advanced Admin Features: Add features like searching/filtering logs and viewing user-specific activity.Containerisation: Add a Dockerfile for easy deployment with Docker.Improved UI/UX: Enhance the frontend with a more modern design and better user feedback.
