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


### Lists and Dropdowns

```js
it('lists and dropdowns', () => {

  // 1
  cy.visit('/');
  cy.get('nav nb-select').click();
  cy.get('.options-lits').contains('Dark').click();
  cy.get('nav nb-select').should('contain', 'Dark');
  cy.get('nb-layout-header nav').should('have.css', 'background-color', 'rgb(34, 43, 69)');
  
  // 2
  cy.get('nav nb-select').then(dropdown => {
    cy.wrap(dropdown).click();
    cy.get('.options-list nb-option').each((listItem, index) => {
      const itemText = listItem.text().trim();
      
      const colors = {
        "Light": "rgb(255, 255, 255)",
        "Dark": "rgb(34, 43, 69)",
        "Cosmic": "rgb(50, 50, 89)",
        "Corporate": "rgb(255, 255, 255)"
      }
      
      cy.wrap(listItem).click();
      cy.wrap(dropdown).should('contain', itemText);
      cy.get('nb-layout-header nav').should('have.css', 'background-color', colors[itemText]);
      if (index < 3) {
        cy.wrap(dropdown).click();
      }
    })
  })
})
```

### Web Tables

```js
it.only('Web tables') {
  cy.visit('/');
  cy.contains('Tables & Data').click();
  cy.contains('Smart Table').click();
  
  // 1
  cy.get('tbody').contains('tr', 'Larry').then(tableRow => {
    cy.wrap(tableRow).find('.nb-edit').click();
    cy.wrap(tableRow).find('[placeholder="Age"]').clear().type('25');
    cy.wrap(tableRow).find('.nb-checkmark').click
  });
  
  // 2
  cy.get('thead').find('.nb-plus').click();
  cy.get('thead').find('tr').eq(2).then(tableRow => {
    cy.wrap(tableRow).find('[placeholder="First Name"]').type('Arghun');
    cy.wrap(tableRow).find('[placeholder="Last Name"]').type('Mousanezhad');
    cy.wrap(tableRow).find('.nb-checkmark').click()
  })
  cy.get('tbody tr').first().find('td').then(tableColumns => {
    cy.wrap(tableColumns).eq(2).should('contain', 'Arghun');
    cy.wrap(tableColumns).eq(3).should('contain', 'Mousanezhad');
  }) 
  
  // 3
  cy.get('thead [placeholder="Age"]').type('20')
  cy.wait(500)
  cy.get('tbody tr').each(tableRow => {
    cy.get(tableRow).find('td').eq(6).should('contain', 20);
  })
}
```

### Web Date Pickers

```js
it('assert property', () => {
  cy.visit('/')
  cy.contains('Forms').click()
  cy.contains('Datepicker').click()
  
  let date = new Date();
  
  cy.contains('nb-card', 'Common Datepicker').find('input').then(input => {
    cy.wrap(input).click()
    cy.get('nb-calendar-day-datepicker').contains('17').click()
    cy.wrap(input).invoke('prop', 'value').should('contain', 'Dec 17, 2019')
  })
})
```

### Popups and Tooltips

```js
it.only('tooltip', () => {
  cy.visit('/')
  cy.contains('Modal & Overlays').click()
  cy.contains('Tooltip').click()
  
  cy.contains('nb-card', 'Colored Tooltips')
    .contains('Default').click()
  cy.get('nb-tooltip').should('contain', 'This is a tooltip')
})
```

### Dialog boxes

```js
it.only('dialog box', () => {
  cy.visit('/')
  cy.contains('Tables & Data').click()
  cy.contains('Smart Table').click()
  
  cy.get('tbody tr').first().find('.nb-trash').click()
  cy.on('Window:confirm', (confirm) => {
    expect(confirm).to.equal('Are you sure you want to delete?')
  })
})
```


## Working with APIs

**Login Command**

support/commands.js
```js
Cypress.Command.add(('loginToApplication') => {
  cy.visit('/login')
  cy.get("[placeholder='Email']").type('arghun.developer@gmail.com')
  cy.get("[placeholder='Password']").type('asd344t1')
  cy.get("form").submit()
})
```

integration/firstTest.js
```js
describe('Test with backend', () => {
  beforeEach('login to the app', () => {
    cy.loginToApplication()
  })
  
  it('verify correct request and response', () => {
    cy.server()
    cy.route('POST', '**/articles').as('postArticles')
    
    cy.contains('New Article').click()
    cy.get('[formcontrolname="title"]').type("This is a title")
    cy.get('[formcontrolname="description"]').type("This is a description")
    cy.get('[formcontrolname="body"]').type("This is a body of Article")
    cy.contains('Publish Article').click()
    
    cy.wait('@postArticles')
    cy.get('@postArticles').then(res => {
      console.log(res)
      expect(res.status).to.equal(200)
      expect(res.request.body.article.body).to.equal('This is a body of Article')
      expect(res.request.body.article.body).to.equal('This is a description')
    })
  })
})
```


## Mocking API Response

in the fixtures folder create a json file for the data you want to mock, for example you get from the server the list of tags, just copy and paste the response you got to the `tags.json` file inside fixtures folder. and then replace the array data wit your own mock data, like: 

tags.json

```js
tags: ["Cypress", "Automation", "Testing"]
```

```js
describe('testing', () => {
  beforeEach('login to the app', () => {
    cy.server()
    cy.route('GET', '**/tags', 'fixture :tags.json')
    cy.loginToTheApplication()
  })
  
  it('should gave tags with routing object', () => {
    cy.get('.tags-list')
    .should('contain', 'Cypress')
    .and('contain', 'automatino')
    .and('contain', 'testing')
  })
})
```

**To skip a test in cypress you should do it like this => `it.skip`**


## Verification of Browser API Calls

```js
describe('verify correct request and response', () => {

  cy.server()
  cy.route('POST', '**/articles').as('postArticles')

  cy.contains('New Article').click()
  cy.get('[formcontrolname="title"]').type('This is a title')
  cy.get('[formcontrolname="description"]').type('This is a description')
  cy.get('[formcontrolname="body"]').type('This is a body of the Article')
  cy.contains('Publish Article').click()
  
  cy.wait('@postArticles')
  cy.get('@postArticles').then(xhr => {
    console.log(xhr)
    expect(xhr.status).to.equal(200)
    expect(xhr.request.body.article.body).to.equal('This is a body of the Article')
    expect(res.request.body.article.body).to.equal('This is a description')
  })
})
```


```js
it('verify global feed likes count', () => {
  cy.route('GET', '**/articles/feed*', '{"articles": [], "articlesCount": 0}')
  cy.route('GET', '**/articles*', 'fixture:articles.json')
  
  cy.contains('Global Feed').click()
  cy.get('app-article-list button').then(listOfbuttons => {
    expect(listOfbuttons[0]).to.contain('1')
    expect(listOfbuttons[1]).to.contain('5')
  })
  
  cy.fixture('articles').then(file => {
    const articleLink = file.articles[1].slug;
    cy.route("POST", "**/articles/"+articleLink+"/favorite", articles)
  })
  
  cy.get('app-article-link button').eq(1).click().should('contain', '6')
})
```
