# Class Assignment 1 - Report

## Introduction

The objective of this report is to furnish a comprehensive overview of the inaugural class assignment. Said assignment
entails the establishment of a GitHub repository employing Git via the Command Line Interface (CLI). It initially
involves the inception of a local repository, followed by the establishment of a remote repository, culminating in the
synchronization of both repositories.

A simple Spring Boot application served as the developmental platform to introduce new functionalities. However,
paramount emphasis was placed on the utilization of Git and GitHub. The demonstrated proficiencies in this assignment
span from the creation of local repositories using Git through the CLI, to the synchronization of said repositories with
a remote GitHub repository, alongside the creation and merging of diverse branches.

This Class Assignment was made by the student José Castro nº1231836 of class B and the outcome of the work undertaken in
this assignment can be
found [here](https://github.com/J2PCastro/devops-23-24-PSM-1231836).

## Table of Contents

1. [Setup](#setup)
2. [Adding Features](#adding-features)
    1. [Part 1: Adding job years field](#part-1-adding-job-years-field)
    2. [Part 2:](#part-2)
    3. [Part 2.1.: Adding a new feature to the application: email field](#21-adding-a-new-feature-to-the-application-email-field)
    4. [Part 2.2.: Create a new branch for fixing bugs: fix-invalid-email](#22-create-a-new-branch-for-fixing-bugs-named-fix-invalid-email)
3. [Alternative to Git: Mercurial](#alternative-to-git-mercurial)
4. [Conclusion](#conclusion)

## Setup

The first step is to clone [this repository](https://github.com/spring-guides/tut-react-and-spring-data-rest) to your
local machine. This repository contains a simple Spring Boot application that we will use to demonstrate the use of Git
and GitHub.
The only directory needed for this assignment is the `basic` directory. The other directories are not needed.
Issues should be created in GitHub to track the progress of the assignment. It is assumed that a remote repository as
already been created in GitHub. If not, it should be created before proceeding.

### 1: Create a local repository

The first task is to create a local repository. This is done by navigating to a directory in your machine and creating
an empty directory giving it a name, without any blank spaces, preferably in all lower case. After this open
the CLI in the directory and run the following commands:

```bash
git init
```

- This command initializes a new Git repository in the directory. This creates a hidden directory named `.git` in the
  directory. This directory contains all the necessary files for the repository.

### 2: Copy the Spring Boot application to the repository

The next task is to copy the Spring Boot application to the repository. This is done by copying the `basic` directory
from the cloned repository to the local repository.

```bash
cp -r path/to/cloned/repository/basic path/to/local/repository
```

- This command copies the `basic` directory from the cloned repository to the local repository.
- The `-r` flag is used to copy directories and their contents recursively.

### 3: Add the files to the repository

The next task is to add the files to the repository. This is done by running the following commands:

```bash
git add .
```

- This command adds all the files in the directory to the staging area making them ready to be committed. This is always
  necessary before committing files to the repository.
- The `.` is a wildcard that matches all files in the directory.

### 4: Commit the files to the repository

The next task is to commit the files to the repository. This is done by running the following commands:

```bash
git commit -m "Initial commit. closes #1"
```

- This command commits the files in the staging area to the repository.
- The `-m` flag is used to specify a message for the commit. This message should be a short description of the changes
  made in the commit.
- It is always a good practice to commit files with a descriptive message.
- The `closes #1` is used to close the issue created in GitHub. This is a good practice to keep track of the progress of
  the assignment.

### 5: Push the repository to GitHub

Assuming a remote repository in GitHub is already created, the next task is to push the commit to GitHub. This is done
by running the following commands:

```bash
git remote add origin <repository-url>
git push -u origin master
```

- The first command adds a remote repository named `origin` with the URL `<repository-url>`. This URL is the URL of the
  remote repository in GitHub. This first step is only necessary if the local repository is not yet linked to the remote
  one, as it is its function.
- The second command pushes the commit to the remote repository. The `-u` flag is used to set the remote repository as
  the default remote repository for the local repository. This means that in the future, the `git push` command can be
  used without specifying the remote repository and the branch to push to.

### 6: Add a new tag

The next task is to add a new tag to the repository. This is done by running the following commands:

```bash
git tag -a v1.1.0 -m "Initial Version"
git push origin v1.1.0
```

- This command adds a new tag named `v1.1.0` to the repository.
- The `-a` flag is used to specify that the tag is an annotated tag. An annotated tag is a tag that contains a message.
- The `-m` flag is used to specify a message for the tag. This message should be a short description of the changes made
  in the tag.
- The second command pushes the tag to the remote repository.

## Adding Features

### Part 1: Adding job years field

This first part is developed in the master branch. The goal is to add a new field to the application which will be the
number of years the user has been in the job.
The steps are as follows:

#### 1. Add the field to the `Employee` class.

```java
private int jobYears;
```

#### 2. Add the new field to the `Employee` class constructor.

```java
public Employee(String firstName, String lastName, String description, int jobYears) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.description = description;
    this.jobYears = jobYears;
}
```

#### 3. Add validation to the `Employee` class constructor so that the parameters are always valid.

```java
    public Employee(String firstName, String lastName, String description, int jobYears) {
    if (!validateArguments(firstName, lastName, description, jobYears)) {
        throw new IllegalArgumentException("Invalid arguments");
    }
    this.firstName = firstName;
    this.lastName = lastName;
    this.description = description;
    this.jobYears = jobYears;
}

private boolean validateArguments(String firstName, String lastName, String description, int jobYears, String email) {
    if (firstName == null || lastName == null || description == null || firstName.trim().isEmpty() || lastName.trim().isEmpty() || description.trim().isEmpty() || jobYears < 0) {
        return false;
    }
    return true;
}
```

#### 4. Add the new field to the `Employee` class `toString` method.

```java

@Override
public String toString() {
    return "Employee{" +
            "id=" + id +
            ", firstName='" + firstName + '\'' +
            ", lastName='" + lastName + '\'' +
            ", description='" + description + '\'' +
            ", jobYears=" + jobYears +
            '}';
}
```

#### 5. Add the new field to the `Employee` class `equals` and `hashCode` methods.

```java

@Override
public boolean equals(Object o) {
    if (this == o) return true;
    if (o == null || getClass() != o.getClass()) return false;
    Employee employee = (Employee) o;
    return jobYears == employee.jobYears && Objects.equals(id, employee.id) && Objects.equals(firstName, employee.firstName) && Objects.equals(lastName, employee.lastName) && Objects.equals(description, employee.description) && Objects.equals(email, employee.email);
}

@Override
public int hashCode() {
    return Objects.hash(id, firstName, lastName, description, jobYears, email);
}
```

#### 6. Add the new field to the `Employee` class `get` and `set` methods.

```java
    public void setJobYears(int jobYears) {
    if (jobYears < 0) {
        throw new IllegalArgumentException("Invalid job years");
    } else {
        this.jobYears = jobYears;
    }
}
```

#### 7. Create an EmployeeTest class to test the new field to ensure it is working as expected.

```java
class EmployeeTest {

    private Employee employee;

    @BeforeEach
    void setUp() {
        String firstName = "Frodo";
        String lastName = "Baggins";
        String description = "ring bearer";
        int jobYears = 1;
        employee = new Employee(firstName, lastName, description, jobYears);
    }

    @Test
    void testEmployee() {
        assertEquals("Frodo", employee.getFirstName());
        assertEquals("Baggins", employee.getLastName());
        assertEquals("ring bearer", employee.getDescription());
        assertEquals(1, employee.getJobYears());
    }

    @Test
    void testEmployeeEquality() {
//        Arrange
        Employee employee1 = new Employee("Frodo", "Baggins", "ring bearer", 1);
        Employee employee2 = new Employee("Frodo", "Baggins", "ring bearer", 1);
//        Act + Assert
        assertEquals(employee1, employee2);
    }

    @Test
    void testEmployeeInequality() {
//        Arrange
        Employee employee1 = new Employee("Frodo", "Baggins", "ring bearer", 1);
        Employee employee2 = new Employee("Frodo", "Baggins", "ring bearer", 2);
//        Act + Assert
        assertNotEquals(employee1, employee2);
    }

    @Test
    void testEmployeeHashCode() {
        assertEquals(-1026807203, employee.hashCode());
    }

    @Test
    void testEmployeeToString() {
        assertEquals("Employee{id=null, firstName='Frodo', lastName='Baggins', description='ring bearer', jobYears=1'}", employee.toString());
    }

    @Test
    void testEmployeeId() {
//        Arrange
        long newId = 10L;
//        Act
        employee.setId(newId);
//        Assert
        assertEquals(newId, employee.getId());
    }

    @Test
    void testEmployeeFirstName() {
//        Arrange
        String newFirstName = "Sam";
//        Act
        employee.setFirstName(newFirstName);
//        Assert
        assertEquals(newFirstName, employee.getFirstName());
    }

    @Test
    void testEmployeeLastName() {
//        Arrange
        String newLastName = "Gamgee";
//        Act
        employee.setLastName(newLastName);
//        Assert
        assertEquals(newLastName, employee.getLastName());
    }

    @Test
    void testEmployeeDescription() {
//        Arrange
        String newDescription = "gardener";
//        Act
        employee.setDescription(newDescription);
//        Assert
        assertEquals(newDescription, employee.getDescription());
    }

    @Test
    void testEmployeeJobYears() {
//        Arrange
        int newJobYears = 2;
//        Act
        employee.setJobYears(newJobYears);
//        Assert
        assertEquals(newJobYears, employee.getJobYears());
    }

    @Test
    void testEmployeeNullName() {
//        Arrange
        String firstName = null;
        String lastName = "baggins";
        String description = "ring bearer";
        int jobYears = 1;
//        Act + Assert
        assertThrows(IllegalArgumentException.class, () -> new Employee(firstName, lastName, description, jobYears));
    }

    @Test
    void testEmployeeEmptyName() {
//        Arrange
        String firstName = "";
        String lastName = "baggins";
        String description = "ring bearer";
        int jobYears = 1;
//        Act + Assert
        assertThrows(IllegalArgumentException.class, () -> new Employee(firstName, lastName, description, jobYears));
    }

    @Test
    void testEmployeeNullLastName() {
//        Arrange
        String firstName = "frodo";
        String lastName = null;
        String description = "ring bearer";
        int jobYears = 1;
//        Act + Assert
        assertThrows(IllegalArgumentException.class, () -> new Employee(firstName, lastName, description, jobYears));
    }

    @Test
    void testEmployeeEmptyLastName() {
//        Arrange
        String firstName = "frodo";
        String lastName = "";
        String description = "ring bearer";
        int jobYears = 1;
//        Act + Assert
        assertThrows(IllegalArgumentException.class, () -> new Employee(firstName, lastName, description, jobYears));
    }

    @Test
    void testEmployeeNullDescription() {
//        Arrange
        String firstName = "frodo";
        String lastName = "baggins";
        String description = null;
        int jobYears = 1;
//        Act + Assert
        assertThrows(IllegalArgumentException.class, () -> new Employee(firstName, lastName, description, jobYears));
    }

    @Test
    void testEmployeeEmptyDescription() {
//        Arrange
        String firstName = "frodo";
        String lastName = "baggins";
        String description = "";
        int jobYears = 1;
//        Act + Assert
        assertThrows(IllegalArgumentException.class, () -> new Employee(firstName, lastName, description, jobYears));
    }

    @Test
    void testEmployeeNegativeJobYears() {
//        Arrange
        String firstName = "frodo";
        String lastName = "baggins";
        String description = "ring bearer";
        int jobYears = -1;
//        Act + Assert
        assertThrows(IllegalArgumentException.class, () -> new Employee(firstName, lastName, description, jobYears));
    }

    @Test
    void testEmployeeNullEmail() {
//        Arrange
        String firstName = "frodo";
        String lastName = "baggins";
        String description = "ring bearer";
        int jobYears = 1;
//        Act + Assert
        assertThrows(IllegalArgumentException.class, () -> new Employee(firstName, lastName, description, jobYears));
    }

    @Test
    void testEmployeeEmptyEmail() {
//        Arrange
        String firstName = "frodo";
        String lastName = "baggins";
        String description = "ring bearer";
        int jobYears = 1;
//        Act + Assert
        assertThrows(IllegalArgumentException.class, () -> new Employee(firstName, lastName, description, jobYears));
    }

    @Test
    void testEmployeeSetNullFirstName() {
//        Arrange
        String firstName = null;
//        Act + Assert
        assertThrows(IllegalArgumentException.class, () -> employee.setFirstName(firstName));
    }

    @Test
    void testEmployeeSetEmptyFirstName() {
//        Arrange
        String firstName = "";
//        Act + Assert
        assertThrows(IllegalArgumentException.class, () -> employee.setFirstName(firstName));
    }

    @Test
    void testEmployeeSetNullLastName() {
//        Arrange
        String lastName = null;
//        Act + Assert
        assertThrows(IllegalArgumentException.class, () -> employee.setLastName(lastName));
    }

    @Test
    void testEmployeeSetEmptyLastName() {
//        Arrange
        String lastName = "";
//        Act + Assert
        assertThrows(IllegalArgumentException.class, () -> employee.setLastName(lastName));
    }

    @Test
    void testEmployeeSetNullDescription() {
//        Arrange
        String description = null;
//        Act + Assert
        assertThrows(IllegalArgumentException.class, () -> employee.setDescription(description));
    }

    @Test
    void testEmployeeSetEmptyDescription() {
//        Arrange
        String description = "";
//        Act + Assert
        assertThrows(IllegalArgumentException.class, () -> employee.setDescription(description));
    }

    @Test
    void testEmployeeSetNegativeJobYears() {
//        Arrange
        int jobYears = -1;
//        Act + Assert
        assertThrows(IllegalArgumentException.class, () -> employee.setJobYears(jobYears));
    }
}
```

#### 8. Add the new field to render methods in the `app.js` Javascript file:

```javascript
   class EmployeeList extends React.Component {
    render() {
        const employees = this.props.employees.map(employee =>
            <Employee key={employee._links.self.href} employee={employee}/>
        );
        return (
            <table>
                <tbody>
                    <tr>
                        <th>First Name</th>
                        <th>Last Name</th>
                        <th>Description</th>
                        <th>Job Years</th>
                    </tr>
                    {employees}
                </tbody>
            </table>
        )
    }
}
````

```javascript
class Employee extends React.Component {
    render() {
        return (
            <tr>
                <td>{this.props.employee.firstName}</td>
                <td>{this.props.employee.lastName}</td>
                <td>{this.props.employee.description}</td>
                <td>{this.props.employee.jobYears}</td>
            </tr>
        )
    }
}
````

#### 9. Add the new field to the run method in the `DatabaseLoader` class (you can also add new entries):

```java
    public void run(String... strings) throws Exception { // <4>
    this.repository.save(new Employee("Frodo", "Baggins", "ring bearer", 1));
}
```

#### Side note:

If needed, you can view the changes made to the files by running the following command:

1. Open the `basic` directory in the terminal.
2. Run the following command:

```bash
./mvnw spring-boot:run
```

3. In a browser, navigate to [here](http://localhost:8080/employees) to view the changes made to the application.

## Commit the changes to the repository

After all the changes are made, the next step is to commit the changes to the repository. This is done by running the
following commands:

```bash
git add .
git commit -m "Add new feature jobYears field and tests for this new feature. closes #2"
git push origin master
```

- The first command adds all the files in the directory to the staging area making them ready to be committed. This is
  always necessary before committing files to the repository.
- The second command commits the files in the staging area to the repository. The `closes #2` is used to close the issue
  created in GitHub.

## Create a new tag for the new version

After the changes are committed to the repository, the next step is to create a new tag for the new version:

```bash
git tag -a v1.2.0 -m "Version 1.2.0 with new JobYears feature"
git push origin v1.2.0
```

- The first command adds a new tag named `v1.2.0` to the repository.
- The second command pushes the tag to the remote repository.

## Create a new tag to mark the end of the first part of the assignment

After all the changes are made, the next step is to create a new tag to mark the end of the first part of the
assignment:

```bash
git tag -a ca1-part1 -m "End of part 1 of class assignment 1"
git push origin ca1-part1
```

- The first command adds a new tag named `ca1-part1` to the repository.
- The second command pushes the tag to the remote repository.

### Part 2:

### 2.1.: Adding a new feature to the application: email field

This second part of the assignment should be developed using branches. The master branch should be used to ”publish” the
”stable” versions of the Tutorial
React.js and Spring Data REST Application.

#### 1. Start by creating a new branch named `email-field`:

```bash
git checkout -b email-field
```

- This command creates a new branch named email-field and switches to it.
- The -b flag is used to create a new branch.
- The email-field is the name of the new branch.

#### 2. Add the field to the `Employee` class:

```java
private String email;
```

#### 3. Add the new field to the `Employee` class constructor and corresponding validations:

```java
    public Employee(String firstName, String lastName, String description, int jobYears, String email) {
    if (!validateArguments(firstName, lastName, description, jobYears, email)) {
        throw new IllegalArgumentException("Invalid arguments");
    }
    this.firstName = firstName;
    this.lastName = lastName;
    this.description = description;
    this.jobYears = jobYears;
    this.email = email;
}

private boolean validateArguments(String firstName, String lastName, String description, int jobYears, String email) {
    if (firstName == null || lastName == null || description == null || firstName.trim().isEmpty() || lastName.trim().isEmpty() || description.trim().isEmpty() || jobYears < 0) {
        return false;
    }
    if (email == null || email.trim().isEmpty()) {
        return false;
    }
    return true;
}
```

#### 4. Add the new field to the `Employee` class `toString` method:

```java

@Override
public String toString() {
    return "Employee{" +
            "id=" + id +
            ", firstName='" + firstName + '\'' +
            ", lastName='" + lastName + '\'' +
            ", description='" + description + '\'' +
            ", jobYears=" + jobYears +
            ", email='" + email + '\'' +
            '}';
}
```

#### 5. Add the new field to the `Employee` class `equals` and `hashCode` methods:

```java

@Override
public boolean equals(Object o) {
    if (this == o) return true;
    if (o == null || getClass() != o.getClass()) return false;
    Employee employee = (Employee) o;
    return jobYears == employee.jobYears && Objects.equals(id, employee.id) && Objects.equals(firstName, employee.firstName) && Objects.equals(lastName, employee.lastName) && Objects.equals(description, employee.description) && Objects.equals(email, employee.email);
}

@Override
public int hashCode() {
    return Objects.hash(id, firstName, lastName, description, jobYears, email);
}
```

#### 6. Add the new field to the `Employee` class `get` and `set` methods:

```java
public String getEmail() {
    return email;

}

public void setEmail(String email) {
    if (email == null || email.trim().isEmpty()) {
        throw new IllegalArgumentException("Invalid email");
    } else {
        this.email = email;
    }
}
```

#### 7. Create an EmployeeTest class to test the new field to ensure it is working as expected:

```java
class EmployeeTest {

    private Employee employee;

    @BeforeEach
    void setUp() {
        String firstName = "Frodo";
        String lastName = "Baggins";
        String description = "ring bearer";
        int jobYears = 1;
        String email = "email@gmail.com";
        employee = new Employee(firstName, lastName, description, jobYears, email);
    }

    @Test
    void testEmployee() {
        assertEquals("Frodo", employee.getFirstName());
        assertEquals("Baggins", employee.getLastName());
        assertEquals("ring bearer", employee.getDescription());
        assertEquals(1, employee.getJobYears());
        assertEquals("email@gmail.com", employee.getEmail());
    }

    @Test
    void testEmployeeEquality() {
//        Arrange
        Employee employee1 = new Employee("Frodo", "Baggins", "ring bearer", 1, "email@gmail.com");
        Employee employee2 = new Employee("Frodo", "Baggins", "ring bearer", 1, "email@gmail.com");
//        Act + Assert
        assertEquals(employee1, employee2);
    }

    @Test
    void testEmployeeInequality() {
//        Arrange
        Employee employee1 = new Employee("Frodo", "Baggins", "ring bearer", 1, "email@gmail.com");
        Employee employee2 = new Employee("Frodo", "Baggins", "ring bearer", 2, "email@gmail.com");
//        Act + Assert
        assertNotEquals(employee1, employee2);
    }

    @Test
    void testEmployeeHashCode() {
        assertEquals(-1026807203, employee.hashCode());
    }

    @Test
    void testEmployeeToString() {
        assertEquals("Employee{id=null, firstName='Frodo', lastName='Baggins', description='ring bearer', jobYears=1, email='email@gmail.com'}", employee.toString());
    }

    @Test
    void testEmployeeId() {
//        Arrange
        long newId = 10L;
//        Act
        employee.setId(newId);
//        Assert
        assertEquals(newId, employee.getId());
    }

    @Test
    void testEmployeeFirstName() {
//        Arrange
        String newFirstName = "Sam";
//        Act
        employee.setFirstName(newFirstName);
//        Assert
        assertEquals(newFirstName, employee.getFirstName());
    }

    @Test
    void testEmployeeLastName() {
//        Arrange
        String newLastName = "Gamgee";
//        Act
        employee.setLastName(newLastName);
//        Assert
        assertEquals(newLastName, employee.getLastName());
    }

    @Test
    void testEmployeeDescription() {
//        Arrange
        String newDescription = "gardener";
//        Act
        employee.setDescription(newDescription);
//        Assert
        assertEquals(newDescription, employee.getDescription());
    }

    @Test
    void testEmployeeJobYears() {
//        Arrange
        int newJobYears = 2;
//        Act
        employee.setJobYears(newJobYears);
//        Assert
        assertEquals(newJobYears, employee.getJobYears());
    }

    @Test
    void testEmployeeEmail() {
//        Arrange
        String newEmail = "email@hotmail.com";
//        Act
        employee.setEmail(newEmail);
//        Assert
        assertEquals(newEmail, employee.getEmail());
    }

    @Test
    void testEmployeeNullName() {
//        Arrange
        String firstName = null;
        String lastName = "baggins";
        String description = "ring bearer";
        String email = "email@gmail.com";
        int jobYears = 1;
//        Act + Assert
        assertThrows(IllegalArgumentException.class, () -> new Employee(firstName, lastName, description, jobYears, email));
    }

    @Test
    void testEmployeeEmptyName() {
//        Arrange
        String firstName = "";
        String lastName = "baggins";
        String description = "ring bearer";
        int jobYears = 1;
        String email = "email@gmail.com";
//        Act + Assert
        assertThrows(IllegalArgumentException.class, () -> new Employee(firstName, lastName, description, jobYears, email));
    }

    @Test
    void testEmployeeNullLastName() {
//        Arrange
        String firstName = "frodo";
        String lastName = null;
        String description = "ring bearer";
        int jobYears = 1;
        String email = "email@gmail.com";
//        Act + Assert
        assertThrows(IllegalArgumentException.class, () -> new Employee(firstName, lastName, description, jobYears, email));
    }

    @Test
    void testEmployeeEmptyLastName() {
//        Arrange
        String firstName = "frodo";
        String lastName = "";
        String description = "ring bearer";
        int jobYears = 1;
        String email = "email@gmail.com";
//        Act + Assert
        assertThrows(IllegalArgumentException.class, () -> new Employee(firstName, lastName, description, jobYears, email));
    }

    @Test
    void testEmployeeNullDescription() {
//        Arrange
        String firstName = "frodo";
        String lastName = "baggins";
        String description = null;
        int jobYears = 1;
        String email = "email@gmail.com";
//        Act + Assert
        assertThrows(IllegalArgumentException.class, () -> new Employee(firstName, lastName, description, jobYears, email));
    }

    @Test
    void testEmployeeEmptyDescription() {
//        Arrange
        String firstName = "frodo";
        String lastName = "baggins";
        String description = "";
        int jobYears = 1;
        String email = "email@gmail.com";
//        Act + Assert
        assertThrows(IllegalArgumentException.class, () -> new Employee(firstName, lastName, description, jobYears, email));
    }

    @Test
    void testEmployeeNegativeJobYears() {
//        Arrange
        String firstName = "frodo";
        String lastName = "baggins";
        String description = "ring bearer";
        int jobYears = -1;
        String email = "email@gmail.com";
//        Act + Assert
        assertThrows(IllegalArgumentException.class, () -> new Employee(firstName, lastName, description, jobYears, email));
    }

    @Test
    void testEmployeeNullEmail() {
//        Arrange
        String firstName = "frodo";
        String lastName = "baggins";
        String description = "ring bearer";
        int jobYears = 1;
        String email = null;
//        Act + Assert
        assertThrows(IllegalArgumentException.class, () -> new Employee(firstName, lastName, description, jobYears, email));
    }

    @Test
    void testEmployeeEmptyEmail() {
//        Arrange
        String firstName = "frodo";
        String lastName = "baggins";
        String description = "ring bearer";
        int jobYears = 1;
        String email = " ";
//        Act + Assert
        assertThrows(IllegalArgumentException.class, () -> new Employee(firstName, lastName, description, jobYears, email));
    }

    @Test
    void testEmployeeSetNullFirstName() {
//        Arrange
        String firstName = null;
//        Act + Assert
        assertThrows(IllegalArgumentException.class, () -> employee.setFirstName(firstName));
    }

    @Test
    void testEmployeeSetEmptyFirstName() {
//        Arrange
        String firstName = "";
//        Act + Assert
        assertThrows(IllegalArgumentException.class, () -> employee.setFirstName(firstName));
    }

    @Test
    void testEmployeeSetNullLastName() {
//        Arrange
        String lastName = null;
//        Act + Assert
        assertThrows(IllegalArgumentException.class, () -> employee.setLastName(lastName));
    }

    @Test
    void testEmployeeSetEmptyLastName() {
//        Arrange
        String lastName = "";
//        Act + Assert
        assertThrows(IllegalArgumentException.class, () -> employee.setLastName(lastName));
    }

    @Test
    void testEmployeeSetNullDescription() {
//        Arrange
        String description = null;
//        Act + Assert
        assertThrows(IllegalArgumentException.class, () -> employee.setDescription(description));
    }

    @Test
    void testEmployeeSetEmptyDescription() {
//        Arrange
        String description = "";
//        Act + Assert
        assertThrows(IllegalArgumentException.class, () -> employee.setDescription(description));
    }

    @Test
    void testEmployeeSetNegativeJobYears() {
//        Arrange
        int jobYears = -1;
//        Act + Assert
        assertThrows(IllegalArgumentException.class, () -> employee.setJobYears(jobYears));
    }

    @Test
    void testEmployeeSetNullEmail() {
//        Arrange
        String email = null;
//        Act + Assert
        assertThrows(IllegalArgumentException.class, () -> employee.setEmail(email));
    }

    @Test
    void testEmployeeSetEmptyEmail() {
//        Arrange
        String email = " ";
//        Act + Assert
        assertThrows(IllegalArgumentException.class, () -> employee.setEmail(email));
    }
}
```

#### 8. Add the new field to render methods in the `app.js` Javascript file:

```javascript
class EmployeeList extends React.Component {
    render() {
        const employees = this.props.employees.map(employee =>
            <Employee key={employee._links.self.href} employee={employee}/>
        );
        return (
            <table>
                <tbody>
                    <tr>
                        <th>First Name</th>
                        <th>Last Name</th>
                        <th>Description</th>
                        <th>Job Years</th>
                        <th>Email</th>
                    </tr>
                    {employees}
                </tbody>
            </table>
        )
    }
}
```

```javascript
class Employee extends React.Component {
    render() {
        return (
            <tr>
                <td>{this.props.employee.firstName}</td>
                <td>{this.props.employee.lastName}</td>
                <td>{this.props.employee.description}</td>
                <td>{this.props.employee.jobYears}</td>
                <td>{this.props.employee.email}</td>
            </tr>
        )
    }
}
```

#### 9. Add the new field to the run method in the `DatabaseLoader` class (you can also add new entries):

```java
    public void run(String... strings) throws Exception { // <4>
    this.repository.save(new Employee("Frodo", "Baggins", "ring bearer", 1, "email@gmail.com"));
}
```

## Commit the changes to the repository

After all the changes are made, the next step is to commit the changes. Since we are working with branches, things are
going to be slightly different. This is done by running the following commands:

```bash
git add .
git commit -m " Add new feature email field and tests for this new feature. closes #3"
git push origin email-field
```

- The first command adds all the files in the directory to the staging area making them ready to be committed.
- The second command commits the files in the staging area to the repository. The `closes #3` is used to close the issue
  created in GitHub.
- The third command pushes the commit to the remote repository. Since the branch wasn't pushed the remote repository
  before, this command not only pushes the commits but also the new branch to the remote repository.

## Merge the branch to the master branch

After all this process it is necessary to merge the branch to the master branch. This is done by running the following
commands:

```bash
git checkout master
git merge --no-ff email-field
git push origin master
```

- The first command switches to the master branch.
- The second command merges the email-field branch to the master branch. The --no-ff flag is used to ensure that a new
  commit is created to merge the branches. This is always a good practice to keep track of the changes made in the
  branches.
- The third command pushes the commit to the remote repository.

## Create a new tag for the new version

After the changes are committed to the repository, the next step is to create a new tag for the new version:

```bash
git tag -a v1.3.0 -m "Version 1.3.0 with new feature email field"
git push origin v1.3.0
```

- The first command adds a new tag named `v1.3.0` to the repository.
- The second command pushes the tag to the remote repository.

### 2.2.: Create a new branch for fixing bugs named `fix-invalid-email`:

Now that the email field is added to the application, it is necessary to fix a bug that allows invalid emails to be
added to the application. This is done by creating a new branch named `fix-invalid-email`:

```bash
git checkout -b fix-invalid-email
```

- This command creates a new branch named fix-invalid-email and switches to it.

#### 1. Add validation to the method validateArguments in the `Employee` class:

```java
    private boolean validateArguments(String firstName, String lastName, String description, int jobYears, String email) {
    if (firstName == null || lastName == null || description == null || firstName.trim().isEmpty() || lastName.trim().isEmpty() || description.trim().isEmpty() || jobYears < 0) {
        return false;
    }
    if (email == null || email.trim().isEmpty() || !email.contains("@") || !email.contains(".")) {
        return false;
    }
    return true;
}
```

#### 2. Add validation to the method setEmail in the `Employee` class:

```java
    public void setEmail(String email) {
    if (email == null || email.trim().isEmpty() || !email.contains("@") || !email.contains(".")) {
        throw new IllegalArgumentException("Invalid email");
    } else {
        this.email = email;
    }
}
```

#### 3. Add tests to the EmployeeTest class to ensure that the new validation is working as expected:

```java

@Test
void testEmployeeSetInvalidEmailSecondTest() {
//        Arrange
    String email = "email@com";
//        Act + Assert
    assertThrows(IllegalArgumentException.class, () -> employee.setEmail(email));
}

@Test
void testCreateEmployeeWithInvalidEmail() {
//         Arrange
    String firstName = "Frodo";
    String lastName = "Baggins";
    String description = "ring bearer";
    int jobYears = 1;
    String email = "emailemail.com";
//         Act + Assert
    assertThrows(IllegalArgumentException.class, () -> new Employee(firstName, lastName, description, jobYears, email));
}

@Test
void testCreateEmployeeWithInvalidEmailSecondTest() {
//         Arrange
    String firstName = "Frodo";
    String lastName = "Baggins";
    String description = "ring bearer";
    int jobYears = 1;
    String email = "email@com";
//         Act + Assert
    assertThrows(IllegalArgumentException.class, () -> new Employee(firstName, lastName, description, jobYears, email));
}
```

## Commit the changes to the repository

After all the changes are made, the next step is to commit the changes. This is done by running the following commands:

```bash
git add .
git commit -m "Add fixes so that email field only takes in valid email. closes #4"
git push origin fix-invalid-email
```

- The first command adds all the files in the directory to the staging area making them ready to be committed.
- The second command commits the files in the staging area to the repository. The `closes #4` is used to close the issue
  created in GitHub.
- The third command pushes the commit to the remote repository. Since the branch wasn't pushed the remote repository
  before, this command not only pushes the commits but also the new branch to the remote repository.

## Merge the branch to the master branch

After all this process it is necessary to merge the branch to the master branch. This is done by running the following
commands:

```bash
git checkout master
git merge --no-ff fix-invalid-email
git push origin master
```

- The first command switches to the master branch.
- The second command merges the fix-email-field branch to the master branch. The --no-ff flag is used to ensure that a
  new
  commit is created to merge the branches. This is always a good practice to keep track of the changes made in the
  branches.
- The third command pushes the commit to the remote repository.

## Create a new tag for the new version

After the changes are committed to the repository, the next step is to create a new tag for the new version:

```bash
git tag -a v1.3.1 -m "Version 1.3.1 with bug fixes to email field"
git push origin v1.3.0
```

- The first command adds a new tag named `v1.3.1` to the repository.
- The second command pushes the tag to the remote repository.

## Create a new tag to mark the end of the second part of the assignment

After all the changes are made, the next step is to create a new tag to mark the end of the second part of the
assignment:

```bash
git tag -a ca1-part2 -m "End of part 2 of class assignment 1"
git push origin ca1-part2
```

- The first command adds a new tag named `ca1-part2` to the repository.
- The second command pushes the tag to the remote repository.

## Alternative to Git: Mercurial

Mercurial is a distributed version control system (DVCS) designed for managing projects of all sizes, from small to very
large. It was created by Matt Mackall in 2005 and is written mostly in Python. Like Git, Mercurial allows multiple
developers to work on the same project simultaneously while keeping track of changes made to the source code.

Mercurial and Git are both popular distributed version control systems (DVCS) used in software development, each with
its own strengths and nuances. While Git is more widely adopted and boasts a larger ecosystem of tools and services,
Mercurial is praised for its simplicity and ease of use. Git's branching model is more flexible and powerful, making it
ideal for complex projects with intricate workflows, whereas Mercurial offers a more straightforward approach to
branching that may appeal to teams seeking simplicity and clarity. Additionally, Git's command-line interface is more
feature-rich and customizable, while Mercurial's interface is often considered more intuitive and user-friendly.
Ultimately, the choice between Mercurial and Git depends on factors such as project complexity, team preferences, and
specific workflow requirements.

### Using Mercurial to the goals of this assignment

If needed, Mercurial can be used to achieve the same goals as Git. To do so, one would follow a similar workflow with
some differences in the commands used:

#### 1. Initializing a new repository

Different from Git, Mercurial uses the `hg` command to perform operations on the repository. To create a new repository,
the following command can be used:

```bash
cd path/to/directory
```

#### 2. Run the following command to create a new repository:

```bash
hg init
```

#### 3. Add the files to the repository:

```bash
hg add .
```

#### 4. Commit the files to the repository:

```bash
hg commit -m "Initial commit"
```

#### 5. Push the repository to a remote location:

Similar to `git push`, Mercurial uses the `hg push` command to push the repository to a remote location:

```bash
hg push <repository-URL>
```

Alternatively, if we only want to push changes to the remote repository, the following command can be used:

```bash
hg push
```

#### 6. Add a tag to the commit:

To add a tag to the commit, the following command can be used:

```bash
hg tag -m "Version 1.0.0" v1.0.0
```

#### 7. Pushing tags:

To push the tags to the remote repository, the following command can be used:

```bash
hg push --new-branch
```

In order to follow the steps of the assignment, the commands used in the tutorial can be replaced by the corresponding
Mercurial commands. For example, to create a new branch in Mercurial, the following command can be used:

#### 8. Creating branches in Mercurial:

```bash
hg branch <branch-name>
```

#### 9. Switching branches:

```bash
hg update <branch-name>
```

#### 10. Merging branches:

```bash
hg merge <branch-name>
```

## Conclusion

In this article, we have discussed the process of using Git to manage a software development project. We
have covered the basic concepts of version control, the benefits of using Git, and the steps involved in setting up a
Git repository, making changes to the code, and collaborating with other developers. We have also discussed the
importance of using branches to manage different features and bug fixes, and the process of merging branches and
creating
tags to mark the end of different parts of the assignment. Finally, we have briefly discussed the alternative of using
Mercurial as a distributed version control system. By following these best practices and using the right tools, software
development teams can effectively manage their projects and collaborate with other developers to build high-quality
software products.

Hope the work developed here meets all exceptions for this class assignment. Thank you for your attention.