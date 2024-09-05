using System;
using System.Collections.Generic;
using System.Threading;

namespace ColorHunt
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Welcome to Color Hunt!");

            // Game Settings (You can customize these)
            int gridSize = 10; // Size of the grid (squares per row/column)
            int numColors = 5; // Number of different colors to use
            int roundTime = 10; // Time in seconds for each round
            int pointsPerCorrect = 10; // Points for a correct click
            int pointsPerIncorrect = -5; // Penalty for an incorrect click

            // Initialize the color list
            List<ConsoleColor> colors = new List<ConsoleColor>() { 
                ConsoleColor.Red, ConsoleColor.Green, ConsoleColor.Blue, ConsoleColor.Yellow, ConsoleColor.Magenta
            };

            // Game loop
            while (true)
            {
                // Generate the grid of colors
                ConsoleColor[,] grid = GenerateColorGrid(gridSize, numColors, colors);

                // Choose the target color
                ConsoleColor targetColor = colors[new Random().Next(colors.Count)];

                // Display the grid
                DisplayGrid(grid, targetColor);

                // Start the timer
                Console.WriteLine($"Time remaining: {roundTime} seconds");
                DateTime startTime = DateTime.Now;

                // Get player input
                while (DateTime.Now.Subtract(startTime).TotalSeconds < roundTime)
                {
                    ConsoleKeyInfo keyInfo = Console.ReadKey(true);
                    if (keyInfo.Key == ConsoleKey.Escape)
                    {
                        Console.WriteLine("Exiting the game...");
                        return; 
                    }

                    // Check if the player clicked on the correct square
                    // ... (Code for input handling and checking, see below)
                }

                // Game over for this round
                Console.WriteLine("Time's up!");
                // ... (Code for displaying round results, see below)

                // Press any key to continue or escape to exit
                Console.WriteLine("Press any key to continue or Escape to exit.");
                Console.ReadKey();
                Console.Clear();
            }
        }

        // Function to generate a grid of random colors
        static ConsoleColor[,] GenerateColorGrid(int size, int numColors, List<ConsoleColor> colors)
        {
            ConsoleColor[,] grid = new ConsoleColor[size, size];
            Random random = new Random();

            for (int row = 0; row < size; row++)
            {
                for (int col = 0; col < size; col++)
                {
                    grid[row, col] = colors[random.Next(numColors)];
                }
            }
            return grid;
        }

        // Function to display the color grid
        static void DisplayGrid(ConsoleColor[,] grid, ConsoleColor targetColor)
        {
            for (int row = 0; row < grid.GetLength(0); row++)
            {
                for (int col = 0; col < grid.GetLength(1); col++)
                {
                    Console.BackgroundColor = grid[row, col];
                    Console.Write(" ");
                    Console.BackgroundColor = ConsoleColor.Black;
                }
                Console.WriteLine();
            }

            Console.WriteLine($"Target Color: {targetColor}");
        }
    }
}

// Inside the "while (DateTime.Now.Subtract(startTime).TotalSeconds < roundTime)" loop
if (keyInfo.Key == ConsoleKey.Escape)
{
    //  (Exit game)
}
else if (keyInfo.Key == ConsoleKey.UpArrow && currentRow > 0)
{
    currentRow--;
}
else if (keyInfo.Key == ConsoleKey.DownArrow && currentRow < gridSize - 1)
{
    currentRow++;
}
else if (keyInfo.Key == ConsoleKey.LeftArrow && currentColumn > 0)
{
    currentColumn--;
}
else if (keyInfo.Key == ConsoleKey.RightArrow && currentColumn < gridSize - 1)
{
    currentColumn++;
}
else if (keyInfo.Key == ConsoleKey.Enter)
{
    // Check if the clicked square is the target color
    if (grid[currentRow, currentColumn] == targetColor)
    {
        // Correct click - update score
        score += pointsPerCorrect;
        Console.WriteLine($"Correct! Score: {score}");
        //  (Generate a new round) 
    }
    else
    {
        // Incorrect click - update score
        score += pointsPerIncorrect;
        Console.WriteLine($"Incorrect! Score: {score}");
        //  (Generate a new round) 
    }
}
