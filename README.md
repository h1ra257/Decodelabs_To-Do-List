# Decodelabs_To-Do-List
"""
DecodeLabs - Industrial Training Kit
Python Programming - Project 1: The To-Do List

Goal: Build a program where users can add tasks to a list and view them.
Key Skill: Lists (append & print loops), enumerate()
Bonus: Persistence using JSON file (so data survives after the program closes)
"""

import json
import os

FILE_NAME = "tasks.json"


def load_tasks():
    """Load tasks from disk (Storage) into memory (RAM)."""
    if os.path.exists(FILE_NAME):
        with open(FILE_NAME, "r") as file:
            return json.load(file)
    return []


def save_tasks(tasks):
    """Save tasks from memory (RAM) to disk (Storage) - Persistence."""
    with open(FILE_NAME, "w") as file:
        json.dump(tasks, file, indent=4)


def add_task(tasks):
    """INPUT: Add a new task to the list."""
    task_name = input("Enter the task you want to add: ").strip()
    if task_name:
        tasks.append(task_name)
        save_tasks(tasks)
        print(f"'{task_name}' added successfully!\n")
    else:
        print("Task cannot be empty!\n")


def view_tasks(tasks):
    """OUTPUT: Display all tasks using enumerate() for index + value."""
    if not tasks:
        print("Your to-do list is empty.\n")
        return

    print("\n----- YOUR TO-DO LIST -----")
    for index, task in enumerate(tasks, start=1):
        print(f"{index}. {task}")
    print("----------------------------\n")


def delete_task(tasks):
    """PROCESS: Remove a task by its number."""
    view_tasks(tasks)
    if not tasks:
        return
    try:
        choice = int(input("Enter task number to delete: "))
        if 1 <= choice <= len(tasks):
            removed = tasks.pop(choice - 1)
            save_tasks(tasks)
            print(f"'{removed}' removed successfully!\n")
        else:
            print("Invalid task number.\n")
    except ValueError:
        print("Please enter a valid number.\n")


def show_menu():
    print("======================================")
    print("   DECODELABS - TO-DO LIST MANAGER")
    print("======================================")
    print("1. Add Task")
    print("2. View Tasks")
    print("3. Delete Task")
    print("4. Exit")


def main():
    tasks = load_tasks()  # Load previously saved tasks (if any)

    while True:
        show_menu()
        choice = input("Enter your choice (1-4): ").strip()

        if choice == "1":
            add_task(tasks)
        elif choice == "2":
            view_tasks(tasks)
        elif choice == "3":
            delete_task(tasks)
        elif choice == "4":
            print("Goodbye! Your tasks have been saved.")
            break
        else:
            print("Invalid choice. Please select between 1-4.\n")


if __name__ == "__main__":
    main()
