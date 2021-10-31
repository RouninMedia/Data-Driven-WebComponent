# Data-Driven-WebComponent
An example of a Data Driven Web Component

___________

> To add: Declarative Shadow DOM Version
___________

**N.B.** I wanted to find out how straightforward (or not) this would be, after reading the following comment by **Orlando Brown**:

_"I remember when I first stumbled across Web Components, my first thought was, "oh yes! watch out React there is a new kid on the block and its native to the browser, oh shit quick let me read the docs and see what it can do..." \[...\] After an hour messing around trying to build a `ul` list with data pulled from a fetch request, something we do everyday in vanilla & JS frameworks it dawned on me, this requires way to much fiddling around to get working. Infact I don't think i actually got it working the way I wanted it to and I don't remember ever going back to it the day after"_

beneath this post by **Lea Verou**: _https://lea.verou.me/2020/09/the-failed-promise-of-web-components/_

## HTML
```
<colour-list scheme="rainbow"></colour-list>
<colour-list scheme="zebra"></colour-list>
```

## Javascript
```
const schemes = {
  rainbowJSON : '["red", "orange", "yellow", "green", "blue", "indigo", "violet"]',
  zebraJSON : '["black", "white", "black", "white", "black"]'
}

class colourList_CustomElement extends HTMLElement {
  
  constructor() {
    super();
    this.root = this.attachShadow({mode: "open"});
  }

  connectedCallback() {

    this.root.innerHTML = `
    
      <style>
        :host {
          display: inline-block;   /* <= Because Custom elements are display:inline by default */
          contain: content;        /* <= Because this delivers an immediate performance win */
        }
        
        ul {
          margin: 0 24px 0 0;
          padding: 0;
          width: 200px;
          list-style-type: none;
        }
        
        li {
          height: 24px;
          text-align: center;
          font-weight: bold;
          text-shadow: 1px 1px rgba(0, 0, 0, 0.7);
        }
        
      </style>
      
    `;
    
    let schemeJSON = schemes[this.getAttribute('scheme') + 'JSON'];
    let colours = JSON.parse(schemeJSON);
    let colourList = document.createElement('ul');
    let listItem;
    
    for (let colour of colours) {
    
      listItem = document.createElement('li');
      let textShadow = (colour === 'black') ? 'text-shadow: 1px 1px rgba(255, 255, 255, 0.7);' : '';
      listItem.setAttribute('style', 'color: '+ colour + '; background-color: ' + colour + ';' + textShadow);
      listItem.textContent = colour;
      colourList.appendChild(listItem);

    }
    
    this.root.appendChild(colourList);
  }
}

customElements.define('colour-list', colourList_CustomElement);

```
