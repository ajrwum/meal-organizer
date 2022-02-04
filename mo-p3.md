# Models

## Category

1. name: String
1. color: String
1. order: String

   Fixed list - not changeable by user

[ boisson, entrée, plat, accompagnement, fromage, dessert, matin sucré/salé ]

## Food

1. name: String
1. category: ObjectId, ref category => **populate**
1. description: String
1. user: ObjectId, ref User

   Crud accessible to user

_option v36: ingredients [ ObjectId, ref Ingredient ]_

## MealType

1. name: String
1. timeslot: String
1. position: Number

   timeslot is a String such as 'minuit - 03:00'

   Fixed list

[ {position: 100, name: petit déjeuner}, brunch (200), déjeuner (300), goûter(400), diner(500), souper(600), collation(700) ]

<!-- ## Day

1. number: Number (to order days in a week)
1. name: String
1. meals: [ ObjectId, ref Meal ]

   Fixed - no crud by user -->

## Meal

<!-- 1. mealtype: ObjectId, ref MealType -->

1. type: ObjectId, ref MealType => **populate**
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

# Routes

## Client

| Route           | Action                                           |   Access    |
| --------------- | ------------------------------------------------ | :---------: |
| /               | Display Homepage                                 |     all     |
| /signin         | Display Signin form                              |     all     |
| /signup         | Display Signup form                              |     all     |
| /foods          | Display all foods                                | logged user |
| /foods/new      | Display form to create food                      | logged user |
| /foods/:id      | Display form to edit a food                      | logged user |
| /meals/:weekId  | Display meals for week(n) (default: currentWeek) | logged user |
| /meals/meal/new | Display form to create meal => **DragnDrop**     | logged user |
| /meals/meal/:id | Display form to edit a meal                      | logged user |

## Server

|  Verb  | Route              | Action                                |   Access    |
| :----: | ------------------ | ------------------------------------- | :---------: |
|  POST  | /signin            | Authenticate user inside Db           |      -      |
|  POST  | /signup            | Create user inside Db + token         |      -      |
|  GET   | /foods             | Get all foods                         | logged user |
|  POST  | /foods/food        | Create a food                         | logged user |
|  GET   | /foods/food/:id    | Get a food                            | logged user |
| PATCH  | /foods/food/:id    | Update a food                         | logged user |
| DELETE | /foods/food/:id    | Delete a food                         | logged user |
|  GET   | /foods/:categoryId | Get all foods matching the categoryId | logged user |
|  GET   | /meals/:weekId     | Get all meals matching the weekId     | logged user |
|  POST  | /meals/meal        | Create a meal                         | logged user |
|  GET   | /meals/meal/:id    | Get a meal                            | logged user |
| PATCH  | /meals/meal/:id    | Update a meal                         | logged user |
| DELETE | /meals/meal/:id    | Delete a meal                         | logged user |

# React Components & Pages

## Pages

- home (not logged in)
- signin (form)
- signup (form)
- create/edit one food (form)
- display one food (with edit & delete feature)
- create/edit meal (meal composition with drag & drop form foods)
- meals (week view)

## Components

- FoodCard
  - data of 1 food for a user
- MealCard (container of foods)
  - data of 1 meal
- Day (container of meals ??? or pure css presentation ???) : to be seen with regard to the drag & drop feature
