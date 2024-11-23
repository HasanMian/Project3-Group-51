import tkinter as tk
import random

class Employee:
    def __init__(self, name, phone, email, pay_rate, hours):
        self.name = name
        self.availability = ["Available"] * 14

def set_availability(employees):
    root = tk.Tk()
    root.title("Availability Schedule")

    shifts = ["Mon 1 AM", "Mon 1 PM", "Tue 1 AM", "Tue 1 PM", 
              "Wed 1 AM", "Wed 1 PM", "Thu 1 AM", "Thu 1 PM",
              "Fri 1 AM", "Fri 1 PM", "Sat 1 AM", "Sat 1 PM",
              "Sun 1 AM", "Sun 1 PM"]

    availability_vars = {emp.name: [tk.StringVar(value="Available") for _ in range(14)] for emp in employees}

    def save_availability():
        for emp in employees:
            emp.availability = [var.get() for var in availability_vars[emp.name]]
        root.destroy()

    for i, emp in enumerate(employees):
        tk.Label(root, text=emp.name).grid(row=0, column=i + 1)
        for j, shift in enumerate(shifts):
            tk.Label(root, text=shift).grid(row=j + 1, column=0)
            tk.OptionMenu(root, availability_vars[emp.name][j], "Available", "Preferred", "Unavailable").grid(row=j + 1, column=i + 1)

    tk.Button(root, text="Save", command=save_availability).grid(row=len(shifts) + 1, columnspan=len(employees) + 1)
    root.mainloop()

    schedule = []
    for i in range(14):
        preferred = [emp for emp in employees if emp.availability[i] == "Preferred"]
        available = [emp for emp in employees if emp.availability[i] in ["Available", "Preferred"]]
        selected = random.choice(preferred) if preferred else (random.choice(available) if available else None)
        schedule.append(selected.name if selected else "Unassigned")
    return schedule

employees = [Employee(name, str(i), f"{name.lower()}@example.com", 20.00 + i, 40) for i, name in enumerate("ABCDEFGH")]
schedule = set_availability(employees)
print("Final Schedule:", schedule)
