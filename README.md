# Multicolumn CheckBoxlist

A module to change the default CheckBoxList display from single column to a multi-column. 


https://github.com/stadium-software/multicolumn-checkboxlist/assets/2085324/9a333f8e-e835-444e-825b-0d60fea534c8


## Description
Sometimes we want to display a checkbox list with a large number of items. Finding and selecting items in such a list is easier when the items are in multiple columns because users don't have to scroll as much. The module below will display the CheckBoxlist in as many columns as will fit into it's container. 


## Version 
1.0 - initial

# Setup

## Application Setup
1. Check the *Enable Style Sheet* checkbox in the application properties

## Global Script Setup
1. Create a Global Script called "MultiColumnCheckboxList"
2. Add the input parameters below to the Global Script
   1. ClassName
3. Drag a *JavaScript* action into the script
4. Add the Javascript below into the JavaScript code property
```javascript
/* Stadium Script Version 1.0 */
let checkboxContainerClass = ~.Parameters.Input.ClassName;
let checkboxContainer = document.querySelector("." + checkboxContainerClass);
let setupCheckboxList = () => {
    let wdth = 0;
    let sheetid = checkboxContainerClass + "-sheet";
    let sheet = document.getElementById(sheetid);
    if (!sheet) { 
        sheet = document.createElement('style');
        sheet.type = "text/css";
        sheet.id = sheetid;
        document.head.appendChild(sheet);
    }
    sheet.innerHTML = "";
    let checkboxes = checkboxContainer.querySelectorAll(".checkbox");
    for (let i=0;i<checkboxes.length;i++) {
        if (checkboxes[i].getBoundingClientRect().width > wdth) {
            wdth = checkboxes[i].getBoundingClientRect().width;
        }
    }
    sheet.innerHTML = "." + checkboxContainerClass + " {width: 100%; & .error-border {width: 100%;display: grid;grid-template-columns: repeat(auto-fill, minmax(" + Math.floor(wdth) + "px, 1fr));}}";
    observer.observe(checkboxContainer, options);
};
let options = {
    childList: true,
    subtree: true,
},
observer = new MutationObserver(setupCheckboxList);
setupCheckboxList();
```

## Page Setup
1. Drag a *CheckBoxlist* control to a page 
2. Add a class to the *CheckBoxlist* classes property to uniquely identify the control (e.g. multi-column-display)
3. Assign items to the CheckBoxList as you normally would

## Page.Load Setup
1. Drag the Global Script called "MultiColumnCheckboxList" into the Page.Load event handler
2. Enter the classname you added to the *CheckBoxlist* in the "ClassName" input parameter field (e.g. multi-column-display)
