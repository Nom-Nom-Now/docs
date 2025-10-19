# Nom Nom Now - Software Requirements Specification 

## Table of contents
- [Table of contents](#table-of-contents)
- [1. Introduction](#1-introduction)
    - [1.1 Purpose](#11-purpose)
    - [1.2 Scope](#12-scope)
    - [1.3 Definitions, Acronyms and Abbreviations](#13-definitions-acronyms-and-abbreviations)
    - [1.4 References](#14-references)
    - [1.5 Overview](#15-overview)
- [2. Overall Description](#2-overall-description)
    - [2.1 Vision](#21-vision)
    - [2.2 Use Case Diagram](#22-use-case-diagram)
	- [2.3 Technology Stack](#23-technology-stack)
- [3. Specific Requirements](#3-specific-requirements)
    - [3.1 Functionality](#31-functionality)
    - [3.2 Usability](#32-usability)
    - [3.3 Reliability](#33-reliability)
    - [3.4 Performance](#34-performance)
    - [3.5 Supportability](#35-supportability)
    - [3.6 Design Constraints](#36-design-constraints)
    - [3.7 Online User Documentation and Help System Requirements](#37-online-user-documentation-and-help-system-requirements)
    - [3.8 Purchased Components](#38-purchased-components)
    - [3.9 Interfaces](#39-interfaces)
    - [3.10 Licensing Requirements](#310-licensing-requirements)
    - [3.11 Legal, Copyright and Other Notices](#311-legal-copyright-and-other-notices)
    - [3.12 Applicable Standards](#312-applicable-standards)
- [4. Supporting Information](#4-supporting-information)

## 1. Introduction

### 1.1 Purpose
This Software Requirements Specification (SRS) describes all specifications for the application "Nom Nom Now". It includes an overview about this project and its vision, detailed information about the planned features and boundary conditions of the development process.


### 1.2 Scope
The project is going to be realized as a Web App.
  
Planned Subsystems are: 
* **View Recipes:** The user wants to be able to look at recipes.
* **Create Recipes:** The user wants to create recipes.
* **Edit Recipes:** The user wants to edit recipes.
* **Create Categories:** The user wants to create categories.
* **Edit Categories:** The user wants to edit categories.
* **Weekly Overview:** The user wants an overview of recipes for the whole week.
* **Account Management:** The user wants to save the recipes to an Account.

### 1.3 Definitions, Acronyms and Abbreviations
| Abbreviation | Explanation                            |
| ------------ | -------------------------------------- |
| SRS          | Software Requirements Specification    |
| UC           | Use Case                               |
| n/a          | not applicable                         |
| tbd          | to be determined                       |
| UCD          | overall Use Case Diagram               |
| FAQ          | Frequently asked Questions             |

### 1.4 References

| Title                                                                               |    Date    | Publishing organization |
|-------------------------------------------------------------------------------------|:----------:|-------------------------|
| [Nom Nom Now Blog](https://github.com/orgs/Nom-Nom-Now/discussions/categories/blog) | 18.10.2025 | Nom Nom Now             |
| [GitHub](https://github.com/Nom-Nom-Now)                                            | 18.10.2025 | Nom Nom Now             |


### 1.5 Overview
The following chapter provides an overview of this project with vision and Overall Use Case Diagram. The third chapter (Requirements Specification) delivers more details about the specific requirements in terms of functionality, usability and design parameters. Finally there is a chapter with supporting information. 
    
## 2. Overall Description

### 2.1 Vision
Nom Nom Now is a web-based application designed for meal planning. Its goal is to assist users in planning a conscious and varied diet. The application will allow users to set daily themes (e.g., "Asian" or "Vegetarian"), based on which it will automatically generate a weekly meal plan with corresponding recipes. From this plan, a shopping list can be generated. The system aims to help users save time, support healthier eating habits, and diversify their daily meals.

### 2.2 Use Case Diagram

![OUCD][OUCD]

- Green: Planned till end of december
- Yellow: Planned till end of june

### 2.3 Technology Stack
The technology we use is:

Tools:
- GitHub Organization (The blog is also posted there)
- vorzugsweise IntelliJ
- Jira (Scrum Board)
- Google Docs (Notes)
- Discord (Kommunikation)

Frontend:
- Vue.js
- Vite
- TypeScript
- i18n

Backend:
- Java mit Spring Boot
- PostgreSQL
- Cache (Redis?)
- Docker

## 3. Specific Requirements

### 3.1 Functionality
This section will explain the different use cases, you could see in the Use Case Diagram, and their functionality.  
Until December we plan to implement:
- 3.1.1 Creating a category
- 3.1.2 Getting an overview
- 3.1.3 Add Categories to the overview of the week
- 3.1.4 Creating an account
- 3.1.5 Logging in
- 3.1.6 Logging out

Until June, we want to implement:
- 3.1.7 creating recipes
- 3.1.8 editing recipes
- 3.1.9 tbd
- 3.1.10 tbd
- 3.1.11 tbd

#### 3.1.1 Creating a category
This feature allows the user to create custom categories to organize recipes. These categories, such as "Vegetarian," "Asian," or "Quick Meals," can then be used to filter recipes and plan the weekly menu.

#### 3.1.2 Getting an overview
This feature provides the user with a "Weekly Overview" of their meal plan. It will display the planned recipes or daily mottos for each day of the week, helping the user to see their plan at a glance.

#### 3.1.3 Add Categories to the overview of the week
This feature allows the user to assign the previously created categories or "mottos" (e.g., "Asian Day") to specific days in their weekly overview. The app can then use this information to suggest suitable recipes.

#### 3.1.4 Creating an account
To save personal recipes, categories and meal plans, the user must be able to create an account. This feature allows a new user to register for the service.

#### 3.1.5 Logging in
This feature allows a registered user to log in to their account to access and manage their personal data, such as recipes and weekly plans.

#### 3.1.6 Logging out
To protect user privacy, this feature allows a user to securely log out of their account, ending the current session.

#### 3.1.7 Creating recipes
This is a core feature of the application. It enables users to add their own recipes, including details such as ingredients, preparation steps and cooking time, and assign them to categories.

#### 3.1.8 Editing recipes
This feature allows users to modify their saved recipes. This is useful for adjusting ingredients, refining preparation steps or correcting details.

### 3.2 Usability
We plan on designing the user interface as intuitive and self-explanatory as possible to make the user feel as comfortable as possible using the app. Though an FAQ document will be available, it should not be necessary to use it.

#### 3.2.1 No training time needed
The application will be designed with a clear, logical and user-friendly interface. Key functions such as creating a recipe, planning the week and generating a shopping list should be immediately accessible and understandable. The goal is that a new user can start using the app productively without requiring a tutorial or help documentation.

#### 3.2.2 Familiar Feeling
The design will adhere to established conventions and patterns for modern web applications. By using familiar icons, navigation structures and interactive elements, we ensure that users can apply their existing knowledge from other applications. This reduces the cognitive load and makes the user experience feel natural and predictable.

### 3.3 Reliability

#### 3.3.1 Availability
The application, including all its subsystems like recipe access and account management, shall be available 95% of the time. Planned maintenance and updates that could lead to downtime should be scheduled for periods of low usage (e.g., late at night) to minimize the impact on users.

#### 3.3.2 Defect Rate
The system must be designed to prevent any loss of user-generated data, such as saved recipes, weekly plans and account information. The database (PostgreSQL) will be managed with robust backup and recovery strategies. Critical user actions, like saving a recipe or creating an account, must be transactional to ensure data integrity, even in the case of a server error.

### 3.4 Performance

#### 3.4.1 Capacity
The system architecture, based on Spring Boot and Docker, must be scalable to handle a large number of concurrent users. It should manage thousands of requests per minute—for viewing recipes, updating weekly plans or generating shopping lists—without a noticeable degradation in performance.

#### 3.4.2 Storage 
The application should be efficient in its use of server-side storage. The database schema will be optimized to store user data (recipes, plans, etc.) compactly. The frontend, built with Vue.js and Vite, will be optimized to ensure small bundle sizes and leverage browser caching, resulting in fast initial load times and a lightweight client-side footprint.

#### 3.4.3 App performance / Response time
User interactions should feel instantaneous. Critical actions, such as loading a recipe page, performing a search or adding a meal to the weekly plan, should have a server response time of under one second under normal load conditions. The frontend will be optimized to render pages quickly and provide immediate feedback for user actions.

### 3.5 Supportability

#### 3.5.1 Coding Standards
To ensure high code quality and long-term maintainability, we adhere to established coding standards. For the backend in Java with Spring Boot, we follow the official Java code conventions. For the frontend in TypeScript with Vue.js, we align with community best practices. Variable names and methods will be named descriptively to improve code readability for all team members and simplify onboarding into new areas of the project.

#### 3.5.2 Testing Strategy
The application will have high test coverage to ensure functionality and prevent regressions. In the backend, unit tests (with JUnit) will be written for the business logic, and integration tests for the REST API endpoints. In the frontend, components will be secured with unit tests (e.g., with Vitest), and critical user flows will be covered by end-to-end tests. This ensures that errors are detected and fixed early.

### 3.6 Design Constraints
The application will be developed as a modern web application with a clear separation between the frontend and backend.

-   **Architecture**: The backend will be implemented as a RESTful API using Java and the Spring Boot framework. The frontend will be developed as a Single-Page-Application (SPA) with Vue.js and TypeScript. This decoupled architecture allows for independent development and scaling of both parts.
-   **Database**: PostgreSQL will be used as the primary database. The use of Redis for caching will be considered to optimize performance for recurring requests.
-   **Deployment**: The entire application will be prepared for operation in Docker containers. This simplifies deployment, ensures a consistent runtime environment and facilitates scalability.
-   **Supported Platforms**: As a web application, compatibility will be ensured with the latest versions of common browsers (such as Google Chrome, Mozilla Firefox, Safari and Microsoft Edge).

### 3.7 Online User Documentation and Help System Requirements
The application is designed for intuitive use, so extensive documentation should ideally not be necessary. In case users have questions, there will be a "Help" section. This will include an FAQ (Frequently Asked Questions) section that provides answers to the most common questions about recipe creation, weekly planning and account management. Additionally, a contact form will be provided for users to send inquiries directly to the development team.

### 3.8 Purchased Components
The project is based exclusively on open-source technologies such as Vue.js, Spring Boot, PostgreSQL and Docker. No commercial components or licenses are currently being purchased. Should this change in the future, this section will be updated accordingly.

### 3.9 Interfaces

#### 3.9.1 User Interfaces
The application will provide the following graphical user interfaces:

-   **Recipe Overview**: A view for displaying, filtering and searching all available recipes.
-   **Recipe Detail Page**: Displays all information about a single recipe, including ingredients, preparation steps and images.
-   **Weekly Planner**: The central interface where users can plan their meals for the week via drag-and-drop or by assigning categories.
-   **Category Management**: An interface for creating and editing recipe categories (e.g., "Vegetarian," "Asian").
-   **Account Management**: Pages for registration, login, logout and managing one's own user profile.
-   **Shopping List**: An automatically generated list of all ingredients required for the recipes selected in the weekly plan.

#### 3.9.2 Hardware Interfaces
(n/a) As a browser-based web application, the software does not interact directly with specific hardware.

#### 3.9.3 Software Interfaces
The primary software interface is the **RESTful API** provided by the backend. The frontend communicates via this API to retrieve data (e.g., load recipes) or send data (e.g., save a new recipe). Communication occurs via standardized HTTP requests, and data is exchanged in JSON format.

#### 3.9.4 Communication Interfaces
Communication between the client (user's web browser) and the server occurs via the HTTPS protocol to ensure secure and encrypted data transmission.

### 3.10 Licensing Requirements
This project is based on open-source technologies. The respective licenses for components like Vue.js, Spring Boot, PostgreSQL and Docker will be listed here.

### 3.11 Legal, Copyright and Other Notices
The development team is not liable for incorrect or incomplete data provided by third-party sources.

### 3.12 Applicable Standards
The development will follow the common clean code standards and naming conventions. A "Definition of Done" will also be created and added here once it is complete.

## 4. Supporting Information
For any further information you can contact the Nom Nom Now Team or check our [Nom Nom Now Blog](https://github.com/orgs/Nom-Nom-Now/discussions/categories/blog). 
The Team Members are:
- Dylan O'Reilly
- Tino Wolter
- Rafael Till 
- Robin Fischer
- Silas Scholler

<!-- Picture-Link definitions: -->
[OUCD]: https://github.com/IB-KA/CommonPlayground/blob/master/UseCaseDiagramCP.png "Overall Use Case Diagram"