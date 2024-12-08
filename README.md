from flask import Flask, request, jsonify

app = Flask(__name__)

# In-memory data storage
books = []
members = []

# Utility function to find item by ID
def find_item_by_id(item_list, item_id):
    return next((item for item in item_list if item['id'] == item_id), None)

# Books Routes
@app.route('/books', methods=['GET'])
def get_books():
    return jsonify(books), 200

@app.route('/books', methods=['POST'])
def add_book():
    data = request.get_json()
    if not data.get('id') or not data.get('title') or not data.get('author'):
        return jsonify({"error": "Missing fields: id, title, and author are required."}), 400
    
    if find_item_by_id(books, data['id']):
        return jsonify({"error": "Book with this ID already exists."}), 400

    books.append(data)
    return jsonify(data), 201

@app.route('/books/<int:book_id>', methods=['GET'])
def get_book(book_id):
    book = find_item_by_id(books, book_id)
    if book:
        return jsonify(book), 200
    return jsonify({"error": "Book not found."}), 404

@app.route('/books/<int:book_id>', methods=['PUT'])
def update_book(book_id):
    data = request.get_json()
    book = find_item_by_id(books, book_id)
    if not book:
        return jsonify({"error": "Book not found."}), 404

    book.update(data)
    return jsonify(book), 200

@app.route('/books/<int:book_id>', methods=['DELETE'])
def delete_book(book_id):
    book = find_item_by_id(books, book_id)
    if book:
        books.remove(book)
        return jsonify({"message": "Book deleted."}), 200
    return jsonify({"error": "Book not found."}), 404

# Members Routes
@app.route('/members', methods=['GET'])
def get_members():
    return jsonify(members), 200

@app.route('/members', methods=['POST'])
def add_member():
    data = request.get_json()
    if not data.get('id') or not data.get('name') or not data.get('email'):
        return jsonify({"error": "Missing fields: id, name, and email are required."}), 400

    if find_item_by_id(members, data['id']):
        return jsonify({"error": "Member with this ID already exists."}), 400

    members.append(data)
    return jsonify(data), 201

@app.route('/members/<int:member_id>', methods=['GET'])
def get_member(member_id):
    member = find_item_by_id(members, member_id)
    if member:
        return jsonify(member), 200
    return jsonify({"error": "Member not found."}), 404

@app.route('/members/<int:member_id>', methods=['PUT'])
def update_member(member_id):
    data = request.get_json()
    member = find_item_by_id(members, member_id)
    if not member:
        return jsonify({"error": "Member not found."}), 404

    member.update(data)
    return jsonify(member), 200

@app.route('/members/<int:member_id>', methods=['DELETE'])
def delete_member(member_id):
    member = find_item_by_id(members, member_id)
    if member:
        members.remove(member)
        return jsonify({"message": "Member deleted."}), 200
    return jsonify({"error": "Member not found."}), 404

if __name__ == '__main__':
    app.run(debug=True)

