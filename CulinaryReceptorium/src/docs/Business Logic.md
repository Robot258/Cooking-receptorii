# **Business Logic - Culinary Receptorium** 

# **1. Entity** 

Recipe 

Recipe is the main entity of the “Culinary Receptorium” mobile application. It represents an individual culinary dish and stores the information required for its identification, viewing, and preparation. 

The recipe is the central object of the system around which the main application operations are built. The user can create a new recipe, view an existing one, modify its information, or delete it. 

Ingredient 

Ingredient represents an individual product used when preparing dishes. An ingredient is stored separately from a recipe because the same product can be used as part of several different dishes. 

The entity is responsible for centralized storage of ingredient information and is used when forming the composition of recipes. 

RecipeIngredient 

RecipeIngredient provides the relationship between a recipe and an ingredient. This component determines which ingredient belongs to a particular recipe, in what quantity, and in which unit of measurement. 

Therefore, RecipeIngredient describes the composition of a dish and allows the same ingredient to be used in several recipes without duplicating the Ingredient entity itself. 

# **2. Value Object (VO)** 

The project does not use a separate set of complex Value Object classes. The required values are stored directly in the corresponding entities because the application's domain does not require an additional abstraction layer for them. The following types of values play an important role in the application's business logic: 

- recipe name; 

- description and cooking instructions; 

- ingredient name; 

- ingredient quantity; 

- unit of measurement; 

- search query text. 

These values are used when creating, editing, searching, and displaying culinary recipes. Before being saved, they must be checked for correctness. 

# **3. Flows** 

Recipe Creation 

The user opens the form for creating a new recipe and enters the main information about the dish. After entering the data, the required ingredients are added to the recipe together with their quantities and units of measurement. 

After confirmation, the data is validated and passed to the local database for storage. The created recipe together with its composition becomes available in the general recipe list. 

Recipe Viewing 

After selecting a recipe from the general list, the application retrieves its data from local storage and forms a detailed representation of the dish. 

The user receives access to the main recipe information and its complete 

composition, including the list of ingredients with their corresponding quantities. Recipe Editing 

During editing, the system loads the existing recipe data and its composition. The user can modify information about the dish, add or remove ingredients, and change their quantities. 

After editing is completed, the new data is validated and saved to local storage. The updated recipe must remain available in the application without requiring the recipe to be created again. 

Ingredient Management 

IngredientActivity provides a separate section for working with the list of ingredients. 

The user can create a new ingredient, view existing ingredients, edit an ingredient name, or delete it. After creation, an ingredient can be used when forming the composition of any recipe. 

Recipe Search 

The user can search among saved recipes using a text query. The application processes the entered value and selects recipes that match the search condition. The search operates on locally stored recipes and does not modify application data. Recipe Deletion 

When deleting a recipe, the user selects a specific recipe, after which the system removes it from local storage. 

The related composition records must also be processed correctly so that no invalid relationships with the deleted dish remain in the database. Ingredient Deletion 

When deleting an ingredient, the system must take into account its use in recipe compositions. 

Related RecipeIngredient records must be processed so that the database structure remains valid and other recipes do not lose their independent data. Recipe Export to PDF 

The application allows a saved recipe to be converted into a separate PDF document. To do this, the system retrieves the current recipe data and its composition and uses them to generate the document. 

The PDF is intended for storing or using the recipe outside the application. The export operation must not modify the data stored in the database. 

Backup 

The application supports creating a backup of its own data. The backup must include recipes, ingredients, and the relationships between them required for complete restoration. 

The backup is created separately from the application's working data and can be used for subsequent restoration. 

Restore 

During restoration, the system retrieves data from a previously created backup and returns it to the application's local storage. 

After restoration is completed, recipes must remain associated with the 

corresponding ingredients, and the data structure must match the state stored in the backup. 

# **4. Feature** 

# MainActivity 

MainActivity is the main screen of the mobile application and the initial entry point for the user. 

It provides access to the key sections of the “Culinary Receptorium”: recipe management, ingredient management, and other functions for managing culinary information. 

# RecipesActivity 

RecipesActivity is responsible for the main recipe section. 

The component provides: 

- displaying saved recipes; 

- searching among recipes; 

- selecting a recipe for viewing; 

- navigating to editing; 

- performing deletion operations. 

Thus, this component serves as the main workspace for the user when working 

with the collection of culinary recipes. 

# RecipeEditActivity 

RecipeEditActivity is responsible for creating and editing recipes. 

Its main task is to provide entry of information about a dish and form its composition using available ingredients. The component also controls the saving of changes made by the user. 

IngredientActivity 

IngredientActivity is responsible for the dedicated ingredient management section. Through this component, the user can maintain their own list of products, edit it, and keep the data up to date for further use when creating recipes. 

SQLite Database Component 

The SQLite component is responsible for persistent local storage of “Culinary Receptorium” data. 

It performs operations for adding, retrieving, modifying, and deleting recipes, ingredients, and relationships between them. 

The local database ensures that information remains available between separate application launches. 

PDF Export Component 

The export component is responsible for generating a PDF file from a selected culinary recipe. 

It receives prepared data and forms an external representation of the recipe suitable for saving or transferring outside the application. 

Backup and Restore Component 

The backup and restore component is responsible for saving the current data set and subsequently returning it to a working state. 

Its use helps protect the user's collection of recipes and ingredients from data loss. 

# **5. Policy** 

Recipe Management Policy 

A recipe must contain correct information about the dish and be suitable for subsequent viewing and editing. 

Obviously incorrect or incomplete data must not be saved when such data is required for the corresponding operation. 

Ingredient Management Policy 

Ingredients must be stored separately from recipes and reused across different dishes. 

Duplication of the same product in the database should be avoided. 

Recipe Composition Policy 

Each RecipeIngredient record must reference an existing recipe and an existing ingredient. 

The quantity and unit of measurement must correspond to the specific use of the ingredient in the recipe. 

Validation Policy 

Before saving or modifying data, the system must verify: 

- completion of required information; 

- correctness of text values; 

- validity of numeric values; 

- correctness of ingredient quantities; 

- presence of required relationships. 

An input error must not result in an invalid data state being saved. 

Editing Policy 

Editing must modify only the selected recipe or ingredient. 

Changing information for one recipe must not affect independent recipes or other user data. 

Deletion Policy 

Deleting a recipe or ingredient must include correct processing of related data. After the operation is completed, the database must not contain relationships to deleted entities. 

Search Policy 

Search operations are used only for retrieving and filtering information. 

Searching must not modify recipes, ingredients, or relationships between them. Export Policy 

The PDF must be generated based on the current data of the selected recipe. 

Export is a read-only operation and must not modify or delete information in the local database. 

Backup Policy 

A backup must contain a consistent set of data sufficient to restore recipes, ingredients, and their relationships. 

During restoration, the system must not create invalid or unrelated records. 

# **6. Custom** 

# Culinary Data Organization 

The main feature of the “Culinary Receptorium” is the combination of two interconnected parts: a recipe catalog and an ingredient catalog. 

The user does not simply store a textual description of a dish but creates a structured recipe with a specific set of products, the quantity of each ingredient, and the corresponding units of measurement. 

Personal Recipe Collection 

The application is designed to build a personal local collection of culinary recipes. The user independently adds required dishes, modifies them, and deletes unnecessary records. 

Thus, the application content is created directly by the user and stored locally on an Android device. 

Unified Ingredient Set 

Ingredients form a separate data set that is used across different recipes. This allows the same product to participate in several dishes without creating a separate record for each recipe. 

Operation Without a Permanent Network Connection 

The main purpose of the application is to work with a personal culinary collection without depending on permanent Internet access. 

Creating, viewing, editing, searching, and deleting recipes and ingredients are performed using locally stored data. 

Transfer and Preservation of Culinary Information 

Recipes can be converted into PDF documents for further use outside the application, while local data can be saved as a backup. 

These functions complement the main application logic and provide the ability to preserve culinary information not only in the working database but also in an external format. 

