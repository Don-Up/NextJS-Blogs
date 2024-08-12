# Tailwind Common Props



## FLEX

#### `flex`
Applies `display: flex;` to an element, enabling a flexible container for layout control.

#### `flex-col`
Sets `flex-direction: column;` to stack flex items vertically.

#### `flex-row`
Sets `flex-direction: row;` to arrange flex items horizontally.

####   `items-center`

Aligns flex items along the center of the cross axis (default to vertically).

![image-20240714084129122](assets/image-20240714084129122.png)

####  `justify-center`

Aligns flex items along the center of the main axis (default to horizontally).

![image-20240714083050728](assets/image-20240714083050728.png)

#### `justify-between`

Distributes flex items evenly with space between them, aligning the first item to the start and the last item to the end of the main axis.

![image-20240714083121266](assets/image-20240714083121266.png)

#### `justify-around`

Distributes flex items evenly with equal space around them. This places an equal amount of space between each item and the container's edges.

![image-20240714083320373](assets/image-20240714083320373.png)

#### `justify-evenly`

Distributes flex items with equal space between them, ensuring even spacing both between each item and the container's edges.

![image-20240714083414542](assets/image-20240714083414542.png)

#### `flex-1`

Sets `flex: 1 1 0%;` allowing the element to grow and fill available space.



## MIN&MAX

#### 1. `min-h-screen`
Sets the minimum height of an element to 100% of the viewport height (`min-height: 100vh`), ensuring the element covers the full height of the screen.

#### 2. `max-w-md`
Sets the maximum width of an element to a medium size, typically 28rem or 448px, constraining the element's width for better readability and layout control.



## BORDER&ROUND

### Tailwind CSS Utility Classes

#### 1. `border`
Adds a border of `1px` width to an element, defaulting to a solid style.

#### 2. `border-gray-300`
Sets the border color to a shade of gray (`#D1D5DB`), providing a subtle border appearance.

#### 3. `border-b`
Adds a border to the bottom of an element only, with a default `1px` solid style.

#### 4. `rounded-md`
Applies medium-sized rounded corners (`border-radius: 0.375rem` or `6px`) to an element, giving it a slightly rounded appearance.



## SIze

#### `w-full`

Sets the element's width to 100% of its parent container's width.



## Pseudo Class

### hover

The `hover` pseudo-class in Tailwind CSS can be used to apply styles when the user hovers over an element.

#### Example in JSX

```jsx
<div className="hover:bg-lightblue">
  Hover over me!
</div>
```

### focus

The `focus` pseudo-class in Tailwind CSS applies styles to an element when it receives focus.

#### Example in JSX

```jsx
<input className="focus:border-blue" type="text" placeholder="Focus on me" />
```

### active

The `active` pseudo-class in Tailwind CSS applies styles to an element when it is being activated, such as during a click.

#### Example in JSX

```jsx
<button className="active:bg-darkblue active:text-white">
  Click me!
</button>
```



