# import sqlite3

# establish connection
conn = sqlite3.connect('demo.db')

# used to execute SQL commands
cursor = conn.cursor()


# create 'Users' table
cursor.execute('''Create Table IF NOT EXISTS Users (
                  user_id INTEGER PRIMARY KEY,
                  username TEXT UNIQUE,
                  password TEXT,
                  created_at TIMESTAMP DEFUALT CURRENT_TIMESTAMP
                  )''')

# create'UserActivities' table
cursor.execute('''CREATE TABLE IF NOT EXISTS UserActivities (
                    activity_id INTEGER PRIMARY KEY,
                    activity TEXT,
                    activity_time TIMESTAMP DEFUALT CURRENT_TIMESTAMP,
                    user_id INTEGER,
                    FOREIGN KEY (user_id) REFERENCES Users(user_id)
  )''')

# create 'UserConnections' table
cursor.execute('''CREATE TABLE IF NOT EXISTS UserConnections (
                  connection_id INTEGER PRIMARY KEY,
                  user1_id INTEGER,
                  user2_id INTEGER,
                  connection_time TIMESTAMP DEFUALT CURRENT_TIMESTAMP,
                  FOREIGN KEY (user1_id) REFERENCES Users(user_id),
                  FOREIGN KEY (user2_id) References Users(user_id)
  )''')

# create indexes for data retrieval
cursor.execute("CREATE INDEX IF NOT EXISTS idx_user_id ON UserActivities(user_id)")
cursor.execute("CREATE INDEX IF NOT EXISTS idx_user1_user2 ON UserConnections(user1_id, user2_id)")

# commit (save) change
conn.commit()

# add (insert) data into Users table
#cursor.execute("INSERT INTO Users (username, email, password) VALUES (?, ?, ?)", ('alice', 'alice@example.com', 'password123'))
#cursor.execute("INSERT INTO Users (username, email, password) VALUES (?, ?, ?)", ('bob', 'bob@example.com', 'secret123'))

#add (insert) data into UserActivities table
cursor.execute("INSERT INTO UserActivities (user_id, activity) VALUES (?, ?)", (1, 'Logged in'))
cursor.execute("INSERT INTO UserActivities (user_id, activity) VALUES (?, ?)", (2, 'Posted a comment'))

#add (insert) data into UserConnections table
cursor.execute("INSERT INTO UserConnections (user1_id, user2_id) VALUES (?, ?)", (1, 2))
cursor.execute("INSERT INTO UserConnections (user1_id, user2_id) VALUES (?, ?)", (2,1))

# commit (save) changes
conn.commit()

# query and print data from the Users table
print("Users:")
cursor.execute("SELECT * FROM Users")
for row in cursor.fetchall():
   print(row)

# query and print data from the UserActivities table
print("\nUser Activities:")
cursor.execute("SELECT * FROM UserActivities")
for row in cursor.fetchall():
  print(row)

# query and print data from the UserConnections table
print("\nUser Connections:")
cursor.execute("SELECT * FROM UserConnections")
for row in cursor.fetchall():
    print(row)

   print(row)

# close the database connections
conn.close()
