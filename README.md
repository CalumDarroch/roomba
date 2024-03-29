# Robot Hoover Technical Exercise

This is a technical exercise written in JavaScript. The aim is to write a program that will simulate a robot hoover navigating a room and hoovering up piles of dirt.

## Requirements

- The program can be run either from the terminal or a webpage.

- The room dimensions will be input as cartesian coordinates (positive integers only). E.g. A room with coordinates X: 5 and Y: 5 would consist of 5 columns and 5 rows, for a total of 25 different positions that the Roomba could occupy.

- Patches of dirt will be input using the same X/Y coordinate format.

- The hoover's starting position will be input in the same format.

- The hoover's driving instructions will be formatted as cardinal directions, i.e. North, South, East and West.

### Input:

The program's input will be a `.txt` file containing all of the above information. Here is an example:

```
5 5
1 2
1 0
2 2
2 3
NNESEESWNWW
```
- Line 1 is the room dimensions.
- Line 2 is the hoover's starting position.
- Subsequent lines are the locations of dirt patches.
- The final line is the driving instructions for the hoover.

### Output

The program will then return a simple 2-line output, for example:

```
1 3
1
```
- Line 1 is the final position of the hoover after it has completed all of its instructions.
- Line 2 is the number of patches of dirt it successfully hoovered up (i.e. occupied the same space as at some point on its journey).

## Installation

To install this project, first clone or download this repo to a directory of your choice.

This project requires [Node.js](https://nodejs.org/en/) to run. If you don't have it, follow that link to install.

Once installed, navigate to the directory and run:
```
npm init
```
This will initialize Node.js on the directory. It will ask you to verify the contents of the `package.json` file - just click 'yes' on everything.

Now, to use the program, you can edit the contents of the `input.txt` file with your desired directions as above. To run the program, simply type:
```
node .
```
into your terminal, and you will see your results!

__Note__: If you're having issues running the program, look at the "Notes / Improvements" section at the end of this README to see if the problem is covered there.

## Approach

### Business logic

The first thing I will do with this program is break down the problem into specific user stories based upon the requirements, tackling the smallest issue first and using TDD to drive the increasing complexity of the program.

```
As a user,
So that my hoover can navigate a room,
I want to be able to input the size of the room.
```
```
As a user,
So that my hoover can complete its journey as planned,
I need the room size to be saved after being set.
```
```
As a user,
So that my hoover can navigate successfully,
I need it to record its initial position.
```
At this point I reached my first edge case: what if the start position is not within the bounds of the room? This will need to be tackled. So:
```
As a user,
So that my hoover doesn't get confused,
I will need to make sure that it can't start outside the room.
```
This will now return an error if the user tries to place the hoover outside of the room. Further down the line I may refactor it so that the start position is moved to the nearest position to that which the user specified.
```
As a user,
So that I can know if the hoover has hit a dirt patch,
I will need to record the locations of the dirt patches in the room.
```
```
As a user,
So that the hoover can go on its journey,
I will need its location to change when it is given an instruction.
```
The hoover can now move North, South, East or West when given a single-character string of "N", "S", "E" or "W" as an argument for the function `moveHoover`.

This raises the next edge case: what happens when the hoover is instructed to move outside the bounds of the room, similar to before with its starting position? For now, I will tackle it in a similar way by raising an error if it is instructed to do so.

Finally for the business logic, I must implement a tally for whenever the hoover moves over a dirt patch:
```
As a user,
So that I can know how effective my instructions have been,
I want to be able to see how many dirt patches the hoover has hoovered up.
```
This also introduces another edge case: we don't want the hoover to count the same dirt patch twice! Once a patch has been hit it will be removed from the array of dirt patches.

Finally, I refactored so no error is thrown when the hoover tries to move outside the room, it just stays stationary.

### Index

To run the app, I needed to create an `index.js` file to interact with the model and display the user's results in the terminal. To do this, I Installed Node.js. and used its built-in `fs` module to split text file using the `readFileSync` [function](https://nodejs.org/api/fs.html#fs_fs_readfilesync_path_options).

I then parsed the file into a 2D array, containing a number of arrays with 2 integer elements, and a final array with the directions as strings. I have annotated the `index.js` file for more info on how I achieved this.

After that, I simply applied the module pattern to `room.js` so I could require it as a module in the index, and passed it the relevant arrays as arguments for its functions before finally printing the desired output to the terminal using `console.log()`.

## Notes / Improvements

- If you try to place the starting position of the hooved outside the bounds of the room, the program will return an error.
- The Jasmine tests do not run properly unless you comment out the final line of `room.js`:
```javascript
module.exports = Room;
```
I'm not entirely sure why, and would try to fix this problem if given more time.
- When parsing the .txt file I had to split the text around `\r\n`. The `\r` escape character (or "return carriage") may not be used if you are running Linux/MacOS, so you may need to remove it from line 6 of `index.js` for the program to work on your system.
