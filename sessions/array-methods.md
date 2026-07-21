## Array methods

Arrays are very powerful data structures in JavaScript. They come with plenty of built in methods to perform a variety of common tasks!

In the following exercises you will solve problems by using built-in array methods. Self-research is highly encouraged!

### Exercise #1 - CRUD operations on array items

Create an array of objects with the following properties:
- id, a string - must be unique among all members
- name, a string
- score, an integer

We want to create a few operations that implement all CRUD operations.

For every part of the question, use at least one array method:

1. Write a read function that gets an ID, and returns the item with the ID. If there’s no item with the given ID - returns null
2. Write a read function that `console.log` all items in the order they are defined. Think about the best way to print the data to the user!  
    Hint: don’t use `for` construct.
3. Write a delete function that gets an ID of one of the items, removes the item with the given ID and returns it. If there’s no item with the given ID - returns null
4. Add a new optional description field to each object. Create a function that gets an array of these objects. The function returns an array of all defined descriptions and modifies the first letter of each description to be uppercase.
5. Write a create function that gets the following parameters and appends a new item to the end of the array:
	1. Required name, must contain at least 2 chars
	2. Required score, must be a non-negative integer
	3. Optional ID. If an ID is not given, creates a new unique ID that no other members have.
6. Write an update function that gets:
	1. Required ID
	2. Optional name. If it’s defined, then it must contain at least 2 integers
	3. Optional score. If it’s defined, then it must be a non-negative integer
7. Create a new function to upsert an item to the array


### Exercise #2

All questions must use at least one array method:
1. Create a function that calculates and returns the average score
2. Create a function that gets a minimum score and returns all users that have a score that's at least the given minimum
3. Create a function that gets 2 arrays of this kind and returns a new array where all objects are sorted by score in ascending order.
4. Write unit tests with Vitest to the 3 functions you just created.