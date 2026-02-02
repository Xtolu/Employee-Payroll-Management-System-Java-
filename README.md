package payroll;

public class Employee {

    private int id;
    private String name;
    private String role;
    private double basicSalary;

    public Employee(int id, String name, String role, double basicSalary) {
        this.id = id;
        this.name = name;
        this.role = role;
        this.basicSalary = basicSalary;
    }

    public int getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public String getRole() {
        return role;
    }

    public double getBasicSalary() {
        return basicSalary;
    }

    @Override
    public String toString() {
        return "Employee ID: " + id +
               ", Name: " + name +
               ", Role: " + role +
               ", Basic Salary: ₦" + basicSalary;
    }
    package payroll;

public class PayrollCalculator {

    private static final double TAX_RATE = 0.15;      // 15%
    private static final double PENSION_RATE = 0.08;  // 8%

    public static double calculateTax(double salary) {
        return salary * TAX_RATE;
    }

    public static double calculatePension(double salary) {
        return salary * PENSION_RATE;
    }

    public static double calculateNetSalary(double salary) {
        double tax = calculateTax(salary);
        double pension = calculatePension(salary);
        return salary - (tax + pension);
    }
}
package payroll;

import java.util.ArrayList;
import java.util.List;

public class EmployeeRepository {

    private List<Employee> employees = new ArrayList<>();

    public void addEmployee(Employee employee) {
        employees.add(employee);
    }

    public List<Employee> getAllEmployees() {
        return employees;
    }

    public Employee findEmployeeById(int id) {
        for (Employee emp : employees) {
            if (emp.getId() == id) {
                return emp;
            }
        }
        return null;
    }
}
package payroll;

public class PayrollService {

    public void generatePayslip(Employee employee) {
        double salary = employee.getBasicSalary();
        double tax = PayrollCalculator.calculateTax(salary);
        double pension = PayrollCalculator.calculatePension(salary);
        double netSalary = PayrollCalculator.calculateNetSalary(salary);

        System.out.println("\n------ PAYSLIP ------");
        System.out.println("Employee Name: " + employee.getName());
        System.out.println("Role: " + employee.getRole());
        System.out.println("Basic Salary: ₦" + salary);
        System.out.println("Tax (15%): ₦" + tax);
        System.out.println("Pension (8%): ₦" + pension);
        System.out.println("Net Salary: ₦" + netSalary);
        System.out.println("----------------------\n");
    }
}
package payroll;

import java.util.Scanner;

public class Main {

    public static void main(String[] args) {

        EmployeeRepository repository = new EmployeeRepository();
        PayrollService payrollService = new PayrollService();
        Scanner scanner = new Scanner(System.in);

        while (true) {
            System.out.println("\nEMPLOYEE PAYROLL SYSTEM");
            System.out.println("1. Add Employee");
            System.out.println("2. View All Employees");
            System.out.println("3. Generate Payslip");
            System.out.println("4. Exit");
            System.out.print("Choose an option: ");

            int choice = scanner.nextInt();

            switch (choice) {
                case 1:
                    System.out.print("Enter ID: ");
                    int id = scanner.nextInt();
                    scanner.nextLine();

                    System.out.print("Enter Name: ");
                    String name = scanner.nextLine();

                    System.out.print("Enter Role: ");
                    String role = scanner.nextLine();

                    System.out.print("Enter Basic Salary: ");
                    double salary = scanner.nextDouble();

                    Employee employee = new Employee(id, name, role, salary);
                    repository.addEmployee(employee);
                    System.out.println("Employee added successfully!");
                    break;

                case 2:
                    for (Employee emp : repository.getAllEmployees()) {
                        System.out.println(emp);
                    }
                    break;

                case 3:
                    System.out.print("Enter Employee ID: ");
                    int empId = scanner.nextInt();
                    Employee emp = repository.findEmployeeById(empId);

                    if (emp != null) {
                        payrollService.generatePayslip(emp);
                    } else {
                        System.out.println("Employee not found.");
                    }
                    break;

                case 4:
                    System.out.println("Exiting system...");
                    scanner.close();
                    System.exit(0);

                default:
                    System.out.println("Invalid option.");
            }
        }
    }
}

}

