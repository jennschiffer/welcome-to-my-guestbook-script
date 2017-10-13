WeLcOmE tO mY gUeStBoOk!!
=========================

## a glitch workshop for empireconf 2017

in this 6-part workshop, students are going to use glitch to take a static web page and turn it into a dynamic node-powered early-web-styled guestbook that their users can post messages to which are saved in a database to be read whenever!

this document outlines the workshop in its entirety! each part has its own glitch app to show the process from static to full-stack for the ease of instruction and also for students to follow along with if they want to get ahead or fall behind

Here is the entire workshop series:
* [part 1](https://glitch.com/edit/#!/empireconf-starter) - getting acquainted with glitch and our static guestbook
* [part 2](https://glitch.com/edit/#!/empireconf-starter-2) - using javascript to show table rows
* [part 3](https://glitch.com/edit/#!/empireconf-starter-3) - allowing folks to post to the guestbook
* [part 4](https://glitch.com/edit/#!/empireconf-starter-4) - taking it full stack using node
* [part 5](https://glitch.com/edit/#!/empireconf-starter-5) - create a database to store guestbook postings
* [part 6](https://glitch.com/edit/#!/empireconf-starter-6) - finish our fully persistent guestbook


### [part one](https://glitch.com/edit/#!/empireconf-starter): getting acquainted with glitch and our static guestbook

in this section, students will be introduced to the glitch dev environment, since that is what they will be working in for the next few hours.

instructor should
1. talk about how glitch works
2. talk about what guestbooks are
3. guide students in the following steps:

students should
1. log in to glitch so they can save their projects forever
2. remix the [starter app](https://glitch.com/edit/#!/empireconf-starter) so they can update their own work
3. make changes to the css and html to personalize their soon to be very cool guestbook



### [part two](https://glitch.com/edit/#!/empireconf-starter-2): using javascript to show table rows

in this section, students will use javascript to show their static guestbook records

instructor should
1. explain how this is laying the foundation for database use
2. talk about what JSON is
3. guide students in the following steps:

students should
1. add a few rows to their html table
2. add a script tag at the footer of index.html
  
  ```
  (function(){
      console.log('hello world!')
  })()
  ```
  
3. comment out or delete html table rows except header
4. create JSON for their guestbook entries
  
  ```
   const guestbookEntries = [
      {
        name: 'jenn',
        twitter: 'jennschiffer',
        message: 'thanks for attending my workshop wowowowow!'
      },
      {
        name: 'pirijan',
        twitter: 'pketh',
        message: 'ヽ(๏∀๏ )ﾉ'
      },
      {
        name: 'empire conf',
        twitter: 'empirejs',
        message: 'welcome to empire conf!'
      }
    ]
  ```
  
5. iterate through each object and append a row to the html table.
  
  ```
  // find the table in our guestbook
  const guestbookTable = document.getElementsByTagName('table')[0];

  // for each entry in guestbookEntries, create a row for that entry
  // and append it to the table
  for (let i = 0; i < guestbookEntries.length; i++) {

      // create tr element
      const newRow = document.createElement('tr');

      // set the inner html of that element to have the record
      newRow.innerHTML = `<tr>
        <td>${i+1}</td>
        <td>${guestbookEntries[i].name}</td>
        <td><a href="https://twitter.com/${guestbookEntries[i].twitter}">@${guestbookEntries[i].twitter}</a></td>
        <td>${guestbookEntries[i].message}</td>
      </tr>`;

      // append that record to the table
      guestbookTable.appendChild(newRow)
  }
  ```
  
6. create a javascript file called `guestbook.js`
7. import `guestbook.js` in `index.html`
8. make sure our guestbook shows those new rows
9. update `README.md` to reflect our new file


### [part three](https://glitch.com/edit/#!/empireconf-starter-3): allowing folks to post to the guestbook

in this section, students will create a form that allows users to post a message which is added to the html table but only locally (when refreshing the page, those messages disappear).


instructor should
1. talk about how hilariously unsecure guestbook forms used to be
2. discuss validation and xss prevention
3. guide students in the following steps:

students should
1. add an html form with name, twitter, message text inputs and submit button
  
  ```
    <!-- here is our form for allowing users to post to the guestbook! -->
    <form>
      <h2>
        Please say nice things to meeeee!
      </h2>

      <label for="name">
        Name: 
        <input id="name" name="name" type="text" placeholder="jenn" />
      </label>

      <label for="twitter">
        Twitter: @
        <input id="twitter" name="twitter" type="text" placeholder="TheRock" />
      </label>

      <label for="message">
        Message: 
        <input id="message" name="message" type="text" placeholder="wowowow cool text!" />
      </label>

      <input id="submitToGuestbook" type="submit" value="Post to the Guestbook!" />
    </form>
  ```

2. style the form to make it fun!

  ```
  form {
      background-color: lightgray;
      margin: 2em 0;
      padding: 1em;
      border: 2px solid #000;
  }

  form label {
      display: block;
      padding: 1em;
      border-bottom: 1px dashed #000;
  }

  form input[type="text"] {
      width: 50%;
      padding: 1em .5em; 
      font-size:1em;
  }

  form input[type="submit"] {
      margin: 1em 0 0;
      font-size: 1em;
  }
  ```
3. create a helper function to add guestbook records

  ```
  // a helper function for appending a row to our guestbook
  const addGuestbookRecord = function(name, twitter, message) {

      // create tr element
      const newRow = document.createElement('tr');

      // get current row count (minus the header row)
      const rowCount = guestbookTable.getElementsByTagName('tr').length - 1;

      // set the inner html of that element to have the record
      newRow.innerHTML = `<tr>
        <td>${rowCount + 1}</td>
        <td>${name}</td>
        <td><a href="https://twitter.com/${twitter}">@${twitter}</a></td>
        <td>${message}</td>
      </tr>`;

      // append that record to the table
      guestbookTable.appendChild(newRow)
  }
  ```
  
  and use it in our iterating for loop:
  
  ```
  // for each entry in guestbookEntries, create a row for that entry
  // and append it to the table
  for (let i = 0; i < guestbookEntries.length; i++) {

      // call the addGuestbookRecord helper to create a row
      addGuestbookRecord(guestbookEntries[i].name, guestbookEntries[i].twitter, guestbookEntries[i].message);
  }
  ```
4. create a submit event handler to add a record when a form is submitted

  ```
   // to handle the form submit, we need to add get the submit button
  const submitButton = document.getElementById('submitToGuestbook');
  
  // and we need to add an event handler to add our data to the guestbook when the submit button is clicked
  submitButton.onclick = function(e) {
    
      // this prevents the page from doing the default form submission,
      // which we want to control ourselves
      e.preventDefault();

      // we want to get our form 
      const guestbookForm = this.form;

      // we want to append a new row with our data
      addGuestbookRecord(guestbookForm.name.value, guestbookForm.twitter.value, guestbookForm.message.value);
    }
  ```
5. validate the form to make sure all fields are filled

  ```
    // and we need to add an event handler to add our data to the guestbook when the submit button is clicked
  submitButton.onclick = function(e) {
    
      // this prevents the page from doing the default form submission,
      // which we want to control ourselves
      e.preventDefault();

      // we want to get our form 
      const guestbookForm = this.form;

      // we should validate it
      if ( !guestbookForm.name.value || !guestbookForm.twitter.value || !guestbookForm.message.value) {
        alert('you need to enter all values to post to my guestbook!') 
      }
      else {
        // we want to append a new row with our data
        addGuestbookRecord(guestbookForm.name.value, guestbookForm.twitter.value, guestbookForm.message.value);
      }
    }
  ```

6. we need to cleanse the data to prevent malicious scripts from being entered onto our site. for example

  ```
  // submit this as a message for bad things to happen lol
  &lt;style>* { background: black; color: black; }&lt;/style>
  ```
  
  so we should create a helper function that converts tags to html entities that won't be called as code
  
  ```
    // a helper function for cleaning our form submission values to prevent xss
  const cleanseString = function(string) {
      return string.replace(/</g, "&lt;").replace(/>/g, "&gt;");
  }
  ```
  
  and we should call that helper function before the arguments are passed to create a new table row
  
  ```
  // we should validate it
  if ( !guestbookForm.name.value || !guestbookForm.twitter.value || !guestbookForm.message.value) {
      alert('you need to enter all values to post to my guestbook!') 
  }
  else {
      // we want to append a new row with our data
      addGuestbookRecord(guestbookForm.name.value, guestbookForm.twitter.value, guestbookForm.message.value);
  }
  ```

7. to prevent row duplicates, let's reset the form at the end of the onsubmit event handler

  ```
  // and reset the form to prevent duplicate records by mistake
  guestbookForm.reset();
  ```


### [part four](https://glitch.com/edit/#!/empireconf-starter-4): taking it full stack using node

in this section, students are gonna go full-stack and turn this static page into a node app so we can, in upcoming steps, add persistence via a sqlite database

instructor should
1. talk about why we need to write server-side code
2. discuss glitch's allowing both front-end and back-end apps by showing the "create an app" modal on [glitch.com](https://glitch.com)
3. guide students in the following steps:

students should
1. add a new file `package.json` and paste the glitch example app file code into it.

  ```
  {
      "//1": "describes your app and its dependencies",
      "//2": "https://docs.npmjs.com/files/package.json",
      "//3": "updating this file will download and update your packages",
      "name": "welcome-to-my-guestbook",
      "version": "0.0.1",
      "description": "a cool 90s style guestbook on new millenium technology",
      "main": "server.js",
      "scripts": {
        "start": "node server.js"
      },
      "dependencies": {
        "express": "^4.16.1"
      },
      "engines": {
        "node": "8.x"
      },
      "repository": {
        "url": "https://glitch.com/edit/#!/empireconf-starter-4"
      },
      "license": "MIT",
      "keywords": [
        "node",
        "glitch",
        "express"
      ]
  }

  ```

2. add a new file `server.js` and paste the glitch example app file code into it

  ```
  // server.js
  // where your node app starts

  // init project
  var express = require('express');
  var app = express();

  // we've started you off with Express, 
  // but feel free to use whatever libs or frameworks you'd like through `package.json`.

  // http://expressjs.com/en/starter/static-files.html
  app.use(express.static('public'));

  // http://expressjs.com/en/starter/basic-routing.html
  app.get("/", function (request, response) {
      response.sendFile(__dirname + '/views/index.html');
  });

  // listen for requests :)
  var listener = app.listen(process.env.PORT, function () {
      console.log('Your app is listening on port ' + listener.address().port);
  });

  ```
  
3. rename `guestbook.js` and `style.css` to have `public/` which will create and move them into a directory called "public"

4. rename `index.html` to have `views/` which will create and move it into a directory called "views"

5. check and make sure that their app does the same thing the static site did in the previous step (there should be no functional or visual changes yet!)

6. update the readme to reflect our new files being added


### [part five](https://glitch.com/edit/#!/empireconf-starter-5): create a database to store guestbook postings

in this section students will use npm to install sqlite into the app and use sqlite to create a database and post all form submissions to

instructors should:
1. show how npm modules can be installed via the glitch `package.json` interface, along with pointing out formatting requirements (no trailing commas,) and discuss why `sqlite3` would be used and not `sqlite`
2. show how the advanced options has a terminal for command line use
3. show how the `.data` folder works
4. show how console logging in the server file of a glitch app works
5. guide students in the following steps:

students should:
1. add `sqlite3` via the dropdown interface in `package.json`
2. check out [the sqlite3 npm docs](https://www.npmjs.com/package/sqlite3) to see how it works and also compare to other modules we aren't using (ie. "sqlite")
3. initialize our sqlite3 database and logic for managing whether one does or does not exist in `.data`
  
  ```
  var fs = require('fs');
  var dbFile = './.data/guestbook.db';
  var exists = fs.existsSync(dbFile);
  var sqlite3 = require('sqlite3').verbose();
  var db = new sqlite3.Database(dbFile);
  ```
  
4. run the create table command if the table does not exist, otherwise console log out all the records

  ```
  // if db does not exist, create it
  db.serialize(function(){
    if (!exists) {
      db.run('CREATE TABLE GuestbookRecords (name TEXT, twitter TEXT, message TEXT)');
      console.log('New table GuestbookRecords created!');
    }
    else {
      console.log('Database "GuestbookRecords" ready to go!');

      // print out every row so far just so we know it's being saved!
      db.each('SELECT * from GuestbookRecords', function(err, row) {
        console.log('record:', row);
      });
    }
  })
  ```
  
4. create an endpoint that the form submission is going to go to and inside of it

  ```
  // and endpoint to hit when submitting guestbook form data
  app.post('/postToGuestbook', function(request, response) {

      console.log('form submitted to /postToGuestbook!')
  })
  ```
  

5. make sure we're sending our form data to the server in our clientside js

  ```
   // now we want to actually submit the form before resetting it
    guestbookForm.submit();
    
    // and reset the form to prevent duplicate records by mistake
    guestbookForm.reset();
  ```

6. we also need to have our long-existing html form tell the form submission where it's going  (the endpoint) and how.

  ```
  <form action="postToGuestbook" method="post">
  ```
  
7. we need to parse the form data sent and express includes a parser for that which we need to import after `app` is initialized

  ```
  var bodyParser = require('body-parser');
  app.use(bodyParser.urlencoded({ extended: true }));
  ```
  
8. update the endpoint to include an insert command that inserts our form data into the database

  ```
  // and endpoint to hit when submitting guestbook form data
  app.post('/postToGuestbook', function(request, response) {

      db.serialize(function() {
        // insert our guestbook record submitted into the database
        var values = `('${request.body.name}', '${request.body.twitter}', '${request.body.message}')`;
        db.run('INSERT INTO GuestbookRecords (name, twitter, message) VALUES ' + values); 
      });
  })
  ```

9. add a couple of guestbook records in the form and you should see them on the page and in the editor logs - but if you refresh the records only show in our editor logs!



### [part six](https://glitch.com/edit/#!/empireconf-starter-6): finish our fully persistent guestbook

in this final section, we're going to create our final endpoint that gives us our database records and use that to populate our guestbook - this way it's persistent in that when we refresh we still see all the submissions!

instructors should:
1. explain how ajax is scary even with abstractions in libraries
2. guide students in the following steps:

students should:
1. delete json records - they're not real and don't belong in our amazing guestbook
2. create an endpoint that sends all of our database records in a stringified json format

  ```
  // add endpoint to hit when getting guestbook db entries, and send back to client
  // to add to the table
  app.get('/getGuestbookEntries', function(request, response) {
      db.all('SELECT * from GuestbookRecords', function(err, rows) {
        response.send(JSON.stringify(rows));
      });
  });
  ```
  
3. make a request via ajax hitting that endpoint to get our database records

  ```
  // use ajax to get our db entries and call the above getGuestbookEntriesListener 
  // to add those entries to our guestbook table
  var entryRequest = new XMLHttpRequest();
  entryRequest.onload = getGuestbookEntriesListener;
  entryRequest.open('get', '/getGuestbookEntries');
  entryRequest.send();
  ```
  
4. create our listener function that we call when the request onload event happens. it should parse and then iterate through its entries like we did with the JSON object

  ```
  // helper function to listen for our db rows request
  const getGuestbookEntriesListener = function() {

      // parse our response to convert to JSON
      const guestbookEntries = JSON.parse(this.responseText);

      // for each entry in guestbookEntries, create a row for that entry
      // and append it to the table
      for (let i = 0; i < guestbookEntries.length; i++) {
        // call the addGuestbookRecord helper to create a row
        addGuestbookRecord(guestbookEntries[i].name, guestbookEntries[i].twitter, `guestbookEntries[i].message);
      }
  }
  ```
  
5. add some records and refresh, it should all still be there \o/

### part seven: pArTy!!

thanks so much for reading, attending, or whatever your participation is in my workshop. for questions, feedback, and saying nice things please email me at `jenn@fogcreek.com` or tweet me at [@jennschiffer](https://twitter.com/jennschiffer)

for more of my apps, check out [my glitch profile](https://glitch.com/@jennschiffer)!


----
### made by [jenn](http://jennmoney.biz/) on [glitch](http://glitch.com)

\ ゜o゜)ノ
