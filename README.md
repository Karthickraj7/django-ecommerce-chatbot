# My Store – React + Django E-commerce

A full-stack e-commerce project with a **Django REST API** backend and a **React** frontend.

> **Status:** The backend (products, cart, orders, coupons, refunds, Stripe payment code, token login) is in place. The React frontend currently has Home, Product List, Login and Signup pages. Checkout and payment screens are not built in React yet. See the [Roadmap](#roadmap).

---

## Features

**Backend (Django)**
- Token-based login and registration (`django-rest-auth` + `django-allauth`)
- Product list API
- Add to cart API and order summary API
- Models for items, orders, addresses, payments, coupons, refunds and user profiles
- Stripe payment code and coupon / refund logic (server-rendered views in `core/views.py`, not yet connected in `home/urls.py`)
- Django admin to manage products, orders, coupons and refunds
- Separate dev and production settings

**Frontend (React)**
- React 16, Redux (with thunk), React Router 5
- Semantic UI for styling
- Login, Signup, Home and Product List pages
- Auth and cart state stored in Redux

## Tech Stack

| Part | Technology |
|------|------------|
| Backend | Python, Django 3.2, Django REST Framework |
| Auth | django-rest-auth, django-allauth, token authentication |
| Payments | Stripe |
| Database | SQLite (development), PostgreSQL (production) |
| Frontend | React, Redux, React Router, Axios, Semantic UI |

## Project Structure

```
my-store/
├── manage.py
├── requirements.txt          # Python packages
├── package.json              # JavaScript packages
├── home/                     # Django project
│   ├── settings/
│   │   ├── base.py           # Shared settings
│   │   ├── dev.py            # Development settings (used by default)
│   │   └── prod.py           # Production settings
│   └── urls.py
├── core/                     # Main Django app
│   ├── models.py             # Database models
│   ├── api/                  # REST API (views, serializers, urls)
│   ├── views.py              # Server-rendered views (checkout, payment, coupons)
│   ├── admin.py
│   └── migrations/
├── media/                    # Uploaded product images
├── public/                   # React public files
└── src/                      # React source code
    ├── containers/           # Pages (Home, Login, Signup, ProductList)
    ├── store/                # Redux actions and reducers
    ├── routes.js
    └── constants.js          # API URLs
```

## Getting Started

### Requirements
- Python 3.9 or newer
- Node.js and npm

### 1. Backend

```bash
# Create and activate a virtual environment
python -m venv env
env\Scripts\activate           # Windows
source env/bin/activate        # Mac / Linux

# Install packages
pip install -r requirements.txt

# Create the database and an admin account
python manage.py migrate
python manage.py createsuperuser

# Run the server
python manage.py runserver
```

The API runs at **http://127.0.0.1:8000**. The admin panel is at **http://127.0.0.1:8000/admin/**. Log in there and add some products (Items) so the frontend has something to show.

### 2. Frontend

Open a second terminal in the same folder:

```bash
npm install
npm start
```

The app opens at **http://localhost:3000**.

> If `npm start` fails with an `ERR_OSSL_EVP_UNSUPPORTED` error on newer Node.js versions, run it with `NODE_OPTIONS=--openssl-legacy-provider` (Mac/Linux: `NODE_OPTIONS=--openssl-legacy-provider npm start`, Windows PowerShell: `$env:NODE_OPTIONS="--openssl-legacy-provider"; npm start`).

### 3. Environment variables (recommended)

| Variable | Purpose |
|----------|---------|
| `DJANGO_SECRET_KEY` | Django secret key (use a long random value in production) |
| `STRIPE_SECRET_KEY` | Your Stripe secret key, needed for payments |

## API Endpoints

| Method | URL | Description | Login needed |
|--------|-----|-------------|--------------|
| GET | `/api/product-list/` | List all products | No |
| POST | `/api/add-to-cart/` | Add a product to the cart (send `slug`) | Yes |
| GET | `/api/order-summary/` | Current cart | Yes |
| POST | `/rest-auth/login/` | Log in, returns a token | No |
| POST | `/rest-auth/registration/` | Sign up | No |

Send the token in the header: `Authorization: Token <your-token>`

## Deployment

```bash
npm run build
```

Production uses `home/settings/prod.py`. Before deploying, set `DJANGO_SETTINGS_MODULE=home.settings.prod`, replace the database, domain and secret key placeholders in that file, and set `ALLOWED_HOSTS`. Note that `prod.py` uses `whitenoise` for static files, which is not in `requirements.txt` yet, so install and configure it first.

## Roadmap

- [ ] Product detail page in React
- [ ] Cart page in React
- [ ] Checkout and Stripe payment pages in React
- [ ] Connect coupon and refund features to the REST API
- [ ] Order history for customers
- [ ] Search and category filters

## Known Issues / Before Going Live

- The API default permission is `AllowAny`; restrict endpoints that should need login.
- The frontend has `http://127.0.0.1:8000` hard-coded in `src/constants.js` and `src/store/actions/auth.js`. Move it to one setting before deploying.
- Prices are stored as floats; consider `DecimalField` for money.
- Dependency versions are fairly old (React 16, `react-scripts` 3). Plan upgrades for a real product.

## Credits

Built with Django, Django REST Framework, React, Redux and Semantic UI.

## License

Add your own license here (for example MIT).
