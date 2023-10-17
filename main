const express = require('express');
const app = express();
const port = 3001;

//Middleware to parse the incoming rquest body
app.use (express.json());

//array to store user data
const USERS=[];
const QUESTIONS=[{
  title: "Two States",
  description: "Given an array, return the maximum of the array?",
  testCases: [{
      input: "[1,2,3,4,5]",
      output: "5"
  }]

}];
const SUBMISSIONS=[];

app.get('/signup', function(req, res) {
  res.send('Hello World!');
});

app.post('/signup', function(req,res){
  const {email, password, role}=req.body;
  if(!email||!password){
    res.status(400).send('Email and Password are required');
  }
  else {
      // check if the user with given email already exists
       const existingUser=USERS.find(user=>user.email===email);
       if(existingUser){
        res.status(409).send('User with proveded email already exists');
       }
       else {
        //Add new user to users array
        USERS.push({email, password, role});
        res.status(200).send('User signed up successfully');
       }
  }
  //res.send('Receieved a POST request for signup')
});

app.post('/add-problem',function(req, res){
  const {email,problem} = req.body;
  const user = USERS.find(user=>user.email===email);
  if (user && user.role === 'admin'){
    // randomly added new problem
    const newProblem = {
      title: `Problem ${Math.floor(Math.random() * 1000)}`,
      description: `This is a randomly generated problem.`,
      testCases: [
        {
          input: "Input for the problem",
          output: "Expected output for the problem"
        }
      ]
    };
      QUESTIONS.push(newProblem);
      res.status(200).send('Problem added successfully')
  } else {
    res.status(403).send('Unauthorised access');
  }
});

app.get('/login', function(req, res) {
  res.send('Hello world from route 2');
});

app.post('/login', function(req,res){
  const {email, password} = req.body;

  if (!email||!password){
    res.status(400).send('Email and password are required');
  } else {
    const user= USERS.find(user => user.email===email);
    if (!user){
      res.status(401).send('User not found');
    } else {
      if (user.password === password){
        const token=generateToken(); //generate token function
        res.status(200).send({token: token});
      } else {
        res.status(401).send('Incorrect password');
      }
    }
  }
});

app.get('/questions', function(req, res) {
  res.status(200).send(QUESTIONS);
});

  app.get('/submissions', function(req, res) {
    const questionID="1";
    const userSubmissions=SUBMISSIONS.filter(submission =>submission.questionID===questionID);
    res.status(200).send(userSubmissions);
  });

  app.post('/submissions', function(req, res){
    const {userID, questionID, code, status}= req.body;
    // assuming request body contains userID,questionID, code and status
    const newSubmission= {userID, questionID, code, status};
    SUBMISSIONS.push(newSubmission);
    res.status(200).send('${newSubmission} added successfully');
  });


app.listen(port, function() {
  console.log(`Example app listening on port ${port}`);
});
