# Modern JavaScript from the Beginning

## Traversy Media/Udemy

- Source: [Modern JavaScript from the Beginning](https://www.udemy.com/modern-javascript-from-the-beginning/)

## Task List Project

### Setting up

1. first up we need to define all our UI variables like:
   - form,
   - taskList
   - clearBtn
   - filter
   - taskInput
2. then we setup a function to load all event listeners
3. don't forget to call the loadEventListeners() function immediately

### Creating tasks

1. first we create a event listener on the form listening for submit with a callback of addTask
2. we then add our new addTask function
3. First up we need to prevent the form from submitting with e.preventDefault();
4. we then run through creating the elements we need to create a single task item
   - first check if the input is empty
   - Create li element
   - Add class
   - Create text node and append to li
   - Create new link element
   - Add class
   - Add icon html
   - Append the link to the li
   - Append li to the ul
   - Clear input

### Deleting tasks

1. first we add an event listener into our loadEventListeners function on the task list ul - we're using event delegation here to bubble down
2. Using console.log(e.target) if helpful as it shows what we're actually clicking, which is the i element - but we want to select the <a> and delete that.
3. We then check we're hitting the parent element <a> but also that it has a class of 'delete-item'
4. then we target the parentElement of <a> which is our <li> and remove() it
5. to add an extra layer of UI we can add an if statement that uses a confirm alert to check before deleting

### Clear Tasks

1. first we add an event listener on the clear tasks button that calls a function called clearTasks
2. there's 2 ways to do this:
   1. first by using innerHTML and setting it to empty
   2. second is a while loop that checks if there is a firstChild in the list, then removes the first child (then loops over the rest removing them until the condition nolonger exists) - doesn't need an initial i value
3. the second loop option is actually faster
4. [checkout the research](https://jsperf.com/innerhtml-vs-removechild)

### Filter Tasks

1. first we add an event listener on the filter which calls a function called filterTasks
2. this function first grabs any value typed into the input
3. we convert it to lowercase as well to keep it consistent
4. doing a console.log on this is helpful to check its working
5. next we want to grab all the list items with a class of collection-item and loop over them
6. we use a forEach loop because querySelectorAll returns a node list
7. we then grab the text content of that task
8. using an if statement we check if any of the text matched our text being typed
9. if there's no match it will equal -1 so we say if it not equal to -1 keep it so say display block
10. otherwise if it does equal -1 we hide the item with display none

### Add tasks to local storage

- we want to keep the data when we reload the page so we're going to store it in local storage

1. first we need to add a function to our addTask function that grabs the taskInput.value as a parameter
2. this function takes the task as a parameter
3. we then create an empty variable named tasks to store each task
4. next we confirm that localStorage doesn't have a item called 'tasks' = if empty we create this new empty array in localStorage
5. if not empty we get the other tasks from localStorage
6. We have to run the data through JSON.parse() to convert it from a string and add it to our tasks array
7. we then then add the new task into the tasks array at the end
8. then we take our new array and add it back to local storage
9. but we need to use JSON.stringify() to convert the array back to a string first

### Persist tasks on refresh

- our tasks are now being kept in local storage but we need to now write the back onto our list when the browser refreshes

1. first we create a new event listener on DOMContentLoaded (happens when all content is loaded in the DOM)
2. then call our new getTasks function
3. in our fucntion the first thing we do is reuse the first part of our storeInLocalStorage function - checks if local storage has some tasks
4. we then need to loop through these tasks pulled from localStorage & add them back to our list
5. we can reuse the first section of our addTask function but change the taskInput.value to just task from our forEach

### Deleting from localStorage

- our delete function works on our list but when we refresh the page the deleted items return, so we need to also delete from localStorage at the same time

1. in our removeTask function we add a new function call removeTaskFromLocalStorage();
2. we need to pass in the whole element as we have no id
   removeTaskFromLocalStorage(e.target.parentElement.parentElement);
3. In our new function we repeat the process of checking localStorage for tasks
4. we then loop over that array and compare the text content of the task we've removed
5. if we get a match we pull it from the tasks array
6. then add the new array, minus the deleted task back to localStorage

### Clear tasks from local storage

- we also need to clear tasks from local storage when we hit the clear tasks button

1. first add a new function call to clearTasks
2. in our new clearTasksFromLocalStorage function we just call clear() on localStorage
