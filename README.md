# Password Generator App

A simple and user-friendly password generator app using Tkinter to create and save random passwords for different websites.

## Features
- Generate random passwords for different websites.
- Save generated passwords to a file for future reference.
- Specify the length of the password.
- Simple and intuitive GUI.

## Requirements
- Python 3.x

## Installation
1. Install Python 3.x from [python.org](https://www.python.org/).

## Usage
1. Run the `password_generator.py` file to start the application.
2. Enter the website name.
3. Enter the desired password length.
4. Click "Generate Password" to create a random password.
5. Click "Save Password" to save the password to a file.

## Code
```python
import tkinter as tk
from tkinter import messagebox
import random
import string
import os

class PasswordGeneratorApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Password Generator")
        self.root.geometry("400x300")

        # Create widgets
        self.label_website = tk.Label(root, text="Website:")
        self.label_website.pack(pady=5)

        self.entry_website = tk.Entry(root)
        self.entry_website.pack(pady=5)

        self.label_password = tk.Label(root, text="Password Length:")
        self.label_password.pack(pady=5)

        self.entry_password_length = tk.Entry(root)
        self.entry_password_length.pack(pady=5)

        self.button_generate = tk.Button(root, text="Generate Password", command=self.generate_password)
        self.button_generate.pack(pady=10)

        self.label_generated_password = tk.Label(root, text="")
        self.label_generated_password.pack(pady=5)

        self.button_save = tk.Button(root, text="Save Password", command=self.save_password)
        self.button_save.pack(pady=10)

        self.password = ""

    def generate_password(self):
        try:
            length = int(self.entry_password_length.get())
        except ValueError:
            messagebox.showerror("Invalid input", "Password length must be a number")
            return

        if length < 1:
            messagebox.showerror("Invalid input", "Password length must be at least 1")
            return

        characters = string.ascii_letters + string.digits + string.punctuation
        self.password = ''.join(random.choice(characters) for _ in range(length))
        self.label_generated_password.config(text=f"Generated Password: {self.password}")

    def save_password(self):
        website = self.entry_website.get()
        if not website or not self.password:
            messagebox.showerror("Error", "Website and generated password cannot be empty")
            return

        directory = "passwords"
        if not os.path.exists(directory):
            os.makedirs(directory)

        file_path = os.path.join(directory, "passwords.txt")
        with open(file_path, "a") as file:
            file.write(f"Website: {website}, Password: {self.password}\n")

        messagebox.showinfo("Success", "Password saved successfully")

# Create the main window
root = tk.Tk()
app = PasswordGeneratorApp(root)
root.mainloop()
