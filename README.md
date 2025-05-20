# Tapsriglar
#İndividual work
import json
import time
import os

filename = "fitness_data.json"
workouts = []

#Welcome Screen with ASCII Art
def welcome_screen():
    print("="*50)
    print("   *** WELCOME TO FITNESS TRACKER ***")
    print("="*50)
    print("Tracking your workouts, one rep at a time!")
    time.sleep(1)

#Main Menu
def main_menu():
    print("\nMain Menu:")
    print("1. Add new workout")
    print("2. View all workouts")
    print("3. Search workout")
    print("4. Update workout")
    print("5. Delete workout")
    print("6. Summary statistics")
    print("7. Save to file")
    print("8. Load from file")
    print("9. Sort workouts")
    print("10. Recursive workout count")
    print("11. Help/Instructions")
    print("12. Clear all data")
    print("0. Exit")

#Input Validation
def input_float(prompt):
    while True:
        try:
            return float(input(prompt))
        except ValueError:
            print("Please enter a valid number.")

#Add new workout
def add_workout():
    try:
        workout = {}
        workout['id'] = input("Enter workout ID: ")
        workout['name'] = input("Enter workout name: ")
        workout['date'] = input("Enter date (YYYY-MM-DD): ")
        workout['calories'] = input_float("Enter calories burned: ")
        workouts.append(workout)
        print("Workout added.")
    except Exception as e:
        print("Error:", e)

#View all
def view_workouts():
    if not workouts:
        print("No records found.")
        return
    print("\n{:<10} {:<20} {:<15} {:<10}".format("ID", "Name", "Date", "Calories"))
    print("-"*60)
    for w in workouts:
        print("{:<10} {:<20} {:<15} {:<10.2f}".format(w['id'], w['name'], w['date'], w['calories']))

#Search
def search_workout():
    key = input("Enter workout ID or name to search: ").lower()
    found = [w for w in workouts if w['id'].lower() == key or w['name'].lower() == key]
    if found:
        for w in found:
            print(w)
    else:
        print("Workout not found.")

#Update
def update_workout():
    wid = input("Enter workout ID to update: ")
    for w in workouts:
        if w['id'] == wid:
            w['name'] = input("New name: ")
            w['date'] = input("New date: ")
            w['calories'] = input_float("New calories burned: ")
            print("Workout updated.")
            return
    print("Workout not found.")

#Delete
def delete_workout():
    wid = input("Enter ID to delete: ")
    global workouts
    workouts = [w for w in workouts if w['id'] != wid]
    print("Workout deleted if it existed.")

#Summary Stats
def summary_stats():
    if not workouts:
        print("No data.")
        return
    total = sum(w['calories'] for w in workouts)
    avg = total / len(workouts)
    print(f"Total Workouts: {len(workouts)}")
    print(f"Total Calories Burned: {total:.2f}")
    print(f"Average Calories per Workout: {avg:.2f}")

#Save
def save_to_file():
    try:
        with open(filename, 'w') as f:
            json.dump(workouts, f)
        print("Data saved.")
    except Exception as e:
        print("Error:", e)

#Load
def load_from_file():
    global workouts
    try:
        if os.path.exists(filename):
            with open(filename, 'r') as f:
                workouts = json.load(f)
            print("Data loaded.")
        else:
            print("No saved data found.")
    except Exception as e:
        print("Error:", e)

#Sort
def sort_workouts():
    if not workouts:
        print("No data to sort.")
        return
    field = input("Sort by (id/name/date/calories): ").lower()
    if field not in ['id', 'name', 'date', 'calories']:
        print("Invalid field.")
        return
    workouts.sort(key=lambda x: x[field])
    print(f"Sorted by {field}.")

Recursive count
def count_recursive(index=0):
    if index >= len(workouts):
        return 0
    return 1 + count_recursive(index + 1)

#Help
def help_menu():
    print("\nInstructions:")
    print("- Use this tracker to log your workouts.")
    print("- You can add, view, update, and delete workouts.")
    print("- Calories burned are tracked.")
    print("- Data is saved to a file locally (JSON format).")

#Clear all data
def clear_all():
    confirm = input("Are you sure you want to delete all data? (yes/no): ")
    if confirm.lower() == "yes":
        workouts.clear()
        print("All data cleared.")
    else:
        print("Cancelled.")

#Main loop + 15. Error handling
def main():
    welcome_screen()
    while True:
        try:
            main_menu()
            choice = input("Select an option: ")
            match choice:
                case "1": add_workout()
                case "2": view_workouts()
                case "3": search_workout()
                case "4": update_workout()
                case "5": delete_workout()
                case "6": summary_stats()
                case "7": save_to_file()
                case "8": load_from_file()
                case "9": sort_workouts()
                case "10": print("Total workouts:", count_recursive())
                case "11": help_menu()
                case "12": clear_all()
                case "0":
                    print("Goodbye!")
                    break
                case _: print("Invalid choice.")
        except Exception as e:
            print("An error occurred:", e)

if __name__ == "__main__":
    main()