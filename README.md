# e-commerce
Jumbotail Project of E-Commerce

from flask import Flask, request
from threading import Thread
from queue import Queue
import sqlite3

app = Flask(__name__)

eventQueue = Queue()

conn = sqlite3.connect('user_journey.db')
cursor = conn.cursor()

cursor.execute('''CREATE TABLE IF NOT EXISTS events
                  (id INTEGER PRIMARY KEY AUTOINCREMENT,
                  user_id INTEGER,
                  step INTEGER,
                  city TEXT,
                  timestamp DATETIME DEFAULT CURRENT_TIMESTAMP)''')

class DRIVER:
    def generateEvent(self, user):
        # Generate events for each user
        # Consider user behavior, event ratios, order of events, etc.
        # Implement batching for sending events
        # Log response times of API calls
        event = {
            'user_id': user,
            'step': 1,
            'city': 'City X'
        }
        return event

@app.route('/webhook', methods=['POST'])
def eventWebhook():
    events = request.get_json()
    for event in events:
        eventQueue.put(event)
    return "Success"

def consumeEvents():
    while True:
        events = []
        while not eventQueue.empty():
            events.append(eventQueue.get())
        
        for event in events:
            cursor.execute("INSERT INTO events (user_id, step, city) VALUES (?, ?, ?)",
                           (event['user_id'], event['step'], event['city']))
            conn.commit()

def calculateUserPercentage():
    total_users = cursor.execute("SELECT COUNT(DISTINCT user_id) FROM events").fetchone()[0]
    steps = [1, 2, 3, 4, 5, 6]
    for step in steps:
        cursor.execute("SELECT COUNT(*) FROM events WHERE step = ?", (step,))
        count = cursor.fetchone()[0]
        print(f"Percentage of users in step {step}: {count * 100 / total_users}%")

def evaluateCityPerformance():
    total_users = cursor.execute("SELECT COUNT(DISTINCT user_id) FROM events").fetchone()[0]
    cursor.execute("SELECT city, COUNT(*) FROM events GROUP BY city")
    city_data = cursor.fetchall()
    for city, count in city_data:
        print(f"City: {city}, Percentage of users: {count * 100 / total_users}%")

if __name__ == '__main__':
    consumerThread = Thread(target=consumeEvents)
    consumerThread.start()
    
    driver = DRIVER()
    for i in range(100):  # Simulate 100 users
        user = i + 1
        event = driver.generateEvent(user)
        eventQueue.put(event)
    
    app.run(host='localhost', port=8888)
