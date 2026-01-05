# Connecting to the Database

## 💾 Connection Details

> [!info] Database Information
> ```
> Host: localhost (or provided by instructor)
> Port: 5432
> Database: contoso
> Username: postgres (or your assigned username)
> Password: (provided by instructor)
> ```

---

## 🔧 Connection Methods

### Method 1: Using pgAdmin (GUI)

**Step 1: Open pgAdmin**
- Launch pgAdmin 4 application

**Step 2: Create New Server Connection**
1. Right-click "Servers" → "Create" → "Server"
2. **General Tab:**
   - Name: `Contoso Retail`
3. **Connection Tab:**
   - Host: `localhost` (or instructor-provided)
   - Port: `5432`
   - Database: `contoso`
   - Username: Your username
   - Password: Your password
   - ✓ Save password

**Step 3: Test Connection**
- Click "Save"
- If successful, you'll see the server in the browser panel

---

### Method 2: Using psql (Command Line)

**MacOS/Linux:**
```bash
psql -h localhost -p 5432 -U postgres -d contoso
```

**Windows:**
```cmd
psql -h localhost -p 5432 -U postgres -d contoso
```

**You'll be prompted for password.**

---

### Method 3: Using DBeaver (Universal GUI)

**Step 1: Create New Connection**
1. Click "New Database Connection" (plug icon)
2. Select "PostgreSQL"
3. Click "Next"

**Step 2: Configure Connection**
- Host: `localhost`
- Port: `5432`
- Database: `contoso`
- Username: Your username
- Password: Your password
- ✓ Save password

**Step 3: Test & Connect**
- Click "Test Connection"
- If successful, click "Finish"

---

### Method 4: Using DataGrip (JetBrains)

**Step 1: Add Data Source**
1. File → New → Data Source → PostgreSQL
2. Enter connection details:
   - Host: `localhost`
   - Port: `5432`
   - Database: `contoso`
   - User: Your username
   - Password: Your password

**Step 2: Test Connection**
- Click "Test Connection"
- Download drivers if prompted
- Click "OK"

---

## ✅ Testing Your Connection

Once connected, run these queries to verify everything works:

### Test 1: List All Tables

```sql
-- Should show: customer, product, store, sales, orders, orderrows, currencyexchange, date
SELECT table_name 
FROM information_schema.tables 
WHERE table_schema = 'public'
ORDER BY table_name;
```

### Test 2: Check Row Counts

```sql
-- Verify you have data in all tables
SELECT 'customer' as table_name, COUNT(*) as row_count FROM customer
UNION ALL
SELECT 'product', COUNT(*) FROM product
UNION ALL
SELECT 'store', COUNT(*) FROM store
UNION ALL
SELECT 'sales', COUNT(*) FROM sales
UNION ALL
SELECT 'orders', COUNT(*) FROM orders
UNION ALL
SELECT 'orderrows', COUNT(*) FROM orderrows
UNION ALL
SELECT 'currencyexchange', COUNT(*) FROM currencyexchange
UNION ALL
SELECT 'date', COUNT(*) FROM date
ORDER BY table_name;
```

**Expected Results:**
```
table_name       | row_count
customer         | ~2,000,000
currencyexchange | ~50,000
date             | ~10,950
orders           | ~5,000,000
orderrows        | ~20,000,000
product          | ~2,517
sales            | ~20,000,000
store            | ~306
```

### Test 3: Sample Data from Each Table

```sql
-- Customer sample
SELECT * FROM customer LIMIT 5;

-- Product sample
SELECT * FROM product LIMIT 5;

-- Sales sample
SELECT * FROM sales LIMIT 5;

-- Store sample
SELECT * FROM store LIMIT 5;
```

### Test 4: Simple Query

```sql
-- Test a basic join query
SELECT 
    p.productname,
    p.categoryname,
    SUM(s.quantity) as units_sold,
    ROUND(SUM(s.quantity * s.netprice), 2) as revenue
FROM sales s
JOIN product p ON s.productkey = p.productkey
WHERE s.orderdate >= '2016-01-01' 
  AND s.orderdate < '2016-02-01'
GROUP BY p.productkey, p.productname, p.categoryname
ORDER BY revenue DESC
LIMIT 10;
```

If this query runs successfully, your connection is working perfectly!

---

## ⚠️ Common Connection Problems

### Problem 1: "Connection refused"

**Symptoms:**
```
psql: error: could not connect to server: Connection refused
```

**Solutions:**
- ✅ Check if PostgreSQL is running
- ✅ Verify the host and port are correct
- ✅ Check firewall settings
- ✅ Confirm database server is started

**Check if PostgreSQL is running:**

**MacOS:**
```bash
brew services list | grep postgresql
```

**Linux:**
```bash
sudo systemctl status postgresql
```

**Windows:**
```
Check Services → PostgreSQL should be "Running"
```

---

### Problem 2: "Database does not exist"

**Symptoms:**
```
FATAL: database "contoso" does not exist
```

**Solutions:**
- ✅ Verify database name spelling (case-sensitive!)
- ✅ Ask instructor if database has been created
- ✅ Check if you have access to the database

**List all databases:**
```sql
-- Connect to default 'postgres' database first
psql -h localhost -U postgres -d postgres

-- Then list databases
\l
```

---

### Problem 3: "Password authentication failed"

**Symptoms:**
```
FATAL: password authentication failed for user "postgres"
```

**Solutions:**
- ✅ Double-check password (case-sensitive!)
- ✅ Verify username is correct
- ✅ Clear saved passwords and re-enter
- ✅ Contact instructor for credential reset

---

### Problem 4: "Permission denied"

**Symptoms:**
```
ERROR: permission denied for table sales
```

**Solutions:**
- ✅ Verify you have SELECT permissions
- ✅ Check with instructor about user privileges
- ✅ Confirm you're using correct username

---

### Problem 5: "Too many connections"

**Symptoms:**
```
FATAL: sorry, too many clients already
```

**Solutions:**
- ✅ Close unused connections/windows
- ✅ Wait a few minutes and retry
- ✅ Contact instructor (server may need restart)

---

## 🔐 Security Best Practices

> [!warning] Important Security Notes
> - **Never share your password** with other students
> - **Don't write passwords** in your SQL files
> - **Use saved credentials** in your SQL client
> - **Log out** when done using shared computers

---

## 💡 Connection Tips

### Tip 1: Save Your Connection

Save connection details in your SQL client so you don't have to re-enter credentials every time.

### Tip 2: Use Multiple Connections Wisely

Most SQL clients allow multiple connections. You can:
- One window for exploring data
- One window for writing queries
- One window for testing

But be mindful of connection limits!

### Tip 3: Set Default Database

Configure your client to open the `contoso` database by default:

**pgAdmin:** Right-click server → Properties → Connection → Maintenance database: `contoso`

**DataGrip:** Edit connection → Schemas → Select `contoso` as default

### Tip 4: Create a Scratch Workspace

Create a new SQL file for practice queries:

```sql
-- scratch.sql
-- Quick tests and experiments

SELECT * FROM sales LIMIT 5;
SELECT * FROM customer LIMIT 5;

-- etc.
```

---

## 📝 Connection String Examples

If you need to connect programmatically:

### Python (psycopg2)
```python
import psycopg2

conn = psycopg2.connect(
    host="localhost",
    port=5432,
    database="contoso",
    user="postgres",
    password="your_password"
)

cursor = conn.cursor()
cursor.execute("SELECT COUNT(*) FROM sales")
print(cursor.fetchone())
```

### Python (SQLAlchemy)
```python
from sqlalchemy import create_engine

engine = create_engine('postgresql://postgres:your_password@localhost:5432/contoso')
```

### R (RPostgreSQL)
```r
library(RPostgreSQL)

drv <- dbDriver("PostgreSQL")
con <- dbConnect(drv, 
                 host="localhost",
                 port=5432,
                 dbname="contoso",
                 user="postgres",
                 password="your_password")
```

### Node.js (pg)
```javascript
const { Client } = require('pg')

const client = new Client({
  host: 'localhost',
  port: 5432,
  database: 'contoso',
  user: 'postgres',
  password: 'your_password',
})

await client.connect()
```

---

## 🆘 Getting Help

If you still can't connect:

1. **Check the error message carefully** - It usually tells you what's wrong
2. **Try the test queries above** - Helps isolate the problem
3. **Ask classmates** - They might have solved the same issue
4. **Contact instructor** - Provide the exact error message you're seeing

---

← [[00-Index|Back to Home]]