# sonweb-flask

A Flask web application with Tailwind CSS for 241-152 BASICAI-2568. Features user authentication, role-based access control (ACL), and a responsive UI.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Project Setup](#project-setup)
- [Configuration](#configuration)
- [Running the Application](#running-the-application)
- [Initialize Admin User](#initialize-admin-user)
- [Project Structure](#project-structure)

## Prerequisites

- **Python 3.8 or higher** installed on your system
- **pip** (Python package manager)
- **Node.js and npm** (for Tailwind CSS build tools)
  - Download from [nodejs.org](https://nodejs.org/)
  - Recommended: LTS version (18.x or later)
  - Verify installation: `node --version` and `npm --version`

## Project Setup

### 1. Clone or Navigate to the Project
```
git clone https://github.com/Pikcolo/sonweb-flask.git
```

### 2. Create and Activate Virtual Environment

**On Windows:**
```
python -m venv venv
.\venv\Scripts\Activate
```
Or
```
py -<version> -m venv venv
venv\Scripts\activate
```

**On macOS/Linux:**
```bash
python3 -m venv venv
source venv/bin/activate
```

### 3. Install Python Dependencies

```bash
cd my-flask-app
pip install -r requirements.txt
```

### 4. Install Frontend Dependencies (Tailwind CSS v4 + DaisyUI)

Navigate to the static directory and set up Tailwind CSS with DaisyUI:

```bash
cd my-flask-app/static
```

#### 4.1 Initialize npm and Create package.json

```bash
npm init -y
```

#### 4.2 Install Tailwind CSS (CLI) and DaisyUI

```bash
npm install -D tailwindcss @tailwindcss/cli daisyui
```

## Configuration

### 1. Configure Tailwind CSS and DaisyUI

#### 1.1 Create input.css

Create or update `my-flask-app/static/src/input.css` with:

```css
/* static/src/input.css */
@import "tailwindcss";
@plugin "daisyui";

/* Tell Tailwind to scan for classes in templates */
@source "../../templates";

/* (Optional) DaisyUI Theme Configuration */
@theme {
  --font-sans: "Inter", sans-serif;
}
```

#### 1.2 Update package.json Scripts

Modify the `scripts` section in `my-flask-app/static/package.json`:

```json
"scripts": {
  "build": "tailwindcss -i ./src/input.css -o ./css/output.css --minify",
  "tw:watch": "tailwindcss -i ./src/input.css -o ./css/output.css --watch"
}
```

### 2. Create Environment File

Copy the sample environment file and configure it:

```bash
copy .env.sample .env
```

Or manually create `.env` in the `my-flask-app` directory with:

```env
FLASK_ENV=development
SECRET_KEY=your_secret_key_here
SQLALCHEMY_DATABASE_URI=sqlite:///database.db
```

**Important Settings:**
- `SECRET_KEY`: Change this to a secure random string for production
- `SQLALCHEMY_DATABASE_URI`: SQLite is fine for development; use a production database for deployment
- `FLASK_ENV`: Set to `production` when deploying

**Note:** Place the `.env` file in the `my-flask-app` directory (same level as `app.py`)

## Running the Application

### 1. Start the Flask Development Server

From the `my-flask-app` directory with the virtual environment activated:

```bash
python app.py
```

The application will be available at: **http://localhost:5000**

### 2. Build Tailwind CSS (Watch Mode for Development)

In a separate terminal, from the `my-flask-app/static` directory:

```bash
npm run tw:watch
```

This will automatically recompile CSS whenever you modify HTML templates or create new CSS classes.


## Initialize Admin User

Before first use, create an initial admin user for authentication:

```bash
# From my-flask-app directory
python scripts/init-admin.py
```

**Default Admin Credentials:**
- Username: `admin`
- Email: `admin@gmail.com`
- Password: `123456`

**Important:** Change these credentials after first login!

## Project Structure

```
sonweb-flask/
├── my-flask-app/                 # Main Flask application
│   ├── app.py                    # Flask app factory and entry point
│   ├── models.py                 # Database models (User, etc.)
│   ├── forms.py                  # WTForms forms
│   ├── acl.py                    # Access Control List (authentication)
│   ├── requirements.txt          # Python dependencies
│   ├── .env.sample              # Environment variables template
│   ├── scripts/
│   │   └── init-admin.py         # Admin user initialization script
│   ├── views/                    # Flask blueprints
│   │   ├── __init__.py           # Blueprint registration
│   │   ├── main.py               # Main routes
│   │   └── accounts.py           # Authentication routes
│   ├── templates/                # HTML templates
│   │   ├── base.html             # Base template
│   │   ├── main/
│   │   │   └── index.html        # Home page
│   │   └── accounts/
│   │       ├── login.html        # Login form
│   │       ├── register.html     # Registration form
│   │       ├── users.html        # Users management
│   │       └── edit_roles.html   # Role editor
│   ├── static/                   # Static files
│   │   ├── css/
│   │   │   └── output.css        # Compiled Tailwind CSS
│   │   ├── src/
│   │   │   └── input.css         # Tailwind CSS source
│   │   ├── package.json
│   │   └── tailwind.config.js
│   └── instance/                 # Instance-specific files (not in repo)
└── README.md                     # This file
```

## Dependencies

The main Python dependencies are:

- **flask**: Web framework
- **flask-sqlalchemy**: ORM database management
- **flask-wtf**: Form handling with CSRF protection
- **flask-login**: User session management
- **email-validator**: Email validation for forms
- **python-dotenv**: Environment variable management

## Troubleshooting

### Port Already in Use

If port 5000 is already in use, modify `app.py`:
```python
app.run(debug=True, port=5001)
```

### Database Issues

To reset the database, delete `instance/database.db` and restart the app.

### Template Not Found Errors

Ensure you're running the app from the `my-flask-app` directory.

### Tailwind CSS Not Working

Rebuild CSS by running `npm run build` in the `static` directory.

## Development Tips

- Use `debug=True` in development for hot-reloading
- Check the console logs for helpful error messages
- Use Flask shell for database debugging: `flask shell`
- Modify templates in `templates/` directory for layout changes

---

