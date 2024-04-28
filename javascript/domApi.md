# DOM API

## **HTML DOM Element `getBoundingClientRect()`**

**<https://www.w3schools.com/jsref/met_element_getboundingclientrect.asp>**

- **Description**  
The **`getBoundingClientRect()`** method returns a **`DOMRect` object** with **8** properties describing the size of an element and its position relative to the **viewport**: `left`, `top`, `right`, `bottom`, `x`, `y`, `width`, `height`.

- **Syntax**

  ```js
  const rect = element.getBoundingClientRect();
  ```

- :memo: **Note:**  
The scrolling that has been done is taken into account. This means that the rectangle's edges (top, left, bottom, and right) change their values every time the scrolling position changes.
