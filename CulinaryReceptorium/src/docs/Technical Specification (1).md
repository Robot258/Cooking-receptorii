# **Technical Specification - Culinary Receptorium** 

# **1. Table of Contents** 

1. ToC — Table of Contents 

2. Scope 

3. Content 

4. Change Policy 

5. Fundamental Principles 

6. Technologies 

7. Architecture 

8. Naming Policy 

9. Testing Policy 

- 10.Document Policy 

- 11.Anti-patterns 

# **2. Scope** 

The Culinary Receptorium is a mobile application intended for structured management of culinary recipes and related ingredient information. The system must provide the user with the ability to store, retrieve, modify and organize recipe information in a consistent digital format. 

The project covers the development of a self-contained Android application with local persistent storage. Core operations with recipes and ingredients must be available without a permanent Internet connection. The system must provide a consistent mechanism for maintaining the relationship between recipes and their ingredients. 

The functional scope of the project includes: 

- creation and modification of recipes; 

- viewing and searching stored recipes; 

- management of ingredients; 

- association of ingredients with recipes; 

- removal of obsolete or unwanted information; 

- persistent local storage; 

- export of recipe information to PDF; 

- backup and restoration of application data. 

The system is intended for individual use. The current scope does not include user accounts, authentication, social functionality, public recipe sharing, online communication, cloud synchronization, remote recipe databases, online purchasing of ingredients or integration with external culinary services. 

The application must not require network connectivity for operations that are defined as core local functionality. Network-dependent functionality may only be introduced as an explicitly defined extension of the project. 

All functionality must operate on a common data model. User interface elements, application logic and data storage must not maintain separate conflicting representations of the same recipe or ingredient information. 

# **3. Content** 

The content of the application must be organized around the concepts required for recipe management. The primary information managed by the system consists of recipes, ingredients and the relationship between them. 

A recipe must contain sufficient structured information to identify and describe the culinary preparation and to provide the information required for its preparation. An ingredient must represent an individual component that can participate in one or more recipes. The relationship between a recipe and its ingredients must support the storage of the information necessary to describe the composition of the recipe, including quantities and measurement information where required. 

The system must provide separate functional areas for: 

- recipe creation and editing; 

- recipe viewing; 

- recipe search and browsing; 

- ingredient management; 

- recipe composition management; 

- data export; 

- data backup and restoration. 

All stored information must be presented in a form that is understandable to the user. Information that belongs to the same logical object must be displayed consistently throughout the application. 

The interface must follow a unified visual approach. The project uses a minimalist mobile interface with a predominantly white visual environment and clearly separated content and interaction elements. Visual consistency must be maintained across all application screens. 

The application must provide clear feedback for operations that modify persistent data. The user must be able to distinguish between an operation that has been successfully completed and an operation that has failed or has not yet been saved. The system must maintain a single authoritative version of persistent recipe and ingredient information. Displayed data must be obtained from the current application state rather than maintained as unrelated copies. 

# **4. Change Policy** 

Changes to the project must be controlled according to their effect on functionality, data, architecture and existing dependencies. Before implementing a change, the affected components and existing behavior must be identified. Changes are divided into: 

- Minor changes — changes that do not modify the data model or fundamental application behavior; 

- Functional changes — changes that introduce, remove or modify uservisible functionality; 

- Structural changes — changes that affect the data model, architecture or interaction between major system components. 

Minor changes may be introduced without restructuring unrelated parts of the project. Functional and structural changes must be checked for compatibility with existing functionality before implementation. 

Any change to persistent data structures must define how existing data will be handled. Existing user data must not be removed or invalidated as an unintended consequence of a structural modification. 

The following rules apply to database changes: 

- changes to the schema must be version-controlled; 

- existing records must be considered during schema modification; 

- relationships between stored entities must remain valid; 

- obsolete fields or structures must not be removed without evaluating their existing data; 

 database changes must be tested before being included in the final version. New functionality must use the existing project technologies and architectural principles unless there is a documented reason to change them. New external dependencies must be introduced only when their functionality cannot reasonably be provided by the existing technology stack. 

After a functional or structural change, all directly affected functionality must be retested. Documentation must be updated whenever a change modifies a documented requirement, rule, architecture decision or technical constraint. Changes must not be implemented solely for convenience if they introduce inconsistency with established project rules. 

# **5. Fundamental Principles** 

The project must be developed according to the principles of simplicity, separation of responsibilities, maintainability, consistency and data integrity. 

Each software component must have a clearly defined responsibility. Components must not combine unrelated functionality merely to reduce the number of files or classes. User interface processing, business rules, persistent data operations and auxiliary file operations must remain logically separated. The following principles are mandatory: 

- Single Responsibility: each component must have one primary responsibility; 

- Separation of Concerns: presentation, application logic and data management must be logically separated; 

- DRY: identical logic must not be unnecessarily duplicated; 

- KISS: the simplest appropriate technical solution must be preferred; 

- Consistency: identical operations and concepts must follow the same rules throughout the application; 

- Maintainability: implementation decisions must allow future modification without unnecessary restructuring; 

- Data Integrity: persistent information must remain valid after every supported operation. 

The application must validate information before persistent storage. Validation must not depend exclusively on the assumption that data received from the user interface is correct. 

The application must preserve the consistency of relationships between stored objects. Operations affecting one object must take into account dependent information where such dependencies exist. 

The project must avoid unnecessary coupling. A modification to one functional area should not require unrelated areas to be changed unless they actually depend on the modified behavior. 

The architecture must remain proportional to the size and requirements of the project. Additional abstractions, patterns or technologies must be introduced only when they solve an identifiable technical problem. 

# **6. Technologies** 

The project must use a native Android development approach with Kotlin as the primary programming language. Kotlin must be used for application behavior, user interaction processing, data handling, validation and communication with persistent storage. 

Android Studio must be used as the primary development environment. Project configuration, source code, resources, compilation and debugging must remain compatible with the Android development environment targeted by the project. SQLite must be used as the local relational persistence mechanism. Persistent recipe and ingredient information must be stored in a structured relational form. The database must support the operations required by the application while maintaining consistency between related records. 

The following technological rules apply: 

- Kotlin must be the primary implementation language; 

- Android platform mechanisms must be preferred for platform-specific functionality; 

- SQLite must be used for local persistent application data; 

- XML-based resources must follow Android resource conventions where XML is used; 

- Material-style interface components must be used consistently within the established visual design; 

- PDF generation must use a mechanism compatible with Android and the application's local operation; 

- backup functionality must use mechanisms compatible with Android storage requirements. 

The project must not introduce a server, cloud database, external API or third-party service unless such functionality becomes an explicit project requirement. Third-party libraries may be used only when they provide functionality that is required by the project and is not adequately provided by the existing platform or project implementation. Every additional dependency must be compatible with the target Android environment and must not introduce unnecessary architectural complexity. 

Technology selection must prioritize stability, compatibility, maintainability and suitability for the actual requirements of the application. 

# **7. Architecture** 

The application architecture must separate the system into logically independent areas responsible for presentation, application processing and data persistence. The presentation layer is responsible for: 

- displaying application data; 

- receiving user input; 

- processing interface events; 

- controlling navigation; 

- presenting operation results and validation messages. 

The presentation layer must not contain extensive database management or unrelated business rules. 

The application logic layer is responsible for processing user operations and enforcing rules that determine how the system behaves. It must coordinate operations between the presentation and data layers without becoming dependent on a specific visual representation of the data. 

The data layer is responsible for persistent storage and retrieval. It must provide controlled access to the local database and must isolate database implementation details from the presentation layer. 

The interaction between architectural areas must follow clear boundaries: 

- presentation components may request operations from the application logic; 

- application logic may request persistent data operations; 

- persistent storage must not directly control the user interface; 

- presentation components must not directly manipulate unrelated database structures; 

- data returned from storage must be validated and processed before being presented where required. 

The application must use a consistent CRUD model for persistent information. Create, read, update and delete operations must follow defined validation and integrity rules. 

Relationships between recipes and ingredients must be represented explicitly in the data model. The architecture must prevent the creation of invalid relationships and must define the behavior of dependent information when related data is modified or removed. 

Auxiliary operations such as PDF generation and backup must be isolated from ordinary recipe management operations. Export must read application data without changing its persistent state. Backup and restoration must operate according to explicitly defined data integrity rules. 

The architecture must allow individual functional areas to be modified without requiring unnecessary changes to unrelated areas. 

# **8. Naming Policy** 

All names used in source code, resources, database structures and documentation must be meaningful, consistent and unambiguous. 

The following naming conventions are mandatory: 

- classes and types use PascalCase; 

- functions and variables use camelCase; 

- constants use uppercase names with underscores; 

- Android resource names use lowercase characters and underscores according to Android conventions; 

- database names use one consistent naming convention throughout the schema. 

Names must describe the purpose of the corresponding element. Generic names, meaningless abbreviations and names that provide no information about the object's purpose must not be used for significant project elements. 

Function names must describe an operation. Variable names must describe the information they contain or the role they perform. Names must not depend on temporary implementation details. 

The terminology used in the source code must correspond to the terminology used in the documentation. The same domain concept must not be represented by several unrelated names in different parts of the system. 

Database names must clearly distinguish entities, attributes and relationships. Names must remain understandable without requiring developers to inspect the implementation before determining their purpose. 

Naming must remain stable during development. Renaming must be performed when an existing name becomes inaccurate, misleading or inconsistent with the project's terminology. 

# **9. Testing Policy** 

Testing must verify both normal system operation and the application's behavior under invalid or unexpected conditions. Testing must be performed after initial implementation and after modifications that can affect existing functionality. Testing must cover the following areas: 

- recipe creation, viewing, editing and deletion; 

- ingredient creation and management; 

- association between recipes and ingredients; 

- search and filtering operations; 

- validation of user input; 

- database CRUD operations; 

- persistent data after application restart; 

- PDF export; 

- backup and restoration; 

- navigation and general application stability. 

Functional tests must verify that each operation produces the expected change in application state and persistent data. 

Validation tests must verify that invalid or incomplete information cannot be stored when it violates the defined requirements. Boundary conditions must be considered for fields that have restrictions on length, quantity or format. Database tests must verify that: 

- new records are stored correctly; 

- existing records can be retrieved; 

- updates affect only the intended records; 

- deletion follows the defined relationship rules; 

- related data remains consistent; 

- data remains available after application restart. 

Export functionality must be tested to ensure that generated PDF documents contain the required information and that export does not modify the original stored data. 

Backup functionality must be tested by creating a known application state, creating a backup, modifying or removing data, restoring the backup and verifying that the expected state has been recovered. 

Any defect discovered during testing must be corrected and the affected functionality must be tested again. When a correction modifies shared logic, related functionality must also be retested. 

Testing results must be considered before a modified functionality is treated as complete. 

# **10. Document Policy** 

Project documentation must define the requirements, rules, constraints and technical decisions necessary for consistent development of the application. Documentation must be: 

- accurate; 

- consistent; 

- current; 

- understandable; 

- directly related to the project; 

- free from contradictory requirements. 

Technical documentation must distinguish between mandatory requirements and optional implementation decisions. A rule described as mandatory must be treated as a project constraint unless it is formally changed. 

Documentation must use the same terminology throughout the project. Names of domain concepts, operations and architectural elements must not change between documents without a defined reason. 

When project requirements or technical rules change, all affected documentation must be reviewed and updated. Documentation must not retain obsolete requirements together with current requirements if this creates ambiguity. Documentation must not describe implementation details that are not required for the purpose of the document. General architectural rules should remain separate 

from detailed business logic, and business rules should remain separate from source-code-specific implementation decisions. 

Each document must have a defined purpose. Information should be placed in the document where it logically belongs rather than duplicated across several documents. 

Documentation must be maintained throughout development rather than created only after implementation is completed. 

# **11. Anti-patterns** 

The following development practices are prohibited or must be avoided because they conflict with the technical requirements of the project. 

- God Component: a single component must not contain most of the application's unrelated functionality. 

- Duplicated Logic: the same validation, database operation or business rule must not be implemented independently in several locations without a justified reason. 

- Direct Database Coupling: presentation components must not contain extensive database implementation logic. 

- Mixed Responsibilities: user interface, business rules, persistence and file processing must not be unnecessarily combined. 

- Uncontrolled Data Modification: persistent data must not be changed outside the rules defined for the corresponding operation. 

- Invalid Relationships: the system must not create or preserve references to nonexistent or invalid related records. 

- Missing Validation: user input must not be persisted without the required validation. 

- Unnecessary Complexity: additional architectural layers, patterns, libraries or services must not be introduced without a concrete technical requirement. 

- Hard-coded Configuration: values that belong to resources, configuration or reusable project settings must not be unnecessarily embedded directly into application logic. 

- Silent Error Handling: failures must not be ignored when they can affect data integrity or application behavior. 

- Inconsistent Naming: different names must not be used for the same conceptual object or operation without a clear distinction. 

- Obsolete Code: unused implementations, dependencies and abandoned approaches must be removed from the active project unless their presence is explicitly justified. 

- Uncontrolled Destructive Operations: deletion and restoration operations must not affect data outside their defined scope. 

- Architecture Violation: lower-level storage mechanisms must not directly control higher-level presentation behavior. 

- Documentation Contradiction: implementation and documentation must not intentionally contain conflicting rules or descriptions. 

