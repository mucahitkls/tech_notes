
Let's consider a real-world application of an automated report generation system for a company that generates different types of reports (e.g., financial, employee performance, and resource utilization) depending on the department's needs. Initially, the system isn't designed with the Open/Closed Principle in mind, leading to a rigid and less maintainable structure.

## Before Applying OCP

### Initial System:

- `ReportGenerator`: A class that handles report generation for different types of reports based on a parameter.

#### report_generator.py (Before)

```python
class ReportGenerator:
    def __init__(self):
        self.reports = []

    def generate_report(self, data, report_type):
        if report_type == 'financial':
            return self.generate_financial_report(data)
        elif report_type == 'employee':
            return self.generate_employee_report(data)
        # More report types lead to more conditional branches

    def generate_financial_report(self, data):
        # Complex logic for generating financial reports
        print(f"Generating financial report with {data}")

    def generate_employee_report(self, data):
        # Complex logic for generating employee performance reports
        print(f"Generating employee report with {data}")

# Usage
report_gen = ReportGenerator()
report_gen.generate_report({"data": "Finance Data"}, "financial")
report_gen.generate_report({"data": "Employee Data"}, "employee")
```

In this non-OCP-compliant design, adding a new report type requires modifying the `ReportGenerator` class, specifically the `generate_report` method.

## After Applying OCP

### Refactored System:

- Abstract `Report` class for all types of reports.
- Specific classes for each report type (FinancialReport, EmployeeReport).
- `ReportGenerator` class now utilizes a report object to generate reports.

#### report_types.py (After)

```python
from abc import ABC, abstractmethod

# Abstract report class
class Report(ABC):
    @abstractmethod
    def generate(self, data):
        pass

class FinancialReport(Report):
    def generate(self, data):
        print(f"Generating financial report with {data}")

class EmployeeReport(Report):
    def generate(self, data):
        print(f"Generating employee report with {data}")

# Add more report types as needed without modifying existing classes
class ResourceUtilizationReport(Report):
    def generate(self, data):
        print(f"Generating resource utilization report with {data}")
```

#### report_generator.py (After)

```python
class ReportGenerator:
    def __init__(self, report):
        self.report = report

    def generate_report(self, data):
        self.report.generate(data)

# Usage
financial_report = FinancialReport()
employee_report = EmployeeReport()
resource_report = ResourceUtilizationReport()

report_gen = ReportGenerator(financial_report)
report_gen.generate_report({"data": "Finance Data"})

report_gen = ReportGenerator(employee_report)
report_gen.generate_report({"data": "Employee Data"})

report_gen = ReportGenerator(resource_report)
report_gen.generate_report({"data": "Resource Data"})
```

## Conclusion

In the refactored example:

- `Report` serves as an abstract base class for all report types.
- Each specific report type (FinancialReport, EmployeeReport, ResourceUtilizationReport) extends the `Report` class.
- `ReportGenerator` is now open for extension (new report types) and closed for modification. New report types can be added without changing existing code.

This OCP-compliant design greatly enhances the system's maintainability and scalability. As new reporting requirements arise, the system can be easily extended with new types of reports without disrupting the existing functionality. This design emphasizes the benefits of using the Open/Closed Principle in a real-world application, leading to a more robust, adaptable, and maintainable codebase.