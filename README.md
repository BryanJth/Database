# BRWSC Hardware Store GUI (SQLite + Python Jupyter)

An interactive **hardware store** mini‑system built with **Python (Jupyter/Colab)** and **SQLite**.  
The UI is implemented using **ipywidgets + HTML/CSS**, supporting **Customer** and **Admin** flows: login/register, browse products, create orders, and manage inventory.

> Note: This is an academic/demo project (not production-ready). Passwords are stored in plain text in the SQLite DB.

---

## Highlights
- **Role-based access**: Customer vs Admin login
- **Customer features**:
  - Registration
  - Browse/search products
  - Cart & checkout
  - Payment page (E‑Wallet / Bank Transfer via QR + card-form options)
  - Profile page
- **Admin features**:
  - Add / delete products
  - View & search order database
  - View product database

---

## Demo Screenshots

![BRWSC Hardware Store GUI](BRWSC%20Hardware%20Store%20GUI.jpg)

---

## Tech Stack
- **Python**: `sqlite3`, `pandas`, `ipywidgets`, `IPython.display`
- **Utilities**: `qrcode`, `Pillow (PIL)`, `re`, `random`, `datetime`

---

## Database Overview (SQLite)

Database file in this repo:
- `penjualan_new(2) (1).db`

Dummy data (from the report):
- `customers`: 80 rows
- `product`: 30 rows
- `orders`: 150 rows
- `order_details`: 476 rows
- `order_payments`: 150 rows

### Tables & columns (from the `.db` file)

#### `admin`
| column | type | notes |
|---|---|---|
| `admin_id` | TEXT | Primary key |
| `username` | TEXT |  |
| `email` | TEXT | used for login |
| `password` | TEXT | plain text (demo) |

#### `customers`
| column | type | notes |
|---|---|---|
| `customer_id` | TEXT | Primary key (e.g., `C001`) |
| `name_cust` | TEXT | customer name |
| `email` | TEXT | used for login |
| `phone_number` | TEXT |  |
| `address` | TEXT |  |
| `password` | TEXT | plain text (demo) |

#### `product`
| column | type | notes |
|---|---|---|
| `product_id` | TEXT | Primary key |
| `product_name` | TEXT |  |
| `product_description` | TEXT |  |
| `price` | INTEGER | code sometimes treats this as “thousands” (×1000 for display) |
| `category` | TEXT |  |
| `stock` | INTEGER |  |
| `rating_product` | REAL | 1–5 scale |

#### `orders`
| column | type | notes |
|---|---|---|
| `order_id` | TEXT | Primary key |
| `customer_id` | TEXT | customer reference (logical FK) |
| `order_date` | TEXT |  |
| `status` | TEXT | code uses `pending` → `proceed` |

#### `order_details`
| column | type | notes |
|---|---|---|
| `order_detail_id` | TEXT | Primary key |
| `order_id` | TEXT | order reference (logical FK) |
| `product_id` | TEXT | product reference (logical FK) |
| `quantity` | INTEGER |  |
| `price_total` | INTEGER | quantity × price |

#### `order_payments`
| column | type | notes |
|---|---|---|
| `payment_id` | TEXT | Primary key |
| `order_id` | TEXT | order reference (logical FK) |
| `payment_type` | TEXT | `e-wallet`, `bank_transfer`, `debit_card`, `credit_card` |
| `payment_date` | TEXT |  |
| `payment_status` | TEXT | code uses `pending` → `proceed` |
| `amout_paid` | REAL | amount paid (typo preserved from DB: `amout_paid`) |

---

## Repository Contents
Current files (as uploaded):
- `GUI CODE (1).ipynb` — main notebook (UI + logic)
- `penjualan_new(2) (1).db` — SQLite database (dummy data)
- `BRWSC Hardware Store GUI.jpg` — UI walkthrough image
- `Makalah.pdf` — report (DB design + ERD)
- `Presentation.pdf` — slides

---

## How to Run

### Option A — Google Colab (recommended)
1. Upload this repo (or the notebook + `.db`) to Colab.
2. Make sure the database file is available in the runtime:
   - Either upload the `.db` to Colab
   - Or rename it to `penjualan_new.db`
3. Open the notebook: `GUI CODE (1).ipynb`
4. Update DB path in `init_db()` if needed:

```python
# inside init_db()
conn = sqlite3.connect('/content/penjualan_new.db')   # Colab default path
```

If your DB file name differs, change it accordingly.

5. Run all cells. The UI should appear (ipywidgets based).

### Option B — Local Jupyter
1. Create a Python env (recommended: Python 3.9+).
2. Install deps:
```bash
pip install pandas ipywidgets qrcode pillow
```
3. Make sure the database file is in the same folder as the notebook **or** update the path inside `init_db()`:
```python
conn = sqlite3.connect('./penjualan_new.db')
```
4. Run the notebook in Jupyter.

> If widgets don’t show, ensure ipywidgets is enabled (JupyterLab typically works out-of-the-box after installing `ipywidgets`).

---

## User Flow (Quick Guide)

### 1) Login
- Input **email + password**
- Choose role: **Customer** or **Admin**
- System validates credentials against SQLite tables (`customers` / `admin`)

### 2) Customer
- **Register** → saved into `customers`
- **Browse/Search products** → reads from `product`
- **Cart & checkout** → writes to:
  - `orders`
  - `order_details`
  - `order_payments`
- **Payment**:
  - E‑Wallet / Bank Transfer: shows **QR code**
  - Debit/Credit: simple form input (demo)
  - On “confirm”, `payment_status` and order `status` are updated

### 3) Admin
- **Product Management**: add/delete items in `product`
- **Order Database**: search and view orders + details + payments

---

## Documentation
- Report: `Makalah.pdf` (database design, entities, ERD, relationships)
- Slides: `Presentation.pdf` (feature walkthrough & implementation notes)

---

## Notes / Limitations
- This is a **demo/academic** system (no real payment gateway).
- Passwords are stored in plain text (not secure).
- Some naming in DB is preserved as-is (e.g., `amout_paid` typo).
- Status values in code are simplified (`pending`/`proceed`) and can be extended.

