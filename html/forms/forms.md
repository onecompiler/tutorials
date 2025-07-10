Forms are used to collect user input and then the input is sent to the server for processing. 

`<form>` element is used to define a form.

```html
<form>
<!--form elements like input select etc-->
</form>
```

Mostly commonly used form attributes

| Attribute | Description |
|----|----|
| action | Specifies the action - how backend script ready to process the data sent.|
| method | GET or POST method to be used to upload data |
| target | Specify the target window or frame like _blank, _parent etc. |
| enctype | Specify how the browser encodes the data, for example: **application/x-www-form-urlencoded** |
| novalidate | Disables built-in form validation |
| autocomplete | Enables/disables form autocomplete (on/off) |

# Form element - `<input>`

`<input>` element defines what kind of input like text fields, drop-down menus, checkboxes, radio buttons, submit buttons etc.

## Traditional Input Types
 
|Type |	Description|
|----|----|
|`<input type="text">` | To define a single-line text input field|
|`<input type="password">` | To define a single-line password input field|
|`<input type="radio">`| To define a radio button |
|`<input type="submit">`| To define a submit button |
|`<input type="checkbox">`| To define a checkbox |
|`<input type="file">`| To define a file upload box|
|`<input type="button">`| To define a clickable button |
|`<input type="reset">`| To define a reset button |
|`<input type="hidden">`| To define a hidden input field |

## HTML5 Input Types

|Type |	Description|
|----|----|
|`<input type="email">` | For email addresses with built-in validation|
|`<input type="tel">` | For telephone numbers|
|`<input type="url">` | For URLs with built-in validation|
|`<input type="number">` | For numeric input with spinner controls|
|`<input type="range">` | For numeric input with a slider control|
|`<input type="date">` | For date selection|
|`<input type="time">` | For time selection|
|`<input type="datetime-local">` | For date and time selection|
|`<input type="month">` | For month and year selection|
|`<input type="week">` | For week selection|
|`<input type="color">` | For color selection|
|`<input type="search">` | For search fields|

## Form Validation Attributes

|Attribute |	Description|
|----|----|
|`required` | Specifies that an input field must be filled out|
|`pattern` | Specifies a regular expression for validation|
|`min` | Specifies minimum value for numeric inputs|
|`max` | Specifies maximum value for numeric inputs|
|`minlength` | Specifies minimum length for text inputs|
|`maxlength` | Specifies maximum length for text inputs|
|`step` | Specifies legal number intervals|
|`placeholder` | Provides a hint about expected input|
|`readonly` | Makes an input field read-only|
|`disabled` | Disables an input field|
|`autocomplete` | Enables/disables autocomplete for a field|


## 1. Text and password input controls with accessibility

```html
<!DOCTYPE html>
<html>
   <head>
      <title>Text and Password Form Controls</title>
   </head>
   <body>
      <form>
         <label for="user-id">ID:</label>
         <input type="text" id="user-id" name="user-id" required placeholder="Enter your ID" /> <br><br>
         
         <label for="password">Password:</label>
         <input type="password" id="password" name="password" required minlength="8" placeholder="Min 8 characters" /> <br><br>
         
         <input type="submit" value="Submit" />
      </form>
   </body>
</html>
```
### Try yourself [here](https://onecompiler.com/html/3vw38t6hu)

## 2. Multi-line text input with labels

Consider you want to give input in more than one line like Address, you can do as shown below:

```html
<!DOCTYPE html>
<html>
   <head>
      <title>Form controls</title>
   </head>
   <body>
      <form>
         <label for="address">Address:</label><br>
         <textarea id="address" name="address" rows="6" cols="70" 
                   placeholder="Enter your complete address" 
                   required maxlength="500"></textarea>
         <br><br>
         <input type="submit" value="Submit" />
      </form>
   </body>
</html>
```
### Try yourself [here](https://onecompiler.com/html/3vw394pwd)

## 3. Radio button form control with accessibility

```html
<!DOCTYPE html>
<html>
   <head>
      <title>Radio Box Form Control Example</title>
   </head>
   <body>
      <form>
         <fieldset>
            <legend>Gender:</legend>
            
            <input type="radio" id="male" name="gender" value="male" required>
            <label for="male">Male</label><br>
            
            <input type="radio" id="female" name="gender" value="female">
            <label for="female">Female</label><br>
            
            <input type="radio" id="non-binary" name="gender" value="non-binary">
            <label for="non-binary">Non-binary</label><br>
            
            <input type="radio" id="prefer-not-to-say" name="gender" value="prefer-not-to-say">
            <label for="prefer-not-to-say">Prefer not to say</label>
         </fieldset>
         <br>
         <input type="submit" value="Submit" />
      </form>
   </body>
</html>
```
### Try yourself [here](https://onecompiler.com/html/3vw3a9d4g)

## 4. Check box form control with labels

```html
<!DOCTYPE html>
<html>
   <head>
      <title>Checkbox Form Control</title>
   </head>
   <body>
      <form>
         <fieldset>
            <legend>Select your skills:</legend>
            
            <input type="checkbox" id="html" name="skills" value="html">
            <label for="html">HTML</label><br>
            
            <input type="checkbox" id="javascript" name="skills" value="javascript">
            <label for="javascript">JavaScript</label><br>
            
            <input type="checkbox" id="nodejs" name="skills" value="nodejs">
            <label for="nodejs">Node.js</label><br>
            
            <input type="checkbox" id="reactjs" name="skills" value="reactjs">
            <label for="reactjs">React.js</label>
         </fieldset>
         <br>
         <input type="submit" value="Submit" />
      </form>
   </body>
</html>
```
### Try yourself [here](https://onecompiler.com/html/3vw3b3dr7)

## 5. HTML5 Input Types Examples

### Email, URL, and Tel inputs
```html
<!DOCTYPE html>
<html>
   <head>
      <title>HTML5 Contact Form</title>
   </head>
   <body>
      <form>
         <label for="email">Email:</label>
         <input type="email" id="email" name="email" required 
                placeholder="user@example.com"><br><br>
         
         <label for="website">Website:</label>
         <input type="url" id="website" name="website" 
                placeholder="https://example.com"><br><br>
         
         <label for="phone">Phone:</label>
         <input type="tel" id="phone" name="phone" 
                pattern="[0-9]{3}-[0-9]{3}-[0-9]{4}"
                placeholder="123-456-7890"><br><br>
         
         <input type="submit" value="Submit" />
      </form>
   </body>
</html>
```

### Number and Range inputs
```html
<!DOCTYPE html>
<html>
   <head>
      <title>Numeric Inputs</title>
   </head>
   <body>
      <form>
         <label for="age">Age:</label>
         <input type="number" id="age" name="age" 
                min="18" max="100" step="1" required><br><br>
         
         <label for="price">Price Range: $<span id="price-value">50</span></label><br>
         <input type="range" id="price" name="price" 
                min="0" max="100" value="50" step="5"
                oninput="document.getElementById('price-value').textContent = this.value"><br><br>
         
         <input type="submit" value="Submit" />
      </form>
   </body>
</html>
```

### Date and Time inputs
```html
<!DOCTYPE html>
<html>
   <head>
      <title>Date and Time Inputs</title>
   </head>
   <body>
      <form>
         <label for="birthday">Birthday:</label>
         <input type="date" id="birthday" name="birthday" 
                min="1900-01-01" max="2024-12-31" required><br><br>
         
         <label for="appointment-time">Appointment Time:</label>
         <input type="time" id="appointment-time" name="appointment-time" 
                min="09:00" max="18:00" required><br><br>
         
         <label for="meeting">Meeting Date & Time:</label>
         <input type="datetime-local" id="meeting" name="meeting" required><br><br>
         
         <label for="month">Select Month:</label>
         <input type="month" id="month" name="month"><br><br>
         
         <label for="week">Select Week:</label>
         <input type="week" id="week" name="week"><br><br>
         
         <input type="submit" value="Submit" />
      </form>
   </body>
</html>
```

### Color and Search inputs
```html
<!DOCTYPE html>
<html>
   <head>
      <title>Color and Search Inputs</title>
   </head>
   <body>
      <form>
         <label for="favcolor">Select your favorite color:</label>
         <input type="color" id="favcolor" name="favcolor" value="#ff0000"><br><br>
         
         <label for="search">Search:</label>
         <input type="search" id="search" name="search" 
                placeholder="Search..." autocomplete="off"><br><br>
         
         <input type="submit" value="Submit" />
      </form>
   </body>
</html>
```

## 6. Form Validation Example
```html
<!DOCTYPE html>
<html>
   <head>
      <title>Form Validation Example</title>
      <style>
         input:invalid {
            border: 2px solid red;
         }
         input:valid {
            border: 2px solid green;
         }
      </style>
   </head>
   <body>
      <form>
         <label for="username">Username (4-12 characters, alphanumeric):</label>
         <input type="text" id="username" name="username" 
                pattern="[a-zA-Z0-9]{4,12}" 
                title="Username must be 4-12 alphanumeric characters"
                required><br><br>
         
         <label for="user-email">Email:</label>
         <input type="email" id="user-email" name="user-email" required><br><br>
         
         <label for="user-age">Age (18-65):</label>
         <input type="number" id="user-age" name="user-age" 
                min="18" max="65" required><br><br>
         
         <label for="password">Password (min 8 chars):</label>
         <input type="password" id="password" name="password" 
                minlength="8" required><br><br>
         
         <input type="submit" value="Register" />
      </form>
   </body>
</html>
```

# Form element - `<select>`

`<select>` element is used to define a drop-down menu.

```html
<!DOCTYPE html>
<html>
   <head>
      <title>Select Form Element</title>
   </head>
   <body>
      <form>
         <label for="languages">Choose a language:</label>
         <select id="languages" name="languages" required>
            <option value="">--Please choose an option--</option>
            <option value="english">English</option>
            <option value="french">French</option>
            <option value="hindi">Hindi</option>
            <option value="italian">Italian</option>
            <option value="japanese">Japanese</option>
         </select>
         <br><br>
         
         <label for="skills-multiple">Select multiple skills (Ctrl/Cmd + Click):</label><br>
         <select id="skills-multiple" name="skills-multiple" multiple size="5">
            <option value="html">HTML</option>
            <option value="css">CSS</option>
            <option value="javascript">JavaScript</option>
            <option value="python">Python</option>
            <option value="java">Java</option>
         </select>
         <br><br>
         
         <input type="submit" value="Submit" />
      </form>
   </body>
</html>
```
### Try yourself [here](https://onecompiler.com/html/3vw3b9hrz)

## Modern Form Best Practices

1. **Always use labels**: Every form control should have an associated `<label>` element for accessibility.

2. **Use semantic HTML**: Use `<fieldset>` and `<legend>` to group related form controls.

3. **Add validation**: Use HTML5 validation attributes like `required`, `pattern`, `min`, `max`, etc.

4. **Provide helpful placeholders**: Use the `placeholder` attribute to give users hints about expected input.

5. **Make forms keyboard accessible**: Ensure all form controls can be navigated using only the keyboard.

6. **Use appropriate input types**: Choose the correct HTML5 input type for better mobile experience and built-in validation.

7. **Add ARIA attributes when needed**: For complex forms, use ARIA attributes to improve screen reader support.

## Complete Form Example

```html
<!DOCTYPE html>
<html>
   <head>
      <title>Complete Registration Form</title>
      <style>
         form { max-width: 500px; margin: 0 auto; }
         fieldset { margin-bottom: 20px; padding: 10px; }
         label { display: inline-block; width: 120px; margin-bottom: 10px; }
         input, select, textarea { margin-bottom: 10px; }
         .error { color: red; }
         input:invalid { border-color: red; }
         input:valid { border-color: green; }
      </style>
   </head>
   <body>
      <h1>User Registration Form</h1>
      <form action="/submit" method="POST" enctype="multipart/form-data">
         <fieldset>
            <legend>Personal Information</legend>
            
            <label for="fullname">Full Name:</label>
            <input type="text" id="fullname" name="fullname" required 
                   placeholder="John Doe" autofocus><br>
            
            <label for="email">Email:</label>
            <input type="email" id="email" name="email" required 
                   placeholder="john@example.com"><br>
            
            <label for="phone">Phone:</label>
            <input type="tel" id="phone" name="phone" 
                   pattern="[0-9]{3}-[0-9]{3}-[0-9]{4}"
                   placeholder="123-456-7890"><br>
            
            <label for="birthdate">Birth Date:</label>
            <input type="date" id="birthdate" name="birthdate" 
                   max="2006-12-31" required><br>
            
            <label for="age">Age:</label>
            <input type="number" id="age" name="age" 
                   min="18" max="120" readonly><br>
         </fieldset>
         
         <fieldset>
            <legend>Account Details</legend>
            
            <label for="username">Username:</label>
            <input type="text" id="username" name="username" 
                   pattern="[a-zA-Z0-9]{4,20}" required
                   title="4-20 alphanumeric characters"><br>
            
            <label for="password">Password:</label>
            <input type="password" id="password" name="password" 
                   minlength="8" required><br>
            
            <label for="website">Website:</label>
            <input type="url" id="website" name="website" 
                   placeholder="https://example.com"><br>
         </fieldset>
         
         <fieldset>
            <legend>Preferences</legend>
            
            <label for="newsletter">Newsletter:</label>
            <input type="checkbox" id="newsletter" name="newsletter" value="yes">
            Subscribe to newsletter<br><br>
            
            <label for="theme-color">Theme Color:</label>
            <input type="color" id="theme-color" name="theme-color" value="#007bff"><br>
            
            <label for="experience">Experience Level:</label>
            <input type="range" id="experience" name="experience" 
                   min="0" max="10" value="5" step="1">
            <output for="experience"></output><br>
         </fieldset>
         
         <fieldset>
            <legend>Additional Information</legend>
            
            <label for="resume">Resume:</label>
            <input type="file" id="resume" name="resume" 
                   accept=".pdf,.doc,.docx"><br>
            
            <label for="bio">Bio:</label><br>
            <textarea id="bio" name="bio" rows="4" cols="50" 
                      maxlength="500" placeholder="Tell us about yourself..."></textarea>
         </fieldset>
         
         <input type="reset" value="Reset Form">
         <input type="submit" value="Register">
      </form>
   </body>
</html>
```

