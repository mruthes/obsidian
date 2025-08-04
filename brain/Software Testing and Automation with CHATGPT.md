**1) User story creation**  

Requirement: As a user I should able to login in ecommerce application so that user can access all teh features from the application.

**Prompt:** 

Create user stories for the following requirement.

Requirement: As a user I should able to login in ecommerce application so that user can access all teh features from the application.

**2) Test case development**

**Prompt:** Create test cases for the following user story

User Story : Basic Login

Title: User Login with Email and Password

As a user,

I want to log in using my email and password,

so that I can access all the features of the ecommerce application.

Acceptance Criteria:

User should be able to navigate to the login page from the homepage.

User should see fields for email and password.

User should be able to enter their email and password.

User should be able to click a "Login" button.

If the email and password are correct, the user should be redirected to their dashboard or homepage.

If the email or password is incorrect, an error message should be displayed.

**3) Syntax Error detection and correction**

**Prompt:** Find syntax errors and correct the below java code.

public class Testing {

    public static void main(String[] args) {

        System.out.println("Hello, World!);

        int number = 10

        if (number = 10) {

            System.out.println("Number is ten");

        } else {

            System.out.println("Number is not ten";

        }

        for (int i = 0; i < 5; i++) {

            System.out.println("i is: " + i;

        }

        String[] names = {"Alice", "Bob", "Charlie"};

        for (String name : names {

            System.out.println(name);

        }

    }

}

**4) Logical Error Detection and Correction:**

**Prompt:** Find out logical errors and correct the below java code.

public class LargestOfTwoNumbers {

    public static void main(String[] args) {

        int num1 = 10;

        int num2 = 20;

        // Logical error: Using ">" instead of ">="

        if (num1 > num2) {

            System.out.println("The largest number is: " + num1);

        } else {

            System.out.println("The largest number is: " + num1); 

        }

    }

}

**5) Test Automation script writing.**

**Prompt:** Write automation test script using Selenium with Java and TestNG for the following test case.

Test Case : Successful User Registration

Preconditions: The user is on the registration page.

Test Steps:

1) Launch chrome browser and open URL "https://demo.nopcommerce.com/"

1) Navigate to the registration page.

2) Enter valid data in all mandatory fields (e.g., first name, last name, email, password, confirm password).

3) Select any required options (e.g., gender, newsletter subscription).

4) Click the "Register" button.

Expected Result: The user is successfully registered and redirected to a welcome page or their account dashboard. 

**6) XPath locator creation**

**Prompt:** Write Selenium XPath Locators for all the elements in the page "https://demo.nopcommerce.com/"

**7) Page Object Model(POM) class creation**

**Prompt 1:** I have a web page "https://demo.nopcommerce.com/login". Create Login page object class for Selenium Java automation framework without Page Factory.

**Prompt 2:** I have a web page "https://demo.nopcommerce.com/login". Create Login page page object class for Selenium Java automation framework with Page Factory.

**8) BDD Feature file scenarios creation.**

**Prompt:** Create feature file scenarios for the test case. 

Test case Title: Successful User Registration

Preconditions: The user is on the registration page.

Test Steps:

1) Navigate to the registration page.

2) Enter valid data in all mandatory fields (e.g., first name, last name, email, password, confirm password).

3) Select any required options (e.g., gender, newsletter subscription).

4) Click the "Register" button.

Expected Result: The user is successfully registered and redirected to a welcome page or their account dashboard. 

**9) Utility creation**

**Prompt 1:** " Create an utility in java to handle excel sheet using Apache POI . I would like to do following operations 1) Count number of rows in excel sheet 2) Count Number of cells in a row 3) Read data from cell 4) Write data into cell.” 

**Prompt 2:** " Create an utility in java to handle MySQL database . I would like to do following operations 1) read data from the table 2) insert data into table 3) update data into table 4) delete rows from a table." 

**10) Test data generation**

**Prompt 1:** " Create test data for registration form in tabular format. Data includes  First Name, Last Name, DOB, Email, Password, Conform Password.” 

**Prompt 2:** " Create test data for registration form in JSON format. Data includes  First Name, Last Name, DOB, Email, Password, Conform Password.” 

**11) Code Review and Understanding:**

**Prompt:** Explain the below code

public class StringReversal {

    public static void main(String[] args) {

        String original = "Hello, World!";

        String reversed = reverseString(original);

        System.out.println("Original: " + original);

        System.out.println("Reversed: " + reversed);

    }

    public static String reverseString(String str) {

        StringBuilder reversed = new StringBuilder();

        for (int i = str.length() - 1; i >= 0; i--) {

            reversed.append(str.charAt(i));

        }

        return reversed.toString();

    }

}



Source: [[SDET-QA]]
Related:[[Prompts for testing automation]]
Tags: #prompts, #chatgpt

