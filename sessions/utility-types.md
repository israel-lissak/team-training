Utility Types

TypeScript features many useful types that help you solve common tasks - these are called “Utility Types”. In the following guided exercises you will get introduced to them.

To begin, create a new TS file and define a type called `PlanetType` that has fields for: 

- ID
- display name
- mass
- primary color
- diameter

Now, create a well-typed function that gets 2 properties: (1) a planet and (2) updated information. The function returns a new planet.

Return after you implement the function

Test your implementation - can you update some of the planet fields? For example, is it easy to just update the display name and not touch any other field?

If not, modify the function so you are not forced to pass all fields each time.

Return after it’s possible

If you haven’t used one, you might want to use one of the built in Utility Types to get the job done. Search around and update your implementation accordingly.

Return once done

The most appropriate utility type here is definitely `Partial<T>`.

Once we have the first one covered, let’s experiment with other utility types.

In each exercise, search and use the most appropriate utility type(s):

1. Modify the update function you just created to make all properties of the provided planet (first parameter) non-editable.
    
2. It should not be possible to edit the ID of a planet. Once an ID is created, it cannot be modified. Please adjust the update function to not accept an ID as one of the editable properties.
    
3. Create a sub-type of `PlanetType` called `ExoticPlanetType` that includes all fields of `PlanetType` besides primary color but adds a new field that indicates if the planet is suitable for humans.
    
4. Create a const object that maps between planet ID (decipher its type directly from `PlanetType`) and various planets. The keys are planet IDs and the planets’ info are the values of the object.
    
5. Take a look at the following function:

```typescript
const getTeamMembersNames = async () => {
    return [ "ofek", "amos", "ben", "israel", "yoav", "adam" ];
};
```
- What’s the return type of the function?
- How do you obtain the return type of functions in TS? Try this right in the code editor.
- How to create a clean type from the actual return type of the function?
    
