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

[ {position: 100, name: petit déjeuner}, brunch (200), déjeuner (300), goûter(400), diner(500), souper(600) ]

<!-- ## Day

1. number: Number (to order days in a week)
1. name: String
1. meals: [ ObjectId, ref Meal ]

   Fixed - no crud by user -->

## Meal

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

   CRD accessible to user (no update)

# Routes

## Client

| Route           | Action                                           |   Access    |
| --------------- | ------------------------------------------------ | :---------: |
| /               | Display Homepage                                 |     all     |
| /signin         | Display Signin form                              |     all     |
| /signup         | Display Signup form                              |     all     |
| /signout        | Link to signout                                  |     all     |
| /foods          | Display all foods                                | logged user |
| /foods/new      | Display form to create food                      | logged user |
| /foods/:id      | Display form to edit a food                      | logged user |
| /meals/:date    | Display meals for week(n) (default: currentWeek) | logged user |
| /meals/meal/new | Display form to create meal => **DragnDrop**     | logged user |
| /meals/meal/:id | Display form to edit a meal                      | logged user |

## Server

|  Verb  | Route              | Action                                    |   Access    |
| :----: | ------------------ | ----------------------------------------- | :---------: |
|  POST  | /signin            | Authenticate user inside Db               |      -      |
|  POST  | /signup            | Create user inside Db + token             |      -      |
|  GET   | /foods             | Get all foods                             | logged user |
|  POST  | /foods/food        | Create a food                             | logged user |
|  GET   | /foods/food/:id    | Get a food                                | logged user |
| PATCH  | /foods/food/:id    | Update a food                             | logged user |
| DELETE | /foods/food/:id    | Delete a food                             | logged user |
|  GET   | /foods/categories  | Get all categories (when creating a food) | logged user |
|  GET   | /foods/:categoryId | Get all foods matching the categoryId     | logged user |
|  POST  | /meals/meal        | Create a meal                             | logged user |
|  GET   | /meals/meal/:id    | Get a meal                                | logged user |
| PATCH  | /meals/meal/:id    | Update a meal                             | logged user |
| DELETE | /meals/meal/:id    | Delete a meal                             | logged user |
|  GET   | /meals/mealtypes   | Get all meal types (when creating a meal) | logged user |
|  GET   | /meals/:date       | Get all meals matching the date's week    | logged user |

# Navigation on client site

## Nav bar (menu)

### User NOT logged in

- MO logo (home)
- Signin
- Signup

_==> Default page = Home ('/')_

explaining what the site offers and calling for signup/signin

### User authenticated

- MO logo (home)
- Foods
- Signout

_==> Default page = Current week of meals ('/meals/:date')_

displaying the current week from Monday to Sunday, showing on each day the meals previously added

if no meal has been added yet, the day is shown empty

## On pages

### /meals/:date

- all actions from navbar
- create a meal
  - access through a + button attached to each day
- edit a meal
  - access through a 'fa pen' button on the meal title line
- delete a meal
  - access through a 'fa trash' button on the meal title line
  - warn the user it is definitive and will not allow the correct display of previous weeks
- display previous week
  - access through a 'Semaine précédente' button on page (above the days display on the left side)
- display next week
  - access through a 'Semaine prochaine' button on page (above the days display on the right side)

### /meals/meal/new OR /meals/meal/:id

- all actions from navbar
- create a food to add to reference list and the current meal
  - access through a 'Ajout aliment' or '+ aliment' button above the list of reference foods

### /foods

- all actions from navbar
- create a food
  - access through a 'Ajout aliment' or '+ aliment' button above the list of existing foods
- display a food
- edit a food
- delete a food

### /foods/food/:id (get)

- all actions from navbar
- edit a food
- delete a food

# Actions to provide on client's site

## /meals/meal/new OR /meals/meal/:id

- modify the type of meal
- add a food to the current meal
  - access through drag and drop from the displayed list of reference foods
- delete a food from the current meal
  - access through a 'fa trash' button next to the food already added to the meal

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

# Utils

## Date

Date filter example on meals for MongoDb:

- { date: {$eq: ISODate('2022-02-04')} }

Date filter between Monday and Sunday of 1 week:

- { date: { $gte: ISODate('2022-01-31'), $lt: ISODate('2022-02-07') } }

- { date: { $gte: ISODate('2022-02-21T00:00:00.000Z'), $lt: ISODate('2022-02-27T23:59:59.999Z') } }

- { $and: [ { user: '61fea6bfca91546d46abaf40' }, { date: { $gte: ISODate('2022-02-21T00:00:00.000Z'), $lt: ISODate('2022-02-27T23:59:59.999Z') } } ] }
