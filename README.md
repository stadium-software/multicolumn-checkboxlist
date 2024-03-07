# Multicolumn CheckBoxList

A module to change the CheckBoxList display from a single column to multiple columns. 

https://github.com/stadium-software/multicolumn-checkboxlist/assets/2085324/f4ad2d2d-e733-4bae-9c52-c64b5da754f3

## Description
Sometimes we want to display a checkbox list with a large number of items. Finding and selecting items in such a list is easier for the user. This module displays the CheckBoxlist in as many columns as will fit into it's container. 

## Version 
1.0 - initial

1.1 - rewrite from row to column display

# Setup

## Application Setup
1. Check the *Enable Style Sheet* checkbox in the application properties

### Global Script Setup
1. Create a Global Script called "MultiColumnCheckboxList"
2. Drag a *JavaScript* action into the script
3. Add the Javascript below into the JavaScript code property
```javascript
/* Stadium Script v1.1 - see https://github.com/stadium-software/multicolumn-checkboxlist */
let checkboxLists = document.querySelectorAll(".multi-column-display");
let initialHeight = 34;
if (checkboxLists) initialHeight = window.getComputedStyle(checkboxLists[0]).getPropertyValue("height").replace("px", "");
let isOverflowX = (element) => {
    return element.scrollWidth != Math.max(element.offsetWidth, element.clientWidth);
};
let options = {
    childList: true,
    subtree: true,
};
let listsSetup = () => {
    for (let i = 0; i < checkboxLists.length; i++) {
        let observer = new MutationObserver(listsSetup);
        observer.disconnect();
        let checkBoxList = checkboxLists[i].children[0];
        let boundingClientRect = checkBoxList.getBoundingClientRect();
        if (boundingClientRect.height != initialHeight) checkBoxList.style.height = initialHeight + "px";
        if (isOverflowX(checkBoxList)) {
            do {
                checkBoxList.style.height = boundingClientRect.height + initialHeight * 3 + "px";
                boundingClientRect = checkBoxList.getBoundingClientRect();
            } while (isOverflowX(checkBoxList));
            do {
                checkBoxList.style.height = boundingClientRect.height - 1 + "px";
                boundingClientRect = checkBoxList.getBoundingClientRect();
            } while (!isOverflowX(checkBoxList));
            checkBoxList.style.height = boundingClientRect.height + 1 + "px";
            checkBoxList.style.visibility = "visible";
        }
        observer.observe(checkBoxList, options);
    }
};
listsSetup();
window.addEventListener("resize", () => {listsSetup();},true);
```

## Page Setup
1. Drag a *CheckBoxlist* control to a page 
2. Add the class "multi-column-display" to the *CheckBoxlist* classes property
3. Assign items to the CheckBoxList as you normally would

## Page.Load Setup
1. Drag the Global Script called "MultiColumnCheckboxList" into the Page.Load event handler

# Styling
The initial height of the checkbox lists can be defined

## Applying the CSS

**Stadium 6.6 or higher**
1. Create a folder called "CSS" inside of your Embedded Files in your application
2. Drag the two CSS files from this repo [*checkboxlist-columns-variables.css*](checkboxlist-columns-variables.css) and [*checkboxlist-columns.css*](checkboxlist-columns.css) into that folder
3. Paste the link tags below into the *head* property of your application
```html
<link rel="stylesheet" href="{EmbeddedFiles}/CSS/checkboxlist-columns.css">
<link rel="stylesheet" href="{EmbeddedFiles}/CSS/checkboxlist-columns-variables.css">
``` 

## Customising CSS
1. Open the CSS file called [*checkboxlist-columns-variables.css*](checkboxlist-columns-variables.css) from this repo
2. Adjust the variables in the *:root* element as you see fit
3. Overwrite the file in the CSS folder of your application with the customised file

## CSS Upgrading
To upgrade the CSS in this module, follow the [steps outlined in this repo](https://github.com/stadium-software/samples-upgrading)
