# Flask-task4
Create a REST API that manages user data.
from flask import Flask, request, jsonify

app = Flask(__name__)

# In-memory user storage
users = {}

# Home route
@app.route('/')
def home():
    return "Welcome to the User Management API!"

# Get all users
@app.route('/users', methods=['GET'])
def get_users():
    return jsonify(users), 200

# Get a single user by ID
@app.route('/users/<user_id>', methods=['GET'])
def get_user(user_id):
    user = users.get(user_id)
    if user:
        return jsonify(user), 200
    return jsonify({"error": "User not found"}), 404

# Create a new user
@app.route('/users', methods=['POST'])
def create_user():
    data = request.get_json()
    user_id = data.get("id")
    if user_id in users:
        return jsonify({"error": "User already exists"}), 400
    users[user_id] = data
    return jsonify({"message": "User created"}), 201

# Update user details
@app.route('/users/<user_id>', methods=['PUT'])
def update_user(user_id):
    data = request.get_json()
    if user_id not in users:
        return jsonify({"error": "User not found"}), 404
    users[user_id].update(data)
    return jsonify({"message": "User updated"}), 200

# Delete a user
@app.route('/users/<user_id>', methods=['DELETE'])
def delete_user(user_id):
    if user_id in users:
        del users[user_id]
        return jsonify({"message": "User deleted"}), 200
    return jsonify({"error": "User not found"}), 404

# Run the app
if __name__ == '__main__':
    app.run(debug=True)
