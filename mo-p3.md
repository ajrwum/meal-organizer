# Models

<!-- ## Category

1. name: String
1. color: String
   Fixed list - not changeable by user -->

## Food

1. name: String
1. category: [ boisson, entrée, plat, accompagnement, fromage, dessert, matin sucré/salé ]
1. description: String
1. user: ObjectId, ref User

   Crud accessible to user

_option v36: ingredients [ ObjectId, ref Ingredient ]_

<!-- ## MealType

1. name: String
1. timeslot: {}
   timeslot is an object {position: 1, displayVal: 1AM-3AM}

   Crud accessible to user -->

<!-- ## Day

1. number: Number (to order days in a week)
1. name: String
1. meals: [ ObjectId, ref Meal ]

   Fixed - no crud by user -->

## Meal

<!-- 1. mealtype: ObjectId, ref MealType -->

1. type: [ {order: 100, name: petit déjeuner}, brunch (200), déjeuner (300), goûter(400), diner(500), souper(600), collation(700) ]
1. foods: [ ObjectId, ref Food ] => **populate**
1. user: ObjectId, ref User
1. date: Date

   Crud accessible to user

   _Option A_ : Affichage par défaut d'une semaine à partir du lundi précédent ou égal à la CurrentDate

   _Option B_ : Affichage par défaut d'une semaine à partir de la CurrentDate (affichage glissant avec apparition de jours vides)

   Hors mode édition, possibilité d'afficher une semaine depuis (CurrentDate - 6 jours) max

   Suppression automatique des meals antérieurs à CurrentDate - 7 jours

## User

1. email: String
1. password: String
1. name: String
<!-- 1. days: [ ObjectId, ref Day ]
1. foods: [ ObjectId, ref Food ]
1. mealTypes: [ ObjectId, ref MealType ] -->

   CRD accessible to user (no update)
