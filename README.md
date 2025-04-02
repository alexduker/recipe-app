# recipe-app
import json

class RecipeApp:
    def __init__(self, file_name="recipes.json"):
        self.file_name = file_name
        self.load_recipes()

    def load_recipes(self):
        try:
            with open(self.file_name, "r") as file:
                self.recipes = json.load(file)
        except (FileNotFoundError, json.JSONDecodeError):
            self.recipes = []

    def save_recipes(self):
        with open(self.file_name, "w") as file:
            json.dump(self.recipes, file, indent=4)

    def add_recipe(self, name, ingredients, instructions):
        recipe = {
            "name": name,
            "ingredients": ingredients,
            "instructions": instructions
        }
        self.recipes.append(recipe)
        self.save_recipes()
        print(f"Recipe '{name}' added successfully!")

    def list_recipes(self):
        if not self.recipes:
            print("No recipes found.")
            return
        for idx, recipe in enumerate(self.recipes, start=1):
            print(f"{idx}. {recipe['name']}")

    def view_recipe(self, name):
        for recipe in self.recipes:
            if recipe["name"].lower() == name.lower():
                print(f"\n🍽 Recipe: {recipe['name']}\n")
                print("Ingredients:")
                for ingredient in recipe["ingredients"]:
                    print(f"- {ingredient}")
                print("\nInstructions:")
                print(recipe["instructions"])
                return
        print("Recipe not found.")

    def delete_recipe(self, name):
        self.recipes = [recipe for recipe in self.recipes if recipe["name"].lower() != name.lower()]
        self.save_recipes()
        print(f"Recipe '{name}' deleted.")

# Example usage
def main():
    app = RecipeApp()
    while True:
        print("\nRecipe App")
        print("1. Add Recipe")
        print("2. List Recipes")
        print("3. View Recipe")
        print("4. Delete Recipe")
        print("5. Exit")
        choice = input("Select an option: ")
        
        if choice == "1":
            name = input("Enter recipe name: ")
            ingredients = input("Enter ingredients (comma-separated): ").split(",")
            instructions = input("Enter cooking instructions: ")
            app.add_recipe(name.strip(), [i.strip() for i in ingredients], instructions.strip())
        elif choice == "2":
            app.list_recipes()
        elif choice == "3":
            name = input("Enter recipe name: ")
            app.view_recipe(name.strip())
        elif choice == "4":
            name = input("Enter recipe name to delete: ")
            app.delete_recipe(name.strip())
        elif choice == "5":
            break
        else:
            print("Invalid choice. Please try again.")

if __name__ == "__main__":
    main()
