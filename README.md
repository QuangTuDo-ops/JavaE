package com.example;

import java.io.Serializable;

public class Employee implements Serializable {
    private int id;
    private String fname;
    private String lname;
    private double hoursRate;
    private String department;

    public Employee(int id, String fname, String lname, String department, double hoursRate) {
        this.id = id;
        this.fname = fname;
        this.lname = lname;
        this.department = department;
        this.hoursRate = hoursRate;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getFname() {
        return fname;
    }

    public void setFname(String fname) {
        this.fname = fname;
    }

    public String getLname() {
        return lname;
    }

    public void setLname(String lname) {
        this.lname = lname;
    }

    public double getHoursRate() {
        return hoursRate;
    }

    public void setHoursRate(double hoursRate) {
        this.hoursRate = hoursRate;
    }

    public String getDepartment() {
        return department;
    }
    public void setDepartment(String department) {
        this.department = department;
    }
    public String toString() {
        String s = "------Employee Information-----";
        s+="Employee ID: " + this.id + "\n";
        s+="First Name: " + this.fname + "\n";
        s+="Last Name: " + this.lname + "\n";
        s+="Department: " + this.department + "\n";
        s+="Base Salary: " + this.hoursRate + "\n";
        return s;
    }
}
package com.example;

import java.io.Serializable;

public class Payroll implements Serializable {
    private Employee employee;
    private double workedHours;
    private double overtimeHours;
    private double bonuses;
    private double deduction;
    private double netPay;

    public Payroll(Employee employee, double workedHours, double overtimeHours, double bonuses, double deduction, double netPay) {
        this.employee = employee;
        this.workedHours = workedHours;
        this.overtimeHours = overtimeHours;
        this.bonuses = bonuses;
        this.deduction = deduction;
        this.netPay = netPay;
    }

    public Employee getEmployee() {
        return employee;
    }

    public void setEmployee(Employee employee) {
        this.employee = employee;
    }

    public double getOvertimeHours() {
        return overtimeHours;
    }

    public void setOvertimeHours(double overtimeHours) {
        this.overtimeHours = overtimeHours;
    }

    public double getWorkedHours() {
        return workedHours;
    }

    public void setWorkedHours(double workedHours) {
        this.workedHours = workedHours;
    }

    public double getNetPay() {
        return netPay;
    }

    public void setNetPay(double netPay) {
        this.netPay = netPay;
    }

    public double getDeduction() {
        return deduction;
    }

    public void setDeduction(double deduction) {
        this.deduction = deduction;
    }

    public double getBonuses() {
        return bonuses;
    }

    public void setBonuses(double bonuses) {
        this.bonuses = bonuses;
    }

    public void calculateNetPay(){
        double hoursRate = employee.getHoursRate();
        double regularPay = hoursRate * workedHours;
        double overtimePay = overtimeHours * (1.5*hoursRate);
        netPay = overtimePay + regularPay + bonuses - deduction;
    }
}
package com.example;

import java.io.Serializable;
import java.util.Scanner;
import java.util.ArrayList;

public class Department implements Serializable {
    private static ArrayList<Employee> employees = new ArrayList<>();
    private static Scanner scanner = new Scanner(System.in);
    public static void EmployeeManager() {
        while (true) {
            System.out.println("\nEmployee Management System");
            System.out.println("1. Add Employee");
            System.out.println("2. View Employees");
            System.out.println("3. Update Employee");
            System.out.println("4. Delete Employee");
            System.out.println("5. Exit");
            System.out.print("Choose an option: ");

            int choice = scanner.nextInt();
            scanner.nextLine();

            switch (choice) {
                case 1:

            }
            }
        }
    private static void addEmployee(){
        System.out.println("Enter Employee ID: ");
        int id = scanner.nextInt();
        scanner.nextLine();
        System.out.println("Enter Employee's First Name: ");
        String firstName = scanner.nextLine();
        System.out.println("Enter Employee's Last Name: ");
        String lastName = scanner.nextLine();
        System.out.println("Enter Employee's Department: ");
        String department = scanner.nextLine();
        System.out.println("Enter Employee's Base Salary: ");
        double hoursRate = scanner.nextDouble();
        Employee employee = new Employee (id,firstName,lastName,department,hoursRate);
        employees.add(employee);
        System.out.println("Employee Added");
    }
    private static void viewEmployees() {
        if (employees.size() == 0) {
            System.out.println("No Employee Found");
        }
        else {
            for (Employee employee : employees) {
                System.out.println(employee);
            }
        }
    }
    private static void updateEmployee() {
        System.out.println("Enter Employee ID to update: ");
        int id = scanner.nextInt();
        scanner.nextLine();
        for (Employee employee : employees) {
            if (employee.getId() == id) {
                System.out.println("Enter Employee's First Name: ");
                String firstName = scanner.nextLine();
                System.out.println("Enter Employee's Last Name: ");
                String lastName = scanner.nextLine();
                System.out.println("Enter Employee's Department: ");
                String department = scanner.nextLine();
                System.out.println("Enter Employee's Base Salary: ");
                double hoursRate = scanner.nextDouble();
                employees.set(id ,new Employee (id,firstName,lastName,department,hoursRate));
                System.out.println("Employee Updated");
                return;
            }
        }
        System.out.println("Employee Not Found");
    }
    private static void deleteEmployee() {
        System.out.println("Enter Employee ID to delete: ");
        int id = scanner.nextInt();
        scanner.nextLine();
        for (Employee employee : employees) {
            if (employee.getId() == id) {
                employees.remove(employee);
                System.out.println("Employee Deleted");
                return;
            }
        }
        System.out.println("Employee Not Found");
    }
}
    
package com.example;

import java.io.*;
import java.util.ArrayList;

public class DataStorage {
    private static final String EMPLOYEE_FILE = "employees.ser";
    private static final String PAYROLL_FILE = "payrolls.ser";

    public static void saveEmployees(ArrayList<Employee> employees) {
        try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(EMPLOYEE_FILE))) {
            oos.writeObject(employees);
            System.out.println("Employee data saved successfully.");
        } catch (IOException e) {
            System.err.println("Error saving employee data: " + e.getMessage());
        }
    }

    public static ArrayList<Employee> loadEmployees() {
        try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream(EMPLOYEE_FILE))) {
            return (ArrayList<Employee>) ois.readObject();
        } catch (IOException | ClassNotFoundException e) {
            System.err.println("Error loading employee data: " + e.getMessage());
            return new ArrayList<>();
        }
    }

    public static void savePayrolls(ArrayList<Payroll> payrolls) {
        try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(PAYROLL_FILE))) {
            oos.writeObject(payrolls);
            System.out.println("Payroll data saved successfully.");
        } catch (IOException e) {
            System.err.println("Error saving payroll data: " + e.getMessage());
        }
    }

    public static ArrayList<Payroll> loadPayrolls() {
        try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream(PAYROLL_FILE))) {
            return (ArrayList<Payroll>) ois.readObject();
        } catch (IOException | ClassNotFoundException e) {
            System.err.println("Error loading payroll data: " + e.getMessage());
            return new ArrayList<>();
        }
    }
}

package com.example;

import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.*;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;

import java.util.ArrayList;

public class HRMApplication extends Application {
    private ArrayList<Employee> employees;

    @Override
    public void start(Stage primaryStage) {
        employees = DataStorage.loadEmployees();

        VBox layout = new VBox(10);
        layout.setPadding(new javafx.geometry.Insets(10));

        Label label = new Label("HR Management System");
        Button addEmployeeButton = new Button("Add Employee");
        Button viewEmployeesButton = new Button("View Employees");
        Button saveButton = new Button("Save Data");

        addEmployeeButton.setOnAction(e -> showAddEmployeeDialog());
        viewEmployeesButton.setOnAction(e -> showEmployeeList());
        saveButton.setOnAction(e -> DataStorage.saveEmployees(employees));

        layout.getChildren().addAll(label, addEmployeeButton, viewEmployeesButton, saveButton);

        Scene scene = new Scene(layout, 400, 300);
        primaryStage.setScene(scene);
        primaryStage.setTitle("HR Management System");
        primaryStage.show();
    }

    private void showAddEmployeeDialog() {
        Stage dialog = new Stage();
        dialog.setTitle("Add Employee");

        VBox dialogLayout = new VBox(10);
        dialogLayout.setPadding(new javafx.geometry.Insets(10));

        TextField idField = new TextField();
        idField.setPromptText("Employee ID");
        TextField firstNameField = new TextField();
        firstNameField.setPromptText("First Name");
        TextField lastNameField = new TextField();
        lastNameField.setPromptText("Last Name");
        TextField departmentField = new TextField();
        departmentField.setPromptText("Department");
        TextField salaryField = new TextField();
        salaryField.setPromptText("Hourly Rate");

        Button saveButton = new Button("Save");
        saveButton.setOnAction(e -> {
            try {
                int id = Integer.parseInt(idField.getText());
                String firstName = firstNameField.getText();
                String lastName = lastNameField.getText();
                String department = departmentField.getText();
                double salary = Double.parseDouble(salaryField.getText());

                Employee employee = new Employee(id, firstName, lastName, department, salary);
                employees.add(employee);
                dialog.close();
                Alert alert = new Alert(Alert.AlertType.INFORMATION, "Employee added successfully!");
                alert.showAndWait();
            } catch (NumberFormatException ex) {
                Alert alert = new Alert(Alert.AlertType.ERROR, "Invalid input. Please try again.");
                alert.showAndWait();
            }
        });

        dialogLayout.getChildren().addAll(idField, firstNameField, lastNameField, departmentField, salaryField, saveButton);

        Scene dialogScene = new Scene(dialogLayout, 300, 200);
        dialog.setScene(dialogScene);
        dialog.show();
    }

    private void showEmployeeList() {
        Stage listStage = new Stage();
        listStage.setTitle("Employee List");

        VBox listLayout = new VBox(10);
        listLayout.setPadding(new javafx.geometry.Insets(10));

        for (Employee employee : employees) {
            Label employeeLabel = new Label(employee.toString());
            listLayout.getChildren().add(employeeLabel);
        }

        Scene listScene = new Scene(listLayout, 400, 400);
        listStage.setScene(listScene);
        listStage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}

