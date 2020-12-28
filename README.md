# AdventureEats
# Recipe
import sqlite3

conn = sqlite3.connect('recipeBook.sqlite')
#create a data structure
cur = conn.cursor()

cur.executescript('''
        DROP TABLE IF EXISTS 'Recipe';

        CREATE TABLE Recipe (
            id  INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT UNIQUE,
            name TEXT NOT NULL UNIQUE,
            amount INTEGER NOT NULL,
            measurement TEXT NOT NULL UNIQUE
        );
        ''')

#This class creates an INGREDIENT object. The INGREDIENT will contain a name, amount, and type of measurement. 
class Ingredient:
    def __init__(self, name, amount, measurement):
        self.name = name
        self.amount = amount
        self.measurement = measurement
    
    #printContents(): This method will display the instances of the INGREDIENT.
    def printContents(self):
        print(self.name)
        print(self.amount)
        print(self.measurement)

#This class creates a RECIPE list. 
class Recipe:
    def __init__(self):
        self.list = []

    #addData() This Method will add RECIPE data to the recipeBook database.
    def addData(self):
        for item in self.list:
            cur.execute("INSERT INTO Recipe (name, amount, measurement) VALUES(?, ?, ?)", (item.name, item.amount, item.measurement))
            conn.commit()
            
    #newIngredient(): This method will append new INGREDIENTs to the RECIPE list, until the user types 'No.' If the user does not type 'yes' or 'no,' then they will be asked to do so again. 
    def newIngredient(self):
        y = 'yes'
        #This loop makes sure that the user types "yes" or "no" in order to continue adding or stop adding ingredients.
        while y == 'yes':
            new = Ingredient(input('Ingredient Name: '), input('Amount of Ingredient: '), input('Type of Measurement used with Ingredient: '))
            self.list.append(new)
            y = str.casefold(input('Would you like to add another ingredient? Please type "yes" or "no": '))
            if y == 'no':
                #Calls addData() method in order to add new data to the database.
                Recipe.addData(self)
                print('Your recipe has been saved to the recipe book.')
                break
            elif y not in {'yes', 'no'}:
                while a not in {'yes', 'no'}:
                    y = str.casefold(input('Try again, would you like to add another ingredient? Please type "yes" or "no": '))

    #printList(): This method will display the contents of the RECIPE list.
    def printList(self):
        for item in self.list:
            print(item.name)
            print(item.amount)
            print(item.measurement)

newRecipe = Recipe()
newRecipe.newIngredient()
newRecipe.printList()

conn.close()
