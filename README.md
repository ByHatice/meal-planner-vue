# Weekly Meal Planner

A simple, responsive weekly meal planner built with Vue.js. This app allows users to plan meals for each day of the week, including breakfast, lunch, and dinner. Users can add new recipes, prioritize their favorites, and remove meals from the schedule.

## Features

- **Weekly Meal Planner**: Organize meals for each day of the week (Monday to Sunday).
- **Add New Recipes**: Add custom recipes to the list with the option to mark them as favorites.
- **Prioritize Recipes**: Mark recipes as favorites to highlight them.
- **Responsive Design**: View the meal planner in a table format on larger screens and in a card format on mobile devices.
- **Remove Recipes**: Remove recipes from the meal plan or from the available recipe list.

## Technologies Used

- **Vue.js**: A progressive JavaScript framework used to build the app.
- **CSS**: For styling and layout (responsive design).

## How to Use

### Clone the Repository

Start by cloning the repository to your local machine:

```bash
git clone https://github.com/ByHatice/meal-planner-vue.git
```

### Install Dependencies

Navigate to the project directory and install the required dependencies:

```bash
cd meal-planner-vue
npm install
```
### Run the Application
Start the development server:

```bash
npm run serve
```

Open your browser and go to `http://localhost:8080` to start using the meal planner.

## Features in Detail
- **Add New Recipes**: You can add new recipes by entering the recipe name and selecting if it should be a favorite.
- **Weekly Schedule**: Each day (Monday to Sunday) has slots for three meals: breakfast, lunch, and dinner. You can select a recipe for each slot.
- **Responsive Views**: The app automatically adjusts for mobile and desktop screens, switching between a table view for larger screens and a card-based view for smaller devices.
- **Delete Recipes**: You can remove recipes both from the meal plan and from the list of available recipes.

## Contributing
We welcome contributions! If you want to improve this project, feel free to fork the repository and submit a pull request.

### Steps to Contribute
1. Fork the repository.
2. Create a new branch (`git checkout -b feature-branch`).
3. Make your changes and commit them (`git commit -am 'Add new feature'`).
4. Push to the branch (`git push origin feature-branch`).
5. Create a new pull request.

## Acknowledgements
- **Vue.js**: For building the user interface and handling the reactive data binding.
- **CSS Grid & Flexbox**: For creating a responsive layout.
