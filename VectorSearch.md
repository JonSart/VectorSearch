//Jonathan Simpson. 8/1/2019. Attempting to solve a problem: How do we do a binary search through a 2D vector?
#include <iostream>
#include <vector>
using std::vector;
using std::cout;
using std::cin;

class Grid
{
public:
	Grid();

private:
	const int sizeX = 200;
	const int sizeY = 150;
	int userAnswer;
	vector<vector<int>> grid;

	void BinarySearch();
	void GrabInput();
};

Grid::Grid()
{
	userAnswer = 0;									
	int xCount = 0;						//Keep track of what number we're at inside the for loop.
	grid.resize(sizeY, vector<int>(sizeX));

	for (int i = 0; i < sizeY; i++)				//Initialize all values in the vector.
	{
		for (int x = 0; x < sizeX; x++)
		{
			grid[i][x] = xCount;
			xCount++;
			
		}
	}

	GrabInput();
}

void Grid::GrabInput()
{
	userAnswer = -1;					//Used to start our loop below.

	cout << "Enter a number to search for within the vector.\n"
		<< "This number should be between 0 at the lowest and "
		<< (sizeX * sizeY) - 1 << " at the highest.\n";

	cin >> userAnswer;

	while (userAnswer < 0 || userAnswer >(sizeX * sizeY - 1))	//Don't allow bad input
	{
		cout << userAnswer << " is not within the proper range.\n"
			<< "This number should be between 0 at the lowest and "
			<< (sizeX * sizeY) - 1 << " at the highest.\n";

		cin >> userAnswer;
	}

	BinarySearch();
}

void Grid::BinarySearch()				//Technically, I don't know if this entire function counts as a binary search.
{							//We treat the row itself as a binary search once we find it.
	int storedY = 0;				//We're iterating through the last value in each row and comparing it to userAnswer. 
	int operations = 0;				//Thus, we need one operation for each row (no matter how big the row is), plus the operations for the binary search in the last row. 
							//Y rows are more computationally expensive than X rows (or columns)
	int first = 0;					//10 Y rows with an X row size of 2000 takes 13 operations.
	int last = sizeX - 1;				//Switched around, it could take up to ~2010 operations.
	int mid = (first + last) / 2;			

	for (int i = 0; i < sizeY; i++)			//Search through the Y axis to find our Y row first.
	{
		if (grid[i][last] >= userAnswer){	//If we find the right row, store the row for later use and end the loop.
			storedY = i;
			i = sizeY;						
		}
		operations++;
	}

	bool loopState = true;
	while (loopState)															
	{
		if (userAnswer == grid[storedY][mid]) {					
			operations++;
			loopState = false;
			cout << "userAnswer: " << userAnswer 
				 << "\nFound location: " << grid[storedY][mid] << ".\n"
			     << "This took: " << operations << " operations.\n";
		}
		if (userAnswer >= grid[storedY][mid])			//Discard results from the lower portion
			first = mid + 1;
		if (userAnswer < grid[storedY][mid] && last == 0)	//Ensure we do not attempt to access an invalid array element. 
			last = mid;
		else if (userAnswer < grid[storedY][mid])		//Discard upper results.
			last = mid - 1;

		mid = (first + last) / 2;
		operations++;
	}

}



int main()
{

	Grid grid;

}



