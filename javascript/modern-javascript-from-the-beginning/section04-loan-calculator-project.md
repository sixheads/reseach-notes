# Modern JavaScript from the Beginning

## Traversy Media/Udemy

- Source: [Modern JavaScript from the Beginning](https://www.udemy.com/modern-javascript-from-the-beginning/)

## Loan Calculator Project

- uses a simple formula to calculate personal loan rates
- you can always google the correct formula and use it in your code

1. first we need to listen for the submit on our #loan-form and on submit have a callback function calculateResults
2. next we create our new calculateResults function
3. prevent default first so the form doesn't refresh the page
4. we then create variables for all our form fields and results

- amount
- interest
- years
- monthlyPayment
- totalPayment
- totalInterest

5.  then we need to setup the formula:
    - principal
    - calculatedInterest
    - calculatedPayments
6.  Next we calculate the monthly payment
7.  then before we write the results to the result fields we first check if the 'monthly' number returned is finite
8.  Then within that if statement if the number is finite we add the results to the result fields by setting their value
9.  In case of an error we setup a showError function
10. this function adds a new element to the page with the error text, then hides the error message after 3 sec with setTimeout

- to add some nice UI experience and a loading image
  1. first we uncomment the loader
  2. then we hide the #loading & #results with css
  3. next we want the calculateResults function to run after a few seconds rather than immediately
  - we remove the event from the calculateResults function
  - now we change the submit event listener to use an annonymous function instead of the calculateResults function
  - add the event here
  - then move the preventDefault into this new function instead

### submit function

- we first make sure we hide the results
- then set the loading to display block
- using setTimeout we run the calculateResults function after 2 sec
- in calculateResults after the calculation we also need to show the result and hide the loading again
- finally in our showError function at the top we also need to hide both the results and loader when the error shows
