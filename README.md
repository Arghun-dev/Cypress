# Cypress

## Describe and write tests 

firstTest.js
```js
describe('Our first suit', () => {

  beforeEach('code for every test', () => {
    // repetiitve code for example a login function to not re-write in each test this code will be run before each of these tests
  })
  
  it('some test name', () => {
      
  })

  it('some test name', () => {

  })

  it('some test name', () => {

  })

  describe('Our sute section', () => {
    it('some test name', () => {
      
    })
  })

  it('first test', () => {
  
  })
  
  it('second test', () => {
  
  })
  
  it('third test', () => {
  
  })
})


describe('Our second suit', () => {
  it('first test', () => {
  
  })
  
  it('second test', () => {
  
  })
  
  it('third test', () => {
  
  })
})
```

## Types of Locators

always add `/// <reference types="cypress" />`

this will help you develop `cypress` it gives you access the cypress syntax highlights.

`cy.get()` => this is the basic method to get any web element.

`cy.get('')` => inside of the `''` you should provide your Locators name

**Locators:**

1. by Tag Name => `cy.get('input')`
2. by ID => `cy.get('#inputEmail1')`
3. by ClassName => `cy.get('.inputEmail1')`
4. by Attribute name => `cy.get('[placeholder]')`
5. by Attribute name and value => `cy.get('[placeholder="Email"]')`
6. by Class value => `cy.get('[class="input-full-width size-medium shape-rectangle"]')`
7. by Tag name and Attribute with value => `cy.get('input[placeholder="Email"]')`
8. by two different attributes => `cy.get('[placeholder="Email"][fullwidth or type="email"]')`
9. by tag name, Attribute with value, ID and Class name => `cy.get('input[placeholder="Email"]#inputEmail1.input-full-width')`
10. The most recommended way by Cypress => `cy.get('[data-cy="inputEmail1"]')`


In order to open our application in the `Cypress`, we need to execute the command `cy.visit('/')`


for example we want to /forms/layouts

in the menu we have **Forms** and **Form Layouts**

```js
describe('Our first suit', () => {
  it ('first test', () => {
    cy.visit('/');
    cy.contains('Forms').click()
    cy.contains('Form Layouts').click()
  })
})
```

to only run one test, run this code

```js
it.only('test to run just this', () => {
  
})
```

Imagine we wanna find the button with the text `Sign in`, but imagine we have two `Sign in` button, but we want to find `Sign in` button with the attribute `status="warning"`

```js
cy.contains('[status="warning"]', 'Sign in');
```

The above code says, hey cypress let's find for me the web element, which contains `Sign in` and with the attribute and value of `status="warning"`

Imagine, we are looking for Sign in button which does not have a specific identifier, how should we do that? in order find this button we need find first unique id in this section all the elements around that button, and then travel to that button.

**This is how you travel through the DOM**

```js
cy.get('#input3')
  .parents('form')
  .find('button')
  .should('contain', 'Sign in')
  .parents('form')
  .find('nb-checkbox')
  .click()
```

**Tip: `find` method is only to call and find child elements and you can not use it to get parent elements**

**How to find sibling elements in `Cypress` => in order to, we first need to travel to the parent element and then travel to the child element**

**Very Important Tip** in Cypress if you want to use the `DRY` you should use `.then` actually you can not save something in a const variable and re use it without using `.then` you know why? Because `Cypress` is `Asynchronous`.

### Invoke Command

**Example: **

How to find the the email input:

```js
it.only('invoke command', () => {
  cy.visit('/');
  cy.contains('Forms').click();
  cy.contains('Form Layouts').click();
  
  // 1
  cy.get('[for="exampleInputEmail1"]').should('contain', 'Email address');
  
  // 2 in this example we got the result of this function `ct.get(?)` => saved it as `label` => the `label` here is jQuery element and then we use jQuery method `.text()` to get the text from this label
  cy.get('[for="exampleInputEmail1"]').then((label) => {
     expect(label.text()).to.equal('Email address');
  })
  
  // 3 in this example we did the same but here, we used Cypress invoke method, to get the `text` from it. We saved this as a text parameter to this function and we make the assertion that it should equal to `Email address`
  cy.get('[for="exampleInputEmail1"]').invoke('text').then((text) => {
     expect(text).to.equal('Email address');
  })
})
```

**Testing checkbox**

```js
cy.contains('nb-card', 'Basic form')
  .find('nb-checkbox')
  .click()
  .find('.custom-checkbox')
  .invoke('attr', 'class')
  // .should('contain', 'checked')
  .then(classValue => {
    expect(classValue).to.contain('checked')
  })
```

**one way to make an assertion, is just using `should` method, and second way of making the assetion is to extract, result of the invoke function, as a parameter, for then function and then make assertion using expect.**

**So, this was the first example of using the invoke function, is getting the attribute values, and making some assertions with these values, or anything you need to do with this values.**


let's say we have a date picker, when you click on to the date picker.

```js
it.only('assert property', () => {
  cy.visit('/');
  cy.contains('Forms').click();
  cy.contains('Datepicker').click();
  
  cy.contains('nb-card', 'Common Datepicker').find('input').then(input => {
    cy.wrap(input).click()
    cy.get('nb-calendar-day-picker').contains('17').click()
    cy.wrap(input).invoke('prop', 'value').should('contain', 'Dec 17, 2019')
  })
})
```


### Checkboxes and Radio Buttons


**Radio Buttons**
```js
cy.visit('/');
cy.contains('Forms').click();
cy.contains('Form Layout').click();

cy.contains('nb-card', 'Using the Grid').find('[type="radio"]').then(radioButtons => {
  cy.wrap(radioButtons).first().check({ force: true }).should('be.checked');
  
  cy.wrap(radioButtons).eq(1).check({ force: true });
  
  cy.wrap(radioButtons).eq(0).should('not.be.checked');
  
  cy.wrap(radioButtons).eq(2).should('be.disabled');
})
```

**CheckBoxes**
```js
it.only('check boxes', () => {
  cy.visit('/');
  cy.contains('Forms').click();
  cy.contains('Form Layouts').click();
  
  cy.get('[type="checkbox"]').check({ force: true }); // you will not be able to uncheck the checkbox with this method, to uncheck the checkbox you have to use click method
  
  cy.get('[type="checkbox"]').click({ force: true }); // this way you can uncheck the checkbox which is previously checked
})
```
