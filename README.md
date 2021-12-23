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
  
  // 2
  cy.get('[for="exampleInputEmail1"]').then((label) => {
     expect(label.text()).to.equal('Email address');
  })
  
  // 3
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
  .should('contain', 'checked')
```
