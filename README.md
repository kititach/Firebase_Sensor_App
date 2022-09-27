# Firebase_Sensor_App

## Edit file index.html
```
<!-- Complete Project Details at: https://RandomNerdTutorials.com/ -->

<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>ESP IoT Firebase App</title>

    <!-- update the version number as needed -->
    <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-app.js"></script>

    <!-- include only the Firebase features as you need -->
    <script src="https://www.gstatic.com/firebasejs/8.8.1/firebase-auth.js"></script>
    <script src="https://www.gstatic.com/firebasejs/8.8.1/firebase-database.js"></script>

    <script>
     // REPLACE WITH YOUR web app's Firebase configuration
      const firebaseConfig = {
        apiKey: "REPLACE_WITH_YOUR_Firebase_CONFIGURATION",
        authDomain: "REPLACE_WITH_YOUR_Firebase_CONFIGURATION",
        databaseURL: "REPLACE_WITH_YOUR_Firebase_CONFIGURATION",
        projectId: "REPLACE_WITH_YOUR_Firebase_CONFIGURATION",
        storageBucket: "REPLACE_WITH_YOUR_Firebase_CONFIGURATION",
        messagingSenderId: "REPLACE_WITH_YOUR_Firebase_CONFIGURATION",
        appId: "REPLACE_WITH_YOUR_Firebase_CONFIGURATION"
      };

      // Initialize firebase
      firebase.initializeApp(firebaseConfig);

      // Make auth and database references
      const auth = firebase.auth();
      const db = firebase.database();

    </script>
    <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.7.2/css/all.css" integrity="sha384-fnmOCqbTlWIlj8LyTjo7mOUStjsKC4pOpQbqyi7RrhN7udi9RwhKkMHpvLbHG9Sr" crossorigin="anonymous">
    <link rel="icon" type="image/png" href="favicon.png">
    <link rel="stylesheet" type="text/css" href="style.css">
</head>

<body>
  <!--TOP BAR-->
  <div class="topnav">
    <h1>Sensor Readings App <i class="fas fa-clipboard-list"></i></h1>
  </div>

  <!--AUTHENTICATION BAR (USER DETAILS/LOGOUT BUTTON)-->
  <div id="authentication-bar" style="display: none;">
    <p><span id="authentication-status">User logged in</span>
       <span id="user-details">USEREMAIL</span>
       <a href="/" id="logout-link">(logout)</a>
    </p>
  </div>

  <!--LOGIN FORM-->
  <form id="login-form" style="display: none;">
    <div class="form-elements-container">
      <label for="input-email"><b>Email</b></label>
      <input type="text" placeholder="Enter Username" id="input-email" required>

      <label for="input-password"><b>Password</b></label>
      <input type="password" placeholder="Enter Password" id="input-password" required>

      <button type="submit" id="login-button">Login</button>
      <p id="error-message" style="color:red;"></p>
    </div>
  </form>

  <!--CONTENT (SENSOR READINGS)-->
  <div class="content-sign-in" id="content-sign-in" style="display: none;">
    <div class="cards">
      <!--TEMPERATURE-->
      <div class="card">
        <p><i class="fas fa-thermometer-half" style="color:#059e8a;"></i> TEMPERATURE</p>
        <p><span class="reading"><span id="temp"></span> &deg;C</span></p>
      </div>
      <!--HUMIDITY-->
      <div class="card">
        <p><i class="fas fa-tint" style="color:#00add6;"></i> HUMIDITY</p>
        <p><span class="reading"><span id="hum"></span> &percnt;</span></p>
      </div>
    </div>
  </div>
    <script src="scripts/auth.js"></script>
    <script src="scripts/index.js"></script>
  </body>
</html>

```

## Edit file style.css
```
html {
    font-family: Verdana, Geneva, Tahoma, sans-serif;
    display: inline-block;
    text-align: center;
}

p {
    font-size: 1.2rem;
}

body {
    margin: 0;
}

.topnav {
    overflow: hidden;
    background-color: #049faa;
    color: white;
    font-size: 1rem;
    padding: 10px;
}

#authentication-bar{
    background-color:mintcream;
    padding-top: 10px;
    padding-bottom: 10px;
}

#user-details{
    color: cadetblue;
}

.content {
    padding: 20px;
}

.card {
    background-color: white;
    box-shadow: 2px 2px 12px 1px rgba(140,140,140,.5);
    padding: 5%;
}

.cards {
    max-width: 800px;
    margin: 0 auto;
    display: grid;
    grid-gap: 2rem;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
}

.reading {
    font-size: 1.4rem;
}

button {
    background-color: #049faa;
    color: white;
    padding: 14px 20px;
    margin: 8px 0;
    border: none;
    cursor: pointer;
    border-radius: 4px;
}
button:hover {
    opacity: 0.8;
}

.form-elements-container{
    padding: 16px;
    width: 250px;
    margin: 0 auto;
}

input[type=text], input[type=password] {
    width: 100%;
    padding: 12px 20px;
    margin: 8px 0;
    display: inline-block;
    border: 1px solid #ccc;
    box-sizing: border-box;
}
```

## Edit file auth.js
```
// listen for auth status changes
auth.onAuthStateChanged(user => {
  if (user) {
    console.log("user logged in");
    console.log(user);
    setupUI(user);
    var uid = user.uid;
    console.log(uid);
  } else {
    console.log("user logged out");
    setupUI();
    }
});
   
  // login
  const loginForm = document.querySelector('#login-form');
  loginForm.addEventListener('submit', (e) => {
  e.preventDefault();
  // get user info
  const email = loginForm['input-email'].value;
  const password = loginForm['input-password'].value;
  // log the user in
  auth.signInWithEmailAndPassword(email, password).then((cred) => {
    // close the login modal & reset form
    loginForm.reset();
    console.log(email);
  })
  .catch((error) =>{
    const errorCode = error.code;
    const errorMessage = error.message;
    document.getElementById("error-message").innerHTML = errorMessage;
    console.log(errorMessage);
   });
});
   
  // logout
  const logout = document.querySelector('#logout-link');
  logout.addEventListener('click', (e) => {
  e.preventDefault();
  auth.signOut();
  });
```

## Edit file index.js
```
const loginElement = document.querySelector('#login-form');
const contentElement = document.querySelector("#content-sign-in");
const userDetailsElement = document.querySelector('#user-details');
const authBarElement = document.querySelector("#authentication-bar");

// Elements for sensor readings
const tempElement = document.getElementById("temp");
const humElement = document.getElementById("hum");
//const presElement = document.getElementById("pres");

// MANAGE LOGIN/LOGOUT UI
const setupUI = (user) => {
  if (user) {
    //toggle UI elements
    loginElement.style.display = 'none';
    contentElement.style.display = 'block';
    authBarElement.style.display ='block';
    userDetailsElement.style.display ='block';
    userDetailsElement.innerHTML = user.email;

    // get user UID to get data from database
    var uid = user.uid;
    console.log(uid);

    // Database paths (with user UID)
    var dbPathTemp = 'UsersData/' + uid.toString() + '/temperature';
    var dbPathHum = 'UsersData/' + uid.toString() + '/humidity';
    //var dbPathPres = 'UsersData/' + uid.toString() + '/pressure';

    // Database references
    var dbRefTemp = firebase.database().ref().child(dbPathTemp);
    var dbRefHum = firebase.database().ref().child(dbPathHum);
    //var dbRefPres = firebase.database().ref().child(dbPathPres);

    // Update page with new readings
    dbRefTemp.on('value', snap => {
      tempElement.innerText = snap.val().toFixed(2);
    });

    dbRefHum.on('value', snap => {
      humElement.innerText = snap.val().toFixed(2);
    });

    /*dbRefPres.on('value', snap => {
      presElement.innerText = snap.val().toFixed(2);
    });*/

  // if user is logged out
  } else{
    // toggle UI elements
    loginElement.style.display = 'block';
    authBarElement.style.display ='none';
    userDetailsElement.style.display ='none';
    contentElement.style.display = 'none';
  }
}
```
