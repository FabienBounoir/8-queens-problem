# 🏛️ 8 Queens Problem - Interactive Visualizer

An interactive web application built with **SvelteKit** to explore and solve the famous 8 queens chess problem.

## 📖 About the Project

This website allows you to visualize, understand, and solve the N-queens problem interactively. I created this project because I was fascinated by the mathematical elegance of this classic problem and wanted to create a visual tool to better understand and share it.

## 🤔 What is the 8 Queens Problem?

The **8 Queens Problem** is a classic puzzle in computer science and mathematics that consists of placing 8 queens on an 8×8 chessboard such that **no queen can capture another**.

### Chess Queen Rules:
- A queen can move **horizontally** (across its row)
- A queen can move **vertically** (down its column)  
- A queen can move **diagonally** in all directions

### The Challenge:
Place 8 queens on the chessboard without them threatening each other. This means:
- ❌ No two queens on the same row
- ❌ No two queens on the same column
- ❌ No two queens on the same diagonal

## 💡 Why is this Problem Interesting?

1. **Algorithmic Complexity**: There are 4,426,165,368 ways to place 8 queens on a chessboard, but only **92 valid solutions**!

2. **Practical Applications**: This problem illustrates important concepts in computer science:
   - **Backtracking** algorithms
   - Combinatorial optimization
   - Constraint satisfaction

3. **Generalization**: The problem can be extended to N queens on an N×N board, offering increasing complexity.

## 🎯 What Does This Site Solve?

This interactive application solves several problems:

### ✅ **Intuitive Visualization**
- Graphical display of the chessboard
- Real-time visualization of squares controlled by each queen
- Clear interface to understand conflicts

### ✅ **Interactive Learning**
- Manual queen placement to test your own solutions
- Visual alerts for conflicts
- Real-time statistics (number of queens, conflicts)

### ✅ **Automatic Solving**
- Integrated backtracking algorithm to find a valid solution
- Ability to see the solution build step by step

### ✅ **Flexibility**
- Adaptation to different board sizes (4×4 to 100×100)
- Color customization
- Configurable display options

## 🚀 Features

- **🎮 Interactive Mode**: Place queens manually by clicking
- **🤖 Auto Solve**: Let the algorithm find a solution
- **📊 Real-time Statistics**: Track placed queens and conflicts
- **🎨 Customization**: Change board colors
- **📱 Responsive**: Works on desktop, tablet, and mobile
- **⚙️ Variable Sizes**: Test the problem with different board sizes

## 🛠️ Technologies Used

- **SvelteKit** - Modern and performant web framework
- **TypeScript** - For robust and maintainable code
- **SCSS** - Advanced and customizable styles
- **Vite** - Fast and efficient build tool

## 🏃‍♂️ Installation and Usage

### Prerequisites
- Node.js (version 18+)
- npm or yarn

### Installation
```bash
# Clone the project
git clone https://github.com/FabienBounoir/8-queens-problem.git

# Navigate to the folder
cd 8-queens-problem

# Install dependencies
npm install

# Start the development server
npm run dev
```

### Available Scripts
```bash
npm run dev        # Development server
npm run build      # Production build
npm run preview    # Build preview
npm run check      # TypeScript checking
npm run lint       # Code linting
npm run format     # Code formatting
```

## 🎓 Algorithm Used

The application uses a recursive **backtracking algorithm**:

1. **Sequential Placement**: Places one queen per row
2. **Constraint Checking**: Verifies that no conflicts exist
3. **Backtracking**: If a dead end is reached, backtracks
4. **Solution**: Continues until a valid configuration is found

This algorithm is particularly elegant as it intelligently explores the space of possible solutions.

## 🎯 Educational Goal

This project perfectly illustrates how a complex mathematical problem can be made accessible through a modern interactive interface. It demonstrates the importance of visualization in understanding abstract algorithms.

---

**Developed with ❤️ by [Fabien Bounoir](https://github.com/FabienBounoir)**

*This project fascinated me because it combines mathematical beauty, algorithmic challenge, and interactive design. I hope it will help you discover or rediscover the elegance of the N-queens problem!*