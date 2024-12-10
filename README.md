
my sql Schema:
<img width="374" alt="image" src="https://github.com/user-attachments/assets/001f4863-f952-4b15-899e-478198380675">


DB Model:
<img width="425" alt="image" src="https://github.com/user-attachments/assets/066338ce-56db-4dae-be10-aae781d6243c">



MySQL Schema::

CREATE DATABASE LibrarySystem;

USE LibrarySystem;

CREATE TABLE Users (
    user_id INT AUTO_INCREMENT PRIMARY KEY,
    email VARCHAR(255) NOT NULL,
    password VARCHAR(255) NOT NULL,
    role ENUM('Admin', 'User') NOT NULL
);

CREATE TABLE Books (
    book_id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    author VARCHAR(255),
    year INT
);

CREATE TABLE BorrowRequests (
    request_id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT,
    book_id INT,
    borrow_date DATE,
    return_date DATE,
    status ENUM('Pending', 'Approved', 'Denied'),
    FOREIGN KEY(user_id) REFERENCES Users(user_id),
    FOREIGN KEY(book_id) REFERENCES Books(book_id)
);


Backend APIs :

from flask import Flask
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = "mysql://root:
<password>@localhost/LibrarySystem"
app.secret_key = "secret-key"
db = SQLAlchemy(app)

from routes.librarian_routes import *
from routes.user_routes import *

if __name__ == "__main__":
    app.run(debug=True)


Database Models:

from app import db

class User(db.Model):
    user_id = db.Column(db.Integer, primary_key=True)
    email = db.Column(db.String(255), nullable=False)
    password = db.Column(db.String(255), nullable=False)
    role = db.Column(db.String(10), nullable=False)

class Book(db.Model):
    book_id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(255))

class BorrowRequest(db.Model):
    request_id = db.Column(db.Integer, primary_key=True)
    user_id = db.Column(db.Integer, db.ForeignKey('User.user_id'))
    book_id = db.Column(db.Integer, db.ForeignKey('Book.book_id'))
    borrow_date = db.Column(db.Date)
    return_date = db.Column(db.Date)
    status = db.Column(db.String(10))  # Pending, Approved, Denied





