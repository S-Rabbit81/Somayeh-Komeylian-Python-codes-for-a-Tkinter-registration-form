#################################################################################################################################################
######################################################## Project for Session 14: ################################################################ 
########################################### Adding Images (or Icons) ############################################################################
#################################################################################################################################################
#################################################################################################################################################

import tkinter as tk
from tkinter import messagebox
import csv
import os
from PIL import Image, ImageTk # 2. Import Pillow

GIRLS_FILE = "girls_data.csv"
BOYS_FILE = "boys_data.csv"

def create_csv_if_not_exists(filename):
    """Creates a new CSV file with headers if it doesn't exist."""
    if not os.path.exists(filename):
        with open(filename, 'w', newline='') as f:
            writer = csv.writer(f)
            writer.writerow(['Name', 'Gender', 'Skills'])

def check_for_existing_user(username):
    """Checks if a username already exists in either CSV file."""
    for filename in [GIRLS_FILE, BOYS_FILE]:
        if os.path.exists(filename):
            with open(filename, 'r') as f:
                reader = csv.reader(f)
                next(reader, None) # Skip the header row
                for row in reader:
                    if row[0] == username:
                        return True
    return False

def save_data():
    """Saves user data to the appropriate CSV file."""
    username = username_entry.get()
    password = password_entry.get()
    gender = gender_var.get()
    selected_skills = []
    if python_var.get():
        selected_skills.append('Python')
    if cpp_var.get():
        selected_skills.append('C++')
    if java_var.get():
        selected_skills.append('Java')

    # Input validation
    if not username or not password or not gender:
        messagebox.showerror('Error', 'Please fill in all required fields.')
        return
    if check_for_existing_user(username):
        messagebox.showerror('Error', 'Username already exists. Please choose another one.')
        return

    # Determine which file to write to
    if gender.lower() == 'girls':
        file_to_save = GIRLS_FILE
    elif gender.lower() == 'boys':
        file_to_save = BOYS_FILE
    else:
        messagebox.showerror('Error', 'Invalid gender selection.')
        return

    # Append data to the CSV file
    try:
        with open(file_to_save, 'a', newline='') as f:
            writer = csv.writer(f)
            skills_str = ', '.join(selected_skills)
            writer.writerow([username, gender, skills_str])
        messagebox.showinfo('Success', 'Registration successful!')
        clear_form()
    except Exception as e:
        messagebox.showerror('File Error', f'An error occurred while writing to the file: {e}')

def clear_form():
    """Clears all input fields in the form."""
    username_entry.delete(0, tk.END)
    password_entry.delete(0, tk.END)
    gender_var.set(None)
    python_var.set(0)
    cpp_var.set(0)
    java_var.set(0)

root = tk.Tk()
root.title('Registration Form')
root.geometry('400x450+400+200')
root.configure(bg='#E0BBE4')

# Create CSV files with headers if they don't exist
create_csv_if_not_exists(GIRLS_FILE)
create_csv_if_not_exists(BOYS_FILE)

# --- Load and resize images ---
# The images are stored in variables to prevent garbage collection.
try:
    img_size = (30, 30)
    # Resize and create PhotoImage for labels
    user_icon_raw = Image.open('Username.png').resize(img_size, Image.LANCZOS)
    user_icon = ImageTk.PhotoImage(user_icon_raw)
    password_icon_raw = Image.open('Password.png').resize(img_size, Image.LANCZOS)
    password_icon = ImageTk.PhotoImage(password_icon_raw)
    gender_icon_raw = Image.open('Gender.png').resize(img_size, Image.LANCZOS)
    gender_icon = ImageTk.PhotoImage(gender_icon_raw)
    skills_icon_raw = Image.open('Skill.png').resize(img_size, Image.LANCZOS)
    skills_icon = ImageTk.PhotoImage(skills_icon_raw)
    
    # Resize and create PhotoImage for radio buttons
    female_icon_raw = Image.open('Female.jpg').resize(img_size, Image.LANCZOS)
    female_icon = ImageTk.PhotoImage(female_icon_raw)
    male_icon_raw = Image.open('Male.jpg').resize(img_size, Image.LANCZOS)
    male_icon = ImageTk.PhotoImage(male_icon_raw)
    
    # Resize and create PhotoImage for check buttons
    python_unchecked_raw = Image.open('Python.jpg').resize(img_size, Image.LANCZOS)
    python_checked_raw = Image.open('Python.jpg').resize(img_size, Image.LANCZOS)
    python_unchecked = ImageTk.PhotoImage(python_unchecked_raw)
    python_checked = ImageTk.PhotoImage(python_checked_raw)

    cpp_unchecked_raw = Image.open('C++.png').resize(img_size, Image.LANCZOS)
    cpp_checked_raw = Image.open('C++.png').resize(img_size, Image.LANCZOS)
    cpp_unchecked = ImageTk.PhotoImage(cpp_unchecked_raw)
    cpp_checked = ImageTk.PhotoImage(cpp_checked_raw)

    java_unchecked_raw = Image.open('Java.png').resize(img_size, Image.LANCZOS)
    java_checked_raw = Image.open('Java.png').resize(img_size, Image.LANCZOS)
    java_unchecked = ImageTk.PhotoImage(java_unchecked_raw)
    java_checked = ImageTk.PhotoImage(java_checked_raw)
    
    # Resize and create PhotoImage for buttons
    save_icon_raw = Image.open('Save.jpg').resize(img_size, Image.LANCZOS)
    save_icon = ImageTk.PhotoImage(save_icon_raw)
    clear_icon_raw = Image.open('Clear.jpg').resize(img_size, Image.LANCZOS)
    clear_icon = ImageTk.PhotoImage(clear_icon_raw)

except FileNotFoundError as e:
    messagebox.showerror('Image Error', f"Missing image file: {e}. Please ensure images are in the same directory.")
    # Exit if images are essential
    root.destroy()
    exit()

# --- Create and place widgets with images ---
# Labels
tk.Label(root, text='Username', image=user_icon, compound='left', bg='#E0BBE4').grid(row=0, column=0, padx=10, pady=5, sticky='w')
username_entry = tk.Entry(root)
username_entry.grid(row=0, column=1, padx=10, pady=5)

tk.Label(root, text='Password', image=password_icon, compound='left', bg='#E0BBE4').grid(row=1, column=0, padx=10, pady=5, sticky='w')
password_entry = tk.Entry(root, show='*')
password_entry.grid(row=1, column=1, padx=10, pady=5)

# Radiobuttons
tk.Label(root, text='Gender', image=gender_icon, compound='left', bg='#E0BBE4').grid(row=2, column=0, padx=10, pady=5, sticky='w')
gender_var = tk.StringVar(value='')
tk.Radiobutton(root, text='Girls', image=female_icon, compound='left', variable=gender_var, value='girls', bg='#E0BBE4').grid(row=2, column=1, sticky='w')
tk.Radiobutton(root, text='Boys', image=male_icon, compound='left', variable=gender_var, value='boys', bg='#E0BBE4').grid(row=3, column=1, sticky='w')

# Checkbuttons
tk.Label(root, text='Skills', image=skills_icon, compound='left', bg='#E0BBE4').grid(row=4, column=0, padx=10, pady=5, sticky='w')
python_var = tk.IntVar()
cpp_var = tk.IntVar()
java_var = tk.IntVar()

tk.Checkbutton(root, text='Python', variable=python_var, image=python_unchecked, selectimage=python_checked, indicatoron=False, compound='left', bg='#E0BBE4').grid(row=4, column=1, sticky='w')
tk.Checkbutton(root, text='C++', variable=cpp_var, image=cpp_unchecked, selectimage=cpp_checked, indicatoron=False, compound='left', bg='#E0BBE4').grid(row=5, column=1, sticky='w')
tk.Checkbutton(root, text='Java', variable=java_var, image=java_unchecked, selectimage=java_checked, indicatoron=False, compound='left', bg='#E0BBE4').grid(row=6, column=1, sticky='w')

# Buttons
tk.Button(root, text='Save Information', image=save_icon, compound='left', command=save_data).grid(row=7, column=0, padx=10, pady=10, sticky='w')
tk.Button(root, text='Clear Form', image=clear_icon, compound='left', command=clear_form).grid(row=7, column=1, padx=10, pady=10, sticky='w')

# Run the application
root.mainloop()
