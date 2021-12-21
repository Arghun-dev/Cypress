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
