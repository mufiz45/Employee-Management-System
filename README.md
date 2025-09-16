# OOP WRAPER 
#!/usr/bin/env python3
"""
Employee Management System - OOP Wrapper Project
Demonstrates key Object-Oriented Programming concepts including:
- Classes and Objects
- Inheritance
- Encapsulation
- Method Overloading
- Method Overriding
- Polymorphism
"""

class Employee:
    """Base Employee class demonstrating encapsulation and basic OOP concepts"""
    
    def _init_(self, employee_id=None, name="", age=0, salary=0.0):
        """Constructor with default parameters for method overloading"""
        self.__employee_id = employee_id  # Private attribute (encapsulation)
        self.__name = name                # Private attribute (encapsulation)
        self.__age = age
        self.__salary = salary
    
    # Alternative constructor (method overloading simulation)
    @classmethod
    def create_with_basic_info(cls, employee_id, name):
        """Alternative way to create an employee with just ID and name"""
        return cls(employee_id, name)
    
    @classmethod
    def create_full_profile(cls, employee_id, name, age, salary):
        """Alternative way to create an employee with full information"""
        return cls(employee_id, name, age, salary)
    
    # Getter methods (encapsulation)
    def get_employee_id(self):
        return self.__employee_id
    
    def get_name(self):
        return self.__name
    
    def get_age(self):
        return self.__age
    
    def get_salary(self):
        return self.__salary
    
    # Setter methods (encapsulation)
    def set_employee_id(self, employee_id):
        self.__employee_id = employee_id
    
    def set_name(self, name):
        self.__name = name
    
    def set_age(self, age):
        if age >= 0:
            self.__age = age
        else:
            print("Age cannot be negative")
    
    def set_salary(self, salary):
        if salary >= 0:
            self.__salary = salary
        else:
            print("Salary cannot be negative")
    
    def display(self):
        """Display employee information (will be overridden in derived classes)"""
        return f"Employee ID: {self._employee_id}\nName: {self.name}\nAge: {self.age}\nSalary: ${self._salary:,.2f}"
    
    def _del_(self):
        """Destructor to clean up resources"""
        print(f"Employee {self.__name} has been removed from the system")


class Manager(Employee):
    """Manager class inheriting from Employee (Inheritance)"""
    
    def _init_(self, employee_id=None, name="", age=0, salary=0.0, department=""):
        """Constructor calling parent constructor using super()"""
        super()._init_(employee_id, name, age, salary)
        self.__department = department  # Additional attribute
    
    # Getter and setter for department
    def get_department(self):
        return self.__department
    
    def set_department(self, department):
        self.__department = department
    
    def display(self):
        """Override display method to include department information"""
        base_info = super().display()
        return f"{base_info}\nDepartment: {self.__department}\nRole: Manager"
    
    def manage_team(self):
        """Manager-specific method"""
        return f"{self.get_name()} is managing the {self.__department} department"


class Developer(Employee):
    """Developer class inheriting from Employee (Inheritance)"""
    
    def _init_(self, employee_id=None, name="", age=0, salary=0.0, programming_language=""):
        """Constructor calling parent constructor using super()"""
        super()._init_(employee_id, name, age, salary)
        self.__programming_language = programming_language  # Additional attribute
    
    # Getter and setter for programming language
    def get_programming_language(self):
        return self.__programming_language
    
    def set_programming_language(self, programming_language):
        self.__programming_language = programming_language
    
    def display(self):
        """Override display method to include programming language information"""
        base_info = super().display()
        return f"{base_info}\nProgramming Language: {self.__programming_language}\nRole: Developer"
    
    def code_project(self):
        """Developer-specific method"""
        return f"{self.get_name()} is coding in {self.__programming_language}"


class EmployeeManagementSystem:
    """Main system class to manage employees"""
    
    def _init_(self):
        self.employees = []
        self.next_id = 1000
    
    def add_employee(self, employee):
        """Add an employee to the system"""
        if employee.get_employee_id() is None:
            employee.set_employee_id(self.next_id)
            self.next_id += 1
        self.employees.append(employee)
        print(f"Employee {employee.get_name()} added successfully!")
    
    def remove_employee(self, employee_id):
        """Remove an employee by ID"""
        for employee in self.employees:
            if employee.get_employee_id() == employee_id:
                self.employees.remove(employee)
                print(f"Employee with ID {employee_id} removed successfully!")
                return True
        print(f"Employee with ID {employee_id} not found!")
        return False
    
    def find_employee(self, employee_id):
        """Find an employee by ID"""
        for employee in self.employees:
            if employee.get_employee_id() == employee_id:
                return employee
        return None
    
    def update_employee(self, employee_id, **kwargs):
        """Update employee information"""
        employee = self.find_employee(employee_id)
        if employee:
            if 'name' in kwargs:
                employee.set_name(kwargs['name'])
            if 'age' in kwargs:
                employee.set_age(kwargs['age'])
            if 'salary' in kwargs:
                employee.set_salary(kwargs['salary'])
            if 'department' in kwargs and isinstance(employee, Manager):
                employee.set_department(kwargs['department'])
            if 'programming_language' in kwargs and isinstance(employee, Developer):
                employee.set_programming_language(kwargs['programming_language'])
            print(f"Employee {employee_id} updated successfully!")
            return True
        else:
            print(f"Employee with ID {employee_id} not found!")
            return False
    
    def display_all_employees(self):
        """Display all employees (demonstrates polymorphism)"""
        if not self.employees:
            print("No employees in the system.")
            return
        
        print("\n" + "="*50)
        print("ALL EMPLOYEES")
        print("="*50)
        for employee in self.employees:
            print(employee.display())  # Polymorphic call
            print("-" * 30)
    
    def check_inheritance(self, employee_id):
        """Demonstrate isinstance() usage"""
        employee = self.find_employee(employee_id)
        if employee:
            print(f"\nInheritance Check for Employee {employee_id}:")
            print(f"Is Employee: {isinstance(employee, Employee)}")
            print(f"Is Manager: {isinstance(employee, Manager)}")
            print(f"Is Developer: {isinstance(employee, Developer)}")
        else:
            print(f"Employee with ID {employee_id} not found!")


def main():
    """Main function demonstrating the Employee Management System"""
    print("=== Employee Management System - OOP Demonstration ===\n")
    
    # Create the management system
    ems = EmployeeManagementSystem()
    
    # Menu-driven interface
    while True:
        print("\n" + "="*50)
        print("EMPLOYEE MANAGEMENT SYSTEM MENU")
        print("="*50)
        print("1. Add Employee")
        print("2. Add Manager")
        print("3. Add Developer")
        print("4. Display All Employees")
        print("5. Find Employee")
        print("6. Update Employee")
        print("7. Remove Employee")
        print("8. Check Inheritance")
        print("9. Demonstrate OOP Concepts")
        print("10. Exit")
        print("="*50)
        
        try:
            choice = int(input("Enter your choice (1-10): "))
            
            if choice == 1:
                # Add regular employee
                name = input("Enter employee name: ")
                age = int(input("Enter employee age: "))
                salary = float(input("Enter employee salary: "))
                
                # Demonstrate method overloading with different constructors
                employee = Employee.create_full_profile(None, name, age, salary)
                ems.add_employee(employee)
            
            elif choice == 2:
                # Add manager
                name = input("Enter manager name: ")
                age = int(input("Enter manager age: "))
                salary = float(input("Enter manager salary: "))
                department = input("Enter department: ")
                
                manager = Manager(None, name, age, salary, department)
                ems.add_employee(manager)
            
            elif choice == 3:
                # Add developer
                name = input("Enter developer name: ")
                age = int(input("Enter developer age: "))
                salary = float(input("Enter developer salary: "))
                prog_lang = input("Enter programming language: ")
                
                developer = Developer(None, name, age, salary, prog_lang)
                ems.add_employee(developer)
            
            elif choice == 4:
                # Display all employees (demonstrates polymorphism)
                ems.display_all_employees()
            
            elif choice == 5:
                # Find employee
                emp_id = int(input("Enter employee ID: "))
                employee = ems.find_employee(emp_id)
                if employee:
                    print("\n" + "="*30)
                    print("EMPLOYEE FOUND")
                    print("="*30)
                    print(employee.display())
                else:
                    print("Employee not found!")
            
            elif choice == 6:
                # Update employee
                emp_id = int(input("Enter employee ID to update: "))
                employee = ems.find_employee(emp_id)
                if employee:
                    print("What would you like to update?")
                    print("1. Name")
                    print("2. Age") 
                    print("3. Salary")
                    if isinstance(employee, Manager):
                        print("4. Department")
                    elif isinstance(employee, Developer):
                        print("4. Programming Language")
                    
                    update_choice = int(input("Enter choice: "))
                    
                    if update_choice == 1:
                        new_name = input("Enter new name: ")
                        ems.update_employee(emp_id, name=new_name)
                    elif update_choice == 2:
                        new_age = int(input("Enter new age: "))
                        ems.update_employee(emp_id, age=new_age)
                    elif update_choice == 3:
                        new_salary = float(input("Enter new salary: "))
                        ems.update_employee(emp_id, salary=new_salary)
                    elif update_choice == 4:
                        if isinstance(employee, Manager):
                            new_dept = input("Enter new department: ")
                            ems.update_employee(emp_id, department=new_dept)
                        elif isinstance(employee, Developer):
                            new_lang = input("Enter new programming language: ")
                            ems.update_employee(emp_id, programming_language=new_lang)
                else:
                    print("Employee not found!")
            
            elif choice == 7:
                # Remove employee
                emp_id = int(input("Enter employee ID to remove: "))
                ems.remove_employee(emp_id)
            
            elif choice == 8:
                # Check inheritance
                emp_id = int(input("Enter employee ID: "))
                ems.check_inheritance(emp_id)
            
            elif choice == 9:
                # Demonstrate OOP concepts
                print("\n" + "="*60)
                print("OOP CONCEPTS DEMONSTRATION")
                print("="*60)
                
                # Create sample employees
                emp1 = Employee.create_with_basic_info(2001, "John Doe")
                emp1.set_age(30)
                emp1.set_salary(50000)
                
                manager1 = Manager(2002, "Jane Smith", 35, 75000, "HR")
                developer1 = Developer(2003, "Bob Johnson", 28, 65000, "Python")
                
                print("1. ENCAPSULATION:")
                print("   - Private attributes accessed via getter/setter methods")
                print(f"   - Employee name (private): {emp1.get_name()}")
                
                print("\n2. INHERITANCE:")
                print("   - Manager and Developer inherit from Employee")
                print(f"   - Manager inherits and adds department: {manager1.get_department()}")
                print(f"   - Developer inherits and adds language: {developer1.get_programming_language()}")
                
                print("\n3. METHOD OVERRIDING:")
                print("   - Each class has its own display() method")
                print("   Employee display:")
                print("   " + emp1.display().replace("\n", "\n   "))
                print("\n   Manager display:")
                print("   " + manager1.display().replace("\n", "\n   "))
                
                print("\n4. POLYMORPHISM:")
                print("   - Same method call, different behavior based on object type")
                employees = [emp1, manager1, developer1]
                for emp in employees:
                    print(f"   {emp._class.name_}: {emp.display().split(chr(10))[1]}")
                
                print("\n5. METHOD OVERLOADING (simulated with class methods):")
                print("   - Multiple ways to create Employee objects")
                print("   - create_with_basic_info(), create_full_profile()")
                
                print("\n6. USE OF super() and isinstance():")
                print(f"   - isinstance(manager1, Employee): {isinstance(manager1, Employee)}")
                print(f"   - isinstance(manager1, Manager): {isinstance(manager1, Manager)}")
                print(f"   - Manager uses super() to call parent constructor")
            
            elif choice == 10:
                print("Thank you for using Employee Management System!")
                print("Demonstrating destructor calls...")
                break
            
            else:
                print("Invalid choice! Please enter a number between 1-10.")
        
        except ValueError:
            print("Invalid input! Please enter a valid number.")
        except Exception as e:
            print(f"An error occurred: {e}")


if _name_ == "_main_":
    main()
