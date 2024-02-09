# hackondata dfr

#Name: Joao Cezar Pereira de Queiroz
#ID: 101015470
#Lab Professor: Maziar Sojoudian

#This is to be able to clear the prompt
from IPython.display import clear_output


choice = ""
employee_list = [
    [1, "John Alber", "hourly", 8, 0, 0, 2],
    [2, "Sarah Rose", "manager", 12, 0, 0, 3],
    [3, "Alex Folen", "manager", 5, 0, 0, 4],
    [4, "Pola Sahari", "hourly", 17, 0, 0, 5]
]

item_list = [
    [1, "Nike shoes", 12.0], 
    [2, "Trampoline", 180],
    [3, "Mercury Bicycle", 15.60],
    [4, "Necklace Set", 80]
]

types = ("hourly","manager")

#Tests to see if what the user entered is a valid int or float
def get_valid_number(prompt_message):
  while True:
    user_input = input(prompt_message)
    if user_input.isdigit():
      return int(user_input)
    elif '.' in user_input:
      try:
        user_input = float(user_input)
        return user_input
      except ValueError:
        pass 
    print("Input must be a number. Please try again.")


def verify_yes_no_input(prompt_message):
  while True:
    user_input = input(prompt_message)
    if user_input == "n" or user_input == "N" or  user_input == "y" or user_input == "Y" :
        return user_input.lower()
    print("Wrong input. Please enter 'y' or 'n'.")


def verify_empty_input(prompt_message):
  while True:
    user_input = input(prompt_message)
    if user_input:
        return user_input
    print("The field can not be empty. Please try again.")


def get_element_index(element_id, element_position, element_list):
    for index, element in enumerate(element_list):
        if element[element_position] == int(element_id):
            return index
    return -1 

def verify_purchase_fields(prompt_message, exist_message,element_position,element_list):
  found = False
  while True:
    user_input = input(prompt_message)
    if user_input and user_input.isdigit():
      if get_element_index(user_input, element_position,element_list) != -1 :
        return user_input
      else:
        print(exist_message)
    else:
      print("You must enter a number. Please try again.")

# this verify if the employeeId and employeeDiscountNumber already exixts
def verify_field_exist(prompt_message, exist_message,element_position,element_list):
  found = False
  while True:
    user_input = input(prompt_message)
    if user_input and user_input.isdigit():
      for element in element_list :
        if element[element_position] == int(user_input):
            found = True
      if found == False :
        return int(user_input)
      print(exist_message)
      found = False
    else:
      print("You must enter a number. Please try again.")


def get_valid_type(prompt_message):
  while True:
    user_input = input(prompt_message)
    if user_input in types:
      return user_input
    print("Please enter one of these values: " + ", ".join(types) + ": ")


def print_items():
  print("Item Number   | Item Name         | Item Cost")
  print("-" * 48)
  if item_list:
    for item in item_list:
        item_number = item[0]
        item_name = item[1]
        item_cost = item[2]

        print(f"{item_number:<12} | {item_name:<18} | ${item_cost:.2f}")
  else:
    print("\nThere are NO items in the list.\n")


def print_employee_summary():
  print("Employee ID | Employee Name         | Employee Type   | Years Worked | Total Purchased | Total Discount | Employee Discount Number")
  print("-" * 130)
  if employee_list:
    for employee in employee_list:
        employee_id, employee_name, employee_type, years_worked, total_purchased, total_discount, employee_discount_number = employee

        print(f"{employee_id:<12} | {employee_name:<20} | {employee_type:<15} | {years_worked:<13} | ${total_purchased:<15.2f} | ${total_discount:<13.2f}  | {employee_discount_number:<23}")
  else:
    print("\nThere are NO employees in the list.\n")

# asks the user to enter employee data and adds to the employee list
def create_employee():
  choice = "y"
  while choice == "y":
    employeeId = verify_field_exist("Enter the employee ID: ", "The employee already exists. Please try again.",0, employee_list)
    employeeName = verify_empty_input("Enter the employee name: ")
    employeeType = get_valid_type("Enter the employee type: ")
    yearsWorked = get_valid_number("Enter the number of years the employee has worked: ")
    employeeDiscountNumber = verify_field_exist("Enter the employee discount number: ","The discount ID already exists. Please try again.", 6, employee_list)

    employee_list.append([employeeId, employeeName,employeeType,yearsWorked, 0, 0,employeeDiscountNumber])
    print("\nEmployee added successfully!\n")
    choice = verify_yes_no_input("Would you like to add another employee (y/n)? ")
    if choice == "y" :
      clear_output()
      continue


# asks the user to enter employee data and adds to the items list
def create_item():
  choice = "y"
  while choice == "y":
    itemNumber = input("Enter the item number: ")
    verify_field_exist("Enter the item number: ", "The item number already exists. Please try again.",0, item_list)
    itemName = verify_empty_input("Enter the item name: ")
    ItemCost = get_valid_number("Enter the item cost: ")
   
    item_list.append([itemNumber, itemName, ItemCost])
    print("\nItem added successfully!\n")
    choice = verify_yes_no_input("Would you like to add another item (y/n)? ")


def calculate_discount(index):
  emp_type  = employee_list[index][2]
  total_discount = employee_list[index][5]
  total_purchase = employee_list[index][4]
  years_worked = employee_list[index][3]
  percent_discount = 0
  discount = 0
  employee_list[index]
  if total_discount <= 200:
    if years_worked <= 5 :
      percent_discount = years_worked *0.02
    else:
      percent_discount = 0.1
    if emp_type == "hourly":
      percent_discount += 0.02
    else:
      percent_discount += 0.1
    discount = total_purchase * percent_discount
    if 200 - total_discount < discount :
      employee_list[index][5] = 200
    else :
      employee_list[index][5] = total_discount + discount

#adds the total purchace to a specific employee
def calculate_purchase(employee_index, item_index):
  total_purchase = employee_list[employee_index][4]
  item_cost = item_list[item_index][2]
  employee_list[employee_index][4] = total_purchase + item_cost


def make_purchase():
  clear_output()
  employee_choice = "y"
  item_choice = "y"
  
  while employee_choice == "y":
    print("\n*****************************MAKE PURCHASE MENU*************************************\n")
    print_items()  
    empDiscountNum = verify_purchase_fields("Enter the employee discount number: ", "The discount number does not exists. Please try again.",6, employee_list)
    employee_index = get_element_index(empDiscountNum, 6, employee_list)
    print("Discount Number: ", empDiscountNum, "  Employee Name: ", employee_list[employee_index][1])
    while choice == "y":
      itemNumber = verify_purchase_fields("Enter the item number: ", "The item number does not exists. Please try again.",0, item_list)
      item_index = get_element_index(itemNumber, 0, item_list)
      calculate_purchase(employee_index, item_index)
      calculate_discount(employee_index)
      
      print("\nPurchase made successfully!\n")
      choice = verify_yes_no_input("Would you like to purchase another item (y/n)? ")
  

while choice != "5":
  print("""  ***************************************************************************************
  *                     Developed by: Joao Cezar Pereira de Queiroz                     *
  *                               ID: 101015470                                         *
  ***************************************************************************************
  *                Please, enter a number to select one of the options:                 *
  *                                                                                     *
  *                             1 - Create Employee                                     *
  *                             2 - Create Item                                         *
  *                             3 - Make Purchace                                       *
  *                             4 - All Employee Summary                                *
  *                             5 - Exit                                                *
  *                                                                                     *
  ***************************************************************************************""")
  choice = input("Enter your option: ")
  if choice == "1":
    clear_output()
    print("\n*****************************CREATE EMPLOYEE MENU*************************************\n")
    print()
    create_employee()
    if verify_yes_no_input("Would you like go back to main menu (y/n)? ") == "y" :
      clear_output()
      continue
    else: 
      choice = "5"
  elif choice == "2":
    print("\n*****************************CREATE ITEM MENU*************************************\n")
    create_item()
    print()
    verify_yes_no_input("Would you like go back to main menu (y/n)? ")

  elif choice == "3":
    clear_output()
    make_purchase()
    print_employee_summary()
    print()
    verify_yes_no_input("Would you like go back to main menu (y/n)? ")
  elif choice == "4":
    print("\n*****************************SUMMARY EMPLOYEE MENU*************************************\n")
    print_employee_summary()
    verify_yes_no_input("Would you like go back to main menu (y/n)? ")
  else:
    print("Invalid option. Enter a number from the menu.")


