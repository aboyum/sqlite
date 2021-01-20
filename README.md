# As simple as it can be, but with uniqe name:
```sqlite
CREATE TABLE users(id integer primary key, name text, age integer);
CREATE UNIQUE INDEX idx_name ON users(name);
INSERT INTO users (name,age) VALUES("ida",65);
INSERT OR REPLACE INTO users (name,age) VALUES("ida",64);

SELECT * FROM users;
2|ida|64    (There is only one user in the list)
```

## Same example written in python
```python
import sqlite3
conn = sqlite3.connect('example.sqlite')

c = conn.cursor()

# Create table
c.execute('''CREATE TABLE users(id integer primary key, name text, age integer);
            CREATE UNIQUE INDEX idx_name ON users(name);''')

# Insert a row of data
c.execute("INSERT INTO users (name,age) VALUES("ida",65)")

# Change the row
c.execute("INSERT OR REPLACE INTO users (name,age) VALUES("ida",64);")

# Save (commit) the changes
conn.commit()

# Save info and print the table
placeholder = c.execute("SELECT * FROM users;")
print(placeholder)

# We can also close the connection if we are done with it.
# Just be sure any changes have been committed or they will be lost.
conn.close()
```

# Table where to colums makes a uniqnes
```sqlite
CREATE TABLE switches(id integer primary key, ip text, port_name text, port_description text);
CREATE UNIQUE INDEX idx_ipport ON switches(ip, port_name);
INSERT OR REPLACE INTO switches (ip,port_name,port_description) VALUES ("10.0.0.1", "gig1/0/1", "Uplink");
INSERT OR REPLACE INTO switches (ip,port_name,port_description) VALUES ("10.0.0.2", "gig1/0/1", "Uplink");

SELECT * FROM switches;
1|10.0.0.1|gig1/0/1|Uplink
2|10.0.0.2|gig1/0/1|Uplink

INSERT OR REPLACE INTO switches (ip,port_name,port_description) VALUES ("10.0.0.2", "gig1/0/1", "link to switch2");
SELECT * FROM switches;
1|10.0.0.1|gig1/0/1|Uplink
3|10.0.0.2|gig1/0/1|link to switch2
```
