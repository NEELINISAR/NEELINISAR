# BookBazaar — Book Catalogue E-Commerce System

A complete Django e-commerce app for buying and selling books online.

## Features

- **Two User Roles**: Buyer (browse, buy, review) and Seller (list, manage, track sales)
- **Book Catalogue**: Search, filter by category/price/condition, sort, live autocomplete
- **Shopping Cart**: AJAX-powered cart with quantity controls and toast notifications
- **Checkout & Orders**: Address form, payment method selection, order tracking
- **Seller Dashboard**: Stats cards, book listings CRUD, sales report
- **Reviews**: Star ratings, one review per user per book
- **Admin Panel**: Customized Django Admin for all models
- **Responsive Design**: Works on desktop, tablet, and mobile

## Quick Start

### 1. Navigate to project directory

```cd book_catalogue_ecommerce

```

### 2. Create and activate virtual environment

```
python -m venv venv
venv\Scripts\activate        # Windows
source venv/bin/activate     # Mac/Linux
```

### 3. Install dependencies

```
pip install -r requirements.txt
```

### 4. Run migrations

```
python manage.py makemigrations accounts
python manage.py makemigrations catalogue
python manage.py makemigrations store
python manage.py makemigrations seller
python manage.py migrate
```

### 5. Load sample data (optional but recommended)

```
python manage.py populate_data
```

This creates:

- 10 categories, 20 authors, 50 books
- 3 sellers (seller1@bookbazaar.com — password: seller123)
- 5 buyers (buyer1@bookbazaar.com — password: buyer123)
- 1 admin (admin@bookbazaar.com — password: admin123)

### 6. Run the development server

```
<!-- Load your custom tags at the top of the HTML file -->
{% load currency_tags %}

<!-- Use the filter and add the Rupee symbol -->
<p class="price">₹{{ book.price|usd_to_inr }}</p>
from django import template

register = template.Library()

@register.filter(name='usd_to_inr')
def usd_to_inr(value):
    """Converts USD to INR. Adjust the exchange rate as needed."""
    exchange_rate = 83.00
    try:
        converted_value = float(value) * exchange_rate
        return f"{converted_value:.2f}"
    except (ValueError, TypeError):
        return value
<!-- Change this -->
<span class="price">${{ book.price }}</span>

<!-- To this -->
<span class="price">₹{{ book.price }}</span>
<img src="{{ book.display_image }}" alt="{{ book.title }}">
from django.db import models

class Book(models.Model):
    # Assuming you already have these fields:
    title = models.CharField(max_length=255)
    price = models.DecimalField(max_digits=10, decimal_places=2)
    cover_image = models.ImageField(upload_to='books/covers/', blank=True, null=True)
    # ... other fields ...

    @property
    def display_image(self):
        """
        Returns the cover image if it exists.
        Otherwise, returns an alphabet image based on the first letter of the title.
        """
        if self.cover_image:
            return self.cover_image.url

        if self.title:
            first_letter = self.title[0].upper()
            if first_letter.isalpha():
                # Make sure these images exist in your static folder!
                return f'/static/images/alphabets/{first_letter}.png'

        # A fallback just in case the title starts with a number or symbol
        return '/static/images/default_book.png'

```

### 7. Open in browser

- Site: http://127.0.0.1:8000/
- Admin: http://127.0.0.1:8000/admin/

## Default Credentials (after populate_data)

| Role   | Email                  | Password  |
| ------ | ---------------------- | --------- |
| Admin  | admin@bookbazaar.com   | admin123  |
| Seller | seller1@bookbazaar.com | seller123 |
| Buyer  | buyer1@bookbazaar.com  | buyer123  |

## URL Structure

| URL                       | Description              |
| ------------------------- | ------------------------ |
| `/`                       | Homepage                 |
| `/catalogue/`             | All books (with filters) |
| `/catalogue/book/<slug>/` | Book detail              |
| `/catalogue/categories/`  | All categories           |
| `/catalogue/search/`      | Search results           |
| `/cart/`                  | Shopping cart            |
| `/cart/checkout/`         | Checkout                 |
| `/cart/orders/`           | Order history            |
| `/accounts/register/`     | Register                 |
| `/accounts/login/`        | Login                    |
| `/accounts/dashboard/`    | User dashboard           |
| `/seller/dashboard/`      | Seller dashboard         |
| `/seller/books/add/`      | Add new book             |
| `/admin/`                 | Django admin             |
