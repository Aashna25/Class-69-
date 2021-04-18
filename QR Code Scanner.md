# QR Code Scanner

● Get permissions to use the Camera in application. 

● Use BarCodeScanner component to scan a QR code. 

● Display data from QR code inside a text component.

recall what new we had learned?

We learned how to write the user stories for an application. Our WiLy application had these user stories: 

● As a teacher, I want to enter the student id, book id and submit the info so that I can issue the book to any student. 

● As a teacher, I want to enter the student id, book id and submit the info so that I can return the book issued by the student back to the library. 

● As a teacher, I want to be able to search for any book's availability in the library so that I can inform the students about the availability of any book.

● As a teacher, I want to search for all the books issued by a particular student so that I can check for the books that were issued by the student. We also learned about how to create bottom tab navigation in our application. We had two tabs - Issue/Return Screen and Search Screen in our application.

In today's class, we will be creating the issue / return form for our application.

We can create a simple form containing two textinputs - for entering student id and book id. We can have a submit button which stores the information in the database.

But our library may have thousands and thousands of books. We might also have hundreds of students. Each book and student will have a unique book / student id. A book id might look like - BAZ43Y5 A student id might look like - ST12TN

What could be the problem when we ask someone to enter these books/student ids manually in the textbox?

It would be cumbersome and error prone for someone to type out these ids in a textbox. They could easily make a mistake and it would mess up the book management

You might have seen a Barcode on all products that you purchase - books, packed food items, etc. These codes have information embedded in them. They can contain strings / text which convey information like - name of the product, price, etc. The advantage of these QR code and Barcode is that they are machine readable and machines can read information printed on them very quickly.

Currently our Book Transaction screen only has a text inside a View component. Let's create a Button (using TouchableOpacity here) which will trigger the QR code scanner and display the scanned output as text.

We will be using an expo package (library) which will help us build Qr code scanner in our application. Let's install this package in our folder where we are working. Do you remember how we install packages using npm?

npm install expo-barcode-scanner

our app needs to ask for Camera permissions first before it can use it for scanning QR codes. Let's import the Barcode scanner component and permissions.

Let's define three states in our application: 

● hasCameraPermissions: null => This will tell if the user has granted camera permission to the application. 

● scanned: false => This will tell if the scanning has completed or not. 

● scannedData: '' => This will hold the scanned data that we get after scanning.

When we press our Scan QR Code Button in our application, we need to request for camera permission. Let's write a function - 

getCameraPermission - which can request for camera permission. Note that this function needs to be asynchronous because it takes time for the user to give camera permission to the application. 

The Permission component which we imported has a predefined function called .askAsync() which can request for various permissions. We will use this to request for camera permission inside our function and change the state of hasCameraPermissions.

 Note that .askAsync() returns an object with 'status' key containing the status of the permission granted by the user. If the user grants permission, status changes to 'granted' *Note: {status} automatically extracts the value from the object with key 'status'.

Let's call the function getCameraPermissions when the TouchableOpacity Button is pressed using its onPress prop. Teacher writes the code and tests it. What do you see? The camera permission is asked only once. If you grant the permission, the application will remember it

We need to tell our application what to do if, hasCameraPermissions is 'null' or 'false'. Let's say that we want to display a text "Request for camera permission" if hasCameraPermissions is false or null. If hasCameraPermissions is 'true', we will display whatever text is inside the scannedData state. Currently scannedData is an empty string. Let us write code for this in our render() function

Note that we can write JSX only in the return function. If we are writing javascript code, we do it inside the curly {} brackets.

Now, we want to display our BarCodeScanner when our Scan Button is clicked. How would we know that our Scan Button has been clicked?

Guide the student towards creating a new state called buttonState which keeps track if the button has been clicked. Let's create a new state called buttonState. 

● buttonState will be 'normal' when the application starts. 

● When the button is clicked to get camera permissions, buttonState should change to 'clicked'

Now, we want to return a BarCodeScanner component when the button is clicked and the user has given camera permissions.

The BarCodeScanner component automatically starts scanning using the Camera. It has a prop called onBarCodeScanned which can call a function to handle data received after scanning. We want to call this function only when scanned is false. Let's write a function called handleBarCodeScanned which is called when the scan is completed. 

● This function automatically receives the type of barcode scanned and the data inside the barcode. We can set the scannedData here to be equal to the data received after scanning. 

● Once the scan has been completed, we also want to set the scanned state to true. We also want to change state for the button to make it back to normal when the scan is completed.

We also need to tell the application what to render when the buttonState is normal. Here, we can render the text and the TouchableOpacity button, we were displaying earlier. When the button is clicked again, we would like to scan the QR code again. This can only happen if we set the scanned state to 'false' again. Otherwise, the handleBarCodeScanned function will not be called.